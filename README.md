# Vérification des caractéristiques systèmes d'un environnement 3 tier linux

## Description

Ce projet Ansible permet de vérifier les caractéristiques de plusieurs machines Linux de type RedHat par rapport à un cahier des charges.  
Les vérifications incluent :

- Le nombre de vCPUs
- La capacité mémoire
- La présence de plusieurs points de montage avec leur capacité de disque
- Les ouvertures réseaux sur des ports spécifiques

Le playbook Ansible effectue également l'installation du paquet `vim` sur toutes les machines et s'assure que `nc` (netcat) est installé pour les vérifications des ports.

## Structure du Projet

- `inventory.yml` : fichier d'inventaire listant les machines cibles, organisé par groupes (serveurs web, applicatifs, bases de données).
- `vars/common.yml` : variables communes à toutes les machines.
- `vars/web.yml` : variables spécifiques aux serveurs web.
- `vars/app.yml` : variables spécifiques aux serveurs applicatifs.
- `vars/db.yml` : variables spécifiques aux serveurs de bases de données.
- `check_system.yml` : playbook Ansible principal qui effectue les vérifications et les installations.

## Utilisation

### Prérequis

- Ansible installé sur votre machine de contrôle.
- Accès SSH aux machines cibles avec les clés appropriées.

### Étapes

1. **Cloner le projet** :
   ```bash
   git clone https://github.com/yatoub/check3TierEnv.git
   cd check3TierEnv
   ```
2. **Configurer l'inventaire** :  
Modifier le fichier `inventory.yml` pour y ajouter vos machines cibles et ajuster les adresses IP et les noms d'hôte.

3. **Configurer les variables** :  
   Adapter les fichiers de variables (`vars/common.yml`, `vars/web.yml`, `vars/app.yml`, `vars/db.yml`) selon vos besoins spécifiques.
4. **Exécuter le playbook** :  
   Lancer la commande suivante pour exécuter le playbook et vérifier les caractéristiques des machines :
   ```bash
   ansible-playbook -i inventory.yml check_system.yml
   ```
### Exemple de configuration d'inventaire

```yaml
all:
  vars:
    ansible_user: votre_utilisateur
    ansible_ssh_private_key_file: chemin_vers_votre_clé_privée

  children:
    web_servers:
      hosts:
        server1:
          ansible_host: 192.168.1.1
    app_servers:
      hosts:
        server2:
          ansible_host: 192.168.1.2
    db_servers:
      hosts:
        server3:
          ansible_host: 192.168.1.3
```
### Exemple de variables pour un serveur web

```yaml
required_vcpus: 2
required_memory_gb: 4
required_mount_points:
  - path: /var/www
    required_size_gb: 50
required_open_ports:
  - port: 80
  - port: 443
  - port: 22
```

### Résultats  
À la fin de l'exécution du playbook, Ansible générera un rapport indiquant si les machines vérifiées répondent aux critères spécifiés dans les fichiers de variables. Toute non-conformité sera clairement signalée.

### Licence  
Ce projet est sous licence [GNU General Public License v3.0.](LICENCE) Vous êtes libre de l'utiliser, de le modifier et de le distribuer selon les termes de cette licence.

Pour toute question ou suggestion, n'hésitez pas à ouvrir une issue ou à contacter le mainteneur du projet.  

Merci d'utiliser ce projet !  