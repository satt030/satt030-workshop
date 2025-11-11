# Step 02: AI Gateway

## What You'll Learn

In this step, you will:
- Understand the idea of an AI Gateway
- Deploy the AI Gateway LiteLLM Operator

## Understanding the Building Blocks

### What is an AI Gateway?

An AI Gateway regulates requests and responses from agents to LLMs.

An agent in this context is a container running Python code that connects to an LLM (either hosted externally, or running on the same cluster).


### Features of an AI Gateway

AI Gateways offer the following potential features:
- Managing access to specific LLM models on a per-use-case / per-team basis.
- Managing virtual keys to control costs on a per-use-case / per-team basis.
- Blocking requests (and responses) if they violate certain guardrails (e.g. if they contain PII or harmful instructions).

The Agentic Layer comes with one AI Gateway implementation based on LiteLLM.

---

## Installation Steps

### Step 1: Install AI Gateway LiteLLM Operator

Install the operator (this assumes that the Runtime operator is already deployed)

```bash
kubectl apply -f https://github.com/agentic-layer/ai-gateway-litellm-operator/releases/download/v0.2.0/install.yaml
```

Wait for the operator to be ready

```bash
kubectl wait --for=condition=Available --timeout=60s -n ai-gateway-litellm-system deployment/ai-gateway-litellm-controller-manager
```

### Step 2: Deploy AI Gateway

```bash
kubectl apply -f ./steps/03-ai-gateway/agentic-layer

```
