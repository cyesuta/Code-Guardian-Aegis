# Spiegazione delle Linee Guida di Sicurezza VibeCoding: Perché Insistiamo Tanto?

Questo documento spiega l'importanza di ogni regola nelle "Linee Guida di Sicurezza". Comprendere questo vi aiuterà a vedere il vostro codice dalla "prospettiva di un hacker" e prevenire problemi prima che si verifichino.

---

## ⭐ Regole Fondamentali

### 【Mai hardcodare i segreti】 & 【Utilizzare variabili d'ambiente】 & 【Ignorare file segreti】

**Perché è importante?**

Queste tre regole sono le leggi supreme di sopravvivenza del mondo digitale. Il vostro codice (specialmente quando si usa Git) verrà copiato, condiviso e archiviato in molti posti. Scrivere password, chiavi API e altri "segreti" direttamente nel codice è come tatuarsi la password della cassaforte sulla fronte - chiunque vi veda (veda il codice) può facilmente aprire la vostra cassaforte.

**Scenario dell'Hacker 😈**
> Adoro cercare `password`, `api_key` o `db_connect` su GitHub. Ho trovato il vostro progetto pubblico e in un file chiamato `config.js` ho visto questo codice: `const db_password = 'Password123!';`. Perfetto! Non ho nemmeno bisogno di attaccare il vostro sito - ora posso provare a connettermi direttamente al vostro database con questa password.

**Conseguenze Catastrofiche 💥**

**Controllo completo del servizio.** Gli hacker possono rubare, alterare, eliminare tutti i dati dei vostri utenti, o utilizzare i vostri servizi API a pagamento per attività illegali, con tutte le fatture a vostro carico.

---

## 📥 Gestione Input Utente

### 【Prevenire Attacchi SQL Injection】

**Perché è importante?**

Immaginate il database come un robot che capisce solo il linguaggio SQL. Se concatenate direttamente l'input utente con i vostri comandi, gli utenti hanno l'opportunità di pronunciare i propri "comandi". Le query parametrizzate dicono al robot: "Ascolta, la parte seguente sono **solo dati** - qualunque cosa contenga, non eseguirla come comandi."

**Scenario dell'Hacker 😈**
> Nel campo login del vostro sito, ho inserito `' OR '1'='1' --` come username. Suppongo che la vostra query SQL sia scritta così: `"SELECT * FROM users WHERE username = '" + userInput + "';`. Il mio input l'ha trasformata in `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` è sempre vero, quindi ho bypassato la verifica password e ho fatto login con successo nel primo account utente (solitamente l'amministratore).

**Conseguenze Catastrofiche 💥**

Gli attaccanti possono bypassare il login, rubare tutti i dati del database (liste utenti, hash password), o persino eliminare il database.

---

## 🔐 Permessi e Autenticazione

### 【Protezione Endpoint API】

**Perché è importante?**

Le interfacce frontend (UI) possono nascondere bottoni, ma gli hacker non si fidano mai delle interfacce. Chiamano direttamente le vostre API backend con strumenti (come Postman, curl). Dovete assumere che ogni endpoint API verrà attaccato direttamente, quindi ogni endpoint deve essere una guardia indipendente, verificando autonomamente l'identità e i permessi dei visitatori.

**Scenario dell'Hacker 😈**
> Ho scoperto che per modificare informazioni personali, il frontend invia una richiesta a `/api/user/update`. Anche se non posso vedere i bottoni di modifica di altre persone, suppongo che questa API distingua gli utenti tramite `userId`. Ho provato a inviare una richiesta a `/api/user/update?userId=1` (ID dell'amministratore) con i dati che volevo modificare. Mio Dio, il server non ha verificato se la richiesta proveniva da me personalmente e ha modificato con successo l'email dell'amministratore!

**Conseguenze Catastrofiche 💥**

Utenti ordinari possono alterare dati di altri utenti o persino amministratori, o eseguire permessi che non dovrebbero avere, causando caos sistemico.