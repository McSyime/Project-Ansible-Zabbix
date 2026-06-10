# Project-Ansible-Zabbix
Dans le cadre de la leçon Netzwerkbetriebsystem, nous devons mettre en oeuvre une solution d'automatisation avec Ansible. 

## Idee und Absicht 

Das Projekt umfasst die Einrichtung einer Laborinfrastruktur, die die Automatisierung, Überwachung und Absicherung mehrerer Linux-Rechner ermöglicht. Das Hauptziel besteht darin, Ansible als zentrales Tool zur Automatisierung der Installation und Konfiguration der verschiedenen Systemkomponenten einzusetzen.

Die Infrastruktur besteht aus einem Raspberry Pi 5, der als zentraler Server fungiert. Auf diesem Rechner laufen Ansible sowie der Überwachungsserver Zabbix. Der Zabbix-Server wird mithilfe von Docker bereitgestellt, um die Installation übersichtlicher, leichter reproduzierbar und näher an einer realen Bereitstellungsumgebung zu gestalten.

Bei den Client-Rechnern handelt es sich um drei virtuelle Debian-Maschinen. Sie dienen als Testrechner zur Simulation einer kleinen IT-Umgebung. Jede VM verfügt über einen SSH-Zugang, einen Zabbix-Agenten und Testbenutzer, die automatisch mit Ansible erstellt werden.

Die Kommunikation zwischen dem Raspberry Pi und den virtuellen Maschinen erfolgt über Tailscale, ein auf WireGuard basierendes VPN. Jeder Rechner verfügt somit über eine Tailscale-IP-Adresse, die es dem Raspberry Pi ermöglicht, mit den Clients zu kommunizieren, auch wenn sich die Rechner nicht unbedingt im selben lokalen Netzwerk befinden.

Ansible wird zur Automatisierung mehrerer Aufgaben eingesetzt: Installation der erforderlichen Pakete, Konfiguration des SSH-Zugangs, Erstellung der Testbenutzer, Installation des Zabbix-Agenten auf den Debian-Rechnern, Konfiguration des Agenten und Autorisierung


