# Tema 1: Administración del Sistema Operativo y Software de Base

> Este tema cubre los conceptos fundamentales del sistema operativo y el software de base, las funciones y responsabilidades del administrador del sistema, el control de cambios en instalaciones software (GCS/ECS/CCC) y los procedimientos de actualización, mantenimiento y reparación del SO en entornos Windows, Linux y móviles. Es uno de los temas con mayor peso práctico del Bloque IV: aparece tanto en preguntas de concepto como en escenarios aplicados de administración. El estudiante debe dominar la terminología exacta (BIOS vs. UEFI, GCS, ECS, CCC), los comandos concretos de cada plataforma y el flujo completo de gestión de cambios.

---

## 1. Sistema Operativo

### 1.1 Definición e introducción

El **sistema operativo (SO)** es el software principal de un ordenador. Su función es **administrar los recursos de hardware** del equipo y **proporcionar servicios a los programas de aplicación**. Funciona en modo privilegiado (modo kernel) y puede ejecutarse en parte en el espacio de usuario.

Una distinción importante: en algunos textos se usa "SO" como sinónimo de "núcleo" o **kernel**, pero esta equivalencia solo es correcta cuando el núcleo es de tipo **monolítico** (el más habitual en los sistemas clásicos). El núcleo Linux, por ejemplo, es el núcleo sobre el que se construyen las distribuciones GNU/Linux.

El kernel se ocupa de la gestión de dispositivos, memoria, procesos y llamadas al sistema. **No gestiona las interfaces gráficas**: eso es responsabilidad del espacio de usuario (servidor de ventanas X.org o Wayland, entorno de escritorio). El kernel solo proporciona acceso al hardware gráfico mediante drivers (DRM/KMS).

La mayoría de los dispositivos electrónicos con microprocesadores (teléfonos móviles, reproductores multimedia, ordenadores, enrutadores) incluyen un SO. Dependiendo del dispositivo, la interfaz puede ser gráfica (GUI), de línea de comandos (CLI), o incluso un control remoto.

**Representación de la información en un sistema informático:** se refiere a la transformación de los datos en una forma comprensible para las computadoras, es decir, su codificación en binario (secuencias de 0s y 1s). Incluye la representación de números enteros (complemento a dos), números reales (IEEE 754), texto (ASCII, Unicode/UTF-8), imágenes y audio. No debe confundirse con la presentación en pantalla (función de la interfaz de usuario) ni con la conversión de formatos entre sistemas operativos (interoperabilidad).

**Arquitecturas de procesador:**

| Arquitectura | Memoria | Ventaja principal | Uso habitual |
|-------------|---------|------------------|--------------|
| **Von Neumann** | Una sola memoria compartida para datos e instrucciones | Sencillez y economía | PCs, servidores (x86, ARM Cortex-A) |
| **Harvard** | Memorias separadas para datos e instrucciones | Acceso simultáneo a datos e instrucciones en el mismo ciclo | Microcontroladores (PIC, AVR), DSPs |

> La principal ventaja de la arquitectura Harvard sobre Von Neumann es que permite acceder simultáneamente a instrucciones y datos, eliminando el cuello de botella del bus compartido. Las CPUs modernas tipo x86 implementan Harvard modificado en las cachés L1 (separadas para datos e instrucciones) y Von Neumann a partir de L2.

### 1.2 Componentes de un SO

Un SO no es un único programa monolítico, sino un conjunto de subsistemas que colaboran entre sí. El diagrama clásico muestra ocho bloques interconectados con flechas bidireccionales: **Control de Procesos**, **Planificación de Procesos**, **Gestión de Memoria**, **Gestión de Dispositivos**, **Gestión de Archivos**, **Concurrencia de Procesos**, **Seguridad** y **Comunicaciones**. Todos interactúan entre sí formando una red de dependencias mutuas. A continuación se detalla cada uno.

#### 1.2.1 Gestión de procesos

Un **proceso** es un programa en ejecución que requiere recursos: tiempo de CPU, memoria, archivos y dispositivos de E/S. El SO tiene las siguientes responsabilidades respecto a los procesos:

- Crear y finalizar procesos.
- Suspender y reanudar procesos.
- Facilitar la comunicación y la sincronización entre procesos.

> Analogía didáctica: la gestión de procesos puede compararse con la organización del trabajo en una oficina donde las tareas tienen prioridades (alta, media, baja).

**Proceso zombie en Unix/Linux:** un proceso que ha finalizado y ha liberado todos sus recursos, pero cuya entrada en la tabla de procesos permanece porque el proceso padre no ha llamado a `wait()` o `waitpid()`. El kernel mantiene esta entrada mínima para que el padre pueda consultar el código de retorno. Si el padre muere antes, el proceso init (PID 1) adopta al huérfano y lo limpia.

```bash
# Ver procesos zombie
ps aux | grep Z
```

**Sesión en Linux:** conjunto de uno o más grupos de procesos con un identificador de sesión (SID) común y una terminal de control compartida. Un proceso es una unidad independiente de ejecución; una sesión es una colección de procesos y recursos relacionados.

#### 1.2.2 Gestión de la memoria principal

La **memoria principal** es una tabla de palabras o bytes con direcciones únicas. Es un recurso compartido entre la CPU y los dispositivos de E/S, es **volátil** (pierde su contenido cuando se corta la alimentación) y es finita. Como varios procesos compiten por ella simultáneamente, el SO actúa de árbitro.

El SO tiene las siguientes responsabilidades:

- Supervisar el uso de la memoria y qué proceso la está utilizando.
- Decidir qué procesos se cargarán en memoria cuando haya espacio disponible.
- Asignar y liberar espacio de memoria según sea necesario.

**Tipos de memoria RAM:**

| Tipo | Tecnología | Refresco | Uso típico |
|------|-----------|----------|-----------|
| **SRAM** (Static RAM) | Biestables (flip-flops, 4-6 transistores) | No necesita | Cachés L1, L2, L3 de CPU |
| **DRAM** (Dynamic RAM) | Condensadores + transistor por celda | Necesita refresco periódico (~64 ms) | Memoria principal (RAM del sistema) |
| **SDRAM** (Synchronous DRAM) | DRAM sincronizada con el reloj del sistema | Necesita refresco | Base de los módulos actuales (DDR4, DDR5) |

> Pregunta frecuente de examen: SDRAM es un tipo de DRAM sincronizada con el reloj del sistema. La SRAM es la basada en biestables y NO necesita refresco; la DRAM es la basada en condensadores que SÍ necesitan refresco.

#### 1.2.3 Gestión del almacenamiento secundario

La memoria principal es volátil y limitada. El SO gestiona el almacenamiento secundario (discos duros, SSD, etc.) para:

- Planificar el uso de los dispositivos de almacenamiento.
- Administrar el espacio libre disponible.
- Asignar y gestionar el almacenamiento para programas y datos.
- Garantizar que los datos se guarden de manera ordenada y segura.

**Sistemas de archivos habituales:**

| Sistema de archivos | SO | Notas |
|--------------------|----|-------|
| **NTFS** | Windows | Nativo de Windows NT y superiores. Soporta journaling, permisos, cifrado. |
| **FAT32** | Windows/multiplataforma | Límite de 4 GB por archivo. Compatible con casi todo. |
| **ext2 / ext3 / ext4** | Linux | Nativos de Linux. ext4 es el más usado actualmente. |
| **XFS** | Linux (Red Hat) | Alto rendimiento, usado por defecto en RHEL. |
| **Btrfs** | Linux (SUSE/Fedora) | Moderno, soporta snapshots y RAID. |
| **ReiserFS** | Linux (SUSE) | Primer FS con journaling en el kernel oficial (2001). |
| **HFS+ / APFS** | macOS | Sistemas de archivos de Apple. |

> Pregunta de examen: NTFS NO es nativo de ninguna distribución Linux (aunque Linux puede leerlo con `ntfs-3g`). ext2, ext3 y ReiserFS sí son nativos de Linux.

**Organización de ficheros secuencial:** los registros se almacenan en orden de inserción y se acceden recorriendo el fichero desde el inicio. Es la organización más simple; eficiente para procesar todos los registros, pero lenta para búsquedas puntuales.

#### 1.2.4 Sistema de entrada y salida (E/S)

Comprende un **sistema de almacenamiento temporal (caché)**, una interfaz para manejar dispositivos genéricos y otra para dispositivos específicos. El SO debe:

- Administrar el almacenamiento temporal (buffering) relacionado con la E/S.
- Manejar las interrupciones generadas por los dispositivos de E/S.

**Tipos de impresora** (pregunta frecuente en examen):

| Tipo | Funcionamiento | Uso típico |
|------|---------------|-----------|
| **Térmica** | Calor sobre papel térmico, sin tinta ni tóner | Tickets de compra, cajeros, TPV — bajo coste, mantenimiento mínimo |
| **Láser** | Tóner + tambor fotosensible + fusora | Oficinas (alto volumen, calidad) |
| **Inyección de tinta** | Inyecta gotitas de tinta sobre el papel | Doméstica/fotografía |
| **Sublimación de tinta** | Colorante vaporizado que se difunde en el sustrato | Fotografía profesional, alta calidad, cara |
| **Tinta sólida** | Cera coloreada fundida | Impresión en color de oficina |

