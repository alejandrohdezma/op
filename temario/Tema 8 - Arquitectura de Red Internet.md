# Tema 8: Internet — Arquitectura de Red, Servicios y Protocolos

> Este tema cubre la arquitectura de Internet desde sus orígenes hasta su estado actual, los principales servicios que ofrece, y en profundidad los protocolos HTTP, HTTPS y SSL/TLS. Es uno de los temas con mayor peso práctico del Bloque IV: el banco de preguntas confirma que el examen se centra especialmente en métodos HTTP y REST, códigos de estado, SSL/TLS, arquitecturas de servidor, Apache y desarrollo web. El estudiante debe dominar los conceptos con suficiente precisión para distinguir entre opciones muy parecidas en preguntas de tipo test.

---

## 1. Internet

### 1.1 Introducción a Internet

**Internet** es una red global de redes que conecta millones de dispositivos en todo el mundo, permitiendo la comunicación y el intercambio de información mediante protocolos estandarizados como **TCP/IP**.

**Historia y evolución:**

- **1969 — ARPANET**: La red precursora de Internet, financiada por el Departamento de Defensa de EE.UU. (DARPA). Conectó inicialmente cuatro universidades: UCLA, Stanford, UC Santa Barbara y Utah. Su diseño descentralizado buscaba que la red sobreviviera a ataques o fallos parciales.
- **1983 — adopción de TCP/IP**: ARPANET migra al protocolo TCP/IP, que se convierte en el estándar de facto.
- **1991 — nacimiento de la WWW**: Tim Berners-Lee (CERN) propone y publica el primer servidor y navegador web, transformando Internet en un servicio de acceso masivo.
- **Años 90 — apertura comercial**: Internet deja de ser exclusivamente académico y militar. Aparecen los primeros ISPs comerciales.
- **2000s en adelante**: Web 2.0 (contenido generado por usuarios), redes sociales, cloud computing, dispositivos móviles e IoT.

**Estructura física:**
Internet funciona mediante una infraestructura de servidores, **cables submarinos**, satélites, centros de datos y redes de acceso, todos interconectados mediante TCP/IP.

**Organismos reguladores:**

| Organismo | Función |
|---|---|
| **ICANN** (Internet Corporation for Assigned Names and Numbers) | Gestión del espacio de nombres de dominio (DNS) y direcciones IP |
| **IANA** (Internet Assigned Numbers Authority) | Asignación de recursos de numeración IP y puertos bien conocidos |
| **IETF** (Internet Engineering Task Force) | Desarrollo de estándares técnicos de Internet (RFCs) |
| **W3C** (World Wide Web Consortium) | Estándares web: HTML, CSS, XML, SVG... |
| **IEEE** | Estándares de redes y hardware (Ethernet, Wi-Fi 802.11) |

---

### 1.2 Direcciones IP

Una **dirección IP** es un identificador único asignado a cada dispositivo conectado a una red para permitir su localización y comunicación.

#### IPv4

Utiliza **32 bits**, representados en cuatro números decimales separados por puntos (notación decimal punteada). Ejemplo: `192.168.1.1`.

Tiene un límite teórico de aproximadamente **4.300 millones de direcciones** (2³²), insuficiente para el número actual de dispositivos.

**Clases de direcciones IPv4** (clasificación clásica, hoy sustituida por CIDR):

| Clase | Rango de red | Uso | Bits de red / host |
|---|---|---|---|
| **A** | 0.0.0.0 — 127.255.255.255 | Redes muy grandes | 8 / 24 |
| **B** | 128.0.0.0 — 191.255.255.255 | Redes medianas | 16 / 16 |
| **C** | 192.0.0.0 — 223.255.255.255 | Redes pequeñas | 24 / 8 |
| **D** | 224.0.0.0 — 239.255.255.255 | Multicast (dirección de grupo) | — |
| **E** | 240.0.0.0 — 247.255.255.255 | Experimental / indefinido | — |

> El diagrama del PDF muestra visualmente estos rangos: Clase A desde 0.0.0.0 hasta 127.255.255.255, con el primer octeto como identificador de red y los tres restantes como identificador de estación; Clase B con dos octetos de red y dos de estación; Clase C con tres de red y uno de estación. Las clases D y E no se dividen en red/estación.

**Direcciones privadas** (no enrutables en Internet, definidas en RFC 1918):
- Clase A: `10.0.0.0/8`
- Clase B: `172.16.0.0/12`
- Clase C: `192.168.0.0/16`

#### IPv6

Utiliza **128 bits**, representados en ocho grupos de cuatro dígitos hexadecimales separados por dos puntos. Ejemplo: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.

Proporciona aproximadamente **340 undecillones** de direcciones (2¹²⁸), diseñado para superar definitivamente la escasez de IPv4. Incluye características nativas de autoconfiguración (SLAAC) y mejor soporte para movilidad y seguridad.

---

### 1.3 Arquitecturas de Internet

La arquitectura de Internet puede analizarse desde dos ángulos: la jerarquía de redes que la forman (ISPs por niveles) y los modelos de organización de los sistemas que la usan (cliente-servidor, P2P, cloud).

#### Jerarquía de redes — Modelo Tier

El PDF presenta un diagrama con tres niveles de redes interconectadas sobre un mapa mundial:

- **Redes Tier 1 (Backbone de Internet)**: Operadores de tránsito global que forman la columna vertebral de Internet. Se interconectan entre sí mediante acuerdos de *peering* gratuito. No necesitan pagar por tránsito. Ejemplos: AT&T, NTT, Telia.

- **Redes Tier 2**: ISPs regionales o nacionales. Se conectan con Tier 1 mediante *tránsito* (pagando) y con otras redes Tier 2 mediante *peering* (generalmente gratuito o en acuerdos bilaterales), a menudo a través de un **IXP**.

- **Redes Tier 3 (ISP)**: Proveedores de acceso a Internet para usuarios finales (hogares, empresas). Compran tránsito a redes Tier 2.

- **Usuarios de Internet**: Organizaciones, empresas y clientes residenciales que se conectan a través de los ISPs Tier 3.

**IXP (Internet Exchange Point — Punto de Intercambio de Tráfico de Internet)**: Infraestructura física donde diferentes redes se conectan para intercambiar tráfico de forma directa sin pasar por terceras redes. Beneficios: reducción de costes, mejora de la latencia y mayor resiliencia. El diagrama del PDF muestra un IXP en el nivel Tier 2, conectado mediante *peering* a varias redes.

Un segundo diagrama en el PDF muestra la conectividad desde el punto de vista del usuario final: un Router de Internet conecta mediante cable a un Access Point Wi-Fi (que da acceso a móviles vía Wi-Fi) y también mediante la red pública a servidores de SyncML, servidores de correo y proveedores de telefonía, que a su vez conectan a móviles vía GPRS/3G.

#### Modelos de organización de sistemas

| Modelo | Descripción | Ventajas | Desventajas |
|---|---|---|---|
| **Cliente-Servidor** | Un servidor central proporciona servicios; los clientes los solicitan | Centralización, control sencillo | Si el servidor falla, los clientes pierden acceso |
| **Peer-to-Peer (P2P)** | Cada dispositivo actúa como cliente y servidor | Distribución de recursos, resiliencia | Menor control de seguridad y datos |
| **Cloud Computing** | Recursos como almacenamiento y aplicaciones desde servidores remotos | Escalabilidad, reducción de costes | Dependencia de la conexión a Internet |

