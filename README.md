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
- Ansible

## Configuration Docker Compose 






