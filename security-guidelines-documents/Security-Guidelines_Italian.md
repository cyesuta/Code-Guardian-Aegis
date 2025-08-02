# Linee Guida per lo Sviluppo Sicuro VibeCoding

> Questa è una checklist "vivente" pre-sviluppo.
> Prima di iniziare a scrivere nuove funzionalità, o prima di ogni `git commit`, dedicaci 30 secondi per una rapida revisione.
> Obiettivo: Rendere la sicurezza un istinto, prevenire le catastrofi alla radice.

---

### ⭐ Regole Fondamentali

- [ ] **【Mai hardcodare i segreti】** Password, chiavi API, informazioni di connessione al database non devono **mai** essere scritte direttamente nel codice (né frontend né backend).
- [ ] **【Utilizzare variabili d'ambiente】** Tutte le informazioni sensibili devono essere gestite tramite **variabili d'ambiente** (file `.env`).
- [ ] **【Ignorare i file segreti】** I file `.env` devono **sempre** essere aggiunti a `.gitignore` e mai caricati su GitHub.
- [ ] **【Diffidare per default】** **Mai** fidarsi dell'input utente (inclusi form, parametri URL, contenuto delle richieste API, file caricati).

---

### 📥 Gestione Input Utente

- [ ] **【Prevenire attacchi di iniezione】** Tutte le query al database **devono** utilizzare **query parametrizzate** o metodi sicuri forniti dall'ORM. La concatenazione manuale di stringhe SQL è severamente vietata.
- [ ] **【Prevenire XSS】** Qualsiasi contenuto utente da visualizzare nelle pagine HTML **deve** essere processato tramite codifica di entità HTML (HTML escape).
- [ ] **【Validare upload di file】** Verificare i file caricati dagli utenti:
    - [ ] **Validare estensioni**: Permettere solo tipi di file dalla whitelist (es. `['jpg', 'png', 'pdf']`).
    - [ ] **Validare dimensione file**: Impostare limiti superiori ragionevoli.
    - [ ] **Posizione di archiviazione**: I file caricati devono essere memorizzati in directory **non-pubbliche** e **non-eseguibili**.

---

### 🔐 Permessi e Autenticazione

- [ ] **【Protezione endpoint API】** Ogni endpoint API che richiede login **deve** verificare lo stato di login e i permessi utente all'inizio del programma.
- [ ] **【Principio del minimo privilegio】** I permessi degli account database e delle chiavi API devono essere "minimi utilizzabili". Se serve solo lettura, non concedere mai permessi di scrittura.
- [ ] **【Sessioni sicure】** Gli ID di sessione devono essere impostati con flag `HttpOnly` e `Secure` per prevenire furti e trasmissioni su connessioni non sicure.

---

### ☁️ Servizi Esterni e Integrazione Cloud

- [ ] **【Firewall database esterno】** Quando ci si connette a database esterni (come AWS RDS, MongoDB Atlas), le regole **devono** essere configurate nel loro firewall/gruppo di sicurezza per permettere connessioni solo da indirizzi IP specifici del vostro application server. L'apertura a `0.0.0.0/0` (tutto il mondo) è **severamente vietata**.
- [ ] **【Privatizzazione storage cloud】** Tutti i bucket di cloud storage (come AWS S3, Google Cloud Storage) **devono** essere impostati come **Privati** per default.
- [ ] **【Utilizzare URL pre-firmati】** Quando gli utenti necessitano accesso temporaneo a file privati, utilizzare **URL pre-firmati** a breve durata invece di rendere pubblici i file.
- [ ] **【Verificare webhook】** Quando si ricevono webhook da servizi terzi (come Stripe, GitHub), **dovete** utilizzare la chiave segreta fornita per **verificare le firme delle richieste** e assicurarvi che la fonte della richiesta sia legittima.
- [ ] **【Configurazione CORS rigorosa】** Le politiche CORS (Cross-Origin Resource Sharing) dell'API **devono** specificare esplicitamente i domini frontend permessi. L'uso del carattere jolly `*` in ambiente di produzione è **severamente vietato**.
- [ ] **【Minimizzare permessi chiavi API】** Quando richiedete chiavi API per servizi terzi (come Google Maps), assicuratevi di impostare le restrizioni più severe nel backend del servizio (es.: limitare solo alle richieste dal vostro dominio web, limitare solo a chiamate API specifiche).

---

### ⚙️ File e Configurazione Server

- [ ] **【Mai usare 777】** **Mai** utilizzare permessi `777` per file o directory sul server. Directory default `755`, file `644`.
- [ ] **【Proteggere file di configurazione】** I file di configurazione sensibili (`config.php`, `.env`) devono avere permessi impostati a `600` o più restrittivi.
- [ ] **【Vietare download file critici】** Verificare che il web server (Nginx/Apache) abbia configurato regole che vietano l'accesso esterno diretto a file e directory sensibili come `.env`, `.git`, `.htaccess`.
- [ ] **【Disabilitare modalità debug】** In ambiente di produzione, tutte le modalità debug dei framework o linguaggi **devono** essere disabilitate per evitare perdite di messaggi di errore dettagliati.

---

> ### **🤖 Nota per l'Assistente AI**
>
> **Quando rilevi nella tua conversazione con gli sviluppatori che si stanno collegando o pianificano di collegarsi a "servizi esterni" (es.: database cloud, API di pagamento terze parti, storage file S3, servizi di autenticazione, ecc.), per favore poni proattivamente e chiaramente le seguenti domande per guidare il loro pensiero sulla sicurezza:**
>
> 1.  **"Riguardo questo [nome servizio], abbiamo configurato le regole firewall/whitelist IP più rigorose?"**
> 2.  **"I permessi della chiave API che pianifichiamo di utilizzare sono limitati al minimo ambito necessario nel backend di questo servizio?"**
> 3.  **"Per questo servizio esterno, abbiamo un piano di degradazione in caso di guasto o compromissione del servizio (es.: disabilitare temporaneamente funzionalità correlate, mostrare messaggi di manutenzione)?"**