#### Arquitecturas de aplicaciones web — Modelos Tier

El PDF denomina estos modelos "Tier" refiriéndose a las capas de la aplicación (no a los ISPs), lo que puede generar confusión. En el contexto de desarrollo de aplicaciones:

| Arquitectura | Descripción | Uso | Ventajas | Desventajas |
|---|---|---|---|---|
| **1-Tier (Monolítica)** | Toda la aplicación (interfaz, lógica de negocio y base de datos) en un solo lugar | Apps pequeñas, simples | Simplicidad de desarrollo y despliegue | Escalabilidad limitada |
| **2-Tier (Cliente-Servidor)** | Divide en cliente (UI + lógica) y servidor de base de datos | Apps empresariales con acceso directo | Mejor distribución de carga | Cambios de lógica requieren actualizar el cliente |
| **3-Tier** | Tres capas separadas: presentación (cliente), lógica de negocio (servidor de aplicaciones) y datos (servidor BD) | Aplicaciones web, servicios backend complejos | Separación clara de responsabilidades, escalabilidad y mantenimiento | Mayor complejidad, necesita red estable entre capas |

> **Clave de examen**: En una arquitectura pura de tres capas, las **reglas de negocio** deben residir en el **servidor de aplicaciones** (capa intermedia), no en el cliente ni en el servidor de datos. Colocarlas en la BD (procedimientos almacenados) acopla la lógica al motor de base de datos y contradice el principio de separación.

---

## 2. Protocolos de Internet

### 2.1 El modelo TCP/IP y sus capas

El **modelo TCP/IP** describe cómo se organiza la comunicación en Internet en cuatro capas, cada una con su responsabilidad. Es el modelo que realmente implementan los sistemas (a diferencia del OSI, que es más teórico). Cada capa añade sus cabeceras al dato que baja desde la aplicación, y las elimina al subir en el destino (encapsulación).

| Capa | Función | Protocolos/tecnologías |
|---|---|---|
| **Aplicación** | Proporciona servicios a las aplicaciones del usuario | HTTP, FTP, SMTP, DNS, Telnet, Gopher... |
| **Transporte** | Transferencia de datos entre dispositivos (extremo a extremo) | TCP, UDP |
| **Interred (Internet)** | Direccionamiento y enrutamiento de paquetes | IP, ICMP, ARP, RARP |
| **Interfaz de red y hardware** | Interfaz con el hardware de red | Ethernet, Token-Ring, FDDI, X.25, Wi-Fi, ATM... |

### 2.2 Protocolos de transporte: TCP vs UDP

TCP y UDP son los dos protocolos de la capa de transporte, y representan dos filosofías distintas: fiabilidad vs velocidad. TCP garantiza que los datos llegan en orden y sin pérdidas (a costa de más latencia). UDP simplemente envía y no se preocupa de si llega o no (ideal cuando la velocidad importa más que la perfección).

| Característica | **TCP** | **UDP** |
|---|---|---|
| Conexión | Orientado a conexión (three-way handshake) | Sin conexión |
| Fiabilidad | Garantiza entrega y orden de paquetes | Sin garantía de entrega |
| Control de flujo | Sí | No |
| Velocidad | Más lento por overhead | Más rápido |
| Uso típico | HTTP/HTTPS, FTP, SMTP, SSH | DNS, streaming, VoIP, juegos online, HTTP/3 |

### 2.3 Puentes, routers y pasarelas (Bridges, Routers y Gateways)

**Puentes (Bridges)**: Conectan segmentos de una misma red, operando en la **capa de enlace de datos** (capa 2). Filtran el tráfico por dirección MAC para reducir la congestión.

**Routers**: Dispositivos que conectan diferentes redes y encaminan paquetes de datos entre ellas, operando en la **capa de red** (capa 3). Utilizan tablas de enrutamiento y protocolos como OSPF o BGP.

**Pasarelas (Gateways)**: Traducen entre diferentes protocolos o tipos de redes, permitiendo la comunicación entre redes heterogéneas. Pueden operar en cualquier capa del modelo.

### 2.4 Enrutamiento IP

El diagrama de enrutamiento IP del PDF ilustra cómo dos hosts (Host A en "dirección de red X" y Host B en "dirección de red Y") se comunican a través de un router. La comunicación fluye por capas:

- La capa de **aplicación** genera el mensaje completo.
- La capa **TCP** lo convierte en segmentos.
- La capa **IP** lo encapsula en datagramas IP para el enrutamiento entre redes.
- La capa de **interfaz de red** (IEEE 802.2 en la red de anillo del Host A; X.25 en la RTC hacia el Host B) lo convierte en tramas para la transmisión física.

El router opera en la capa IP: recibe el datagrama, consulta su tabla de rutas y lo reenvía por la interfaz de salida adecuada (aquí, de la red de anillo IEEE 802.2 a la red X.25/RTC).

### 2.5 Servicios de red fundamentales

| Protocolo | Puerto | Función |
|---|---|---|
| **DNS** | 53 (UDP/TCP) | Resolución de nombres de dominio a direcciones IP |
| **SMTP** | 25 / 587 | Envío de correo electrónico |
| **POP3** | 110 / 995 (TLS) | Descarga de correo electrónico desde servidor |
| **IMAP** | 143 / 993 (TLS) | Acceso sincronizado a correo en servidor |
| **FTP** | 20/21 | Transferencia de archivos |
| **SSH** | 22 | Acceso remoto seguro |
| **Telnet** | 23 | Acceso remoto sin cifrado (obsoleto) |
| **HTTP** | 80 | Navegación web |
| **HTTPS** | 443 | Navegación web segura |
| **DHCP** | 67/68 (UDP) | Asignación dinámica de IPs |

---

## 3. Protocolo HTTP

### 3.1 Introducción

**HTTP** (*HyperText Transfer Protocol*) es el protocolo de la capa de aplicación que hace funcionar la web tal como la conocemos. Cada vez que el navegador pide una página, una imagen o llama a una API, está usando HTTP. Fue diseñado por Tim Berners-Lee en el CERN y publicado como estándar por el IETF.

Características fundamentales que hay que conocer bien:
- **Sin estado (*stateless*)**: Cada solicitud es independiente de las anteriores. El servidor no recuerda quién eras en la petición anterior. Por eso existen las cookies y los tokens de sesión: para añadir estado por encima de un protocolo que no lo tiene.
- **Basado en texto**: Las peticiones y respuestas HTTP/1.x son texto plano legible por humanos (esto cambia en HTTP/2, que es binario).
- **Modelo petición-respuesta**: El cliente siempre inicia; el servidor siempre responde. Nunca al revés (salvo Server Push en HTTP/2).
- **Puerto por defecto**: 80 (HTTP) y 443 (HTTPS).

### 3.2 Estructura de una solicitud HTTP

Una solicitud HTTP está compuesta por:

