# Tema 3: Administración de Servidores de Correo, Contenedores y Microservicios

Este tema cubre tres bloques tecnológicos esenciales en la administración de sistemas: los protocolos y servidores de correo electrónico, la arquitectura orientada a servicios y los servicios web, y la administración de contenedores y microservicios. En las oposiciones del Bloque IV (Sistemas y Comunicaciones), el peso del banco de preguntas se concentra en los protocolos SOA/SOAP/UDDI, los comandos Docker y la arquitectura de microservicios. El estudiante debe dominar los puertos de los protocolos de correo, el funcionamiento del ciclo SMTP, la diferencia entre hipervisores tipo 1 y tipo 2, y los conceptos operativos de Docker y Kubernetes.

---

## 1. Protocolos de Correo Electrónico

### 1.1 Introducción

El **correo electrónico** es el servicio más utilizado en Internet. Su funcionamiento se apoya en la suite **TCP/IP**, que proporciona tres protocolos principales para el enrutamiento del correo:

- **SMTP** (Simple Mail Transfer Protocol): envío entre servidores.
- **POP3** (Post Office Protocol v3): descarga de mensajes al cliente.
- **IMAP** (Internet Message Access Protocol): acceso y sincronización remota.

El flujo completo del correo electrónico tiene dos ciclos diferenciados: el **cliente emisor** se conecta a su servidor de correo mediante SMTP; ese servidor retransmite el mensaje por Internet hasta el **servidor receptor** del destinatario, también usando SMTP; finalmente, el **cliente receptor** descarga o accede al mensaje mediante POP3 o IMAP.

---

### 1.2 SMTP

**SMTP (Simple Mail Transfer Protocol)** es el estándar que facilita la transferencia de correos electrónicos entre servidores a través de conexiones punto a punto. Opera en **tiempo real** (en línea), encapsula los datos en tramas TCP/IP, y por defecto usa el **puerto 25**. Sus mensajes se componen de texto ASCII plano.

**Limitaciones de SMTP:**
- No ofrece mecanismo para que el cliente descargue mensajes del servidor (de eso se ocupan POP3 e IMAP).
- No autentica al remitente de forma nativa, lo que facilita la suplantación de identidad y el spam.
- Deja los correos en cola en el servidor hasta que el sistema receptor pueda recibirlos.

#### Comandos SMTP

| Comando   | Descripción |
|-----------|-------------|
| `HELO`    | Abre sesión con el servidor (identificación del cliente) |
| `EHLO`    | Variante extendida de HELO (RFC 1869 / RFC 5321), solicita lista de extensiones |
| `MAIL FROM` | Indica la dirección del remitente |
| `RCPT TO` | Indica la dirección del destinatario |
| `DATA`    | Inicia el cuerpo del mensaje; finaliza con una línea que contiene solo un punto (`.`) |
| `QUIT`    | Cierra la sesión |
| `RSET`    | Aborta la transacción en curso y borra todos los registros |
| `SEND`    | Entrega el mensaje directamente a una terminal |
| `SOML`    | Entrega a una terminal o a un buzón |
| `SAML`    | Entrega a una terminal y a un buzón |
| `VRFY`    | Solicita la verificación de un argumento (dirección) |
| `EXPN`    | Solicita la confirmación del argumento (expansión de listas) |
| `HELP`    | Muestra información sobre un comando |
| `NOOP`    | Reinicia temporizadores sin acción real |
| `TURN`    | Solicita al servidor que invierta los roles cliente-servidor |

#### Códigos de respuesta SMTP

El servidor siempre responde con un código numérico de tres dígitos. El primer dígito indica la categoría:

| Código | Categoría |
|--------|-----------|
| `2XX`  | Operación completada con éxito |
| `3XX`  | Aceptada, pero se esperan más datos del cliente |
| `4XX`  | Error transitorio; se puede reintentar |
| `5XX`  | Error permanente; no debe repetirse la instrucción |

Ejemplos comunes: `220 Service ready`, `250 OK`, `354 Start mail input`, `421 Service non available`, `550 No such user here`.

#### Flujo de una sesión SMTP

```
S: 220 Servidor SMTP
C: HELO miequipo.midominio.com
S: 250 Hello, please to meet you
C: MAIL FROM: <yo@midominio.com>
S: 250 Ok
C: RCPT TO: <destinatario@sudominio.com>
S: 250 Ok
C: DATA
S: 354 End data with <CR><LF>.<CR><LF>
C: Subject: Campo de asunto
C: From: yo@midominio.com
C: To: destinatario@sudominio.com
C:
C: Hola,
C: Esto es una prueba.
C: Hasta luego.
C:
C: .
C: <CR><LF>.<CR><LF>
S: 250 Ok: queued as 12345
C: quit
S: 221 Bye
```

#### Formato del mensaje SMTP

El mensaje se divide en dos partes tras la orden `DATA`:

- **Cabecera**: las primeras líneas del mensaje, con los campos `Subject`, `From` y `To`. Son fundamentales para que el cliente de correo organice y muestre correctamente los mensajes. No deben confundirse con las órdenes del protocolo (`MAIL FROM`, `RCPT TO`), que pertenecen al protocolo pero no al formato del mensaje.
- **Cuerpo del mensaje**: el contenido real, compuesto únicamente por texto y terminado con una línea que contiene solo un punto.

Para visualizarlo: el **remitente** (usuario con cliente Outlook) entrega su correo al **SMTP server emisor** (la "oficina de correos" del remitente), que lo transmite a través de Internet al **SMTP server receptor** (la "oficina de correos" del destinatario), desde donde los destinatarios recogen sus mensajes.

---

### 1.3 POP3

