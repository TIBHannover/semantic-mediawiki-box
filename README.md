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
http://192.168.98.121/
```
Die Anmeldung an der VM via SSH erfolgt in diesem Beispiel noch vereinfacht und ohne Passwort mit dem Benutzer "vagrant". Der Benutzer hat das sudo-Recht.
```
vagrant ssh
```

## Verknüpfung mit edu-sharing

* Der edu-sharing-plugin Task wird ausgeführt, wenn im Vagrantfile der Eintrag "ansible.skip_tags = [ "edu-sharing-plugin" ]" entfernt/einkommentiert ist.
* Wurde bei der Installation der edu-sharing-plugin Task ausgeführt, dann wurde das edu-sharing-plugin bereits ins Mediawiki kopiert und muss nun noch manuell beim edu-sharing registriert werden.
* Mediawiki-installation
    * editieren der {{ mediawiki_path }}/extensions/edu-sharing/conf/homeApplication.properties.xml
        * host - Mediawiki-host
        * inline_helper - Url zur Mediawiki-Erweiterung
        * alle edu-sharing-URL's anpassen
        * public key des edu-sharing-repos anpassen
            * Der Key ist zu finden wenn man sich im edu-sharing als Admin einloggt unter 'Admin-Tools' > 'Applications' > 'homeApplication.properties.xml bearbeiten' oder direkt auf dem edu-sharing-System in dieser Datei
* Edu-Sharing-Installation
    * app-wiki.properties.xml lokal erstellen und MEDIAWIKI_PUBLIC_KEY, MEDIAWIKI_DOMAIN, MEDIAWIKI_HOST eintragen
    ```
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
        <properties>
        <entry key="public_key">-----BEGIN PUBLIC KEY-----
        MEDIAWIKI_PUBLIC_KEY
        -----END PUBLIC KEY-----
        </entry>
        <entry key="appcaption">mediawiki</entry>
        <entry key="appid">wiki</entry>
        <entry key="domain">MEDIAWIKI_DOMAIN</entry>
        <entry key="host">MEDIAWIKI_HOST</entry>
        <entry key="trustedclient">true</entry>
        <entry key="type">LMS</entry>
        </properties>
    ```
        * Der MEDIAWIKI_PUBLIC_KEY ist zu finden in der der o.g. {{ mediawiki_path }}/extensions/edu-sharing/conf/homeApplication.properties.xml
    * Einloggen in edu-sharing als Administrator
    * Admin-Tools > Applications
        * Mediawiki anbinden indem die gerade erstellte xml-Datei hochgeladen wird
* Ggf muss der Browser-Cache neu geladen werden damit der edu-sharing-Button in der Toolbar erscheint (Firefox zB via `<Strg>+<F5>`)


## Infrastructure as Code

Dieses Projekt bietet auch gleichzeitig einen guten Einstieg in „Infrastructure as Code“ mit Vagrant und Ansible:

1.	Aufbau der VM -> Vagrantfile
2.	Installation der Komponenten in der VM -> ansible/system.yml
  *	Jeder Punkt in ansible/system.yml ruft in dem entsprechenden Verzeichnis unter ansible/punkt/tasks/main.yml auf
	* Im einfachsten Fall wird mit git nur ein Paket aus dem Standard-Repo von Linux installiert
	* Bei mariadb wird zusätzlich noch eine Datei kopiert und ein Service gestartet
  * Mit mediawiki kommen noch Variablen, Templates  und Includes dazu
  * In php werden ferner noch Dateien manipuliert mit „lineinfile“ und über handler Aktionen ausgeführt
