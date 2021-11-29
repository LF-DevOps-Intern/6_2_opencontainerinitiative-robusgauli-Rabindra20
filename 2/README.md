### 2. Deploy Postgres Client in cluster(psql).
Steps:
Create postgresql-configmap.yml file <br/>
```
nano postgresql-configmap.yml
```
Add below line code in postgresql-configmap.yml file and save<br/>
```
apiVersion: v1
kind: ConfigMap
metadata:
 name: postgresql-configmap
 labels:
  app: postgresql
data:
 POSTGRES_DB: postgresdb
 POSTGRES_USER: rabindra
 POSTGRES_PASSWORD: rabindra@1
```
![configyml](https://user-images.githubusercontent.com/53372486/143779508-cbfe678b-f483-453b-bb9d-6dc287e66a61.png)<br/>

Run postgresql-configmap.yml file<br/>
```
kubectl create -f postgresql-configmap.yml --namespace=internship
```
![config](https://user-images.githubusercontent.com/53372486/143779522-a32334cc-5f98-447d-8ec7-588ba65feda4.png)<br/>

Create postgresql-storage.yml file<br/>
```
sudo nano postgresql-storage.yml
```
Add below line code in postgresql-storage.yml file and save<br/>
```
apiVersion: v1
kind: PersistentVolume
metadata:
 name: postgresql-pv
 labels:
  type: local
  app: postgresql
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
 name: postgresql-pvclaim
 labels:
  app: postgresql
spec:
 storageClassName: manual
 accessModes:
  - ReadWriteMany
 resources:
  requests:
   storage: 2Gi
```
![storageyml](https://user-images.githubusercontent.com/53372486/143779519-5b6dffa6-bba2-44eb-bea7-2a54f198b807.png)<br/>

Run postgresql-storage.yml file<br/>
```
kubectl create -f postgresql-storage.yml --namespace=internship
```
![storage](https://user-images.githubusercontent.com/53372486/143779518-551b236c-395b-4758-857d-b55433cbfab6.png)<br/>

Create postgresql-deployment.yml file<br/>
```
nano postgresql-deployment.yml
```
Add below line code in postgresql-deployment.yml file and save<br/>
```
apiVersion: apps/v1 
kind: Deployment
metadata:
 name: postgresql-deployment
spec:
 replicas: 1
 selector:
  matchLabels:
   app: postgresql
 template:
  metadata:
    labels:
     app: postgresql
  spec:
   containers:
    - name: postgresql
      image: postgres:10.4
      imagePullPolicy: "IfNotPresent"
      ports:
       - containerPort: 8686
      envFrom:
       - configMapRef:
          name: postgresql-configmap
      volumeMounts:
       - mountPath: /var/lib/postgresql/data
         name: postgredb
   volumes:
   - name: postgredb
     persistentVolumeClaim:
      claimName: postgresql-pvclaim
```
![deployml](https://user-images.githubusercontent.com/53372486/143779510-f20ff592-64d3-44e2-87aa-dc278f1d79a4.png)<br/>

Run postgresql-deployment.yml file
```
kubectl create -f postgresql-deployment.yml --namespace=internship
```
![deplo](https://user-images.githubusercontent.com/53372486/143779509-cde6dc52-1d02-4cd3-ab91-81a3b1d59af8.png)<br/>

Create postgresql-service.yml file<br/>
```
nano postgresql-service.yml
```
Add below line code in postgresql-service.yml file and save<br/>
```
apiVersion: v1
kind: Service
metadata:
 name: postgresql
 labels:
  app: postgresql
spec:
 type: NodePort
 ports:
 - port: 8686
 selector:
  app: postgresql
```
![serviceyml](https://user-images.githubusercontent.com/53372486/143779517-ac24c212-5082-4cff-927a-704bafc9e521.png)<br/>

Run postgresql-service.yml file
```
kubectl create -f postgresql-service.yml --namespace=internship
```
![service](https://user-images.githubusercontent.com/53372486/143779516-7d55d981-f2ac-48ab-91ab-221f030c0196.png)<br/>

Checking svc <br/>
```
kubectl get svc
```
![svc](https://user-images.githubusercontent.com/53372486/143779521-bbdb3219-697b-4d89-ae08-394aaed09086.png)<br/>

Checking pv<br/>
```
kubectl get pv
```
![pv](https://user-images.githubusercontent.com/53372486/143779513-c5e2c656-b6fa-4057-b673-6594575fa634.png)<br/>

Checking pvc <br/>
```
kubectl get pvc
```
![pvc](https://user-images.githubusercontent.com/53372486/143779515-2b9b5c31-67ab-4d79-b276-4d5f7d29ac64.png)<br/>

Checking pods<br/>
```
kubectl get pods --namespace=internship
```
![pods](https://user-images.githubusercontent.com/53372486/143779512-a94f52fa-5973-4980-96e9-c83e5a4c8f37.png)<br/>

Checking All<br/>
```
kubectl get all --namespace=internship
```
![getall](https://user-images.githubusercontent.com/53372486/143779511-1c767c09-e142-478e-9fc3-ed819e9af0d0.png)<br/>











