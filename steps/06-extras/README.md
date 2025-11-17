
## Bonus: GitOps with Flux

See https://opengitops.dev/ for a primer on GitOps.

### Flux - GitOps Delivery
Flux enables declarative, Git-based deployment of all platform components.



**Installation:**
```bash
flux bootstrap github \
  --token-auth \
  --owner=<your-github-username> \
  --repository=<your-repo-name> \
  --branch=main \
  --path=steps/06-extras/gitops \
  --personal
```

**What it does:**
- Continuously syncs Kubernetes manifests from your Git repository
- Ensures cluster state matches repository state
- Enables GitOps workflows for all subsequent installations

