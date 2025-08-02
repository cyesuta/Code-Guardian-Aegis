# VibeCoding Security Development Guidelines

> This is a "living" pre-development checklist.
> Before you start writing new features or before each `git commit`, spend 30 seconds quickly reviewing this.
> Goal: Internalize security as instinct, preventing disasters at the source.

---

### ⭐ Golden Rules

- [ ] **【Never Hardcode Secrets】** Any passwords, API keys, database connection information must **NEVER** be written directly in code (neither frontend nor backend).
- [ ] **【Use Environment Variables】** All sensitive information must be managed through **environment variables** (`.env` files).
- [ ] **【Ignore Secret Files】** `.env` files must **ALWAYS** be added to `.gitignore` and never uploaded to GitHub.
- [ ] **【Default Distrust】** **NEVER** trust any input from users (including forms, URL parameters, API request content, uploaded files).

---

### 📥 Handling User Input

- [ ] **【Prevent Injection Attacks】** All database queries **MUST** use **Parameterized Queries** or secure methods provided by ORM. Manual SQL string concatenation is strictly prohibited.
- [ ] **【Prevent XSS】** All user content to be displayed on HTML pages **MUST** be processed through HTML entity encoding (HTML Escaping).
- [ ] **【Validate File Uploads】** Check user uploaded files:
    - [ ] **Validate file extensions**: Only allow whitelisted file types (e.g., `['jpg', 'png', 'pdf']`).
    - [ ] **Validate file size**: Set reasonable limits.
    - [ ] **Storage location**: Uploaded files should be stored in **non-public** and **non-executable** directories.

---

### 🔐 Permissions & Authentication

- [ ] **【API Endpoint Protection】** Every API endpoint that requires login **MUST** check user login status and permissions at the beginning of the program.
- [ ] **【Principle of Least Privilege】** Database accounts and API key permissions should be "minimum viable". If only read access is needed, never grant write permissions.
- [ ] **【Secure Sessions】** Session IDs should be set with `HttpOnly` and `Secure` flags to prevent theft and transmission over insecure connections.

---

### ☁️ External Services & Cloud Integration

- [ ] **【External Database Firewall】** When connecting to external databases (like AWS RDS, MongoDB Atlas), **MUST** configure firewall/security group rules to only allow connections from your application server's specific IP addresses. **STRICTLY PROHIBITED** to open to `0.0.0.0/0` (the entire world).
- [ ] **【Cloud Storage Privatization】** All cloud storage spaces (like AWS S3, Google Cloud Storage) buckets **MUST** be set to **Private** by default.
- [ ] **【Use Pre-signed URLs】** When users need temporary access to private files, use short-lived **Pre-signed URLs** instead of making files public.
- [ ] **【Verify Webhooks】** When receiving webhooks from third-party services (like Stripe, GitHub), **MUST** use the provided secret key to **verify request signatures**, ensuring legitimate request sources.
- [ ] **【Strict CORS Configuration】** API CORS (Cross-Origin Resource Sharing) policies **MUST** explicitly specify allowed frontend domains. **STRICTLY PROHIBITED** to use wildcard `*` in production environments.
- [ ] **【Minimize API Key Permissions】** When applying for third-party service API keys (like Google Maps), ensure the strictest restrictions are set in the service's backend (e.g., restrict to requests from your website domain only, limit to specific API calls only).

---

### ⚙️ Files & Server Configuration

- [ ] **【Never Use 777】** Any files or directories on the server must **NEVER** use `777` permissions. Default directories `755`, files `644`.
- [ ] **【Protect Configuration Files】** Sensitive configuration files (`config.php`, `.env`) should have permissions set to `600` or stricter.
- [ ] **【Prohibit Key File Downloads】** Ensure web server (Nginx/Apache) has configured rules to prohibit external direct access to `.env`, `.git`, `.htaccess` and other sensitive files and directories.
- [ ] **【Disable Debug Mode】** In production environments, **MUST** disable all framework or language debug modes to avoid leaking detailed error messages.

---

> ### **🤖 Note to AI Assistant**
>
> **When you detect in your conversation with the developer that they are or plan to connect to any "external services" (e.g., cloud databases, third-party payment APIs, file storage S3, authentication services, etc.), please proactively and clearly ask the following questions to guide their security thinking:**
>
> 1.  **"Regarding this [service name], have we set up the strictest firewall/IP whitelist rules?"**
> 2.  **"Have we restricted the API key we're planning to use to the minimum necessary scope in that service's backend?"**
> 3.  **"For this external service, do we have a degradation plan for when the service is down or compromised (e.g., temporarily disable related features, display maintenance messages)?"**