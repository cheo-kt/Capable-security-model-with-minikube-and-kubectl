# Capable-security-model-with-minikube-and-kubectl



This project was developed for the course **Plataformas 2** by **Sergio Fernando Florez Sanabria (A00396046)**.  

## **1. Project Overview**

This project shows a simple security model using **Minikube** and **Kubernetes**.
It includes a backend, a PostgreSQL database, and an Ingress to manage external access.
All resources are deployed inside the **`platforms`** namespace.

---

## **2. Folder Structure**

```
Capable-security-model-with-minikube-and-kubectl/
│
├── k8s/
│   ├── backend/
│   │   ├── backend-deployment.yaml
│   │   └── backend-service.yaml
│   ├── db/
│   │   ├── db-deployment.yaml
│   │   └── db-service.yaml
│   └── ingress/
│       └── ingress.yaml
│
└── README.md
```

---

## **3. Step-by-Step Setup**

### **Step 1: Start Minikube**

```bash
minikube start
```

### **Step 2: Create Namespace**

```bash
kubectl create namespace platforms
```

### **Step 3: Apply Database Deployment and Service**

```bash
kubectl apply -f k8s/db/db-deployment.yaml -n platforms
kubectl apply -f k8s/db/db-service.yaml -n platforms
```

### **Step 4: Apply Backend Deployment and Service**

```bash
kubectl apply -f k8s/backend/backend-deployment.yaml -n platforms
kubectl apply -f k8s/backend/backend-service.yaml -n platforms
```

### **Step 5: Apply Ingress**

```bash
kubectl apply -f k8s/ingress/ingress.yaml -n platforms
```

### **Step 6: Verify Resources**

```bash
kubectl get all -n platforms
kubectl get ingress -n platforms
```

---

## **4. Common Errors and Fixes**

### **Error 1: /bin/bash not found**

```
OCI runtime exec failed: exec failed: unable to start container process: exec: "/bin/bash": no such file or directory
```

**Cause:** The container image does not include bash.
**Fix:** Replace `/bin/bash` with `/bin/sh` in exec commands.

---

### **Error 2: Timed out waiting for the condition**

```
error: timed out waiting for the condition
```

**Cause:** The pod for testing network connections did not start on time.
**Fix:** Run the pod without `--rm` and check logs manually.

---

### **Error 3: ContainerCreating delay**

```
Error from server (BadRequest): container "debug-db" in pod "debug-db" is waiting to start: ContainerCreating
```

**Cause:** Kubernetes was still pulling the BusyBox image.
**Fix:** Wait a few seconds and check again with:

```bash
kubectl get pods -n platforms
```

---

## **5. Tests Performed**

### **Test 1: Get Backend Pod Name**

```bash
POD_B=$(kubectl get pods -n platforms -l app=backend -o jsonpath='{.items[0].metadata.name}')
```

### **Test 2: Check Connection from Backend to Database**

```bash
kubectl exec -n platforms -it $POD_B -- /bin/sh -c "apt-get update >/dev/null 2>&1 || true; nc -zv db-svc 5432 && echo OK || echo FAILED"
```

If `/bin/sh` is not available in the image, use a temporary pod for testing.

---

### **Test 3: Create a Debug Pod to Test Database Connection**

```bash
kubectl run debug-db -n platforms --image=busybox --restart=Never -- /bin/sh -c "nc -zv db-svc 5432 && echo OK || echo FAILED"
```

Check logs after the pod finishes:

```bash
kubectl logs -n platforms debug-db
```

**Expected Output:**

```
db-svc (10.x.x.x:5432) open
OK
```

---

### **Test 4: Verify Ingress Access**

Get Minikube IP:

```bash
minikube ip
```

Then access the backend:

```bash
curl http://<minikube-ip>/
```

If configured correctly, it should return a valid HTTP response from the backend.

---

## **6. Clean Up Resources**

When you finish the tests, delete all the resources to free cluster space:

```bash
kubectl delete namespace platforms
minikube stop
```

---

## **7. Final Result**

The deployment was successful.
The backend and database communicate correctly through the internal Kubernetes service **db-svc**.
The Ingress exposes the backend service using the Minikube IP.
All tests returned **OK**, confirming a stable and functional connection inside the cluster.

---
## **8. Photos**
<img width="705" height="410" alt="image" src="https://github.com/user-attachments/assets/e809406d-3ad3-45ea-9a49-9c3b5385180f" />
<img width="1600" height="851" alt="image" src="https://github.com/user-attachments/assets/b8069f7a-9438-4740-90ee-a3a59c856ebc" />
<img width="877" height="537" alt="image" src="https://github.com/user-attachments/assets/1d3ad028-1197-4020-a8fc-43aea4ba045f" />
<img width="1112" height="825" alt="image" src="https://github.com/user-attachments/assets/bead6bf2-caf3-4e30-90ee-d731a52e134d" />
<img width="1033" height="572" alt="image" src="https://github.com/user-attachments/assets/4f358fba-e292-4ab3-96d8-46258b273206" />
<img width="845" height="553" alt="image" src="https://github.com/user-attachments/assets/9c52fc10-1a75-49f4-ae0f-eb82522df93d" />
<img width="1546" height="416" alt="image" src="https://github.com/user-attachments/assets/52c28dbe-0d3d-491c-8873-39754016d1b8" />
<img width="1004" height="856" alt="image" src="https://github.com/user-attachments/assets/a914ed80-46ca-45cf-ae63-a9179056ecb2" />
!<img width="1271" height="631" alt="image" src="https://github.com/user-attachments/assets/fab2ce39-7c1c-4d2f-b014-4f1ff1b36dac" />
<img width="1600" height="322" alt="image" src="https://github.com/user-attachments/assets/aa2ad449-410c-4588-8250-3ce05897c1df" />
<img width="1600" height="557" alt="image" src="https://github.com/user-attachments/assets/3a4e492c-29a9-45bc-a154-bbdf5c5877ca" />
<img width="1600" height="435" alt="image" src="https://github.com/user-attachments/assets/38c8d8e4-28fd-411a-a778-c0b783eeaa0e" />

---
## ** 9. test**

<img width="1542" height="76" alt="image" src="https://github.com/user-attachments/assets/7c89d7c7-3549-4364-a1a9-bea4de73d2ef" />
<img width="1600" height="94" alt="image" src="https://github.com/user-attachments/assets/085ed64e-3448-412a-b47c-630f55769f02" />
<img width="1600" height="78" alt="image" src="https://github.com/user-attachments/assets/6a3b0650-4641-460e-a0ab-ab32f0e0d77a" />
<img width="1600" height="242" alt="image" src="https://github.com/user-attachments/assets/e33c143f-71cb-4128-ad2c-3beedb5ba7d4" />
<img width="1600" height="103" alt="image" src="https://github.com/user-attachments/assets/0f845841-07a5-4586-9a86-dc90a31e17dd" />
<img width="1473" height="152" alt="image" src="https://github.com/user-attachments/assets/100d4063-42c5-42b0-9526-a7e0bc4e91a5" />


