> Para tickets de compra de bajo coste se usa siempre **impresora térmica**: no necesita consumibles (tinta/tóner), es rápida, silenciosa y muy fiable.

#### 1.2.5 Sistemas de archivos

El SO tiene las siguientes responsabilidades respecto a los archivos:

- Crear, eliminar y gestionar archivos y directorios.
- Ofrecer funciones para abrir, cerrar, leer y escribir archivos.
- Establecer la correspondencia entre archivos y unidades de almacenamiento físico.
- Realizar copias de seguridad de archivos para proteger los datos.

#### 1.2.6 Sistemas de protección

Los **sistemas de protección** son mecanismos que controlan el acceso de programas y usuarios a los recursos del sistema. El SO:

- Distingue entre acceso autorizado y no autorizado.
- Especifica los controles de seguridad que deben aplicarse.
- Garantiza que se utilicen los mecanismos de protección.

#### 1.2.7 Sistemas de comunicaciones

Para mantener comunicaciones con otros sistemas, el SO debe:

- Controlar el envío y la recepción de información a través de las interfaces de red.
- Crear y mantener puntos de comunicación que permitan a las aplicaciones enviar y recibir información.
- Establecer y gestionar conexiones virtuales entre aplicaciones locales y remotas.

#### 1.2.8 Programas de sistema

Los **programas de sistema** son aplicaciones de utilidad suministradas con el SO pero que no son parte integral de él. Ofrecen un entorno útil para el desarrollo y la ejecución de otros programas. Sus funciones incluyen: manipulación y modificación de archivos, información sobre el estado del sistema, soporte para lenguajes de programación y comunicación entre programas.

#### 1.2.9 Gestor de recursos

El SO actúa como un **gestor de recursos** y administra:

- La CPU (donde reside el microprocesador).
- Los dispositivos de E/S.
- La memoria principal.
- Los dispositivos de almacenamiento secundario.
- Los procesos en ejecución.

### 1.3 Sistemas operativos más comunes

El diagrama de cuota de mercado de escritorio (StatCounter, 2022) muestra:

| Sistema Operativo | Porcentaje |
|------------------|-----------|
| Windows | 75,44 % |
| OS X (macOS) | 15,17 % |
| Desconocido | 4,52 % |
| Linux | 2,55 % |
| Chrome OS | 2,31 % |
| FreeBSD | 0,01 % |
| Otros | ~0 % |

Sistemas GNU/Linux más conocidos: **Debian**, **Ubuntu**, **Mandriva**, **Puppy**, **Red Hat Enterprise Linux**, **SUSE**, **OpenSUSE**.

Otros SO destacados: OS X, Unix, Solaris, FreeBSD, OpenBSD, Google Chrome OS, Plan 9, HP-UX, ReactOS, BeOS.

**Windows vs Linux — características destacadas:**

| Característica | Windows destaca | Linux destaca |
|---------------|----------------|--------------|
| **Usabilidad** | Sí — interfaz gráfica intuitiva, pensada para usuarios no técnicos | Curva de aprendizaje mayor (aunque distribuciones como Ubuntu mejoran esto) |
| **Personalización** | Limitada (sistema propietario) | Alta — entornos de escritorio intercambiables, kernel recompilable |
| **Flexibilidad** | Principalmente escritorio corporativo | Servidores, supercomputadores, móviles, embebidos, routers... |
| **Coste** | Licencia propietaria | Generalmente gratuito (software libre) |

---

## 2. Software de Base

### 2.1 Definición

El **software de sistema**, o **software de base**, se encarga de controlar y comunicarse con el sistema operativo. Su función principal es proporcionar control sobre el hardware del ordenador y brindar soporte a otros programas, en contraposición al software de aplicación.

Ejemplos: bibliotecas como **OpenGL** y **PNG** para aceleración gráfica, demonios que controlan la temperatura y la velocidad del disco duro (`hdparm`), o la frecuencia del procesador (`cpudyn`).

Uno de los ejemplos más destacados de software de base es **Microsoft Windows**. El proyecto **GNU** ha desempeñado un papel fundamental proporcionando herramientas de programación que, combinadas con el núcleo Linux, forman las distribuciones **GNU/Linux** (software libre).

### 2.2 Cargadores de programas

El **cargador de programas** (*loader*) se encarga de cargar programas en la memoria desde sus ejecutables. Normalmente:

- Forma parte del núcleo del SO.
- Se carga al iniciar el sistema y permanece en memoria hasta que se apaga el ordenador.
- En sistemas con núcleo paginable, puede residir en una parte paginable de la memoria.
- En el entorno Unix, el cargador gestiona la llamada del sistema `execve()`.

### 2.3 Controladores de dispositivos (drivers)

Un **controlador de dispositivos** o **driver** es un programa informático que permite al SO interaccionar con los periféricos, proporcionando una interfaz para utilizar el dispositivo. Sin el driver adecuado, el SO no sabe cómo hablar con el hardware: la impresora, la tarjeta gráfica o el USB simplemente no funcionan.

- El número de drivers = el número de tipos de periféricos existentes.
- Existen drivers **oficiales** (genéricos ofrecidos por los SO) y versiones **no oficiales** de terceros.

El diagrama de la arquitectura muestra tres capas: en la parte superior las **aplicaciones de usuario**, en el medio el **núcleo del sistema operativo** (que contiene los controladores A, B y C), y en la parte inferior los **dispositivos** físicos (A, B, C). El Administrador de dispositivos de Windows permite actualizar, deshabilitar, desinstalar y analizar cambios de hardware.

**Diferencia entre driver y tabla de gestión de dispositivos (DM):** un driver es una capa de abstracción entre el software y el hardware; la DM gestiona la asignación de recursos (IRQ, puertos de E/S, DMA) a los dispositivos.

### 2.4 Compiladores

Un **compilador** es un programa informático que traduce un programa escrito en un lenguaje de programación a un lenguaje común (generalmente **lenguaje máquina**). Este proceso se denomina **compilación**.

El proceso de compilación tiene **2 tareas principales**:

1. **Análisis** (Frontend):
   - Análisis **léxico**: descompone el programa fuente en componentes léxicos (tokens).
   - Análisis **sintáctico**: agrupa los tokens en frases gramaticales.
   - Análisis **semántico**: verifica la validez semántica de las sentencias.

2. **Síntesis** (Backend):
   - Genera la salida en un lenguaje objetivo (código intermedio o código objeto).
   - Incluye fases de generación de código y optimización.

El diagrama del compilador muestra el flujo: **Programa Fuente** → **Compilador** → **Código objeto**. Si hay errores de sintaxis, el flujo regresa a la fase de edición y corrección.

**Frontend vs. Backend del compilador:**

- **Frontend**: analiza el código fuente, comprueba la validez, genera el árbol de derivación y rellena la tabla de símbolos.
- **Backend**: genera el código máquina a partir de los resultados del frontend.

El objetivo es que el mismo backend sirva para varios lenguajes y el mismo frontend genere código para varias plataformas.

**Estructuras de datos fundamentales:** el compilador (y el SO en general) hace un uso intensivo de estructuras como la **pila** (stack). Una pila es una estructura LIFO (Last In, First Out) con dos operaciones básicas: `push` (insertar) y `pop` (extraer el elemento del tope). Si se intenta hacer `pop` sobre una pila vacía, se produce una condición de error denominada **stack underflow** (la mayoría de lenguajes lanzan una excepción en este caso). El exceso de elementos genera un **stack overflow**.

En una pila LIFO con elementos insertados en orden A → B → C → D → E, el primero en salir al hacer `pop()` es **E** (el último en entrar). El primero en entrar (A) es el último en salir.

**Características destacadas del lenguaje Java** (frecuente en exámenes de context Java EE):
- **Sí soporta programación genérica** (generics, desde Java 5): permite definir clases y métodos con tipos parametrizados, aportando seguridad de tipos en tiempo de compilación.
- **NO permite sobrecarga de operadores** (a diferencia de C++): los operadores tienen comportamiento fijo.
- **NO tiene punteros explícitos** (a diferencia de C/C++): la gestión de memoria es automática mediante el recolector de basura (Garbage Collector).

### 2.5 Programas utilitarios

Una **utilidad** es una herramienta que realiza:

- Tareas de mantenimiento.
- Soporte para la construcción y ejecución de programas.
- Tareas informáticas en general.

No se incluyen en esta categoría las bibliotecas de sistemas, middleware ni herramientas de desarrollo. El diagrama de capas muestra la jerarquía: **Hardware** → **Sistema Operativo** → **Utilidades** → **Programas de Aplicación** → **Usuario Final**. Las utilidades están a caballo entre el software de sistema y el software de aplicación, siendo usadas tanto por el programador como por el usuario final.

