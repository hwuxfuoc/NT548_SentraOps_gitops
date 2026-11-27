# Contributing Guide

## Quy tắc chung

- **Không push thẳng vào `main`** — mọi thay đổi phải qua Pull Request
- **Không sửa `dev/values.yaml` thủ công** — file này do CD pipeline tự cập nhật
- Mỗi PR cần ít nhất 1 người review trước khi merge

---

## Cách cập nhật image tag cho staging / prod

### Staging

```bash
git checkout -b bump/staging-<sha_ngắn>
# Sửa staging/values.yaml: đổi tag thành SHA muốn deploy
git add staging/values.yaml
git commit -m "chore: bump staging to <sha>"
git push -u origin bump/staging-<sha_ngắn>
# Mở PR → review → merge → vào ArgoCD UI bấm Sync
```

### Prod

Tương tự staging, nhưng PR cần approval từ lead trước khi merge.

---

## Cách cập nhật Helm chart

Khi chart trong app repo thay đổi, cần sync lại vào gitops repo:

```bash
# Từ app repo
cp -r helm/ ../NT548_SentraOps_gitops/helm/

# Vào gitops repo
cd ../NT548_SentraOps_gitops
git checkout -b sync/helm-chart-<version>
git add helm/
git commit -m "chore: sync helm chart from app repo vX.Y.Z"
git push -u origin sync/helm-chart-<version>
# Mở PR → review → merge
```

---

## Branch naming

| Loại | Pattern | Ví dụ |
|---|---|---|
| Bump môi trường | `bump/<env>-<sha>` | `bump/staging-a1b2c3d` |
| Sync chart | `sync/helm-chart-<ver>` | `sync/helm-chart-0.2.0` |
| Thêm tính năng ArgoCD | `feat/<tên>` | `feat/add-prod-app` |
| Fix | `fix/<tên>` | `fix/prod-nodeport` |
