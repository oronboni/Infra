[[_TOC_]]

## Prerequisites
- [Docker for Desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.18.6/bin/windows/amd64/kubectl.exe) (You might not need is, can be configured via Docker For Desktop. Just make sure you have kubectl on your machine)
- [Helm](https://github.com/helm/helm/releases/tag/v2.16.1) (Note that we still use Helm 2 for this lab, which is end of life soon)


#Overview
----
In his lab you are going to:
- Create local k8s cluster 
- Deploy 2 services
- setup access to your services by using ingress controller
- Automated the deployment with helm (Bonus milestone)

# Notes
----
- If you are connecting to any of k8s clusters, please make sure you are backup your kube.config 
usually located at 
- [x] WIN C:\Users\\<username>\.kube 
- [x] MAC/Linux ~/.kube/config

# Glossary
----
- k8s is kubernetes
 

# Task 1 - Enable kubernetes with docker-desktop
---
## 1. Install [docker-desktop](https://www.docker.com/products/docker-desktop) 
and follow the instructions.

## 2. Enable k8s within docker-desktop
- [x] open the dashboard (Right click on Docker icon & press on dashboard)
![image.png](image-09c459ea-ce12-4b99-b42b-444be486aadf.png)
![image.png](image-cd25c460-0d1b-4895-b748-d20d02a7126e.png)

 - [x] Press on settings and then Mark the checkbox for enable Kubernetes
![image.png](image-f14291ad-cce4-4c01-be5c-9abd13a9aec0.png)
- [x] Make sure k8s is up and running from UI
![image.png](image-b962fb5b-af69-4cd6-9852-fd7f3abaea97.png)
- [x] Make sure k8s is up and running from CLI
![image.png](image-39701dfe-a7e5-4245-a5ad-636cc176e265.png)

# Task 2 - Create namespace with kubectl command 
---

## 1. Create mdatp namespace yaml
Create a yaml file "namespace.yaml"
```
apiVersion: v1 
kind: Namespace 
metadata: 
  name: citi
```

## 2. Create & validate namespace
```
kubectl apply -f namespace.yaml
kubectl get namespaces
```
![image.png](namespace.PNG)

# Task 3 - Create Limits 
## 1. Create Limits yaml
create a yaml file "LimitsCreation.yaml"
```
apiVersion: v1
kind: LimitRange
metadata:
  name: limit-range
  namespace: citi
spec:
  limits:
  - defaultRequest:
      cpu: 100m
      memory: 256Mi
    max:
      cpu: 1000m
      memory: 3Gi
    min:
      cpu: 5m
      memory: 5Mi
    type: Container
```

## 2. Create & Validate the limits
```
kubectl apply -f LimitsCreation.yaml
kubectl get limits -n citi
kubectl describe limits -n citi limit-range
```
![image.png](limits.PNG)

# Task 4 - Create Services & Deployments
## 1. Create Deployment and services yaml
Create a yaml file "deployments-and-services.yaml"
```
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: coffee 
  namespace: citi
spec: 
  replicas: 2 
  selector: 
    matchLabels: 
      app: coffee 
  template: 
    metadata: 
      labels: 
        app: coffee 
    spec: 
      containers: 
      - name: coffee 
        image: nginxdemos/hello:plain-text 
        ports: 
        - containerPort: 80

--- 

apiVersion: v1 
kind: Service 
metadata: 
  name: coffee-svc 
  namespace: citi
spec: 
  ports: 
  - port: 80 
    targetPort: 80 
    protocol: TCP 
    name: http 
  selector: 
    app: coffee 
    
--- 
apiVersion: apps/v1

kind: Deployment 
metadata: 
  name: tea 
  namespace: citi
spec: 
  replicas: 3 
  selector: 
    matchLabels: 
      app: tea  
  template: 
    metadata: 
      labels: 
        app: tea  
    spec: 
      containers: 
      - name: tea  
        image: nginxdemos/hello:plain-text 
        ports: 
        - containerPort: 80

--- 

apiVersion: v1
kind: Service
metadata:
  name: tea-svc
  namespace: citi
  labels: 
spec: 
  ports: 
  - port: 80 
    targetPort: 80 
    protocol: TCP 
    name: http 
  selector: 
    app: tea 

```
## 2. Create & validate services and deployments
```
kubectl apply -f deployments-and-services.yaml
kubectl get svc -n citi
kubectl get deployments -n citi
```
![image.png](deployment.PNG)

# Task 5 - Cleanup 
## 1. Clean all the resources
```
kubectl delete namespace citi
```
## 2. Validate the cleanup
```
kubectl get ns
kubectl get pods -n citi
```
![image.png](image-4b77c7f3-0dae-45c2-af25-74f8d20a86a6.png)
# Task 6 - Use Helm to automate Tasks 2,3,4
## 1. Install Helm
- [x] Download the latest release[here](https://helm.sh/docs/intro/install/)
- [x] Extract to folder, and add the folder created to environment variables (path). 

## 2. Verify installation
```
helm version
```
![image.png](image-cea88f03-421b-45a0-b672-dd53c9c44b83.png)
## 3. Create your first helm chart
```
helm create my_first_helm_chart
```
![image.png](image-1b0eeff0-cc50-4281-98bf-f347f1b3d58e.png)

## 4. Cleanup helm initial directory
- [x] Clean values.yaml file, should be empty
- [x] templates directory should be empty
- [x] Remove all files except the files below
![image.png](image-b01d9682-b8a9-48d4-b94f-98e4863e22a2.png)
## 5. Copy the files to helm templates directory
![image.png](image-bf7c40f9-d958-4fdd-9d53-3f8638f4f954.png)
## 6. Install the helm chart
```
helm upgrade --install my-first-release my_first_release_chart
```
![image.png](image-89052a04-8f80-406d-a162-12861ad1e080.png)
## 7. Validate the helm chart installation
![image.png](image-7eb31d8a-8524-4a79-8669-dda6086edfaf.png)

## 8. Cleanup chart installation
```
helm del my-first-release
```
![image.png](image-717b4163-8da5-40bd-b3de-9ccb8b5712e1.png)


# Task 7 - Use Helm with values
## 1. Update your values.yaml with:
```
cafenamespace: test 
```
## 2. Find and replace mdatp to parameter
Go over all the files under templates and replace "mdatp" with "{{ .Values.cafenamespace }}"
## 3. Install the helm chart
```
helm upgrade --install my-first-release my_first_release_chart
```
## 4.Validate the helm chart installation
![image.png](image-d3da8da8-b19a-4721-82e6-4462522638b1.png)

# Task 8 - Create Ingress Controller
## 1. Setup ingress controller as a cluster “entry point” 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.22.0/deploy/mandatory.yaml 
```
## 2. Create nginx service yaml
Create a yaml file "ingress-nginx-service.yaml"
```
apiVersion: v1 
kind: Service 
metadata: 
  name: ingress-nginx 
  namespace: ingress-nginx 
  labels: 
    app.kubernetes.io/name: ingress-nginx 
    app.kubernetes.io/part-of: ingress-nginx 
spec: 
  type: NodePort 
  ports: 
    - name: http 
      port: 80 
      targetPort: 80 
      protocol: TCP 
    - name: https 
      port: 443 
      targetPort: 443 
      protocol: TCP 
  selector: 
    app.kubernetes.io/name: ingress-nginx 
    app.kubernetes.io/part-of: ingress-nginx 

```
## 3. create the nginx service and get the service port
```
kubectl apply -f ingress-nginx-service.yaml
kubectl get svc -n ingress-nginx 
```
![image.png](image-64b292ef-f00e-4717-9a4b-514679f5324c.png)

## 4. Create Ingress rules yaml file

Create file "ingress.yaml"
```
apiVersion: apps/v1
kind: Ingress 
metadata: 
  name: coffee-ingress 
  namespace: citi 
  annotations: 
    kubernetes.io/ingress.class: nginx 
    nginx.ingress.kubernetes.io/rewrite-target: /$1 
spec: 
  rules: 
  - host: cafe.com 
    http: 
      paths:  
      - backend: 
          serviceName: coffee-svc 
          servicePort: 80 
        path: /coffee   
--- 
 
apiVersion: apps/v1
kind: Ingress 
metadata: 
  name: tea-ingress 
  namespace: citi 
  annotations: 
    kubernetes.io/ingress.class: nginx 
    nginx.ingress.kubernetes.io/rewrite-target: /$1 
spec: 
  rules: 
  - host: cafe.com 
    http: 
      paths:  
      - backend: 
          serviceName: tea-svc 
          servicePort: 80 
        path: /tea
```
## 5. Create ingress rules
```
kubectl apply -f ingress.yaml
```
![image.png](image-4396c7e6-2fde-40ad-8039-3c2de3dd415d.png)

## 6. Validate the ingress rules
### Update hosts file
add new record to hosts file (C:\Windows\System32\drivers\etc\hosts or /etc/hosts) 
```
127.0.0.1 cafe.com 
```

### troubleshoot for hosts file update
In case of cafe.com not redirect to 127.0.0.1
- [x] Edge browser - then try flushing your system's DNS cache (ipconfig /flushdns on Windows).
- [x] Chrome browser - has its own DNS cache. please visit chrome://net-internals/#sockets and click "Flush socket pools".

### Test the ingress rules
try to access from your browser GET HTTP option you want  (curl/postman/etc)
you should see the nginx response and the round robin change in the server-name/server-ip
- [x]  Browser
----
### Find the relevant port (in this case 32275)
```
kubectl get svc -n ingress-nginx
```
![image.png](image-1b5d62f1-3984-47cf-80f9-eacc7e9f76cf.png)
----
- http://cafe.com:<port>/coffee (in our case http://cafe.com:32275/coffee)
- http://cafe.com:<port>/tea (in our case http://cafe.com:32275/tea)
![image.png](image-9cd61252-e37b-4fcb-9c3c-24491433d820.png)
- [x] Postman extenstion
----
You can also user postman (chrome extension)
- http://cafe.com:32275/tea
![image.png](image-814b3745-9b88-4bdf-91b0-46f0bb79a920.png)
- http://cafe.com:32275/coffee
![image.png](image-0af319a5-1f80-40ab-86f7-c6ebccb840c8.png)


