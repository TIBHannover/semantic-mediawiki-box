# MediaWiki in einer Box

Dieses Projekt bietet die Möglichkeit, ein MediaWiki mit minimalem Aufwand in einer virtuellen Maschine aufzusetzen. Voraussetzung ist die Installation von
[Git](https://git-scm.com/downloads),  [Vagrant](https://www.vagrantup.com/downloads.html) und [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

## Installation

Die folgenden Schritte im Terminal (Linux/macOS) oder in der GitBash (Windows) ausführen.
```
git clone https://git.tib.eu/boxes/semantic-mediawiki-box.git
cd semantic-mediawiki-box
vagrant up
```
Wenn die Installation durchgelaufen ist (ca. 5min) kann das Semantic MediaWiki im Browser aufgerufen werden mit
```
http://192.168.98.101/
```
Die Anmeldung an der VM via SSH erfolgt in diesem Beispiel noch vereinfacht und ohne Passwort mit dem Benutzer "vagrant". Der Benutzer hat das sudo-Recht.
```
vagrant ssh
```

## Infrastructure as Code

Dieses Projekt bietet auch gleichzeitig einen guten Einstieg in „Infrastructure as Code“ mit Vagrant und Ansible:

1.	Aufbau der VM -> Vagrantfile
2.	Installation der Komponenten in der VM -> ansible/system.yml
  *	Jeder Punkt in ansible/system.yml ruft in dem entsprechenden Verzeichnis unter ansible/punkt/tasks/main.yml auf
	* Im einfachsten Fall wird mit git nur ein Paket aus dem Standard-Repo von Linux installiert
	* Bei mariadb wird zusätzlich noch eine Datei kopiert und ein Service gestartet
  * Mit mediawiki kommen noch Variablen, Templates  und Includes dazu
  * In php werden ferner noch Dateien manipuliert mit „lineinfile“ und über handler Aktionen ausgeführt