**Groupware:** software diseñado para facilitar el trabajo en grupo. Se clasifica en tres categorías funcionales:

| Categoría | Función | Herramientas típicas |
|-----------|---------|---------------------|
| **Comunicación** | Intercambio de información | Correo electrónico, mensajería instantánea |
| **Colaboración** | Trabajo simultáneo sobre el mismo contenido | Editores de documentos conjuntos (Google Docs) |
| **Coordinación** | Organizar quién hace qué y cuándo | **Calendarios compartidos** |

> Los **calendarios compartidos** son la herramienta prototípica de **coordinación** en groupware: permiten programar reuniones, gestionar disponibilidades y sincronizar agendas. Los editores de documentos conjuntos son de **colaboración**; las bases de datos compartidas son de gestión de información.

**Gestores documentales (DMS — Document Management System):** son sistemas que permiten almacenar, organizar y recuperar documentos de forma centralizada. Constan de tres elementos fundamentales:

- **Contenido**: los documentos en sí (archivos de texto, PDFs, imágenes...).
- **Metadatos**: datos descriptivos asociados a cada documento (autor, fecha, tipo, palabras clave) que permiten clasificar y recuperar la información.
- **Búsquedas**: mecanismos para localizar documentos, tanto por metadatos como por búsqueda de texto completo (full-text search).

> Pregunta de examen: los tres elementos de un gestor documental son **contenido, metadatos y búsquedas**. El versionado y la auditoría son funcionalidades adicionales, no elementos estructurales básicos.

### 2.6 Entornos de escritorio

Un **entorno de escritorio** (Desktop Environment, DE) es un conjunto de software diseñado para proporcionar una experiencia de usuario amigable en un ordenador. Es una implementación de una interfaz gráfica de usuario (GUI) que facilita el acceso y la configuración de programas y servicios.

El primer entorno de escritorio moderno se desarrolló en la **década de 1980**. Los más famosos: **Windows** (Microsoft) y **Mac** (Classic y Cocoa).

Los entornos de escritorio no brindan acceso completo a todas las funciones del SO; para ello se recurre a la **Interfaz de Línea de Comandos (CLI)**.

**Entornos de escritorio populares en Linux:**

| Entorno | Biblioteca | Distribución asociada |
|---------|-----------|----------------------|
| **KDE Plasma** | Qt | Kubuntu, openSUSE |
| **GNOME** | GTK | Ubuntu, Fedora |
| **XFCE** | GTK | Xubuntu, Manjaro |
| **LXQt** | Qt | Lubuntu |
| **Cinnamon** | GTK | Linux Mint |
| **MATE** | GTK | Ubuntu MATE |

El diagrama de la pila gráfica muestra la arquitectura de capas desde abajo: **hardware** → **núcleo (kernel)** → **servidor de pantalla (display server)** + **gestor de ventanas (window manager)** → **interfaz gráfica (GUI)** → **usuario**. Ejemplos de servidores de pantalla: X.Org Server, Weston, KWin, Mutter, Quartz Compositor, SurfaceFlinger. Protocolos: **X11**, **Wayland**, Quartz.

> KDE es un entorno gráfico de escritorio, NO un sistema operativo ni un gestor de arranque.

### 2.7 Interfaz de línea de comandos (CLI)

Una **Interfaz de Línea de Comandos (CLI)** es un método que permite a los usuarios interactuar con programas informáticos mediante instrucciones en forma de texto simple.

Conceptos diferenciados:
- **CLI**: el método en sí.
- **Shell**: el programa que implementa la CLI (bash, zsh, PowerShell).
- **Emulador de terminal**: programa que proporciona acceso a la CLI en un entorno gráfico (Konsole, GNOME Terminal).
- **Consola**: terminal física o virtual del sistema.

La evolución de las interfaces de usuario: **CLI** (Codified, Strict) → **GUI** (Metaphor, Exploratory) → **NUI** (Direct, Intuitive).

**Funcionamiento de la CLI:**
- El sistema muestra un **prompt** que indica que el usuario puede escribir órdenes.
- Las órdenes tienen la estructura: `PROMPT> aplicación [parámetros] archivos_o_URL`
- La salida normal se envía a **stdout** (salida estándar).
- Los errores se envían a **stderr** (salida de error estándar).
- Es posible utilizar **scripts** para automatizar secuencias de comandos.

**PowerShell** es una CLI que permite ejecutar scripts y facilita la configuración, administración y automatización de tareas en múltiples plataformas. Trabaja con objetos de **.NET** y se basa en el **Common Language Runtime (CLR)**. Su uso principal es la administración de sistemas Windows, pero también está disponible en Linux y macOS.

**Comandos útiles en scripts bash:**

```bash
# test condicional sobre ficheros
[ -e ruta ]   # true si el fichero/directorio EXISTE (cualquier tipo)
[ -f ruta ]   # true si existe y es un fichero regular
[ -d ruta ]   # true si existe y es un directorio
[ -x ruta ]   # true si existe y tiene permiso de ejecución
[ -L ruta ]   # true si existe y es un enlace simbólico
[ ! -e ruta ] # true si NO existe (! niega la condición)

# crear directorios
mkdir -p /ruta/completa/subdirectorio   # crea toda la jerarquía si no existe; no da error si ya existe
```

La redirección `> /dev/null 2>&1` descarta toda la salida (stdout y stderr). `/dev/null` es el "agujero negro" de Linux: todo lo que se escribe ahí desaparece.

**Shebang (`#!`):** la primera línea de un script ejecutable en Unix/Linux que indica al sistema operativo qué intérprete debe usar para ejecutarlo. Debe ser la primera línea del fichero.

```bash
#!/bin/bash        # script de Bash
#!/bin/sh          # shell POSIX genérico
#!/usr/bin/python3 # script Python 3
#!/usr/bin/env python3  # Python 3 usando PATH (más portable)
```

Cuando el kernel detecta `#!` al inicio de un fichero, lee la ruta del intérprete y lo lanza pasándole el script como argumento. No es un comentario (aunque el resto de líneas con `#` sí lo son), ni importa librerías, ni define variables de entorno.

### 2.8 BIOS y UEFI

#### BIOS

La **BIOS** (Basic Input/Output System — Sistema Básico de Entrada y Salida) es el primer programa que se ejecuta cuando se enciende el ordenador. Se almacena en un chip de memoria no volátil (tradicionalmente ROM, hoy en día EEPROM o memoria flash) en la placa base. Sus funciones son:

- Iniciar y probar el hardware del ordenador (**POST**: Power-On Self Test).
- Cargar un sistema operativo desde un dispositivo de almacenamiento.
- Proporcionar una interfaz básica para que los programas y SO se comuniquen con el hardware.

La BIOS está siendo **reemplazada** gradualmente por la **UEFI**.

#### UEFI

La **UEFI** (Unified Extensible Firmware Interface — Interfaz de Firmware Extensible Unificada) es la especificación que define la interfaz entre el firmware del equipo y el SO, reemplazando a la BIOS. Sus características principales:

- Utiliza **GPT** (GUID Partition Table) como esquema de particionado.
- Soporta discos de más de **2 TB** (MBR estaba limitado a 2 TB).
- Hasta **128 particiones** primarias por disco (MBR solo 4).
- Interfaz gráfica con soporte de ratón.
- Arranque más rápido.
- Compatible con BIOS mediante el módulo **CSM** (Compatibility Support Module).
- Incluye **Secure Boot**: modo de arranque seguro que verifica la firma digital del cargador de arranque antes de ejecutarlo, impidiendo bootkits y rootkits.

**Comparativa BIOS/MBR vs. UEFI/GPT:**

| Característica | BIOS + MBR | UEFI + GPT |
|---------------|-----------|-----------|
| Tamaño máximo de disco | 2 TB | ~9,4 ZB |
| Particiones primarias máximas | 4 | 128 |
| Interfaz | Texto 16-bit | Gráfica (ratón, red) |
| Arranque seguro | No | Sí (Secure Boot) |
| Velocidad de arranque | Normal | Más rápido |

> Pregunta frecuente: UEFI utiliza GPT. La limitación de 2 TB corresponde a MBR (BIOS), NO a UEFI. "Secure Boot" es el nombre correcto del modo de arranque seguro de UEFI (no "Safe Boot" ni "Safe UEFI").

### 2.9 Hipervisores

Un **monitor de máquina virtual** o **hipervisor** es una plataforma que permite ejecutar múltiples sistemas operativos simultáneamente en un mismo ordenador.

**Dos tipos principales:**

| Tipo | Descripción | Ejemplos |
|------|-------------|---------|
| **Tipo 1** (nativo / bare-metal) | Se ejecuta directamente sobre el hardware, sin SO anfitrión | VMware ESXi, Microsoft Hyper-V |
| **Tipo 2** (hosted) | Se ejecuta sobre un SO existente (anfitrión) | VirtualBox, VMware Workstation |

