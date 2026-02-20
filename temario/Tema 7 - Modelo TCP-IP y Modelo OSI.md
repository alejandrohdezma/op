# Tema 7: El Modelo TCP/IP y el Modelo OSI

> Este tema aborda los dos marcos de referencia fundamentales para entender las comunicaciones en red: el **Modelo OSI** de ISO (7 capas) y el **Modelo TCP/IP** del DoD/IETF (4 capas). Se estudian en profundidad las capas, sus PDUs, los protocolos asociados (IP, TCP, UDP, ICMP, ARP, DNS, DHCP, HTTP, FTP...), el direccionamiento IPv4/IPv6, la división en subredes (CIDR/VLSM), NAT y los protocolos de enrutamiento. En la oposición es uno de los temas más prolíficos en preguntas: el banco de examen asigna 25 preguntas clasificadas aquí, con especial peso en subredes CIDR, puertos conocidos, PDUs por capa OSI y el papel de protocolos como ARP, DNS, ICMP, TCP o UDP. El estudiante debe dominar los conceptos teóricos y ser capaz de realizar cálculos de subredes, identificar protocolos por puerto y distinguir con precisión las capas a las que pertenece cada protocolo.

---

## 1. El Modelo OSI

### 1.1 ¿Qué es el Modelo OSI?

El **Modelo OSI** (Open Systems Interconnection) fue desarrollado por la **ISO** (International Organization for Standardization) en **1984** como una arquitectura de referencia para la comunicación entre sistemas heterogéneos.

Su objetivo es permitir que equipos de distintos fabricantes y sistemas operativos puedan interoperar bajo un conjunto de normas internacionales comunes, facilitando la interoperabilidad sin depender de implementaciones propietarias.

**Principios clave del Modelo OSI:**

- Las funciones de red se dividen en **7 capas**, cada una con una tarea específica.
- Cada capa se comunica únicamente con la capa inmediatamente superior e inferior.
- Las capas superiores interactúan con las aplicaciones de usuario; las inferiores gestionan la transmisión física.
- Cada capa puede implementarse de forma independiente (modularidad).
- Es un modelo **teórico de referencia**; en la práctica, el modelo TCP/IP es el que realmente se usa en Internet.

> **Nota de examen:** La pregunta "¿Cuál es la principal diferencia entre OSI y TCP/IP en cuanto a capas?" tiene respuesta clara: OSI tiene 7 capas, TCP/IP tiene 4 (o 5). OSI **siempre** tiene más capas.

---

### 1.2 Las 7 capas del Modelo OSI

La siguiente tabla resume las capas, su PDU (**Protocol Data Unit**, la unidad de datos que maneja cada capa), su función principal y los protocolos o tecnologías más representativos.

| Capa | Nombre | PDU | Función principal | Protocolos / Tecnologías |
|:---:|:---|:---:|:---|:---|
| 7 | Aplicación | Datos | Interfaz con el usuario final; servicios de red | HTTP, FTP, SMTP, DNS, DHCP, SNMP, SSH, Telnet, LDAP |
| 6 | Presentación | Datos | Traducción, cifrado y compresión de datos | SSL/TLS, MIME, JPEG, MPEG, ASCII, EBCDIC |
| 5 | Sesión | Datos | Establecimiento, gestión y cierre de sesiones | RPC, NetBIOS, SIP |
| 4 | Transporte | Segmento (TCP) / Datagrama (UDP) | Comunicación fiable extremo a extremo, puertos | TCP, UDP |
| 3 | Red | **Paquete** | Enrutamiento y direccionamiento lógico | IP, ICMP, IGMP, RIP, OSPF, BGP |
| 2 | Enlace de datos | **Trama** | Comunicación dentro de la LAN, direcciones MAC | Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11), PPP |
| 1 | Física | **Bits** | Transmisión de bits por el medio físico | Cables de cobre, fibra óptica, ondas de radio, conectores |

> **Regla mnemotécnica** (de arriba a abajo): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing → Aplicación, Presentación, Sesión, Transporte, Red (_Network_), Datos (_Data Link_), Física (_Physical_).

---

#### 1.2.1 Capa 7 — Aplicación

**Función principal:** Proporciona servicios de red directamente a las aplicaciones de usuario. Es la capa más cercana al usuario final.

La capa de Aplicación **no** proporciona los servicios de red en sí misma, sino que facilita que las aplicaciones utilicen la red. Cada protocolo de esta capa está diseñado para un servicio concreto.

**Protocolos representativos:**

- **HTTP** (puerto 80) y **HTTPS** (puerto 443): transferencia de páginas web. RFC 7230/9110.
- **FTP** (puertos 20/21): transferencia de archivos. RFC 959.
- **SMTP** (puerto 25): envío de correo electrónico. RFC 5321.
- **DNS** (puerto 53): resolución de nombres de dominio. RFC 1034/1035.
- **DHCP** (puertos 67/68): asignación dinámica de parámetros de red. RFC 2131.
- **SSH** (puerto 22): acceso remoto seguro cifrado. RFC 4251.
- **Telnet** (puerto 23): acceso remoto interactivo, sin cifrado. RFC 854.
- **SNMP** (puerto 161): gestión de dispositivos de red. RFC 1157.
- **LDAP** (puerto 389): acceso a servicios de directorio. RFC 4511.
- **NTP** (puerto 123): sincronización de tiempo. RFC 5905.
- **POP3** (puerto 110): recepción de correo electrónico. RFC 1939.
- **IMAP** (puerto 143): acceso a buzón de correo. RFC 3501.

> **Pregunta frecuente:** "¿Qué protocolo NO pertenece a la capa de aplicación?" → **ARP** es la respuesta típica, ya que opera en la frontera entre la capa de red y la de enlace.

---

#### 1.2.2 Capa 6 — Presentación

**Función principal:** Se encarga de la traducción del formato de los datos entre la red y la aplicación. Opera como "traductor" entre distintas representaciones de datos.

**Funciones específicas:**
- **Traducción de datos:** convierte formatos (por ejemplo, de EBCDIC a ASCII).
- **Cifrado/descifrado:** protege la información durante la transmisión (SSL/TLS).
- **Compresión de datos:** reduce el tamaño para mejorar la velocidad (JPEG, MPEG, ZIP).

**Protocolos y formatos:**
- **SSL/TLS** — seguridad en la capa de transporte (en implementaciones modernas, TLS opera realmente entre las capas 4 y 7).
- **MIME** — extensiones multimedia para correo electrónico.

---

#### 1.2.3 Capa 5 — Sesión

**Función principal:** Controla el establecimiento, mantenimiento y terminación ordenada de sesiones de comunicación entre aplicaciones.

**Funciones específicas:**
- **Establecimiento de sesión:** inicia y sincroniza las conexiones.
- **Gestión de la sesión:** controla la comunicación a lo largo de toda la sesión, incluidos puntos de control (checkpoints) para reanudar tras un fallo.
- **Cierre de sesión:** finaliza las sesiones de forma ordenada.

**Protocolos:**
- **RPC** (Remote Procedure Call): permite ejecutar procedimientos en sistemas remotos.
- **NetBIOS**: comunicación entre aplicaciones en red local Windows.

---

#### 1.2.4 Capa 4 — Transporte

**Función principal:** Proporciona comunicación fiable (o no fiable) extremo a extremo entre procesos de aplicación, independientemente de la red subyacente.

**Características clave:**
- **Segmentación:** divide los datos en unidades manejables (segmentos/datagramas).
- **Control de flujo:** regula la cantidad de datos enviados sin saturar al receptor.
- **Control de errores:** verifica la integridad de los datos (checksums).
- **Multiplexación mediante puertos:** identifica cada proceso de aplicación con un número de puerto.

**Protocolos:**

