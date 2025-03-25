# **Alquimia Runtime Helm Chart**  

This Helm chart deploys **Alquimia Runtime**, an event-driven and serverless engine for managing AI agents in enterprise environments. Built on **Knative** and Kubernetes, it ensures scalable and efficient deployment.

## **Prerequisites**

- A running Openshift or Kubernetes cluster.

- Openshift Serverless (Knative) runtime installed.

- Openshift Service Mesh (Istio) for networking.

- AMQ Streams (Strimzi) for event-driven messaging.

- A Redis instance for memory and cache management

- A Couchdb instance to manage agent configurations

- Optional: Vector store (e.g., Qdrant, Chroma, or ElasticSearch) for RAG capabilities.

## **📦Helm chart repository**
The Helm chart is currently available on [Artifact Hub](https://artifacthub.io/packages/helm/alquimiaruntime/alquimia-runtime)
 and will soon be available in the **Red Hat catalog**.

## **📌 Installation**  

To install the chart with the default values:  

```bash
helm install my-release . --namespace alquimia-runtime
```
To override values:
```bash
helm install my-release . --namespace alquimia-runtime -f values.override.yaml
```

Or using --set:
```bash
helm install my-release . \
  --set services.hermes.image="alquimiaai/hermes:latest" \
  --set services.leviathan.image="alquimiaai/leviathan:latest"
```

## **⚙️ Configurable Parameters**  

The following table lists the configurable parameters of this chart and their default values.

### **1️⃣ Eventing Configuration (`eventing`)**  

| Parameter | Description | Default |
|-----------|-------------|---------|
| `eventing.brokers` | List of Knative `Broker` resources | See values.yaml |
| `eventing.configBroker.name` | Name of the ConfigMap for Kafka Broker | `kafka-broker-config` |
| `eventing.configBroker.namespace` | Namespace where the ConfigMap is located | `knative-eventing` |
| `eventing.triggers` | List of Knative `Trigger` resources | See values.yaml |
| `eventing.sequences` | List of Knative `Sequence` resources | See values.yaml |
| `eventing.sinkBindings` | List of Knative `SinkBinding` resources | See values.yaml |

#### **🔹 Example - Custom Broker Configuration**
```yaml
eventing:
  brokers:
    - name: inbound
      serviceRef: alquimia-hermes
    - name: normalized
      serviceRef: alquimia-hermes
```

### **2️⃣ Service Configuration (`services`)**  

| Parameter | Description | Default |
|-----------|-------------|---------|
| `services.hermes.name` | Name of the hermes service	 | `"alquimia-hermes"` |
| `services.hermes.image` | Image for hermes service	 | `""` |
| `services.leviathan.name` | Name of the leviathan service | `"alquimia-leviathan"` |
| `services.leviathan.image` | Image for leviathan service | `""` |

#### **🔹Example - Setting Custom Images**

```yaml
services:
  hermes:
    image: "alquimiaai/hermes:latest"
  leviathan:
    image: "alquimiaai/leviathan:latest"
```

### **3️⃣ Configuration Variables (`configuration`)**  

| Parameter | Description | Default |
|-----------|-------------|---------|
| `configuration.name` | Name of the ConfigMap | `"alquimia-env"` |
| `configuration.namespace` | Namespace for the ConfigMap | `"alquimia-runtime"` |


#### **🔹Environment Variables**
| Category | Variable | Description |
|-----------|-------------|---------|
| **General Configuration** | `DEBUG` | Enable or disable debug mode (`true`/`false`)|
|  |`API_URL`| Base URL of the API |
|   |`API_TOKEN`| Token for API authentication |
|  |`DEFAULT_COLLECTION_ID`| Default collection identifier |
| **Caching & Storage** |`ALQUIMIA_CACHE`| Cache configuration for Alquimia |
| |`TRANSFORMERS_CACHE`| Path for transformers cache |
| |`HF_HOME`| Path for Hugging Face models |
| |`SENTENCE_TRANSFORMERS_HOME`| Path for Sentence Transformers cache |
| **Object Storage (S3)** |`ALQUIMIA_S3_ACCESS_KEY_ID`| S3 access key ID |
| |`ALQUIMIA_S3_SECRET_KEY`| S3 secret access key |
| |`ALQUIMIA_S3_BUCKET`| S3 bucket name |
| |`ALQUIMIA_S3_ENDPOINT_URL`| S3 endpoint URL |
| **Database & Message Brokers** |`REDIS_URL`| Redis connection URL |
| |`COUCHDB_URL`| CouchDB connection URL |
| |`QDRANT_URL`| Qdrant vector database URL |
| |`QDRANT_API_KEY`| API key for Qdrant authentication |
| **Observability & Monitoring** |`APM_SERVER_URL`| APM server URL |
| |`APM_SECRET_TOKEN`| Secret token for APM authentication |
| **Email Configuration** |`SMTP_SERVER`| SMTP server for sending emails |
| |`IMAP_SERVER`| IMAP server for receiving emails |
| |`GMAIL_USERNAME`| Gmail username |
| |`GMAIL_PASSWORD`| Gmail password |
| **Integrations** |`SLACK_BOT_TOKEN`| Slack bot authentication token |


#### **🔹Example - Custom Environment Variables**
```yaml
configuration:
  env:
    API_URL: "https://api.alquimia.ai"
    DEBUG: "true"
```

## **🛠️ Usage Examples**  

### **🚀 Deploy with Custom Images and Configurations**  
```bash
helm install my-alquimia-app . \
  --set services.hermes.image="alquimiaai/hermes:latest" \
  --set services.leviathan.image="alquimiaai/leviathan:latest" \
  --set configuration.env.DEBUG="true"
```

### **🔄 Upgrade an Existing Deployment**  
```bash
helm upgrade my-alquimia-app . -f values.override.yaml
```

### **🗑️ Uninstall**  
```bash
helm uninstall my-alquimia-app
```