**POP3 (Post Office Protocol versión 3)** permite a los clientes locales de correo obtener mensajes almacenados en un servidor remoto. Se sitúa en la **capa de aplicación** del modelo OSI (capa 7). Evolucionó desde POP1 y POP2 hasta la versión actual.

- **Puerto estándar**: **110** (sin cifrado) / **995** (POP3S con SSL/TLS).
- **Funcionamiento**: el cliente se conecta, descarga todos los mensajes nuevos al dispositivo local, los elimina del servidor por defecto, y se desconecta.
- Especialmente útil en entornos con **conexión intermitente** a Internet.

**Ventajas de POP3:**

| Ventaja | Detalle |
|---------|---------|
| Descarga local | Los mensajes quedan en el dispositivo; accesibles sin conexión |
| Ahorro de espacio en servidor | No ocupa espacio en el servidor una vez descargado |
| Gestión eficiente con Outlook/Thunderbird | Organización de bandeja y carpetas locales |

**Desventajas de POP3:**

| Desventaja | Detalle |
|------------|---------|
| Sin sincronización multidispositivo | Un mensaje leído en un dispositivo puede aparecer no leído en otro |
| Descarga completa | En cada conexión se bajan todos los correos nuevos, se lean o no |
| Sin carpetas compartidas | Las carpetas, plantillas y borradores no se sincronizan |
| Eliminación del servidor | Por defecto borra los mensajes, imposibilitando acceso desde otro dispositivo |

---

### 1.4 IMAP

**IMAP (Internet Message Access Protocol)** es una alternativa más completa a POP3. Permite acceder al buzón de correo en Internet y sincronizar los mensajes entre dispositivos. Admite modos en línea y fuera de línea.

- **Puerto estándar**: **143** (sin cifrado) / **993** (IMAPS con TLS).
- El servidor Gmail es un ejemplo paradigmático de servidor IMAP.

**Características diferenciales de IMAP4:**

- **Modos de operación conectado y desconectado**: IMAP4 mantiene la conexión mientras la interfaz está activa, descargando mensajes bajo demanda. POP solo se conecta brevemente para descargar.
- **Conexión de múltiples clientes simultáneos al mismo buzón**: POP requiere que el cliente sea el único conectado. IMAP4 permite accesos simultáneos desde varios dispositivos y detecta los cambios realizados por otros clientes.
- **Acceso a partes MIME individuales**: IMAP4 permite obtener partes concretas de mensajes MIME (por ejemplo, solo el texto sin los adjuntos) sin descargar el mensaje completo.
- **Información del estado del mensaje**: IMAP4 usa señales (leído, respondido, eliminado) almacenadas en el servidor, visibles desde cualquier cliente.
- **Búsqueda en servidor**: permite buscar mensajes sin descargarlos, ahorrando tiempo y ancho de banda.
- **Gestión de carpetas en servidor**: el usuario puede crear, renombrar y eliminar carpetas, incluyendo carpetas compartidas.

Con IMAP, un único **servidor de correo central** sirve de forma sincronizada a múltiples dispositivos (smartphone, ordenador de escritorio, tablet), a diferencia de POP3, que descarga a un solo equipo sin retener copia en el servidor.

#### Tabla comparativa IMAP4 vs POP3

| Característica | IMAP4 | POP3 |
|----------------|-------|------|
| Almacenamiento | En el servidor | En local |
| Múltiples dispositivos | Sí | Difícil |
| Conexión | Permanente mientras se usa | Puntual para descargar |
| Sincronización de estado | Sí (leído/no leído en servidor) | No |
| Carpetas en servidor | Sí | Solo bandeja de entrada |
| Búsqueda remota | Sí | No |
| Descarga de adjuntos | Parcial (bajo demanda) | Descarga todo el mensaje |
| Puerto estándar | 143 / 993 (TLS) | 110 / 995 (TLS) |
| Ejemplo | Gmail, Outlook.com | Yahoo Mail (POP clásico) |

---

### 1.5 Tabla de puertos de correo

| Protocolo | Puerto sin cifrado | Puerto seguro (TLS/SSL) | Función |
|-----------|-------------------|------------------------|---------|
| SMTP | 25 | 465 (SMTPS) / 587 (envío autenticado) | Envío entre servidores |
| POP3 | 110 | 995 | Descarga de mensajes |
| IMAP | 143 | 993 | Acceso y sincronización |

> **Dato de examen**: Los puertos estándar para POP y SMTP son **110 y 25**, respectivamente. El puerto 80 es HTTP y el 23 es Telnet.

---

### 1.6 Clientes de correo: ejemplos

| Cliente | Envío | Recepción | Observaciones |
|---------|-------|-----------|---------------|
| Microsoft Outlook | SMTP | IMAP, POP3 y protocolo propietario Exchange | Integrado con Exchange Server |
| Mozilla Thunderbird | SMTP | IMAP, POP3 | Código abierto |
| Apple Mail (macOS) | SMTP | IMAP, POP3 | Integrado en macOS |
| Gmail | SMTP | IMAP | Sincronización multidispositivo |
| Microsoft Exchange Server | SMTP | POP, IMAP y protocolo propietario | Colaboración empresarial |
| Yahoo Mail | SMTP | IMAP | También soportó POP3 históricamente |

---

## 2. Servidores de Correo Electrónico

### 2.1 Introducción y componentes

Un **servidor de correo** es una aplicación ubicada en un servidor de red que proporciona servicios de correo electrónico. El componente clave es el **MTA (Mail Transfer Agent)**, que desempeña tres funciones:

- **Recibe mensajes** de otros MTA (actuando como servidor).
- **Envía mensajes** hacia otros MTA (actuando como cliente).
- **Actúa como intermediario** entre el MSA (Mail Submission Agent, que acepta mensajes de usuarios) y el siguiente MTA en la cadena de entrega.

