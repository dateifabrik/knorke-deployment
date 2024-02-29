## Voraussetzungen:

- Webserver (Apache oder Nginx) inkl. notwendiger Module
- MySQL (MariaDB) inkl. User mit _Grant Option_ und _All Privileges_
- [adminer](https://www.adminer.org) eingerichtet
- Datenbank angelegt
- Domain angelegt mit https (inkl. SSL-Zertifikat)
- [devenv](https://devenv.sh/) (und optional, aber empfohlen [direnv](https://direnv.net/))
- node und ein Node-Version-Manager (nvm, n)
- PHP mit benötigten Extensions
- git
- ssh-Zugang zum Repository (ggf. ssh-Key erzeugen und hinterlegen(lassen))  

Das sind Grundlagen, die jeder beherrschen sollte.

## Ausgangslage: neues Projekt

### Einrichtung des Projekts:

DB-Dump von live-DB erzeugen und importieren

    
    mysqldump OPTIONEN -h'HOST' -u'USER' -p DB
    // **diese Optionen setzen!**
    --skip-lock-tables
    --insert-ignore
    --hex-blob
    --no-tablespaces
    --single-transaction
    --quick
    --opt

◦ wahlweise steht shopware-cli (zum Beispiel auf MaxCluster) zur Verfügung
◦ shopware-cli project dump DBNAME --username DBUSER --password PASSWORD
◦ diese Optionen setzen!
    ▪ --clean
    ▪ --anonymize
    ▪ --skip-lock-tables
    ▪ 
          
    • Lokaler Import des Datenbank-Dumps (via Terminal)

        ◦ ggf. definer aus sql-Datei entfernen, z.B. mit sed
        ◦ importierender USER muss über entsprechende Rechte verfügen (siehe oben Voraussetzungen lokal)
        ◦ sql-Dateien:
            ▪ mysql -h'HOST' -u'USER' -p DB
        ◦ sql.gz-Dateien:
            ▪ gunzip < PATH/TO/DB_BACKUP_NAME .sql.gz | mysql -h'HOST' -u'USER' -p DB

    • Repository klonen
    • in geklontes Verzeichnis wechseln (Verzeichnis = lokale Domain mit https)
    • möglichst keine .env verwenden, sondern .env.local
    • .env bzw. .env.local an lokale Gegebenheiten anpassen, ggf. Standard .env zu Hilfe nehmen: https://dateifabrik.de/docs/shopware6/#standard-env
    • darauf achten, daß der Part SHOPWARE_CDN_STRATEGY_DEFAULT=id auskommentiert enthalten ist, da sonst keine Bilder angezeigt werden (falls nach lokal kopiert)
    • .gitignore prüfen, diese Ordner/Dateien dürfen in der Regel nicht in‘s Repo:
        ◦ .env.local
        ◦ .env.local.php
        ◦ .env.*.local
        ◦ custom/*
        ◦ alles in config/jwt (ausser .gitignore)
    • auth.json von live kopieren
    • config/jwt nach lokal kopieren


---

TERRA AUSTRALIA
	



Ausgangslage: Projekt bereits vorhanden

Sonderfälle Varnish/Redis...