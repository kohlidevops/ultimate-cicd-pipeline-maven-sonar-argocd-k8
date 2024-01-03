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
