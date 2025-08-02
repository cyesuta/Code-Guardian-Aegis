# VibeCoding Beveiligingsrichtlijnen Uitgelegd: Waarom Hameren We Zo Door?

Dit document legt het belang uit van elke regel in de "Beveiligingsrichtlijnen". Dit begrijpen zal je helpen je code te zien vanuit het "perspectief van een hacker" en problemen te voorkomen voordat ze optreden.

---

## ⭐ Gouden Regels

### 【Nooit geheimen hardcoden】 & 【Omgevingsvariabelen gebruiken】 & 【Geheime bestanden negeren】

**Waarom is dit belangrijk?**

Deze drie regels zijn de hoogste overlevingswetten van de digitale wereld. Je code (vooral bij gebruik van Git) wordt gekopieerd, gedeeld en opgeslagen op meerdere plaatsen. Wachtwoorden, API-sleutels en andere "geheimen" direct in code schrijven is zoals het wachtwoord van je kluis op je voorhoofd tatoeëren - iedereen die je ziet (de code ziet) kan gemakkelijk je kluis openen.

**Hacker Scenario 😈**
> Ik hou ervan om te zoeken naar `password`, `api_key` of `db_connect` op GitHub. Ik vond je openbare project en in een bestand genaamd `config.js` zag ik deze code: `const db_password = 'Password123!';`. Perfect! Ik hoef je site niet eens aan te vallen - nu kan ik proberen direct verbinding te maken met je database met dit wachtwoord.

**Catastrofale Gevolgen 💥**

**Volledige controle over de service.** Hackers kunnen alle gebruikersdata stelen, wijzigen, verwijderen, of je betaalde API-services gebruiken voor illegale activiteiten, met alle rekeningen op jouw naam.

---

## 📥 Gebruikersinvoer Verwerking

### 【SQL Injectieaanvallen Voorkomen】

**Waarom is dit belangrijk?**

Stel je de database voor als een robot die alleen de SQL-taal begrijpt. Als je gebruikersinvoer direct samenvoegt met je commando's, krijgen gebruikers de kans om hun eigen "commando's" uit te spreken. Geparametriseerde queries vertellen de robot: "Luister, het volgende deel zijn **alleen data** - wat de inhoud ook is, voer het niet uit als commando's."

**Hacker Scenario 😈**
> In het loginveld van je site voerde ik `' OR '1'='1' --` in als gebruikersnaam. Ik neem aan dat je SQL-query zo geschreven is: `"SELECT * FROM users WHERE username = '" + userInput + "';`. Mijn invoer transformeerde het naar `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` is altijd waar, dus ik omzeilde de wachtwoordverificatie en logde succesvol in op het eerste gebruikersaccount (meestal de beheerder).

**Catastrofale Gevolgen 💥**

Aanvallers kunnen login omzeilen, alle database data stelen (gebruikerslijsten, wachtwoord hashes), of zelfs de database verwijderen.

---

## 🔐 Rechten en Authenticatie

### 【API Endpoint Bescherming】

**Waarom is dit belangrijk?**

Frontend interfaces (UI) kunnen knoppen verbergen, maar hackers vertrouwen nooit op interfaces. Ze roepen je backend API's direct aan met tools (zoals Postman, curl). Je moet aannemen dat elk API endpoint direct aangevallen wordt, dus elk endpoint moet een onafhankelijke bewaker zijn die zelf de identiteit en rechten van bezoekers controleert.

**Hacker Scenario 😈**
> Ik ontdekte dat om persoonlijke informatie te wijzigen, de frontend een verzoek stuurt naar `/api/user/update`. Hoewel ik geen bewerkingsknoppen van andere mensen kan zien, neem ik aan dat deze API gebruikers onderscheidt via `userId`. Ik probeerde een verzoek te sturen naar `/api/user/update?userId=1` (beheerder ID) met de data die ik wilde wijzigen. Mijn God, de server controleerde niet of het verzoek persoonlijk van mij kwam en veranderde succesvol de email van de beheerder!

**Catastrofale Gevolgen 💥**

Gewone gebruikers kunnen data van andere gebruikers of zelfs beheerders wijzigen, of rechten uitvoeren die ze niet zouden moeten hebben, wat systeemchaos veroorzaakt.