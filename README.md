# Workshop: Architecting and Building a K8s-based AI Platform

This is the companion repository for a full-day workshop at the Continuous Lifecycle Conference in Mannheim on November 18, 2025.

## Abstract

Companies increasingly recognize the strategic importance of Artificial Intelligence, yet deploying AI solutions at scale presents significant challenges. This hands-on workshop provides a practical, step-by-step guide to designing and building a cloud-native AI platform.

You'll learn how to create a unified platform that simplifies the entire AI lifecycle using Kubernetes.

**What you'll build:**
- Agent runtime and orchestration layer
- API gateways for unified agent and model access
- Observability and monitoring stack

This repo guides you through the process of instantiating your own AI platform.

## Agenda

10:00 Uhr: Einführung
- Vorstellung und Zielsetzung
- Architektur-Deep-Dive: Bausteine einer KI-Plattform
- (Kaffeepause um 11:30)

13:00 Uhr: Mittagspause

14:00 Uhr: Technologie-Stack unter der Lupe
- Deployment und Betrieb von Agentic Workloads
- (Kaffeepause um 15:30)
- Offene Diskussion, Q&A und Wrap-up

ca. 17:00 Uhr: Ende

## Overview

- Step 0: Platform setup
- Step 1: Instantiating a runtime layer to enable agent orchestration
- Step 2: Provisioning of Agent Gateways
- Step 3: Provisioning of AI Gateways
- ~~Step 4: Setting up a Quality Plane (WIP)~~
- ~~Step 5: Setting up a Service Plane (WIP)~~
- Step 6: (Bonus) GitOps

## Setup

1. Fork this repository

2. Create a Github Codespace

![img.png](img.png)

3. Claim a vCluster by entering your name (or just an emoji) in this spreadsheet: https://ethercalc.net/mzku2p0zjwkk 

4. In your Github Codespace, decrypt the kubeconfig (Kubernetes credentials file) for your vCluster.
```
# Usage: ./decrypt-kubeconfig.sh <encrypted-file> <password> [output-file]

./decrypt-kubeconfig.sh kubeconfigs/participant-1-kubeconfig.yaml.enc <password-announced-during-workshop> kubeconfig.yaml 
```

5. Connect to your vCluster
    ```
    export KUBECONFIG=kubeconfig.yaml 
   
    kubectl get pods
    ```

6. Start hacking!

---