El diagrama de hipervisores muestra dos arquitecturas: el tipo 1 tiene el hipervisor directamente sobre el hardware real, con los SO huésped encima; el tipo 2 tiene un SO anfitrión sobre el hardware real, luego el software de virtualización, y encima los SO huéspedes con su hardware virtual.

### 2.10 Gestores de arranque (Bootloaders)

Un **gestor de arranque** (*bootloader*) es un programa que prepara todo lo necesario para iniciar el SO. Por lo general se utilizan **cargadores de arranque multietapas** (varios programas pequeños que se ejecutan secuencialmente).

El proceso de arranque comienza cuando la CPU ejecuta un programa contenido en una memoria de solo lectura en una dirección predefinida.

**Gestor de arranque Flash:** usado en sistemas embebidos (aplicaciones automotrices). Reside en la memoria Flash y es la primera aplicación que se ejecuta tras un reinicio. Decide si una aplicación está lista para cargarse en la ECU. Se basa en el protocolo **CAN** (Controller Area Network).

**Arranque desde red (PXE):** permite a un ordenador arrancar y cargar su SO desde una red en lugar de un disco local. Utilizado en estaciones de trabajo sin disco, enrutadores y ordenadores gestionados centralmente (bibliotecas, escuelas). El protocolo estándar es **PXE** (Preboot Execution Environment).

---

## 3. Administrador del Sistema: Funciones y Responsabilidades

### 3.1 Definición

El **administrador del sistema** (*sysadmin*) es el encargado de la instalación, soporte y mantenimiento de los servidores u otros sistemas informáticos, así como de la planificación y respuesta a interrupciones del servicio y la programación de comandos de administración.

### 3.2 Funciones

El sysadmin es, en la práctica, el responsable de que todo funcione. Sus funciones cubren desde lo más rutinario hasta lo más crítico:

- **Administración de usuarios**: instalación y mantenimiento de cuentas de usuario.
- **Verificación del funcionamiento de periféricos** y programación de reparaciones en caso de fallos de hardware.
- **Monitorización del rendimiento del sistema**.
- **Creación y gestión de sistemas de archivos**.
- **Instalación de software**.
- **Desarrollo de políticas de copias de seguridad y recuperación de datos**.
- **Supervisión de la comunicación de red**.
- **Actualización de sistemas y software** según las nuevas versiones disponibles.
- **Aplicación de políticas de seguridad para los usuarios**.

### 3.3 Responsabilidades

Las responsabilidades solapan con las funciones, pero son más específicas: describen de qué responde el administrador ante la organización. El examen las distingue, así que conviene conocer ambas listas.

- Control de sistemas y programas informáticos.
- Realización de copias de seguridad de datos.
- Aplicación de actualizaciones del SO y cambios de configuración.
- Instalación y configuración de nuevo hardware y software.
- Gestión de información de cuentas de usuario (adiciones, modificaciones y restablecimiento de contraseñas).
- Atención a consultas de naturaleza técnica.
- Responsabilidad en temas de seguridad.
- Documentación detallada de la configuración del sistema.
- Optimización del rendimiento del sistema.
- Mantenimiento de la red para asegurar su funcionamiento adecuado.

**Gestión de incidencias (ITIL):** ITIL (Information Technology Infrastructure Library) es el marco de referencia estándar para la gestión de servicios TI. En él, la gestión de incidencias sigue este orden:

1. **Identificación y registro** — primera etapa: detectar y documentar la incidencia (apertura del ticket).
2. **Clasificación y priorización** — categoría, urgencia e impacto.
3. **Diagnóstico inicial** — soporte de primer nivel.
4. **Escalado** — si no se resuelve en primer nivel.
5. **Investigación y diagnóstico** — análisis de causa raíz.
6. **Resolución y recuperación**.
7. **Cierre de la incidencia**.

> Pregunta de examen: la **primera** etapa de la gestión de incidencias es la **identificación y registro**. No se puede clasificar ni escalar una incidencia que aún no ha sido detectada y documentada.

### 3.4 La cuenta administrador (root)

La **cuenta de admin o root** tiene acceso y control total sobre el sistema. Puede eliminar archivos críticos del sistema sin posibilidad de recuperarlos (salvo copia de seguridad previa). Muchas tareas se automatizan usando **scripts de Shell**:

- Crear nuevos usuarios.
- Restauración de contraseñas.
- Bloqueo/desbloqueo de cuentas.
- Monitorización de seguridad del servidor.

**Cron — planificación de tareas en Linux:** el demonio `cron` permite ejecutar tareas automáticas en horarios definidos. El fichero de configuración es el `crontab`. Formato de cada línea:

```
# minuto  hora  día-mes  mes  día-semana  comando
  0       3     1,15     *    *           /scripts/backup.sh
```

Ejemplos de expresiones frecuentes en examen:

| Expresión | Significado |
|-----------|-------------|
| `0 3 1,15 * *` | A las 3:00 los días 1 y 15 de cada mes |
| `0,15,30,45 * * * *` | Cada 15 minutos (o equivalente: `*/15 * * * *`) |
| `*/3 * * * *` | Cada 3 minutos |
| `0 0 * * 0` | A medianoche los domingos |

La redirección `> /dev/null 2>&1` al final de un comando cron **descarta** toda salida (stdout y stderr). Sin ella, cron envía la salida por correo al usuario propietario. `/dev/null` es el dispositivo nulo de Linux: todo lo que se escribe ahí desaparece.

**SU vs. SUDO en Linux:**

| Característica | `su` | `sudo` |
|---------------|------|--------|
| Significado | Switch/Substitute User | Substitute User DO |
| Abre sesión completa | Sí | No (comando a comando) |
| Contraseña requerida | Del usuario destino | Del usuario actual |
| Configuración | No | `/etc/sudoers` |
| Registro en logs | No | Sí (syslog/journald) |
| Granularidad | Sesión completa | Comando a comando |

```bash
su -           # Cambia a root con su entorno
sudo comando   # Ejecuta un comando como root
sudo -u www-data cat /var/log/nginx/access.log  # Ejecuta como otro usuario
```

> SUDO permite ejecutar un comando simulando ser otro usuario. SU es acrónimo de "Switch User" (o Substitute User), NO de "supreme user". La sesión de SU NO se cierra automáticamente al terminar una tarea.

**Características del superusuario root en Linux:**
- Puede modificar cualquier archivo del sistema, independientemente de su propietario.
- Puede instalar, actualizar o eliminar paquetes de software.
- Puede configurar la red, interfaces y firewall.
- Puede gestionar todos los servicios del sistema.

**Estructura de directorios Linux (FHS — Filesystem Hierarchy Standard):**

| Directorio | Contenido |
|-----------|-----------|
| `/bin` | Binarios esenciales (sin subdirectorios, según FHS) |
| `/boot` | Archivos del gestor de arranque y kernel |
| `/etc` | Archivos de configuración |
| `/var/log` | Ficheros de log del sistema |
| `/tmp` | Archivos temporales |
| `/usr/bin` | Binarios de usuario no esenciales |
| `/sbin` | Binarios del sistema para root |

**Directorios en Windows de 64 bits:**

| Directorio | Contenido |
|-----------|-----------|
| `C:\Windows\System32` | DLLs y ejecutables del sistema de **64 bits** |
| `C:\Windows\SysWOW64` | DLLs del sistema de **32 bits** (emulación WOW64) |
| `%ProgramFiles%` | Aplicaciones de **64 bits** |
| `%ProgramFiles(x86)%` | Aplicaciones de **32 bits** |

**WOW6432Node:** es una **clave del registro** de Windows (no un directorio, ni una variable de entorno). Cuando una aplicación de 32 bits intenta acceder a `HKEY_LOCAL_MACHINE\SOFTWARE`, Windows la redirige transparentemente a `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node`.

**Comandos útiles en CMD de Windows (para administración):**

```cmd
# Eliminar un directorio con todo su contenido de forma recursiva
rmdir /s tmp          # Correcto (también: rd /s tmp)
rmdir /s /q tmp       # Sin confirmación (quiet mode)

# INCORRECTO — sintaxis Unix, no válida en CMD de Windows:
rmdir -r tmp          # Error: los modificadores en CMD usan / no -
del -r tmp            # Error: del elimina ficheros, no directorios; además -r no existe en CMD
```

> En PowerShell el equivalente es `Remove-Item -Recurse -Force tmp` o `rm -r tmp` (alias). En CMD de Windows los modificadores siempre van con barra (`/s`), no con guión (`-r` como en Unix).

### 3.5 Monitor de comunicación de red

Un **monitor de comunicación de red** es una herramienta que supervisa y evalúa el rendimiento de una red informática, enfocada en detectar problemas internos (fallos en servidores, problemas de infraestructura). Puede:

- Comprobar el estado de un servidor web (enviando solicitudes HTTP periódicas).
- Monitorear un servidor de correo (usando SMTP/IMAP/POP3).
- Evaluar tiempo de respuesta, disponibilidad, consistencia y fiabilidad.

