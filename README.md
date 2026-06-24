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

`NETWORK ID     NAME                    DRIVER    SCOPE
335d65726c92   bridge                  bridge    local
bd74d6bbf446   host                    host      local
0136323de94e   none                    null      local
9ef1f4c1673b   zabbix-docker_default   bridge    local`

## Monitoring du serveur 
Comme pour un client, le serveur a besoin de Zabbix-agent pour être surveillé. Pour le configurer il faut modifier le fichier `zabbix_agentd.conf` comme ceci : 

`Server=100.75.102.123 
ServerActive=100.75.102.123 
Hostname=MaxPi`