1. **Método**: Define la acción a realizar (GET, POST, PUT, DELETE...).
2. **URI** (*Uniform Resource Identifier*): Identifica el recurso a obtener o modificar.
3. **Versión de HTTP**: Define la versión del protocolo utilizada.
4. **Encabezados (*headers*)**: Información adicional sobre la solicitud (tipo de contenido, codificación, credenciales...).
5. **Cuerpo de la solicitud (*body*)**: Datos enviados al servidor, presente en métodos como POST o PUT.

```
GET http://es.kioskea.net HTTP/1.0
Accept: Text/html
If-Modified-Since: Saturday, 15-January-2017 14:37:11 GMT
User-Agent: Mozilla/4.0 (compatible; MSIE 5.0; Windows 95)
```

### 3.3 Estructura de una respuesta HTTP

Una respuesta HTTP contiene:

1. **Versión de HTTP**.
2. **Código de estado**: Indica el resultado de la solicitud (200 OK, 404 Not Found...).
3. **Encabezados de respuesta**: Información adicional (tipo de contenido, longitud, fecha...).
4. **Cuerpo de la respuesta**: El recurso solicitado (HTML, JSON, imagen...).

```
HTTP/1.0 200 OK
Date: Sat, 15 Jan 2000 14:37:12 GMT
Server: Microsoft-IIS/2.0
Content-Type: text/HTML
Content-Length: 1245
Last-Modified: Fri, 14 Jan 2000 08:25:13 GMT

[cuerpo con el HTML de la página]
```

### 3.4 Métodos HTTP

Los métodos HTTP definen la semántica de la operación. Son fundamentales para el diseño de APIs REST.

| Método | Operación CRUD | Idempotente | Seguro | Descripción |
|---|---|---|---|---|
| **GET** | Read | Sí | Sí | Recupera un recurso. No modifica el estado del servidor. |
| **POST** | Create | No | No | Crea un nuevo recurso. Cada llamada puede crear uno nuevo. |
| **PUT** | Update (completo) | Sí | No | Reemplaza completamente un recurso existente. |
| **PATCH** | Update (parcial) | No* | No | Actualiza parcialmente un recurso. |
| **DELETE** | Delete | Sí | No | Elimina un recurso. |
| **HEAD** | — | Sí | Sí | Igual que GET pero solo devuelve cabeceras, sin cuerpo. |
| **OPTIONS** | — | Sí | Sí | Devuelve los métodos permitidos para un recurso (usado en CORS). |
| **TRACE** | — | Sí | Sí | Devuelve la solicitud tal como fue recibida (diagnóstico). |

> **Idempotente**: realizar la misma petición múltiples veces produce el mismo resultado. **Seguro**: no modifica el estado del servidor.

> **Clave de examen**: GET = leer, POST = crear, PUT = actualizar todo, PATCH = actualizar parte, DELETE = eliminar.

### 3.5 Códigos de estado HTTP

Los códigos de estado se agrupan en cinco familias según el primer dígito. Conocerlas es imprescindible para el examen.

| Rango | Categoría | Descripción general |
|---|---|---|
| **1xx** | Informativos | La solicitud fue recibida y se está procesando |
| **2xx** | Éxito | La solicitud fue recibida, entendida y aceptada |
| **3xx** | Redirección | Se requiere una acción adicional para completar la solicitud |
| **4xx** | Error del cliente | La solicitud contiene errores o no puede ser completada |
| **5xx** | Error del servidor | El servidor falló al procesar una solicitud válida |

**Códigos más importantes** (los que aparecen en el banco de examen):

| Código | Nombre | Cuándo se usa |
|---|---|---|
| **200** | OK | Éxito general. La solicitud se procesó correctamente. |
| **201** | Created | Se creó un nuevo recurso (típico en POST). Incluye cabecera `Location`. |
| **204** | No Content | Éxito sin cuerpo de respuesta (típico en DELETE o PUT sin retorno). |
| **301** | Moved Permanently | El recurso se ha movido permanentemente a otra URL. |
| **302** | Found | Redirección temporal. |
| **304** | Not Modified | El recurso no ha cambiado; el cliente puede usar su caché. |
| **400** | Bad Request | Solicitud malformada o inválida. |
| **401** | Unauthorized | Se requiere autenticación. |
| **403** | Forbidden | Acceso denegado aunque la autenticación sea correcta. |
| **404** | Not Found | El recurso no existe en el servidor. |
| **405** | Method Not Allowed | El método HTTP no está permitido para ese recurso. |
| **500** | Internal Server Error | Error genérico del servidor. |
| **502** | Bad Gateway | El servidor actúa como proxy y recibió una respuesta inválida. |
| **503** | Service Unavailable | El servidor no puede atender la solicitud temporalmente. |

> **Clave de examen**: Un error **500** indica un **error del servidor**, no del cliente ni de la red. Ante un 500 masivo, la acción correcta es escalar al equipo responsable del servidor. Ante un error 500, ejecutar `ping` no es la solución prioritaria porque el servidor ya está respondiendo (la conectividad existe).

### 3.6 Estructura de una URL/URI

Una **URI** (*Uniform Resource Identifier*) identifica un recurso. Una **URL** (*Uniform Resource Locator*) es un tipo de URI que además especifica cómo acceder al recurso.

```
esquema://[usuario:contraseña@]host[:puerto][/ruta][?consulta][#fragmento]
```

Ejemplo:
```
https://www.ejemplo.com:8080/productos/zapatos?color=rojo&talla=42#descripcion
│      │                 │   │                │                    │
esquema host             puerto ruta           query string         fragmento
```