| Característica | **TCP** | **UDP** |
|:---|:---:|:---:|
| RFC | RFC 9293 (antes RFC 793) | RFC 768 |
| Orientado a conexión | Sí (handshake 3 vías) | No |
| Fiabilidad | Garantizada (ACKs + retransmisión) | No garantizada |
| Orden de entrega | Garantizado | No garantizado |
| Control de flujo | Sí (ventana deslizante) | No |
| Control de congestión | Sí | No |
| Cabecera | 20 bytes mínimo | 8 bytes |
| Velocidad | Menor (mayor overhead) | Mayor (menor overhead) |
| Uso típico | HTTP, FTP, SMTP, SSH | DNS, DHCP, TFTP, streaming, VoIP |

> **PDU de capa 4:** **segmento** en TCP, **datagrama** en UDP. No confundir con el término "datagrama IP" de la capa de red.

---

#### 1.2.5 Capa 3 — Red

**Función principal:** Determina cómo los datos se envían de una red a otra, asignando direcciones lógicas (IP) y seleccionando rutas.

**Características clave:**
- **Enrutamiento:** selección de la mejor ruta entre el origen y el destino.
- **Direccionamiento lógico:** direcciones IP únicas para cada dispositivo.
- **Fragmentación de paquetes:** divide paquetes grandes para adaptarlos a la MTU del enlace.

**PDU: Paquete** (también llamado "datagrama IP").

**Protocolos:**
- **IP** (IPv4 — RFC 791, IPv6 — RFC 8200): direccionamiento y entrega de paquetes.
- **ICMP** (RFC 792): mensajes de control y errores (ping, traceroute).
- **ICMPv6** (RFC 4443): equivalente de ICMP para IPv6, incluye NDP.
- **ARP** (RFC 826): resolución de IP a MAC (capa 3/2).
- **IGMP**: gestión de grupos multicast.
- Protocolos de enrutamiento: RIP, OSPF, BGP.

---

#### 1.2.6 Capa 2 — Enlace de datos

**Función principal:** Comunicación dentro de la red local (LAN), detección y corrección de errores en la capa física.

**Características clave:**
- **Encapsulación de tramas:** agrupa datos en tramas con cabecera y cola.
- **Direccionamiento MAC:** identifica dispositivos dentro de una red local.
- **Control de errores:** detecta errores mediante CRC/FCS.
- **Control de acceso al medio (MAC sublayer):** gestiona el acceso compartido al medio.

**PDU: Trama** (frame).

**Protocolos y estándares:**
- **Ethernet** (IEEE 802.3): redes locales cableadas.
- **Wi-Fi** (IEEE 802.11): redes locales inalámbricas.
- **Bluetooth** (IEEE 802.15): redes personales de corto alcance.
- **PPP**: punto a punto.

---

#### 1.2.7 Capa 1 — Física

**Función principal:** Define los medios y métodos para la transmisión física de bits a través del medio de comunicación.

**Características clave:**
- **Señalización:** cómo se codifican los bits en señales eléctricas, ópticas o de radio.
- **Medios de transmisión:** cables de cobre (par trenzado, coaxial), fibra óptica, ondas de radio.
- **Especificaciones físicas:** conectores, voltajes, velocidad de transmisión, distancias máximas.

**PDU: Bits.**

---

### 1.3 Encapsulación y desencapsulación en OSI

Cuando los datos viajan de la capa de aplicación hacia el medio físico, cada capa añade su propia cabecera (y en la capa 2 también un trailer). Este proceso se llama **encapsulación**. En el receptor, el proceso inverso se llama **desencapsulación**.

```
Emisor (de arriba a abajo):
┌─────────────────────────────────────────────────────────────────┐
│ Capa 7 (Aplicación)   │ DATOS                                   │
├───────────────────────┼─────────────────────────────────────────┤
│ Capa 6 (Presentación) │ H6 │ DATOS                              │
├───────────────────────┼────┴────────────────────────────────────┤
│ Capa 5 (Sesión)       │ H5 │ H6 │ DATOS                         │
├───────────────────────┼────┴────┴────────────────────────────────┤
│ Capa 4 (Transporte)   │ H4(TCP/UDP) │ H5│H6│ DATOS  → SEGMENTO │
├───────────────────────┼─────────────┴───┴──┴────────────────────┤
│ Capa 3 (Red)          │ H3(IP) │ H4│H5│H6│ DATOS  → PAQUETE    │
├───────────────────────┼────────┴───┴──┴──┴────────────────────── ┤
│ Capa 2 (Enlace)       │ H2(MAC)│ H3│H4│...│DATOS│FCS→ TRAMA    │
├───────────────────────┼────────────────────────────────────────── ┤
│ Capa 1 (Física)       │ 010110100101001... → BITS               │
└─────────────────────────────────────────────────────────────────┘

Receptor (de abajo a arriba): proceso inverso — cada capa
elimina su cabecera y pasa el contenido a la capa superior.
```

---

## 2. El Modelo TCP/IP

### 2.1 Origen e historia

El **Modelo TCP/IP** (también llamado **pila de protocolos de Internet** o **suite TCP/IP**) fue desarrollado por el Departamento de Defensa de EE. UU. (DoD) a través de la ARPA (Advanced Research Projects Agency). Su nombre proviene de sus dos protocolos más importantes: **TCP** (Transmission Control Protocol) e **IP** (Internet Protocol).

> **Pregunta frecuente en examen:** "TCP/IP proviene de los nombres de tres protocolos" → **FALSO**. Proviene de exactamente **dos**: TCP e IP.