> La proliferación de dispositivos de optimización de redes puede dificultar la medición precisa del tiempo de respuesta entre puntos, ya que limitan la visibilidad extremo a extremo.

**Herramientas de análisis de rendimiento Unix/Linux:**

```bash
uptime    # Tiempo encendido, usuarios conectados y carga media (1, 5, 15 min)
sar       # Estadísticas históricas del sistema (CPU, memoria, I/O, red)
free      # Uso de memoria RAM y swap
top       # Monitor de procesos en tiempo real
iostat    # Estadísticas de I/O de discos y CPU
netstat   # Conexiones de red, tablas de enrutamiento (o 'ss' en sistemas modernos)
vmstat    # Memoria virtual, procesos, CPU, I/O
```

**Ficheros de log en Unix/Linux:**

| Fichero | Contenido |
|---------|-----------|
| `/var/log/messages` | Mensajes generales del sistema (RHEL/CentOS) |
| `/var/log/syslog` | Mensajes generales del sistema (Debian/Ubuntu) |
| `/var/log/secure` | Autenticación y seguridad (RHEL/CentOS) |
| `/var/log/auth.log` | Autenticación y seguridad (Debian/Ubuntu) |
| `/var/log/kern.log` | Mensajes del kernel |
| `/var/log/dmesg` | Mensajes del arranque del kernel |
| `/var/log/cron` | Ejecuciones de tareas cron |

> `/var/log/stats` NO existe como fichero de log estándar en Unix/Linux.

### 3.6 Políticas de seguridad

Las **políticas de seguridad** son reglas o directrices configuradas por los administradores para proteger los recursos de un ordenador o una red. Garantizan la integridad, confidencialidad y disponibilidad de los datos y sistemas.

Se pueden configurar:
- **A nivel local** en un ordenador.
- **A través de la red** mediante **Directivas de Grupo (GPO)** en entornos Windows.

**Aspectos que las políticas de seguridad pueden controlar:**

- **Autenticación de usuarios**: cómo se autentican en la red o dispositivo.
- **Acceso a recursos**: a qué recursos pueden acceder y qué operaciones realizar.
- **Registro de eventos**: auditoría y registro de acciones (detectar intrusiones).
- **Pertenencia a grupos**: cómo la membresía afecta a los permisos.

**Tipos de directivas en el Editor de Directivas de Grupo Local:**

| Directiva | Descripción |
|----------|-------------|
| **De contraseñas** | Reglas de duración y complejidad de contraseñas. |
| **De bloqueo de cuentas** | Condiciones y período de bloqueo de cuentas. |
| **Kerberos** | Duración y aplicación de tickets de autenticación (cuentas de dominio). |
| **De auditoría** | Qué eventos se registran (exitosos o fallidos) en el Registro de Seguridad. |
| **Asignación de derechos de usuario** | Qué usuarios/grupos tienen derechos de inicio de sesión o privilegios. |
| **Opciones de seguridad** | Nombres de cuentas Administrador/Invitado, acceso a unidades, instalación de controladores. |

### 3.7 Gestión de dispositivos móviles (MDM)

**MDM** (Mobile Device Management) es el conjunto de tecnologías que permite gestionar de forma centralizada y remota los dispositivos móviles corporativos. Funcionalidades: inscripción, distribución de apps, políticas de seguridad, borrado remoto, inventario.

Soluciones MDM conocidas: Microsoft Intune, VMware Workspace ONE (AirWatch), Jamf, IBM MaaS360.

**Modelos de propiedad de dispositivos móviles (Android Enterprise):**

| Acrónimo | Nombre | Descripción |
|---------|--------|-------------|
| **BYOD** | Bring Your Own Device | El empleado usa su dispositivo personal |
| **COPE** | Corporate Owned, Personally Enabled | Corporativo con uso personal permitido |
| **COBO** | Corporate Owned, Business Only | Corporativo solo para trabajo |
| **COSU** | Corporate Owned, Single Use | Corporativo para una única tarea (modo quiosco), gestionado con MDM |

> COSU (modo quiosco) se gestiona con un MDM. BYOD es el modelo opuesto (dispositivo personal del empleado).

**Términos relacionados:**

| Acrónimo | Nombre | Descripción |
|---------|--------|-------------|
| MDM | Mobile Device Management | Gestión de dispositivos móviles |
| MAM | Mobile Application Management | Gestión solo de aplicaciones |
| EMM | Enterprise Mobility Management | MDM + MAM + MCM |
| UEM | Unified Endpoint Management | Gestión de todos los endpoints |

---

## 4. Control de Cambios de los Programas de una Instalación

Esta sección aborda uno de los temas más conceptuales del Tema 1. Puede parecer abstracta, pero el examen pregunta nombres exactos (GCS, ECS, CCC) y situaciones concretas (¿quién aprueba un cambio? ¿cuándo se aplica el control formal?). Conviene entender el flujo completo antes de memorizar términos.

### 4.1 La configuración del software

La **configuración del software** se refiere a toda la información generada durante el proceso de ingeniería de software. No es solo el código: incluye también los documentos y los datos. Consta de **tres categorías principales**:

1. **Programas de ordenador** (código fuente y ejecutable).
2. **Documentos que describen los programas** (técnicos y de usuario).
3. **Estructuras de datos** contenidas en el programa o externas a él.

El cambio es inevitable en el desarrollo de software: los requisitos evolucionan, se descubren errores, los clientes piden nuevas funciones. El problema es que un cambio mal gestionado puede romper lo que ya funcionaba. Por eso existe la GCS.

### 4.2 Gestión de Configuraciones del Software (GCS)

La **GCS** (Gestión de Configuraciones del Software) — en inglés **SCM** (Software Configuration Management) — es el conjunto de actividades que permiten gestionar los cambios de forma ordenada a lo largo de todo el ciclo de vida del software. Sin GCS, cada cambio es un riesgo; con GCS, cada cambio es trazable y reversible.

La GCS responde a cuatro preguntas clave:

- ¿Cómo se identifican y gestionan las múltiples versiones? → **Identificación**
- ¿Cómo se controlan los cambios? → **Control de cambios**
- ¿Quién aprueba y prioriza los cambios? → **Auditorías de configuraciones**
- ¿Cómo se garantiza la correcta implementación? → **Generación de informes**

### 4.3 Línea base y Elementos de Configuración del Software (ECS)

**Línea base (baseline):** concepto de gestión de configuraciones que ayuda a controlar los cambios sin impedir los cambios justificados. Se define como un punto del ciclo de vida del software en el cual se aplica el control de configuraciones a un elemento específico.

El diagrama de ciclo de vida muestra las fases: **Planificación** → **Requisitos** → **Diseño** → **Codificación** → **Prueba** → **Mantenimiento**. Bajo cada fase existe una línea base correspondiente: línea base de planificación, de requisitos, de diseño, de codificación, de prueba y de configuración del software.

**Elementos de Configuración del Software (ECS):** información creada como parte del proceso de ingeniería de software que forma la base de las técnicas de gestión de configuraciones. Los ECS más importantes son:

1. Especificación del sistema.
2. Plan del proyecto software.
3. Especificación de requerimientos del software.
4. Manual de usuario preliminar.
5. Especificación de diseño (preliminar y detallado).
6. Listados del código fuente.
7. Planificación y procedimiento de prueba (casos de prueba y resultados).
8. Manuales de operación y de instalación.
9. Programas ejecutables.
10. Manual de usuario.
11. Documentos de mantenimiento (informes de problemas, peticiones de mantenimiento, órdenes de cambios de ingeniería).
12. Estándares y procedimientos de ingeniería del software.

### 4.4 Identificación de la configuración

La tarea de identificación tiene como objetivos principales:

- **Definir una estructura de documentación** organizada lógicamente.
- **Proporcionar métodos para gestionar los cambios** a medida que se producen.
- **Relacionar los cambios con información relevante** (quién, qué, cuándo, por qué, cómo).

**Sistema de referencia de documentos (formato `XXX-YYY-Z-RL-NNN`):**

| Campo | Significado |
|-------|------------|
| `XXX-YYY` | Identificador de empresa (XXX) + identificador de proyecto (YYY) |
| `Z` | Tipo de elemento: P=Plan, R=Requisitos, D=Diseño, S=Fuente, T=Prueba, U=Manual usuario, I=Guía instalación, M=Manual mantenimiento |
| `RL` | Nivel de revisión |
| `NNN` | Código de atributo (ej. fecha) |

Ejemplo: `CTA-001-P-0-3/16` = plan del proyecto 1 de la empresa CTA, documento original, bajo control de cambios en marzo de 2016.

**Tres enfoques para el control de la documentación:**

1. Mantener todos los documentos en una **biblioteca de ingeniería** con estructura previamente definida.
2. Establecer una **biblioteca de software independiente** con herramientas de procesamiento de texto.
3. Establecer un **sistema de referencia** con número único para cada documento.

