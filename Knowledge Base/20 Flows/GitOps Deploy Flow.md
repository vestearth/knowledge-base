# GitOps Deploy Flow

## Purpose

This note explains how Games-Labs services deploy through CI + ArgoCD, and why the
pipeline changed from two jobs (`build-push` -> `rollout-restart`) to a single
`build-deploy` job with SHA-pinned images.

It exists to answer one recurring question: **what was `rollout-restart` for, and
why do the newer workflows not have it?**

> 🇹🇭 โน้ตนี้อธิบายว่า service ของ Games-Labs deploy ผ่าน CI + ArgoCD อย่างไร
> และทำไม pipeline ถึงเปลี่ยนจาก 2 job (`build-push` -> `rollout-restart`)
> เหลือ job เดียว (`build-deploy`) ที่ pin image เป็น `sha-<short>`
> คำถามหลักที่โน้ตนี้ตอบ: **`rollout-restart` มีไว้ทำไม แล้วทำไมตัวใหม่ไม่มีแล้ว**

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-Wallet/.github/workflows/deploy.yml` (new style, kustomize mode)
- `/Users/earth/Documents/GitHub/Games-Labs-Logs/.github/workflows/deploy.yml` (new style, directory mode — template)
- `/Users/earth/Documents/GitHub/Games-Labs-Order/.github/workflows/deploy.yml` (old style until PR #3 merges)
- `/Users/earth/Documents/GitHub/<service>/k3s/argocd-app.yaml` (ArgoCD Application per service)
- `/Users/earth/Documents/GitHub/ai-dev-office/runs/TASK-084/findings.md` (Wallet review)
- `/Users/earth/Documents/GitHub/ai-dev-office/runs/TASK-086/findings.md` (Logs / directory-mode template)

## Old Flow (build-push -> rollout-restart)

```text
push code to main
  -> job build-push: build image, push ghcr.io/...:latest (mutable tag)
  -> job rollout-restart (continue-on-error):
       kubectl apply argocd-app.yaml (bootstrap)
       sleep 30 (guess ArgoCD sync time)
       kubectl create configmap/secret from GitHub Secrets
       wait loop until deployment exists
       kubectl rollout restart deployment <service>   <-- "the kick"
