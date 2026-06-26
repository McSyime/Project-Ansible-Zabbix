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

## Configuration tailscale 

Lancer le service puis se connecter avec ses identifiants. Afin de rester cohérent, seuls les adresses IP Tailscale seront utilisées pour les différentes configs.

## Configuration Docker Compose 

Dans le dossier, créer un fichier `docker-compose.yml` comme dans le Repo. Modifier les mots de passe si nécessaire. Ensuite, démarrer les conteneurs avec `sudo docker compose up -d` puis contrôler le statut avec `sudo docker ps`.

Ensuite, il faut contrôler que les conteneurs puissent communiquer entre eux avec cette commande `sudo docker network ls`. Le résultat attendu doit ressembler à ça : 

<img width="954" height="170" alt="image" src="https://github.com/user-attachments/assets/7477cbca-76e4-4c09-bb24-f3e45e6a8dea" />

## Monitoring du serveur 
Comme pour un client, le serveur a besoin de Zabbix-agent pour être surveillé. Pour le configurer il faut modifier le fichier `zabbix_agentd.conf` comme ceci : 

```ini
Server=IP_TAILSCALE_SERVER_ZABBIX
ServerActive=IP_TAILSCALE_SERVER_ZABBIX
Hostname=HOSTNAME_SERVER
```

# Configuration des clients 
Dans le VagrantFile, l'installation de tailscale et la clé ssh publique du serveur Ansible et Zabbix est déjà introduite. Il suffit donc de faire `tailscale up` et d'ajouter les machines dans notre réseau tailscale. Ensuite. effectuer une première connexion du serveur aux machines clients pour que le serveur soit reconnu par les clients lors des automatisations avec ansible.

## Modifier l'inventaire 
Sur le serveur Ansible, modifier le fichier ```inventory.ini``` comme dans le repo avec les noms de groupe, les adresses IP et noms de Hosts correspondants. Les adresses IP doivent être celles du réseaux Tailscale. Voir exemple dans le repo. 

Une fois que le fichier est modifié, testé avec `ansible all -i inventory.ini -m ping`. Le résultat attendu ressemble a cela : 
```
[WARNING]: Host 'vm1' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.19/reference_appendices/interpreter_discovery.html for more information.
vm1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Host 'vm3' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.19/reference_appendices/interpreter_discovery.html for more information.
vm3 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Host 'vm2' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.19/reference_appendices/interpreter_discovery.html for more information.
vm2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
```
# Automatisations
## Automatisation des dépendances
La configuration de base est achevée et nous sommes dans le coeur du sujet. Comme automatisation, je propose d'automatiser l'installation de zabbix-agent sur les clients et de modifier le fichier `zabbix_agentd.conf` automatiquement par rapport à l'adresse IP du serveur. Voir le fichier ```setup-vms.yml``` du repo. 

## Automatisation des Hosts dans Zabbix WebGUI
Afin d'automatiser l'ajout de client dans l'interface Zabbix, nous avons besoin de générer un Token pour notre playbook. Il suffit d'aller dans le WebGUI et dans Users, API Token et de le générer pour l'utilisateur admin. 

Ensuite, prendre le Playbook du repo ```add_hosts_to_zabbix.yml``` et insérer le Token et le nom des groupes selon le fichier ```inventory.ini```

Une fois le playbook lancé, les clients se retrouvent dans Data Collection du WebGUI de Zabbix automatiquement avec les bons attributs. 

## Automatisation du Dashboard 
Pour intégrer les vms à une Dashboard prédéfinie, voir le Playbook ```create_dashboard_vms.yml```. A savoir que si nous modifions le Dashboard et que nous exécutons le playbook pour intégrer une autre VM, la Dashboard va se réinitialiser. 