Ejemplos de software MTA: **Sendmail**, **Mercury Mail Transport System**, **Lotus Notes/Domino**, **Microsoft Exchange Server**.

Otros agentes del ecosistema de correo:
- **MUA (Mail User Agent)**: cliente de correo del usuario final (Outlook, Thunderbird).
- **MDA (Mail Delivery Agent)**: entrega el mensaje al buzón del destinatario.
- **MSA (Mail Submission Agent)**: acepta el correo del MUA y lo entrega al MTA.

### 2.2 Proceso de intercambio de correo electrónico

El proceso completo de intercambio involucra cinco pasos:

1. **Creación del correo**: el remitente usa su cliente de correo (MUA) para redactar y crear el mensaje en formato estándar.
2. **Almacenamiento en el servidor local**: el mensaje se envía al almacén administrado por el servidor de correo local del remitente, generando una solicitud de envío.
3. **Negociación y envío**: el MTA local recupera el archivo e inicia la negociación SMTP con el servidor del destinatario para el envío.
4. **Recepción y almacenamiento en el buzón**: el servidor del destinatario corrobora la operación, recibe el mensaje y lo deposita en el buzón correspondiente (esencialmente un registro en una base de datos).
5. **Recuperación por el cliente receptor**: el software cliente del receptor (mediante POP3 o IMAP) recupera el archivo desde el servidor, almacenando una copia en el equipo del receptor.

### 2.3 Seguridad en el correo electrónico

El correo electrónico es, por diseño, bastante inseguro: cuando envías un mensaje, quedan copias en el servidor de origen y en el de destino, y las políticas de cada servidor pueden variar. Un servidor podría retener copias, reenviarlas a otros registros o incluso añadir destinatarios adicionales sin que el remitente lo sepa. Por eso, saber quién administra los servidores de correo que utilizas es fundamental.

Para mitigar estos riesgos, existen tres mecanismos de autenticación ampliamente adoptados: **SPF**, **DKIM** y **DMARC**.

#### SPF (Sender Policy Framework)

**SPF** extiende el protocolo SMTP para verificar qué máquinas están autorizadas a enviar correos en nombre de un dominio específico. Utiliza **registros DNS** para publicar una lista de IPs autorizadas. Cuando un servidor receptor recibe un correo, consulta el registro SPF del dominio remitente y verifica si la IP de origen está en la lista. Previene la suplantación de identidad (spoofing).

#### DKIM (DomainKeys Identified Mail)

**DKIM** añade una **firma digital criptográfica** en la cabecera del correo. El servidor emisor firma el mensaje con su clave privada; el servidor receptor verifica la firma usando la clave pública publicada en el DNS del dominio. Garantiza que el mensaje no ha sido alterado en tránsito y que proviene del dominio declarado.

#### DMARC (Domain-based Message Authentication, Reporting and Conformance)

**DMARC** se apoya en SPF y DKIM para indicar al servidor receptor qué hacer cuando un correo no supera las verificaciones. La política DMARC puede establecer que los mensajes fallidos sean: entregados, puestos en cuarentena o rechazados. Además, genera informes sobre los resultados de autenticación. Se configura también mediante registros DNS.

| Mecanismo | Qué verifica | Cómo funciona |
|-----------|-------------|---------------|
| SPF | IP del servidor emisor | Registro DNS con IPs autorizadas |
| DKIM | Integridad y origen del mensaje | Firma criptográfica en la cabecera |
| DMARC | Política de actuación | Se apoya en SPF y DKIM; publica política en DNS |

### 2.4 Funciones del administrador de correo electrónico

El administrador de correo no solo "mantiene el servidor funcionando": su rol abarca toda la cadena de responsabilidades técnicas, organizativas y de seguridad del sistema de mensajería:

- **Evaluación del rendimiento**: controles para garantizar el buen funcionamiento de la infraestructura de correo.
- **Gestión de problemas**: investiga y resuelve incidencias en colaboración con otros departamentos.
- **Planificación y estrategia**: colabora con dirección para resolver problemas de conectividad y planificar expansiones.
- **Documentación y normativas**: prepara documentos sobre el uso y los estándares del sistema.
- **Seguridad y acceso**: recomienda medidas de seguridad y gestiona accesos privilegiados.
- **Capacitación**: elabora materiales de formación para los empleados.
- **Estructura profesional**: desarrolla normas de protocolo de comunicación para la organización.
- **Colaboración con RRHH**: establece pautas para proteger la confidencialidad del empleado.
- **Gestión de cuentas departamentales**: mantiene cuentas de correo generales y participa en migraciones.
- **Control de almacenamiento**: supervisa niveles de almacenamiento y plazos de retención de correos.

### 2.5 Lotus Notes / HCL Notes

**Lotus Notes** (ahora **HCL Notes**) es un sistema cliente/servidor desarrollado por Lotus Software (filial de IBM). Su arquitectura se compone de:

- **Lotus Domino**: el servidor central.
- **Lotus Notes**: el cliente para correo y colaboración.
- **Domino Administrator**: cliente para la administración del servidor Domino.
- **Domino Designer**: entorno integrado de desarrollo (IDE) para crear aplicaciones Notes.

**Funcionalidades principales**: correo electrónico, gestión de calendarios y agendas, intercambio de bases de datos (documentos, procedimientos, manuales, foros de discusión), flujos de trabajo empresariales (solicitudes de vacaciones, anticipos, etc.). Ofrece servicios de servidor de correo (POP3 e IMAP), aplicaciones propias y HTTP.

### 2.6 Microsoft Exchange Server

**Microsoft Exchange Server** es un software propietario de colaboración entre usuarios desarrollado por Microsoft. Comprende un servidor de correo, un cliente de correo electrónico y aplicaciones de trabajo en grupo.