```

### What rollout-restart was for

The deployment manifest pinned `image: ...:latest`, a **mutable tag**. Pushing a
new image to the same tag changes nothing observable:

- Kubernetes sees no spec change (the manifest still says `:latest`), so no rollout happens.
- ArgoCD sees no git diff, so it never syncs the new build.

So a new image reached production only by force: `kubectl rollout restart` stamps a
`restartedAt` annotation on the pod template, which triggers a rolling restart, and
`imagePullPolicy: Always` makes the new pods re-pull whatever `:latest` currently
points to. The `sleep 30` + wait loop existed for first-deploy bootstrap, when the
deployment might not exist yet.

> 🇹🇭 **rollout-restart คือ "ตัวเตะ"** — เพราะ deployment เดิม pin `image: ...:latest`
> ซึ่งเป็น mutable tag เวลา CI push image ใหม่ขึ้น GHCR ด้วย tag ชื่อเดิม:
>
> - **Kubernetes ไม่รู้** — Deployment spec ใน cluster ไม่ได้เปลี่ยนสักตัวอักษร
>   (ก็ยังเขียนว่า `:latest` เหมือนเดิม) เลยไม่มี rollout เกิดขึ้น
> - **ArgoCD ก็ไม่รู้** — git ไม่ได้เปลี่ยน เลยไม่มี diff ให้ sync
>
> ผลคือ push image ใหม่แล้ว**ไม่มีอะไรเกิดขึ้นเลย** จึงต้องมี job ที่สองมาเตะให้ตื่น:
> `kubectl rollout restart` ไปแปะ annotation `restartedAt` บน pod template
> → K8s เห็นว่า template เปลี่ยน → rolling restart → pod ใหม่เกิด
> → `imagePullPolicy: Always` บังคับให้ pull `:latest` ล่าสุดมา
> นี่คือวิธีที่ image ใหม่ขึ้น production ในระบบเก่า
>
> ส่วน `sleep 30` + wait loop คือรอเผื่อ deploy ครั้งแรกที่ ArgoCD
> ยังสร้าง deployment ไม่เสร็จ จะได้มีตัวให้ restart — เป็นการเดาเวลาล้วน ๆ

### Why the old flow was a problem

- Deployed version is undefined — git cannot tell you what is running, and there is no git rollback.
- The imperative restart fights ArgoCD `selfHeal` (two controllers driving one workload).
- `continue-on-error: true` made failed deploys report green.
- Timing was guesswork (`sleep 30`, 24x5s polling).

> 🇹🇭 ปัญหาของแบบเก่า: (1) ไม่มีทางรู้จาก git ว่าตอนนี้ production รันเวอร์ชันไหน
> และ rollback ผ่าน git ไม่ได้ (2) การ restart แบบ imperative ตีกับ ArgoCD `selfHeal`
> — สองตัวคุม workload เดียวกัน (3) `continue-on-error: true` ทำให้ deploy ล้มแต่ CI ขึ้นเขียว
> (4) จังหวะเวลาเป็นการเดาล้วน ๆ

## New Flow (build-deploy, single job)

```text
push code to main (paths-ignore: k3s/**)
  -> job build-deploy:
       build image, push :latest AND :sha-<short7>   (immutable tag)
       kubectl apply argocd-app.yaml + ConfigMap/Secret   <-- config FIRST
       yq-pin the sha- tag into k3s/deployment.yaml (or kustomization.yaml)
       git commit + push "ci: pin <image> to sha-... [skip ci]"
  -> ArgoCD sees the git diff -> syncs -> Deployment spec actually changed
  -> Kubernetes performs a normal rolling update to the exact tag
```

The rollout now happens because the **declared spec changed**, not because an
unchanged spec was force-restarted. No kick needed.

> 🇹🇭 ตัวใหม่เปลี่ยนวิธี "บอกข่าว" — แทนที่จะเตะ pod ให้ไป pull tag เดิม
> เรา**เขียนเวอร์ชันใหม่ลง git ตรง ๆ**:
>
> ```text
> เก่า:  push :latest → (ไม่มีใครรู้) → ต้อง rollout restart เตะเอง
> ใหม่:  push :sha-33e0442 → commit tag นี้ลง deployment.yaml → ArgoCD เห็น diff
>        → sync → K8s เห็น image เปลี่ยนจริง → rolling update เอง
> ```
>
> rollout เกิดจาก **spec เปลี่ยนจริง** ไม่ใช่จากการบังคับ restart ของที่หน้าตาเหมือนเดิม
> — ได้ของแถมทั้งชุด: รู้แน่ว่ารันเวอร์ชันไหน, rollback = `git revert`,
> ไม่ตีกับ selfHeal, ไม่ต้องเดาเวลาด้วย sleep

### Why one job instead of two

- **Ordering**: ConfigMap/Secret must be applied *before* the image commit, so the
  pods ArgoCD rolls out read fresh config. One job guarantees the sequence.
- **Fail loudly**: no `continue-on-error`. If any step fails, the run is red and the
  image tag is never committed — an unready build cannot deploy.

> 🇹🇭 ทำไมรวมเป็น job เดียว: (1) **ลำดับสำคัญ** — ต้อง apply ConfigMap/Secret
> *ก่อน* commit image tag เพื่อให้ pod ใหม่ที่ ArgoCD roll ออกมาอ่านค่าล่าสุด
> แยก 2 job แล้วการันตีลำดับยากกว่า (2) **ให้พังดัง ๆ ทั้งสาย** — ถ้า step ไหนล้ม
> ทั้ง run แดงและ image tag จะไม่ถูก commit = ของที่ไม่พร้อมจะไม่ถูก deploy

## Two Render Modes

| Mode | Services | How the tag is pinned |
| --- | --- | --- |
| kustomize (`k3s/kustomization.yaml` exists) | Wallet | `images[].newTag` in kustomization.yaml |
| directory (no kustomization.yaml) | all others | `containers[0].image` in deployment.yaml |

Do not introduce kustomize into a directory-mode service just for pinning — pin the
deployment.yaml field directly to keep the blast radius small.

> 🇹🇭 ArgoCD render ได้ 2 โหมด: ถ้า repo มี `kustomization.yaml` (Wallet) จะ pin tag
> ผ่าน kustomize `images` transformer แต่ service อื่นเป็น directory mode
> (apply ทุก yaml ใน `k3s/` ตรง ๆ) ให้ pin ที่ field `image:` ใน deployment.yaml เลย
> **อย่า**ยัด kustomize เข้าไปใน service ที่เป็น directory mode แค่เพื่อ pin tag
> — เปลี่ยนโหมด render โดยไม่จำเป็นจะเพิ่มความเสี่ยง

## Critical Rules

- The deployed version must always be readable from git (`sha-<short>` tag, never bare `:latest`).
- Rollback = `git revert` the tag-pin commit; ArgoCD returns to the previous immutable image.
- Never reintroduce `kubectl rollout restart` next to ArgoCD `selfHeal` — they fight.
- `paths-ignore: [k3s/**]` prevents the CI tag-bump commit from re-triggering the build loop.
- A config-only change does not roll pods by itself until the next code deploy (no image diff). Add a Reloader if hot config reload is ever needed.
- ArgoCD `ignoreDifferences` on ConfigMap `/data` (+ `RespectIgnoreDifferences=true`) is what lets CI-applied config coexist with `selfHeal` — keep it when touching argocd-app.yaml.
- CI needs `permissions: contents: write` and the `GITHUB_TOKEN` must be allowed to push to main (branch protection).

> 🇹🇭 กฎสำคัญ (สรุป):
>
> - เวอร์ชันที่ deploy ต้องอ่านได้จาก git เสมอ (tag `sha-<short>` ห้ามใช้ `:latest` เปล่า ๆ)
> - Rollback = `git revert` commit ที่ pin tag → ArgoCD กลับไป image เดิมที่เคยรันจริง
> - ห้ามเอา `kubectl rollout restart` กลับมาใช้คู่กับ `selfHeal` — สองตัวจะตีกัน
> - `paths-ignore: [k3s/**]` กันไม่ให้ commit tag-bump ของ CI ไป trigger build วนลูป
> - แก้ config อย่างเดียว (image ไม่เปลี่ยน) pod จะ**ไม่** restart เองจนกว่าจะ deploy
>   code ครั้งถัดไป — ถ้าอยากได้ hot reload ค่อยเพิ่ม Reloader ทีหลัง
> - `ignoreDifferences` บน ConfigMap `/data` คือสิ่งที่ทำให้ config ที่ CI apply
>   อยู่ร่วมกับ `selfHeal` ได้ — เวลาแก้ argocd-app.yaml อย่าลบทิ้ง
> - CI ต้องมี `permissions: contents: write` และ `GITHUB_TOKEN` ต้อง push เข้า main ได้
>   (ระวัง branch protection)

## Migration Status (2026-06-10)

- Merged (new flow): `Games-Labs-Wallet` (PR 5), `Games-Labs-Logs` (PR 1).
- PRs open (old flow still on main): Game (PR 6), User (PR 1), Order (PR 3), api-gateway (PR 6), Auth (PR 1), Missions (PR 40).
- Deferred: `Games-Labs-backoffice` (waiting on in-flight TASK-081 work; its apply step has no Secret/ConfigMap).
- Out of scope: `Games-Labs-Provider` (no k3s), `shared-lib` (library).

> 🇹🇭 สถานะ ณ 2026-06-10: Wallet กับ Logs ใช้ flow ใหม่แล้ว ที่เหลือ 6 ตัว PR ยังเปิดรอรีวิว
> (main ยังเป็น flow เก่า) ส่วน backoffice รอ TASK-081 เสร็จก่อน
> Provider/shared-lib ไม่เกี่ยว (ไม่มี k3s / เป็น library)

## Related

- [[Backoffice API Contract Map]]
- AI Dev Office delivery skills: `ai-skills/skills/gitops-deploy-review`, `cicd-pipeline-review`, `k8s-deploy-review`
