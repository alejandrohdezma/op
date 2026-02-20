# Tema 4: Administración de Redes de Área Local

> Este tema cubre la administración completa de redes de área local en entornos corporativos: desde la gestión centralizada de usuarios y dispositivos hasta la monitorización del tráfico. En las oposiciones del Bloque IV aparece con regularidad alta: el banco de preguntas incluye 33 preguntas que tocan SNMP/RMON, TETRA, DHCP, RADIUS, LDAP, almacenamiento (NAS/DAS/SAN/RAID) y herramientas de diagnóstico. El estudiante debe dominar los conceptos técnicos con precisión (puertos, RFC, siglas exactas, niveles OSI) porque las preguntas discriminan por detalles concretos.

---

## 1. Administración de Redes de Área Local

### 1.1 Introducción y funciones del administrador

El **administrador de red** es quien mantiene todo funcionando: que los usuarios puedan entrar, que los datos fluyan, que cuando algo falle se detecte rápido. La **administración de una LAN** cubre dos grandes fases:

1. **Fase inicial**: instalación y configuración de servidores, protocolos y documentación de la red.
2. **Fase continua**: garantizar disponibilidad, corregir incidencias y optimizar el rendimiento.

Las **tareas principales del administrador** son:

- **Instalación y mantenimiento**: instalación del **Sistema Operativo de Red (NOS)** y garantía de su funcionamiento a lo largo del tiempo.
- **Formación y capacitación**: adquirir conocimiento sobre el hardware y software de la red. La administración puede gestionarse internamente o mediante **outsourcing**.
- **Determinación de servicios**: identificar los servicios necesarios (DNS, DHCP, correo, web) y configurar los accesos de los usuarios.
- **Diagnóstico de problemas y mejoras**: monitorización continua para detectar incidencias y anticiparse a problemas mediante la definición de umbrales de alarma.
- **Documentación del sistema**: registrar toda la configuración de la red para facilitar el mantenimiento.
- **Comunicación con usuarios**: informar sobre cambios planificados, incidencias activas y procedimientos relacionados con la red.

### 1.2 Funciones agrupadas de administración

| Función | Descripción |
|---|---|
| **Gestión de Usuarios** | Administración masiva de permisos, directivas de seguridad y acceso a través de grupos o dominios |
| **Gestión de Recursos** | Administración de archivos, impresoras y dispositivos en red |
| **Direccionamiento** | Asignación dinámica de IP (DHCP), segmentación con VLAN, NAT, gestión DNS |
| **Gestión de Servicios** | Correo electrónico, servidores web, gestores de datos |
| **Gestión de Red** | Supervisión con SNMP, NMS, alarmas preventivas |

### 1.3 SDN: Software Defined Networking

**SDN (Software Defined Networking)** es una arquitectura de red que separa el **plano de control** (las decisiones de enrutamiento) del **plano de datos** (el reenvío de paquetes). En las redes tradicionales ambos planos están integrados en cada dispositivo (router, switch), lo que obliga a configurarlos uno a uno. SDN centraliza el control en un software llamado **controlador SDN**.

| Concepto | Descripción |
|---|---|
| **Plano de datos** | Hardware de red que reenvía paquetes según las tablas de flujo |
| **Plano de control** | Lógica de decisión (rutas, políticas); centralizada en el controlador SDN |
| **Controlador SDN** | Software central que programa todos los dispositivos (ej. OpenDaylight, ONOS, VMware NSX) |
| **OpenFlow** | Protocolo estándar de comunicación entre controlador y dispositivos |

**Ventajas de SDN** en entornos corporativos y administraciones públicas:

- **Gestión programática centralizada**: toda la red se configura desde un punto único, reduciendo errores.
- **Automatización**: aprovisionamiento rápido de redes para nuevos servicios.
- **Microsegmentación**: aplicar políticas de seguridad granulares (cumplimiento del ENS).
- **Visibilidad**: monitorización centralizada del tráfico.

SDN **no** elimina la necesidad de firewalls perimetrales físicos, ni sustituye los protocolos de enrutamiento dinámico por segmentación estática. Puede convivir con BGP u OSPF y complementar la seguridad tradicional.

### 1.4 El ciclo de gestión de red

Las herramientas de gestión de red, conocidas como **NMS (Network Management Station)**, no solo recogen datos pasivamente: definen umbrales de error y disparan alarmas antes de que el problema sea grave. El ciclo es sencillo: **monitorizar → evaluar → mejorar**.

Para hacerlo de forma remota y escalable se usa **SNMP** (Simple Network Management Protocol), que permite al NMS consultar el estado de cualquier dispositivo de la red y recibir alertas automáticas (traps) cuando algo sale mal.

---

## 2. Gestión de Usuarios

### 2.1 Introducción: derechos de acceso y permisos

Cuando un usuario se conecta a la red, el administrador controla exactamente qué puede y qué no puede hacer. Para eso existen dos conceptos que conviene no confundir:

- **Derechos de acceso**: operaciones que un usuario o grupo puede realizar sobre un servidor o estación de trabajo (p. ej., apagar el servidor, iniciar sesión localmente).
- **Permisos**: marcas en recursos concretos (archivos, directorios) que dicen quién puede leer, escribir o ejecutar.

Un usuario puede tener derecho a ejecutar una aplicación de monitorización, pero tener permisos restringidos sobre ciertos directorios del sistema. Ambas cosas coexisten.

En redes mixtas con **diferentes sistemas operativos** (Windows y Linux, por ejemplo) la cosa se complica porque los modelos de permisos no son equivalentes y hay que gestionar ambos.

### 2.2 Tipos de usuarios en una LAN

| Tipo | Descripción |
|---|---|
| **Usuarios Locales** | Definidos en un único equipo; utilizan una base de datos local para almacenar la información de acceso |
| **Usuarios de Dominio** | Gestionados por un servidor de dominio; el usuario proporciona nombre, contraseña y dominio al iniciar sesión |
| **Grupos de Usuarios** | Facilitan la administración agrupando usuarios con criterios comunes (p. ej., departamento); se les asignan permisos similares |

Los registros de acceso a la red se almacenan en archivos conocidos como **logs**. Los administradores pueden implementar políticas como cambios periódicos de contraseña o desactivación de cuentas inactivas.

### 2.3 Comparativa: control de acceso en UNIX vs. Windows

