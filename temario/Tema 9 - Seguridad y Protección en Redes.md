# Tema 9: Seguridad y Protección en Redes de Comunicaciones

> Este tema cubre los fundamentos y mecanismos de defensa que permiten proteger las redes de comunicaciones frente a accesos no autorizados, ataques y pérdida de datos. Se estructura en seis grandes bloques: conceptos de seguridad en redes, seguridad perimetral, sistemas de cortafuegos, acceso remoto seguro, redes privadas virtuales (VPN) y seguridad en el puesto de trabajo. En la oposición del Bloque IV este tema tiene un peso directo en el área de Sistemas y Comunicaciones; el banco de preguntas refleja especial atención al acceso remoto (RDP, VNC, SSH), las copias de seguridad, los firewalls de siguiente generación y la arquitectura DMZ. El estudiante debe dominar los protocolos concretos, sus puertos, sus limitaciones de seguridad y los escenarios de aplicación típicos en una Administración Pública.

---

## 1. Seguridad y protección en redes de comunicaciones

### 1.1 La seguridad en las redes de información

La **seguridad en redes** es el conjunto de medidas técnicas, organizativas y legales orientadas a proteger la **confidencialidad**, la **integridad** y la **disponibilidad** (triada CIA) de los datos que circulan por una red. Estos tres pilares se definen como:

- **Confidencialidad**: solo los usuarios autorizados pueden acceder a la información.
- **Integridad**: los datos no pueden ser alterados sin autorización durante la transmisión o el almacenamiento.
- **Disponibilidad**: los sistemas y servicios están accesibles cuando los usuarios legítimos los necesitan.

### 1.2 Ataques a la seguridad de la red

Los ataques a la seguridad explotan debilidades del sistema o del comportamiento humano. Sus impactos incluyen pérdida de datos, interrupción del servicio y robo de información confidencial.

Ejemplos habituales clasificados por naturaleza:

| Categoría | Ejemplos |
|---|---|
| Intercepción pasiva | Eavesdropping, packet sniffing, snooping |
| Modificación | Data diddling, man-in-the-middle (MITM) |
| Suplantación | Spoofing (IP, ARP, DNS) |
| Denegación de servicio | DoS, DDoS, SYN Flood, flooding |
| Acceso no autorizado | Fuerza bruta, obtención de contraseñas |
| Malware | Virus, troyanos, ransomware, bombas lógicas |
| Ingeniería social | Phishing, vishing, pretexting, smishing |
| Ataques web | SQL injection, XSS, CSRF, SSRF |
| Desfiguración | Defacement (cambio de apariencia de un sitio web) |

**SYN Flood**: ataque de denegación de servicio en el que el atacante envía miles de paquetes SYN sin completar el tercer paso del 3-Way Handshake TCP. El servidor agota su tabla de conexiones semiestablecidas y deja de admitir conexiones legítimas. Los datos almacenados no se ven comprometidos; el objetivo es la disponibilidad.

### 1.3 Herramientas de seguridad

| Herramienta | Función |
|---|---|
| **Autenticación** | Verificar la identidad del usuario (contraseñas, 2FA, biometría) |
| **Autorización** | Controlar el acceso a recursos según el rol (RBAC) |
| **Cifrado** | Codificar datos para proteger su confidencialidad (SSL/TLS, AES) |
| **Filtros de paquetes** | Controlar el tráfico mediante reglas por IP/puerto |
| **Firewalls** | Barreras de protección que permiten o bloquean tráfico |
| **VLAN** | Segmentar la red lógicamente para mejorar la seguridad interna |
| **IDS/IPS** | Detectar y responder a intentos de intrusión |
| **Auditoría** | Revisión sistemática de eventos para identificar vulnerabilidades |

### 1.4 Vulnerabilidades y amenazas

- **Vulnerabilidad**: debilidad en el sistema susceptible de ser explotada (software desactualizado, mala configuración, errores de código).
- **Amenaza**: agente o circunstancia con potencial para explotar una vulnerabilidad (hacker, malware, fallo eléctrico).
- **Riesgo**: combinación de la probabilidad de que una amenaza explote una vulnerabilidad y el impacto resultante.

### 1.5 Análisis de riesgos

No puedes proteger todo con el mismo nivel de inversión — los recursos son limitados. El análisis de riesgos sirve para saber dónde poner el foco: qué activos son más críticos, qué amenazas son más probables y qué impacto tendría cada incidente. Existen dos enfoques complementarios:

- **Enfoque cualitativo**: evalúa los riesgos en términos descriptivos (alto, medio, bajo) según la probabilidad e impacto percibidos. Es más rápido de realizar y fácil de comunicar a la dirección, pero menos preciso.
- **Enfoque cuantitativo**: asigna valores numéricos a la probabilidad e impacto para calcular un nivel de riesgo objetivo. Permite priorizar inversiones en euros y justificar el gasto en seguridad, aunque requiere más datos y esfuerzo.

### 1.6 Matriz de control

Herramienta que identifica y clasifica los controles de seguridad aplicables a cada riesgo identificado. Facilita la priorización de medidas de protección.

Ejemplo de matriz de control:

| Ítem | Activo | Amenaza | Consecuencia | Prob. Ocurr. | Impacto | Capac. Reacción | Prob. Impacto |
|---|---|---|---|---|---|---|---|
| 1 | Servidor DHCP | Daño | Caída de la Red | 0,3 | 10 | 2 | 3 |
| 1 | Servidor DHCP | Desconfiguración | Caída de la Red | 0,7 | 10 | 4 | 7 |

Esta tabla permite identificar en qué amenazas debe invertirse más en controles, priorizando aquellas con mayor probabilidad de impacto real.

### 1.7 Requisitos del plan de seguridad

Un plan de seguridad no es un documento que se escribe una vez y se archiva: debe ser un proceso vivo que evolucione con la organización. Para que sea eficaz, debe abordar tres dimensiones:

- **Tiempo, gente y recursos**: la seguridad necesita dedicación continua. No sirve de nada un análisis de riesgos si no hay personal que lo ejecute ni presupuesto para implementar los controles. El plan debe definir quién hace qué, cuándo y con qué medios.
- **Topología de red y servicios**: el plan debe reflejar la realidad de la organización — qué redes existen, qué servicios se exponen, qué tecnologías se usan y de qué otras depende. Sin este mapa es imposible identificar todos los vectores de ataque.
- **Especificación de funciones**: define roles, procesos, niveles de autorización y responsabilidades. Hay que saber quién puede acceder a qué, quién responde ante un incidente y qué procedimientos se siguen.

### 1.8 Políticas de seguridad

Las políticas de seguridad son el conjunto de normas que regulan el uso aceptable de los activos de la organización. Sin políticas claras, cada empleado interpreta la seguridad a su manera — y eso es un riesgo. Las políticas cubren cuatro ámbitos principales:

