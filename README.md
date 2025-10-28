# workshop
Companion Repo for the "Architecting and Building a K8s-based AI Platform" Workshop (Continuous Lifecycle / KI Navigator)
# Workshop: Architecting and Building a K8s-based AI Platform

Companion Repo for our Workshop (Continuous Lifecycle / KI Navigator)

Let's build a cloud-native AI platform!

Todo: Abstract, Screenshots, Table of Contents, Agenda

This repo guides you through the process of instantiating your own AI platform.

## Workshop Setup

1. Fork this repo

2. Unencrypt the kubeconfig (Kubernetes credentials file) for your vCluster.
```
# Usage: ./decrypt-kubeconfig.sh <encrypted-file> <password> [output-file]

./decrypt-kubeconfig.sh kubeconfigs-encrypted/participant-1-kubeconfig.yaml.enc <password-announced-during-workshop> kubeconfig.yaml 
```

2. Connect to your vCluster
    ```
    export KUBECONFIG=/path/to/your/credentials.yaml
   
    kubectl get pods
    ```

