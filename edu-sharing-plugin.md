# Verknüpfung mit edu-sharing (optionales Plugin)

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