### 4.5 El proceso de control de cambios (GCS)

El **control de cambios** es un proceso que permite la evaluación y aprobación de las modificaciones en los ECS a lo largo del ciclo de vida. Existen **tres tipos**:

| Tipo | Descripción | Cuándo se aplica |
|------|-------------|-----------------|
| **Control Individual** (Informal) | El técnico realiza cambios según sea necesario; registro informal. | Etapas iniciales del desarrollo (cambios frecuentes). |
| **Control de Gestión** | Procedimiento formal de revisión y aprobación para cada cambio propuesto. | Después de que el ECS ha sido aprobado (menos cambios). |
| **Control de Cambios Formal** | Evaluación del impacto por el CCC; se genera una Orden de Cambio (OC). | Fase de mantenimiento (producto ya implementado). |

**Flujo del proceso de control (diagrama GCS):**

El diagrama muestra el flujo completo: **Necesidad de cambio** → Generación de **petición de cambio** → **Evaluación** (aspectos técnicos, efectos laterales, impacto general, costes) → Generación formal de petición → **Decisión del ACC** (Autoridad de Control de Cambios). Si se aprueba: **Orden de Cambio de Ingeniería** → cola de cambios → Otras tareas GCS. Si se rechaza: informar al cliente.

### 4.6 Comité de Control de Cambios (CCC)

El **Comité de Control de Cambios (CCC)** — también denominado **ACC** (Autoridad de Control de Cambios) en algunos textos — es la entidad de gobierno que gestiona todos los asuntos relacionados con la GCS. Ambos acrónimos se refieren a la misma figura; en el examen pueden aparecer indistintamente. Sus responsabilidades:

- Evaluar el impacto de cambios en el sistema.
- Categorizar y priorizar las solicitudes de cambio.
- Resolver conflictos entre disciplinas y organizaciones.
- Asegurar el cumplimiento de prácticas de registro y contabilidad.

Cuando se presenta una **solicitud de cambio**: se analiza por el equipo de desarrollo → se presenta un informe al CCC → el CCC **aprueba o rechaza** → si aprueba, se genera una **Orden de Cambio (OC)** con: descripción del cambio, restricciones aplicables y criterios para revisiones y auditorías.

### 4.7 Auditorías e informes de estado (GCS)

**Auditorías de configuración:** verifican que:

- Se ha realizado el cambio especificado en la OCI (Orden de Cambio de Ingeniería).
- Se ha llevado a cabo una revisión técnica formal.
- Se han seguido los estándares de ingeniería del software.
- Los cambios en los ECS se han registrado apropiadamente (fecha, autor).
- Se han seguido los procedimientos del GCS para señalar, registrar y comunicar el cambio.
- Se han actualizado todos los ECS relacionados con el cambio.

**Generación de Informes de Estado de la Configuración (GIEC):** es el proceso que responde a las preguntas: ¿Qué ocurrió? ¿Quién lo llevó a cabo? ¿Cuándo sucedió? ¿Qué otras partes se vieron afectadas? El producto de este proceso es el **Informe de Estado de la Configuración (IEC)**.

El diagrama de la GIEC muestra: **Identificación de configuración** y **Control de configuración** generan cambios hacia el nodo central de **Generación de informes de estado**; la **Auditoría de configuración** aporta deficiencias. El resultado es el **Informe IEC** y la **Base de datos de IEC**.

> Nota: GIEC = el proceso (Generación de Informes de Estado de la Configuración); IEC = el producto (Informe de Estado de la Configuración).

### 4.8 Sistemas de control de versiones

Los sistemas de control de versiones (VCS) son herramientas de GCS. Deben distinguirse de herramientas de inventario como OCS Inventory NG (que registra el software instalado en equipos, no versiones de código).

**Sistemas de control de versiones reconocidos:**

| Sistema | Tipo | Año |
|---------|------|-----|
| CVS | Centralizado | 1986 |
| **Subversion (SVN)** | Centralizado | 2000 |
| **Git** | Distribuido | 2005 |
| **Mercurial (hg)** | Distribuido | 2005 |
| **BitKeeper** | Distribuido | 1998 |
| **Fossil SCM** | Distribuido | ~2006 |

```bash
git init         # Crear un repositorio Git vacío
git clone URL    # Clonar un repositorio existente
```

> VND NO es un sistema de control de versiones. OCS Soft (OCS Inventory NG) es una herramienta de inventario de activos TI, no un VCS.

---

## 5. Actualización, Mantenimiento y Reparación del Sistema Operativo

### 5.1 Por qué son necesarias las actualizaciones

Un SO no es estático: el hardware evoluciona, se descubren vulnerabilidades y los usuarios demandan nuevas funciones. Un sistema sin actualizar es un sistema vulnerable. Las razones para actualizar son tres:

1. **Actualizaciones de hardware**: la evolución del hardware requiere nuevos controladores y programas.
2. **Actualizaciones de programas**: se descubren vulnerabilidades o fallos que se corrigen en versiones posteriores.
3. **Nuevas funcionalidades**: los SO incorporan nuevas características aprovechables mediante actualizaciones.

Mantener el SO y las aplicaciones actualizadas es esencial para la **seguridad contra intrusiones y malware**, ya que los atacantes explotan activamente los fallos conocidos en versiones antiguas.

### 5.2 Actualización en Windows

**Microsoft** lanza actualizaciones de seguridad periódicas **el segundo martes de cada mes** (conocido como "Patch Tuesday"). Si existe una vulnerabilidad importante, se publica una actualización extraordinaria.

**Windows 10 — Ruta de actualización:**
```
Inicio → Configuración → Actualización y seguridad → Windows Update
```

**Windows 10 — Comprobación del estado de activación:**
```
Inicio → Configuración → Actualización y seguridad → Activación
```

**Ediciones oficiales de Windows 10:**

| Edición | Público |
|---------|--------|
| Home | Usuarios domésticos |
| Pro | Profesionales y PYMEs |
| Enterprise | Grandes empresas (licencia por volumen) |
| Education | Instituciones educativas |
| Pro Education | Centros educativos con funciones Pro |
| S (S Mode) | Dispositivos educativos de bajo coste (solo apps de la Store) |
| Pro for Workstations | Estaciones de trabajo de alto rendimiento |
| IoT | Dispositivos de Internet de las Cosas |

> "Windows 10 Enterprise plus" NO existe como edición oficial.

**Windows 11 — Novedades destacadas:**
- Lanzamiento oficial: **5 de octubre de 2021**.
- Requiere mínimo **4 GB de RAM** (Windows 10 requería 2 GB).
- Solo disponible en versión de **64 bits** (los procesadores de 32 bits no son compatibles).
- Requiere chip de seguridad **TPM 2.0**, responsable de almacenar las claves de cifrado y proteger la privacidad de archivos sensibles.

**Sysprep — Despliegue de imágenes de Windows:** cuando se captura una imagen de Windows de un equipo y se quiere desplegar en múltiples equipos, es obligatorio **generalizar** la imagen primero con la herramienta **Sysprep** (`C:\Windows\System32\Sysprep\sysprep.exe`). La generalización elimina:
- El **SID** (Security Identifier) único del equipo de origen — cada equipo de destino generará el suyo propio.
- Los **drivers** específicos del hardware del equipo de origen.
- Otros identificadores únicos del sistema.

Sin generalizar, todos los equipos de destino compartirían el mismo SID, causando conflictos en el dominio Active Directory. El TPM y el Secure Boot no son requisitos previos para instalar una imagen.

**Registro de Windows (Regedit) — claves raíz:**

| Clave | Contenido |
|-------|-----------|
| `HKEY_CLASSES_ROOT` (HKCR) | Asociaciones de extensiones de archivo con aplicaciones. Para cambiar qué programa abre un tipo de fichero, se modifica aquí |
| `HKEY_CURRENT_USER` (HKCU) | Configuración y preferencias del usuario actual |
| `HKEY_LOCAL_MACHINE` (HKLM) | Configuración del sistema (software, hardware) |
| `HKEY_CURRENT_CONFIG` (HKCC) | Perfil de hardware actual |
| `HKEY_USERS` (HKU) | Datos de todos los perfiles de usuario |

> Para cambiar la aplicación que abre un tipo de fichero (p. ej. `.ext`), modificar la subclave correspondiente en **HKEY_CLASSES_ROOT**.

### 5.3 Actualización en Linux

**Actualización de distribuciones basadas en Debian/Ubuntu:**

```bash
# Actualizar lista de paquetes disponibles
sudo apt-get update

# Actualizar paquetes instalados
sudo apt-get upgrade

# Instalar un paquete nuevo
sudo apt-get install nombre-paquete
# o en versiones modernas:
sudo apt install nombre-paquete

# Eliminar un paquete
sudo apt-get remove nombre-paquete

# Eliminar un paquete incluyendo sus ficheros de configuración
sudo apt-get purge nombre-paquete

# Actualizar a la última versión estable de la distribución
sudo update-manager --devel-release
```