**Características principales:**
- Soporta POP, IMAP y correo web (Outlook Web Access/OWA).
- Su cliente nativo es **Microsoft Outlook**.
- Servicio de correo electrónico ininterrumpido con alta disponibilidad.
- Integra servicios de voz con mensajería unificada.
- Integración con **SharePoint** para colaboración más efectiva.
- Funciones de análisis para prevenir el filtrado de información confidencial.
- **Políticas de retención** para clasificación y eliminación de correos.
- Acceso seguro desde múltiples plataformas (escritorio, web, móvil).

---

## 3. Arquitectura Orientada a Servicios (SOA) y Servicios Web

### 3.1 SOA: principios y características

La **Arquitectura Orientada a Servicios (SOA)** es una forma de pensar los sistemas de una organización como un conjunto de servicios reutilizables e independientes. En lugar de construir un sistema monolítico donde todo está entrelazado, SOA propone descomponer la funcionalidad en **componentes débilmente acoplados** (los servicios) que se describen de forma estandarizada y pueden ser descubiertos e invocados por otros sistemas.

**Principios fundamentales de SOA:**

- **Débil acoplamiento**: los servicios son independientes entre sí; un fallo en un servicio no se propaga al resto.
- **Reutilización**: los servicios pueden reutilizarse en distintos procesos de negocio sin duplicar código.
- **Interoperabilidad**: servicios desarrollados en distintas tecnologías y lenguajes pueden comunicarse entre sí.
- **Abstracción**: el consumidor solo necesita conocer la interfaz del servicio, no su implementación interna.
- **Descubrimiento**: los servicios pueden localizarse a través de un directorio (UDDI).

> **Dato de examen**: SOA **reduce** (no aumenta) el acoplamiento entre componentes. Un fallo en un servicio no afecta a los demás gracias a su independencia.

### 3.2 Estándares del ecosistema SOAP/SOA

Los servicios web basados en SOAP se articulan en torno a tres estándares fundamentales:

#### SOAP (Simple Object Access Protocol)

**SOAP** es un protocolo de mensajería basado en **XML** para el intercambio de información estructurada en entornos distribuidos. Estandarizado por el W3C, puede funcionar sobre distintos transportes (HTTP, SMTP, TCP), aunque HTTP es el más habitual.

Estructura de un mensaje SOAP:

```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <!-- Cabecera opcional: seguridad, transacciones... -->
  </soap:Header>
  <soap:Body>
    <!-- Contenido del mensaje (petición o respuesta) -->
    <GetProductoRequest>
      <Id>123</Id>
    </GetProductoRequest>
  </soap:Body>
</soap:Envelope>
```

#### WSDL (Web Services Description Language)

**WSDL** es un lenguaje basado en XML para describir la interfaz de un servicio web: operaciones disponibles, tipos de datos, mensajes y el endpoint de acceso. Es el "contrato" del servicio: define qué hace y cómo llamarlo.

#### UDDI (Universal Description, Discovery and Integration)

**UDDI** es el directorio o catálogo de servicios web. Funciona como las "páginas amarillas" de los servicios web: permite a las empresas **publicar** sus servicios, a los consumidores **descubrirlos** e **integrarlos** en sus aplicaciones. Los registros UDDI almacenan los WSDL y los datos de contacto de los proveedores.

> **Dato de examen**: UDDI es el **catálogo** de servicios web basados en SOAP. WSDL es el lenguaje de **descripción** del servicio (el contrato). No confundirlos.

| Estándar | Rol | Analogía |
|----------|-----|----------|
| SOAP | Protocolo de mensajes (XML) | El lenguaje de comunicación |
| WSDL | Descripción del servicio | El manual de instrucciones |
| UDDI | Directorio de servicios | Las páginas amarillas |

### 3.3 ESB (Enterprise Service Bus)

Imagina una empresa con diez aplicaciones distintas que necesitan comunicarse entre sí: sin un intermediario, necesitarías 45 conexiones punto a punto. El **ESB (Bus de Servicios Empresariales)** resuelve este problema: es un patrón arquitectónico en el que un componente software centralizado actúa como intermediario universal para todas las aplicaciones. Funciones del ESB:

- Transforma modelos de datos entre aplicaciones con formatos distintos.
- Gestiona la conectividad y la mensajería.
- Enruta mensajes al destino correcto.
- Convierte protocolos de comunicación (por ejemplo, de SOAP a REST).
- Compone múltiples solicitudes en una sola operación.

Todas las aplicaciones se conectan al bus en lugar de comunicarse directamente entre sí, reduciendo la complejidad de integración de forma exponencial.

### 3.4 Servicios web: REST vs SOAP

| Característica | SOAP | REST |
|----------------|------|------|
| Naturaleza | Protocolo estricto | Estilo arquitectónico |
| Formato de mensajes | Solo XML | JSON, XML, HTML, etc. |
| Transporte | HTTP, SMTP, TCP... | HTTP exclusivamente |
| Descripción | WSDL | OpenAPI/Swagger |
| Seguridad | WS-Security (a nivel de mensaje) | HTTPS, OAuth 2.0, JWT |
| Verbos | No usa verbos HTTP con semántica | GET, POST, PUT, PATCH, DELETE |
| Estado | Sin estado o con estado | Sin estado (stateless) |
| Uso típico | Banca, servicios gubernamentales, ERP | APIs web modernas |

**Verbos HTTP en REST:**

| Verbo | Operación CRUD | Descripción |
|-------|----------------|-------------|
| GET | Read | Obtiene un recurso |
| POST | Create | Crea un recurso nuevo |
| PUT | Update/Replace | Reemplaza un recurso completo |
| PATCH | Update/Modify | Modifica parcialmente un recurso |
| DELETE | Delete | Elimina un recurso |

