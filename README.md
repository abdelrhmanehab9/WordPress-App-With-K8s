# 🚀 Deploying a Wordpress Application using Kubernetes (Wordpress-app-using-K8s)

This repository contains the necessary Kubernetes YAML manifest files to deploy a complete and stable **Wordpress** application, backed by a **MySQL** database, onto any Kubernetes cluster. The project aims to demonstrate how to deploy multi-component applications, utilize Persistent Storage features, and manage sensitive data (Secrets) within a K8s environment.

---

## 🌟 Key Features

* **Integrated Deployment:** Includes Deployment and Service for both the Wordpress application and the MySQL database.
* **Persistence:** Uses `PersistentVolumeClaim` (PVC) and `PersistentVolume` (PV) to ensure that database data and Wordpress files are not lost upon container restarts.
* **Security:** Utilizes a `Secret` to securely store MySQL database credentials instead of embedding them directly in the Deployment files.
* **Resource Separation:** Application components are divided into separate YAML files for easier management and application.

---

## 🛠️ Prerequisites

For a successful deployment, the following requirements must be met:

1.  **A Running Kubernetes Cluster (K8s Cluster):** (e.g., Minikube, Kind, or a managed cloud cluster like GKE/EKS/AKS).
2.  **`kubectl` Command-line Tool:** Installed and configured to interact with your cluster.
3.  **Docker / Basic Kubernetes knowledge.**

---

## 📂 Project Structure

The repository contains the following files, organized based on the components (database and application) and the type of Kubernetes resource:

| File | Description |
| :--- | :--- |
| `mysql-secret.yaml` | Defines the **Secret** resource to securely store the MySQL database username and password. |
| `mysql-sc.yaml` | Defines the **StorageClass** for the database. |
| `mysql-pvc.yaml` | Defines the **PersistentVolumeClaim** for the MySQL database to ensure permanent data storage. |
| `mysql-svc.yaml` | Defines the **Service** (ClusterIP type) for the MySQL database, allowing the Wordpress application to access it. |
| `mysql-app.yaml` | Defines the **Deployment** to run the MySQL container. |
| `wp-sc.yaml` | Defines the **StorageClass** for the Wordpress application. |
| `wp-pv.yaml` | Defines the **PersistentVolume** for the Wordpress application. |
| `wp-pvc.yaml` | Defines the **PersistentVolumeClaim** for the Wordpress application to store application files (themes, plugins, uploads). |
| `wp-app.yaml` | Defines the **Deployment** to run the Wordpress application container. |
| `wp-svc.yaml` | Defines the **Service** (preferably NodePort or LoadBalancer type) to access the Wordpress application from outside the cluster. |

---

## 🚀 Deployment Steps

Follow these steps to deploy the Wordpress application and database onto your cluster:

### 1. Deploying the Database (MySQL)

The database components must be deployed in order, starting with security and storage:

```bash
# 1. Apply the Secret for credentials
kubectl apply -f mysql-secret.yaml

# 2. Apply the StorageClass and PersistentVolumeClaim
kubectl apply -f mysql-sc.yaml
kubectl apply -f mysql-pvc.yaml

# 3. Apply the Deployment and Service for the database
kubectl apply -f mysql-app.yaml
kubectl apply -f mysql-svc.yaml
2. Deploying the Wordpress Application
After ensuring the database is running, deploy the Wordpress application components:

Bash

# 1. Apply the StorageClass, PersistentVolume, and PersistentVolumeClaim
kubectl apply -f wp-sc.yaml
kubectl apply -f wp-pv.yaml
kubectl apply -f wp-pvc.yaml

# 2. Apply the Deployment and Service for the Wordpress application
kubectl apply -f wp-app.yaml
kubectl apply -f wp-svc.yaml
3. Verifying the Deployment Status
Check that all Pods are running correctly:

Bash

kubectl get pods
# You should see 'Running' for both MySQL and Wordpress Pods
And verify that the Services are created:

Bash

kubectl get svc
🌐 Accessing the Application
If the Service type for wp-svc.yaml is NodePort (common for local testing like Minikube):

Find the Node IP and the external port (NodePort) for the Service:

Bash

kubectl get svc wp-svc
You will find a port number in the 30000-32767 range. Visit your browser using the address: http://[Node-IP]:[NodePort]

If the Service type is LoadBalancer (common in public clouds):

Get the external IP address assigned to the Service:

Bash

kubectl get svc wp-svc
Visit the external IP address shown in the EXTERNAL-IP column.
