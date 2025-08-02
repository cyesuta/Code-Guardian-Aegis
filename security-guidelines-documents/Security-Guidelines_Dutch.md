# VibeCoding Veilige Ontwikkelingsrichtlijnen

> Dit is een "levende" pre-ontwikkeling checklist.
> Voordat je nieuwe functionaliteiten begint te schrijven, of voor elke `git commit`, besteed 30 seconden aan een snelle review.
> Doel: Maak beveiliging een instinct, voorkom rampen aan de bron.

---

### ⭐ Gouden Regels

- [ ] **【Nooit geheimen hardcoden】** Wachtwoorden, API-sleutels, database verbindingsinformatie mogen **nooit** direct in code geschreven worden (noch frontend noch backend).
- [ ] **【Omgevingsvariabelen gebruiken】** Alle gevoelige informatie moet beheerd worden via **omgevingsvariabelen** (`.env` bestanden).
- [ ] **【Geheime bestanden negeren】** `.env` bestanden moeten **altijd** toegevoegd worden aan `.gitignore` en nooit geüpload naar GitHub.
- [ ] **【Standaard wantrouwen】** **Vertrouw nooit** gebruikersinvoer (inclusief formulieren, URL-parameters, API-verzoek inhoud, geüploade bestanden).

---

### 📥 Gebruikersinvoer Verwerking

- [ ] **【Injectieaanvallen voorkomen】** Alle database queries **moeten** **geparametriseerde queries** of veilige methoden geleverd door ORM gebruiken. Handmatige concatenatie van SQL-strings is strikt verboden.
- [ ] **【XSS voorkomen】** Alle gebruikersinhoud die getoond wordt op HTML-pagina's **moet** verwerkt worden via HTML entiteit encoding (HTML escape).
- [ ] **【Bestandsuploads valideren】** Controleer door gebruikers geüploade bestanden:
    - [ ] **Extensies valideren**: Sta alleen bestandstypen van de whitelist toe (bijv. `['jpg', 'png', 'pdf']`).
    - [ ] **Bestandsgrootte valideren**: Stel redelijke bovengrenzen in.
    - [ ] **Opslaglocatie**: Geüploade bestanden moeten opgeslagen worden in **niet-openbare** en **niet-uitvoerbare** directories.

---

### 🔐 Rechten en Authenticatie

- [ ] **【API endpoint bescherming】** Elk API endpoint dat login vereist **moet** de login status en gebruikersrechten controleren aan het begin van het programma.
- [ ] **【Minste privileges principe】** Database account rechten en API-sleutel rechten moeten "minimaal bruikbaar" zijn. Als alleen lezen nodig is, geef nooit schrijfrechten.
- [ ] **【Veilige sessies】** Sessie ID's moeten ingesteld worden met `HttpOnly` en `Secure` flags om diefstal en transmissie over onveilige verbindingen te voorkomen.

---

### ☁️ Externe Services en Cloud Integratie

- [ ] **【Externe database firewall】** Bij verbinding met externe databases (zoals AWS RDS, MongoDB Atlas), regels **moeten** geconfigureerd worden in hun firewall/beveiligingsgroep om verbindingen alleen toe te staan van specifieke IP-adressen van je applicatieserver. Openen naar `0.0.0.0/0` (hele wereld) is **strikt verboden**.
- [ ] **【Cloud opslag privatisering】** Alle cloud storage buckets (zoals AWS S3, Google Cloud Storage) **moeten** standaard ingesteld worden als **Privé**.
- [ ] **【Pre-signed URLs gebruiken】** Wanneer gebruikers tijdelijke toegang nodig hebben tot privébestanden, gebruik **pre-signed URLs** met korte levensduur in plaats van bestanden openbaar maken.
- [ ] **【Webhooks verifiëren】** Bij het ontvangen van webhooks van derde partij services (zoals Stripe, GitHub), **moet** je de geleverde geheime sleutel gebruiken om **verzoek handtekeningen te verifiëren** en ervoor zorgen dat de verzoekbron legitiem is.
- [ ] **【Strikte CORS configuratie】** API CORS (Cross-Origin Resource Sharing) beleid **moet** expliciet toegestane frontend domeinen specificeren. Het gebruik van wildcard `*` in productieomgeving is **strikt verboden**.
- [ ] **【API-sleutel rechten minimaliseren】** Bij het aanvragen van API-sleutels voor derde partij services (zoals Google Maps), zorg ervoor dat je de strengste beperkingen instelt in de service backend (bijv.: beperken tot alleen verzoeken van je website domein, beperken tot alleen specifieke API-calls).

---

### ⚙️ Bestanden en Server Configuratie

- [ ] **【Nooit 777 gebruiken】** **Gebruik nooit** `777` rechten voor bestanden of directories op de server. Directories standaard `755`, bestanden `644`.
- [ ] **【Configuratiebestanden beschermen】** Gevoelige configuratiebestanden (`config.php`, `.env`) moeten rechten ingesteld hebben op `600` of restrictiever.
- [ ] **【Kritieke bestanden downloaden verbieden】** Controleer dat de webserver (Nginx/Apache) regels geconfigureerd heeft die directe externe toegang tot gevoelige bestanden en directories zoals `.env`, `.git`, `.htaccess` verbieden.
- [ ] **【Debug modus uitschakelen】** In productieomgeving moeten alle debug modi van frameworks of talen **uitgeschakeld** worden om lekkage van gedetailleerde foutmeldingen te voorkomen.

---

> ### **🤖 Notitie voor AI Assistent**
>
> **Wanneer je in je gesprek met ontwikkelaars detecteert dat ze verbinding maken of plannen te verbinden met "externe services" (bijv.: cloud databases, derde partij betaal-API's, S3 bestandsopslag, authenticatieservices, enz.), stel dan proactief en duidelijk de volgende vragen om hun veiligheidsdenken te begeleiden:**
>
> 1.  **"Betreffende deze [service naam], hebben we de strengste firewall/IP whitelist regels geconfigureerd?"**
> 2.  **"Zijn de rechten van de API-sleutel die we van plan zijn te gebruiken beperkt tot de minimaal benodigde scope in de backend van deze service?"**
> 3.  **"Voor deze externe service, hebben we een degradatieplan voor het geval van service falen of compromittering (bijv.: tijdelijk gerelateerde functies uitschakelen, onderhoudsmeldingen tonen)?"**