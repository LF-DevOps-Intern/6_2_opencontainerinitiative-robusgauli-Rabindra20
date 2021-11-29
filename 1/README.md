### 1. Deploy Postgres database using PVC & PV cluster
Steps:<br/>
Create postgres-configmap.yml file <br/>
```
nano postgres-configmap.yml
```
Add below line code in postgres-configmap.yml file and save<br/>
```
apiVersion: v1
kind: ConfigMap
metadata:
 name: postgres-configmap
 labels:
  app: postgres
data:
 POSTGRES_DB: postgresdb
 POSTGRES_USER: rabindra
 POSTGRES_PASSWORD: rabindra@1
```
![configmapyml](https://user-images.githubusercontent.com/53372486/143772049-b48b40a7-791b-4d49-adb7-6c8d56c2db08.png)<br/>

Run postgres-configmap.yml file<br/>
```
kubectl create -f postgres-configmap.yml
```
![configmap](https://user-images.githubusercontent.com/53372486/143772048-f22fef57-85dd-4ec1-be2f-650591a1855c.png)<br/>

Create postgres-storage.yml file<br/>
```
sudo nano postgres-storage.yml
```
Add below line code in postgres-storage.yml file and save<br/>
```
apiVersion: v1
kind: PersistentVolume
metadata:
 name: postgres-pv
 labels:
  type: local
  app: postgres
spec:
 storageClassName: manual
 capacity:
  storage: 5Gi
 accessModes:
  - ReadWriteMany
 hostPath:
  path: "/mnt/data"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
 name: postgres-pvclaim
 labels:
  app: postgres
spec:
 storageClassName: manual
 accessModes:
  - ReadWriteMany
 resources:
  requests:
   storage: 2Gi
```
![storageyml](https://user-images.githubusercontent.com/53372486/143772046-4d81992c-057d-4715-aaa4-72e180e1ae22.png)<br/>

Run postgres-storage.yml file<br/>
```
kubectl create -f postgres-storage.yml
```
![storage](https://user-images.githubusercontent.com/53372486/143772045-e5ba2bf0-d8f5-457a-a9c6-058d614acb36.png)<br/>

Create postgres-deployment.yml file<br/>
```
nano postgres-deployment.yml
```
Add below line code in postgres-deployment.yml file and save<br/>
```
apiVersion: apps/v1 
kind: Deployment
metadata:
 name: postgres-deployment
spec:
 replicas: 1
 selector:
  matchLabels:
   app: postgres
 template:
  metadata:
    labels:
     app: postgres
  spec:
   containers:
    - name: postgres
      image: postgres:10.4
      imagePullPolicy: "IfNotPresent"
      ports:
       - containerPort: 8789
      envFrom:
       - configMapRef:
          name: postgres-configmap
      volumeMounts:
       - mountPath: /var/lib/postgresql/data
         name: postgredb
   volumes:
   - name: postgredb
     persistentVolumeClaim:
      claimName: postgres-pvclaim
```
![deploymyml](https://user-images.githubusercontent.com/53372486/143772051-efabff0d-a49d-42d4-a1bb-60cd1ddc078f.png)<br/>

Run postgres-deployment.yml file
```
kubectl create -f postgres-deployment.yml
```
![deploym](https://user-images.githubusercontent.com/53372486/143772050-ba2efe82-192b-475e-ba87-fdc49cadf22d.png)<br/>

Create postgres-service.yml file<br/>
```
nano postgres-service.yml
```
Add below line code in postgres-service.yml file and save<br/>
```
apiVersion: v1
kind: Service
metadata:
 name: postgres
 labels:
  app: postgres
spec:
 type: NodePort
 ports:
 - port: 8789
 selector:
  app: postgres
```
![serviceyml](https://user-images.githubusercontent.com/53372486/143772044-f70b9ebd-d634-4469-a97f-e65fd6991a23.png)<br/>

Run postgres-service.yml file
```
kubectl create -f postgres-service.yml 
```
![service](https://user-images.githubusercontent.com/53372486/143772042-91c287ae-03a2-4c6f-ac2a-35cd150a68ae.png)<br/>

Checking svc <br/>
```
kubectl get svc
```
![geysvc](https://user-images.githubusercontent.com/53372486/143772039-5f5edccf-1894-4dd0-85f2-683d43c0805b.png)<br/>

Checking pv<br/>
```
kubectl get pv
```
![getpv](https://user-images.githubusercontent.com/53372486/143772033-a0d301aa-39e4-487a-881e-409f252e2163.png)<br/>

Checking pvc <br/>
```
kubectl get pvc
```
![getpvc](https://user-images.githubusercontent.com/53372486/143772037-b2103c99-2859-45ee-9a52-cdaed9a22e15.png)<br/>

Checking pods<br/>
```
kubectl get pods
```
![getpod](https://user-images.githubusercontent.com/53372486/143772061-a9c2b3d5-6ad8-45d5-994f-8ca952324c6f.png)<br/>

Checking cluster-info<br/>
```
kubectl cluster-info
```
![info](https://user-images.githubusercontent.com/53372486/143772040-1390e199-353b-4287-962b-fe2b07937a83.png)<br/>








