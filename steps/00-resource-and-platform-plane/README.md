## Cluster information


Your [virtual] cluster includes API Keys for Gemini, exposed as a secret (see `kubectl get secret api-key-secrets -n showcase-news`) Please use responsibly.

## Troubleshooting

Ensure your kubectl is pointing to your local cluster:

```bash
kubectl config current-context
kubectl cluster-info
```
