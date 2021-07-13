Jitsi Meet Docker
=========

Ce rôle sert à déployer/remettre en état un serveur Jitsi Meet via le projet Docker Compose officiel fournit par l'équipe de développement. 
En résumé, on automatise la procédure dite «Quick Install» décrite ici : https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker

Requirements
------------

### Matériel
* 1 vcore + 1 Go de RAM + BEAUCOUP de bande passante
* Voir cet article sur la performance : https://jitsi.org/jitsi-videobridge-performance-evaluation/
* Voir ce videotutoriel pour le passage à l'échelle : https://jitsi.org/blog/new-tutorial-video-scaling-jitsi-meet-in-the-cloud/

### Système d'exploitation
* Ubuntu 20.04 (seul SE testé)

### Docker et Docker Compose
* IMPORTANT : Docker et Docker Compose NE SONT PAS installés par le rôle
* NOTE : Les paquets .deb disponibles dans Ubuntu 20.04 (universe) sont suffisants

Role Variables
--------------

### defaults/main.yml

Les valeurs par défaut dans defaults/main.yml présupposent le déploiment d'une instance publique directement accessible à tous via Internet. Si c'est ce que vous voulez, vous n'avez en principe qu'à inscrire un vrai nom de domaine dans les variables 'url_publique' et 'letsencrypt_domaine' et une vraie adresse de courriel dans 'letsencrypt_courriel'.

Si ce n'est pas exactement ce que vous voulez (votre instance sera derrière un serveur NAT, derrière un proxy, devra permettre l'authentification, etc.), vous devrez après le déploiement ajuster les variables du fichier .env du projet Docker Compose sur votre serveur. Ces variables sont très bien documentées dans le fichier .env lui-même et aussi dans le guide officiel : https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker

Dependencies
------------

Aucune dépendance à d'autres rôles Ansible.

Example Playbook
----------------

```
- name: "Deployer un serveur Jitsi Meet via Docker Compose"
  hosts: 
    - all
  vars:
    # Supplanter ici les variables du role 'ansible-docker-jitsi-role' au besoin
    utilisateur_unix: "jitsi442"
    fuseau_horaire: "UTC-5"
    url_publique: "https://jitsi.unvraidomaine.org"
    letsencrypt_domaine: "jitsi.unvraidomaine.org"
    letsencrypt_courriel: "un_nom@jitsi.unvraidomaine.org"

  pre_tasks:
    - name: "Installer Docker et Docker Compose"
      apt:
        name: docker.io, docker-compose
        state: present
      become: yes
  roles:
    - ansible-docker-jitsi-role
```

License
-------

GPL-3.0-only

Author Information
------------------

Mathieu GP, admin. sys. pour Coderbunker
