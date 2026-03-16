# Kubernetes ConfigMap & Secret Lab

This project demonstrates how to manage application configuration and sensitive data in Kubernetes using **ConfigMaps** and **Secrets**.
The lab shows how to separate configuration from application code and securely inject environment variables into Pods and Deployments.

---

## 📌 Objectives

* Create a **ConfigMap** using `kubectl`
* Inject ConfigMap values into Pods as environment variables
* Select specific keys from ConfigMap
* Create and manage **Kubernetes Secrets**
* Inject Secret values into Pods
* Combine **ConfigMap and Secret** in the same Pod
* Use ConfigMap and Secret in a **Deployment with multiple replicas**

---

## 🧰 Technologies Used

* Kubernetes
* kubectl
* YAML
* Busybox container

---

## 📂 Project Structure

```
kubernetes-configmap-secret-lab
│
├── 01-configmap.yaml
├── 02-configmap-env.yaml
├── 03-configmap-selective-env.yaml
├── 04-secret.yaml
├── 05-secret-env.yaml
├── 06-combined-config-secret.yaml
├── 07-deployment-config.yaml
│
└── README.md
```

---

## ⚙️ ConfigMap

A **ConfigMap** is used to store non-sensitive configuration data such as:

* Application environment
* Ports
* Feature flags
* Application settings

Example:

```
kubectl create configmap app-config \
--from-literal=APP_ENV=production \
--from-literal=APP_PORT=8080 \
--from-literal=APP_COLOR=blue \
--from-literal=MAX_CONNECTIONS=100
```

Verify the ConfigMap:

```
kubectl get configmap
kubectl describe configmap app-config
```

---

## 🔐 Secret

A **Secret** is used to store sensitive data such as:

* Database credentials
* API tokens
* Passwords

Example:

```
kubectl create secret generic db-secret \
--from-literal=DB_USER=admin \
--from-literal=DB_PASSWORD='p@ssw0rd123' \
--from-literal=DB_NAME=myapp
```

Check the secret:

```
kubectl get secret
kubectl describe secret db-secret
```

Note: Kubernetes stores Secret values encoded in **Base64**.

---

## 🚀 Inject ConfigMap into Pod

Example Pod that loads environment variables from a ConfigMap:

```
envFrom:
  - configMapRef:
      name: app-config
```

Verify environment variables inside the container:

```
kubectl exec env-pod -- env
```

---

## 🔑 Inject Secret into Pod

Example Pod loading database credentials from a Secret:

```
envFrom:
  - secretRef:
      name: db-secret
```

Verify variables:

```
kubectl exec db-pod -- env | grep DB
```

---

## 🔗 Combine ConfigMap & Secret

A Pod can load both configuration and sensitive data simultaneously:

```
envFrom:
  - configMapRef:
      name: app-config
  - secretRef:
      name: db-secret
```

---

## 📦 Deployment with ConfigMap & Secret

Example Deployment using both ConfigMap and Secret with **3 replicas**:

```
replicas: 3
```

Each Pod receives the same configuration automatically.

Verify Pods:

```
kubectl get pods
kubectl exec <pod-name> -- env
```

---

## 🧠 Key Concepts Learned

* Separating configuration from application code
* Managing application settings using ConfigMaps
* Securing sensitive data with Kubernetes Secrets
* Injecting environment variables into containers
* Scaling applications using Deployments
* Maintaining reusable and centralized configuration

---

## ✔️ Verification Commands

```
kubectl get pods
kubectl logs env-pod
kubectl exec env-pod -- env
kubectl exec selective-pod -- env
kubectl exec db-pod -- env | grep DB
kubectl exec fullapp-pod -- env | grep -E 'APP|DB'
```

---

## 📖 Summary

This lab demonstrates best practices for handling configuration and secrets in Kubernetes.
Using **ConfigMaps and Secrets** improves security, maintainability, and scalability when running containerized applications in a Kubernetes cluster.

---

⭐ Part of my Kubernetes learning journey.
