# **Alquimia Runtime AI - Step by Step**

This Helm chart deploys the base infrastructure for the **Alquimia AI** runtime on top of Kubernetes and OpenShift, using serverless and event-driven technologies such as **Knative Serving** and **Knative Eventing**. It is designed for enterprises looking for scalable, secure, production-ready AI solutions.
---
## **🚀 Main features**
- Automated deployment of AI inference microservices, evaluation and Alchemy AI tools.
- Event-driven architecture (Brokers, Triggers, Sequences, SinkBindings).
- Flexible and customizable configuration via `values.yaml`.
- Advanced OpenShift integration and support for standard Kubernetes (>=1.24).
---
## **📋 Prerequisites**
- Kubernetes >= 1.24
- Helm 3.x
- Knative (Serving, Eventing and Kafka)
- Istio Mesh
---

## **📦 Installation**
Quick install from official repository:
````bash
helm repo add alchemy-ai https://www.alquimia.ai/helm-charts
helm install alchemy-runtime alchemy-ai/alchemy-runtime-helm --namespace alchemy --create-namespace
````
Local installation:
````bash
helm install my-release . --namespace alchemy-runtime
````
Overwrite values with custom file:
````bash
helm install my-release . --namespace alchemy-runtime -f values.override.yaml
````
Or using `--set`:
````bash
helm install my-release . \
  --set serving.services[0].image="alchemyai/leviathan:latest" \
  --set serving.services[1].image="alchemyai/hermes:latest"
````

---
## **⚙️ Configurable parameters**
### **1️⃣ Eventing Configuration (`eventing`)**
| Parameter | Description | Default value |
|-----------|-------------|------------------|
| `eventing.brokers` | Knative `Broker` resource list | View values.yaml |
| `eventing. triggers` | | Knative resource list `Trigger` | See values.yaml |
| `eventing.sequences` | Knative resource list `Sequence` | See values.yaml |
| `eventing.sinkBindings` | Knative resource list `SinkBinding` | See values.yaml |
#### **🔹 Example - Custom Broker Configuration**
````yaml
eventing:
 brokers:
    - name: inbound
 deadLetterUri: /deadletter/inbound
    - name: process
 deadLetterUri: /deadletter/process
````
### **2️⃣ Services Configuration (`serving.services`)**
| Parameter | Description | Default value |
|-----------|-------------|------------------|
| `serving.services` | List of Knative microservices | See values.yaml |
| `serving.services[0]. name` | service name | "alchemy-leviathan" |
| `serving.services[0].image` | service image | "alchemyai/leviathan:latest" |
| `serving.services[1].name` | service name | "alchemy-hermes" |
| `serving.services[1].image` | service image | "alchemyai/hermes:latest" |

#### **🔹 Example - Custom Images**
```yaml
serving:
 services:
    - name: alchemy-leviathan
 image: "alchemyai/leviathan:v2.0.0"
    - name: alchemy-hermes
 image: "alchemyai/hermes:latest"
````
### **3️⃣ Environment and configuration variables (`serving. env`)**
| Key | Description | Default value |
| ------- | -------------|-------------|------------------|
| `DEBUG` | Debug mode (`true`/`false`) | "False" |
| `REDIS_HOST` | Redis host | "" |
| `REDIS_PASSWORD` | Redis password | "" |
| `COUCHDB_URL` | CouchDB URL | "" |
| `API_TOKEN` | API token | "" |
| ... | ... | ... |
#### **🔹 Example - Custom environment variables**
````yaml
serving:
 env:
 API_URL: "https://api.alquimia.ai"
 DEBUG: "true"
````
You can see all available values and their descriptions in [`values.yaml`](./values.yaml).

---
## **🛠️ Resources it deploys**
This chart automatically creates the following Kubernetes resources (using Knative):
- **Brokers** (`eventing.knative.dev/v1`): event channels for orchestrating flows.
- Triggers** (`eventing.knative.dev/v1`): rules for routing events to services.
- Sequences** (`flows.knative.dev/v1`): chained step flows for advanced processing.
- SinkBindings** (`sources.knative.dev/v1`): bind services to event brokers.
- ConfigMaps**: for environment and configuration variables.
- **Knative Services** (`serving.knative.dev/v1`): serverless deployment of Alchemy AI microservices.
---
## **🧩 Usage examples**
### **🚀 Deployment with images and custom configuration**
````bash
helm install my-alquimia-app . \
  --set serving.services[0].image="alchemyai/leviathan:latest" \
  --set serving.services[1].image="alquimiaai/hermes:latest" \
  --set serving.env.DEBUG="true"
````
### **🔄 Upgrade an existing deployment**
````bash
helm upgrade my-alchemy-app . -f values.override.yaml
````
### **🗑️ Uninstall**
````bash
helm uninstall my-alchemy-app upgrade my-alquimia-app . -f values.override.yaml
```

---
## **📞 Support and contact**
- **Website:** [alchemy.ai](https://www.alquimia.ai/)
- **Support:** [https://www.alquimia.ai/](https://www.alquimia.ai/)
- **Maintainer:** Jose Luis Cruz (<joseluis.cruz@alquimia.ai>)
---
This chart is part of the Alquimia AI ecosystem and is geared towards companies looking to deploy scalable, secure and production-ready AI solutions on top of Kubernetes and OpenShift.