Welcome to the workshop.

What's your starting point?

- Browser-based editor

What's included in your [virtual] cluster?
- API Keys for Gemini
  - Exposed as a secret (see `kubectl get secret api-key-secrets -n showcase-news`). Please use responsibly.



### Verify Kubernetes Context

Ensure your kubectl is pointing to your local cluster:

```bash
kubectl config current-context
kubectl cluster-info
```

Prerequisites

Before starting, ensure you have the following installed and configured:
(TODO Felix: Ensure these tools are installed and working in the devcontainer, then remove this section)

Required Software
•	Docker - For containerization
•	kubectl - Kubernetes command-line tool
•	Tilt - Install Tilt for local development
•	curl - For downloading manifests

Access Requirements
•	Gemini API Key - Get yours from Google AI Studio
•	Local Kubernetes Cluster - We recommend:
•	Docker Desktop (with Kubernetes enabled)
•	Rancher Desktop (with Kubernetes enabled)
•	kind
•	k3s or k3d
