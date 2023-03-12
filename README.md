# thema-config

## Ziele

- Konfiguration eines Themas ist an _einer_ Stelle gespeichert.
- Transparenz schaffen: Besseres Verständnis eines Themas (technische Aspekte).
- Themen können jederzeit und einzeln (ohne Einwirkungen auf andere Themen) publiziert werden.
- Lokale Entwicklungsumgebungen sind überall einfach zu verwenden. Testumgebungen sind beliebig horizontal (in der Menge) skalierbar und öffentlich verfügbar.
- Thema-Config /-Repo soll validiert werden können: Ist alles drin, was benötigt wird? etc.
- Teilkonfigurationen sind problemlos möglich, d.h. ich will nur XTF aus Edit-DB publizieren. Oder die XTF der ÖREB-Rahmenmodells im Datenbezug zur Verfügung stellen (Annahme: es gibt keinen Publisher-Job oder so).
- Schema-Jobs flexibler ("Pub" in "Edit", ...)
- SO-Modelle werden aus dem lokalen Repo verwendet.

## Beispiele (?)

- HBA-Sachen, da aktuell und es noch offene Punkte (aka Bugs) gibt, die man im Code-Repo verwalten sollte (Issues).
- Seltene Baumarten, da explizites Edit-Modell und "Json-Rendering". Und in Pubmodell Vererberbung.
- Abbaustellen, da ganz simpel.

## todo
- Dev-Umgebung:
  * DB "bitnami"
  * SFTP (nur das Zertifikatgedöns war doof, sonst war es easy mit einem Dockerimage, glaubs): Zertifikate erstellen wie in Anleitung und Container starten (eigenes Image mit Zertifikaten?). In Gretl-Image muss man localhost:22 (or whatever) zu den knownhosts hinzufügen. Sonst kommt _die_ Meldung.
- INTERLIS-Modelle: Wie komme ich zum Repo? (Themen-Repo abgrasen? Im Prinzip gleich. Eventuell gibts ja ein Repo-Repo-Configfile, wo drin steht, welches Amt etc.)
  * -> PoC im Branch o.ä.
- Modell/Struktur wie das Repo aussehen muss/darf. Dann kann mit Pipeline geprüft werden, ob i.O.
  * -> Prüfung via Pipeline?
- Datensuche mit einzelnen XTF-Dateien 
  * PoC. Wo liegen sie? SFTP oder HTTP?
- GRETL-Job:
  * Db2Db: nicht interessant
  * Reicht Publisher-Task? Dann da wichtigste: der Metapublisher.
- Subunits:
  * statisch: GeoJSON vollständig manuell
  * dynamisch: GeoJSON mit Platzhalter. Datum muss in einerm File zwischengespeichert werden. Diese wird nachgeführt mit dem GRETL-Job. Eigentlich könnte gleich das GeoJSON als Speicherfile verwendet werden.
- Wie kann das HTML während Entwicklung einfach erzeugt werden, zwecks Validierung?

## Schema

- `<Thema>`: Name muss aus technischen Gründen nicht einer Konvention folgen. Was in ein Thema gehört, ist nicht exakt definiert.
  - `ili`: sämtliche Datenmodelle zum Thema
  - `publication`: 0..* Konfigurationen für die Thema-Publikation.
    - `<Thema-Publikation>`: Name des Ordners entspricht des Identifiers der Thema-Publikation
      - `meta.toml`: Zusatzinformationen für die Thema-Publikation, die entweder nicht direkt aus dem Datenmodell ableitbar sind oder Informationen aus dem Modell überschreiben sollen.
  - `gretl`: GRETL-Jobs
    - `<GRETL-Job>`: Technisch frei wählbarer Name des GRETL-Jobs. Konvention wie heute.
  - `schema`: Schema-Jobs
    - `<Schema-Job>`: Technisch frei wählbarer Name des Schema-Jobs. Konvention wie heute. 
  - `security`: Ablage einer vereinfachten Schutzbedarfsanalyse für die Daten
    - `<???.md>`: Markdown entsteht aus Github-Formular-Issue. Entweder wird das hierhin kopiert oder bleibt ein Issue oder sonst was. Muss evaluiert werden. Wenn es aber in Github ist, ist jede Änderung getrackt.
- `shared`
  - `core_data`: Stammdaten