Los estándares TCP/IP se definen en documentos públicos llamados **RFC** (Request for Comments), mantenidos y publicados por la **IETF** (Internet Engineering Task Force, [ietf.org](https://www.ietf.org)).

### 2.2 Las 4 capas del Modelo TCP/IP

| Capa TCP/IP | Equivalente OSI | Función | Protocolos |
|:---|:---|:---|:---|
| 4. Aplicación | Capas 5, 6 y 7 | Servicios al usuario | HTTP, FTP, SMTP, DNS, DHCP, SSH, Telnet, SNMP, LDAP |
| 3. Transporte | Capa 4 | Comunicación extremo a extremo | TCP, UDP |
| 2. Internet | Capa 3 | Direccionamiento y enrutamiento | IP, ICMP, ARP, IGMP |
| 1. Acceso a red | Capas 1 y 2 | Transmisión física en la red local | Ethernet, Wi-Fi, PPP |

> Algunos textos presentan el modelo TCP/IP con **5 capas** separando la Física y el Enlace de datos. Ambas representaciones son válidas; la de 4 capas es la original.

### 2.3 Comparativa OSI vs TCP/IP

```
Modelo OSI (7 capas)          Modelo TCP/IP (4 capas)
┌──────────────────────┐      ┌──────────────────────┐
│ 7. Aplicación        │      │                      │
│ 6. Presentación      │ ──►  │ 4. Aplicación        │
│ 5. Sesión            │      │                      │
├──────────────────────┤      ├──────────────────────┤
│ 4. Transporte        │ ──►  │ 3. Transporte        │
├──────────────────────┤      ├──────────────────────┤
│ 3. Red               │ ──►  │ 2. Internet          │
├──────────────────────┤      ├──────────────────────┤
│ 2. Enlace de datos   │      │                      │
│ 1. Física            │ ──►  │ 1. Acceso a red      │
└──────────────────────┘      └──────────────────────┘
```

**Diferencias clave:**
- OSI es un **modelo teórico/de referencia**; TCP/IP es el modelo **práctico** que usa Internet.
- OSI tiene **7 capas**; TCP/IP tiene **4** (o 5 en la variante extendida).
- TCP/IP fusiona sesión + presentación + aplicación en una sola capa de Aplicación.
- TCP/IP fusiona física + enlace en la capa de Acceso a red.
- TCP/IP **no es un modelo propietario**: está definido en RFCs públicas mantenidas por la IETF.

---

## 3. Otras pilas de protocolos (contexto histórico)

Antes de que TCP/IP se impusiera como estándar universal, existían varias pilas de protocolos propietarias que competían entre sí. En las oposiciones pueden aparecer como "respuestas incorrectas" en preguntas de contexto, así que conviene saber qué son y por qué quedaron obsoletas.

- **NetBIOS/NetBEUI** — Desarrollado por IBM y Microsoft para redes pequeñas locales. Su gran limitación: **no es enrutable**, así que no puede salir de la LAN. Hoy está prácticamente en desuso (aunque NetBIOS sobrevivió encapsulado sobre TCP/IP durante años en Windows).
- **IPX/SPX** — Protocolo propietario de Novell para sus servidores NetWare. IPX hace las veces de IP (capa de red) y SPX hace las veces de TCP (transporte fiable). Era habitual en los 90 en redes corporativas, pero fue desplazado por TCP/IP.
- **TCP/IP** — El ganador. Estándar abierto, enrutable, escalable y mantenido por la IETF a través de RFCs públicas. Es la base de Internet y de prácticamente toda red moderna.

> La pregunta de examen típica con estas pilas pregunta cuál "no es enrutable" (NetBEUI) o cuál era el protocolo de red de Novell (IPX).

---

## 4. Capa de Red: El Protocolo IP

### 4.1 IPv4

**IPv4** ([RFC 791](https://www.rfc-editor.org/rfc/rfc791)) es la versión 4 del Protocolo de Internet. Usa **direcciones de 32 bits** (4 octetos), escritas en notación decimal separada por puntos (por ejemplo, `192.168.1.10`).

#### 4.1.1 Estructura de la cabecera IPv4

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
├─────────┬────────┬──────────────────┬──────────────────────────┤
│ Versión │  IHL  │  Tipo de servicio│     Longitud total        │
├─────────┴────────┼──────────────────┼───┬──────────────────────┤
│    Identificador │    Flags         │   │  Desplazamiento       │
├──────────────────┼──────────────────┼───┴──────────────────────┤
│       TTL        │    Protocolo     │   Checksum de cabecera    │
├──────────────────┴──────────────────┴──────────────────────────┤
│                    Dirección IP de origen (32 bits)             │
├────────────────────────────────────────────────────────────────┤
│                    Dirección IP de destino (32 bits)            │
├────────────────────────────────────────────────────────────────┤
│              Opciones (si IHL > 5)     │       Relleno          │
└────────────────────────────────────────────────────────────────┘
```

Campos clave:
- **TTL** (Time To Live): número máximo de saltos (routers) que puede atravesar el paquete. Cada router lo decrementa en 1; si llega a 0, se descarta y se envía un mensaje ICMP "Time Exceeded".
- **Protocolo:** identifica el protocolo de la capa superior (1 = ICMP, 6 = TCP, 17 = UDP).
- **Flags:** bit "Don't Fragment" (DF) y "More Fragments" (MF) para fragmentación.

#### 4.1.2 Clases de direcciones IPv4 (direccionamiento classful)

El direccionamiento **classful** es el sistema original de clases de red. Fue reemplazado por CIDR en 1993 ([RFC 1518](https://www.rfc-editor.org/rfc/rfc1518)), pero sigue siendo materia de examen.

| Clase | Rango del 1er octeto | Rango de IPs | Máscara por defecto | Parte de red | Parte de host | Uso |
|:---:|:---:|:---|:---:|:---:|:---:|:---|
| **A** | 1–126 | 1.0.0.0 – 126.255.255.255 | /8 (255.0.0.0) | **1 byte** (8 bits) | 3 bytes (24 bits) | Redes muy grandes |
| **B** | 128–191 | 128.0.0.0 – 191.255.255.255 | /16 (255.255.0.0) | **2 bytes** (16 bits) | 2 bytes (16 bits) | Redes medianas |
| **C** | 192–223 | 192.0.0.0 – 223.255.255.255 | /24 (255.255.255.0) | **3 bytes** (24 bits) | 1 byte (8 bits) | Redes pequeñas |
| **D** | 224–239 | 224.0.0.0 – 239.255.255.255 | N/A | — | — | **Multicast** |
| **E** | 240–255 | 240.0.0.0 – 255.255.255.255 | N/A | — | — | **Experimental** |

> **Pregunta frecuente:** "Una dirección IPv4 de clase B, la parte de red es de..." → **2 bytes** (16 bits). La máscara por defecto es /16 (255.255.0.0).

#### 4.1.3 Direcciones especiales IPv4

| Rango | Tipo | RFC |
|:---|:---|:---:|
| 10.0.0.0/8 | Privado | RFC 1918 |
| 172.16.0.0/12 | Privado | RFC 1918 |
| 192.168.0.0/16 | Privado | RFC 1918 |
| 127.0.0.0/8 | Loopback (localhost) | RFC 5735 |
| 169.254.0.0/16 | APIPA / Link-local | RFC 3927 |
| 224.0.0.0/4 | Multicast (clase D) | RFC 5771 |
| 255.255.255.255 | Broadcast limitado | — |

> **Pregunta frecuente:** Para asignar una IP pública a un servidor web se necesita una IP fuera de los rangos RFC 1918 y de los rangos especiales. El rango 169.254.0.0/16 (APIPA) **no es RFC 1918** y no se usa para diseño de redes organizativas.

#### 4.1.4 Subredes y CIDR

**CIDR** (Classless Inter-Domain Routing, [RFC 4632](https://www.rfc-editor.org/rfc/rfc4632)) permite definir máscaras de red de longitud variable, liberando del sistema de clases. La notación CIDR expresa la máscara como un sufijo `/n` donde `n` es el número de bits de red.

**Fórmula fundamental:**
```
Hosts utilizables = 2^(32 - n) - 2

donde n es la longitud del prefijo (por ejemplo, /24 → n=24, bits de host=8)
```

Se restan 2 porque la **dirección de red** (bits de host todos a 0) y el **broadcast** (bits de host todos a 1) no son asignables a hosts.

**Tabla de prefijos frecuentes:**

| Prefijo | Máscara | Hosts utilizables | Descripción |
|:---:|:---|:---:|:---|
| /8 | 255.0.0.0 | 16.777.214 | Clase A por defecto |
| /16 | 255.255.0.0 | 65.534 | Clase B por defecto |
| /20 | 255.255.240.0 | 4.094 | Subred mediana-grande |
| /22 | 255.255.252.0 | 1.022 | Subred mediana |
| /24 | 255.255.255.0 | 254 | Clase C por defecto, muy habitual |
| /25 | 255.255.255.128 | 126 | Mitad de una /24 |
| /26 | 255.255.255.192 | 62 | Un cuarto de /24 |
| /28 | 255.255.255.240 | 14 | Subred pequeña |
| /30 | 255.255.255.252 | 2 | Enlace punto a punto |

**Ejemplo de cálculo de subred — paso a paso:**

Dado `10.40.7.5/25`:
```
1. Máscara /25 → 255.255.255.128 → último octeto: 10000000 (128)
2. AND con IP: 5 (00000101) AND 128 (10000000) = 0 → red: 10.40.7.0
3. Broadcast: dirección de red + todos bits de host a 1:
   0 OR 127 (01111111) = 127 → broadcast: 10.40.7.127
4. Hosts válidos: 10.40.7.1 a 10.40.7.126 (126 hosts)
```

**VLSM** (Variable Length Subnet Masking): extensión de CIDR que permite usar máscaras de diferente longitud dentro de la misma red. Se usa para dividir un bloque de IPs en subredes de distintos tamaños según las necesidades.

**Ejemplo VLSM — dividir 10.90.80.0/24 en 4 subredes iguales:**
```
Se toman 2 bits del campo de host (/24 + 2 = /26)
- Subred 1: 10.90.80.0/26    → hosts .1–.62,    broadcast .63
- Subred 2: 10.90.80.64/26   → hosts .65–.126,   broadcast .127
- Subred 3: 10.90.80.128/26  → hosts .129–.190,  broadcast .191
- Subred 4: 10.90.80.192/26  → hosts .193–.254,  broadcast .255
```

**Elección de máscara mínima para un número de hosts dado:**

| Hosts requeridos | Prefijo mínimo | Hosts disponibles |
|:---:|:---:|:---:|
| ≤ 14 | /28 | 14 |
| ≤ 62 | /26 | 62 |
| ≤ 126 | /25 | 126 |
| ≤ 254 | /24 | 254 |
| ≤ 1.022 | /22 | 1.022 |
| ≤ 4.094 | /20 | 4.094 |
| ≤ 65.534 | /16 | 65.534 |

---

#### 4.1.5 ARP (Address Resolution Protocol)

**ARP** ([RFC 826](https://www.rfc-editor.org/rfc/rfc826)) resuelve una dirección IP en una dirección MAC dentro de la misma red local.

**Proceso ARP:**
```
1. Host A quiere comunicarse con 192.168.1.20 (en la misma LAN)
2. Host A envía ARP Request en BROADCAST:
   "¿Quién tiene la IP 192.168.1.20? Dígame su MAC"
3. Host B (con IP 192.168.1.20) responde con ARP Reply en UNICAST:
   "Soy yo. Mi MAC es AA:BB:CC:DD:EE:FF"
4. Host A guarda la asociación IP→MAC en su caché ARP
```

| Protocolo | Resolución | RFC |
|:---|:---|:---:|
| **ARP** | IP → MAC | RFC 826 |
| **RARP** (obsoleto) | MAC → IP | RFC 903 |
| **NDP** (IPv6) | IP → MAC (sustituto de ARP) | RFC 4861 |

> **Importante:** ARP opera en la frontera entre la capa de red y la de enlace. NO pertenece a la capa de aplicación. En IPv6, ARP es reemplazado por el **NDP** (Neighbor Discovery Protocol), parte de ICMPv6.

**ARP Spoofing:** ataque en el que un actor malicioso envía respuestas ARP falsas para asociar su MAC a la IP de otro equipo (como el gateway), habilitando ataques Man-in-the-Middle.

---

#### 4.1.6 ICMP (Internet Control Message Protocol)

**ICMP** ([RFC 792](https://www.rfc-editor.org/rfc/rfc792)) permite a los dispositivos de red reportar errores y enviar mensajes de control sobre el funcionamiento de la red IP.

- Los mensajes ICMP se encapsulan dentro de datagramas IP.
- El número de protocolo IP para ICMP es el **1**.
- ICMP **no es un protocolo de transporte**; no tiene puertos.

**Tipos de mensajes ICMP más importantes:**

| Tipo | Nombre | Uso |
|:---:|:---|:---|
| 0 | Echo Reply | Respuesta a ping |
| 3 | Destination Unreachable | Destino inaccesible |
| 5 | Redirect | Informa de mejor ruta |
| 8 | Echo Request | Solicitud de ping |
| 11 | Time Exceeded | TTL expirado (traceroute) |
| 12 | Parameter Problem | Error en cabecera IP |

**Herramientas que usan ICMP:**
- `ping`: envía ICMP Echo Request (tipo 8) y espera Echo Reply (tipo 0).
- `traceroute`/`tracert`: envía paquetes con TTL creciente; cuando el TTL llega a 0 en un router, este envía ICMP Time Exceeded (tipo 11) al origen. En Windows (`tracert`) los sondas son ICMP; en Linux/macOS (`traceroute`) son UDP por defecto.

> **Pregunta frecuente:** "¿Qué protocolo permite reportar al origen errores en el procesamiento de datagramas IP?" → **ICMP**.

---

### 4.2 IPv6

**IPv6** ([RFC 8200](https://www.rfc-editor.org/rfc/rfc8200), anteriormente RFC 2460) es la versión 6 del Protocolo de Internet, diseñada para sustituir a IPv4 ante el agotamiento del espacio de direcciones.

**Características principales de IPv6:**
- **Direcciones de 128 bits** (frente a 32 bits de IPv4): espacio prácticamente inagotable (2¹²⁸ ≈ 3,4 × 10³⁸ direcciones).
- **Notación:** grupos de 4 dígitos hexadecimales separados por `:`. Ejemplo: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.
- **Simplificaciones de notación:**
  - Omitir ceros a la izquierda: `0db8` → `db8`.
  - Comprimir grupos consecutivos de ceros con `::` (solo una vez): `2001:db8::8a2e:370:7334`.
- **Sin necesidad de NAT:** el enorme espacio de direcciones permite asignar IPs públicas únicas globalmente.
- **IPsec integrado** como parte del estándar (aunque opcional en la práctica).
- **Autoconfiguración sin estado (SLAAC):** los dispositivos pueden configurarse automáticamente sin DHCP ([RFC 4862](https://www.rfc-editor.org/rfc/rfc4862)).
- **Cabecera simplificada:** 40 bytes fijos (sin fragmentación en routers, sin checksum en la cabecera).

#### 4.2.1 Tipos de direcciones IPv6 (RFC 4291)

| Tipo | Descripción | Prefijo |
|:---|:---|:---|
| **Unicast global** | Equivalente a la IP pública IPv4, enrutable en Internet | `2000::/3` |
| **Unicast link-local** | Válida solo en el enlace local, equivalente a APIPA | `FE80::/10` |
| **Unicast unique-local** | Equivalente a las IPs privadas RFC 1918 | `FC00::/7` |
| **Multicast** | Un paquete se entrega a un grupo de interfaces | `FF00::/8` |
| **Anycast** | Se entrega a la interfaz más cercana del grupo | (toma del espacio unicast) |
| **Loopback** | Equivalente a 127.0.0.1 | `::1/128` |
| **No especificada** | Sin dirección asignada aún | `::/128` |

> **Nota:** IPv6 **no tiene broadcast**. El broadcast se sustituye por multicast específico (por ejemplo, `FF02::1` para todos los nodos del enlace).

#### 4.2.2 Ventajas de IPv6 sobre IPv4

| Característica | IPv4 | IPv6 |
|:---|:---:|:---:|
| Tamaño de dirección | 32 bits | 128 bits |
| Espacio de direcciones | ~4.300 millones | ~3,4 × 10³⁸ |
| Configuración automática | DHCP (con estado) | SLAAC (sin estado) o DHCPv6 |
| Fragmentación | En routers y hosts | Solo en hosts origen |
| Broadcast | Sí | No (sustituido por multicast) |
| IPsec | Opcional | Integrado en el estándar |
| Cabecera | Variable (20–60 bytes) | Fija (40 bytes) |
| NAT | Necesario | Opcional (no necesario) |
| ARP | Sí | No (sustituido por NDP/ICMPv6) |

---

## 5. Capa de Transporte en detalle

### 5.1 TCP — Transmission Control Protocol

**TCP** ([RFC 9293](https://datatracker.ietf.org/doc/html/rfc9293), que reemplaza al histórico [RFC 793](https://datatracker.ietf.org/doc/html/rfc793) de 1981) es el protocolo de transporte orientado a conexión y fiable de la suite TCP/IP.

#### 5.1.1 Three-Way Handshake (establecimiento de conexión)

Antes de enviar datos, TCP establece una conexión mediante el **handshake de tres vías**:

```
   Cliente                                 Servidor
      │                                        │
      │──── SYN (seq=x) ──────────────────────►│  Paso 1: cliente solicita conexión
      │                                        │
      │◄─── SYN-ACK (seq=y, ack=x+1) ──────────│  Paso 2: servidor acepta y sincroniza
      │                                        │
      │──── ACK (ack=y+1) ─────────────────────►│  Paso 3: cliente confirma
      │                                        │
      │           CONEXIÓN ESTABLECIDA         │
      │ ◄══════ Transferencia de datos ══════► │
      │                                        │
      │──── FIN ──────────────────────────────►│  Cierre de conexión (4 vías)
      │◄─── FIN-ACK ────────────────────────── │
```

- **SYN** (Synchronize): el cliente propone un número de secuencia inicial (ISN).
- **SYN-ACK**: el servidor acepta y propone su propio ISN.
- **ACK** (Acknowledgement): el cliente confirma.

Tras el handshake, ambos extremos están en estado **ESTABLISHED** y puede comenzar la transferencia de datos.

#### 5.1.2 Control de flujo y congestión

- **Control de flujo (ventana deslizante):** el receptor anuncia en cada ACK cuántos bytes puede recibir (tamaño de ventana). El emisor no puede enviar más datos de los que cabe en la ventana.
- **Control de congestión** ([RFC 5681](https://www.rfc-editor.org/rfc/rfc5681)): mecanismos para evitar saturar la red:
  - **Slow Start:** comienza con una ventana de congestión pequeña y la dobla con cada ACK.
  - **Congestion Avoidance:** crece linealmente cuando se detecta pérdida.
  - **Fast Retransmit / Fast Recovery:** detecta pérdidas antes de que expire el temporizador.

#### 5.1.3 Cierre de conexión TCP

El cierre de una conexión TCP usa **4 mensajes** (four-way handshake):
```
Cliente ──FIN──► Servidor
Cliente ◄──ACK── Servidor
Cliente ◄──FIN── Servidor
Cliente ──ACK──► Servidor
```
Cada extremo puede cerrar su lado de la conexión de forma independiente (half-close).

---

### 5.2 UDP — User Datagram Protocol

**UDP** ([RFC 768](https://www.rfc-editor.org/rfc/rfc768)) es el protocolo de transporte sin conexión y sin garantías de entrega.

**Características:**
- **No orientado a conexión:** no hay handshake previo.
- **No fiable:** no hay acuses de recibo ni retransmisiones.
- **No garantiza el orden** de entrega de los datagramas.
- **Sin control de flujo ni de congestión.**
- **Cabecera de solo 8 bytes** (muy eficiente).
- **Baja latencia:** ideal cuando la velocidad prima sobre la fiabilidad.

**Cabecera UDP:**
```
 0               1               2               3
 ┌───────────────┬───────────────┬───────────────┬───────────────┐
 │ Puerto origen │ Puerto destino│   Longitud    │   Checksum    │
 └───────────────┴───────────────┴───────────────┴───────────────┘
         (16 bits)      (16 bits)     (16 bits)      (16 bits)
```

**Protocolos que usan UDP:**

| Protocolo | Puerto | Uso |
|:---|:---:|:---|
| DNS | 53 | Resolución de nombres (consultas) |
| DHCP | 67 (servidor) / 68 (cliente) | Asignación dinámica de IP |
| **TFTP** | 69 | Transferencia simple de archivos (RFC 1350) |
| NTP | 123 | Sincronización de tiempo |
| SNMP | 161 / 162 | Gestión de dispositivos de red |
| RTP | Variable | Streaming de audio/vídeo en tiempo real |
| QUIC | Variable | HTTP/3 |

> **Pregunta frecuente:** "¿Cuál es una afirmación correcta sobre UDP?" → "Es usado por TFTP". TFTP usa UDP en el puerto 69 porque necesita ser simple y ligero (RFC 1350).

---

## 6. Puertos bien conocidos (Well-Known Ports)

Los **puertos** son identificadores numéricos (0–65535) que permiten distinguir diferentes servicios en un mismo host. La **IANA** ([iana.org](https://www.iana.org/assignments/service-names-port-numbers)) gestiona la asignación oficial.

**Rangos:**
- **0–1023:** Well-Known Ports (puertos bien conocidos, requieren privilegios de administrador).
- **1024–49151:** Registered Ports (puertos registrados).
- **49152–65535:** Dynamic/Private Ports (puertos dinámicos o efímeros).

**Tabla de puertos esenciales para el examen:**

| Puerto | Protocolo | Capa | Descripción |
|:---:|:---|:---:|:---|
| 20 | FTP-Data | TCP | Transferencia de datos FTP |
| 21 | FTP-Control | TCP | Control de sesión FTP |
| 22 | SSH | TCP | Acceso remoto seguro (cifrado) |
| 23 | Telnet | TCP | Acceso remoto interactivo (sin cifrar) |
| 25 | SMTP | TCP | Envío de correo electrónico |
| 53 | DNS | TCP/UDP | Resolución de nombres de dominio |
| 67 | DHCP (servidor) | UDP | Servidor DHCP |
| 68 | DHCP (cliente) | UDP | Cliente DHCP |
| 69 | TFTP | UDP | Transferencia simple de archivos |
| 80 | HTTP | TCP | Páginas web (sin cifrar) |
| 110 | POP3 | TCP | Recepción de correo (descarga) |
| 123 | NTP | UDP | Sincronización de tiempo de red |
| 143 | IMAP | TCP | Acceso a buzón de correo (en servidor) |
| 161 | SNMP | UDP | Gestión de dispositivos de red |
| 389 | LDAP | TCP | Servicio de directorio |
| 443 | HTTPS | TCP | HTTP sobre TLS (web segura) |
| 3389 | RDP | TCP | Escritorio remoto Windows |

> **Pregunta frecuente:** "El puerto 80 es utilizado por..." → **HTTP**. "El protocolo del stack TCP/IP que define el acceso interactivo a un ordenador remoto es..." → **Telnet** (puerto 23). La alternativa segura es SSH (puerto 22).

---

## 7. DHCP

**DHCP** (Dynamic Host Configuration Protocol, [RFC 2131](https://www.rfc-editor.org/rfc/rfc2131)) permite la asignación dinámica y automática de parámetros de red a los clientes.

**Parámetros que asigna DHCP:**
- Dirección IP
- Máscara de subred
- Puerta de enlace predeterminada (gateway)
- Servidores DNS
- Tiempo de concesión (lease time)

### 7.1 Proceso DORA

El proceso de asignación de IP en DHCP se conoce como **DORA** (Discover → Offer → Request → Acknowledge):

```
   Cliente (sin IP)                         Servidor DHCP
        │                                        │
        │─── DHCPDISCOVER (broadcast UDP) ──────►│  1. Discover
        │    (¿Hay algún servidor DHCP?)          │
        │                                        │
        │◄── DHCPOFFER (unicast/broadcast) ───── │  2. Offer
        │    (Te ofrezco IP: 192.168.1.50)        │
        │                                        │
        │─── DHCPREQUEST (broadcast UDP) ───────►│  3. Request
        │    (Acepto la IP 192.168.1.50)          │
        │                                        │
        │◄── DHCPACK (unicast/broadcast) ─────── │  4. Acknowledge
        │    (Confirmado. IP: 192.168.1.50/24)    │
        │                                        │
```

- **Puerto servidor:** UDP 67.
- **Puerto cliente:** UDP 68.
- Los mensajes DISCOVER y REQUEST se envían en **broadcast** porque el cliente aún no tiene IP.

---

## 8. DNS — Domain Name System

**DNS** ([RFC 1034](https://www.rfc-editor.org/rfc/rfc1034), [RFC 1035](https://www.rfc-editor.org/rfc/rfc1035)) es el servicio que traduce nombres de dominio legibles (como `www.ejemplo.es`) en direcciones IP. Puerto **53** (UDP para consultas, TCP para transferencias de zona).

### 8.1 Jerarquía DNS

```
                    . (raíz / root)
                   /|\
                  / | \
               .es .com .org  ← TLD (Top-Level Domains)
               /       \
           .junta     .ejemplo  ← Dominios de 2º nivel
             /             \
          intranet        www  ← Subdominios / hosts
```

- **Servidores raíz:** 13 clústeres (a.root-servers.net a m.root-servers.net).
- **TLD:** dominios de primer nivel (.es, .com, .org, .net, etc.).
- **Dominios de segundo nivel:** gestionados por registradores (p. ej., ejemplo.com).
- **Zonas:** cada servidor autoritativo gestiona una zona.

### 8.2 Tipos de registros DNS

| Tipo | RFC | Descripción |
|:---|:---:|:---|
| **A** | RFC 1035 | Dirección IPv4 de un host |
| **AAAA** | RFC 3596 | Dirección IPv6 de un host |
| **CNAME** | RFC 1035 | Alias canónico (nombre alternativo) |
| **MX** | RFC 1035 | Servidor de correo del dominio |
| **NS** | RFC 1035 | Servidor de nombres autoritativo para la zona |
| **SOA** | RFC 1035 | Start of Authority — información de la zona |
| **PTR** | RFC 1035 | DNS inverso (IP → nombre) |
| **TXT** | RFC 1035 | Texto arbitrario (SPF, DKIM, verificación) |
| **SRV** | RFC 2782 | Localización de servicios |
| **ANY** | RFC 1035 (limitado por RFC 8482) | Consulta de todos los registros |

> **Pregunta frecuente:** "¿Qué tipo de registro DNS no existe en ninguna RFC?" → **NNS** es un nombre inventado. ANY (RFC 1035, limitado por RFC 8482), A, AAAA, MX, CNAME, NS, SOA, PTR, TXT → todos existen en RFCs.

### 8.3 Resolución recursiva vs iterativa

**Resolución recursiva:**
- El cliente delega toda la resolución al **resolver recursivo** (normalmente su servidor DNS configurado).
- El resolver realiza todas las consultas necesarias y devuelve la respuesta final al cliente.
- El cliente recibe directamente la IP o un error.

**Resolución iterativa:**
- El cliente (o resolver) consulta sucesivamente a distintos servidores.
- Cada servidor devuelve la mejor referencia que tiene (referral) hacia un servidor más cercano a la respuesta.
- El resolver sigue la cadena hasta obtener la respuesta autoritativa.

```
Resolución recursiva (típica):
Cliente → Resolver DNS → Servidor raíz → TLD → Autoritativo → Resolver → Cliente

Resolución iterativa:
Cliente → Servidor raíz (te reenvío al TLD)
Cliente → Servidor TLD (te reenvío al autoritativo)
Cliente → Servidor autoritativo (aquí está la IP)
```

### 8.4 DNS inverso (PTR)

El **DNS inverso** resuelve una dirección IP en un nombre de dominio, usando la zona especial `in-addr.arpa` (IPv4) o `ip6.arpa` (IPv6).

Ejemplo: para resolver la IP `8.8.8.8`, se consulta `8.8.8.8.in-addr.arpa`, que devuelve `dns.google`.

Comando `nslookup`:
```bash
# Consulta directa: nombre → IP (al servidor DNS por defecto)
nslookup www.ejemplo.es

# Consulta inversa: IP → nombre (al servidor 80.58.61.250)
nslookup 8.8.8.8 80.58.61.250
```
> **Pregunta frecuente:** El comando `nslookup 8.8.8.8 80.58.61.250` solicita una **resolución inversa** de la IP 8.8.8.8, usando 80.58.61.250 como servidor DNS. El segundo argumento es el servidor a usar, no el nombre a resolver.

---

## 9. Protocolos de Enrutamiento

Los routers necesitan saber cómo llegar a cada red. Esa información puede configurarse manualmente (rutas estáticas) o aprenderse automáticamente a través de **protocolos de enrutamiento dinámico**. En el examen importa distinguir los tipos y sus características clave (métrica, ámbito, convergencia).

### 9.1 Clasificación

Los protocolos de enrutamiento permiten a los routers intercambiar información sobre las redes que conocen para construir sus tablas de enrutamiento.

| Categoría | Tipo | Protocolos |
|:---|:---|:---|
| **IGP** (Interior Gateway Protocol) | Vector distancia | RIP (v1/v2) |
| **IGP** | Estado de enlace | OSPF, IS-IS |
| **IGP** | Híbrido | EIGRP (Cisco propietario) |
| **EGP** (Exterior Gateway Protocol) | Vector de ruta | BGP |

### 9.2 RIP — Routing Information Protocol

**RIP** ([RFC 2453](https://www.rfc-editor.org/rfc/rfc2453) para RIPv2) es el protocolo de enrutamiento por vector de distancia más simple.

- **Métrica:** número de **saltos** (hops) hasta el destino.
- **Máximo de saltos:** 15. Una red a 16 saltos se considera inalcanzable.
- **Actualización:** envía su tabla completa cada **30 segundos** a sus vecinos.
- **Convergencia lenta** en redes grandes.

| Característica | RIPv1 | RIPv2 |
|:---|:---:|:---:|
| Máscara de subred | No (classful) | Sí (classless) |
| Autenticación | No | Sí (MD5) |
| Multidifusión | No (broadcast) | Sí (224.0.0.9) |
| VLSM/CIDR | No | Sí |

### 9.3 OSPF — Open Shortest Path First

**OSPF** ([RFC 2328](https://www.rfc-editor.org/rfc/rfc2328) para OSPFv2, [RFC 5340](https://www.rfc-editor.org/rfc/rfc5340) para OSPFv3/IPv6) es el protocolo de enrutamiento por **estado de enlace** más usado en redes empresariales.

- **Algoritmo:** Dijkstra (SPF, Shortest Path First).
- **Métrica:** coste (inversamente proporcional al ancho de banda del enlace).
- **Áreas:** organiza la red en áreas (el área 0 es el **backbone**). Las otras áreas conectan con el backbone.
- **LSA** (Link State Advertisements): los routers intercambian LSAs para construir la **LSDB** (Link State Database), un mapa completo de la topología.
- **Convergencia rápida** y escalable.
- **Sin límite de saltos.**

### 9.4 BGP — Border Gateway Protocol

**BGP** ([RFC 4271](https://www.rfc-editor.org/rfc/rfc4271)) es el protocolo de enrutamiento **exterior** que une los Sistemas Autónomos (AS) de Internet.

- **Tipo:** vector de ruta (path vector).
- **Sistema Autónomo (AS):** conjunto de redes bajo una misma administración, identificado con un ASN (Autonomous System Number).
- **Uso:** es el protocolo que hace funcionar Internet, utilizado por ISPs y grandes organizaciones.
- **Sesiones eBGP** (exterior): entre routers de distintos AS.
- **Sesiones iBGP** (interior): entre routers del mismo AS.

| Característica | RIP | OSPF | BGP |
|:---|:---:|:---:|:---:|
| Tipo | Vector distancia | Estado de enlace | Vector de ruta |
| Ámbito | IGP (interior) | IGP (interior) | EGP (exterior) |
| Métrica | Saltos (máx. 15) | Coste (ancho de banda) | Múltiples atributos |
| Convergencia | Lenta | Rápida | Lenta (estabilidad) |
| Escalabilidad | Pequeñas redes | Grandes empresas | Internet (global) |
| RFC | RFC 2453 | RFC 2328 | RFC 4271 |

---

## 10. NAT — Network Address Translation

**NAT** ([RFC 3022](https://www.rfc-editor.org/rfc/rfc3022)) es la técnica que permite traducir direcciones IP privadas (RFC 1918) a IPs públicas para permitir el acceso a Internet.

**Motivación:** el espacio de IPs públicas IPv4 es limitado. NAT permite que muchos dispositivos con IPs privadas compartan una o pocas IPs públicas.

### 10.1 Tipos de NAT

| Tipo | Descripción | Uso típico |
|:---|:---|:---|
| **NAT estático** (1:1) | Una IP privada siempre se mapea a la misma IP pública | Servidores accesibles desde Internet |
| **NAT dinámico** | Un pool de IPs públicas se asigna dinámicamente a petición | Acceso a Internet con múltiples IPs públicas |
| **PAT / NAPT / Masquerade** | Muchas IPs privadas comparten **una** IP pública usando distintos puertos TCP/UDP | Redes domésticas y empresariales (el más común) |

### 10.2 Funcionamiento de PAT

```
Red privada (10.0.0.0/24)         Router NAT/PAT            Internet
                                  (IP pública: 203.0.113.1)
┌─────────────────────────────────────────────────────────────────┐
│ 10.0.0.2:50001 ──►  203.0.113.1:60001 ──────────────────────── ► │
│ 10.0.0.3:50001 ──►  203.0.113.1:60002 ──────────────────────── ► │
│ 10.0.0.4:50001 ──►  203.0.113.1:60003 ──────────────────────── ► │
└─────────────────────────────────────────────────────────────────┘

El router mantiene una tabla de traducción:
IP privada:puerto ↔ IP pública:puerto
```

---

## 11. Capa de Aplicación: Protocolos principales

Ya se han visto los puertos en la sección anterior. Aquí se desarrollan los protocolos más importantes con el detalle suficiente para responder preguntas de examen sobre su funcionamiento interno.

### 11.1 HTTP y HTTPS

- **HTTP** (HyperText Transfer Protocol, [RFC 7230](https://www.rfc-editor.org/rfc/rfc7230) / [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)): protocolo sin estado para transferencia de hipermedios. Puerto **80**.
- **HTTPS**: HTTP sobre TLS ([RFC 8446](https://www.rfc-editor.org/rfc/rfc8446)). Puerto **443**. Cifra el tráfico entre el cliente y el servidor.

### 11.2 FTP

- **FTP** (File Transfer Protocol, [RFC 959](https://www.rfc-editor.org/rfc/rfc959)): transferencia de archivos. Usa dos conexiones TCP:
  - Puerto **21**: canal de control (comandos).
  - Puerto **20**: canal de datos.
- Transmite credenciales en texto plano. Su alternativa segura es **SFTP** (sobre SSH) o **FTPS** (FTP sobre TLS).

### 11.3 SMTP, POP3, IMAP

| Protocolo | Puerto | RFC | Función |
|:---|:---:|:---:|:---|
| **SMTP** | 25 (587 autenticado) | RFC 5321 | Envío de correo (Mail Transfer Agent) |
| **POP3** | 110 | RFC 1939 | Descarga de correo del servidor al cliente |
| **IMAP** | 143 | RFC 3501 | Acceso al buzón en el servidor (sincronización) |

### 11.4 SSH y Telnet

| Protocolo | Puerto | Cifrado | Uso |
|:---|:---:|:---:|:---|
| **Telnet** | 23 | No (texto plano) | Acceso remoto interactivo (en desuso) |
| **SSH** | 22 | Sí (cifrado) | Acceso remoto seguro |

> **Pregunta frecuente:** "El protocolo que define el acceso interactivo a un ordenador remoto es **Telnet**". Hoy se usa SSH por seguridad, pero el enunciado histórico se refiere a Telnet.

### 11.5 SNMP

**SNMP** (Simple Network Management Protocol): gestión de dispositivos de red (routers, switches, servidores). Usa UDP puertos 161 (consultas) y 162 (traps).

### 11.6 NTP

**NTP** (Network Time Protocol, [RFC 5905](https://www.rfc-editor.org/rfc/rfc5905)): sincronización de relojes en red. Puerto UDP 123.

---

## 12. Diagnóstico de red — comandos clave

En el examen pueden aparecer preguntas sobre qué comando usar en cada situación (comprobar si un host es alcanzable, rastrear la ruta, consultar DNS, ver la tabla ARP...). Estos son los comandos que hay que tener claros:

```bash
# Comprobar conectividad (ICMP Echo Request/Reply)
ping 8.8.8.8
ping www.ejemplo.es

# Rastrear la ruta (ICMP Time Exceeded, TTL creciente)
traceroute 8.8.8.8          # Linux/macOS
tracert 8.8.8.8             # Windows

# Consultar DNS
nslookup www.ejemplo.es                # Consulta directa al DNS por defecto
nslookup 8.8.8.8 80.58.61.250         # Consulta inversa usando servidor específico
dig www.ejemplo.es A                   # Consulta registro A

# Ver tabla ARP
arp -a
ip neigh show                         # Linux

# Ver tabla de enrutamiento
route print                           # Windows
ip route show                         # Linux

# Añadir ruta por defecto (Windows)
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1
route add -p 0.0.0.0 mask 0.0.0.0 192.168.12.1  # Persistente

# Ver configuración de red
ipconfig /all                         # Windows
ip addr show                          # Linux
```

> **Pregunta frecuente:** `route add 0.0.0.0 mask 0.0.0.0 192.168.12.1` añade la **ruta por defecto** con gateway 192.168.12.1. El destino `0.0.0.0` con máscara `0.0.0.0` representa "cualquier destino" — es la ruta de último recurso.

---

## Resumen para el examen

Los siguientes puntos concentran lo que el banco de preguntas de este tema evalúa con más frecuencia:

1. **PDUs por capa OSI:** bits (física), trama (enlace), **paquete** (red), segmento/datagrama (transporte), datos (sesión/presentación/aplicación). La pregunta "¿qué PDU usa la capa de red?" → **paquete**.

2. **OSI tiene 7 capas, TCP/IP tiene 4.** OSI siempre tiene más capas. "TCP/IP proviene de tres protocolos" → **FALSO** (son dos: TCP e IP).

3. **Clases IPv4:** Clase A → 1 byte de red, Clase B → **2 bytes de red** (/16), Clase C → 3 bytes de red (/24).

4. **Subredes y CIDR:** fórmula `2^(32-n) - 2` hosts. Para alojar 250 hosts → /24 (254 hosts). Para 3.000 hosts → /20 (4.094 hosts). Para 4 subredes de una /24 → usar /26 (tomar 2 bits).

5. **Dirección de broadcast** de una subred: dirección de red + todos los bits de host a 1. Ejemplo: 10.40.7.5/25 → red 10.40.7.0, broadcast **10.40.7.127**.

6. **ARP:** resuelve IP → MAC en la LAN local. **No es de la capa de aplicación.** En IPv6 lo sustituye NDP.

7. **ICMP:** reporta errores de datagramas IP (ping, traceroute). No tiene puertos. "¿Qué protocolo reporta errores de datagramas IP?" → **ICMP**.

8. **UDP:** no orientado a conexión, sin garantías. **TFTP** usa UDP (puerto 69). DNS también usa UDP (puerto 53). UDP es de la **capa de transporte** (OSI capa 4), no de la capa de enlace.

9. **Puertos clave:** 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 67/68 DHCP, 80 HTTP, 110 POP3, 143 IMAP, **443 HTTPS**. "Puerto 80" → **HTTP**.

10. **Telnet** (puerto 23): acceso remoto interactivo, texto plano. Respuesta a "protocolo de acceso interactivo remoto" en contexto TCP/IP.

11. **DNS:** "ping a IP funciona pero no al nombre de dominio" → falla **DNS**. Comando `nslookup IP servidor` → **resolución inversa**.

12. **DHCP proceso DORA:** Discover → Offer → Request → Acknowledge. Puertos UDP 67 (servidor) y 68 (cliente).

13. **Rangos privados RFC 1918:** 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16. APIPA (169.254.0.0/16) **no es RFC 1918**.

14. **El rango 10.0.0.0/8** es el más adecuado para diseños con muchas subredes de distintos tamaños.

15. **Direcciones multicast** (224–239.x.x.x) no se asignan como IPs de servidores públicos. Loopback (127.x.x.x) no se usa en red.

16. **NAT/PAT:** permite que muchos hosts con IP privada compartan una IP pública. PAT (o Masquerade) es el tipo más común.

17. **Protocolos de enrutamiento:** RIP → vector distancia, métrica saltos, máximo 15. OSPF → estado de enlace, Dijkstra, áreas, sin límite de saltos. BGP → exterior, vector de ruta, entre Sistemas Autónomos.

18. **Three-way handshake TCP:** SYN → SYN-ACK → ACK. Es lo que hace TCP para establecer una conexión orientada a conexión y fiable.

19. **Comunicación entre subredes distintas** (ej. 192.168.1.x y 192.168.2.x con /24) requiere un **router** correctamente configurado. ARP no resuelve comunicación entre subredes.

20. **Registro DNS inexistente en ninguna RFC:** "NNS" es un nombre inventado. ANY (RFC 1035), SPF (RFC 7208, via registro TXT), A, AAAA, MX, CNAME, NS, SOA, PTR, TXT → todos existen en RFCs.

---

## Preguntas frecuentes en exámenes

El banco de examen clasifica **25 preguntas** en este tema. El análisis de su distribución muestra los siguientes focos:

### Subredes y cálculo CIDR (7 preguntas — el área más evaluada)

Es el área con más peso. Las preguntas piden calcular la dirección de broadcast, el número de hosts, la máscara mínima necesaria o la subred a la que pertenece una IP.

**Ejemplos representativos:**

- *"10.40.7.5/25 → dirección de broadcast"* → 10.40.7.127 (la máscara /25 da bloques de 128; la primera subred va de .0 a .127).
- *"10.64.81.129 con máscara 255.255.255.252 → broadcast"* → 10.64.81.131 (/30 da bloques de 4; subred .128, hosts .129 y .130, broadcast .131).
- *"192.168.10.64/26 → hosts asignables"* → 62 (2⁶ − 2).
- *"Para 3.000 hosts, máscara mínima"* → /20 (4.094 hosts).
- *"Para 250 hosts, máscara más ajustada"* → /24 (254 hosts).

**Técnica de cálculo rápido:**
```
/30 → bloque 4   → 2 hosts
/29 → bloque 8   → 6 hosts
/28 → bloque 16  → 14 hosts
/27 → bloque 32  → 30 hosts
/26 → bloque 64  → 62 hosts
/25 → bloque 128 → 126 hosts
/24 → bloque 256 → 254 hosts
```

### Puertos y protocolos de aplicación (5 preguntas)

Las preguntas asocian un número de puerto a su protocolo o un servicio a su protocolo correcto.

- Puerto 80 → HTTP.
- Puerto 22 → SSH.
- Puerto 23 → Telnet (acceso interactivo remoto).
- "¿Qué protocolo NO pertenece a la capa de aplicación?" → ARP.
- "¿Qué protocolo falla si el ping a IP funciona pero no al nombre de dominio?" → DNS.

### PDUs y capas OSI/TCP-IP (4 preguntas)

- PDU de la capa de red → paquete.
- Número de capas: OSI 7 vs TCP/IP 4.
- "TCP/IP proviene de 3 protocolos" → FALSO.
- La capa responsable de la comunicación fiable → **capa de transporte** (TCP).

### Protocolos de resolución y control (4 preguntas)

- ARP: IP → MAC.
- ICMP: reporta errores de datagramas.
- UDP: no orientado a conexión; usado por TFTP.
- DHCP vs ARP vs DNS: cada uno tiene su papel claro y diferenciado.

### DNS avanzado (3 preguntas)

- Tipos de registros: NNS no existe.
- `nslookup IP servidor` → resolución inversa.
- DNS falla: ping a IP funciona, ping a nombre no.

### Rangos IP y direccionamiento especial (2 preguntas)

- Identificar IP válida pública vs privada: 194.x.x.x (pública), 172.16-31.x.x (privada RFC 1918), 239.x.x.x (multicast, no válida como pública), 127.x.x.x (loopback, no válida en red).
- Rango más adecuado para diseño con subredes variadas → 10.0.0.0/8.

---

## Referencias

- [RFC 791 — Internet Protocol (IPv4)](https://www.rfc-editor.org/rfc/rfc791) — Especificación original de IPv4.
- [RFC 792 — Internet Control Message Protocol (ICMP)](https://www.rfc-editor.org/rfc/rfc792) — Mensajes de control y error en redes IP.
- [RFC 826 — Address Resolution Protocol (ARP)](https://www.rfc-editor.org/rfc/rfc826) — Resolución de IP a dirección MAC.
- [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768) — Protocolo de transporte sin conexión.
- [RFC 793 — Transmission Control Protocol (TCP)](https://datatracker.ietf.org/doc/html/rfc793) — Especificación original de TCP (reemplazada por RFC 9293).
- [RFC 9293 — Transmission Control Protocol (TCP)](https://datatracker.ietf.org/doc/html/rfc9293) — Especificación actualizada de TCP (2022).
- [RFC 854 — Telnet Protocol Specification](https://www.rfc-editor.org/rfc/rfc854) — Protocolo de acceso remoto interactivo.
- [RFC 959 — File Transfer Protocol (FTP)](https://www.rfc-editor.org/rfc/rfc959) — Protocolo de transferencia de archivos.
- [RFC 1034 — Domain Names: Concepts and Facilities](https://www.rfc-editor.org/rfc/rfc1034) — Conceptos del sistema DNS.
- [RFC 1035 — Domain Names: Implementation and Specification](https://www.rfc-editor.org/rfc/rfc1035) — Implementación del DNS y registros básicos.
- [RFC 1350 — TFTP Protocol](https://www.rfc-editor.org/rfc/rfc1350) — Trivial File Transfer Protocol sobre UDP.
- [RFC 1918 — Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918) — Rangos de direcciones IP privadas.
- [RFC 2131 — Dynamic Host Configuration Protocol (DHCP)](https://www.rfc-editor.org/rfc/rfc2131) — Protocolo de asignación dinámica de IPs.
- [RFC 2328 — OSPF Version 2](https://www.rfc-editor.org/rfc/rfc2328) — Protocolo de enrutamiento OSPF.
- [RFC 2453 — RIP Version 2](https://www.rfc-editor.org/rfc/rfc2453) — Protocolo de enrutamiento RIP v2.
- [RFC 3022 — Traditional IP Network Address Translator (NAT)](https://www.rfc-editor.org/rfc/rfc3022) — Funcionamiento de NAT/PAT.
- [RFC 4271 — Border Gateway Protocol 4 (BGP-4)](https://www.rfc-editor.org/rfc/rfc4271) — Protocolo de enrutamiento exterior BGP.
- [RFC 4291 — IP Version 6 Addressing Architecture](https://www.rfc-editor.org/rfc/rfc4291) — Tipos y formato de direcciones IPv6.
- [RFC 4632 — Classless Inter-Domain Routing (CIDR)](https://www.rfc-editor.org/rfc/rfc4632) — Enrutamiento sin clases y VLSM.
- [RFC 8200 — Internet Protocol, Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc8200) — Especificación de IPv6.
- [IANA — Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers) — Registro oficial de puertos.
- [IANA — IPv4 Address Space Registry](https://www.iana.org/assignments/ipv4-address-space) — Registro de bloques de IPs asignados.
- [ccnadesdecero.es — Modelos TCP/IP y OSI](https://ccnadesdecero.es/modelos-tcp-ip-osi-caracteristicas/) — Guía práctica en español sobre capas y características.
- [subnetmaster.es — Modelo OSI y TCP/IP](https://subnetmaster.es/guia/modelo-osi-y-tcp-ip/) — Guía sobre OSI, encapsulamiento y TCP/IP en español.
