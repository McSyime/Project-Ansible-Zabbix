# Projekt-Ansible-Zabbix
Automatisierte Einrichtung einer überwachten SSH-Infrastruktur mit Ansible und Docker
<img width="1672" height="941" alt="Netwerk Schema" src="https://github.com/user-attachments/assets/5fb7b233-f41f-484b-80b7-b49549c63eb8" />

## Ausgangslage

Da wir viele VMs erstellen und ich zuhause langsam ein eigenes HomeLab aufbaue, möchte ich die Überwachung dieser Maschinen mit Zabbix-Agents automatisieren. Der Zabbix-Server befindet sich auf einem separaten Server.

Die Automatisierung konzentriert sich also auf die Installation der nötigen Software, um die Maschinen in meinem Netzwerk zu überwachen – egal ob virtuell oder physisch. Der Server selbst wird ebenfalls in das Monitoring eingebunden.

# Serverkonfiguration
## Zu installieren:
- Tailscale
- Docker
- Zabbix-Server
- Zabbix-Agent
- Ansible

## Tailscale-Konfiguration

Den Dienst starten und sich mit den eigenen Zugangsdaten verbinden. Um konsistent zu bleiben, werden für alle Konfigurationen nur die Tailscale-IP-Adressen verwendet.

## Docker-Compose-Konfiguration

Im entsprechenden Verzeichnis eine Datei `docker-compose.yml` gemäss dem Repo erstellen. Falls nötig die Passwörter anpassen. Danach die Container mit `sudo docker compose up -d` starten und anschliessend prüfen, ob alles korrekt läuft.

Danach kontrollieren, ob die Container miteinander kommunizieren können, mit dem Befehl `sudo docker network ls`. Das erwartete Ergebnis sollte ungefähr so aussehen:

<img width="954" height="170" alt="image" src="https://github.com/user-attachments/assets/7477cbca-76e4-4c09-bb24-f3e45e6a8dea" />

## Monitoring des Servers
Wie bei einem Client braucht auch der Server den Zabbix-Agenten, um überwacht zu werden. Dafür muss die Datei `zabbix_agentd.conf` wie folgt angepasst werden:

```ini
Server=IP_TAILSCALE_SERVER_ZABBIX
ServerActive=IP_TAILSCALE_SERVER_ZABBIX
Hostname=HOSTNAME_SERVER
```

# Konfiguration der Clients
Auf dem Ansible-Server sind die Installation von Tailscale und der öffentliche SSH-Key des Ansible- und Zabbix-Servers bereits hinterlegt. Es reicht also, `tailscale up` auszuführen und die Maschinen zu unserem Netzwerk hinzuzufügen.

## Inventar anpassen
Auf dem Ansible-Server die Datei `inventory.ini` so anpassen wie im Repo beschrieben, also mit den Gruppennamen, den IP-Adressen und den passenden Hostnamen. Die IP-Adressen müssen jene des Tailscale-Netzwerks sein.

Sobald die Datei angepasst ist, mit `ansible all -i inventory.ini -m ping` testen. Das erwartete Ergebnis sieht ungefähr so aus:

```
[WARNING]: Host 'vm1' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. S[...]
vm1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Host 'vm3' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. S[...]
vm3 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Host 'vm2' is using the discovered Python interpreter at '/usr/bin/python3.11', but future installation of another Python interpreter could cause a different interpreter to be discovered. S[...]
vm2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
```

# Automatisierungen
## Automatisierung der Abhängigkeiten
Die Grundkonfiguration ist abgeschlossen, und wir sind nun beim Kern des Projekts angekommen. Als erste Automatisierung schlage ich vor, die Installation von `zabbix-agent` auf den Clients zu automatisieren und die Datei `za[...]

## Automatisierung der Hosts in der Zabbix-WebGUI
Um das Hinzufügen von Clients in der Zabbix-Oberfläche zu automatisieren, benötigen wir für unser Playbook ein Token. Dazu im WebGUI unter Users > API Token ein Token generieren und dieses anschliessend verwenden.

Danach das Playbook aus dem Repo `add_hosts_to_zabbix.yml` nehmen und das Token sowie die Gruppennamen gemäss der Datei `inventory.ini` eintragen.

Sobald das Playbook mit `ansible-playbook -i inventory.ini add_hosts_to_zabbix.yml` ausgeführt wurde, erscheinen die Clients in der Data Collection der Zabbix-WebGUI automatisch mit den richtigen Attributen.

## Automatisierung des Dashboards
Um die VMs in ein vordefiniertes Dashboard zu integrieren, das Playbook `create_dashboard_vms.yml` mit `ansible-playbook -i inventory.ini create_dashboard_vms.yml` ausführen. Falls wir etwas ändern[...]






