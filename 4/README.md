### 4. Create a database(internship) and few tables in database.
Steps:<br/>
For creating database<br/>
```
CREATE DATABASE internship;
```
![create database](https://user-images.githubusercontent.com/53372486/143779076-7016eda1-2135-4b28-b55c-25c59c3c722f.png)<br/>

For checking database<br/>
```
\l
```
![l](https://user-images.githubusercontent.com/53372486/143779073-bf76c842-4105-4663-a77b-a31680021299.png)<br/>

Use database<br/>
```
\c internship
```
![c](https://user-images.githubusercontent.com/53372486/143779075-a3c58fff-db9b-4480-9602-e819a6f87eca.png)<br/>

Create tables<br/>
```
CREATE TABLE Devops(
ID INT PRIMARY KEY NOT NULL,
DEPT CHAR(50) NOT NULL,
EMP_ID INT NOT NULL
);
CREATE TABLE
internship=# CREATE TABLE Developer(
ID INT PRIMARY KEY NOT NULL,
DEPT CHAR(50) NOT NULL,
EMP_ID INT NOT NULL
);
```
![createt table](https://user-images.githubusercontent.com/53372486/143779078-910d1d83-8742-4f38-909f-81b10cfa4e8d.png)<br/>

Checking table<br/>
```
\d
```
![d](https://user-images.githubusercontent.com/53372486/143779071-63c3707b-e83b-4aa0-a20c-f2635cfd612d.png)<br/>



