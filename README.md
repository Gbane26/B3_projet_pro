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

