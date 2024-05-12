## Kubernetes Node Metrics Collection with Helm

**This project utilizes Helm charts to deploy a solution that collects node metrics such as CPU, memory, and disk usage from Kubernetes nodes and stores them in individual files.**

**Helm Charts:**

Helm is a package manager for Kubernetes that simplifies application deployment and management. This project leverages Helm charts to package and deploy the necessary resources for collecting node metrics.

**Components:**

1. **Node Exporter:** A Prometheus exporter for hardware and OS metrics, deployed as a DaemonSet on all nodes.

2. **Metrics Collection Cronjob:** A Kubernetes cronjob scheduled to run at regular intervals. It executes a containerized script to collect node metrics and store them in files.

3. **Persistent Volume Claim (PVC):** Ensures the collected metrics data persists even on pod restarts.

**Prerequisites (for reference):**

- Metric Server (collects resource metrics for pods and nodes)
- EBS CSI Driver Addon (manages Amazon EBS volumes)
- Bastion Server (optional, for secure access)
- Python (for scripting and automation)

**Deployment with Helm:**

1. **Prerequisites:**

   - Ensure Helm is installed and configured on your local machine or within your cluster.

2. **Helm Charts:**

   The project directory contains two Helm charts:

     - `metrics-collector`: This chart deploys the cronjob and PVC for collecting and storing node metrics.
     - `node-exporter`: This chart deploys Node Exporter as a DaemonSet on all nodes.

3. **Deployment Steps:**

   - Navigate to the project directory containing the Helm charts.
   - Deploy the `node-exporter` chart first:

     ```bash
     helm install node-exporter .
     ```

   - Deploy the `metrics-collector` chart:

     ```bash
     helm install metrics-collector .
     ```

4. **Verification:**

   - Use `kubectl get pods` to check the status of pods from both charts and ensure they are running.
   - Access the Node Exporter URL (typically `http://<node-exporter-ip>:<port>/metrics`) to verify it exposes metrics.

**File Structure:**
<img width="452" alt="image" src="https://github.com/ali509/EKS-node-metrics-analysis-01/assets/39634565/30bd055e-a5c7-4aea-aec2-d366765bfea8">

node-exporter/ (Exports all node matrices to the Node Exporter URL)
|-- Chart.yaml 
|-- values.yaml 
|-- templates/
|   |-- configmap.yaml (Environment variables for Node Exporter)
|   |-- daemonset.yaml (defines the Node Exporter deployment as a DaemonSet)
|   |-- service.yaml (service of type LB for Node Exporter)
metrics-collector/ (Collect node metrics from node-exporter and store them in files)
|-- Chart.yaml
|-- values.yaml
|-- templates/
|   |-- cronjob.yaml (defines the cronjob for collecting metrics)
|   |-- pod.yaml (Runs a simple application to keep the pod running.)
|   |-- pvc.yaml (defines the PVC for persistent storage)


**Validation and Monitoring**

- Verify metric data collection and storage in the PVC.
- Access the pod created by `nodemetrics.yaml` to view collected data.
