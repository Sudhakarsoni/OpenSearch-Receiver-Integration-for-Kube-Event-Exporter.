# 📦 Kube Event Exporter with OpenSearch Receiver

This guide provides step-by-step instructions to configure **Kube Event Exporter** to forward Kubernetes events to **OpenSearch** for observability and analysis.

---

## 🧾 Prerequisites

- A working **Kubernetes cluster**
- **Helm CLI** installed
- **kubectl** configured
- **OpenSearch** endpoint accessible
- Valid **OpenSearch credentials**

---

## 🛠️ Step 1: Add and Update Helm Repo

```bash
helm repo add kube-event https://charts.bitnami.com/bitnami
helm repo update
```
## ⚙️ Step 2: Create Custom values.yaml for OpenSearch
```bash
logLevel: error
logFormat: json
maxEventAgeSeconds: 10
kubeQPS: 60
kubeBurst: 60

 route:
   routes:
     # Route for OpenSearch and drop own-namespace and Normal events
     - drop:
         - namespace: "*event-exporter*"
         - type: "Normal"
       match:
         - receiver: "opensearch-receievr"

 receivers:
   - name: "opensearch-receievr"
     opensearch:
       hosts:
         - https://{Indexer_IP}:9200/

       index: kube-events
       indexFormat: "kube-events-{2006-01-02}"
       username: {Enter OpenSearch-Indexer_User}
       password: {Enter OpenSearch-Indexer_Pass}
       useEventID: true
       deDot: true
       layout:
       tls: 
         insecureSkipVerify: true
```
## 🚀 Step 3: Install with Custom Config
```bash
helm install kube-event-exporter bitnami/kube-event-exporter --namespace event-exporter --create-namespace
```
---

## 🤝 Contributing

Feel free to open issues or submit pull requests to improve this guide.

---

## 👨‍💻 Author

**Sudhakar Soni**
Linux & DevOps Engineer
GitHub: [@sudhakarsoni](https://github.com/sudhakarsoni)

---

## 📎 References

- [Bitnami Helm Chart](https://github.com/bitnami/charts/tree/main/bitnami/kube-event-exporter)
- [OpenSearch Doc](https://opensearch.org/docs/)