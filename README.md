# ultimate-cicd-pipeline-maven-sonar-argocd-k8

### Install & configure Jenkins on ubuntu22

You can refer below document to install and configure Jenkins, Docker plugin in Jenkins. (upto Docker plugin installation)

```
https://github.com/kohlidevops/Jenkins-Zero-To-Hero
```

### To install Sonarqube scanner in Jenkins

Plugins - Sonar qube scanner - Install without restart

### To install Sonarqube in ubuntu22

```
sudo apt-get install zip unzip -y
sudo su -
adduser sonarqube
su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/3583abac-4e85-4fae-8d00-02097291fb4d)

### To generate a Token in Sonarqube

This need, because Jenkins needs to be authenticate sonarqube server.

Sonarqube - My account - security - Token - Generate new token

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/b5854949-d831-4e37-8ca8-11b527c596a2)

### To add sonarqube token in Jenkins credentials manager

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/465ba499-d448-4f0c-b1e0-ca30a31eaef6)

### Static code analysis stage

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/d7488f4f-3b32-49ed-b40b-79b9c7e31863)

### Docker build and push

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/47f1f955-6d39-4b49-b054-6bc34c1a9786)

### Github credentials

First add a token

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/43ab5519-8011-46ea-9dd1-f0a22c94d97d)

Then add a stage

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/de39630b-0b74-46ad-86c5-036c7a3ea3d8)

### start the build and check the CI status

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/122ce97e-c646-463d-ab72-d255245a1471)

As of now, it has been succeeded with checkout, build, test, code analysis, docker build & push, finally update the build number has been replaced with Tag "replaceImageTag" in deployment yaml file of manifests repo.

### Start the CD process

To setup the ArgoCD with minikube (kubernetes) pls use the following link.

```
https://github.com/kohlidevops/GitOps-ArgoCD
```

Yup! It has been installed successfully and i can see my argocd portal

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/bf1d410b-07c3-47aa-9f70-2902cc0e57df)

To create a new application in ArgoCD

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/fe005fb3-acfb-4227-9fac-aa1d0ee91b97)

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/0a3fe503-b82d-4e9f-81f8-0b4d1ced298c)

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/0e0491f3-af62-4e9e-8061-d9074e8c3c29)

##### Namespace would be "default". Its missing here!

Then create a application. My application has been deployed successfully.

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/090500fb-7ff8-4e0e-80e5-1eee9881f63c)

### To check kubectl deploy and pods

```
kubectl get deploy
kubectl get pods
```

Pods are running! 

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/09402571-a5dd-4cba-ae13-74f5ec2d0174)

### To check the svc

If im checking with svc then it showing NodePort

```
kubectl get svc
```

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/cb854fcb-73c1-468a-9060-e5c5238b34d1)

### To change the NodePort to ClusterIP

To change ClusterIP for this app using below commands

```
kubectl get svc
kubectl get service spring-boot-app-service -o yaml > service.yaml
sudo nano service.yaml

change
NodePort to ClusterIP in "type"
save and exit

kubectl apply -f service.yaml
kubectl get svc
```

Now its listening with ClusterIP

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/8325dd7a-5264-48ec-b756-7135688f4b64)

Bydefault, this app is running with port:80.

So we can port-forward with some other port to avoid conflicting. Just use below command to access the app with port:9090

```
nohup kubectl port-forward svc/spring-boot-app-service 9090:80 --address='0.0.0.0' &
```

Now I can able to access the app through browser

![image](https://github.com/kohlidevops/ultimate-cicd-pipeline-maven-sonar-argocd-k8/assets/100069489/d76773f5-2b12-44c4-aa8c-e8012077b229)

Then I did some changes in startApplication.java and update this tag "replaceImageTag" in manifests deployment.yml file. Now start the build and check the status.


