# Project-Ansible-Zabbix
Automatisierte Einrichtung einer überwachten SSH-Infrastruktur mit Ansible und Docker

## Situation de départ 

Comme nous créons beaucoup de VMs et que je commence à monter un HomeLab chez moi, je souhaite automatiser le contrôle de ces dernières avec des agents Zabbix. Le serveur Zabbix se trouve sur un Raspberry Pi 5 et dispose d'un webGUI et d'une DB MariaDB pour stocker les informations des tasks par exemple. 

L'automatisation se concentre donc sur l'installation des Software nécessaires pour les contrôle des machines (virtuelles ou non) de mon réseau. Concernant le server, il sera installé dans un conteneur Docker Compose car il nécessite d'avoir une DB, le server Zabbix et Zabbix webGUI pour la configuration de la surveillance des machines. 

# Configuration du serveur 
## A installer : 
- Tailscale
- Docker
- Zabbix-server
- Zabbix agent
- Ansible

##Configuration tailscale 

Lancer le service puis se connecter avec ses identifiants. Afin de rester cohérent, seuls les adresses IP Tailscale seront utilisées pour les différentes configs.

## Configuration Docker Compose 

Dans le dossier, créer un fichier `docker-compose.yml` comme dans le Repo. Modifier les mots de passe si nécessaire. Ensuite, démarrer les conteneurs avec `sudo docker compose up -d` puis contrôler le statut avec `sudo docker ps`.

Ensuite, il faut contrôler que les conteneurs puissent communiquer entre eux avec cette commande 'sudo docker network ls'. Le résultat attendu doit ressembler à ça : 

<img width="954" height="170" alt="image" src="https://github.com/user-attachments/assets/7477cbca-76e4-4c09-bb24-f3e45e6a8dea" />

## Monitoring du serveur 
Comme pour un client, le serveur a besoin de Zabbix-agent pour être surveillé. Pour le configurer il faut modifier le fichier `zabbix_agentd.conf` comme ceci : 

```ini
Server=100.75.102.123 
ServerActive=100.75.102.123 
Hostname=MaxPi
"""




