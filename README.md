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

3. Bootstrap flux
    ```
    flux bootstrap github \
      --token-auth \
      --owner=my-github-username \
      --repository=my-repository-name \
      --branch=main \
      --path=clusters/my-cluster \
      --personal
    ```


Building Blocks
1. Resource Plane (vCluster - already provisioned)
   - nothing to do
2. Platform Plane (Grafana already provisioned; Flux optional)
   - nothing to do (or optional)
3. Model & Data Plane (Ollama operator already provisioned - TODO!)
   - nothing to do
4. Service Plane
   - Agentic Layer!
     - a) agent-runtime (und agents mit ADK bauen) (Nutzen: 
     - b) agent-gateway (Nutzen: Sorgt für Kompatibilität durch OpenAI-kompatible API)
     - c) (extra Observability tooling)
     - d) ai-gateway 
5. Quality Plane
    - INSTALL TESTBENCH OPERATOR! (WIP) - should be done after installing agents
?6?. Compliance Plane (Future...)

####User Serving Plane
### Chatting with LLMs
- https://agentgateway.dev/
- A2A Inspector...
- (Beliebiges anderes OpenAI-kompatibles Frontend)

### Defining Agents


---

## Part 1: Building blocks of an AI platform

An AI platform consists of multiple layers that work together to provide a complete solution for deploying and managing AI workloads. This workshop will guide you through implementing the following layers:

### 1. Resource Plane
The foundation layer providing compute resources and cluster management.
- **vCluster**: Isolated Kubernetes clusters within the parent cluster
- **Crossplane** (optional): Infrastructure provisioning

### 2. Platform Plane
Core platform services for deployment, monitoring, and operations.
- **Delivery**: Flux for GitOps-based continuous delivery
- **Monitoring & Logging**: Grafana LGTM stack and OpenTelemetry Collector
- Security, FinOps, and advanced operability features are out of scope for this workshop

### 3. Integration & Delivery Plane
Services for model hosting and data integration.
- **Model Plane**: Ollama Operator for LLM deployment (deployed in parent cluster)
- Data plane (vector databases) is out of scope for this workshop

### 4. Quality Plane
Testing and validation infrastructure for AI workloads.
- **Testkube**: Test orchestration
- **ragas**: LLM evaluation framework
- Pre/post-deployment validation (Keptn) is out of scope

### 5. Compliance Plane
(To be determined - optional for workshop)

### 6. Service Plane
User-facing services and access management.
- **User Serving**: Agent runtime and orchestration
    - Agent-Runtime-Operator for deploying agents
    - Agent CRD for declarative agent definitions
- **Access Plane**: Gateway and routing
    - AI-Gateway-Operator with LiteLLM for model routing
    - Agent-Gateway-Operator with KrakenD for agent routing

### 7. Agentic Layer
Specialized components for agent-based workloads.
- Agent-Runtime-Operator (based on ADK)
- Agent-Gateway-Operator (KrakenD implementation)
- AI-Gateway-Operator (LiteLLM implementation)

## Part 2: Tech Stack Deep-Dive

### Flux - GitOps Delivery
Flux enables declarative, Git-based deployment of all platform components.

**Installation:**
```bash
flux bootstrap github \
  --token-auth \
  --owner=<your-github-username> \
  --repository=<your-repo-name> \
  --branch=main \
  --path=clusters/my-cluster \
  --personal
```

**What it does:**
- Continuously syncs Kubernetes manifests from your Git repository
- Ensures cluster state matches repository state
- Enables GitOps workflows for all subsequent installations

### Grafana LGTM & OpenTelemetry
Observability stack for monitoring your AI platform.

**Components:**
- **Loki**: Log aggregation
- **Grafana**: Visualization and dashboards
- **Tempo**: Distributed tracing
- **Mimir**: Metrics storage
- **OTEL Collector**: Telemetry collection and forwarding

**Installation:**
```bash
# Deploy via Flux
kubectl apply -f platform/monitoring/
```

### Ollama Operator
Manages LLM deployments in the parent cluster, accessible to all vClusters.

**Features:**
- Declarative model management
- Automatic model downloading and serving
- Resource management for GPU/CPU workloads

**Installation (Parent Cluster Only):**
```bash
kubectl apply -f integration/ollama-operator/
```

**Usage:**
```yaml
apiVersion: ollama.io/v1alpha1
kind: Model
metadata:
  name: llama2
spec:
  model: llama2:7b
```

### Testkube & ragas
Quality assurance for LLM applications.

**Testkube**: Orchestrates test execution
**ragas**: Evaluates LLM output quality (relevance, faithfulness, etc.)

**Installation:**
```bash
# Deploy testbench-operator
kubectl apply -f quality/testbench-operator/

# Define an experiment
kubectl apply -f quality/experiments/sample-eval.yaml
```

### AI-Gateway-Operator (LiteLLM)
Routes requests to different LLM providers and models.

**Features:**
- Unified API for multiple providers (Ollama, OpenAI, etc.)
- Load balancing and fallbacks
- Cost tracking and rate limiting

**Installation:**
```bash
kubectl apply -f service/ai-gateway-operator/
```

**Configuration:**
```yaml
apiVersion: gateway.ai/v1alpha1
kind: ModelRouter
metadata:
  name: default-router
spec:
  routes:
    - name: ollama-llama2
      provider: ollama
      model: llama2:7b
      endpoint: http://ollama.parent-cluster:11434
```

### Agent-Runtime-Operator
Deploys and manages autonomous agents using the Agent Development Kit (ADK).

**Features:**
- Declarative agent definitions via CRD
- Lifecycle management (deploy, scale, update)
- Integration with AI gateways

**Installation:**
```bash
kubectl apply -f agentic/agent-runtime-operator/
```

**Usage:**
```yaml
apiVersion: agents.agentic.io/v1alpha1
kind: Agent
metadata:
  name: customer-support-agent
spec:
  runtime: adk
  model: llama2:7b
  tools:
    - web-search
    - knowledge-base
```

### Agent-Gateway-Operator (KrakenD)
Routes and manages traffic between agents following the A2A protocol.

**Features:**
- Agent-to-Agent (A2A) communication
- Service discovery via `.well-known/agent-card.json`
- Rate limiting and authentication

**Installation:**
```bash
kubectl apply -f agentic/agent-gateway-operator/
```

## Part 3a: Testbench

## Part 3b: Compliance

## Part 4: Deploying Agentic Workloads
