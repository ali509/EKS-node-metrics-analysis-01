## EKS-node-metrics-analysis-01
Kubernetes Node Metrics Collection with Helm
This project utilizes Helm charts to deploy a solution that collects node metrics such as CPU, memory, and disk usage from Kubernetes nodes and stores them in individual files.

Helm Charts:

Helm is a package manager for Kubernetes that simplifies application deployment and management. This project leverages Helm charts to package and deploy the necessary resources for collecting node metrics.

Components:

Node Exporter: A Prometheus exporter for hardware and OS metrics, deployed as a DaemonSet on all nodes.

Metrics Collection Cronjob: A Kubernetes cronjob scheduled to run at regular intervals. It executes a containerized script to collect node metrics and store them in files.

Persistent Volume Claim (PVC): Ensures the collected metrics data persists even on pod restarts.

Deployment with Helm:

Prerequisites:

Ensure Helm is installed and configured on your local machine or within your cluster.
Helm Charts:

The project directory contains two Helm charts:

metrics-collector: This chart deploys the cronjob and PVC for collecting and storing node metrics.
node-exporter: This chart deploys Node Exporter as a DaemonSet on all nodes.
Deployment Steps:

Navigate to the project directory containing the Helm charts.

Deploy the node-exporter chart first:

Bash
helm install node-exporter . -n <namespace>  # Replace `<namespace>` with your desired namespace
Use code with caution.
content_copy
Deploy the metrics-collector chart:

Bash
helm install metrics-collector . -n <namespace>  # Replace `<namespace>` with your desired namespace
Use code with caution.
content_copy
Verification:

Use kubectl get pods to check the status of pods from both charts and ensure they are running.
Access the Node Exporter URL (typically http://<node-exporter-ip>:<port>/metrics) to verify it exposes metrics.
File Structure:

metrics-collector/
|-- Chart.yaml (defines the chart for metrics collection)
|-- values.yaml (customizable values for the metrics-collector chart)
|-- templates/
|   |-- cronjob.yaml (defines the cronjob for collecting metrics)
|   |-- pod.yaml (defines the pod for running the metrics collection script)
|   |-- pvc.yaml (defines the PVC for persistent storage)
node-exporter/
|-- Chart.yaml (defines the chart for Node Exporter deployment)
|-- values.yaml (customizable values for the Node Exporter chart)
|-- templates/
|   |-- configmap.yaml (configuration for Node Exporter)
|   |-- daemonset.yaml (defines the Node Exporter deployment as a DaemonSet)
|   |-- service.yaml (service definition for Node Exporter)
Previously Used Deployment Steps (for reference):

This section details the steps for deploying using kubectl commands, which you might find helpful for understanding the underlying concepts.

Note: This section remains for reference purposes; the recommended approach is deployment using Helm charts.

Prerequisites (for reference):

Metric Server (collects resource metrics for pods and nodes)
EBS CSI Driver Addon (manages Amazon EBS volumes)
Bastion Server (optional, for secure access)
Python (for scripting and automation)
Deployment Steps (using kubectl - for reference):

Build and Push Docker Image (if applicable)
Apply Manifest Files:
pvc.yaml for persistent storage
cronjob.yaml to schedule metrics collection
Additional manifest files (Node Exporter, etc.)
Sanity Checking (using kubectl get pods and kubectl describe pod)
Debugging and Logging (using kubectl logs)
Access Node Exporter URL
Validation and Monitoring

Verify metric data collection and storage in the PVC.
Access the pod created by nodemetrics.yaml to view collected data.
Additional Notes:

This approach leverages Helm charts for streamlined deployment and management.
The previous deployment steps (using kubectl) are provided for reference.
Consider customizing the Helm chart values for specific needs.
