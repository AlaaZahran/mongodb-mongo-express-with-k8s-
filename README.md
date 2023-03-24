# mongodb-mongo-express-with-k8s-

# Project Title
Create mongodb pod  and mongo-express pod using k8s minkube cluster.



## Description 
This project aims to create a mongo-express  pod ( frontend ) that can accessible by web broswer and send http request to mongo-express external server (loadbalancer) to send request to mongo-express pod that role to forward traffic to mongodb internal service (cluster ip ) that load traffic to mongodb pod ( backend ) so you can create mongodb .


## Project diagram
![oiaegytz7mmzbbf6lyj8](https://user-images.githubusercontent.com/46306526/227519687-55c3c19e-bcfd-4228-9de4-5b103e454138.jpg)
## Mongo DB Steps
1- Create mongo-secret.yml to encrypt mongodb username and password using base64 encryption then apply file by :
```
kubectl apply -f mongo-secret.yml 
```
2- Create mongodb-deployment.yml which contain all mongodb specification like :

 *  name: mongodb
  * image: [mongo](https://hub.docker.com/_/mongo)
  * port : 27017
  * environment variable from mongo-secret.yml :

     * MONGO_INITDB_ROOT_USERNAME
     * MONGO_INITDB_ROOT_PASSWORD
then apply file by:
```
kubectl apply -f mongodb-deployment.yml 
```

3- Create mongodb-service.yml to create cluster ip  service which has : 

* Internal ip : inside cluster
* Ports:

   * service port: 27017
   * targetPort (mongodb pod): 27017
apply file by :
```
kubectl apply -f mongodb-service.yml 
```
Validate : 

![image](https://user-images.githubusercontent.com/46306526/227531446-8af11e87-b075-40c6-9268-ec40016e5cd8.png)

![image](https://user-images.githubusercontent.com/46306526/227530387-5dab1762-c99b-48ae-88f6-8bfafa882c0f.png)


## Mongo DB Steps
1- Create mongo-secret.yml to encrypt mongodb username and password using base64 encryption then apply file by :
```
kubectl apply -f mongo-secret.yml 
```
2- Create mongodb-deployment.yml which contain all mongodb specification like :

 *  name: mongodb
  * image: [mongo](https://hub.docker.com/_/mongo)
  * port : 27017
  * environment variable from mongo-secret.yml :

     * MONGO_INITDB_ROOT_USERNAME
     * MONGO_INITDB_ROOT_PASSWORD
then apply file by:
```
kubectl apply -f mongodb-deployment.yml 
```



3- Create mongo-express-service.yml to create loadbalancer service which has : 

* Internal ip : inside cluster
* External ip : public ip to asscess service 
* Ports:

   * service port: 8081
   * targetPort (mongo-express pod): 8081
   * Node port : specified for mongo-express pod to allow be accessible through internet with public ip
apply file by :
```
kubectl apply -f mongo-express-service.yml
```
Validate : 


![image](https://user-images.githubusercontent.com/46306526/227537987-fc0f7567-3c15-46c6-8396-2bf629f708d0.png)

![image](https://user-images.githubusercontent.com/46306526/227537184-c5f30826-c09a-4eea-9779-12b28771c80b.png)


## Test 
Assign external ip for mongo-express by :
 ```
 minikube service mongo-expree-service
```
 ![image](https://user-images.githubusercontent.com/46306526/227538634-04cb4c8a-8a59-4d6e-b222-c8ffa7f470d6.png)

validate 
![Screenshot 2023-03-24 131542](https://user-images.githubusercontent.com/46306526/227538912-8d444278-695d-4ce2-a870-4689920c2d8d.png)
