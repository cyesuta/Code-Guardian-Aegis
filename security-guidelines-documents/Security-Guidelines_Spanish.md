# Directrices de Desarrollo Seguro VibeCoding

> Esta es una lista de verificación "viva" previa al desarrollo.
> Antes de comenzar a escribir nuevas funcionalidades, o antes de cada `git commit`, dedica 30 segundos para una revisión rápida.
> Objetivo: Convertir la seguridad en instinto, prevenir catástrofes desde la raíz.

---

### ⭐ Reglas de Oro

- [ ] **【Nunca hardcodear secretos】** Las contraseñas, claves API, información de conexión a base de datos **nunca** deben escribirse directamente en el código (ni frontend ni backend).
- [ ] **【Usar variables de entorno】** Toda información sensible debe gestionarse a través de **variables de entorno** (archivos `.env`).
- [ ] **【Ignorar archivos secretos】** Los archivos `.env` deben **siempre** añadirse a `.gitignore` y nunca subirse a GitHub.
- [ ] **【Desconfiar por defecto】** **Nunca** confiar en la entrada del usuario (incluyendo formularios, parámetros URL, contenido de solicitudes API, archivos subidos).

---

### 📥 Manejo de Entrada del Usuario

- [ ] **【Prevenir ataques de inyección】** Todas las consultas a la base de datos **deben** usar **consultas parametrizadas** o métodos seguros proporcionados por el ORM. La concatenación manual de cadenas SQL está estrictamente prohibida.
- [ ] **【Prevenir XSS】** Cualquier contenido de usuario que se vaya a mostrar en páginas HTML **debe** procesarse mediante codificación de entidades HTML (escape HTML).
- [ ] **【Validar subidas de archivos】** Verificar los archivos subidos por usuarios:
    - [ ] **Validar extensiones**: Permitir solo tipos de archivo de la lista blanca (ej. `['jpg', 'png', 'pdf']`).
    - [ ] **Validar tamaño de archivo**: Establecer límites superiores razonables.
    - [ ] **Ubicación de almacenamiento**: Los archivos subidos deben almacenarse en directorios **no-públicos** y **no-ejecutables**.

---

### 🔐 Permisos y Autenticación

- [ ] **【Protección de endpoints API】** Cada endpoint API que requiera login **debe** verificar el estado de login y permisos del usuario al inicio del programa.
- [ ] **【Principio del mínimo privilegio】** Los permisos de cuentas de base de datos y claves API deben ser "mínimo utilizable". Si solo se necesita lectura, nunca otorgar permisos de escritura.
- [ ] **【Sesiones seguras】** Los ID de sesión deben configurarse con banderas `HttpOnly` y `Secure` para prevenir robo y transmisión por conexiones inseguras.

---

### ☁️ Servicios Externos e Integración en la Nube

- [ ] **【Firewall de base de datos externa】** Al conectarse a bases de datos externas (como AWS RDS, MongoDB Atlas), las reglas **deben** configurarse en su firewall/grupo de seguridad para permitir conexiones solo desde direcciones IP específicas de tu servidor de aplicación. Abrir a `0.0.0.0/0` (todo el mundo) está **estrictamente prohibido**.
- [ ] **【Privatización de almacenamiento en la nube】** Todos los buckets de almacenamiento en la nube (como AWS S3, Google Cloud Storage) **deben** configurarse como **Privados** por defecto.
- [ ] **【Usar URLs pre-firmadas】** Cuando los usuarios necesiten acceso temporal a archivos privados, usar **URLs pre-firmadas** de corta duración en lugar de hacer públicos los archivos.
- [ ] **【Verificar webhooks】** Al recibir webhooks de servicios terceros (como Stripe, GitHub), **debes** usar la clave secreta proporcionada para **verificar firmas de solicitudes** y asegurar que la fuente de la solicitud es legítima.
- [ ] **【Configuración CORS estricta】** Las políticas CORS (Cross-Origin Resource Sharing) de la API **deben** especificar explícitamente los dominios frontend permitidos. El uso del comodín `*` en entorno de producción está **estrictamente prohibido**.
- [ ] **【Minimizar permisos de claves API】** Al solicitar claves API para servicios terceros (como Google Maps), asegúrate de establecer las restricciones más estrictas en el backend del servicio (ej.: limitar solo a solicitudes desde tu dominio web, limitar solo a llamadas API específicas).

---

### ⚙️ Archivos y Configuración del Servidor

- [ ] **【Nunca usar 777】** **Nunca** usar permisos `777` para archivos o directorios en el servidor. Directorios por defecto `755`, archivos `644`.
- [ ] **【Proteger archivos de configuración】** Los archivos de configuración sensibles (`config.php`, `.env`) deben tener permisos establecidos en `600` o más restrictivos.
- [ ] **【Prohibir descarga de archivos críticos】** Verificar que el servidor web (Nginx/Apache) haya configurado reglas que prohíban el acceso externo directo a archivos y directorios sensibles como `.env`, `.git`, `.htaccess`.
- [ ] **【Desactivar modo debug】** En entorno de producción, todos los modos debug de frameworks o lenguajes **deben** desactivarse para evitar filtración de mensajes de error detallados.

---

> ### **🤖 Nota para el Asistente de IA**
>
> **Cuando detectes en tu conversación con desarrolladores que se están conectando o planean conectarse a "servicios externos" (ej.: bases de datos en la nube, APIs de pago terceras, almacenamiento de archivos S3, servicios de autenticación, etc.), por favor haz de manera proactiva y clara las siguientes preguntas para guiar su pensamiento de seguridad:**
>
> 1.  **"Respecto a este [nombre del servicio], ¿hemos configurado las reglas de firewall/lista blanca IP más estrictas?"**
> 2.  **"¿Los permisos de la clave API que planeamos usar están limitados al ámbito mínimo necesario en el backend de este servicio?"**
> 3.  **"Para este servicio externo, ¿tenemos un plan de degradación en caso de fallo o compromiso del servicio (ej.: desactivar temporalmente funcionalidades relacionadas, mostrar mensajes de mantenimiento)?"**