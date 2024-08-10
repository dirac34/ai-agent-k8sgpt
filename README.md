# ai-agent-k8sgpt

To create an AI agent in Python that interacts with `k8sGPT`, you'll need to integrate your Python code with Kubernetes and have it communicate with the `k8sGPT` service. `k8sGPT` is a tool designed to simplify troubleshooting and diagnostics in Kubernetes clusters by leveraging GPT-based AI models to analyze and interpret Kubernetes states and logs.

Here's a step-by-step guide to building an AI agent that connects with `k8sGPT`:

### Prerequisites

1. **Kubernetes Cluster**: You need access to a Kubernetes cluster where `k8sGPT` is deployed.
2. **Kubernetes Client for Python**: Install the `kubernetes` Python package to interact with the Kubernetes API.
3. **k8sGPT Deployment**: `k8sGPT` should be deployed and running in your Kubernetes cluster.

### Step 1: Install Required Python Packages

First, you need to install the necessary Python packages.

```bash
pip install kubernetes requests
```

### Step 2: Set Up the Kubernetes Client

The Kubernetes client allows your Python script to interact with the Kubernetes API.

```python
from kubernetes import client, config

# Load Kubernetes configuration
config.load_kube_config()

# Create a Kubernetes API client
v1 = client.CoreV1Api()
```

### Step 3: Interact with `k8sGPT`

Assuming `k8sGPT` is exposed via a service, you can interact with it using HTTP requests from your Python agent. For example, you might query `k8sGPT` for insights or recommendations based on the current state of the cluster.

```python
import requests

class K8sGPTAgent:
    def __init__(self, service_url):
        self.service_url = service_url

    def get_insights(self):
        response = requests.get(f"{self.service_url}/api/v1/insights")
        if response.status_code == 200:
            return response.json()
        else:
            return {"error": "Failed to fetch insights from k8sGPT"}

    def analyze_namespace(self, namespace):
        response = requests.get(f"{self.service_url}/api/v1/analyze/{namespace}")
        if response.status_code == 200:
            return response.json()
        else:
            return {"error": f"Failed to analyze namespace {namespace}"}

# Initialize the agent with the k8sGPT service URL
k8sgpt_agent = K8sGPTAgent(service_url="http://k8sgpt-service.default.svc.cluster.local")

# Fetch and print insights
insights = k8sgpt_agent.get_insights()
print(insights)

# Analyze a specific namespace
namespace_analysis = k8sgpt_agent.analyze_namespace("default")
print(namespace_analysis)
```

### Step 4: Example of Full Agent

Here's the full code that brings everything together:

```python
from kubernetes import client, config
import requests

# Load Kubernetes configuration
config.load_kube_config()

# Create a Kubernetes API client
v1 = client.CoreV1Api()

class K8sGPTAgent:
    def __init__(self, service_url):
        self.service_url = service_url

    def get_insights(self):
        response = requests.get(f"{self.service_url}/api/v1/insights")
        if response.status_code == 200:
            return response.json()
        else:
            return {"error": "Failed to fetch insights from k8sGPT"}

    def analyze_namespace(self, namespace):
        response = requests.get(f"{self.service_url}/api/v1/analyze/{namespace}")
        if response.status_code == 200:
            return response.json()
        else:
            return {"error": f"Failed to analyze namespace {namespace}"}

# Initialize the agent with the k8sGPT service URL
k8sgpt_agent = K8sGPTAgent(service_url="http://k8sgpt-service.default.svc.cluster.local")

# Fetch and print general insights
insights = k8sgpt_agent.get_insights()
print("Cluster Insights:", insights)

# Analyze a specific namespace, e.g., "default"
namespace_analysis = k8sgpt_agent.analyze_namespace("default")
print(f"Namespace 'default' Analysis:", namespace_analysis)
```

### Step 5: Deploy and Run Your Agent

You can run this Python script from within your cluster (e.g., in a Kubernetes Job or Pod) or from an external system with access to the cluster's API server.

### Explanation:

- **K8s Client**: The Kubernetes client is used to interact with your Kubernetes cluster (optional, but useful if you need to fetch cluster data).
- **HTTP Requests**: The agent sends HTTP requests to the `k8sGPT` service to retrieve insights or perform analysis.
- **Service URL**: The service URL should point to where `k8sGPT` is exposed (e.g., via a Kubernetes Service).

### Conclusion:

This Python AI agent interacts with `k8sGPT` to gather insights and perform analyses on your Kubernetes cluster. It can be expanded to automate troubleshooting, make decisions based on `k8sGPT`'s insights, or trigger alerts and actions depending on the findings.