- **Equipos**: normativas para el uso seguro del hardware (quién puede usar qué equipo, cómo se gestionan los portátiles fuera de la oficina, qué pasa cuando un dispositivo se pierde o roba).
- **Software**: solo se instala software autorizado y debidamente licenciado; se mantiene actualizado mediante parches de seguridad.
- **Personal**: formación continua en buenas prácticas de seguridad. El eslabón más débil de la cadena de seguridad es, habitualmente, el humano.
- **Información**: protección de datos sensibles y confidenciales — clasificación de la información, quién puede acceder a ella y cómo debe manejarse.

---

## 2. Seguridad perimetral informática

La **seguridad perimetral** es el conjunto de mecanismos que protegen el límite entre la red interna de una organización y las redes externas no confiables (como Internet). Su objetivo es impedir que las amenazas externas penetren en la red corporativa y controlar qué tráfico entra y sale.

### 2.1 Funciones de la seguridad perimetral

Piensa en la seguridad perimetral como la valla y el guardia de seguridad de un edificio: su función es controlar quién entra y salir, vigilar que no pase nada raro y mantener el registro de accesos. Concretamente:

- **Protección del perímetro**: impedir el acceso no autorizado a la red desde el exterior.
- **Monitoreo continuo**: supervisión constante del tráfico para detectar actividades sospechosas en tiempo real.
- **Control de acceso**: asegurar que solo usuarios y dispositivos autorizados ingresen a la red; el resto queda fuera.

### 2.2 Herramientas de seguridad perimetral

No hay una sola herramienta que lo proteja todo. La seguridad perimetral real es una combinación de varias capas:

- **Cortafuegos (firewall)**: la primera línea de defensa. Bloquean o permiten el tráfico basado en reglas de seguridad.
- **Sistemas IDS/IPS**: van más allá del firewall; analizan el contenido del tráfico para detectar patrones de ataque y, en el caso del IPS, bloquearlo automáticamente.
- **Honeypots**: sistemas señuelo diseñados para atraer a atacantes y estudiar sus técnicas sin poner en riesgo la red principal. Si alguien ataca un honeypot, el equipo de seguridad aprende cómo trabaja el atacante.
- **Proxies y gateways**: actúan como intermediarios inspeccionando el contenido de las comunicaciones; útiles para filtrar contenido web y evitar fugas de información.
- **VLAN**: segmentación lógica de la red. Si un segmento es comprometido, el daño queda contenido en esa VLAN y no se propaga al resto.

### 2.3 IDS/IPS: detección y prevención de intrusiones

Los **IDS** (Intrusion Detection Systems) monitorizan el tráfico o la actividad del sistema y generan alertas cuando detectan comportamientos sospechosos, pero no bloquean el tráfico por sí solos. Los **IPS** (Intrusion Prevention Systems) añaden la capacidad de bloquear o rechazar automáticamente el tráfico malicioso.

| Característica | IDS | IPS |
|---|---|---|
| Función | Detectar y alertar | Detectar, alertar y bloquear |
| Posición en la red | Fuera del flujo de tráfico (pasivo) | En línea (inline), dentro del flujo |
| Acción ante amenaza | Solo genera alerta | Bloquea el tráfico malicioso |
| Riesgo de falso positivo | Baja (no interrumpe servicio) | Alta (puede bloquear tráfico legítimo) |

Clasificación adicional por ámbito de monitorización:

| Tipo | Descripción |
|---|---|
| **NIDS** (Network-based IDS) | Monitoriza el tráfico de red en un segmento completo |
| **HIDS** (Host-based IDS) | Monitoriza actividad en un equipo concreto (logs, llamadas al sistema) |
| **NIPS** (Network-based IPS) | Inspección inline del tráfico de red |
| **HIPS** (Host-based IPS) | Bloquea actividad maliciosa en un host concreto |

**Métodos de detección**:
- **Por firmas**: compara el tráfico con patrones predefinidos de ataques conocidos. Muy preciso para ataques conocidos, incapaz de detectar ataques nuevos (zero-day).
- **Por anomalías/comportamiento**: detecta desviaciones respecto al tráfico normal. Puede detectar ataques nuevos, pero genera más falsos positivos.

