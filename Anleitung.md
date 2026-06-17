# Vorbereitungen (Grundkonfiguration)

1. Nachdem Sie das Repo kopiert haben, starten Sie die drei VMs mit Vagrant.

2. Installieren Sie auf dem Server:
- Tailscale  
- Ansible
- Docker
- Ansible
- Zabbix Server

3. Auf VMs installieren :
- Tailscale
- Zabbix Agent
  
4. Einstellungen

Falls erforderlich, tauschen Sie die erforderlichen SSH-Schlüssel zwischen dem Server und den zu überwachenden Rechnern aus und testen Sie die Verbindungen.

Starten Sie Tailscale auf allen Maschinen und integrieren Sie diese in das Tailscale-Netzwerk. 

Die Tailscale-IP-Adressen aller zu überwachenden Rechner abrufen und in eine Datei namens „inventory.ini“ eintragen. Zum Beispiel:

  



