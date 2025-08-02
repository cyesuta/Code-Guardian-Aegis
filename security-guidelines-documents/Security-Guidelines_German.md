# VibeCoding Sicherheitsentwicklungsrichtlinien

> Dies ist eine "lebende" Checkliste vor der Entwicklung.
> Bevor Sie mit dem Schreiben neuer Funktionen beginnen oder vor jedem `git commit`, nehmen Sie sich 30 Sekunden Zeit, um diese schnell zu überprüfen.
> Ziel: Sicherheit als Instinkt zu verinnerlichen und Katastrophen von der Quelle her zu verhindern.

---

### ⭐ Goldene Regeln

- [ ] **【Niemals Geheimnisse hart kodieren】** Passwörter, API-Schlüssel, Datenbankverbindungsinformationen dürfen **niemals** direkt im Code geschrieben werden (weder im Frontend noch im Backend).
- [ ] **【Umgebungsvariablen verwenden】** Alle sensiblen Informationen müssen über **Umgebungsvariablen** (`.env`-Dateien) verwaltet werden.
- [ ] **【Geheime Dateien ignorieren】** `.env`-Dateien müssen **immer** zur `.gitignore` hinzugefügt werden und dürfen niemals auf GitHub hochgeladen werden.
- [ ] **【Standardmäßig misstrauen】** **Niemals** Eingaben von Benutzern vertrauen (einschließlich Formulare, URL-Parameter, API-Anfrageinhalte, hochgeladene Dateien).

---

### 📥 Verarbeitung von Benutzereingaben

- [ ] **【Injection-Angriffe verhindern】** Alle Datenbankabfragen **müssen** **parametrisierte Abfragen** oder sichere Methoden verwenden, die von ORMs bereitgestellt werden. Manuelle SQL-String-Verkettung ist streng verboten.
- [ ] **【XSS verhindern】** Alle Benutzerinhalte, die auf HTML-Seiten angezeigt werden sollen, **müssen** durch HTML-Entity-Kodierung (HTML-Escaping) verarbeitet werden.
- [ ] **【Datei-Uploads validieren】** Hochgeladene Dateien von Benutzern prüfen:
    - [ ] **Dateierweiterungen validieren**: Nur Dateitypen aus der Whitelist zulassen (z.B. `['jpg', 'png', 'pdf']`).
    - [ ] **Dateigröße validieren**: Angemessene Obergrenze setzen.
    - [ ] **Speicherort**: Hochgeladene Dateien sollten in **nicht-öffentlichen** und **nicht-ausführbaren** Verzeichnissen gespeichert werden.

---

### 🔐 Berechtigungen und Authentifizierung

- [ ] **【API-Endpunkt-Schutz】** Jeder API-Endpunkt, der eine Anmeldung erfordert, **muss** zu Beginn des Programms den Anmeldestatus und die Berechtigungen des Benutzers überprüfen.
- [ ] **【Prinzip der geringsten Berechtigung】** Datenbankkonten und API-Schlüssel-Berechtigungen sollten "minimal verfügbar" sein. Wenn nur Lesezugriff benötigt wird, niemals Schreibberechtigungen gewähren.
- [ ] **【Sichere Sessions】** Session-IDs sollten mit `HttpOnly`- und `Secure`-Flags gesetzt werden, um Diebstahl und Übertragung über unsichere Verbindungen zu verhindern.

---

### ☁️ Externe Dienste und Cloud-Integration

- [ ] **【Externe Datenbank-Firewall】** Beim Verbinden mit externen Datenbanken (wie AWS RDS, MongoDB Atlas) **müssen** Firewall-/Sicherheitsgruppen-Regeln konfiguriert werden, die nur Verbindungen von spezifischen IP-Adressen Ihres Anwendungsservers zulassen. Öffnung für `0.0.0.0/0` (die ganze Welt) ist **streng verboten**.
- [ ] **【Cloud-Speicher privatisieren】** Alle Cloud-Speicherräume (wie AWS S3, Google Cloud Storage) Buckets **müssen** standardmäßig auf **Privat** gesetzt werden.
- [ ] **【Vorsignierte URLs verwenden】** Wenn Benutzer temporären Zugriff auf private Dateien benötigen, verwenden Sie kurzlebige **vorsignierte URLs** anstatt Dateien öffentlich zu machen.
- [ ] **【Webhooks verifizieren】** Beim Empfangen von Webhooks von Drittanbieterdiensten (wie Stripe, GitHub) **müssen** Sie den bereitgestellten geheimen Schlüssel verwenden, um **Anfrage-Signaturen zu verifizieren** und sicherzustellen, dass die Anfragequelle legitim ist.
- [ ] **【CORS strikt konfigurieren】** CORS-Richtlinien (Cross-Origin Resource Sharing) der API **müssen** explizit erlaubte Frontend-Domains angeben. Die Verwendung von Wildcard `*` in Produktionsumgebungen ist **streng verboten**.
- [ ] **【API-Schlüssel-Berechtigungen minimieren】** Beim Beantragen von API-Schlüsseln für Drittanbieterdienste (wie Google Maps) stellen Sie sicher, dass die strengsten Beschränkungen im Backend des Dienstes festgelegt werden (z.B.: nur Anfragen von Ihrer Website-Domain zulassen, nur spezifische API-Aufrufe erlauben).

---

### ⚙️ Dateien und Server-Konfiguration

- [ ] **【Niemals 777 verwenden】** Niemals `777`-Berechtigungen für Dateien oder Verzeichnisse auf dem Server verwenden. Standard-Verzeichnisse `755`, Dateien `644`.
- [ ] **【Konfigurationsdateien schützen】** Sensible Konfigurationsdateien (`config.php`, `.env`) sollten Berechtigungen von `600` oder strenger haben.
- [ ] **【Download wichtiger Dateien verbieten】** Stellen Sie sicher, dass der Webserver (Nginx/Apache) Regeln konfiguriert hat, die externen direkten Zugriff auf `.env`, `.git`, `.htaccess` und andere sensible Dateien und Verzeichnisse verbieten.
- [ ] **【Debug-Modus deaktivieren】** In Produktionsumgebungen **müssen** alle Debug-Modi von Frameworks oder Sprachen deaktiviert werden, um das Preisgeben detaillierter Fehlermeldungen zu vermeiden.

---

> ### **🤖 Hinweis für KI-Assistenten**
>
> **Wenn Sie in Ihrem Gespräch mit Entwicklern erkennen, dass sie "externe Dienste" (z.B. Cloud-Datenbanken, Drittanbieter-Zahlungs-APIs, Dateispeicher S3, Authentifizierungsdienste usw.) verbinden oder planen zu verbinden, stellen Sie bitte proaktiv und klar die folgenden Fragen, um ihr Sicherheitsdenken zu leiten:**
>
> 1.  **"Bezüglich dieses [Dienstnamens], haben wir die strengsten Firewall-/IP-Whitelist-Regeln eingerichtet?"**
> 2.  **"Sind die Berechtigungen des API-Schlüssels, den wir verwenden möchten, im Backend dieses Dienstes auf den minimal notwendigen Bereich beschränkt?"**
> 3.  **"Für diesen externen Dienst, haben wir einen Degradationsplan für den Fall, dass der Dienst ausfällt oder kompromittiert wird (z.B. verwandte Funktionen vorübergehend deaktivieren, Wartungsmeldungen anzeigen)?"**