| Aspecto | UNIX/Linux | Windows |
|---|---|---|
| **Modelo** | Sencillo: propietario, grupo, resto (rwx) | Listas de control de acceso (ACL) granulares |
| **Flexibilidad** | Limitada pero eficiente | Alta, permite condiciones por usuario específico |
| **Complejidad** | Baja | Alta; puede introducir errores y mayor latencia |
| **Archivos clave** | `/etc/passwd`, `/etc/shadow`, `/etc/group` | Fichero SAM (contraseñas cifradas) |
| **Política de autenticación** | Módulos PAM (`/etc/pam.d/`) | Active Directory + Kerberos |

Ejemplo de permisos en UNIX: `dr-x-w-r--` representa: directorio (`d`), propietario puede leer y ejecutar (`r-x`), grupo puede escribir (`-w-`), resto solo lectura (`r--`).

```bash
# Ver permisos en Linux
ls -la /directorio

# Cambiar permisos
chmod 750 archivo.sh

# Cambiar propietario
chown usuario:grupo archivo.sh
```

### 2.4 Perfiles de usuario en redes

El **perfil de usuario** define el entorno de trabajo que ve el usuario al iniciar sesión: fondo de pantalla, accesos directos, aplicaciones que se lanzan automáticamente, restricciones sobre la configuración del sistema... Básicamente, el administrador decide qué puede tocar cada usuario y qué no.

Puntos clave:

- Permiten **forzar la ejecución de aplicaciones al inicio de sesión** o bloquear cambios en la interfaz gráfica.
- Los **NOS** incluyen utilidades para asociar perfiles a cuentas individuales o a grupos enteros.
- Los perfiles pueden ser **locales** (solo en esa máquina) o **globales** (almacenados en servidor).
- Con los **perfiles móviles** (*roaming profiles*), el usuario ve exactamente el mismo entorno desde cualquier equipo de la red.

### 2.5 Plataforma Windows: Active Directory y dominios

En entornos Windows con muchos nodos se organizan mediante **grupos o dominios**. El mecanismo central es **Active Directory (AD)**, que utiliza los protocolos **LDAP**, **DNS**, **DHCP** y **Kerberos**.

#### Tipos de grupos en Windows

| Tipo | Ámbito |
|---|---|
| **Global** | Contiene usuarios de un mismo dominio |
| **Local** | Incluye usuarios y grupos globales de diferentes dominios |
| **Universal** | Puede contener usuarios, grupos globales y grupos de distintos dominios |

Windows distingue entre:
- **Grupos de trabajo (Workgroup)**: redes pequeñas sin servidor de dominio; cada equipo gestiona sus propias cuentas.
- **Dominios**: redes empresariales con un servidor de dominio centralizado que replica los cambios automáticamente a todos los controladores.

Los usuarios se almacenan en el **fichero SAM** con contraseñas cifradas. El objetivo principal de un dominio es **unificar la autenticación y autorización** de usuarios para acceder a los recursos del dominio (Single Sign-On basado en Kerberos).

La herramienta estándar de gestión gráfica es **ADUC (Active Directory Users and Computers)**, incluida en las **RSAT (Remote Server Administration Tools)**.

### 2.6 LDAP: Estructura del directorio