**Herramientas destacadas**:
- [**Snort**](https://www.snort.org/): NIDS/NIPS de código abierto mantenido por Cisco. Opera en Windows, Linux y Unix.
- [**Suricata**](https://suricata.io/): NIDS/NIPS de código abierto con capacidad multihilo (multithreading). Compatible con las reglas de Snort.

### 2.4 SIEM (Security Information and Event Management)

Un **SIEM** centraliza la recopilación, normalización y correlación de eventos de seguridad procedentes de múltiples fuentes (firewalls, IDS, servidores, endpoints). Permite:

- Detectar ataques complejos que no son visibles analizando cada fuente por separado.
- Descartar falsos positivos mediante correlación.
- Cumplir con requisitos de auditoría y normativa (ENS, RGPD).
- Gestionar incidentes con una visión unificada.

Ejemplos: IBM QRadar, Splunk, Microsoft Sentinel, Elastic SIEM.

### 2.5 DMZ (Zona Desmilitarizada)

La **DMZ** (Demilitarized Zone) es un segmento de red intermedio entre Internet y la red interna corporativa (LAN). En ella se ubican los servidores que deben ser accesibles desde el exterior: servidores web, servidores de correo, DNS públicos, servidores de aplicaciones expuestos, etc.

La filosofía de la DMZ es el **aislamiento por compromiso**: si un servidor de la DMZ es atacado y comprometido, el atacante queda contenido en esa zona y no puede acceder directamente a la red interna.

**Servicios que se colocan habitualmente en la DMZ**:
- Servidor web (HTTP/HTTPS)
- Servidor de correo (SMTP de entrada)
- Servidor DNS público
- Servidor FTP/SFTP público
- Proxies inversos y balanceadores de carga
- Servidor VPN (punto de entrada)

**Regla de tráfico fundamental en la DMZ**:

> El tráfico con origen en la DMZ y destino la red interna debe estar **bloqueado por defecto**. Un servidor de la DMZ no necesita iniciar conexiones hacia la LAN; si lo hace, es señal de que ha sido comprometido.

```
Flujos de tráfico permitidos en arquitectura DMZ:
  Internet  -->  DMZ         : PERMITIDO (servicios publicados)
  DMZ       -->  Internet    : PERMITIDO (respuestas + DNS, actualizaciones)
  LAN       -->  DMZ         : PERMITIDO (administración, usuarios internos)
  LAN       -->  Internet    : PERMITIDO (con filtrado y proxy)
  DMZ       -->  LAN         : BLOQUEADO (regla fundamental)
  Internet  -->  LAN         : BLOQUEADO
```

**Arquitectura con un único cortafuegos (Single Firewall DMZ)**:

```
                    INTERNET
                        |
                   [FIREWALL]
                  /     |     \
           Eth0(WAN) Eth1(DMZ) Eth2(LAN)
                       |          |
                  [Servidores]  [Usuarios]
                   Web, Mail     internos
```

El firewall dispone de tres o más interfaces. Es el único punto de fallo de la red. Más económico, pero menos seguro: si el firewall es comprometido, todas las zonas quedan expuestas.

**Arquitectura con doble cortafuegos (Dual Firewall DMZ)**:

```
            INTERNET
                |
          [FIREWALL EXTERNO]
          (solo permite tráfico a DMZ)
                |
              [DMZ]
          Web / Mail / DNS
                |
          [FIREWALL INTERNO]
          (solo permite tráfico DMZ→LAN
           bajo petición explícita)
                |
              [LAN]
           Red corporativa
```

El firewall externo filtra el tráfico entre Internet y la DMZ. El firewall interno controla el acceso entre la DMZ y la LAN. Es la arquitectura más segura: si el primer firewall es comprometido, el segundo protege la LAN. Puede usar firewalls de fabricantes distintos para reducir el riesgo de vulnerabilidades comunes.

---

## 3. Sistemas de cortafuegos

### 3.1 Concepto y evolución histórica

Un **cortafuegos** (firewall) es un sistema de hardware o software que controla y filtra el tráfico de red entre dos o más zonas de seguridad, aplicando un conjunto de reglas que determinan qué tráfico se permite y cuál se bloquea.

Los primeros cortafuegos surgieron a finales de los años 80 como simples filtros de paquetes: miraban la cabecera IP y decidían si dejar pasar o no. Con el tiempo, los atacantes se volvieron más sofisticados y los firewalls tuvieron que evolucionar para analizar no solo el quién y el adónde, sino el contexto y el contenido de las comunicaciones:

| Generación | Tipo | Capacidades |
|---|---|---|
| 1ª (años 80) | Filtrado de paquetes | IP, puerto, protocolo |
| 2ª (años 90) | Inspección de estado (stateful) | Contexto de conexión TCP |
| 3ª (años 90-00) | Proxy / capa de aplicación | Contenido HTTP, FTP, DNS |
| 4ª (2000s) | UTM / NGFW | DPI, antivirus, filtrado URL, IPS integrado |

### 3.2 Tipos de cortafuegos

| Tipo | Capa OSI | Descripción |
|---|---|---|
| **Filtrado de paquetes** (packet filtering) | L3-L4 | Analiza cabeceras IP/TCP/UDP: IP origen/destino, puertos, protocolo. Sin estado (stateless). Implementado mediante ACLs en routers. |
| **Inspección de estado** (stateful inspection) | L3-L4 | Mantiene una tabla de conexiones activas y verifica que los paquetes pertenecen a una sesión legítima. |
| **Proxy / pasarela de aplicación** (application gateway) | L7 | Actúa como intermediario entre cliente y servidor. Inspecciona el contenido a nivel de aplicación (HTTP, FTP, DNS). Rompe la conexión extremo a extremo. |
| **NGFW** (Next Generation Firewall) | L3-L7 | Integra inspección de estado + DPI + filtrado URL + antivirus + IPS + control de aplicaciones + inspección SSL/TLS. |
| **UTM** (Unified Threat Management) | L3-L7 | Similar al NGFW pero orientado a pymes; unifica múltiples funciones de seguridad en un único dispositivo. |
| **Cortafuegos personal** | L3-L7 | Instalado en el propio equipo del usuario; protege el dispositivo individual. |
| **WAF** (Web Application Firewall) | L7 | Especializado en proteger aplicaciones web. Detecta y bloquea ataques como SQL injection, XSS, CSRF. |

**Diferencia clave entre NGFW/WAF y un router L3**:

Un router L3 solo puede filtrar por IP y puerto (capas 3-4). No puede inspeccionar el contenido cifrado HTTPS ni realizar filtrado de URLs porque estas operaciones requieren capacidades de capa 7. Un NGFW realiza inspección SSL/TLS (descifra, analiza y vuelve a cifrar el tráfico) para detectar amenazas ocultas en el tráfico cifrado.

**WAF vs Firewall tradicional**:
- Un **firewall tradicional** protege el perímetro de red (tráfico entre redes).
- Un **WAF** protege aplicaciones web específicas inspeccionando el contenido de las peticiones HTTP/HTTPS. Es la herramienta adecuada para prevenir SQL injection, XSS y otros ataques web. Herramientas: [ModSecurity](https://owasp.org/www-project-modsecurity-core-rule-set/), AWS WAF, Azure Application Gateway WAF.

### 3.3 Ventajas y limitaciones de los cortafuegos

**Ventajas**:
- Protección contra accesos no autorizados desde el exterior.
- Registro y monitorización del tráfico para detectar intentos de intrusión.
- Segmentación de la red en zonas con distintos niveles de confianza.

**Limitaciones**:
- **No protege contra amenazas internas**: un empleado malicioso dentro de la red puede actuar sin cruzar el firewall.
- **Puede ser eludido**: técnicas como el tunneling sobre puertos permitidos (HTTP/HTTPS) pueden sortear reglas básicas.
- **No detecta malware cifrado** sin inspección SSL/TLS.

### 3.4 Reglas y políticas del cortafuegos

Las reglas de un cortafuegos se evalúan en orden secuencial. Cada regla especifica:
- Interfaz de entrada/salida
- IP/red de origen y destino
- Puerto y protocolo
- Acción: ALLOW / DENY / DROP / LOG

Principio básico: **denegación implícita** (deny all by default). Todo lo que no esté expresamente permitido se bloquea.

Ejemplo con `iptables` (Linux):

```bash
# Permitir tráfico SSH entrante desde una IP específica
iptables -A INPUT -p tcp --dport 22 -s 192.168.1.100 -j ACCEPT

# Bloquear todo el tráfico entrante de la DMZ a la LAN interna
iptables -A FORWARD -i eth1 -o eth2 -j DROP

# Permitir tráfico HTTP/HTTPS de la LAN hacia Internet
iptables -A FORWARD -i eth2 -o eth0 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -i eth2 -o eth0 -p tcp --dport 443 -j ACCEPT

# Regla final: denegar todo lo demás
iptables -A INPUT -j DROP
iptables -A FORWARD -j DROP
```

### 3.5 OWASP Top 10 y vulnerabilidades web

La fundación [**OWASP**](https://owasp.org) (Open Web Application Security Project) publica periódicamente una lista de las diez vulnerabilidades web más críticas. La edición de 2021 es la referencia vigente:

| Posición | Categoría | Descripción | Herramientas de detección |
|---|---|---|---|
| A01 | **Broken Access Control** | Usuarios acceden a recursos sin autorización (IDOR) | Revisión de código, DAST |
| A02 | **Cryptographic Failures** | Uso de algoritmos débiles o datos sensibles en claro | Scanners SSL, análisis de código |
| A03 | **Injection** | SQL injection, command injection, LDAP injection | WAF, consultas parametrizadas |
| A04 | **Insecure Design** | Falta de controles de seguridad en el diseño | Threat modeling |
| A05 | **Security Misconfiguration** | Configuraciones por defecto inseguras | Herramientas de hardening |
| A06 | **Vulnerable Components** | Dependencias con CVEs conocidos | OWASP Dependency-Check |
| A07 | **Auth Failures** | Credenciales débiles, ausencia de MFA | MFA, gestión de sesiones |
| A08 | **Software Integrity Failures** | Actualizaciones sin verificación de firma | Firma de código, SBOM |
| A09 | **Logging Failures** | Ausencia de registros de auditoría | SIEM, políticas de log |
| A10 | **SSRF** | El servidor realiza peticiones HTTP a destinos controlados por el atacante | Validación de URLs, WAF |

**SQL Injection**: el atacante inserta código SQL en campos de entrada de la aplicación para manipular las consultas a la base de datos. Contramedidas: consultas parametrizadas (prepared statements), WAF, principio de mínimo privilegio en la cuenta de BD.

**XSS (Cross-Site Scripting)**: inyección de scripts maliciosos en páginas web que se ejecutan en el navegador de la víctima para robar cookies o tokens de sesión. Contramedida: escapado de salida, Content Security Policy (CSP).

**CSRF (Cross-Site Request Forgery)**: fuerza al navegador de la víctima a enviar peticiones no deseadas a una aplicación donde está autenticada. Contramedida: tokens anti-CSRF, cabecera SameSite en cookies.

**Herramienta de auditoría dinámica recomendada**: [OWASP ZAP](https://www.zaproxy.org/) (Zed Attack Proxy) — analiza aplicaciones web en tiempo de ejecución buscando inyecciones y configuraciones inseguras. Es diferente a SonarQube (análisis estático de código) y a Dependency-Check (análisis de dependencias con CVEs).

---

## 4. Acceso remoto seguro a redes

El **acceso remoto** permite a usuarios, administradores y sistemas conectarse a una red o equipo desde una ubicación diferente. En entornos corporativos y de Administración Pública es imprescindible que este acceso sea seguro.

### 4.1 Políticas de seguridad en el acceso remoto

- **Restricción de acceso**: definir quién puede acceder remotamente y a qué recursos.
- **Autenticación reforzada**: uso de métodos como la autenticación de dos factores (2FA/MFA).
- **Principio de mínimo privilegio**: el usuario accede solo a los recursos que necesita.
- **Cifrado del canal**: toda comunicación remota debe viajar cifrada.
- **Registro y auditoría**: todas las sesiones remotas deben quedar registradas.

**MFA (Multi-Factor Authentication)**: combina dos o más factores de verificación:
- Algo que sabes: contraseña, PIN.
- Algo que tienes: token OTP (Google Authenticator), tarjeta inteligente, SMS.
- Algo que eres: huella dactilar, reconocimiento facial.

### 4.2 Protocolos y herramientas de acceso remoto

| Protocolo / Herramienta | Puerto | Cifrado | Modo | Notas |
|---|---|---|---|---|
| **Telnet** | TCP 23 | No | Línea de comandos | Obsoleto. Transmite en texto claro. No usar. |
| **SSH** | TCP 22 | Sí (AES, ChaCha20) | Línea de comandos | Estándar de facto. Soporta claves públicas. |
| **RDP** | TCP 3389 | Sí (TLS) | Escritorio gráfico | Protocolo propietario Microsoft. |
| **VNC** | TCP 5900+ | No (nativo) | Escritorio gráfico | Añadir cifrado tunelizando sobre SSH. |
| **FTP** | TCP 20/21 | No | Transferencia de ficheros | Usar SFTP (sobre SSH) o FTPS en su lugar. |

#### SSH (Secure Shell)

**SSH** ([RFC 4251](https://www.rfc-editor.org/rfc/rfc4251)) es el protocolo estándar para el acceso remoto seguro por línea de comandos. Características:

- Cifrado extremo a extremo de toda la comunicación (autenticación + datos).
- Autenticación por contraseña o por **par de claves público/privada** (más seguro).
- Funcionalidades adicionales: reenvío de puertos, túneles, transferencia de ficheros (SCP/SFTP).
- Implementación de referencia: [OpenSSH](https://www.openssh.com/) (preinstalado en la mayoría de distribuciones Linux).

```bash
# Generar par de claves RSA (4096 bits)
ssh-keygen -t rsa -b 4096 -C "usuario@organizacion.es"

# Copiar la clave pública al servidor remoto
ssh-copy-id usuario@servidor.ejemplo.es

# Conectar al servidor remoto
ssh usuario@servidor.ejemplo.es

# Conectar a un puerto no estándar
ssh -p 2222 usuario@servidor.ejemplo.es

# Túnel SSH (port forwarding) para tunelizar VNC sobre SSH
ssh -L 5901:localhost:5901 usuario@servidor.ejemplo.es
```

Telnet es el predecesor de SSH, pero **transmite todo en texto claro** (incluidas las contraseñas). Su uso en producción es una mala práctica de seguridad.

#### RDP (Remote Desktop Protocol)

**RDP** es el protocolo propietario de Microsoft para acceso remoto con escritorio gráfico. Opera en el **puerto TCP 3389** por defecto.

Características avanzadas de RDP:

- **Redirección de audio**: el sonido del equipo remoto se reproduce en los altavoces locales, y el micrófono local puede usarse en la sesión remota.
- **Redirección de vídeo/cámara**: la cámara web local está disponible en la sesión remota.
- **Redirección de portapapeles**: permite copiar archivos del equipo remoto al local y viceversa, siempre que los permisos estén habilitados.
- **Redirección de unidades de disco**: las unidades locales aparecen como unidades de red en la sesión remota.
- **Redirección de impresoras, puertos COM, smart cards y dispositivos USB**.
- **Profundidad de color configurable**: 8, 15, 16, 24 o 32 bits, adaptable al ancho de banda disponible. Versiones modernas usan compresión H.264/AVC con adaptación automática.
- **NLA (Network Level Authentication)**: requiere autenticación antes de establecer la sesión completa, reduciendo la superficie de ataque.

Consideraciones de seguridad: el puerto 3389 expuesto directamente a Internet es un vector de ataque muy explotado (fuerza bruta, BlueKeep). Lo correcto es protegerlo detrás de una VPN o un bastion host.

#### VNC (Virtual Network Computing)

**VNC** utiliza el protocolo **RFB** (Remote Frame Buffer) para transmitir la interfaz gráfica de un equipo remoto y recibir eventos de teclado y ratón. Es multiplataforma (Windows, Linux, macOS) y de código abierto.

Implementaciones libres: [TigerVNC](https://tigervnc.org/), TightVNC, LibVNCServer, x11vnc.

VNC no cifra el tráfico de forma nativa; se recomienda tunelizarlo sobre SSH para añadir cifrado y autenticación.

```bash
# Arrancar un servidor VNC en Linux (TigerVNC)
vncserver :1 -geometry 1920x1080 -depth 24

# Conectar de forma segura: crear túnel SSH primero
ssh -L 5901:localhost:5901 usuario@servidor.ejemplo.es
# Luego conectar el cliente VNC a localhost:5901
```

**Diferencia clave**: SSH proporciona acceso por **línea de comandos** (terminal); VNC proporciona acceso **gráfico** al escritorio completo.

#### Herramientas de control remoto corporativo

Para soporte técnico y administración remota en entornos corporativos se utilizan herramientas como **TeamViewer** y **AnyDesk**, que facilitan el acceso sin necesidad de configurar VPN o abrir puertos. Permiten transferencia de archivos, acceso desatendido y compatibilidad multiplataforma.

> Nota ENS: En entornos de Administración Pública con requisitos del ENS, se prefieren soluciones corporativas propias (VPN + RDP o SSH) sobre herramientas de terceros como TeamViewer, por razones de soberanía y control de los datos.

**Herramientas que NO son de control remoto**: WinRAR/7-Zip (compresión de archivos), Zoom/Meet (videoconferencia — comparten pantalla pero no ofrecen control real del equipo), NTP (sincronización de relojes), SMTP (correo), NFS (sistema de ficheros en red), SNMP (monitorización de red).

### 4.3 Bastion host y jump server

Si necesitas administrar remotamente servidores internos, ¿los expones todos directamente a Internet? Obviamente no. Un **bastion host** (o jump server) es la solución: un servidor específicamente endurecido y expuesto a Internet, que actúa como único punto de entrada para la administración remota de la red interna. Los administradores se conectan primero al bastion host (normalmente por SSH) y desde ahí saltan al resto de servidores internos. Así reduces la superficie de ataque al concentrar toda la exposición en un único servidor que puede ser auditado, monitorizado y endurecido de forma exhaustiva.

---

## 5. Redes Privadas Virtuales (VPN)

Una **VPN** (Virtual Private Network) resuelve un problema muy concreto: ¿cómo conectar de forma segura dos redes (o un usuario y una red) a través de Internet, que es una red pública y no confiable? La respuesta es crear un canal de comunicación cifrado y autenticado sobre esa red pública, de modo que los equipos comunicantes se comporten como si estuviesen en la misma red privada. Todo el tráfico viaja cifrado por Internet, y cualquiera que lo intercepte solo ve datos ininteligibles.

### 5.1 Requerimientos básicos de una VPN

- **Cifrado**: proteger la confidencialidad de los datos transmitidos.
- **Autenticación**: verificar la identidad de los usuarios y dispositivos conectados.
- **Integridad**: garantizar que los datos no han sido modificados en tránsito.

### 5.2 Tipos de VPN por arquitectura de conexión

| Tipo | Descripción | Caso de uso |
|---|---|---|
| **Acceso remoto** (Remote Access VPN) | Un usuario individual se conecta a la red corporativa desde cualquier lugar | Teletrabajo, empleados en movilidad |
| **Site-to-Site VPN** | Conecta dos redes completas (routers o firewalls en cada extremo) | Interconexión de oficinas remotas |
| **Router a router** | Variante de site-to-site entre dispositivos de routing | Sucursales corporativas |
| **Firewall a firewall** | Mejora la seguridad entre redes de distintas organizaciones | Conexiones B2B |
| **VPN Híbrida** | Combina VPN tradicional con conexiones MPLS | Flexibilidad para distintas necesidades |

### 5.3 Protocolos VPN

| Protocolo | Capa | Cifrado | Seguridad | Notas |
|---|---|---|---|---|
| **PPTP** | L2 | MPPE (débil) | Baja | Obsoleto. Vulnerable. Compatible legacy. Puerto TCP 1723. |
| **L2TP/IPSec** | L2+L3 | IPSec (AES) | Alta | L2TP aporta el túnel, IPSec el cifrado. Más lento (doble encapsulación). UDP 1701 + 500/4500. |
| **IPSec** (solo) | L3 | AES, 3DES | Alta | Estándar ampliamente soportado. Configuración más compleja. |
| **OpenVPN** | L3/L2 | OpenSSL/TLS | Muy alta | Código abierto. Puerto configurable (UDP 1194 por defecto). Traversal NAT bueno. |
| **WireGuard** | L3 | ChaCha20/Poly1305 | Muy alta | Moderno, rápido, base de código reducida. Puerto UDP 51820. |
| **SSL VPN** | L7 | TLS (443) | Alta | Funciona sobre HTTPS (puerto 443). Ideal para acceso desde redes restrictivas. Puede funcionar vía navegador o con cliente ligero; no requiere configuración especial de red en el cliente. |
| **MPLS** | L2.5 | Depende del proveedor | Media | Técnicamente no es una VPN de cifrado sino una red privada basada en etiquetado de paquetes. No cifra por defecto. Rendimiento consistente y predecible. Costoso y dependiente del operador. |

### 5.4 IPSec en profundidad

**IPSec** es un conjunto de protocolos para asegurar comunicaciones IP mediante cifrado y autenticación a nivel de capa de red (capa 3). A diferencia de TLS, que protege a nivel de aplicación, IPSec asegura todo el tráfico IP independientemente del protocolo de aplicación que se use encima. Es el estándar utilizado para VPNs site-to-site en entornos corporativos. Está compuesto por:

#### Modos de operación

| Modo | Qué protege | Uso típico |
|---|---|---|
| **Modo Transporte** | Solo la carga útil (payload) del paquete IP. La cabecera IP original queda visible. | Comunicación entre dos hosts que son a la vez los extremos del túnel |
| **Modo Túnel** | El paquete IP completo (cabecera + datos) se encapsula dentro de un nuevo paquete IP. | VPN entre routers o firewalls (gateways); el destino final queda cifrado |

#### Protocolos de seguridad

| Protocolo | Función | Cifrado |
|---|---|---|
| **AH** (Authentication Header) | Proporciona autenticación e integridad del paquete IP completo. | No cifra (solo autentica) |
| **ESP** (Encapsulating Security Payload) | Proporciona cifrado de la carga útil, más autenticación e integridad. | Sí (AES, 3DES) |
| **IKE** (Internet Key Exchange) | Gestiona la negociación de parámetros y el intercambio de claves criptográficas. | N/A (protocolo de señalización) |

En la práctica, **ESP es el protocolo más usado** porque proporciona tanto cifrado como autenticación. AH sin ESP no cifra los datos.

**IKE** opera en dos fases:
- **Fase 1**: establece un canal seguro (ISAKMP SA) para proteger las negociaciones.
- **Fase 2**: negocia los parámetros de la SA de IPSec (algoritmos, claves, duración).

```
Diagrama IPSec modo túnel (Site-to-Site):

[Oficina A]----[FW/Router A]=====[INTERNET]=====[FW/Router B]----[Oficina B]
                    |                                  |
                 Endpoint                           Endpoint
                 IPSec                              IPSec
                    |<======= Túnel IPSec ESP =======>|
                         (paquetes originales cifrados
                          dentro de nuevos paquetes IP)
```

Referencias oficiales: [AWS — ¿Qué es IPsec?](https://aws.amazon.com/what-is/ipsec/), [Cloudflare — IPSec VPN](https://www.cloudflare.com/learning/network-layer/what-is-ipsec/), [CCN — Recomendaciones VPN IPSec](https://www.ccn.cni.es/en/docman/documentos-publicos/boletines-pytec/108-pildorapytec-19sep2019-vpn/file).

---

## 6. Seguridad en el puesto de trabajo

El perímetro de red protege la frontera entre la organización e Internet, pero ¿qué pasa con las amenazas que ya están dentro? Un empleado que conecta un USB infectado, un portátil robado con datos sin cifrar o credenciales comprometidas son amenazas que el firewall no puede detener. La seguridad en el puesto de usuario (endpoint security) protege los dispositivos finales — ordenadores de sobremesa, portátiles, móviles — frente a este tipo de amenazas que no son capturadas por la seguridad perimetral.

### 6.1 Amenazas en el puesto de trabajo

- **Malware**: virus, troyanos, ransomware, spyware que afectan al dispositivo.
- **Filtración de contenido confidencial**: exfiltración de datos sensibles por empleados o por malware.
- **Credenciales comprometidas**: accesos no autorizados por contraseñas robadas o débiles.
- **Seguridad en movilidad**: redes Wi-Fi inseguras, dispositivos sin cifrado de disco.
- **Uso inadecuado de herramientas criptográficas**: técnicas de cifrado débiles o mal implementadas.

### 6.2 Cifrado de disco

El **cifrado de disco completo** (Full Disk Encryption, FDE) protege los datos de un dispositivo ante robo o extravío físico. Aunque un atacante extraiga el disco y lo conecte a otro equipo, los datos aparecen como información aleatoria e ilegible sin la clave de descifrado.

Herramientas:

| Herramienta | Plataforma | Características |
|---|---|---|
| **BitLocker** | Windows 10/11 Pro, Enterprise | Integrado en Windows. Usa AES-128/256 bits. Protege la clave mediante chip TPM o contraseña/PIN. Solución recomendada para portátiles corporativos con Windows. |
| **VeraCrypt** | Windows, Linux, macOS | Código abierto. Sucesor de TrueCrypt. Soporta contenedores cifrados y cifrado de disco completo. |
| **LUKS** | Linux | Estándar de cifrado de disco en Linux. Integrado en el kernel. |

Un método de login robusto en Windows **no protege** ante la extracción física del disco: puede eludirse arrancando desde un medio externo o montando el disco en otro sistema operativo. BitLocker sí protege en este escenario.

### 6.3 Herramientas antimalware

Programas que identifican y eliminan software malicioso (virus, troyanos, spyware, ransomware). Las categorías principales son:

- **Antivirus tradicional**: detección por firmas de malware conocido.
- **EDR (Endpoint Detection and Response)**: detección basada en comportamiento, con capacidad de respuesta automatizada y análisis forense. Va más allá del antivirus clásico.
- **Antispam**: filtrado de correo no deseado y phishing.

### 6.4 DLP (Data Loss Prevention)

Imagina que un empleado, malintencionado o simplemente despistado, intenta enviar un fichero con datos de ciudadanos a su correo personal. Las herramientas **DLP** (también denominadas de control de contenidos confidenciales) están diseñadas exactamente para detectar y bloquear ese tipo de fuga de información sensible. Actúan monitorizando y bloqueando:

- Transferencias de archivos a dispositivos USB no autorizados.
- Envío de datos por correo electrónico o plataformas de nube no autorizadas.
- Impresión o copia de documentos clasificados.
- Envío de correos electrónicos salientes con información sensible sin cifrar.

### 6.5 Control de dispositivos USB

Un USB puede ser un vector de ataque en los dos sentidos: sirve para sacar datos hacia fuera (exfiltración) o para meter malware hacia dentro (el famoso "USB abandonado en el aparcamiento que alguien recoge y conecta"). Las soluciones DLP y las políticas de grupo (GPO en Windows) permiten bloquear o controlar qué tipos de dispositivos USB pueden conectarse a los equipos corporativos, y registrar cuándo y quién lo hace.

### 6.6 Autenticación y certificación digital

- **Certificados digitales**: vinculan una clave pública a la identidad de una persona u organización, emitidos por una Autoridad de Certificación (CA). Base de la identidad digital en la Administración Pública (DNIe, certificados de empleado público).
- **Firma electrónica**: garantiza la autenticidad e integridad de documentos electrónicos.
- **Autenticación biométrica**: huella dactilar, reconocimiento facial, iris.
- **2FA/MFA**: segundo factor obligatorio para el acceso a sistemas críticos.

Un error SSL/TLS típico en aplicaciones Java (`ValidatorException: No trusted certificate found`) indica que la aplicación intenta conectarse a una URL **HTTPS** y la CA que firmó el certificado del servidor no está en el truststore de la JVM. No indica que el certificado esté revocado ni que falte una clave privada.

### 6.7 Gestión de acceso e identidades (IAM)

¿Quién puede acceder a qué, y qué puede hacer una vez dentro? Eso es exactamente lo que gestiona un sistema **IAM** (Identity and Access Management). Es la diferencia entre tener un empleado de soporte que puede reiniciar un servidor y uno que puede borrarlo todo. Los conceptos clave:

- **RBAC** (Role-Based Access Control): los permisos se asignan a roles (no directamente a personas), y los usuarios se asignan a roles. Cambiar los permisos de un rol afecta a todos los usuarios de ese rol de golpe.
- **Principio de mínimo privilegio**: cada usuario tiene solo los permisos estrictamente necesarios para hacer su trabajo. Si una cuenta es comprometida, el daño queda limitado a lo que esa cuenta podía hacer.
- **Directorio activo (Active Directory)**: solución Microsoft para gestión centralizada de identidades en entornos Windows corporativos. Permite gestionar usuarios, grupos, políticas (GPO) y autenticación desde un único punto.

### 6.8 Copias de seguridad y recuperación

Si un ataque de ransomware cifra todos los datos de la organización, la única salida posible (sin pagar el rescate) es restaurar desde una copia de seguridad limpia. Las **copias de seguridad** (backups) son el último recurso y, al mismo tiempo, la medida más eficaz para garantizar la disponibilidad y la capacidad de recuperación ante cualquier tipo de incidente: ransomware, borrado accidental, fallo de hardware o desastre físico.

#### Tipos de copia de seguridad

| Tipo | Qué copia | Espacio | Tiempo de backup | Tiempo de restauración |
|---|---|---|---|---|
| **Completa** (Full) | Todos los datos, siempre | Alto | Largo | Corto (una sola copia) |
| **Incremental** | Solo cambios desde el último backup (completo o incremental) | Muy bajo | Corto | Largo (necesita full + todos los incrementales) |
| **Diferencial** | Solo cambios desde el último backup **completo** | Medio | Medio | Medio (solo necesita full + último diferencial) |
| **Espejo** (Mirror) | Copia exacta en tiempo real | Igual al origen | Continuo | Inmediato |

**Deduplicación**: técnica que identifica y elimina bloques de datos duplicados entre copias, reduciendo el espacio de almacenamiento requerido. No aumenta la tolerancia a fallos ni el espacio; lo reduce.

#### Modelo GFS (Grandfather-Father-Son / Abuelo-Padre-Hijo)

Esquema de rotación de copias con tres niveles:

| Nivel | Frecuencia | Retención | Propósito |
|---|---|---|---|
| **Son** (Hijo) | Diaria | 1 semana | Recuperación rápida ante fallos recientes |
| **Father** (Padre) | Semanal (completa) | 1 mes | Recuperación ante errores descubiertos días después |
| **Grandfather** (Abuelo) | Mensual (completa) | 1-7 años | Archivo histórico, cumplimiento legal (RGPD, ENS, auditorías) |

El **Grandfather** son copias completas **mensuales** con retención **a largo plazo**, usadas para archivo histórico o cumplimiento legal. No son copias diarias ni snapshots horarios.

#### Regla 3-2-1

- **3** copias de los datos (1 original + 2 copias).
- **2** soportes o medios diferentes (por ejemplo, disco local + cinta o nube).
- **1** copia fuera de las instalaciones (offsite o nube).

La versión extendida **3-2-1-1-0** añade: 1 copia offline o inmutable + 0 errores verificados en la restauración.

#### RTO y RPO

| Métrica | Definición | Ejemplo |
|---|---|---|
| **RPO** (Recovery Point Objective) | Máxima pérdida de datos tolerable medida en tiempo | RPO de 2 horas: se pueden perder hasta 2 horas de datos |
| **RTO** (Recovery Time Objective) | Tiempo máximo para restaurar el servicio tras un incidente | RTO de 4 horas: el sistema debe estar operativo en 4 horas |

Un **RPO bajo** requiere backups frecuentes (o replicación continua). Un **RTO bajo** requiere procedimientos de restauración rápidos y sistemas de alta disponibilidad.

#### RAID 10

**RAID 10** combina **RAID 1** (mirroring) y **RAID 0** (striping):
- Con 4 discos de 100 GB: **capacidad útil = 200 GB** (50% del total), y **tolerancia a fallos: sí**, porque cada pareja tiene su espejo.
- Si un disco falla, su pareja espejo conserva todos los datos. No hay pérdida de información.

No confundir con RAID 5 (paridad distribuida, capacidad = N-1 discos) ni con RAID 0 (striping sin redundancia, sin tolerancia a fallos).

### 6.9 Herramientas antifraude

Software orientado a detectar y prevenir fraudes financieros y suplantación de identidad, mediante:
- Detección de transacciones sospechosas.
- Monitorización de comportamiento inusual.
- Análisis de datos en tiempo real.

---

## Resumen para el examen

Los siguientes puntos reflejan lo que más aparece en el banco de preguntas de este tema:

1. **VNC** permite control remoto en **modo gráfico** usando el protocolo RFB. SSH permite control remoto en modo **línea de comandos**. NTP es sincronización de relojes, no control remoto.
2. **TeamViewer y AnyDesk** son herramientas de control remoto corporativo. WinRAR/7-Zip son compresores, Zoom/Meet son videoconferencia (no control remoto real).
3. **RDP** opera en el puerto **TCP 3389**. Permite redirección de audio, cámara, portapapeles y unidades de disco. La profundidad de color es **configurable** (no fija), adaptable al ancho de banda.
4. En una conexión **RDP**, es posible copiar archivos del equipo remoto al local si se tienen los permisos adecuados (redireccion de portapapeles y unidades).
5. **SSH** (puerto TCP 22) cifra toda la comunicación. **Telnet** (puerto TCP 23) transmite en texto claro y está obsoleto. SSH es el protocolo correcto para acceso seguro por línea de comandos en Linux.
6. La medida de seguridad fundamental en acceso remoto es implementar **MFA (autenticación multifactor)**. Desactivar el firewall es una mala práctica. El modo no supervisado sin controles adicionales es un riesgo.
7. Un **NGFW (Next Generation Firewall / Cortafuegos L7)** puede inspeccionar tráfico HTTPS, filtrar URLs y actuar como antivirus. Un router L3 solo filtra hasta capa 4 (IP/puerto). Un hub retransmite todo el tráfico sin filtrar.
8. La razón por la que un router L3 **no puede** realizar inspección de tráfico HTTPS es que solo opera hasta la capa 3 del modelo OSI y no puede analizar el contenido de capa de aplicación (capa 7).
9. En arquitectura **DMZ**, el tráfico que **siempre hay que bloquear** es el que tiene origen en la DMZ y destino la red interna. Los servidores de la DMZ no deben iniciar conexiones hacia la LAN.
10. **WAF (Web Application Firewall)** es la herramienta adecuada para prevenir ataques de **SQL injection** sobre aplicaciones web. Filtrar puertos en el router o cambiar el puerto HTTP no protege contra este tipo de ataque.
11. Un ataque **SYN Flood** genera un número masivo de intentos de 3-Way Handshake TCP incompletos. Su objetivo es la denegación de servicio (disponibilidad). Los datos almacenados **no se ven comprometidos**.
12. **BitLocker** es la herramienta adecuada para cifrar el disco de portátiles Windows ante robo o extravío. Un firewall o un keylogger no ofrecen esta protección.
13. La **deduplicación** en backups reduce el espacio de almacenamiento requerido. No aumenta la tolerancia a fallos.
14. En el modelo **GFS**, el **Grandfather** son copias completas **mensuales** con retención a largo plazo para archivo histórico y cumplimiento legal.
15. **RAID 10** con 4 discos de 100 GB ofrece **200 GB** de capacidad útil y tolerancia al fallo de un disco completo gracias al mirroring. No usa paridad.
16. **IPSec** usa los protocolos **AH** (solo autenticación) y **ESP** (cifrado + autenticación). El protocolo **IKE** gestiona el intercambio de claves. En **modo túnel** se encapsula el paquete IP completo; en **modo transporte** solo se protege la carga útil.
17. **L2TP/IPSec**: L2TP aporta el túnel (capa 2), IPSec aporta el cifrado. Es más lento por la doble encapsulación.
18. **PPTP** es el protocolo VPN más antiguo y de menor seguridad. **WireGuard** es el más moderno y eficiente.
19. El error Java `ValidatorException: No trusted certificate found` indica que la aplicación accede a una URL **HTTPS** cuya CA no está en el truststore de la JVM. No indica certificado revocado ni falta de clave privada.
20. **OWASP ZAP** es la herramienta para detectar inyecciones y configuraciones inseguras **en tiempo de ejecución** (análisis dinámico DAST). SonarQube hace análisis estático de código; Dependency-Check analiza dependencias con CVEs.

---

## Preguntas frecuentes en exámenes

El banco de 21 preguntas de este tema se concentra en los siguientes subtemas:

### Acceso remoto (6 preguntas — el bloque más examinado)

Las preguntas giran en torno a identificar qué herramienta o protocolo se usa para cada tipo de acceso remoto, las características técnicas de RDP (redirecciones, color, transferencia de archivos) y las medidas de seguridad para el acceso remoto (MFA).

**Ejemplo representativo**: "¿Cuál de las siguientes herramientas permite el control remoto en modo gráfico?" → **VNC** (usa el protocolo RFB; no confundir con NTP ni con HTTPS).

**Otra trampa frecuente**: la pregunta sobre qué software NO es de control remoto → **MyDesk** (nombre ficticio; TeamViewer y VNC sí son herramientas reales de control remoto).

### Firewalls y seguridad perimetral (4 preguntas)

Se evalúa la diferencia entre tipos de firewall por capa OSI, por qué un router L3 no puede inspeccionar HTTPS, y cuál es el tráfico a bloquear en una arquitectura DMZ.

**Ejemplo representativo**: "¿Cuál es el dispositivo más indicado para filtrado URL, antivirus e inspección HTTPS?" → **NGFW (cortafuegos L7)**.

**Concepto clave**: el tráfico **DMZ → LAN** es el que siempre se bloquea. Los flujos de LAN → DMZ y LAN → Internet son habitualmente permitidos.

### Copias de seguridad (4 preguntas)

Se examina la deduplicación, el modelo GFS (con especial atención al rol del Grandfather), RAID 10 y los conceptos RTO/RPO.

**Ejemplo representativo**: "En el modelo GFS, ¿qué representa el Grandfather?" → Copias completas **mensuales** con retención a largo plazo para archivo histórico y cumplimiento legal.

**Trampa**: el Son (Hijo) son copias diarias de recuperación rápida. Los snapshots horarios no son parte del modelo GFS.

### Seguridad web y ataques (3 preguntas)

SQL injection, SYN Flood y el uso de WAF y OWASP ZAP son los contenidos más evaluados.

**Ejemplo representativo**: "¿Qué herramienta minimiza los ataques de SQL injection en una aplicación web pública?" → **WAF**, porque analiza las peticiones HTTP buscando patrones de inyección SQL a nivel de capa 7. Un filtro IP en el router o cambiar el puerto no ayuda.

### Seguridad en el puesto (3 preguntas)

BitLocker para cifrado de disco, MFA como medida fundamental y el error de certificado HTTPS en Java.

**Ejemplo representativo**: "¿Qué herramienta protege los datos de un portátil en caso de robo?" → **BitLocker** (cifrado de disco completo). Un firewall o un keylogger no ofrecen esta protección frente a extracción física del disco.

### VPN y protocolos (1 pregunta)

Se evalúan los tipos de VPN y sus protocolos (PPTP, L2TP, IPSec, OpenVPN, WireGuard).

---

## Referencias

- [OWASP Top 10:2021](https://owasp.org/Top10/es/A00_2021_Introduction/) — Lista oficial de las vulnerabilidades web más críticas
- [OWASP ZAP](https://www.zaproxy.org/) — Herramienta de análisis dinámico de aplicaciones web
- [OpenSSH](https://www.openssh.com/) — Implementación de referencia del protocolo SSH
- [TigerVNC](https://tigervnc.org/) — Implementación libre de VNC para Linux
- [AWS — ¿Qué es IPsec?](https://aws.amazon.com/what-is/ipsec/) — Explicación técnica de IPSec
- [Cloudflare — ¿Qué es IPSec?](https://www.cloudflare.com/learning/network-layer/what-is-ipsec/) — Guía sobre IPSec y VPN
- [CCN — Recomendaciones de Seguridad para VPN IPSec](https://www.ccn.cni.es/en/docman/documentos-publicos/boletines-pytec/108-pildorapytec-19sep2019-vpn/file) — Guía oficial del CCN-CERT
- [Microsoft — BitLocker](https://learn.microsoft.com/es-es/windows/security/operating-system-security/data-protection/bitlocker/) — Documentación oficial de BitLocker
- [Microsoft — RDP](https://learn.microsoft.com/es-es/windows/win32/termserv/remote-desktop-protocol) — Documentación oficial del protocolo RDP
- [Cibersafety — DMZ y seguridad perimetral](https://cibersafety.com/dmz-seguridad-perimetral/) — Guía sobre arquitecturas DMZ
- [A2Secure — IDS, IPS, HIDS, SIEM](https://www.a2secure.com/blog/ids-ips-hids-nips-siem-que-es-esto/) — Explicación de sistemas de detección de intrusos
- [Veeam — Tipos de backup: completo, incremental, diferencial](https://www.veeam.com/blog/gfs-backup-retention-policy.html) — Explicación del modelo GFS
- [Palo Alto Networks — NGFW vs IDS vs IPS](https://www.paloaltonetworks.com/cyberpedia/firewall-vs-ids-vs-ips) — Comparativa de sistemas de seguridad perimetral
- [INCIBE — Diseño seguro de redes con DMZ](https://www.incibe.es/empresas/blog/diseno-seguro-redes-dmz) — Guía práctica del INCIBE
- [Oracle — Modos de transporte y túnel en IPsec](https://docs.oracle.com/cd/E19957-01/820-2981/ipsec-ov-13/index.html) — Documentación técnica IPSec