> **Dato de examen crítico**: **WS-Security NO es una característica de REST**. Es un estándar del stack WS-* diseñado para SOAP. REST usa HTTPS, OAuth 2.0 y JWT para seguridad.

### 3.5 Protocolos de mensajería instantánea

**XMPP (Extensible Messaging and Presence Protocol)** y **SIMPLE (SIP for Instant Messaging and Presence Leveraging Extensions)** son **protocolos de mensajería instantánea basados en estándares abiertos** (no propietarios ni cerrados):

- **XMPP**: basado en XML sobre TCP, estandarizado por la IETF (RFC 6120, RFC 6121). Fue usado por WhatsApp en sus inicios (versión modificada) y por Google Talk.
- **SIMPLE**: extensión del protocolo SIP para añadir mensajería instantánea y presencia. Definido por la IETF (RFC 3428, RFC 3856).

---

## 4. Contenedores y Microservicios

### 4.1 Introducción: evolución desde el monolito

El desarrollo de aplicaciones ha evolucionado de las **aplicaciones monolíticas** (toda la lógica en un único componente desplegable, con fuerte acoplamiento entre sus partes) hacia los **microservicios en contenedores**.

**Limitaciones de las aplicaciones monolíticas:**
- Acoplamiento estrecho entre componentes.
- Dificultad para escalar individualmente una parte de la aplicación.
- Un fallo en un módulo puede comprometer toda la aplicación.
- Las actualizaciones requieren redesplegar todo el sistema.

Los **microservicios** permiten un desarrollo desacoplado, escalabilidad eficiente y cambios focalizados, siendo especialmente relevantes en entornos de nube.

### 4.2 Arquitectura de microservicios

Los **microservicios** son una aproximación al desarrollo y despliegue de aplicaciones que descompone la funcionalidad en **componentes independientes**, cada uno implementando una sola función y comunicándose con contratos bien definidos.

**Características:**
- Cada microservicio es autónomo y tiene su propia lógica de negocio (y habitualmente su propia base de datos).
- Pueden escalarse horizontalmente de forma **independiente**.
- Admiten actualizaciones independientes sin afectar al resto del sistema.
- Permiten usar distintas tecnologías y lenguajes para cada servicio (**polyglot programming**).

#### Ventajas de los microservicios

| Ventaja | Descripción |
|---------|-------------|
| Escalabilidad granular | Se escala solo el servicio que lo necesita |
| Despliegue independiente | Actualizar un servicio no requiere desplegar toda la aplicación |
| Aislamiento de fallos | Un fallo en un servicio no colapsa el sistema completo |
| Adopción de tecnologías recientes | Cada servicio puede usar la tecnología más adecuada |
| Desarrollo paralelo | Equipos distintos pueden trabajar en servicios diferentes |

#### Desventajas de los microservicios

| Desventaja | Descripción |
|------------|-------------|
| Desafíos en la partición | Decidir cómo dividir la aplicación en servicios es complejo |
| Complejidad en la comunicación | Mayor latencia y complejidad al implementar comunicación entre servicios |
| Limitaciones en transacciones atómicas | Las transacciones distribuidas son difíciles; se requiere coherencia eventual |
| Complejidad operativa | Gestionar numerosos servicios independientes es más complejo |
| Dificultades en la refactorización de contratos | Cambios en la partición pueden romper la compatibilidad con los clientes |

#### Microservicios vs Monolito

| Característica | Monolito | Microservicios |
|----------------|----------|----------------|
| Despliegue | Una sola unidad | Múltiples servicios independientes |
| Escalabilidad | Todo o nada | Granular por servicio |
| Acoplamiento | Alto | Bajo (débilmente acoplado) |
| Tecnología | Homogénea | Heterogénea (polyglot) |
| Tolerancia a fallos | Un fallo puede romper todo | Aislamiento de fallos |
| Complejidad operativa | Baja | Alta |

### 4.3 Comunicación entre microservicios

En una aplicación basada en microservicios (sistema distribuido que se ejecuta en varios equipos), la **comunicación entre servicios es esencial**. Los servicios interactúan mediante protocolos entre procesos como HTTP, TCP, AMQP u otros protocolos binarios.

Existen dos enfoques principales:

1. **Comunicación REST basada en HTTP**: para consultar datos de forma síncrona entre servicios.
2. **Mensajería asíncrona basada en eventos**: un microservicio publica un evento cuando ocurre algo significativo; otros microservicios suscritos a ese evento lo reciben a través de un **bus de eventos** y reaccionan en consecuencia.

El **bus de eventos** permite la comunicación de publicación y suscripción entre microservicios, facilitando la propagación de cambios sin que los componentes se reconozcan explícitamente entre sí.

```
Microservice A --[Event]--> Event Bus --[Event]--> Microservice B
                                      --[Event]--> Microservice C
```

### 4.4 Contenedores

La **contenedorización** es un método ágil de desarrollo de software donde una aplicación y sus dependencias se empaquetan y prueban como una unidad llamada **imagen de contenedor**, para luego desplegarse en un sistema operativo host.

**Conceptos clave:**

| Concepto | Definición |
|----------|-----------|
| **Host de contenedor** | La máquina que ejecuta los contenedores |
| **Imagen de contenedor** | Base inmutable (de solo lectura) de un contenedor; plantilla |
| **Contenedor** | Instancia en tiempo de ejecución de una imagen (con capa de escritura) |
| **Imagen del SO de contenedor** | Primera capa base que compone el contenedor |
| **Repositorio de contenedor** | Almacén de imágenes y dependencias (Docker Hub) |

