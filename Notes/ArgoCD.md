---
categories:
  - "[[Guides]]"
type: []
org: []
start: 2026-09-06
year:
url:
status:
created: 2026-09-06
---
**Pulling New Service Images when `:latest` is being used**:

- **UI:** open the `arr` or `media` app → select the Deployment → Restart.
- **CLI:**
  ```bash
  argocd app actions run arr restart --kind Deployment --resource-name radarr
  argocd app actions run media restart --kind Deployment --resource-name jellyfin
  ```
