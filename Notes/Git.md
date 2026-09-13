---
categories:
  - "[[Guides]]"
topics: []
url:
created: 2026-09-06
---
## Login SSH
**Generate a key**
```bash
ssh-keygen -t ed25519 -C "new-host" -f ~/.ssh/id_ed25519
```

**Print the public key**
```bash
cat ~/.ssh/id_ed25519.pub
```

**Add that public key on GitHub:**
- Your account: GitHub → Settings → SSH and GPG keys → New SSH key  
    This covers every private repo you can access, and allows push.
- One repo only: repo → Settings → Deploy keys  
    Read-only unless you check write access. Do not reuse the ArgoCD deploy key (`~/.ssh/argocd_homelab_deploy`) as your workstation key.

**Confirm GitHub accepts the key**
```bash
ssh -T git@github.com
```

**Expect:** 
> Hi user! You've successfully authenticated...

