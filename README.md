# **Alquimia Runtime Helm Chart**

Este Helm chart despliega la infraestructura base para el runtime de **Alquimia AI** sobre Kubernetes y OpenShift, utilizando tecnologías serverless y event-driven como **Knative Serving** y **Knative Eventing**. Está diseñado para empresas que buscan soluciones de IA escalables, seguras y listas para producción.

---

## **🚀 Características principales**
- Despliegue automatizado de microservicios de inferencia, evaluación y herramientas de Alquimia AI.
- Arquitectura orientada a eventos (Brokers, Triggers, Sequences, SinkBindings).
- Configuración flexible y personalizable mediante `values.yaml`.
- Integración avanzada con OpenShift y soporte para Kubernetes estándar (>=1.24).

---

## **📋 Requisitos previos**
- Kubernetes >= 1.24
- Helm 3.x
- Knative (Serving, Eventing y Kafka)
- Istio Mesh (recomendado para OpenShift)

---

## **📦 Instalación**

Instalación rápida desde el repositorio oficial:
```bash
helm repo add alquimia-ai https://www.alquimia.ai/helm-charts
helm install alquimia-runtime alquimia-ai/alquimia-runtime-helm --namespace alquimia --create-namespace
```

Instalación local:
```bash
helm install my-release . --namespace alquimia-runtime
```

Sobrescribir valores con archivo personalizado:
```bash
helm install my-release . --namespace alquimia-runtime -f values.override.yaml
```

O usando `--set`:
```bash
helm install my-release . \
  --set serving.services[0].image="alquimiaai/leviathan:latest" \
  --set serving.services[1].image="alquimiaai/hermes:latest"
```

---

## **⚙️ Parámetros configurables**

### **1️⃣ Configuración de Eventing (`eventing`)**

| Parámetro | Descripción | Valor por defecto |
|-----------|-------------|------------------|
| `eventing.brokers` | Lista de recursos Knative `Broker` | Ver values.yaml |
| `eventing.triggers` | Lista de recursos Knative `Trigger` | Ver values.yaml |
| `eventing.sequences` | Lista de recursos Knative `Sequence` | Ver values.yaml |
| `eventing.sinkBindings` | Lista de recursos Knative `SinkBinding` | Ver values.yaml |

#### **🔹 Ejemplo - Configuración personalizada de brokers**
```yaml
eventing:
  brokers:
    - name: inbound
      deadLetterUri: /deadletter/inbound
    - name: process
      deadLetterUri: /deadletter/process
```

### **2️⃣ Configuración de Servicios (`serving.services`)**

| Parámetro | Descripción | Valor por defecto |
|-----------|-------------|------------------|
| `serving.services` | Lista de microservicios Knative | Ver values.yaml |
| `serving.services[0].name` | Nombre del servicio | "alquimia-leviathan" |
| `serving.services[0].image` | Imagen del servicio | "alquimiaai/leviathan:latest" |
| `serving.services[1].name` | Nombre del servicio | "alquimia-hermes" |
| `serving.services[1].image` | Imagen del servicio | "alquimiaai/hermes:latest" |

#### **🔹 Ejemplo - Imágenes personalizadas**
```yaml
serving:
  services:
    - name: alquimia-leviathan
      image: "alquimiaai/leviathan:v2.0.0"
    - name: alquimia-hermes
      image: "alquimiaai/hermes:latest"
```

### **3️⃣ Variables de entorno y configuración (`serving.env`)**

| Clave | Descripción | Valor por defecto |
|-------|-------------|------------------|
| `DEBUG` | Modo debug (`true`/`false`) | "False" |
| `REDIS_HOST` | Host de Redis | "" |
| `REDIS_PASSWORD` | Contraseña de Redis | "" |
| `COUCHDB_URL` | URL de CouchDB | "" |
| `API_TOKEN` | Token de API | "" |
| ... | ... | ... |

#### **🔹 Ejemplo - Variables de entorno personalizadas**
```yaml
serving:
  env:
    API_URL: "https://api.alquimia.ai"
    DEBUG: "true"
```

Puedes ver todos los valores disponibles y sus descripciones en [`values.yaml`](./values.yaml).

---

## **🛠️ Recursos que despliega**

Este chart crea automáticamente los siguientes recursos de Kubernetes (usando Knative):

- **Brokers** (`eventing.knative.dev/v1`): canales de eventos para orquestar flujos.
- **Triggers** (`eventing.knative.dev/v1`): reglas para enrutar eventos a servicios.
- **Sequences** (`flows.knative.dev/v1`): flujos de pasos encadenados para procesamiento avanzado.
- **SinkBindings** (`sources.knative.dev/v1`): vinculan servicios a brokers de eventos.
- **ConfigMaps**: para variables de entorno y configuración.
- **Knative Services** (`serving.knative.dev/v1`): despliegue serverless de los microservicios de Alquimia AI.

---

## **🧩 Ejemplos de uso**

### **🚀 Despliegue con imágenes y configuración personalizada**
```bash
helm install my-alquimia-app . \
  --set serving.services[0].image="alquimiaai/leviathan:latest" \
  --set serving.services[1].image="alquimiaai/hermes:latest" \
  --set serving.env.DEBUG="true"
```

### **🔄 Actualizar un despliegue existente**
```bash
helm upgrade my-alquimia-app . -f values.override.yaml
```

### **🗑️ Desinstalar**
```bash
helm uninstall my-alquimia-app
```

---

## **📞 Soporte y contacto**
- **Sitio web:** [alquimia.ai](https://www.alquimia.ai/)
- **Soporte:** [https://www.alquimia.ai/](https://www.alquimia.ai/)
- **Mantenedor:** Jose Luis Cruz (<joseluis.cruz@alquimia.ai>)

---

Este chart es parte del ecosistema de Alquimia AI y está orientado a empresas que buscan desplegar soluciones de IA escalables, seguras y listas para producción sobre Kubernetes y OpenShift. 