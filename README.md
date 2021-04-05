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