Los contenedores son **entornos operativos aislados y portátiles** que ejecutan aplicaciones sin afectar a otros contenedores ni al host. A diferencia de las máquinas virtuales, los contenedores **comparten el kernel del sistema operativo del host**, lo que reduce el consumo de recursos.

#### Contenedores vs Máquinas Virtuales

La diferencia arquitectónica es clave:

- **Máquinas Virtuales**: sobre la infraestructura hay un **Host Operating System**, encima un **Hypervisor**, y sobre él cada VM tiene su propio **Guest OS** completo, más los binarios/librerías y la aplicación. Cada VM es pesada (incluye un SO completo).
- **Contenedores**: sobre la infraestructura hay un **Operating System** compartido, encima un **Container Engine** (como Docker), y sobre él cada contenedor solo incluye los binarios/librerías y la aplicación, sin SO propio.

| Característica | Máquina Virtual | Contenedor |
|----------------|----------------|-----------|
| Aislamiento | Completo (SO propio) | A nivel de proceso (kernel compartido) |
| Tamaño | Gigabytes | Megabytes |
| Tiempo de inicio | Minutos | Segundos |
| Portabilidad | Limitada | Alta |
| Overhead | Alto | Bajo |
| Gestión | Hipervisor | Container Engine (Docker) |

### 4.5 Docker

**Docker** es un proyecto de **código abierto** que facilita el despliegue automatizado de aplicaciones en contenedores de software. Se basa en la biblioteca **libcontainer** y aprovecha las facilidades de virtualización del kernel Linux.

**Mecanismos del kernel Linux que usa Docker:**

Docker se apoya en la biblioteca `libcontainer`, que a su vez se comunica con el **Linux kernel** mediante: `cgroups` (control de recursos), `namespaces` (aislamiento de procesos, red, usuarios), `SELinux`/`AppArmor` (control de acceso), `Netlink`, `Netfilter` y `capabilities`. Esto permite ejecutar contenedores independientes en una sola instancia de Linux sin necesidad de un hipervisor.

**Ventajas de Docker:**
- Empaqueta aplicaciones y sus dependencias en contenedores virtuales ejecutables en cualquier servidor Linux.
- Brinda **flexibilidad y portabilidad**: el mismo contenedor funciona en desarrollo, pruebas y producción.
- Compatible con servicios en la nube: **AWS**, **Google Cloud Platform**, **Microsoft Azure**.
- Se integra con herramientas de automatización como **Ansible**, **Chef** y **Puppet**.

#### Comandos Docker esenciales

```bash
# Construir una imagen desde un Dockerfile
docker build -t nombre_imagen .

# Ejecutar un contenedor en segundo plano
docker run -d --name mi_contenedor nombre_imagen

# Ejecutar con política de reinicio ante fallo
docker run -d --restart=on-failure nombre_imagen

# Ejecutar siempre al reiniciar el daemon
docker run -d --restart=always nombre_imagen

# Crear una imagen a partir de un contenedor existente
docker commit mi_contenedor nueva_imagen

# Crear un volumen para persistir datos
docker volume create mis_datos

# Montar un volumen al lanzar un contenedor
docker run -d -v mis_datos:/app/data nombre_imagen

# Listar contenedores en ejecución
docker ps

# Listar todas las imágenes
docker images

# Exportar una imagen a un archivo tar
docker save -o imagen.tar nombre_imagen

# Cargar una imagen desde un archivo tar
docker load -i imagen.tar

# Exportar el sistema de ficheros de un contenedor
docker export mi_contenedor > contenedor.tar
```

#### Políticas de reinicio de Docker (`--restart`)

| Política | Comportamiento |
|----------|---------------|
| `no` (por defecto) | No reinicia el contenedor |
| `on-failure[:N]` | Reinicia solo si el código de salida es distinto de 0 (fallo) |
| `always` | Reinicia siempre, incluso si se paró manualmente o al reiniciar el daemon |
| `unless-stopped` | Como `always`, pero no reinicia si fue parado manualmente antes de reiniciar el daemon |

> **Dato de examen**: La sintaxis correcta usa **doble guion**: `--restart=on-failure`. Un solo guion (`-restart`) es incorrecto y provocará un error.

#### Subcomandos de volúmenes Docker

| Comando | Función |
|---------|---------|
| `docker volume create` | Crea un volumen |
| `docker volume ls` | Lista los volúmenes |
| `docker volume inspect` | Muestra detalles de un volumen |
| `docker volume rm` | Elimina un volumen |
| `docker volume prune` | Elimina todos los volúmenes no utilizados |

> **Dato de examen**: `docker volume mount`, `docker volume attach` y `docker volume detach` **no existen** como subcomandos de Docker.

#### docker commit vs docker save vs docker export

| Comando | Origen | Destino | Uso |
|---------|--------|---------|-----|
| `docker commit` | Contenedor | Imagen (con historial) | Capturar el estado de un contenedor como imagen |
| `docker save` | Imagen | Archivo tar (con capas) | Exportar imagen para mover entre sistemas |
| `docker load` | Archivo tar (de save) | Imagen | Importar imagen exportada con save |
| `docker export` | Contenedor | Archivo tar (sistema de ficheros plano) | Exportar el sistema de ficheros del contenedor |
| `docker import` | Archivo tar (de export) | Imagen (sin historial) | Importar sistema de ficheros como imagen |

### 4.6 Hipervisores: Tipo 1 vs Tipo 2

Un **hipervisor** (o monitor de máquina virtual) es el software que permite ejecutar múltiples máquinas virtuales sobre un único hardware físico.

#### Hipervisor Tipo 1 (Bare-metal / Nativo)

Se ejecuta **directamente sobre el hardware físico** del servidor, sin necesidad de un sistema operativo anfitrión. También llamado **bare-metal** o nativo.

