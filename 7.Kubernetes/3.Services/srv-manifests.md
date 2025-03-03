
## 01-backend-deployment.yml

```yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-restapp
  labels: 
    app: backend-restapp
    tier: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend-restapp
  template: 
    metadata:
      labels:
        app: backend-restapp
        tier: backend
    spec:
      containers:
        - name: backend-restapp
          image: stacksimplify/kube-helloworld:1.0.0
          ports:
            - containerPort: 8080
            
```

## 02-backend-clusterip-service.yml

```yml

apiVersion: v1
kind: Service
metadata:
  name: my-backend-service ## VERY VERY IMPORTANT - NGINX PROXYPASS needs this name
  labels: 
    app: backend-restapp
    tier: backend
spec:
  #type: Cluster IP is a default service
  selector:
    app: backend-restapp
  ports: 
    - name: http
      port: 8080 # ClusterIp Service Port
      targetPort: 8080 # Container Port

```

## 03-frontend-deployment.yml

```yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-nginxapp
  labels: 
    app: frontend-nginxapp
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend-nginxapp
  template: 
    metadata:
      labels:
        app: frontend-nginxapp
        tier: frontend
    spec:
      containers:
        - name: frontend-nginxapp
          image: stacksimplify/kube-frontend-nginx:1.0.0
          ports:
            - containerPort: 80

```

## 04-frontend-nodeport-service.yml

```yml


apiVersion: v1
kind: Service
metadata:
  name: frontend-nginxapp-nodeport-service
  labels: 
    app: frontend-nginxapp
    tier: frontend     
spec:
  type: NodePort 
  selector:
    app: frontend-nginxapp
  ports: 
    - name: http
      port: 80
      targetPort: 80
      nodePort: 31234


```