También se puede usar `ALT+F2` para abrir el cuadro de ejecución e introducir `update-manager --devel-release`.

**Gestores de paquetes por familia de distribución Linux:**

| Familia | Gestor | Formato |
|---------|--------|---------|
| Debian/Ubuntu | `apt`, `apt-get`, `dpkg` | `.deb` |
| Red Hat/Fedora/CentOS | `dnf`, `yum`, `rpm` | `.rpm` |
| Arch Linux | `pacman` | `.pkg.tar.zst` |
| openSUSE | `zypper`, `rpm` | `.rpm` |

**Comprobación de la versión del kernel:**

```bash
uname -r       # Muestra solo la versión del kernel
uname -a       # Información completa del sistema
cat /etc/os-release   # Versión del SO en distros modernas
```

**Verificación de paquetes instalados:**

```bash
dpkg -l nombre-paquete    # Verificar si un paquete está instalado (Debian/Ubuntu)
rpm -q nombre-paquete     # En sistemas basados en RPM
```

**Wine — Ejecutar aplicaciones de Windows en Linux:**

```bash
wine notepad.exe          # Ejecutar un ejecutable de Windows con Wine
winecfg                   # Configuración de Wine
wine --version            # Ver versión de Wine
```

> Wine (Wine Is Not an Emulator) es una capa de compatibilidad que traduce las llamadas de la API de Windows a equivalentes de Linux. La sintaxis correcta es `wine programa.exe` (sin flags `-cf` ni `-x`).

**Directorio `/bin` según FHS:** no debe tener subdirectorios. Contiene binarios esenciales disponibles para todos los usuarios durante el arranque. No contiene archivos del gestor de arranque (eso es `/boot`) ni archivos temporales (eso es `/tmp`).

**Sistema de impresión en Linux — CUPS:**

**CUPS** (Common Unix Printing System) es el sistema de impresión estándar en Ubuntu y en la mayoría de distribuciones Linux modernas. Fue creado originalmente por Apple. Usa el protocolo **IPP** (Internet Printing Protocol) y también soporta TCP/IP directo (puerto 9100, AppSocket/JetDirect). La interfaz web de administración está en `http://localhost:631`.

Comandos principales de CUPS:

| Comando | Función |
|---------|---------|
| `lpadmin` | **Añadir/gestionar impresoras** (administración). Ejemplo: `lpadmin -p nombre -E -v socket://IP:9100 -m everywhere` |
| `lpstat` | **Ver estado** de impresoras y trabajos en cola. `lpstat -p` lista impresoras; `lpstat -o` muestra trabajos pendientes |
| `lp -d nombre fichero` | Enviar un fichero a imprimir |
| `lpq` | Ver cola de impresión de una impresora concreta |
| `lprm` | Cancelar un trabajo de impresión |

> LPD (Line Printer Daemon) es el sistema de impresión clásico Unix (obsoleto). LPRng es una versión mejorada de LPD. Ninguno de los dos es el sistema por defecto en Ubuntu moderno: ese es **CUPS**.

### 5.4 Actualización en Android

La actualización del sistema Android puede ser más complicada que en Windows debido a:
- La **coexistencia de varias versiones** del SO en el mercado.
- La necesidad de que los **fabricantes de dispositivos "validen"** las actualizaciones.
- En algunos casos, la **operadora de telefonía** también debe autorizarlas.

La **descarga** de actualizaciones se automatiza, pero la **instalación debe iniciarse manualmente** cuando se recibe la notificación. Ruta en Android: **Ajustes → Acerca del dispositivo → Actualizaciones software**.

**Frameworks de desarrollo de apps móviles:**

| Tipo | Descripción | Ejemplos |
|------|-------------|---------|
| **Nativo** | Código específico para cada plataforma | Swift/SwiftUI (iOS), Kotlin/Java (Android) |
| **Híbrido/multiplataforma** | Un código base genera apps para varias plataformas | **NativeScript**, React Native, Flutter, Ionic, Xamarin |
| **Web móvil** | Apps web adaptadas para móvil | Progressive Web Apps (PWA) |

> **NativeScript** es un framework de desarrollo **híbrido** (multiplataforma) que usa JavaScript/TypeScript para generar apps nativas para iOS y Android. No debe confundirse con **SwiftUI** (nativo solo para Apple) ni con **jQuery** (biblioteca de manipulación del DOM web, sin relación con apps móviles).

### 5.5 Actualización en iOS

Similar al proceso de Android para dispositivos móviles. Ruta: **Ajustes → General → Actualización de software → Descargar e instalar**.

### 5.6 Mantenimiento del SO

Los SO pueden experimentar **desgaste con el tiempo** debido a su uso y factores externos. El mantenimiento básico del sistema implica:

1. **Liberar espacio en disco** (datos residuales no útiles).
2. **Desfragmentar el disco**.
3. **Corregir problemas de registro**.

**Datos residuales no útiles:** archivos temporales de instalación, archivos temporales de internet, registros de errores del sistema, registros antiguos de programas. En Windows, se usa la herramienta **"Liberador de espacio"**.

**Fragmentación del disco:** en sistemas modernos, los datos no se almacenan de manera secuencial en el disco, lo que puede causar ralentizaciones. Windows ofrece el **"Desfragmentador de disco"** (en SSD, en realidad realiza un "trim" en lugar de una desfragmentación tradicional).

**Errores en el registro de Windows:** algunos programas y acciones generan entradas incorrectas en el registro. Windows proporciona **Regedit** para administrarlo, aunque existen programas de terceros para simplificar esta tarea.

**Mantenimiento ante virus y malware:** antivirus actualizado + conductas correctas en internet. Programas de terceros: **CCleaner** (archivos residuales, desinstalador), **TuneUp** (mantenimiento general, liberador de espacio, desfragmentador).

### 5.7 Reparación del SO Windows

La reparación de SO Windows es compleja (gestiona infinidad de archivos). Existen métodos sencillos que pueden ayudar a recuperar el SO evitando una reinstalación completa.

> Nota: estos métodos son válidos solo cuando el equipo pasa el POST correctamente (no hay conflictos de hardware).

#### 5.7.1 Opciones de inicio avanzadas de Windows

En **Windows 7** (y versiones anteriores) se accede pulsando **F8** durante el arranque, antes de que aparezca el logo de Windows. En **Windows 10/11** este menú ya no es accesible con F8 (el arranque es demasiado rápido); en su lugar se accede desde `Configuración → Sistema → Recuperación → Inicio avanzado` o manteniendo pulsada la tecla Mayús al hacer clic en "Reiniciar".

**Principales opciones de inicio:**

- **La última configuración válida conocida**: inicia Windows con la configuración del último arranque correcto.
- **Modo seguro**: carga un conjunto mínimo de drivers y servicios.
- **Modo seguro con funciones de red**: modo seguro con soporte de red.
- **Modo seguro con símbolo del sistema**: modo seguro con CLI.
- **Habilitar el registro de arranque**.
- **Habilitar vídeo de baja resolución** (640×480).
- **Iniciar Windows normalmente**.

#### 5.7.2 Opciones de recuperación del sistema

Cuando las opciones de inicio básicas no resuelven el problema, se accede a las **Opciones de recuperación del sistema** desde:

1. El disco de instalación de Windows.
2. Un disco de reparación creado previamente (con `SDCLT` → "Crear un disco de reparación del sistema").

El menú de opciones de recuperación ofrece:

| Opción | Función |
|--------|---------|
| **Reparación de inicio** | Soluciona automáticamente los problemas que impiden que Windows se inicie. |
| **Restaurar sistema** | Restaura Windows a un estado anterior mediante puntos de restauración. |
| **Recuperación de imagen del sistema** | Restaura una copia idéntica de toda la partición del SO. |
| **Diagnóstico de memoria de Windows** | Comprueba errores de hardware de memoria. |
| **Símbolo del sistema** | Abre una ventana de CLI para reparaciones manuales. |

#### 5.7.3 SFC (System File Checker)

La herramienta **SFC** (Comprobador de Archivos del Sistema) analiza todos los archivos del sistema y reemplaza los defectuosos por sus copias originales, recuperándolas de `DLLCACHE` o del disco de instalación de Windows.

Es especialmente útil cuando el sistema arranca pero se producen mensajes de error en aplicaciones o el sistema se vuelve inestable.

```
# Abrir CMD como Administrador (Windows + R → CMD → Administrador)
SFC /SCANNOW
```

---

## Resumen para el examen

1. **El SO es el software principal** que administra los recursos de hardware y proporciona servicios a los programas de aplicación. Funciona en modo privilegiado.

2. **Componentes del SO:** gestión de procesos, memoria, almacenamiento secundario, E/S, archivos, protección, comunicaciones y programas de sistema.

3. **Proceso zombie:** ha finalizado y liberado recursos, pero su entrada permanece en la tabla de procesos porque el padre no ha llamado a `wait()`.