**Ventajas:**
- Mayor rendimiento por acceso directo al hardware.
- Mayor seguridad (superficie de ataque reducida, sin SO subyacente).
- Menor latencia.

**Ejemplos:** VMware ESXi, Microsoft Hyper-V (como rol de servidor), Citrix XenServer, KVM.

#### Hipervisor Tipo 2 (Hosted / Alojado)

Se ejecuta como una **aplicación sobre un sistema operativo anfitrión (host)**. También llamado **hosted**.

**Ventajas:**
- Más fácil de instalar y configurar.
- Adecuado para desarrollo, pruebas y uso personal.

**Limitaciones:**
- Mayor overhead por la capa adicional del SO host.
- Menor rendimiento que el tipo 1.
- Menos adecuado para entornos de producción a gran escala.

**Ejemplos:** VMware Workstation, Oracle VirtualBox, Parallels Desktop, QEMU.

#### Tabla comparativa de hipervisores

| Característica | Tipo 1 (Bare-metal) | Tipo 2 (Hosted) |
|----------------|--------------------|--------------------|
| Instalación | Directamente sobre hardware | Sobre SO anfitrión |
| Otro nombre | Nativo, bare-metal | Alojado, hosted |
| Rendimiento | Alto (acceso directo) | Menor (overhead del SO) |
| Seguridad | Mayor | Menor |
| Uso típico | Centros de datos, producción | Desarrollo, pruebas, escritorio |
| Ejemplos | VMware ESXi, Hyper-V, XenServer | VirtualBox, VMware Workstation, Parallels |

> **Dato de examen**: Los hipervisores tipo 2 se llaman **"hosted"** (alojados). Los tipo 1 se llaman **"bare-metal"**. "Unhosted" no es una clasificación estándar.

### 4.7 Kubernetes

**Kubernetes (K8s)** es una plataforma de **orquestación de contenedores** de código abierto, desarrollada originalmente por Google. Se utiliza para desplegar, escalar y gestionar aplicaciones en contenedores de forma automatizada.

> **Importante**: Kubernetes gestiona los contenedores que se ejecutan sobre el sistema operativo, pero **no actualiza el sistema operativo del host subyacente**. Para actualizaciones de SO se usan herramientas como SCCM/Ansible.

#### Conceptos fundamentales de Kubernetes

| Concepto | Definición |
|----------|-----------|
| **Pod** | Unidad mínima desplegable. Contiene uno o más contenedores que comparten red y almacenamiento. Cada pod tiene una IP única. |
| **Nodo (Node)** | Máquina (física o virtual) que ejecuta pods. Puede ser un servidor o una instancia cloud. |
| **Clúster** | Conjunto de nodos coordinados. Tiene al menos un nodo maestro (control plane) y varios nodos worker. |
| **Deployment** | Objeto que gestiona el despliegue de una aplicación, incluyendo actualizaciones y reversiones sin interrupciones. |
| **Service** | Objeto que expone un pod o grupo de pods en la red, proporcionando un punto de acceso estable (aunque los pods cambien). |
| **Namespace** | Agrupación lógica de recursos dentro de un clúster (para multi-tenancy). |
| **ConfigMap / Secret** | Objetos para gestionar configuración y credenciales de forma externa a los contenedores. |

**Arquitectura de un clúster Kubernetes:**
- **Control Plane (nodo maestro)**: gestiona el clúster. Incluye el API Server, el Scheduler y el Controller Manager.
- **Nodos worker**: ejecutan los pods. Incluyen el agente `kubelet`, `kube-proxy` y el container runtime (como Docker o containerd).

**Diferencia clave**: SCCM y Ansible pueden actualizar sistemas operativos de servidores y estaciones de trabajo. Kubernetes **no puede** realizar actualizaciones del SO del host; su función es gestionar microservicios en contenedores.

---

## Resumen para el examen

- **SMTP usa el puerto 25** (entre servidores), 587 (envío autenticado) y 465 (SMTPS). **POP3 usa el puerto 110** (995 con TLS). **IMAP usa el puerto 143** (993 con TLS). Los puertos TCP estándar para POP y SMTP son **110 y 25**.
- **SMTP solo envía**; no puede recuperar correos del servidor. Para eso existen POP3 e IMAP.
- **POP3 descarga y elimina** del servidor por defecto; no sincroniza entre dispositivos. **IMAP sincroniza** y mantiene los mensajes en el servidor.
- El comando HELO puede sustituirse por **EHLO** (RFC 1869 / RFC 5321) para solicitar la lista de extensiones del servidor. Si no las soporta, responde con `500 Syntax error, command unrecognized`.
- **SPF** verifica la IP del servidor emisor mediante DNS. **DKIM** verifica la integridad del mensaje mediante firma criptográfica. **DMARC** define la política de actuación ante fallos de SPF o DKIM.
- En **SOA**, los servicios son **débilmente acoplados**: un fallo en un servicio no se propaga al resto. La reutilización del código es una ventaja, no una limitación.
- **SOAP** es el protocolo que realiza el intercambio de datos en XML en servicios web. **UDDI** es el catálogo ("páginas amarillas") de servicios web. **WSDL** es el lenguaje de descripción del servicio.
- **WS-Security es exclusiva de SOAP**, no de REST. REST usa HTTPS, OAuth 2.0 y JWT. Esta distinción aparece directamente en el banco de preguntas.
- **XMPP y SIMPLE son protocolos de mensajería instantánea basados en estándares abiertos** (no propietarios).
- Los **microservicios** dividen una aplicación en pequeños servicios independientes. Cada uno se encarga de una funcionalidad específica, lo que facilita la escalabilidad y el mantenimiento.
- Los **contenedores** comparten el kernel del SO host (a diferencia de las VMs, que tienen SO propio). Son más ligeros, arrancan en segundos y son altamente portátiles.
- **`docker commit`** crea una imagen a partir de un contenedor. **`docker volume create`** crea un volumen para persistir datos. La política de reinicio usa doble guion: **`--restart=on-failure`**.
- **Hipervisor tipo 1** (bare-metal): directamente sobre el hardware. Ejemplos: VMware ESXi, Hyper-V, XenServer. **Hipervisor tipo 2** (hosted): sobre un SO anfitrión. Ejemplos: VirtualBox, VMware Workstation.
- **Kubernetes** es una herramienta de **orquestación de contenedores**, no de actualización de SO. Para actualizar SO se usan SCCM o Ansible.
- La unidad mínima de Kubernetes es el **Pod**. Un **Deployment** gestiona el ciclo de vida de los pods. Un **Service** expone los pods en la red.
- **`docker volume mount`**, **`docker volume attach`** y **`docker volume detach`** no existen como subcomandos de Docker.

