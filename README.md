<p align="center">
  <img src="apple-touch-icon.png" width="96" alt="Stoppuhr mit Weihnachtsmütze">
</p>

<h1 align="center">Christmas Games: Wikipedia-Speedrun</h1>

<p align="center">
  Ein Familienspiel für Weihnachten: Wer klickt sich am schnellsten durch Wikipedia?<br>
  <strong><a href="https://liamgraefe.github.io/christmas-games/">▶ Jetzt spielen</a></strong>
</p>

---

## Worum geht's?

Alle Teams bekommen denselben Start- und Zielartikel, zum Beispiel **Weihnachtsbaum → Albert Einstein**. Sie dürfen sich nur über Links im Fließtext der Artikel bis zum Ziel klicken. Die App misst Zeit und Klicks und vergibt die Punkte.

Gedacht ist das Spiel für **3 Teams à 4 Personen**. Mehr oder weniger Teams gehen auch.

## Regeln

- Klickbar sind nur **blaue Links in normalen Absätzen**. Infoboxen, Tabellen, Listen, Bildunterschriften, Fußnoten und Navigationsleisten sind gesperrt.
- Zurück-Button, Suche und Adresszeile sind tabu.
- Die Uhr läuft nur, solange ein Artikel angezeigt wird. Ladezeiten zählen nicht.
- Pro Runde gibt es **1 Punkt für die schnellste Zeit** und **1 Punkt für die wenigsten Klicks**. Bei Gleichstand bekommen alle Gleichplatzierten den Punkt.
- Wer aufgibt, bekommt in dieser Runde keine Punkte.

## So wird gespielt

1. Ein Gerät öffnet die Seite und tippt auf **„Neues Spiel erstellen“**. Es erscheint ein 5-stelliger Spielcode.
2. Mit **„Link kopieren“** den Link an alle schicken. Die anderen können auch den Code eingeben.
3. Start- und Zielartikel eintragen oder mit 🎲 auswürfeln, dann **„Runde starten“**.
4. Jedes Team tippt auf seinem eigenen Gerät bei seinem Team auf **„Spielen“** und dann auf **„Los geht's!“**. Alle spielen gleichzeitig.
5. Die Ergebnisse erscheinen sofort auf allen Geräten. Wenn alle fertig sind, auf **„Runde auswerten“** tippen und danach eine **„Nächste Runde“** starten.

Hat ein Team kein eigenes Gerät, kann man sein Ergebnis mit **„Manuell“** eintragen.

## Ideen für Runden

| Start | Ziel |
|---|---|
| Weihnachtsbaum | Albert Einstein |
| Lebkuchen | Mondlandung |
| Rentier | Python (Programmiersprache) |
| Glühwein | Titanic (Schiff) |
| Leipzig | Pinguine |

## Technik

- **Eine einzige Datei:** `index.html` mit HTML, CSS und JavaScript, ohne Build-Schritt und ohne Abhängigkeiten. Gehostet wird sie über GitHub Pages.
- **Wikipedia:** Artikel werden live über die [MediaWiki-API](https://www.mediawiki.org/wiki/API:Main_page) geladen (`action=parse`). Die App filtert die Links so, dass nur Links in Absätzen klickbar bleiben. Deutsche und englische Wikipedia sind umschaltbar.
- **Gemeinsamer Spielstand:** Der Spielstand liegt in der [Firebase Realtime Database](https://firebase.google.com/docs/database), jeder Spielraum unter `speedrun/<CODE>`. Alle Änderungen laufen über Transaktionen, damit sich gleichzeitige Eingaben nicht überschreiben.
- **Lokaler Modus:** Ist oben in `index.html` keine Firebase-Konfiguration eingetragen (`window.FIREBASE_CONFIG = null`), läuft die App auf einem einzigen Gerät. Der Punktestand wird dann im Browser gespeichert.

### Eigene Firebase-Datenbank verwenden

1. In der [Firebase-Konsole](https://console.firebase.google.com) ein Projekt anlegen und eine **Realtime Database** erstellen.
2. Diese Regeln eintragen:
   ```json
   {
     "rules": {
       "speedrun": {
         "$room": {
           ".read": true,
           ".write": true,
           ".validate": "$room.matches(/^[A-Z0-9]{5}$/)"
         }
       }
     }
   }
   ```
3. Eine Web-App hinzufügen und die `firebaseConfig` oben in `index.html` bei `window.FIREBASE_CONFIG` einfügen. Die `databaseURL` muss enthalten sein.

> Der Firebase-`apiKey` ist kein Geheimnis. Er kennzeichnet nur das Projekt. Wer worauf zugreifen darf, regeln allein die Datenbankregeln.

## Lokal testen

```bash
git clone https://github.com/liamgraefe/christmas-games.git
cd christmas-games
python3 -m http.server 8000
# dann http://localhost:8000 öffnen
```

---

<p align="center">Frohe Weihnachten und viel Spaß beim Klicken! 🎄</p>