**Esquemas URI más comunes (registrados en IANA)**:
- `http://` y `https://` — web ([RFC 7230](https://datatracker.ietf.org/doc/html/rfc7230))
- `ftp://` — FTP ([RFC 1738](https://www.rfc-editor.org/rfc/rfc1738))
- `mailto:` — correo
- `ssh://` — SSH ([RFC 4248](https://www.rfc-editor.org/rfc/rfc4248))
- `ldap://` — LDAP

> **Clave de examen**: `scp://` **no** es un esquema URI registrado oficialmente en IANA. SCP usa la sintaxis `usuario@servidor:/ruta`, no una URI estándar. En cambio, `ftp://ftp.rs.internic.net/domain/named.root` y `http://192.168.0.13:8080` sí son URIs válidas.

### 3.7 Versiones de HTTP

HTTP ha evolucionado en respuesta a los mismos problemas de rendimiento: cargar una web moderna requiere docenas de recursos (CSS, JS, imágenes, fuentes...) y HTTP/1.x se queda corto cuando cada recurso necesita su propia conexión o esperar en fila.

| Versión | Año | Características clave |
|---|---|---|
| **HTTP/0.9** | 1991 | Solo método GET, solo HTML, sin cabeceras |
| **HTTP/1.0** | 1996 | Cabeceras, múltiples tipos de contenido, una conexión por petición |
| **HTTP/1.1** | 1997 | Conexiones persistentes (keep-alive), pipelining, host virtual obligatorio. Estándar durante más de 15 años. |
| **HTTP/2** | 2015 | Multiplexación, compresión de cabeceras (HPACK), server push, binario en lugar de texto. Definido en [RFC 7540](https://datatracker.ietf.org/doc/html/rfc7540). |
| **HTTP/3** | 2022 | Basado en **QUIC** (sobre UDP en lugar de TCP). Resuelve el head-of-line blocking de HTTP/2. Definido en [RFC 9114](https://datatracker.ietf.org/doc/html/rfc9114). |

#### HTTP/2 en detalle

El PDF destaca las características principales de HTTP/2:

- **Mejora la velocidad y eficiencia** del transporte de datos.
- **Multiplexación**: permite enviar múltiples solicitudes y respuestas en paralelo sobre una sola conexión TCP, eliminando el problema de "head-of-line blocking" de HTTP/1.1.
- **Compresión de cabeceras** (algoritmo HPACK): reduce el tamaño de las cabeceras repetitivas.
- **Priorización de recursos**: el cliente puede indicar qué recursos son más urgentes.
- **Server Push**: el servidor puede enviar recursos al cliente antes de que los solicite.
- **Formato binario**: más eficiente de procesar que el texto plano de HTTP/1.x.

#### HTTP/3 y QUIC

HTTP/3 ([RFC 9114](https://datatracker.ietf.org/doc/html/rfc9114), 2022) es la versión más reciente. Sus principales diferencias con HTTP/2:

- Corre sobre **QUIC** en lugar de TCP. QUIC usa **UDP** como transporte.
- **Resuelve el head-of-line blocking real**: en HTTP/2, un paquete TCP perdido bloquea todos los streams de esa conexión. QUIC gestiona cada stream de forma independiente.
- **Handshake más rápido**: integra TLS 1.3 en el propio handshake QUIC, logrando conexión en **1-RTT** (e incluso 0-RTT para servidores conocidos).
- Adoptado por más del 95% de los navegadores principales y el 34% del top 10 millones de sitios web (datos 2024).

---

## 4. Protocolo HTTPS

### 4.1 Diferencias HTTP vs HTTPS

**HTTPS** (*HTTP Secure*) es HTTP sobre una capa de seguridad proporcionada por **SSL/TLS**. Una idea clave que hay que grabar: la diferencia no está en cómo funciona HTTP (eso no cambia), sino en que se añade una capa de cifrado y autenticación entre HTTP y TCP. Es como meter el mismo mensaje dentro de un sobre sellado en lugar de enviarlo en una postal.

| Característica | HTTP | HTTPS |
|---|---|---|
| Puerto por defecto | 80 | 443 |
| Cifrado | No | Sí (TLS) |
| Autenticación del servidor | No | Sí (certificados digitales) |
| Integridad de datos | No garantizada | Sí (MAC/AEAD) |
| Rendimiento | Menor carga | Mayor carga (procesamiento criptográfico) |
| SEO | Penalizado por navegadores modernos | Favorecido por motores de búsqueda |

### 4.2 Posición en el modelo de capas

HTTPS opera en la **capa de aplicación**, pero introduce una **capa adicional de cifrado** (TLS) entre la capa de aplicación (HTTP) y la capa de transporte (TCP). El esquema de capas queda:

```
[Aplicación: HTTP]
       ↕
[Seguridad: TLS/SSL]
       ↕
[Transporte: TCP]
       ↕
[Internet: IP]
```

### 4.3 Certificados digitales y Autoridades de Certificación

¿Cómo sabe tu navegador que el servidor con el que habla es realmente `www.banco.com` y no alguien haciéndose pasar por él? A través de los **certificados digitales**. Para establecer una conexión HTTPS, el servidor presenta un certificado emitido por una **CA** (*Certification Authority*, Autoridad de Certificación) en la que el navegador ya confía.

El certificado contiene:
- La **clave pública** del servidor.
- El **nombre del dominio** al que corresponde.
- La **firma digital** de la CA, que garantiza su autenticidad.
- La **fecha de validez**.

El navegador verifica el certificado contra su lista de CAs de confianza (incluida en el sistema operativo o en el propio navegador). Si la CA es de confianza y el dominio coincide, la conexión se establece con candado verde.

**Tipos de validación de certificados**:
- **DV** (*Domain Validation*): Solo verifica la propiedad del dominio. Más rápido y económico. Usado por Let's Encrypt.
- **OV** (*Organization Validation*): Verifica además la existencia legal de la organización.
- **EV** (*Extended Validation*): Verificación exhaustiva. Muestra el nombre de la organización en la barra de direcciones.

**Let's Encrypt**: CA gratuita y automatizada, lanzada en 2016 por la ISRG, que ha democratizado el uso de HTTPS. Emite certificados DV con validez de 90 días y renovación automática.

---

## 5. Protocolo SSL/TLS

### 5.1 Introducción y evolución

**SSL** (*Secure Sockets Layer*) fue desarrollado por Netscape en los años 90 para resolver un problema obvio: HTTP enviaba todo en texto plano y cualquiera en la red podía leer tus contraseñas. Su sucesor, **TLS** (*Transport Layer Security*), fue definido por el IETF y es el estándar actual. Aunque coloquialmente todo el mundo sigue diciendo "SSL", lo que realmente se usa hoy es TLS. El nombre SSL quedó pegado en el lenguaje popular aunque técnicamente esté obsoleto.

| Versión | Año | Estado actual |
|---|---|---|
| SSL 2.0 | 1995 | Obsoleto, vulnerable |
| SSL 3.0 | 1996 | Obsoleto, vulnerable (ataque POODLE) |
| TLS 1.0 | 1999 | Obsoleto desde 2020 (RFC 8996) |
| TLS 1.1 | 2006 | Obsoleto desde 2020 (RFC 8996) |
| **TLS 1.2** | 2008 | Ampliamente usado, seguro con configuración correcta ([RFC 5246](https://www.rfc-editor.org/rfc/rfc5246)) |
| **TLS 1.3** | 2018 | Versión actual y recomendada ([RFC 8446](https://tools.ietf.org/html/rfc8446)) |

### 5.2 Características de TLS

El PDF describe tres propiedades fundamentales que TLS proporciona:

- **Privacidad**: Las conexiones protegidas con TLS utilizan **cifrado simétrico** para proteger los datos transmitidos (AES-GCM, ChaCha20-Poly1305 en TLS 1.3).
- **Verificación de identidad**: La **criptografía de clave pública** (asimétrica) se utiliza para autenticar la identidad del servidor (y opcionalmente del cliente).
- **Integridad**: Los mensajes se verifican mediante **códigos de autenticación de mensaje** (MAC/AEAD), detectando cualquier modificación no autorizada durante la transmisión.

### 5.3 Cifrado híbrido en TLS

> **Clave de examen**: TLS/SSL utiliza **tanto cifrado simétrico como asimétrico** (criptografía híbrida). La respuesta correcta ante "¿qué tipo de cifrado usa TLS/SSL?" es siempre **simétrico y asimétrico**.

El proceso combina ambos tipos aprovechando las ventajas de cada uno:

**Fase 1 — Handshake (criptografía ASIMÉTRICA)**:
1. El cliente se conecta al servidor y solicita su identidad.
2. El servidor envía su **certificado digital** (que contiene su clave pública).
3. El cliente verifica el certificado contra una CA de confianza.
4. Se realiza el **intercambio de claves** mediante RSA o ECDHE (*Elliptic Curve Diffie-Hellman Ephemeral*).
5. Ambas partes derivan las **claves de sesión simétricas** a partir del intercambio.

**Fase 2 — Transferencia de datos (criptografía SIMÉTRICA)**:
- Todos los datos de la sesión se cifran con la clave de sesión acordada.
- La criptografía simétrica es mucho más rápida y adecuada para grandes volúmenes de datos.

```
Cliente                                    Servidor
  │                                           │
  │──── ClientHello (versiones, cipher suites) ──→│
  │←─── ServerHello + Certificado ──────────────│
  │←─── (clave pública en certificado) ─────────│
  │                                           │
  │── Verificación del certificado (CA) ──────│
  │── Generación clave de sesión (ECDHE) ─────│
  │                                           │
  │═══════ Datos cifrados con clave simétrica ══│
```

### 5.4 Capas del protocolo TLS

TLS opera en dos subcapas:

- **Protocolo de Registro TLS** (*TLS Record Protocol*): Gestiona la encapsulación, compresión (deshabilitada en TLS 1.3 por seguridad) y transmisión de datos. Fragmenta los datos de la aplicación en bloques, los cifra y los envía.

- **Protocolo de Enlace TLS** (*TLS Handshake Protocol*): Negocia las configuraciones de seguridad (versión, cipher suite) y autentica a las partes antes de que comience el intercambio de datos. También incluye el protocolo de cambio de especificación de cifrado y el protocolo de alerta.

### 5.5 Mejoras de TLS 1.3 respecto a TLS 1.2

([RFC 8446](https://tools.ietf.org/html/rfc8446), publicado en agosto de 2018):

- **Handshake en 1-RTT** (vs 2-RTT en TLS 1.2): reduce la latencia del establecimiento de conexión un 30-50%.
- **0-RTT Session Resumption**: para conexiones a servidores ya visitados, permite enviar datos en el primer paquete.
- **Perfect Forward Secrecy obligatorio**: solo permite ECDHE para intercambio de claves. Si la clave privada del servidor es comprometida, las sesiones pasadas no pueden descifrarse.
- **Eliminación de algoritmos inseguros**: se eliminan RSA para intercambio de claves, RC4, DES, 3DES, MD5, SHA-1, compresión TLS y renegociación insegura.
- **Cipher suites simplificadas** (solo AEAD): AES-128-GCM, AES-256-GCM, ChaCha20-Poly1305.
- **Cifrado del handshake**: en TLS 1.3, los certificados también se transmiten cifrados.

---

## 6. Principales servicios de Internet

### 6.1 FTP (File Transfer Protocol)

**FTP** es el protocolo estándar para la transferencia de archivos entre un cliente y un servidor. Usa dos canales separados: uno para el control (comandos) y otro para los datos. Esta separación es una de sus particularidades técnicas que puede aparecer en el examen.

- **Puertos**: 21 (control) y 20 (datos en modo activo).
- **Uso**: Subida y descarga de archivos grandes, gestión de archivos en servidores remotos.
- **Limitación de seguridad**: Las credenciales y datos viajan sin cifrar, igual que Telnet. La alternativa segura es **SFTP** (SSH File Transfer Protocol, usa SSH) o **FTPS** (FTP sobre TLS).
- URI válida: `ftp://ftp.rs.internic.net/domain/named.root` ([RFC 1738](https://www.rfc-editor.org/rfc/rfc1738)).

### 6.2 Telnet

Telnet permite acceso remoto a dispositivos y servidores mediante terminal de texto. Su problema fatal: **no cifra nada**. Las contraseñas y datos viajan en texto plano, lo que significa que cualquiera que esté escuchando el tráfico puede leerlos sin esfuerzo. Por eso fue sustituido por **SSH** y hoy está prácticamente en desuso en entornos de producción.

- **Puerto**: 23.

### 6.3 WWW (World Wide Web)

La **World Wide Web** es un sistema de documentos y recursos enlazados por hipertexto que se acceden mediante navegadores web. No es sinónimo de Internet: la WWW es un servicio que corre sobre Internet.

- **Protocolo base**: HTTP y HTTPS.
- **Lenguajes**: HTML (estructura), CSS (presentación), JavaScript (comportamiento).
- **Inventor**: Tim Berners-Lee (CERN, 1991).
- **Organismo de estándares**: W3C.

### 6.4 Correo electrónico

Los principales protocolos del correo electrónico:

| Protocolo | Función | Puerto | Seguro |
|---|---|---|---|
| **SMTP** | Envío de correo | 25 / 587 | 587 (STARTTLS) |
| **POP3** | Descarga de correo (elimina del servidor) | 110 | 995 (SSL) |
| **IMAP** | Acceso sincronizado (conserva en servidor) | 143 | 993 (SSL) |

**Correo Web (Webmail)**: Servicios como Gmail y Outlook que permiten acceder y gestionar correos electrónicos a través de un navegador web. Ventaja: acceso desde cualquier dispositivo con conexión a Internet.

### 6.5 Grupos de Noticias (News) — NNTP

Antes de que existieran Reddit o los foros web, los **newsgroups** eran el lugar donde la gente debatía online. Funcionan con el protocolo **NNTP** (*Network News Transfer Protocol*): grupos de discusión organizados por tema donde los usuarios publican y leen mensajes. El más famoso fue **Usenet**. Hoy están prácticamente obsoletos, pero el examen puede mencionarlos como protocolo de Internet clásico.

### 6.6 Mensajería Instantánea

Comunicación en tiempo real: texto, voz y archivos entre usuarios. La diferencia con el correo es la inmediatez. Ejemplos actuales: WhatsApp, Telegram, Slack. Técnicamente, muchos usan protocolos propietarios o derivados del estándar abierto **XMPP** (*Extensible Messaging and Presence Protocol*), que es el que podría aparecer en un examen.

### 6.7 Redes Sociales

Plataformas para la interacción social y el intercambio de contenido. Desde el punto de vista técnico, son aplicaciones web que utilizan todo lo que hemos visto: HTTP/HTTPS, APIs REST, bases de datos, CDNs... Ejemplos: Facebook, Twitter/X, LinkedIn, Instagram. El examen no suele preguntar sobre ellas directamente, pero sirven de contexto para entender por qué la escalabilidad y las APIs son tan importantes.

### 6.8 DNS (Domain Name System)

**DNS** es el servicio que traduce nombres de dominio legibles por humanos (`www.ejemplo.com`) en direcciones IP (`93.184.216.34`) que los equipos pueden usar para enrutar paquetes.

- **Puerto**: 53 (UDP para consultas estándar, TCP para transferencias de zona y respuestas largas).
- **Jerarquía DNS**: Root servers → TLD (`.com`, `.es`) → Nameservers autoritativos → Registro local.
- **DDNS** (*Dynamic DNS*): Servicio que actualiza automáticamente los registros DNS cuando cambia una IP dinámica. Solución para alojar servidores con IP dinámica usando un dominio fijo (servicios como No-IP, DuckDNS). Para usar el propio dominio registrado, se configura una redirección en el registrador del dominio hacia el hostname DDNS.

### 6.9 VoIP y Streaming

- **VoIP** (*Voice over IP*): Transmisión de voz sobre redes IP (Skype, Teams, Zoom). Usa UDP por su baja latencia. TLS/SRTP para cifrado.
- **Streaming**: Transmisión continua de audio o vídeo (YouTube, Netflix, Spotify). Usa protocolos como HLS (*HTTP Live Streaming*), DASH, o RTMP.

---

## 7. Desarrollo web y arquitecturas modernas

### 7.1 REST y APIs RESTful

**REST** (*Representational State Transfer*) es el estilo arquitectónico dominante para diseñar APIs web. Lo propuso Roy Fielding en su tesis doctoral en 2000. Lo importante: REST **no es un protocolo**, es un conjunto de principios que, cuando se aplican sobre HTTP, dan lugar a una API RESTful. Cuando el examen pregunta "qué método HTTP se usa para obtener/crear/actualizar/eliminar un recurso en REST", está preguntando exactamente sobre esto.

**Principios REST**:
1. **Sin estado (*stateless*)**: Cada petición debe contener toda la información necesaria; el servidor no almacena estado de sesión del cliente.
2. **Cliente-servidor**: Separación de preocupaciones entre cliente (UI) y servidor (datos/lógica).
3. **Interfaz uniforme**: Uso de recursos identificados por URIs y manipulados mediante representaciones (JSON, XML).
4. **Caché**: Las respuestas deben indicar si son cacheables.
5. **Sistema por capas**: El cliente no necesita saber si habla con el servidor final o con un intermediario.
6. **Código bajo demanda** (opcional): El servidor puede enviar código ejecutable al cliente.

**Mapeo REST — CRUD — HTTP**:

| Operación CRUD | Método HTTP | URL ejemplo | Respuesta típica |
|---|---|---|---|
| **Create** | POST | `POST /usuarios` | 201 Created |
| **Read** (colección) | GET | `GET /usuarios` | 200 OK |
| **Read** (recurso) | GET | `GET /usuarios/42` | 200 OK |
| **Update** (completo) | PUT | `PUT /usuarios/42` | 200 OK / 204 No Content |
| **Update** (parcial) | PATCH | `PATCH /usuarios/42` | 200 OK / 204 No Content |
| **Delete** | DELETE | `DELETE /usuarios/42` | 204 No Content |

**Richardson Maturity Model** (modelo de madurez REST):

| Nivel | Nombre | Descripción |
|---|---|---|
| **0** | Pantano de POX | Un único endpoint, un único método (habitualmente POST) |
| **1** | Recursos | URIs individuales por recurso |
| **2** | Verbos HTTP | Uso correcto de GET, POST, PUT, DELETE + códigos de estado |
| **3** | HATEOAS | Los recursos incluyen hipervínculos a acciones posibles (*Hypermedia As The Engine Of Application State*) |

> Fuente: [Martin Fowler — Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)

> **Clave de examen**: Para recuperar la información de un usuario específico en REST se usa **GET**. Para eliminarlo, **DELETE**. GET es **seguro e idempotente**; DELETE es **idempotente pero no seguro**.

### 7.2 Servidor web Apache HTTP Server

**Apache HTTP Server** (mantenido por la Apache Software Foundation) es uno de los servidores web más utilizados del mundo. Sus características clave:

- **Multiplataforma**: Disponible para Linux, Unix, BSD, macOS y Windows.
- **Extensible**: Sistema modular (`mod_*`). Ejemplos:
  - `mod_ssl`: soporte HTTPS/TLS
  - `mod_rewrite`: reescritura de URLs
  - `mod_proxy`: proxy inverso y balanceo de carga
  - `mod_auth_basic`: autenticación básica HTTP
- **Software libre**: Licencia Apache License 2.0.
- **Virtual Hosting**: Permite alojar múltiples sitios en un mismo servidor.

**Fichero de configuración principal**: `httpd.conf` (en sistemas Red Hat/CentOS: `/etc/httpd/conf/httpd.conf`; en Debian/Ubuntu: `/etc/apache2/apache2.conf`). Contiene directivas como:
- `Listen 80` — puerto de escucha.
- `DocumentRoot "/var/www/html"` — directorio raíz de documentos.

> Nota: `catalina.properties` es de Apache **Tomcat** (servidor de aplicaciones Java). `wls.properties` es de Oracle **WebLogic**. El fichero de Apache HTTP Server es `httpd.conf`.

**VirtualHost y directivas clave**:

```apache
<VirtualHost *:80>
    ServerName ejemplo.red.zaragoza.es
    DocumentRoot /var/www/ejemplo

    <Directory /almacen/log/>
        Options Indexes FollowSymlinks MultiViews
        AllowOverride AuthConfig
        AuthType basic
        AuthName "Logs de Gestion"
        AuthUserFile /etc/httpd/conf.d/.htpasswd_ejemplo
        Require valid-user
    </Directory>
</VirtualHost>
```

- `ServerName`: Establece el nombre de dominio al que responderá este VirtualHost cuando lleguen peticiones HTTP con ese nombre en la cabecera `Host:`. No define la IP (eso va en `<VirtualHost *:80>`) ni configura DNS.
- `Options Indexes`: Permite que Apache muestre un **listado automático del contenido del directorio** si no existe un fichero índice (`index.html`). Sin esta opción, se devuelve un 403 Forbidden.
- `Require valid-user`: Requiere que el usuario se autentique con credenciales válidas definidas en el fichero `AuthUserFile`. No permite acceso sin restricción (eso sería `Require all granted`), no filtra por IP (eso sería `Require ip`) y no requiere certificados SSL (eso requeriría `SSLVerifyClient`).

### 7.3 Tecnologías de desarrollo web

**Frontend (cliente)**:
- **HTML5**: Estructura y semántica del documento. Introduce APIs como Web Storage, Canvas, WebSockets.
- **CSS3** (*Cascading Style Sheets*): Presentación visual. CSS son las siglas de **Cascading Style Sheets** (no "Cascading Sheets Style" ni "Cascading Sheet Style"). Propiedades como `display: none` eliminan el elemento del flujo de renderizado por completo (sin dejar espacio); `visibility: hidden` lo oculta pero conserva su espacio.
- **JavaScript**: Lógica en el cliente. Frameworks: React, Angular, Vue.js.

**Web Storage (HTML5)**:

| API | Persistencia | Alcance |
|---|---|---|
| **`localStorage`** | Persiste hasta eliminación explícita (aunque el navegador se cierre y reabra) | Dominio y protocolo |
| **`sessionStorage`** | Se elimina al cerrar la pestaña o ventana del navegador | Pestaña individual |

> **Clave de examen**: `sessionStorage` no tiene temporizador de inactividad; se elimina al cerrar la **pestaña**, no por tiempo de inactividad. `localStorage` persiste indefinidamente hasta que se llama a `removeItem()` o `clear()`.

**Server Side Rendering (SSR)**: Técnica por la cual el servidor genera el HTML completo y lo envía al cliente, logrando cargas iniciales más rápidas y mejor SEO. Frameworks: Next.js (React), Nuxt.js (Vue), Angular Universal. Opuesto: **Client Side Rendering (CSR)**, donde el navegador ejecuta JavaScript para generar el HTML dinámicamente (SPAs: Single Page Applications).

**Backend**:
- Lenguajes: Java/Jakarta EE, Python, Node.js, PHP, Ruby, Go.
- **EJB** (*Enterprise JavaBeans*): Componentes de negocio del **lado del servidor** (no del cliente). Se despliegan en un **contenedor EJB** dentro del servidor de aplicaciones. Facilitan la gestión declarativa de transacciones. La versión EJB se desplegó en JBoss/WildFly, IBM WebSphere, Oracle WebLogic.

**Jakarta EE** (antes Java EE) — cronología que hay que dominar para el examen:
- **J2EE** fue creado por **Sun Microsystems** (no por el MIT, ni por Oracle, ni por IBM).
- Oracle adquirió Sun en 2010 y continuó el desarrollo bajo el nombre Java EE.
- **Java EE 8** (2017) fue la **última versión** bajo el paraguas de Oracle.
- Oracle cedió el control a la **Eclipse Foundation** en 2018. Al traspasar el proyecto, el nombre "Java" no podía usarse por derechos de marca, así que el proyecto se rebautizó **Jakarta EE**.
- Jakarta EE es la continuación directa y compatible de Java EE. Si ves "Java EE 9", no existe: lo que vino después de Java EE 8 fue Jakarta EE 8.

### 7.4 Desarrollo móvil

**iOS vs Android**:

| Característica | iOS | Android |
|---|---|---|
| Fabricante | Apple | Google (AOSP) |
| Lenguaje principal moderno | **Swift** (presentado en 2014) | **Kotlin** / Java |
| Lenguaje histórico | Objective-C | Java |
| Última versión (2024) | **iOS 18** / **iPadOS 18** | Android 15 |

> **Clave de examen**: El lenguaje para desarrollo iOS **moderno** es **Swift**. Objective-C es el lenguaje heredado. Java es de Android. iOS 18 e iPadOS 18 fueron lanzados el 16 de septiembre de 2024; **ambos comparten el número de versión principal** (18).

**Componentes principales de una app Android**:
1. **Activity** — pantalla de interfaz de usuario (lo que el usuario ve)
2. **Service** — tarea en segundo plano sin UI
3. **BroadcastReceiver** — receptor de eventos del sistema
4. **ContentProvider** — gestor de datos compartidos entre apps

> **Clave de examen**: El componente de Android que representa una pantalla de UI es la **Activity**.

**Frameworks de desarrollo multiplataforma**:

| Framework | Tecnología | Ventaja principal |
|---|---|---|
| **Xamarin** (.NET MAUI) | C# / .NET | Reutilización de código entre Android, iOS y Windows |
| **React Native** | JavaScript / React | Componentes nativos con JS |
| **Flutter** | Dart | UI consistente en todas las plataformas |
| **Ionic** | HTML/CSS/JS + Angular/React/Vue | Web technologies en app nativa |
| **Onsen UI** | Web Components | Agnóstico de framework, soporta Angular, React, Vue |

> **Clave de examen**: Xamarin permite reutilizar código entre plataformas pero **no elimina completamente** la necesidad de conocimientos específicos de plataforma. Ni Ionic ni Onsen UI **requieren AngularJS obligatoriamente**; ambos soportan múltiples frameworks.

---

## 8. Apache HTTP Server — Configuración avanzada

### 8.1 DNS dinámico (DDNS)

Imagina que quieres alojar un servidor web en casa con tu propio dominio registrado, pero tu ISP te asigna una IP diferente cada vez que reinicias el router. Cada vez que la IP cambia, tu dominio apuntaría a una dirección incorrecta. La solución es **DDNS** (*Dynamic DNS*).

Para alojar un servidor HTTP con IP dinámica usando un dominio registrado sin interrupciones:

1. **Activar un servicio DDNS** (No-IP, DuckDNS, etc.). Esto asigna un subdominio propio (ej. `miservidor.ddns.net`) y un cliente que detecta los cambios de IP y actualiza automáticamente el registro DNS.
2. **Configurar una redirección** en el registrador del dominio propio hacia el hostname DDNS.

Actualizar el DNS manualmente no es viable: la propagación DNS puede tardar hasta 48 horas y las IPs dinámicas pueden cambiar varias veces al día.

---

## Resumen para el examen

Los subtemas más frecuentes en el banco de 29 preguntas son: métodos HTTP y REST (GET/POST/PUT/DELETE), códigos de estado HTTP, SSL/TLS, configuración Apache, arquitecturas de aplicación, desarrollo web (HTML/CSS/JS), y desarrollo móvil (iOS/Android).

- **HTTP es sin estado** (*stateless*): cada petición es independiente. La sesión se gestiona con cookies u otros mecanismos externos.
- **Métodos HTTP en REST**: GET (leer), POST (crear), PUT (actualizar completo, idempotente), PATCH (actualizar parcial), DELETE (eliminar, idempotente). DELETE es idempotente: la misma petición DELETE varias veces produce el mismo resultado.
- **Códigos de estado**: 2xx = éxito; 4xx = error del cliente; 5xx = error del servidor. Error **500** = error del servidor → escalar al equipo responsable del servidor. **404** = recurso no encontrado. **201** = recurso creado. **204** = éxito sin cuerpo.
- **TLS/SSL usa cifrado híbrido**: asimétrico en el handshake (RSA/ECDHE para intercambiar claves), simétrico en la transmisión de datos (AES-GCM). La respuesta siempre es "simétrico Y asimétrico".
- **Versiones TLS**: TLS 1.0 y 1.1 están **obsoletos** (RFC 8996, 2020). TLS 1.2 sigue siendo válido. **TLS 1.3** (RFC 8446, 2018) es el actual: 1-RTT, forward secrecy obligatorio, solo AEAD.
- **HTTP/2**: multiplexación, compresión de cabeceras, binario, sobre TCP. **HTTP/3**: sobre QUIC/UDP, resuelve el head-of-line blocking, handshake integrado con TLS 1.3.
- **Arquitectura 3 capas**: las reglas de negocio van en el **servidor de aplicaciones** (capa intermedia), no en el cliente ni en la BD.
- **Apache**: fichero de configuración = `httpd.conf`. `ServerName` define el dominio del VirtualHost. `Require valid-user` exige autenticación con usuario y contraseña. `Options Indexes` muestra listado de directorio si no hay fichero índice. `catalina.properties` es de Tomcat, no de Apache HTTP Server.
- **URI válidas**: `ftp://`, `http://`, `ssh://` son esquemas IANA registrados. `scp://` no lo es.
- **CSS**: Cascading **Style** Sheets (style antes de sheets). `display: none` elimina el elemento del layout por completo; `visibility: hidden` lo oculta pero conserva el espacio.
- **Web Storage**: `sessionStorage` se elimina al cerrar la pestaña (no por inactividad). `localStorage` persiste indefinidamente hasta eliminación explícita.
- **SSR** (*Server Side Rendering*): el servidor genera el HTML completo y lo envía al cliente (más rápido en carga inicial, mejor SEO). Diferente de CSR (SPA, el navegador genera el HTML con JS).
- **iOS**: lenguaje moderno = **Swift**. Versión actual = **iOS 18** / **iPadOS 18** (ambas versión 18, lanzadas en septiembre 2024).
- **Android**: componente de pantalla = **Activity**. Lenguaje moderno = **Kotlin**.
- **Jakarta EE**: última versión como Java EE = **Java EE 8** (2017). Creado por **Sun Microsystems** (no MIT). EJBs son componentes del **lado del servidor**, desplegados en un contenedor EJB.
- **Xamarin**: ventaja principal = reutilización de código entre plataformas. No elimina completamente la necesidad de conocimientos específicos de plataforma.
- **DDNS**: solución para servidor HTTP con IP dinámica. Actualizar el DNS manualmente no es viable por los tiempos de propagación (hasta 48 horas).

---

## Preguntas frecuentes en exámenes

El análisis del banco de 29 preguntas revela los siguientes focos temáticos ordenados por frecuencia:

### 1. Métodos HTTP y REST (5 preguntas)
Las preguntas más frecuentes piden identificar qué método usar para cada operación, y qué característica (idempotencia, seguridad) tienen.

Ejemplos representativos:
- *"¿Cuál es el método HTTP para eliminar un recurso en REST?"* → DELETE
- *"¿Qué método HTTP es el más apropiado para recuperar un usuario específico?"* → GET
- *"¿Qué protocolo de comunicación usarías para una API de microservicios accesible por la web?"* → HTTPS

### 2. Códigos de estado HTTP (3 preguntas)
Se pregunta tanto por el significado concreto de un código como por la acción a tomar ante ese código.

Ejemplos representativos:
- *"¿Qué código obtenemos ante un error en el servidor?"* → 500
- *"¿Qué significa el código 204?"* → Solicitud correcta sin cuerpo de respuesta (no devuelve nada; diferente de 201 = creado, y de 404 = no encontrado)
- *"Varios usuarios obtienen Error 500, ¿cómo actúas?"* → Error del servidor; escalar a soporte del portal

### 3. Configuración Apache HTTP Server (4 preguntas)
Preguntas muy concretas sobre directivas de configuración de VirtualHost.

Ejemplos representativos:
- *"¿En qué fichero está la configuración del puerto de escucha?"* → `httpd.conf`
- *"¿Qué hace `ServerName`?"* → Define el dominio al que responderá el VirtualHost
- *"¿Qué hace `Require valid-user`?"* → Requiere autenticación con usuario y contraseña del fichero `AuthUserFile`
- *"¿Qué hace `Options Indexes`?"* → Permite mostrar listado del directorio si no hay fichero índice

### 4. SSL/TLS (1 pregunta directa, implícita en otras)
Siempre aparece la pregunta del tipo de cifrado que usa TLS.

Ejemplo representativo:
- *"En una conexión TLS/SSL se utiliza cifrado de tipo..."* → Simétrico y asimétrico

### 5. Arquitecturas de aplicación (2 preguntas)
Se pregunta sobre las capas y sobre dónde reside la lógica de negocio.

Ejemplos representativos:
- *"En arquitectura pura de tres capas, ¿dónde están las reglas de negocio?"* → Servidor de aplicaciones
- *"¿Cuál es la frase incorrecta sobre EJBs?"* → "Los EJBs son del lado del cliente" (FALSO; son del servidor)

### 6. Desarrollo web — HTML/CSS/JS (3 preguntas)
Preguntas sobre comportamiento de propiedades CSS, Web Storage y SSR.

Ejemplos representativos:
- *"¿Qué significan las siglas CSS?"* → Cascading Style Sheets
- *"¿Qué hace `display: none`?"* → Elimina el elemento por completo, sin ocupar espacio
- *"¿Qué diferencia hay entre `localStorage` y `sessionStorage`?"* → sessionStorage se elimina al cerrar la pestaña
- *"¿Cómo se llama la técnica de renderizar HTML en el servidor?"* → Server Side Rendering (SSR)

### 7. Desarrollo móvil iOS/Android (4 preguntas)
Preguntas sobre lenguajes de programación, versiones y componentes.

Ejemplos representativos:
- *"¿Qué lenguaje se usa principalmente para iOS moderno?"* → Swift
- *"¿Cuál es la última versión de iOS e iPadOS?"* → Ambas en versión 18
- *"¿Qué componente de Android representa una pantalla de UI?"* → Activity
- *"¿Qué ventaja principal ofrece Xamarin?"* → Reutilización de código entre plataformas

### 8. Jakarta EE / Java EE (3 preguntas)
Preguntas sobre historia, versiones y componentes.

Ejemplos representativos:
- *"¿Cuál es la última versión de Java EE antes del traspaso a Jakarta EE?"* → Java EE 8
- *"¿Cuál es la afirmación falsa sobre Java EE?"* → "Java EE tiene sus raíces en el MIT" (FALSO; fue creado por Sun Microsystems)
- *"¿Cuál es la frase incorrecta sobre EJBs?"* → Los EJBs son del lado del cliente (FALSO)

### 9. URI y estructuras de red (2 preguntas)
Preguntas sobre validez de URIs y soluciones de DNS dinámico.

Ejemplos representativos:
- *"¿Qué URI no tiene nomenclatura correcta?"* → `scp://server1:22` (SCP no es un esquema URI registrado en IANA)
- *"IP dinámica + dominio registrado sin interrupciones"* → Servicio DDNS + redirección en el registrador

---

## Referencias

- [RFC 9114 — HTTP/3](https://datatracker.ietf.org/doc/html/rfc9114) — Especificación oficial de HTTP/3 sobre QUIC (IETF, 2022)
- [RFC 8446 — TLS 1.3](https://tools.ietf.org/html/rfc8446) — Especificación oficial de TLS 1.3 (IETF, 2018)
- [RFC 5246 — TLS 1.2](https://www.rfc-editor.org/rfc/rfc5246) — Especificación de TLS 1.2
- [RFC 3986 — URIs genéricos](https://www.rfc-editor.org/rfc/rfc3986) — Sintaxis estándar de URIs
- [RFC 7540 — HTTP/2](https://datatracker.ietf.org/doc/html/rfc7540) — Especificación de HTTP/2
- [RFC 1738 — URLs](https://www.rfc-editor.org/rfc/rfc1738) — Especificación de esquemas FTP, HTTP en URLs
- [MDN Web Docs — HTTP](https://developer.mozilla.org/es/docs/Web/HTTP) — Documentación completa de HTTP, métodos y códigos de estado
- [MDN Web Docs — Códigos de estado HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Status) — Referencia completa de códigos de estado
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/2.4/) — Documentación oficial de Apache, directivas y módulos
- [Richardson Maturity Model — Martin Fowler](https://martinfowler.com/articles/richardsonMaturityModel.html) — Descripción del modelo de madurez REST
- [Cloudflare — How TLS works](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/) — Explicación del handshake TLS
- [IANA URI Schemes Registry](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml) — Registro oficial de esquemas URI
- [Jakarta EE — Eclipse Foundation](https://jakarta.ee/) — Página oficial de Jakarta EE
- [Apple Swift](https://developer.apple.com/swift/) — Lenguaje de programación para iOS/macOS
- [Android Developer Guide](https://developer.android.com/guide/components/fundamentals) — Componentes fundamentales de Android
- [No-IP — Dynamic DNS](https://www.noip.com/support/knowledgebase/what-is-dynamic-dns/) — Servicio DDNS
- [CCN-CERT — Píldora TLS](https://www.ccn.cni.es/index.php/es/docman/documentos-publicos/boletines-pytec/378-pildorapytec-nov2020-seguridad-tls/file) — Guía del CCN sobre seguridad en TLS
