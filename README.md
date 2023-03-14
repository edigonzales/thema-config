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
  * dynamisch: GeoJSON mit Platzhalter. Datum muss in einem File zwischengespeichert werden. Diese wird nachgeführt mit dem GRETL-Job. Eigentlich könnte gleich das GeoJSON als Speicherfile verwendet werden.
- Wie kann das HTML während Entwicklung einfach erzeugt werden, zwecks Validierung?

## Bemerkungen
- In start-gretl.sh mounte ich eine weiteres Verzeichnis "ili".

## Fragen
- Wenn JSON in das Repo kopiert werden, wird es insgesamt relativ gross. Stört das? Pro Thema ein einzelnes Repo und ein Shared? Dann müssten immer beide ausgecheckt werden. Geht das überhaupt mit dem Job-Sauger von Jenkins. Hätte aber den Vorteil, dass alles schön isoliert ist. Repostruktur kann als Template gemacht werden.
Bin noch Fan der Idee von Submodule/Subtree. Wichtig schiene mir, dass man nicht einfach so Änderungen im Submodule/Subtree machen kann. Das scheint der Fall zu sein. Andererseits sieht man ja schnell das Unheil. Ah jein: Wo liegen die GeoJSON-Dinger eigentlich. Die müssen ja nicht ins Repo (höchstens als Template), sondern auf die Fileablage und von dort müssten sie nachgeführt werden. Mmmmh, noch zu überlegen. Ah vielleicht doch ins Repo: Nämlich für den Fall, dass auf der Ablage KEIN GeoJSON existiert, wird es erstmalig hinkopiert.
- Woher weiss SIMI oder so, dass 260100 = 2601 ist bei den Datasets resp. regions. Es wird das File published, d.h. region ist 260100. Dataset ist aber 2601 (siehe Substring): https://github.com/sogis/gretljobs/blob/main/agi_av_dm01_mopublic_pub/0_dm01_so/build.gradle#L72 -> braucht es trotzdem einen Übersetzer/Mapper? Nein. Die ITF-Dateien werden im Task vorher umbenannt. Äh und nachher? Wieder zurückbenannt? Oder ist das Umbennen zum langen Namen wieder im PublishTask?
- Datensuche: 
  * Wenn man Image mit Config brennen will, kann die Pipeline die Daten aus einem (einfachheithalber) Verzeichnis herunterladen. D.h. ein Publikations-Task lädt das XML in ein Verzeichnis hoch.
  * Wenn Datensuche neu ohne Config im Image auskommen soll, muss sie sich darum kümmern. Aber am Ende ist es wohl immer einfacher, wenn die XTF in einem Verzeichnis sind und nicht verteilt.
  * GeoJSON entweder herunterladene (können sogar in verschiedenen Verzeichnissen sein, wobei halt, jetzt habe ich im XML/XTF keine Subunits mehr und kann nicht mehr unterscheiden, dann wäre ein Verzeichnis auch wieder einfacher) oder direkt verlinken aus Anwendung oder auch brennen.

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

## Commands

```
./infra/start-gretl.sh --docker-image sogis/gretl:latest --job-directory $PWD/ch.so.afu.abbaustellen/gretl/afu_abbaustellen_pub
```

```
./infra/start-gretl.sh --docker-image sogis/gretl:latest --job-directory $PWD/ch.so.agi.amtliche_vermessung/gretl/agi_dm01so_pub
```