---

## Preguntas frecuentes en exámenes

El banco de 15 preguntas de este tema se distribuye así por subtemas:

### Subtema más frecuente: SOA y servicios web (5 preguntas)

Las preguntas abordan:
- **Ventaja de SOA**: "los fallos en un servicio no afectan a los demás" (bajo acoplamiento, tolerancia a fallos).
- **SOAP como protocolo de intercambio en XML**: SOAP es el estándar del W3C para mensajes XML en servicios web.
- **UDDI como catálogo de servicios web**: "páginas amarillas" para publicar y descubrir servicios.
- **Característica que NO corresponde a REST**: WS-Security es de SOAP, no de REST.
- **XMPP y SIMPLE son estándares abiertos** de mensajería instantánea.

Ejemplo representativo:
> "¿Cuál de las siguientes características NO corresponde a un servicio REST?"
> Respuesta correcta: "Se puede utilizar WS-Security para garantizar tanto la identificación del origen como la integridad y confidencialidad de la información" (WS-Security es del stack SOAP/WS-*, no de REST).

### Segundo subtema: Docker (4 preguntas)

Las preguntas abordan:
- **`docker run -d --restart=on-failure`**: sintaxis correcta para reinicio ante fallo (doble guion).
- **`docker commit`**: crea una imagen a partir de un contenedor.
- **`docker volume create`**: crea un volumen para persistir datos.
- Diferencia entre `docker save`, `docker export`, `docker load` e `docker import`.

### Tercer subtema: hipervisores y microservicios (3 preguntas)

- **Tipo 2 = Hosted**: los hipervisores de tipo 2 también se llaman "hosted" (alojados sobre un SO).
- **Arquitectura de microservicios**: divide la aplicación en pequeños servicios independientes que se comunican entre sí.
- **Kubernetes no actualiza el SO**: SCCM y Ansible sí pueden. Kubernetes solo orquesta contenedores.

### Cuarto subtema: puertos de correo (1 pregunta)

- Los puertos TCP estándar para POP y SMTP son **110 y 25** (no 80/25 ni 110/23).

---

## Referencias

- [SMTP — RFC 5321](https://www.rfc-editor.org/rfc/rfc5321) — Especificación oficial del protocolo SMTP.
- [POP3 — RFC 1939](https://www.rfc-editor.org/rfc/rfc1939) — Especificación oficial del protocolo POP3.
- [IMAP — RFC 3501](https://www.rfc-editor.org/rfc/rfc3501) — Especificación oficial del protocolo IMAP4.
- [DMARC, DKIM y SPF — Cloudflare](https://www.cloudflare.com/learning/email-security/dmarc-dkim-spf/) — Explicación clara de los tres mecanismos de autenticación de correo.
- [SOAP — W3C](https://www.w3.org/TR/soap12/) — Especificación oficial del protocolo SOAP.
- [WSDL — W3C](https://www.w3.org/TR/wsdl20/) — Especificación oficial del lenguaje WSDL.
- [UDDI — Wikipedia](https://es.wikipedia.org/wiki/UDDI) — Descripción del catálogo de servicios web UDDI.
- [SOA — IBM](https://www.ibm.com/think/topics/soa) — Qué es la Arquitectura Orientada a Servicios.
- [SOA — AWS](https://aws.amazon.com/what-is/service-oriented-architecture/) — Guía de AWS sobre SOA.
- [Docker — Documentación oficial](https://docs.docker.com/) — Referencia de comandos y conceptos Docker.
- [Docker restart policies](https://docs.docker.com/engine/containers/start-containers-automatically/) — Políticas de reinicio de contenedores.
- [Docker volumes](https://docs.docker.com/engine/storage/volumes/) — Gestión de volúmenes en Docker.
- [Kubernetes — Documentación oficial](https://kubernetes.io/es/docs/concepts/) — Conceptos fundamentales de Kubernetes.
- [Pods — Kubernetes](https://kubernetes.io/es/docs/concepts/workloads/pods/pod/) — Definición y uso de pods en Kubernetes.
- [Hipervisores tipo 1 vs tipo 2 — Pure Storage](https://www.purestorage.com/la/knowledge/type-1-vs-type-2-hypervisor.html) — Comparativa detallada de tipos de hipervisores.
- [Hipervisores — Red Hat](https://www.redhat.com/en/topics/virtualization/what-is-a-hypervisor) — Qué es un hipervisor y sus tipos.
- [Microservicios — Martin Fowler](https://martinfowler.com/articles/microservices.html) — Artículo fundacional sobre arquitectura de microservicios.
- [XMPP — RFC 6120](https://www.rfc-editor.org/rfc/rfc6120) — Especificación del protocolo XMPP.
