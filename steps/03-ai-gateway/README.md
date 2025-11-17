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
kubectl apply -k ./steps/03-ai-gateway/agentic-layer
```

Wait for the AI Gateway to be ready:

```bash
kubectl wait --for=condition=Available --timeout=120s -n ai-gateway deployment/ai-gateway-litellm
```

### Step 3: Modify agents to use the AI gateway.

We want to modify our agents to use the new AI Gateway instead of connecting to OpenAI (or Google) directly. 

Modify the existing Agents by overriding their definitions:

```bash
kubectl apply -k steps/03-ai-gateway/showcase-news
```

Compare the workload definitions in steps/03-ai-gateway/showcase-news with steps/01-agentic-layer-runtime/showcase-news. What do you notice?



> [!TIP]
> Test the AI Gateway's model access control! First, query the news-agent successfully. Then, edit the AiGateway configuration to remove the `gemini-1.5-pro` model from the `aiModels` list. Try querying the news-agent again - you should see it fail because the AI Gateway is now blocking access to that model. This demonstrates how you can control which models are available to your agents centrally.

