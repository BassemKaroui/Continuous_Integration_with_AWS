# AJC : Projet fondamental

## Description du projet

Mise en place d’une chaîne d’intégration continue avec AWS.
- Première phase :
Installation automatisé d’un environnment de Labs via Ansible.
  * Une VM Gitlab.
  * Une VM serveur Web (apache2).
  * Une VM en local type Linux. (virtualBox,WSL,GitBash,SSH).
  
- Seconde phase :
Mise en place via GitLab du CI (continious integration) pour la démonstration nous cherchons a deployer
de façon automatiser l’intégration de pages Web statiques. Le framework utilisé sera JEKYLL. (à decouvrire) Faire un test de jekyll avec un exemple de site.(il existe de nombreux exemples).
Nous aimerions un déclenchement de l’integration des pages web (Jekyll) sur la machine virtuelle serveur
web AWS.

## Description du repo


## Déroulement du projet

### Etape 1
La VM locale est déjà créée avec une distro Debian. Il faut donc installer Ansible :
 - `echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> /etc/apt/sources.list` (déjà  en root donc pas besoin du `tee -a`)
 - `apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367`
 - `apt update`
 - `apt install ansible`

### Etape 2
Récupérer *access_key_id* et *secret_access_key* depuis AWS console pour pouvoir créer dans VMs sur AWS en utilisant Ansible :
 - Sur la console AWS : **IAM &rarr; Users &rarr; Add user &rarr; Access type : Programmatic access &rarr; next &rarr; Attach existing policies directly &rarr; Filter: Policy type: AmazonEC2FullAccess &rarr; Create user &rarr; Download .csv**
 - Installer *boto* puis configurer le fichier *~/.boto* avec les données d'accès : 
    * `yes | apt install python-pip` 
    * `pip install boto`
    * Ajouter *access_key_id* et *secret_access_key* dans le fichier *~/.boto*
  * OU BIEN : utiliser des variables d'environnement à la place de *boto* :
    * `export AWS_ACCESS_KEY_ID=...` 
    * `export AWS_SECRET_ACCESS_KEY=...`

### Etape 3
Pour la création des deux VMs AWS on a besoin d'une clé privée pour pouvoir établir une connexion SSH avec les deux VMs par la suite. Je vais donc utiliser la clé que j'ai déjà créée pour d'autres VMs. Pour ce faire, il faut juste mettre la clé nommée *ec2_bassem.pem* dans un dossier *keys* à côté du fichier [VM_local_Debian/playbook.yml](./VM_local_Debian/playbook.yml) (`ansible-playbook playbook.yml`) qu'on va lancer par la suite pour créer les deux VMs AWS : GitLab et Apache2.

### Etape 4
Une fois les deux VMs sont créées il faut modifier le fichier */etc/ansible/hosts* : 
* `echo "[gitlab_server]" >> /etc/ansible/hosts` 
* `echo "IP1 ansible_ssh_user=admin ansible_ssh_private_key_file=/root/projet_fondamental/keys/ec2_bassem.pem" >> /etc/ansible/hosts` . Il faut remplacer *IP1* par l'adresse IP public de la VM qui va être utilisée pour deployer GitLab.
* `echo "[web_server]" >> /etc/ansible/hosts`
* `echo "IP2 ansible_ssh_user=admin ansible_ssh_private_key_file=/root/projet_fondamental/keys/ec2_bassem.pem" >> /etc/ansible/hosts` . Il faut remplacer *IP2* par l'adresse IP public de la VM qui va être utilisée pour deployer Apache2q.
