# thema-config

Ziele:
- Konfiguration eines Themas ist an _einer_ Stelle gespeichert.
- Transparenz schaffen: Besseres Verständnis eines Themas (technische Aspekte).
- Themen können jederzeit und einzeln (ohne Einwirkungen auf andere Themen) publiziert werden.
- Lokale Entwicklungsumgebungen sind überall einfach zu verwenden. Testumgebungen sind beliebig horizontal (in der Menge) skalierbar und öffentlich verfügbar.
- Thema-Config /-Repo soll validiert werden können: Ist alles drin, was benötigt wird? etc.
- Teilkonfigurationen sind problemlos möglich, d.h. ich will nur XTF aus Edit-DB publizieren. Oder die XTF der ÖREB-Rahmenmodells im Datenbezug zur Verfügung stellen (Annahme: es gibt keinen Publisher-Job oder so).


Beispiele (?):
- HBA-Sachen, da aktuell und es noch offene Punkte (aka Bugs) gibt, die man im Code-Repo verwalten sollte (Issues).
- Seltene Baumarten, da explizites Edit-Modell und "Json-Rendering". Und in Pubmodell Vererberbung.
- Abbaustellen, da ganz simpel.

TODO:
- INTERLIS-Modelle: Wie komme ich zum Repo? (Themen-Repo abgrasen? Im Prinzip gleich. Eventuell gibts ja ein Repo-Repo-Configfile, wo drin steht, welches Amt etc.)
- Modell/Struktur wie das Repo aussehn muss/darf. Dann kann mit Pipeline geprüft werden, ob i.O.


## cli / dev env

- Was ist mit Jenkins?


```
soctrl
```


```
soctrl> dev [start|stop]
```


```
soctrl> gretl path/to/job-directory
```


```
soctrl> topics ls
```

???
```
soctrl> topics awjf_seltene_baumarten ls
```
