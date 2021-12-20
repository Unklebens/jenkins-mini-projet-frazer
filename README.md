**CE README A ETE REDIGE DE MANIERE CHRONOLOGIQUE IL Y A DES ERREURS QUI SONT CORRIGES PAR LA SUITE**





# installation de jenkins

## Userdata?

pour une raison que je ne maitrise pas encore mettre le script cité plus bas en Userdata de la EC2 AWS ne fonctionne pas en tant qu'ubuntu, peut etre qu'il y a des variables d'environnement qui sont données a root que ubuntu ne peut pas utiliser 
j'ai la flemme de chercher je fais ce qui marche *basta*

(**Lors de la cration de l'environnement de staging je me suis rendu compte que je n'avais pas mis de shebang ce qui a surement causé le probleme**)

un expert c'est qqun qui s'est beaucoup trompé donc là j'ai monté mon niveau d'expertise héhé ^^


## Installation semi-manuelle

vi install.sh avec les valeurs ci dessous

```
sudo apt update
sudo apt install default-jre -y
sudo apt install default-jdk -y
sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update -y
sudo apt install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

```
chmod +x install.sh

sudo ./install.sh
```

le mot de passe d'installation de jenkins est affiché en dernière ligne du script

Comme dirais Apache "It WORKS!"


# Configuration Jenkins

installation des plugin par defaut

creation du user

IP : http://3.80.182.159:8080/

ajout du plugin slack notifications

# Config Slack

connexion au slack sur le navigateur

autorisation MFA (ce que je possède)

creation du canal #mini-projet

installation de Jenkins sur slack 

choix du canal

Configuration du plugin slack notifications sur jenkins (canal & token) en suivant Procédure https://ajc-devopsdiscussion.slack.com/services/B02RCB4L3KP?added=1

ajout du secret

![Screenshot](https://github.com/Unklebens/jenkins-mini-projet-frazer/blob/main/asset/token.PNG)

Test de connexion

![Screenshot](https://github.com/Unklebens/jenkins-mini-projet-frazer/blob/main/asset/testslackok.PNG)


# Creation environnements 

## Userdata staging

```
#!/bin/bash
sudo apt-get update -y
sudo apt-get install git -y
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
usermod -aG docker ubuntu
```

IP Staging : 3.92.214.193

## environnement PROD = EC2 jenkins

commit des nouveau host sur le Github

# creation du pipeline

new item

nom : mini-projet

Projet URL : https://github.com/Unklebens/static-website-example.git

cocher Github hook

Pipeline from SCM

Save

## build 1
Docker not found

installation de docker en cli **A ajouter au script d'installation ou au userdata avec le shebang**

```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo usermod -aG docker ubuntu
```


## Build 2
Permission denied

reboot de la EC2 pour reevaluer les droits aux groupes


## Build 3

oubli du token DH dans les credentials (je veux aller trop vite )

ajout token dockerhub en secret text avec id: dockerhub_password



## Build 4

Oubli de la cle privée (encore trop vite mais p*^ù$n je suis vraiment trop con)

ajout sshusername with private key pair cle privée EC2 avec id ec2_prod_private_key



## Build 5

deploy statging OK


![Screenshot](https://github.com/Unklebens/jenkins-mini-projet-frazer/blob/main/asset/staging.PNG)

Test OK

deploy prod confirmé

![Screenshot](https://github.com/Unklebens/jenkins-mini-projet-frazer/blob/main/asset/prod.PNG)





Notif Slack OK


![Screenshot](https://github.com/Unklebens/jenkins-mini-projet-frazer/blob/main/asset/slack.PNG)


# Nettoyage correction et fix URL images du README.md