4. **BIOS vs. UEFI:** UEFI usa GPT, soporta discos > 2 TB y hasta 128 particiones. "Secure Boot" es el arranque seguro de UEFI. La limitación de 2 TB es de MBR (BIOS).

5. **Sistemas de archivos:** NTFS es nativo de Windows, NO de Linux. ext2, ext3, ext4 y ReiserFS son nativos de Linux. SDRAM es DRAM sincronizada con el reloj.

6. **Directorios Windows 64 bits:** System32 contiene binarios de 64 bits; SysWOW64 contiene binarios de 32 bits. WOW6432Node es una clave del registro (no directorio ni variable de entorno).

7. **SU vs. SUDO:** SUDO ejecuta un comando puntual como otro usuario; SU abre una sesión completa. SU = "Switch User" (no "supreme user"). SUDO requiere la contraseña del usuario actual.

8. **GCS/SCM:** gestión de configuraciones del software. Fases: Identificación → Control de cambios → Auditorías → Generación de informes. El CCC (Comité de Control de Cambios) aprueba o rechaza las solicitudes.

9. **Tipos de control de cambios:** Individual (informal, etapas iniciales), De Gestión (después de la aprobación del ECS), Formal (fase de mantenimiento, con CCC).

10. **ECS (Elementos de Configuración del Software):** información creada durante la ingeniería del software (código fuente, ejecutables, documentos, datos de prueba, manuales...).

11. **Actualizaciones Windows:** Microsoft publica parches de seguridad el **segundo martes de cada mes** (Patch Tuesday). Windows 11 requiere TPM 2.0 y 4 GB de RAM mínimo.

12. **Actualizaciones Linux (Debian/Ubuntu):** `apt-get update` actualiza la lista de paquetes; `apt-get upgrade` actualiza los paquetes; `apt-get install paquete` instala software. El gestor de distribución completa es `update-manager --devel-release`.

13. **Reparación Windows:** modo seguro (F8 en Windows 7; Mayús+Reiniciar o desde Configuración en Windows 10/11), SFC /SCANNOW (reparación de archivos del sistema), restaurar sistema (puntos de restauración), recuperación de imagen.

14. **MDM y COSU:** los dispositivos corporativos de uso único (modo quiosco) son COSU y se gestionan con MDM. BYOD es el modelo opuesto (dispositivo personal).

15. **Herramientas de análisis en Linux:** `uptime`, `sar`, `free`, `top`, `iostat`, `netstat` son herramientas reales de diagnóstico de rendimiento. `apk` es un gestor de paquetes (Alpine), no una herramienta de rendimiento. `diskstat` no es un comando estándar.

16. **FHS — Directorio `/bin`:** no debe tener subdirectorios. Los archivos del gestor de arranque están en `/boot`; los temporales en `/tmp`.

17. **KDE:** es un entorno gráfico de escritorio (no un SO ni un gestor de arranque). Usa las bibliotecas Qt.

18. **Wine:** ejecuta aplicaciones de Windows en Linux. Sintaxis: `wine programa.exe`.

19. **Control de versiones:** Git, SVN, Mercurial, BitKeeper y Fossil son VCS. OCS Inventory NG es un inventario de activos (NO un VCS). VND no existe como VCS.

20. **Fragmentación:** los SSD no se deben desfragmentar de la misma forma que los HDD (se usa "trim"). La fragmentación afecta principalmente a los discos magnéticos (HDD).

---

## Preguntas frecuentes en exámenes

### Subtemas con mayor presencia en el banco de preguntas (62 preguntas)

#### A. UEFI, BIOS y arranque (5 preguntas)

Las preguntas sobre UEFI son frecuentes y miden si el estudiante conoce los términos exactos:

- **UEFI** reemplaza a la BIOS y utiliza **GPT** (no MBR).
- El modo de arranque seguro de UEFI se llama **Secure Boot** (no "Safe Boot" ni "Safe UEFI").
- UEFI **sí tiene compatibilidad con BIOS** mediante el CSM.
- La limitación de 2 TB corresponde a **MBR**, no a UEFI/GPT.

#### B. Comandos Linux y gestión de paquetes (10+ preguntas)

Este bloque concentra el mayor número de preguntas prácticas:

```bash
# Instalar un paquete en Ubuntu/Debian
apt-get install pdfsam

# Comandos de diagnóstico de rendimiento (todos válidos)
uptime, sar, free, top, iostat, netstat

# Ficheros de log NO estándar: /var/log/stats (no existe)
# Ficheros de log reales: /var/log/messages, /var/log/secure

# Borrar directorio en Windows (recursivo)
rmdir /s tmp          # Correcto en CMD
rmdir -r tmp          # INCORRECTO (sintaxis Unix, no Windows)

# Ejecutar aplicación de Windows en Linux
wine notepad.exe      # Correcto
```

#### C. Estructura de directorios Windows 64 bits (3 preguntas)

Confusión habitual entre System32 (64 bits) y SysWOW64 (32 bits, emulación):

- `C:\Windows\System32` + `%ProgramFiles%` → aplicaciones de **64 bits**.
- `C:\Windows\SysWOW64` + `%ProgramFiles(x86)%` → aplicaciones de **32 bits**.
- **WOW6432Node** → clave del **registro** (no directorio, no variable de entorno).

#### D. Superusuario y permisos en Linux (4 preguntas)

- Root puede cambiar la configuración de red (la afirmación "no puede" es **falsa**).
- `su` = "Switch User" (no "supreme user").
- `sudo` permite ejecutar un comando **como otro usuario**.
- La sesión de `su` NO se cierra automáticamente.

#### E. Control de versiones y GCS (5 preguntas)

- **VND** no es un VCS.
- **OCS Soft** (OCS Inventory NG) no es un VCS; es un inventario de activos TI.
- Elementos válidos: Subversion, Mercurial, BitKeeper, Fossil SCM, Git.
- **MDM** (Mobile Device Management) para gestión centralizada de dispositivos móviles.

#### F. Sistemas de archivos y memoria (4 preguntas)

- **NTFS** NO es nativo de Linux.
- **ext2**, **ReiserFS**, **XFS**, **Btrfs** sí son nativos de Linux.
- **SDRAM** es un tipo de DRAM sincronizada con el reloj; correcta.
- **SRAM** usa flip-flops y no necesita refresco; se usa en cachés de CPU.

#### G. Procesos en Unix (2 preguntas)

- Proceso **zombie**: ha finalizado, liberado sus recursos, pero permanece en la tabla de procesos.
- Un **proceso** es una unidad de ejecución; una **sesión** es una colección de procesos relacionados con una terminal de control común.

#### H. Windows — Versiones y activación (2 preguntas)

- "Windows 10 Enterprise plus" **no existe**.
- Activación en Windows 10: `Inicio → Configuración → Actualización y seguridad → Activación`.
- Windows 11: lanzado el 5/10/2021, requiere TPM 2.0 y 4 GB de RAM mínimo.

---

## Referencias

- [UEFI Forum — Especificación UEFI](https://uefi.org/specifications) — Documentación oficial de UEFI y Secure Boot.
- [Microsoft Docs — Diferencias GPT y MBR](https://learn.microsoft.com/es-es/windows-server/storage/disk-management/initialize-new-disks) — Guía oficial sobre esquemas de particionado.
- [Microsoft Docs — SFC (System File Checker)](https://support.microsoft.com/es-es/topic/uso-del-comprobador-de-archivos-de-sistema-para-reparar-archivos-del-sistema-que-faltan-o-daados-79aa86cb-ca52-166a-92a3-966e85d4094e) — Instrucciones oficiales de uso del SFC /SCANNOW.
- [Filesystem Hierarchy Standard (FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) — Estándar de jerarquía de directorios de Linux.
- [Ubuntu Documentation — apt](https://help.ubuntu.com/community/AptGet/Howto) — Guía oficial de gestión de paquetes APT.
- [Linux man pages — su(1) y sudo(8)](https://man7.org/linux/man-pages/) — Documentación de los comandos su y sudo.
- [Android Enterprise — COSU](https://developers.google.com/android/management/cosu) — Documentación oficial sobre dispositivos de uso único.
- [Microsoft Intune (MDM)](https://learn.microsoft.com/es-es/mem/intune/) — Solución MDM de Microsoft.
- [Git SCM](https://git-scm.com/) — Documentación oficial de Git.
- [WineHQ](https://www.winehq.org/) — Documentación oficial de Wine (capa de compatibilidad de Windows para Linux).
- [Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html) — Referencia de herramientas de análisis de rendimiento en Linux.
- [Microsoft Docs — WOW64 y registro](https://docs.microsoft.com/es-es/windows/win32/winprog64/registry-redirector) — Redirección del registro para aplicaciones de 32 bits en Windows de 64 bits.
- Silberschatz, Galvin & Gagne — *Operating System Concepts* (10.ª ed.) — Referencia académica estándar para sistemas operativos.
- Pressman, R. S. — *Ingeniería del Software: Un Enfoque Práctico* — Referencia para GCS, ECS y CCC.
