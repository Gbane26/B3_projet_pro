# Projet Ansible Mastodon 

Membre du groupe : 
- Ahamadou Yaya GBANE
- Soumaila COULIBALY
- Serigne Saliou SECK
- El Schua KHAHITOM


## Usage

```
ansible-playbook main.yml
```
Comment l'utiliser : 

- Etape 1 : Mettre en place un environmment (machines) avec    acces ssh sans mot de pass
                

- Etape 2 : déploiement "principal" : 
                  ansible-playbook main.yml

## Notes

Dans ce projet vous avez plusieurs roles

- update : Pour les mise à jour  
- ssh : Pour la configuration de POSTGRESQL
- selinux : YARN et NODEJS 
- parefeu : Pour Installation de Ruby
- permission
- event : Pour Configuration de nginx



## Difficultés rencontrés 

- Lors de la création d'un utilisateur postgres dans le role postgresql ; Utilisateur non privilégié

- Ruby : non Idempotents

- connexion refusée lors des telechargement de fichier en ligne 

```
#Exemple 

- name: Ajouter clé Yarnpkg
  get_url:
    url: https://dl.yarnpkg.com/debian/pubkey.gpg
    dest: /etc/apt/trusted.gpg.d/yarnpkg.asc
  when: ansible_os_family == "Debian"

```
Tant tot get_url passe tant tot il refuse alors que rien a été modifié a son niveau 


## Fichier inventory 

Pour l'inventory on a decidé de le faire sur une seule machine;

```
alma-infra1 ansible_host=XX.XX.XX.12
```

# Sécurisation des Serveurs AlmaLinux avec Ansible

## Introduction

Ce playbook Ansible est conçu pour sécuriser des serveurs AlmaLinux. Il exécute une série de tâches visant à renforcer la sécurité, configurer les services essentiels, et mettre en place des politiques de sécurité strictes. Chaque tâche est incluse à partir de fichiers séparés, permettant ainsi une meilleure modularité et un entretien facilité.

## Architecture du Playbook

### Structure Modulaire

- Chaque composant de la sécurisation est séparé dans des fichiers de tâches spécifiques (`.yml`), permettant une organisation claire et la réutilisation de tâches similaires dans d'autres playbooks.
- Le playbook principal inclut chaque fichier de tâches via le module `include_tasks`.

### Organisation des Tâches

- Les tâches sont organisées selon des fonctions de sécurité spécifiques : mise à jour des paquets, désactivation des services inutiles, configuration du pare-feu, politique de mots de passe, et bien d’autres.
- Cela permet un découpage logique des différentes étapes de sécurisation, facilitant ainsi la maintenance et l'extension.

### Choix Techniques

- **Ansible** : Utilisation d’Ansible pour l’automatisation des tâches de sécurisation, permettant un déploiement cohérent sur plusieurs serveurs.
- **AlmaLinux** : Le playbook cible AlmaLinux, un fork communautaire de Red Hat Enterprise Linux (RHEL), ce qui assure une grande compatibilité avec des environnements de production.
- **Firewalld** : Choisi pour la gestion du pare-feu en raison de sa flexibilité et de son intégration native avec AlmaLinux.
- **SELinux** : Renforcement de la sécurité du système grâce à l’activation de SELinux, une couche de sécurité supplémentaire essentielle dans les environnements Linux.
- **AIDE** : L’installation et la configuration d’AIDE permettent une détection avancée des intrusions en surveillant les modifications de fichiers.

### Sécurité et Conformité

- Le playbook met en œuvre des pratiques de sécurité standard telles que la configuration stricte de SSH, la gestion des permissions des fichiers, et la désactivation des comptes utilisateurs inactifs.
- La configuration de cron jobs assure que les contrôles de sécurité sont effectués régulièrement.

### Extensibilité

- Ce playbook est conçu pour être extensible, permettant d'ajouter de nouvelles tâches de sécurité ou de personnaliser les configurations existantes en fonction des besoins spécifiques de l’organisation.

## Schéma d'Architecture

```plaintext
+------------------------+       +------------------------+
|    Playbook Principal   |       |     Inventaire Ansible  |
|                        |<----->|  Groupe 'alma9'         |
| - include_tasks        |       |  Liste des serveurs     |
|                        |       +------------------------+
+------------+-----------+
             |
             v
+-------------------------+
|  Fichiers de Tâches      |
|  - checkconnexion.yml    |
|  - update.yml            |
|  - services.yml          |
|  - parefeu.yml           |
|  - password.yml          |
|  - disable.yml           |
|  - ssh.yml               |
|  - selinux.yml           |
|  - event.yml             |
|  - permission.yml        |
|  - cron.yml              |
|  - security_system.yml   |
|  - aide.yml              |
+-------------------------+
