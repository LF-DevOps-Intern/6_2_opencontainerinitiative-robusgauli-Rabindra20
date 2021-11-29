### 3. connect Postgres database from Postgres Client using core-dn's host name.
Steps:<br/>
Excute postgresql<br/>
```
kubectl exec -it postgresql-deployment-7594d8974f-tgtqd --namespace=internship bash
```
![connect postgres](https://user-images.githubusercontent.com/53372486/143778501-2697e0b0-c4e3-45b5-86f9-6b9bd93dd559.png)<br/>

Connect postgres<br/>
```
psql -h postgres.default -p 8789:31561 -U rabindra -d  postgres -W
```
![connect](https://user-images.githubusercontent.com/53372486/143778504-2f9ed627-acb6-44af-acd7-01235dd741b6.png)<br/>