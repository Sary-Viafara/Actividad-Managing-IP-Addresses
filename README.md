### **1. ¿Qué es una dirección IP?**
Completa la tabla así:

| Concepto | Definición simple | Nivel técnico breve | Ejemplo |
|----------|-------------------|---------------------|--------|
| **IP** | Es como la dirección de tu casa, pero para un dispositivo en una red. Sirve para que otros dispositivos sepan dónde encontrarte. | Identificador único numérico asignado a cada dispositivo en una red. Permite el direccionamiento y enrutamiento de datos. | Tu computadora tiene una IP para conectarse a internet. |
| **IPv4** | El formato más común. Son cuatro números separados por puntos. Es como un número de teléfono tradicional. | Protocolo de 32 bits con unos 4.300 millones de direcciones únicas. Ejemplo: `192.168.1.1`. | La IP de tu router suele ser `192.168.1.1` o `192.168.0.1`. |
| **IPv6** | Un formato más nuevo con letras y números, para que nunca falten direcciones. Es como un código alfanumérico más largo. | Protocolo de 128 bits que resuelve el agotamiento de IPv4. Ejemplo: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`. | Se usa en móviles y dispositivos modernos de forma creciente. |

---

### **2. Tipos de IP (clave para developers)**

**Diferencias primero:**
- **IP pública vs privada**: La pública es la que te identifica en internet (única en el mundo). La privada es la que usas dentro de tu red local (casa, oficina) y no es accesible directamente desde fuera.
- **IP estática vs dinámica**: Estática no cambia nunca; dinámica puede cambiar cada vez que te conectas. La dinámica es más común para usuarios domésticos.

Ahora la tabla:

| Tipo | ¿Qué es? | ¿Cuándo se usa? | Ejemplo real |
|------|----------|-----------------|--------------|
| **Pública** | Dirección visible en internet, única mundialmente. | Para que cualquier persona en internet pueda acceder a tu servidor web, API o servicio. | La IP de un servidor de AWS donde está desplegado tu backend: `54.123.45.67`. |
| **Privada** | Dirección válida solo dentro de una red local (como tu casa u oficina). | Para comunicar tu computadora con tu impresora, o tu frontend local con un backend local sin exponerlo a internet. | `192.168.1.15` (tu PC), `192.168.1.100` (tu base de datos local). |
| **Estática** | IP fija que no cambia con el tiempo. | Cuando necesitas que un servicio siempre esté en la misma dirección, por ejemplo, un servidor de base de datos crítico. | La IP de una base de datos en la nube que configuras en tu backend como `10.0.1.50`. |
| **Dinámica** | IP que puede cambiar cada vez que el dispositivo se conecta a la red. | En redes domésticas o móviles para simplificar la gestión. | La IP que te asigna tu router en casa cuando enciendes el WiFi. Puede cambiar si apagas el router. |

---

### **3. Conexión directa con desarrollo (CRÍTICO)**

Vamos con las respuestas a preguntas clave:

- **¿Qué IP usa tu backend cuando está en local?**
  - `localhost` o `127.0.0.1`. Es una IP especial que siempre apunta al propio equipo.
- **¿Qué IP usa tu backend en producción?**
  - Una **IP pública** (si está en internet) o una **IP privada** si está dentro de una red privada en la nube (como AWS VPC). Generalmente se accede mediante un nombre de dominio.
- **¿Por qué no puedes acceder a `localhost` de otro computador?**
  - Porque `localhost` es solo “tú mismo”. En cada computadora, `localhost` apunta a esa computadora. Si intentas desde otro PC, estarías pidiendo “conéctate a ti mismo” en ese otro PC, no al tuyo.
- **¿Qué pasa cuando haces `ping 127.0.0.1` o levantas un servidor en `127.0.0.1`?**
  - Te estás comunicando con tu propio equipo. Es un “bucle” interno. Es útil para probar sin afectar la red.
- **¿Qué IP usa una base de datos en la nube?**
  - Por lo general, tiene **dos** direcciones: una **IP privada** (dentro de la nube) que usan tus servicios internos, y a veces una **IP pública** (con seguridad estricta) si necesitas conectarte desde fuera.

---

### **4. Caso práctico real (debugging)**

**Escenario:** “Tu frontend funciona, pero no logra conectarse al backend.”

- **¿Podría ser problema de IP? ¿por qué?**
  - Sí, es de las causas más comunes. El frontend está intentando llamar a una IP o dominio que no es el correcto, o que no es accesible desde el navegador.

- **¿Qué revisarías primero?**
  1. Abrir las herramientas de desarrollador (F12) → pestaña **Network** y ver qué URL está intentando llamar el frontend.
  2. Verificar si esa URL es correcta (por ejemplo, en local debe ser `http://localhost:3000` pero en producción debe ser `https://api.misitio.com`).
  3. Probar manualmente desde el navegador o con `curl` si esa URL responde.

