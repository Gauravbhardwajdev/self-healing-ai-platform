🚀 Self-Healing AI Inference Platform

### *GitOps-Driven, Cloud-Native & Programmable Infrastructure for Scalable ML Inference*

An enterprise-grade AI inference platform that treats infrastructure as code, Kubernetes as the control plane, and Git as the single source of truth. Designed with GitOps, event-driven architecture, cloud-native MLOps, and automated self-healing to deliver resilient, scalable, and observable machine learning workloads.

---

## ✨ Key Highlights

* **🔄 GitOps-based Continuous Reconciliation** with Argo CD
* **☁️ Programmable Cloud Infrastructure** using Crossplane & Pulumi
* **⚡ High-throughput Event Streaming** via Apache Kafka
* **🤖 Production-grade ML Model Serving** using KServe & FastAPI
* **📊 End-to-End Observability** with OpenTelemetry, Prometheus & Grafana
* **🛡️ Self-Healing Infrastructure & Applications**
* **📈 Automatic Scaling** based on real-time workload metrics

---

## 🏗️ Architecture

```
+----------------------+
|     Git Repository   |
+----------+-----------+
           |
           v
    Argo CD (GitOps)
           |
+----------+-----------+
|                      |
v                      v
Kubernetes Cluster   Crossplane ----> Cloud Resources (S3, DBs, Networks)
|
+---> Kafka Topic
       |
       v
    Inference Worker (FastAPI Consumer)
       |
       v
    KServe Models <----> OpenTelemetry SDK <----> Prometheus + Grafana
                                                        |
                                                        v
                                            Autoscaling & Self-Healing
```



## 🎯 Platform Workflow

1️⃣ Declarative Infrastructure

Infrastructure is described as Kubernetes Custom Resources instead of static Terraform templates. Crossplane provisions and continuously manages:

* Storage Buckets
* Databases
* Networking
* Cloud Resources

2️⃣ GitOps Reconciliation

Argo CD continuously compares the desired state in Git with the live Kubernetes cluster. Whenever drift is detected:

* Infrastructure is restored automatically
* Applications are resynchronized
* Manual configuration changes are reverted instantly

3️⃣ Event-Driven Inference

Instead of sending inference requests directly to model servers, requests follow a decoupled path:
Plaintext

```
Client ──> Kafka Topic ──> Inference Worker ──> KServe Model
```
Benefits: Zero dropped requests, seamless handling of flash traffic spikes, and complete loose coupling.

4️⃣ Cloud-Native Model Serving

Inference workers consume Kafka messages and forward them to dynamically deployed KServe models.

Features: Scale-to-zero capabilities, high throughput, and Kubernetes-native orchestration.

5️⃣ Observability & Feedback Loop

Every request is instrumented using OpenTelemetry. Metrics flow dynamically to inform runtime adjustments:
Plaintext
```
Application
     ↓
OpenTelemetry
     ↓
Prometheus
     ↓
Grafana
     ↓
Autoscaling Decisions
```

| Layer                       | Technology          | Purpose                           |
| --------------------------- | ------------------- | --------------------------------- |
| **Infrastructure**          | Crossplane + Pulumi | Programmable cloud infrastructure |
| **GitOps**                  | Argo CD             | Continuous reconciliation         |
| **Container Orchestration** | Kubernetes          | Cluster management                |
| **Messaging**               | Apache Kafka        | Asynchronous event streaming      |
| **Inference API**           | FastAPI             | Kafka consumer & request routing  |
| **Model Serving**           | KServe              | Scalable ML inference             |
| **Observability**           | OpenTelemetry       | Distributed tracing               |
| **Metrics**                 | Prometheus          | Monitoring                        |
| **Visualization**           | Grafana             | Dashboards & alerting             |

📂 Repository Structure
```
.
├── .github/
│   └── workflows/          # CI pipelines
├── cluster-provisioning/    # Pulumi base infrastructure setup
├── platform-infra/         # Crossplane infrastructure manifests
├── apps/
│   ├── inference-worker/   # FastAPI service & Kafka consumer
│   └── manifests/          # Argo CD application wrappers
└── monitoring/             # OpenTelemetry, Prometheus & Grafana configs
```

## ⚙️ Self-Healing Scenarios

🛡️ Scenario 1 — Infrastructure Drift

Failure: A cloud resource (Database/S3 Bucket) is accidentally deleted or altered directly from the cloud console.
Recovery: Crossplane instantly detects the out-of-band drift, compares it against the Kubernetes CRDs, and recreates the resource to its proper state automatically.

🚀 Scenario 2 — Massive Traffic Spike
Failure: Hundreds of thousands of inference requests arrive simultaneously.

Recovery
```
Incoming Requests
        ↓
      Kafka Queue
        ↓
 Consumer Lag Metrics
        ↓
 Prometheus
        ↓
 Kubernetes HPA
        ↓
 KServe Pods
```
Result:

* No request loss
* Automatic scaling (e.g. 2 → 20 pods)
* Graceful scale-down after traffic subsides

💥 Scenario 3 — Application Crash
Failure: The inference worker crashes due to an unexpected runtime exception.

Recovery
* Kubernetes Liveness Probe detects failure
* Pod is removed from service
* Kubernetes launches a replacement pod
* Kafka retains pending messages
* Processing resumes without data loss

---
🚀 Getting Started
Prerequisites
* Kubernetes Cluster (EKS, GKE, AKS, or kind)
* kubectl
* Helm v3
* Docker
* Git

1. Install Crossplane
```
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane \
  crossplane-stable/crossplane \
  --namespace crossplane-system \
  --create-namespace
```

2. Install Argo CD
```
kubectl create namespace argocd

kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Bootstrap the Platform
```
kubectl apply -f apps/manifests/root-application.yaml
```

---
## 📈 Production Features
* ✅ GitOps Continuous Deployment
* ✅ Infrastructure Drift Detection
* ✅ Event-Driven Architecture
* ✅ Cloud-Native Model Serving
* ✅ Horizontal Pod Autoscaling
* ✅ Distributed Tracing
* ✅ Automated Recovery
* ✅ Zero-Downtime Deployments 
* ✅ Infrastructure as Kubernetes APIs
* ✅ End-to-End Observability

---
## 🔮 Future Enhancements
* 🚧 Multi-cluster GitOps deployments
* 🚧 Service Mesh (Istio/Linkerd)
* 🚧 GPU Autoscaling
* 🚧 Canary & Blue-Green Deployments
* 🚧 Argo Rollouts Integration
* 🚧 Multi-region Disaster Recovery
* 🚧 ML Model Registry Integration
* 🚧 Policy Enforcement with Kyverno


