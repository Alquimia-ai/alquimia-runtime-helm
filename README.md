# **Alquimia Runtime Helm Chart**  

This Helm chart deploys the **Alquimia Runtime** application, including its event-driven components using **Knative Eventing** and **Knative Serving**.  

## **prerequisites**
Istio Mesh
Knative(Serving, Eventing y Kafka)

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
| Key | Description | Default |
|-----------|-------------|---------|
| `HF_HOME` | Path for Hugging Face storage	| `""` |
| `QDRANT_API_KEY` | API Key for Qdrant	| `""` |
| `REDIS_URL` | Redis connection string	| `""` |
| `ALQUIMIA_S3_ACCESS_KEY_ID` | S3 Access Key | `""` |
| `GMAIL_USERNAME` | Gmail username for email service | `""` |
| `GMAIL_PASSWORD` | Gmail password | `""` |
| `DEBUG` | Debug mode (`true`/`false`)	| `""` |

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