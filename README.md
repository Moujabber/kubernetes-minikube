# kubernetes-minikube
#### Kubernetes : est un logiciel qui permet de déployer les applications, les envoyer s’exécuter dans une infrastructure cloud 
Kubernetes :    
•	Cherche des images docker et les envoient s’exécuter : deploiement en application     
•	Maintenir les applications dans leur état inital : kebernetes redemarre les machines si ça plante etc. sans intervention humaine    
•	Scale : cloon les applications 
#### Cluster : ensemble de machine dans laquel Kubernetes est installé 
#### Minikube : est un des outils pour exécuter Kubernetes localement. 
## Docker installation
[Installation de Docker sur Windows](https://docs.docker.com/desktop/install/windows-install/)
## Kerbernetes Minikube installation 
[Documentation Minikube](https://minikube.sigs.k8s.io/docs/start/)
## Télécharger le projet My Service
Une fois le projet téléchargé allez vers le répertoire `cd kubernetes-minikube/myservice`

### Tester le projet My Service en utilisant Docker 
Exécuter le projet java : `.\gradlew build` under Windows   
Construction de l'image docker : `docker build -t myservice . `  
vérification de la création : `docker images`   
Démarrer le container : `docker run -p 4000:8080 -t myservice`   
Vérifier l'ID du container : `docker ps `  
arrêter un container : `docker stop containerID `  
### Publier l'image dans docker hub    
Tag the docker image: `docker tag imageID yourDockerHubName/imageName:version      
Example: docker tag 1dsd512s0d myDockerID/myservice:1  `    
se connecter sur docker :` docker login `

pousser l'image vers docker hub : `docker push daviamoujabber/myservice:1`

Example: docker push myDockerID/myservice:1

### Créer le deploiement de kubernetes de docker image 
ne pas oublier d'allumer son minikube si ça n'est pas déjà fait : `minikube start `   
`kubectl create deployment myservice --image=daviamoujabber/myservice:1`

pour vérifier la création du déploiement : 
`kubectl get deployments`
`kubectl get pods`
`kubectl logs myservice-6dcb678cbf-hbmgd  ` 

sinon on peut vérifier sur le dashboard : `minikube dashboard` 

### Expose HTTP et HTTPS route en utilisant NodePort 
`kubectl expose deployment myservice --type=NodePort --port=8080`
`minikube service myservice --url`
### Scaling and load balancing (cloon) 
vérification si le myservice tourne toujours : `kubectl get deployments  ` 
`kubectl get pods`   
`kubectl scale --replicas=2 deployment/myservice `  
`kubectl get deployments  ` 
`kubectl get pods  ` 
### Création de service de type LoadBalancer 
`kubectl get deployments`

`kubectl expose deployment myservice --type=LoadBalancer --port=8080`

`minikube service myservice --url `  
Vérifier dans le navigateur en copiant l'url, si ça a fonctionné 
### Création de son déploiement et service en utilisant les fichiers yaml 
modifier les 3 fichier yaml dans la partie selector avec le nom de notre service ensuite exécuter ses fichiers :   ` kubectl apply -f myservice-deployment.yml`
`kubectl apply -f myservice-service.yml  ` 
`kubectl apply -f myservice-loadbalancing-service.yml`

Vérifier dans le navigateur en copiant l'url, si ça a fonctionné 