**LDAP (Lightweight Directory Access Protocol)** es el protocolo estándar para consultar y modificar directorios de red. Piénsalo como una base de datos especializada en usuarios, grupos y recursos, optimizada para lecturas frecuentes. La versión actual es **LDAPv3**, definida en la [RFC 4511](https://www.rfc-editor.org/rfc/rfc4511).

La información se organiza en un árbol llamado **DIT (Directory Information Tree)**. Cada nodo del árbol se identifica de forma única mediante su **DN (Distinguished Name)**, que describe la ruta completa desde ese nodo hasta la raíz del árbol.

#### Componentes del DN

| Componente | Significado | Ejemplo |
|---|---|---|
| **DC** | Domain Component | `DC=empresa,DC=es` |
| **O** | Organization | `O=Acme` |
| **OU** | Organizational Unit | `OU=RRHH` |
| **CN** | Common Name | `CN=jperez` |

El DN se construye **de más específico a más general**, separado por comas:

```
CN=jperez,OU=RRHH,DC=empresa,DC=es
```

#### Atributos estándar y personalizados

- **Atributos estándar**: definidos en RFC con OID fijos (p. ej., `cn`, `sn`, `mail`, `uid`, `objectClass`). Definidos en [RFC 4519](https://www.rfc-editor.org/rfc/rfc4519).
- **Atributos personalizados**: extensiones al esquema definidas por la organización para necesidades propias (p. ej., `numeroEmpleado`). Requieren registrar un OID privado.

La diferencia clave: el atributo estándar tiene **definición fija** internacional; el personalizado se define según las **necesidades específicas de la organización**.

#### Consulta LDAP de ejemplo

```bash
# Buscar un usuario en el directorio
ldapsearch -H ldap://servidor.empresa.es \
  -b "DC=empresa,DC=es" \
  -D "CN=admin,DC=empresa,DC=es" \
  -W "(cn=jperez)"
```

#### JNDI: API Java para directorios

**JNDI (Java Naming and Directory Interface)** es la API estándar de Java (`javax.naming`) para que las aplicaciones Java accedan a servicios de nombres y directorio como LDAP, DNS, NIS o RMI Registry. Aparece en el examen porque se confunde con otras APIs:

- **JMS** (Java Message Service): cola de mensajes asíncrona — no tiene nada que ver con directorios.
- **JTA** (Java Transaction API): gestión de transacciones distribuidas — tampoco.

Si el examen pregunta "¿qué API de Java se usa para acceder a un directorio LDAP?", la respuesta es **JNDI**.

### 2.7 Plataforma UNIX/Linux: NIS y NFS

En UNIX/Linux no hay Active Directory, pero el problema es el mismo: ¿cómo consigo que el usuario Juan pueda iniciar sesión en cualquier máquina de la red y ver sus archivos? La respuesta tradicional usa dos servicios complementarios:

- **NIS (Network Information Service)**: centraliza usuarios, contraseñas y grupos en un servidor maestro. Los clientes consultan al servidor NIS en lugar de mirar sus archivos locales `/etc/passwd` y `/etc/shadow`.
- **NFS (Network File System)**: permite que los directorios home de los usuarios estén almacenados en un servidor y se monten de forma transparente en cualquier máquina. Juan ve su `/home/juan` igual desde cualquier equipo.

La configuración de políticas de autenticación en Linux se realiza a través de **PAM (Pluggable Authentication Modules)**, ubicados en `/etc/pam.d/`. Los módulos principales son:

| Módulo PAM | Función |
|---|---|
| `auth` | Autenticación del usuario |
| `account` | Gestión de cuentas (expiración, restricciones) |
| `password` | Gestión de contraseñas |
| `session` | Configuración de sesiones de usuario |

El control de recursos por usuario se gestiona mediante `/etc/security/limits.conf`.

---

## 3. Gestión de Dispositivos

### 3.1 Introducción: dispositivos físicos y lógicos

En una LAN hay que gestionar no solo los servidores, sino todos los dispositivos que se conectan: discos, impresoras, puertos, unidades de cinta... El sistema operativo representa cada dispositivo físico como un **dispositivo lógico** (un archivo especial en UNIX, un nombre de dispositivo en Windows) y usa **controladores (drivers)** para que las aplicaciones no tengan que preocuparse del hardware concreto.

#### Dispositivos en UNIX

| Dispositivo | Ruta |
|---|---|
| Consola | `/dev/tty1` |
| Puerto paralelo | `/dev/lp0` |
| Puerto serie | `/dev/ttyS0` |
| Disco IDE maestro | `/dev/hda` |
| Disco SATA/SCSI | `/dev/sda` |
| CD-ROM | `/dev/sr0` |
| USB | `/dev/sdb`, `/dev/sdc`... |

En UNIX los dispositivos de almacenamiento deben **montarse** antes de su uso con el comando `mount`. Se distinguen:
- **Dispositivos de bloque**: discos (acceso aleatorio, en bloques).
- **Dispositivos de caracteres**: impresoras, puertos serie (acceso secuencial).

#### Dispositivos en Windows

| Nombre | Descripción |
|---|---|
| `COM` | Puertos seriales |
| `LPT` | Puertos paralelos (en desuso) |
| `PRN` | Referencia a la impresora |
| `NUL` | Dispositivo virtual nulo |

### 3.2 Gestión de servidores

¿Cuántos servidores necesita una organización? No hay respuesta única, pero hay criterios claros:

- Varios servidores se justifican por **seguridad** (separar roles: BD, web, correo) o por **distribución de carga** (balancear peticiones).
- Más servidores = más coste y más carga administrativa. Hay que buscar el equilibrio.
- En organizaciones grandes, la **consolidación de servidores** (pocos servidores muy potentes o virtualización) puede ser más eficiente que tener muchos físicos pequeños.

### 3.3 Gestión de estaciones cliente

| Modelo | Descripción |
|---|---|
| **Trabajo local** | Programas y datos residen en el cliente; datos de usuario pueden centralizarse en servidores |
| **Ejecución remota** | Los programas están en el servidor; el usuario los lanza remotamente |
| **Clientes "bobos"** | Thin clients sin disco local; arrancan remotamente a través de un sistema operativo en red |
| **Aplicaciones distribuidas** | Parte en el cliente, parte en el servidor; modelo cliente-servidor |

### 3.4 Gestión de discos

#### Tipos de disco

| Tipo | Siglas | Característica |
|---|---|---|
| Disco duro magnético | **HDD** | Almacenamiento masivo, bajo coste |
| Disco de estado sólido | **SSD** | Más rápido, menor consumo energético |
| Disco híbrido | **SSHD** | Combina HDD y SSD |

Las **interfaces** comunes son: **SATA** (Serial ATA), **SAS/SCSI** (Serial Attached SCSI) y **NVMe** (Non-volatile memory express, la más rápida).

#### Estructura lógica del disco

El diagrama de un disco muestra su estructura lógica de izquierda a derecha:
- **Sector de arranque** (primer sector): contiene información sobre las particiones.
- **Espacio particionado**: donde residen las particiones. En Windows cada partición recibe una letra (C:, D:, E:...).
- **Espacio sin particionar**: área del disco no asignada a ninguna partición.

Una vez creadas las particiones, se formatea con un sistema de archivos:

| Sistema de Archivos | Descripción |
|---|---|
| **FAT32** | El más antiguo y compatible; limitaciones de tamaño (4 GB por archivo) |
| **NTFS** | Sucesor de FAT en Windows; mayor seguridad y rendimiento |
| **exFAT** | Compatible con Windows, Linux y Unix; ideal para dispositivos portátiles |

#### Soluciones de almacenamiento

Hay tres grandes arquitecturas para almacenar datos en una red, y el examen pregunta sus siglas y protocolos con frecuencia:

| Solución | Siglas | Descripción |
|---|---|---|
| Almacenamiento de conexión directa | **DAS** | Cada estación tiene sus propios discos conectados directamente; sin compartición |
| Almacenamiento centralizado | — | Múltiples servidores comparten discos físicamente conectados entre sí |
| Almacenamiento en red | **NAS** | Los discos están conectados a la red; acceso mediante NFS (Unix) y CIFS/SMB (Windows) |
| Red de área de almacenamiento | **SAN** | Red de alta velocidad dedicada exclusivamente al almacenamiento; coexisten dos redes: la LAN normal y la SAN de datos |

> **Nota de examen**: en un dispositivo **NAS**, los protocolos de compartición de archivos son **NFS** (entornos Unix/Linux) y **CIFS/SMB** (entornos Windows). NTFS y FAT son sistemas de archivos locales, no protocolos de red.

#### RAID: Redundant Array of Independent Disks

Un solo disco puede fallar. **RAID** es la solución: usar varios discos físicos como si fueran uno solo, con redundancia para sobrevivir al fallo de uno (o más) de ellos. Cada nivel RAID ofrece un equilibrio diferente entre rendimiento, capacidad y tolerancia a fallos.

| Nivel | Nombre | Descripción | Mínimo discos |
|---|---|---|---|
| **RAID 0** | Striping | Distribución equitativa entre discos; alto rendimiento; **sin redundancia** | 2 |
| **RAID 1** | Mirroring | Replica los datos en 2+ discos; mejora lectura; no mejora escritura | 2 |
| **RAID 2** | — | Usa códigos de detección/corrección de errores; poco uso; lento | Varios |
| **RAID 3** | — | Almacena paridad en disco dedicado; divide datos en bytes; no se usa en práctica | 3 |
| **RAID 4** | — | Similar a RAID 3 pero divide en bloques | 3 |
| **RAID 5** | Striping con paridad | Combina RAID 0 + paridad distribuida; buen rendimiento; protección de datos | 3 |
| **RAID 6** | Paridad doble | RAID 5 + bloque de paridad adicional; mayor redundancia; puede perder 2 discos | 4 |
| **RAID 10** | Striping + Mirroring | Combina RAID 0 y RAID 1 | 4 |

El diagrama del **RAID 5** con 4 discos muestra que los datos (A1, A2, A3) y la paridad (Ap) se distribuyen rotativamente entre todos los discos, de modo que si falla uno se puede reconstruir la información.

El diagrama del **RAID 6** con 5 discos muestra dos bloques de paridad (p, q) distribuidos entre los discos, permitiendo tolerar el fallo simultáneo de hasta dos discos.

**Administración de RAID**:
- **Hot swap (intercambio en caliente)**: reemplazo de discos sin apagar el sistema.
- **Hot spare (reserva caliente)**: disco adicional preconectado que entra en funcionamiento automáticamente ante un fallo.

#### Tareas de mantenimiento de discos

| Tarea | Descripción |
|---|---|
| **Desfragmentación** | Mejora eficiencia de acceso en Windows reorganizando archivos fragmentados |
| **Compresión** | Ahorra espacio pero introduce latencia adicional en el acceso |
| **Comprobación de errores** | Detecta y corrige sectores defectuosos |
| **Copia de seguridad** | Esencial para la continuidad; tres tipos: completa, incremental y diferencial |

El diagrama de tipos de copia de seguridad ilustra la diferencia:
- **Full (completa)**: copia todo el conjunto de datos cada vez, independientemente de copias anteriores.
- **Differential (diferencial)**: copia todo lo modificado desde la última copia completa.
- **Incremental**: copia solo lo modificado desde la última copia incremental (la más eficiente en espacio).

### 3.5 Gestión de impresoras

En redes de área local existen dos tipos de impresoras:
- **Locales**: conectadas a un equipo específico.
- **De red**: ofrecen servicio de impresión a dispositivos conectados a la red.

En Windows la instalación se realiza mediante el **Asistente para agregar impresoras**. En UNIX las impresoras en red se configuran a través de TCP/IP en el archivo `/etc/printcap`.

Términos clave en la gestión de impresoras:

| Término | Descripción |
|---|---|
| **Dispositivo de impresión** | El hardware que produce documentos impresos |
| **Impresora lógica** | Dispositivo lógico del SO conectado al dispositivo físico a través de un puerto |
| **Controlador de impresora** | Traduce documentos electrónicos al formato de la impresora |
| **Cola de impresora** | Gestor de documentos en espera de impresión |
| **Spooler** | Administrador de trabajos en espera; distribuye, informa y avisa |

Al diseñar un sistema de impresión en red se consideran tres factores:
1. **Elección de dispositivos**: calidad, volumen y velocidad.
2. **Asignación a equipos**: potencia y almacenamiento del servidor de impresión; posibilidad de asignar por departamentos.
3. **Acceso**: derechos de acceso por usuario o grupo.

### 3.6 Gestión de fax

Aunque el fax es tecnología en desuso, los temarios de oposiciones lo incluyen porque muchas administraciones públicas aún lo usan. La gestión de fax en un NOS implica tres pasos:

1. **Preparación del módem/fax**: configurar velocidad de transmisión, puerto de conexión, etc.
2. **Configuración del software en el NOS**: gestiona envío y recepción a través de la línea telefónica.
3. **Integración en el sistema de mensajería**: el fax se incorpora al correo electrónico para recepción centralizada y distribución a los usuarios correspondientes (fax-to-email).

---

## 4. Monitorización y Control de Tráfico

### 4.1 Introducción

Si no mides, no puedes mejorar. La **monitorización de redes** sirve exactamente para eso: detectar problemas antes de que los usuarios los noten y tener datos para justificar mejoras de infraestructura. Las funciones de gestión de red se dividen en dos categorías:

- **Monitorización**: analiza el estado de la red y sus componentes (lectura, observación).
- **Control**: implementa acciones correctoras ajustando la configuración de los componentes (escritura, actuación).

La **información de monitorización** se clasifica en:

| Tipo | Descripción |
|---|---|
| **Estática** | Configuración actual de la red y sus elementos |
| **Dinámica** | Eventos en tiempo real: paquetes enviados/recibidos, obtenidos directamente o a través de elementos de monitorización |
| **Estadística** | Basada en la información dinámica: número medio de paquetes transmitidos, informes de gestión |

El ciclo de optimización de una red se resume en: **monitorizar → evaluar → mejorar**.

### 4.2 Problemas de rendimiento y soluciones

| Problema | Solución |
|---|---|
| CPU del servidor sobrecargada | Actualización de hardware, adición de procesadores, distribución de carga |
| Escasez de memoria | Ampliación de memoria; evitar uso excesivo de paginación |
| Disco duro lento | Tecnologías de acceso más rápido (SSD/NVMe), aumento del número de discos |
| Tráfico de red saturado | Monitorización de colisiones, segmentación con switches, cambio tecnológico |

Ante problemas de elevado tráfico en la red, las soluciones son:

1. **Cambio de topología**: segmentación con más switches y pasarelas.
2. **Identificación y concentración de nodos generadores de tráfico**: agruparlos en segmentos más rápidos.
3. **Ajuste de la planificación de subredes**: modificar máscaras de red.
4. **Chequeo de cables y electricidad**: verificar longitudes y especificaciones eléctricas.
5. **Cambio tecnológico**: pasar de Ethernet a Fast Ethernet, Gigabit Ethernet, o fibra óptica.
6. **Desconexión de equipos problemáticos**.
7. **Implementar analizadores de red** para generación de estadísticas.

### 4.3 SNMP: Simple Network Management Protocol

**SNMP** es el protocolo estándar de gestión de redes y el más preguntado en el examen de este bloque. Funciona con un mecanismo de pregunta/respuesta: el gestor pregunta al agente, el agente responde. La información disponible en cada dispositivo se describe en una base de datos jerárquica llamada **MIB (Management Information Base)**, y cada dato tiene un identificador único llamado **OID (Object Identifier)**.

#### Tres componentes de una red gestionada por SNMP

El diagrama conceptual de SNMP muestra tres componentes interconectados:
- **NMS (Network Management System)**: el gestor; ejecuta aplicaciones para supervisar y controlar los dispositivos administrados.
- **Dispositivos Administrados**: routers, switches, servidores que ejecutan el agente SNMP y exponen su MIB.
- **Agentes**: software en los dispositivos que recopila información y la pone a disposición del NMS; envía **traps** (alarmas) ante eventos.

#### Puertos SNMP

> **Dato de examen**: SNMP utiliza **UDP 161** (el agente escucha solicitudes del gestor) y **UDP 162** (el gestor escucha notificaciones/traps de los agentes).

#### Versiones de SNMP

| Versión | RFC | Seguridad | Características |
|---|---|---|---|
| **SNMPv1** | RFC 1157 (1990) | Community string en texto claro | Versión original; insegura |
| **SNMPv2c** | RFC 1901 (1996) | Community string en texto claro | Añade GetBulkRequest; más eficiente |
| **SNMPv3** | RFC 3411-3418 (2002) | Autenticación (MD5/SHA) + cifrado (DES/AES) | Versión recomendada actualmente |

Las **community strings** en SNMPv1/v2c actúan como contraseñas en texto claro. Las más comunes por defecto son `public` (lectura) y `private` (escritura), lo que representa un riesgo de seguridad.

Las aplicaciones de monitorización suelen abordar las **áreas de control sugeridas por la ISO**: seguridad, rendimiento, configuración de dispositivos y control de fallos.

### 4.4 RMON: Remote Network Monitoring

**RMON** (Remote Network MONitoring) es una especificación del **IETF** que extiende SNMP con capacidades de monitorización proactiva. La gracia de RMON es que usa **sondas** distribuidas por la red que recogen estadísticas localmente y las reportan al NMS, en lugar de que el NMS tenga que ir preguntando continuamente.

> **Precisión de examen**: RMON es del **IETF** (no del IEEE). Su función es **monitorización** (no administración ni gestión genérica). Esto sale en el examen.

| Versión | RFC | Capas OSI monitorizadas |
|---|---|---|
| **RMON 1** | [RFC 2819](https://datatracker.ietf.org/doc/html/rfc2819) (2000) | Capas 1 y 2 (física y MAC/enlace) |
| **RMON 2** | [RFC 4502](https://datatracker.ietf.org/doc/rfc4502/) (2006) | Extiende a **capas superiores** (red, transporte, aplicación: capas 3-7) |

RMON 1 proporciona nueve grupos de estadísticas para Ethernet: estadísticas, historial, alarmas, hosts, hostTopN, matriz, filtros, captura de paquetes y eventos.

**SMON (Switched Monitoring)** es una extensión más avanzada que puede monitorizar dispositivos de red y redes privadas virtuales (VLANs).

Otros protocolos relacionados:
- **SNMPv2**: evolución de SNMP.
- **DMI (Desktop Management Interface)**: gestión de escritorio.
- **CMIP (Common Management Information Protocol)**: basado en el modelo OSI, definido por las recomendaciones **ITU-T X.700**.

### 4.5 Herramientas de monitorización de red

Existe un ecosistema amplio de herramientas, desde soluciones comerciales de gama alta hasta software libre. Para el examen lo importante es saber qué hace cada una y no confundirlas entre sí.

#### SolarWinds

Plataforma comercial de monitorización de redes, servidores y aplicaciones. Su punto fuerte es la visualización: hace **mapeo automático de la red** y muestra en el panel NPM (Network Performance Monitor) el estado de todos los nodos con semáforos (verde/amarillo/rojo). Útil para detectar cuellos de botella de un vistazo.

#### Wireshark (anteriormente Ethereal)

Analizador de protocolos de red muy utilizado en la administración de redes. Permite analizar el tráfico capturado en archivos generados por herramientas como **TCPDump**.

#### TCPDump (Linux/UNIX) y WinDump (Windows)

Herramientas de línea de comandos para capturar y guardar paquetes de red en archivos para análisis posterior con Wireshark u otras herramientas.

#### Pandora FMS

Herramienta de monitorización para redes, servidores y aplicaciones. La versión gratuita puede monitorizar desde 100 dispositivos hasta varios miles de nodos.

#### Nagios

Herramienta de supervisión de código abierto ampliamente conocida. Su núcleo permite la instalación de complementos para supervisar elementos de red específicos. Versiones: **Nagios Core** (gratuita) y **Nagios XI** (comercial).

#### ManageEngine OpManager

Monitorización de redes, servidores y aplicaciones con gestión avanzada del rendimiento y disponibilidad de recursos críticos como routers, switches, firewalls y controladores de dominio.

#### Zabbix

Herramienta de monitorización con interfaz gráfica potente y configuración sencilla. Puede monitorizar servicios de red, servidores y hardware **sin necesidad de instalar agentes** (mediante SNMP o ICMP).

#### Otras herramientas

| Herramienta | Descripción |
|---|---|
| **GroundWork** | Combina Nagios y otras herramientas especializadas para una solución global |
| **Zenoss** | Monitoriza redes, servidores, aplicaciones, almacenamiento y servidores virtuales; versión comunitaria y comercial |
| **Monitis** | Solución popular para pymes |
| **Icinga** | Derivada de Nagios; mejora la interfaz gráfica; diseñada para redes complejas |
| **op5 Monitor** | Basada en Nagios; enfocada en hardware, tráfico y servicios |
| **OpenNMS** | Software libre y flexible; requiere aprendizaje para su uso |

### 4.6 Comandos de diagnóstico de red

El banco de preguntas incluye escenarios de diagnóstico. Los comandos más relevantes:

| Comando | Sistema | Función |
|---|---|---|
| `ipconfig` | Windows | Muestra configuración IP básica de los adaptadores |
| `ipconfig /all` | Windows | Configuración completa: IP, máscara, gateway, DNS, MAC, estado DHCP |
| `ipconfig /release` | Windows | Libera la IP asignada por DHCP |
| `ipconfig /renew` | Windows | Solicita nueva IP al servidor DHCP |
| `ipconfig /flushdns` | Windows | Vacía la caché DNS |
| `ip addr show` / `ifconfig` | Linux | Equivalente a `ipconfig` en Linux |
| `cat /etc/resolv.conf` | Linux | Muestra el servidor DNS configurado |
| `ping [destino]` | Ambos | Comprueba conectividad (ICMP Echo); verifica también resolución DNS |
| `tracert [destino]` | Windows | Traza la ruta hasta el destino, mostrando cada salto |
| `traceroute [destino]` | Linux | Equivalente a `tracert` en Linux |
| `netstat -a` | Ambos | Muestra **todas las conexiones TCP activas y puertos UDP/TCP en escucha** |
| `netstat -r` | Ambos | Muestra la tabla de enrutamiento |
| `route print` | Windows | Muestra la tabla de enrutamiento (no muestra DNS) |
| `nslookup` | Ambos | Consulta servidores DNS |

> **Distinción de examen**: `ipconfig` muestra la configuración IP; `netstat -a` muestra conexiones activas y puertos en escucha; `route print` muestra la tabla de enrutamiento. No confundir sus funciones.

El **descubrimiento automático de impresoras** (usando WS-Discovery, mDNS/Bonjour) se basa en tráfico **broadcast o multicast**. Los routers **no reenvían tráfico broadcast** entre redes distintas, por lo que el descubrimiento falla si la impresora está en una red diferente aunque el `ping` (unicast) funcione correctamente.

### 4.7 Compartición de recursos: SMB/CIFS y NFS

**SMB (Server Message Block)** / **CIFS (Common Internet File System)** es el protocolo nativo de Windows para **compartir archivos e impresoras**. Opera sobre los puertos **TCP 445** y **TCP 139**. CIFS fue publicado por Microsoft como SMB 1.0 en 1996. Las versiones modernas (SMB 2.0, 2.1, 3.0, 3.1.1) ya no usan la denominación CIFS.

**NFS (Network File System)** es el protocolo de compartición de archivos en entornos Unix/Linux.

**Samba** es una implementación libre del protocolo SMB para sistemas Unix/Linux, que permite la interoperabilidad con clientes Windows.

| Protocolo | Entorno | Función |
|---|---|---|
| **SMB/CIFS** | Windows | Compartir archivos e impresoras en red |
| **NFS** | Unix/Linux | Compartir sistemas de archivos en red |
| **Samba** | Unix/Linux | Implementación de SMB para interoperabilidad con Windows |

### 4.8 DHCP y el proceso DORA

**DHCP (Dynamic Host Configuration Protocol)** evita tener que configurar manualmente la IP en cada equipo. Cuando un equipo se conecta, negocia automáticamente su configuración (IP, máscara, puerta de enlace, DNS) con un servidor DHCP. El proceso de obtención de IP se denomina **DORA** y es el que suele preguntar el examen:

1. **D**iscover: el cliente envía un broadcast buscando servidores DHCP.
2. **O**ffer: el servidor DHCP responde con una oferta de IP.
3. **R**equest: el cliente solicita la IP ofrecida.
4. **A**cknowledge: el servidor confirma la asignación.

> **Rogue DHCP**: si se instala un servidor DHCP no autorizado que responde más rápido que el servidor legítimo, **muchos** (no todos) de los equipos que soliciten una nueva concesión recibirán configuración incorrecta. Los equipos con concesión activa no se ven afectados de inmediato. La protección es el **DHCP Snooping** en switches gestionables.

La renovación (T1) se hace en **unicast** directamente al servidor original, por lo que el rogue DHCP puede no interceptarla. Solo en T2 (expiración del tiempo de renovación) el cliente hace broadcast y el rogue puede intervenir.

### 4.9 Autenticación WiFi: WPA2-Enterprise y RADIUS

En una red WiFi corporativa no puedes poner una clave compartida para todos: si alguien se va de la empresa, ¿cambias la clave para 500 usuarios? La solución es **WPA2-Enterprise**, que usa credenciales individuales a través del protocolo **RADIUS**.

**RADIUS (Remote Authentication Dial-In User Service)** es el protocolo estándar AAA (Authentication, Authorization, Accounting), definido en [RFC 2865](https://www.rfc-editor.org/rfc/rfc2865). Es el protocolo utilizado para **autenticar y autorizar usuarios en una red**.

#### WPA2-Personal vs. WPA2-Enterprise

| Aspecto | WPA2-Personal (PSK) | WPA2-Enterprise (802.1X) |
|---|---|---|
| **Autenticación** | Clave precompartida (PSK) | 802.1X + RADIUS |
| **Uso típico** | Hogar, pymes | Entornos corporativos |
| **Gestión de credenciales** | Una clave para todos | Credenciales individuales |
| **Revocación de acceso** | Cambiar la clave para todos | Deshabilitar usuario en el directorio |

La arquitectura de autenticación WiFi Enterprise sigue este flujo:

```
Cliente WiFi ←[EAP/802.1X]→ Controladora WiFi ←[RADIUS]→ Servidor RADIUS ←[LDAP/Kerberos]→ AD/eDirectory
```

El protocolo que configura la **controladora WiFi** para la autenticación con el directorio es **RADIUS** (no LDAP directamente). LDAP lo usa el servidor RADIUS internamente para validar credenciales contra Active Directory o eDirectory.

Servidores RADIUS comunes: **Microsoft NPS** (Network Policy Server), **FreeRADIUS** (open source), **Cisco ISE**.

Evolución de seguridad WiFi:
- **WEP**: deprecado en 2004; inseguro (descifrable en minutos).
- **WPA**: TKIP; mejoró WEP pero también superado.
- **WPA2**: AES-CCMP; estándar dominante durante años.
- **WPA3**: SAE (Simultaneous Authentication of Equals); protección contra ataques de diccionario; cifrado de 192 bits en Enterprise.

### 4.10 TETRA: Sistema de Radiocomunicación

**TETRA** son las siglas de **TErrestrial Trunked RAdio**. El nombre original en ETSI era *Trans-European Trunked Radio*, pero la denominación oficial actual es **Terrestrial Trunked Radio**. Es un estándar de radio digital troncalizada desarrollado por **ETSI** (European Telecommunications Standards Institute), ampliamente utilizado por servicios de seguridad pública (policía, bomberos, emergencias médicas).

> **Trampa de examen**: algunas preguntas ponen "Trans European Trunked Radio" como opción. Ojo: ese era el nombre original histórico, pero el nombre oficial vigente es **Terrestrial** Trunked Radio. Presta atención a cómo formula la pregunta tu examen concreto.

#### Características técnicas

| Característica | Valor |
|---|---|
| **Organismo** | ETSI (estándar EN 300 392) |
| **Modulación** | π/4 DQPSK |
| **Acceso múltiple** | **TDMA** (Time Division Multiple Access) con **4 slots** por portadora de 25 kHz |
| **Velocidad de datos** | Hasta 28,8 kbps |
| **Cifrado** | TEA1, TEA2, TEA3 (extremo a extremo) |

> **Dato crítico de examen**: TETRA usa **TDMA** (no FDMA ni CDMA). Cada portadora de 25 kHz se divide en **4 ranuras de tiempo**, permitiendo que 4 usuarios compartan la misma frecuencia.

#### Bandas de frecuencia TETRA

| Banda | Uso |
|---|---|
| **380-400 MHz** | Sistemas de seguridad pública y emergencias (policía, bomberos, SAMU) |
| **410-430 MHz** | Sistemas civiles y comerciales (transporte, utilities) |
| 450-470 MHz / 870-876 MHz | Uso adicional según país |

#### Comparativa de técnicas de acceso múltiple

| Técnica | División por | Sistemas que la usan |
|---|---|---|
| **FDMA** | Frecuencia | PMR analógico, AMPS |
| **TDMA** | Tiempo | **TETRA**, GSM, D-AMPS |
| **CDMA** | Código | UMTS (3G), CDMA2000 |
| **OFDMA** | Frecuencia + Tiempo | LTE (4G), Wi-Fi 6 (802.11ax) |

#### Interfaces estándar de TETRA

Para garantizar un mercado abierto e interoperabilidad, TETRA define interfaces estándar:
- **Interfaz Aire (Air Interface)**: comunicación entre terminales y estaciones base.
- **Interfaz de Equipo Terminal (TEI/PEI)**: conexión entre el terminal de radio y dispositivos externos.
- **Interfaz entre Sistemas (ISI)**: interoperabilidad entre infraestructuras de distintos fabricantes.
- **Modo Directo (DMO)**: comunicación directa entre terminales sin infraestructura.

> **Nota de examen**: **"Interfaz Modo Indirecto"** no existe en TETRA. Es un término ficticio. Las interfaces reales son: Interfaz Aire, Interfaz de Equipo Terminal e Interfaz entre Sistemas.

#### Tipos de llamadas en TETRA

La ventaja de TETRA frente a la radio analógica convencional es que permite **voz y datos en tiempo real** con cifrado. Para coordinación de emergencias ofrece:

- **Llamadas individuales**: comunicación punto a punto con cifrado.
- **Llamadas grupales**: comunicación simultánea con todos los miembros de un grupo (coordinación táctica).
- **Llamadas de prioridad**: niveles de prioridad para no bloquear comunicaciones urgentes.
- **Push-To-Talk (PTT)**: comunicación instantánea sin marcar.
- **Modo Directo (DMO)**: funciona sin infraestructura de red.
- **Localización GPS integrada**.

---

## Resumen para el examen

Los siguientes puntos concentran el mayor número de preguntas del banco para este tema:

1. **SNMP usa UDP 161 y UDP 162**. UDP 161 para solicitudes gestor→agente; UDP 162 para traps/notificaciones agente→gestor.
2. **RMON es del IETF** (no del IEEE); RMON 1 = [RFC 2819](https://datatracker.ietf.org/doc/html/rfc2819), monitoriza capas 1-2; RMON 2 = [RFC 4502](https://datatracker.ietf.org/doc/rfc4502/), extiende a **capas superiores de red** (3-7).
3. **TETRA = Terrestrial Trunked Radio**, estándar ETSI, usa **TDMA** (4 slots por portadora de 25 kHz), **no FDMA ni CDMA**. Bandas: **380-400 MHz** (emergencias), **410-430 MHz** (civiles).
4. **TETRA permite comunicación de voz y datos en tiempo real**: llamadas individuales, grupales y de prioridad; PTT; cifrado.
5. **"Interfaz Modo Indirecto" no existe en TETRA**. Las interfaces reales son: Aire, Equipo Terminal, entre Sistemas.
6. **NAS usa NFS y CIFS** como protocolos de compartición. NTFS y FAT son sistemas de archivos locales, no protocolos de red.
7. **SMB/CIFS**: protocolo de compartición de archivos e impresoras de Windows; puertos TCP 445 y 139.
8. **RADIUS** es el protocolo AAA (Authentication, Authorization, Accounting) para autenticación en red; en WPA2-Enterprise, la controladora WiFi habla RADIUS con el servidor de autenticación (no LDAP directamente).
9. **WPA2-Enterprise** = 802.1X + RADIUS; WPA2-Personal = PSK. WEP está deprecado y es inseguro.
10. **LDAP DN**: de más específico a más general: `CN=usuario,OU=dept,DC=empresa,DC=es`. RFC clave: [RFC 4511](https://www.rfc-editor.org/rfc/rfc4511) (protocolo), [RFC 4514](https://www.rfc-editor.org/rfc/rfc4514) (representación DN), [RFC 4519](https://www.rfc-editor.org/rfc/rfc4519) (atributos de usuario).
11. **JNDI** (Java Naming and Directory Interface) es la API de Java para acceder a directorios. No confundir con JMS (mensajería) ni JTA (transacciones).
12. **El objetivo principal de un dominio** es unificar la autenticación y autorización (Single Sign-On). No es solo "proporcionar acceso a recursos" (consecuencia) ni "supervisar usuarios" (función secundaria).
13. **ADUC (Active Directory Users and Computers)** es la herramienta gráfica estándar para gestionar directorios y autenticación en Windows.
14. **`ipconfig`** muestra configuración IP; **`netstat -a`** muestra conexiones activas y puertos en escucha; **`traceroute/tracert`** traza la ruta y detecta retrasos; **`ping`** verifica conectividad básica.
15. **El descubrimiento automático de impresoras usa broadcast/multicast**. Los routers no reenvían broadcast, por lo que si la impresora está en otra red, el ping funciona (unicast) pero el descubrimiento falla.
16. **Rogue DHCP**: un servidor DHCP no autorizado más rápido afecta a "muchos" (no a todos) los equipos que soliciten IP nueva. Protección: **DHCP Snooping**.
17. **RAID 0**: striping, sin redundancia, alto rendimiento. **RAID 1**: mirroring. **RAID 5**: striping + paridad distribuida (mínimo 3 discos). **RAID 6**: paridad doble (mínimo 4 discos). **Hot swap** = reemplazar disco sin apagar. **Hot spare** = disco de reserva en espera.
18. **DNS no es un protocolo de administración de red**; es un servicio de resolución de nombres. SNMP, RMON e ICMP sí son protocolos de administración/diagnóstico.
19. **Tipos de backup**: Full (todo cada vez), Differential (desde último full), Incremental (desde último incremental).
20. **Split-horizon DNS**: instalar un servidor DNS interno que resuelva nombres públicos a IPs privadas es la solución más mantenible y centralizada para evitar que el tráfico salga a Internet y vuelva por NAT.

---

## Preguntas frecuentes en exámenes

### Subtemas con mayor presencia en el banco de preguntas

**1. TETRA (6 preguntas)** — El subtema más examinado. Se pregunta sobre:
- El significado exacto del acrónimo: **"Terrestrial Trunked Radio"**. Las opciones trampa habituales son "Trunk Ratio", "Trunking Ratio" y variantes absurdas. Nota: "Trans-European Trunked Radio" era el nombre original histórico, pero el examen suele buscar la denominación oficial actual.
- El tipo de acceso al medio: **TDMA** (no FDMA, no CDMA).
- Las bandas de frecuencia: **380-400 MHz** y **410-430 MHz**.
- Las interfaces estándar: se pregunta cuál NO existe ("Interfaz Modo Indirecto" es la trampa).
- Las características que facilitan la coordinación de emergencias: **llamadas individuales, grupales y de prioridad** (no videoconferencia ni CDMA/internet).

**2. SNMP y RMON (4 preguntas)** — Se pregunta sobre:
- Puertos: **UDP 161 y UDP 162** (no TCP, no otros números).
- RMON 1 = **RFC 2819**; RMON 2 = RFC 4502.
- RMON 2 monitoriza las **capas superiores de red** (3-7), no solo capas 1-2.
- RMON es del **IETF** y su función es **monitorización** (no administración ni gestión genérica).

**3. RADIUS y autenticación (3 preguntas)** — Se pregunta sobre:
- En WPA2-Enterprise, la controladora WiFi usa **RADIUS** (no LDAP, no NCP).
- RADIUS es el protocolo **AAA** para autenticar y autorizar usuarios en redes (no SSH, no SSL/TLS).
- Diferencia WPA2-Personal (PSK) vs. WPA2-Enterprise (802.1X + RADIUS).

**4. LDAP y Active Directory (3 preguntas)** — Se pregunta sobre:
- Sintaxis del DN: **`CN=usuario,OU=dept,O=org`** (formato tipado LDAP; el orden invertido sin tipos es la trampa).
- JNDI como API Java de directorios (no JMS, no JTA).
- ADUC como herramienta gráfica de gestión (no PowerShell, no "System Manager").
- El objetivo del dominio: **unificar autenticación y autorización**.

**5. Almacenamiento y NAS (2 preguntas)** — Se pregunta sobre:
- Protocolos de NAS: **NFS y CIFS** (no NTFS ni FAT, que son sistemas de archivos locales).
- SMB/CIFS: para compartir archivos e impresoras (no para proxy inverso ni multimedia).

**6. DHCP y redes (2 preguntas)** — Se pregunta sobre:
- Rogue DHCP: afecta a "muchos" equipos que soliciten IP nueva (no "todos").
- Split-horizon DNS: solución más mantenible para acceso interno a servidores por nombre público.

**7. Diagnóstico de red y comandos (4 preguntas)** — Se pregunta sobre:
- `ipconfig`: verificar configuración IP en Windows.
- `netstat -a`: conexiones activas y puertos en escucha (no configuración IP).
- `traceroute`/`tracert`: trazar ruta y detectar retrasos (no `ping`, que no muestra la ruta).
- `ping`: verificar conectividad y resolución DNS básica.
- Descubrimiento automático de impresoras: falla entre redes distintas porque los routers no reenvían broadcast.

---

## Referencias

- [RFC 4511 — LDAP: The Protocol (LDAPv3)](https://www.rfc-editor.org/rfc/rfc4511) — Especificación completa del protocolo LDAP versión 3.
- [RFC 4514 — LDAP: String Representation of Distinguished Names](https://www.rfc-editor.org/rfc/rfc4514) — Sintaxis estándar del DN.
- [RFC 4519 — LDAP: User Schema](https://www.rfc-editor.org/rfc/rfc4519) — Atributos estándar de usuario en LDAP.
- [RFC 2865 — RADIUS](https://www.rfc-editor.org/rfc/rfc2865) — Especificación del protocolo RADIUS (AAA).
- [RFC 2819 — RMON 1 MIB](https://datatracker.ietf.org/doc/html/rfc2819) — Remote Network Monitoring Management Information Base.
- [RFC 4502 — RMON 2 MIB](https://datatracker.ietf.org/doc/rfc4502/) — Versión 2 de RMON, monitorización en capas superiores.
- [RFC 2131 — DHCP](https://www.rfc-editor.org/rfc/rfc2131) — Protocolo de configuración dinámica de host.
- [IEEE 802.1X](https://en.wikipedia.org/wiki/IEEE_802.1X) — Estándar de control de acceso a redes de área local basado en puertos.
- [ETSI TETRA](https://www.etsi.org/technologies/tetra) — Portal oficial del estándar TETRA en ETSI.
- [TCCA — TETRA + Critical Communications Association](https://tcca.info/) — Información técnica y de despliegue de TETRA.
- [Wi-Fi Alliance — WPA3](https://www.wi-fi.org/discover-wi-fi/security) — Especificaciones de seguridad Wi-Fi.
- [Microsoft — Active Directory Domain Services](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) — Documentación oficial de AD.
- [Microsoft — SMB Protocol Overview](https://learn.microsoft.com/en-us/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) — Descripción del protocolo SMB/CIFS.
- [Oracle — JNDI Documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/jndi/) — API Java para acceso a directorios.
- [RedIRIS — Guía Básica LDAP](https://www.rediris.es/ldap/doc/gb/gb-ldap-02.html) — Guía de estructura y nombres en directorios LDAP.
- [Wikipedia — LDAP](https://es.wikipedia.org/wiki/Protocolo_ligero_de_acceso_a_directorios) — Artículo en español sobre LDAP.
- [Wikipedia — TETRA](https://es.wikipedia.org/wiki/TETRA) — Artículo en español sobre TETRA.
- [Nagios](https://www.nagios.org/) — Herramienta de monitorización open source.
- [Zabbix](https://www.zabbix.com/) — Plataforma de monitorización empresarial.
- [SolarWinds](https://www.solarwinds.com/) — Suite comercial de monitorización de redes.
