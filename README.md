# NT548_SentraOps_gitops

GitOps repository cho dự án SentraOps — NT548 UIT.

Repo này là **nguồn sự thật duy nhất** cho trạng thái mong muốn của cluster k3s. ArgoCD pull từ repo này để deploy, không nhận webhook từ CI.

---

## Cấu trúc

```
├── helm/sentraops/          # Helm chart (sync từ app repo)
│   ├── Chart.yaml
│   ├── values.yaml          # default values
│   └── templates/
├── dev/values.yaml          # override image.tag — CD tự cập nhật
├── staging/values.yaml      # override image.tag — cập nhật thủ công qua PR
├── prod/values.yaml         # override image.tag — cập nhật thủ công qua PR
└── apps/
    ├── sentraops-dev.yaml    # ArgoCD Application — auto-sync
    ├── sentraops-staging.yaml
    └── sentraops-prod.yaml
```

---

## Vòng GitOps

```
developer merge vào main (app repo)
        ↓
CD pipeline build & push image lên GHCR
        ↓
CD pipeline cập nhật dev/values.yaml (tag = git SHA)
        ↓
ArgoCD detect commit mới trong gitops repo
        ↓
ArgoCD deploy lên namespace sentraops-dev trên k3s
```

---

## Môi trường

| Môi trường | Namespace | Sync | NodePort |
|---|---|---|---|
| dev | sentraops-dev | Auto (prune + selfHeal) | 30080 |
| staging | sentraops-staging | Manual | 30081 |
| prod | sentraops-prod | Manual | 30082 |

---

## Liên quan

- App repo: [NT548_SentraOps_app](https://github.com/hwuxfuoc/NT548_SentraOps_app)
- ArgoCD UI: `http://35.75.47.226:30443`
