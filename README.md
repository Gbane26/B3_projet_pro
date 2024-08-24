# Projet Annuel : Durcissement de l'OS AlmaLinux9 via recommendations ANNSSI

Membre du groupe : 
- Ahamadou Yaya GBANE
- Soumaila COULIBALY
- Serigne Saliou SECK
- El Schua KLAHITOM


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

```

# Installation et Test de la Solution de Sécurisation des Serveurs AlmaLinux

## Prérequis

Avant de commencer, assurez-vous que les éléments suivants sont disponibles :

1. **Ansible** doit être installé sur votre machine de contrôle (poste de travail ou serveur de gestion).
   - Pour installer Ansible sur AlmaLinux :
     ```bash
     sudo dnf install ansible -y
     ```

2. **Accès SSH** : Assurez-vous que vous avez un accès SSH fonctionnel aux serveurs AlmaLinux que vous souhaitez sécuriser. Vous devrez configurer vos clés SSH pour un accès sans mot de passe.
   - Pour ajouter une clé SSH à un serveur :
     ```bash
     ssh-copy-id -i ~/.ssh/id_rsa.pub user@XX.XX.XX.11
     ```

3. **Serveurs AlmaLinux** : Identifiez les adresses IP ou les noms d’hôte des serveurs AlmaLinux cibles et regroupez-les sous le groupe `alma9` dans votre inventaire Ansible.

## Étapes d'Installation

### 1. Préparation de l'Environnement

1. **Créer le Répertoire du Projet** :
   - Créez un répertoire pour votre playbook Ansible et naviguez dans ce répertoire :
     ```bash
     mkdir -p ~/B3_projet_pro
     cd ~/B3_projet_pro
     ```

2. **Créer le Fichier d'Inventaire** :
   - Créez un fichier `inventory.ini` dans le répertoire du projet, qui listera les serveurs AlmaLinux à sécuriser :
     ```ini
     [alma9]
     AlmaLinux9-server ansible_host=XX.XX.XX.11
     AlmaLinux9-server ansible_host=XX.XX.XX.12
     ```

### 2. Création du Playbook Ansible

1. **Créer le Playbook Principal** :
   - Créez un fichier `main.yml` contenant les instructions pour sécuriser vos serveurs :
     ```yaml
     ---
     - name: Sécurisation des serveurs AlmaLinux
       hosts: alma9
       gather_facts: no
       become: yes
       tasks:
         - name: Update system packages
           include_tasks: tasks/update.yml
         - name: Disable unnecessary services
           include_tasks: tasks/services.yml
         - name: Configure firewall with firewalld
           include_tasks: tasks/parefeu.yml
         - name: Configure password policy
           include_tasks: tasks/password.yml
         - name: Disable inactive user accounts
           include_tasks: tasks/disable.yml
         - name: Configure SSH settings
           include_tasks: tasks/ssh.yml
         - name: Enable SELinux
           include_tasks: tasks/selinux.yml
         - name: Configure security logging
           include_tasks: tasks/event.yml
         - name: Configure file permissions
           include_tasks: tasks/permission.yml
         - name: Configure cron jobs for security checks
           include_tasks: tasks/cron.yml
         - name: Configure Advanced System Security Settings
           include_tasks: tasks/security_system.yml
         - name: Install and Configure AIDE (Advanced Intrusion Detection Environment)
           include_tasks: tasks/aide.yml
     ```

2. **Créer les Fichiers de Tâches** :
   - Dans le répertoire du projet, créez un sous-répertoire nommé `tasks` :
     ```bash
     mkdir -p tasks
     ```
   - Ajoutez ensuite les fichiers de tâches dans ce répertoire. Par exemple, pour `update.yml`, créez un fichier `tasks/update.yml` :
     ```yaml
   ---
    - name: Ensure all packages are up to date
      ansible.builtin.yum:
        name: '*'
        state: latest
     ```

   - Répétez cette étape pour tous les fichiers de tâches mentionnés dans le playbook principal (`aide.yml`, `services.yml`, etc.).

### 3. Exécution du Playbook

1. **Exécuter le Playbook** :
   - Une fois tous les fichiers configurés, vous pouvez exécuter le playbook pour sécuriser vos serveurs AlmaLinux :
     ```bash
     ansible-playbook -i inventory main.yml
     ```

### 4. Vérification de l'Installation

1. **Vérification des Configurations** :
   - Après l'exécution du playbook, connectez-vous à vos serveurs AlmaLinux pour vérifier que les configurations ont été appliquées correctement.
   - Par exemple, vous pouvez vérifier :
     - Les règles du pare-feu avec `firewall-cmd --list-all`.
     - L’état de SELinux avec `sestatus`.
     - La configuration SSH en regardant le fichier `/etc/ssh/sshd_config`.

### 5. Maintenance et Extensibilité

- Pour ajouter de nouvelles configurations ou ajuster celles existantes, modifiez les fichiers de tâches correspondants dans le répertoire `tasks` puis réexécutez le playbook.
- Pour une mise à jour régulière des serveurs, vous pouvez intégrer ce playbook dans un pipeline d’intégration continue (CI) ou l’exécuter périodiquement via un cron job sur votre machine de contrôle.

## Conclusion

Ce guide vous a montré comment installer et tester un playbook Ansible conçu pour sécuriser des serveurs AlmaLinux. Grâce à sa structure modulaire, ce playbook est facile à maintenir et à étendre, offrant une solution robuste pour la gestion de la sécurité sur vos serveurs.