- **¿Qué errores típicos ocurren?**
  - **IP mal configurada**: el frontend apunta a `localhost` pero el backend está en otra máquina.
  - **Puerto incorrecto**: el backend escucha en `8080` pero el frontend llama al `3000`.
  - **Firewall o reglas de seguridad**: el puerto está bloqueado en el servidor.
  - **CORS**: aunque la IP esté bien, el backend rechaza la petición por origen no permitido.

---

### **5. DHCP y DNS (lo mínimo que TODO developer debe saber)**

- **¿Qué hace DHCP?**
  - Asigna automáticamente direcciones IP privadas a los dispositivos que se conectan a una red. Así no tienes que configurar cada IP a mano.

- **¿Qué hace DNS?**
  - Traduce nombres de dominio (como `google.com`) a direcciones IP. Los humanos recordamos nombres, las máquinas necesitan IPs.

- **¿Qué pasa cuando escribes `google.com` en el navegador? (flujo simple)**
  1. Tu computador pregunta al **servidor DNS** configurado (generalmente el router) si sabe la IP de `google.com`.
  2. El DNS resuelve la consulta y devuelve una IP (ej. `142.250.185.46`).
  3. Tu navegador establece una conexión con esa IP (mediante el puerto 443 para HTTPS).
  4. Se descarga el contenido y se muestra la página.

---

### **6. Analogía obligatoria**

Usando la analogía del barrio:

| Concepto | Analogía |
|----------|----------|
| **IP** | La dirección de una casa (ej. Calle Siempreviva 742). |
| **DNS** | Una agenda de contactos donde buscas a “Casa de Juan” y encuentras la dirección real. |
| **DHCP** | El urbanizador que asigna las direcciones a cada casa nueva automáticamente cuando se construye el barrio. |

---

### **BONUS (Nivel Bootcamp Pro)**

- **¿Qué es NAT y por qué es importante?**
  - NAT (Network Address Translation) es un mecanismo que permite que múltiples dispositivos en una red privada compartan una misma IP pública para salir a internet. Tu router hace esto: asigna IPs privadas a tus dispositivos, pero al salir a internet usa su IP pública. Es importante porque **ahorra direcciones IPv4** y añade una capa de seguridad natural.

- **¿Cómo funciona una IP en Docker o contenedores?**
  - Cada contenedor Docker tiene su propia **IP privada** dentro de una red virtual creada por Docker. Los contenedores pueden comunicarse entre sí mediante esas IPs. Cuando expones un puerto (`-p 8080:80`), Docker hace un “redireccionamiento” desde tu máquina host hacia la IP del contenedor.

- **¿Qué pasa con las IP en AWS (EC2)?**
  - Una instancia EC2 suele tener:
    - **IP privada** (dentro de la VPC, fija mientras la instancia existe).
    - **IP pública** (asignada automáticamente, puede cambiar si se reinicia la instancia a menos que uses una **Elastic IP**).
    - Elastic IP es una IP pública estática que puedes asignar y reasignar entre instancias para mantener una dirección fija.



¿Necesitas que profundice en algún punto o que te ayude con las diapositivas?
