# Tema 6: Comunicaciones, Medios de Transmisión, Redes de Conmutación y Difusión

> Este tema cubre los fundamentos de las comunicaciones digitales y analógicas: el modelo de sistema de comunicación, los parámetros de señal (frecuencia, ancho de banda, teoremas de Nyquist y Shannon), la modulación, la multiplexación y los principales medios de transmisión (par trenzado, coaxial, fibra óptica). También abarca los equipos terminales y de interconexión, las redes públicas (RTB, RDSI, xDSL), las técnicas de conmutación (circuitos, mensajes, paquetes), las redes de difusión y las comunicaciones inalámbricas (WiFi y telefonía móvil hasta 5G). En la oposición del Bloque IV este tema genera preguntas muy concretas sobre velocidades, distancias, estándares y categorías de cable; hay que dominar las tablas de datos y no confundir los tipos de par trenzado, los estándares WiFi o los niveles de conmutación.

---

## 1. Comunicaciones

### 1.1 Modelo de sistema de comunicación

Un **sistema de comunicación** transfiere información desde un punto emisor hasta un punto receptor a través de un canal. Parece simple, pero entender sus componentes es clave para todo lo que viene después en el tema.

Sus elementos básicos son:

- **Emisor**: genera la información a transmitir.
- **Canal**: medio físico por el que viaja la señal (cable, aire, fibra...).
- **Receptor**: recibe y decodifica la información.
- **Señal**: representación física de la información (voltaje, luz, ondas de radio...).
- **Ruido**: perturbaciones no deseadas que alteran la señal. El enemigo número uno de las comunicaciones.

**Herramientas de medición** (salen en examen para identificarlas):
- **Osciloscopio**: muestra la señal en el dominio del tiempo (ves la forma de onda).
- **Analizador lógico**: captura y decodifica señales digitales.
- **Analizador de espectros**: muestra la señal en el dominio de la frecuencia (ves qué frecuencias se están usando).

### 1.2 Organismos de normalización

| Ámbito | Organismo | Descripción |
|--------|-----------|-------------|
| Internacional | **IEC** | Electrotecnia |
| Internacional | **ISO** | Estándares generales |
| Internacional | **IETF** | Protocolos de Internet (RFC) |
| Internacional | **ITU** | Telecomunicaciones (UIT en español) |
| Internacional | **CCITT** | Rama ITU para telefonía y telegrafía |
| Internacional | **CCIR** | Radiocomunicaciones (absorbido por ITU-R) |
| Europeo | **ECMA** | Estándares de información y comunicación |
| Europeo | **CEN** | Normalización europea |
| Europeo | **CENELEC** | Electrotecnia europea |
| Europeo | **ETSI** | Telecomunicaciones europeas |
| Nacional (ES) | **AENOR** | Normalización española |
| Nacional (USA) | **ANSI** | Estándares americanos |
| Nacional (UK) | **BSI** | Estándares británicos |
| Asociación | **EIA** | Electrónica e informática |
| Asociación | **IEEE** | Ingenieros eléctricos y electrónicos |

### 1.3 Tipos de señales

La distinción analógica/digital es fundamental: toda la evolución de las telecomunicaciones va de convertir lo analógico en digital para poder procesarlo y transmitirlo con mucha más fiabilidad.

- **Analógica**: varía de forma continua en el tiempo (voz, audio, vídeo analógico). El problema es que el ruido se acumula.
- **Digital**: toma valores discretos (0 y 1). Mucho más resistente al ruido: si la señal llega degradada pero aún reconocible como 0 o 1, la información se recupera perfecta.
- **Periódica**: se repite con un período T (ondas de radio, portadoras).
- **No periódica**: no tiene patrón repetitivo (tráfico de datos real).

### 1.4 Parámetros de comunicación

| Parámetro | Descripción | Fórmula |
|-----------|-------------|---------|
| **Frecuencia (f)** | Ciclos por segundo | Hz |
| **Período (T)** | Duración de un ciclo | T = 1/f |
| **Longitud de onda (λ)** | Distancia que recorre la señal en un ciclo | λ = c/f (c = 3×10⁸ m/s) |
| **Amplitud** | Valor máximo de la señal | — |
| **Velocidad de transmisión** | Bits por segundo (bps) | — |
| **Ancho de banda** | Rango de frecuencias utilizables del canal | Hz |

Los dos teoremas que marcan los límites físicos de cualquier comunicación (y que salen mucho en examen):

**Teorema de Nyquist** (canal ideal, sin ruido):
```
Capacidad máxima = 2 × B × log₂(M)
```
donde B es el ancho de banda en Hz y M el número de niveles de señal. Aumentar M tiene un límite: si hay ruido, no puedes distinguir infinitos niveles.

**Teorema de Shannon** (canal real, con ruido):
```
Capacidad máxima = B × log₂(1 + S/N)
```
donde S/N es la relación señal/ruido. Este es el límite teórico absoluto: da igual cuánta ingeniería apliques, no puedes superarlo.

**Relación señal/ruido en dB:**
```
S/N (dB) = 10 × log₁₀(S/N)
```

### 1.5 Conversión analógico-digital (A/D) y digital-analógico (D/A)

Convertir señales analógicas (como la voz) a digital es lo que permite el mundo de las telecomunicaciones moderno. El proceso A/D tiene tres etapas:

1. **Muestreo**: se toman "fotografías" de la señal analógica en instantes de tiempo regulares. Según el **teorema de Nyquist**, la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima de la señal (si no, se pierde información y aparece el aliasing):
   ```
   fs ≥ 2 × fmax
   ```
2. **Cuantización**: a cada muestra se le asigna el valor discreto más cercano de los disponibles. Aquí es donde se introduce el único error inevitable: el error de cuantización.
3. **Codificación**: cada valor cuantizado se representa en binario. Con n bits, se tienen 2ⁿ niveles posibles.

**Relación señal/ruido en cuantización:**
```
SNR = 6,02 × n – 2,27 dB
```
donde n es el número de bits por muestra. A más bits, mejor calidad.

**Ejemplo clásico de examen**: voz telefónica → muestreo a 8 kHz (la voz va de 0 a 4 kHz, así que 2×4 kHz = 8 kHz), 8 bits/muestra → 8.000 muestras/s × 8 bits = **64 kbps** (esto es un canal B de RDSI).

### 1.6 Codificación de la información

Antes de transmitir, la información se codifica. Hay varios niveles: primero se representa como números (sistemas de numeración), luego se protege contra errores, y finalmente se comprime para reducir el volumen.

#### Sistemas de numeración y códigos

| Código | Base | Dígitos |
|--------|------|---------|
| **Binario** | 2 | 0, 1 |
| **BCD** (Binary Coded Decimal) | — | cada dígito decimal en 4 bits |
| **Octal** | 8 | 0-7 |
| **Hexadecimal** | 16 | 0-9, A-F |
| **ASCII** | — | 7 bits, 128 caracteres |
| **Unicode** | — | hasta 4 bytes, universal |

#### Códigos detectores y correctores de errores

- **Paridad simple**: añade 1 bit; detecta errores impares.
- **CRC** (Cyclic Redundancy Check): detección de ráfagas de errores.
- **Checksum**: suma de verificación.
- **Distancia de Hamming**: mínima diferencia entre palabras de código válidas. Para detectar d errores se necesita distancia d+1; para corregir d errores, distancia 2d+1.

#### Compresión

| Tipo | Sin pérdidas | Con pérdidas |
|------|-------------|--------------|
| **Formatos** | ZIP, RAR, PNG, FLAC | JPEG, MP3, MPEG |
| **Recuperación** | Datos originales intactos | Pérdida de calidad aceptable |

#### Codificación digital de línea

| Código | Descripción |
|--------|-------------|
| **NRZ-L** (Non-Return to Zero Level) | El nivel de tensión determina el bit: 1 = nivel alto, 0 = nivel bajo |
| **NRZ-M** | Cambio de nivel en cada '1'; sin cambio en cada '0' |
| **NRZ-S** | Cambio de nivel en cada '0'; sin cambio en cada '1' |
| **RZ** (Return to Zero) | La señal vuelve a cero a mitad de cada bit; fácil sincronización pero ocupa más ancho de banda |
| **Manchester** | Transición en cada bit: 0→alto-bajo, 1→bajo-alto. Usado en Ethernet 10BASE-T |
| **Bifase-M** | Transición al inicio de cada bit; transición adicional en cada '1' |
| **AMI** (Alternate Mark Inversion) | Los '1' alternan entre +V y -V; los '0' son nivel cero. Permite detectar errores |
| **B8ZS** | Variante AMI para T1 (EE.UU.): sustituye 8 ceros consecutivos por una secuencia especial |
| **HDB3** | Variante AMI para E1 (Europa): sustituye 4 ceros consecutivos por una secuencia especial |

---

## 2. Modos de Transmisión y Comunicación

### 2.1 Modulación

La **modulación** adapta la señal de información a las características del canal de transmisión. Sin modulación, la señal digital o de baja frecuencia no podría viajar por radio o por ciertos cables. La portadora (una señal senoidal de alta frecuencia) actúa como "vehículo" que transporta la información.

#### Modulación analógica

| Tipo | Parámetro modulado | Ejemplo |
|------|--------------------|---------|
| **AM** (Amplitud Modulada) | Amplitud de la portadora | Radio AM |
| **FM** (Frecuencia Modulada) | Frecuencia de la portadora | Radio FM (87,5-108 MHz) |
| **PM** (Fase Modulada) | Fase de la portadora | — |

#### Modulación digital

| Tipo | Descripción | Uso |
|------|-------------|-----|
| **ASK** (Amplitude Shift Keying) | Amplitud varía según el bit | Baja eficiencia |
| **FSK** (Frequency Shift Keying) | Frecuencia varía según el bit | Módems, Bluetooth |
| **PSK** (Phase Shift Keying) | Fase varía según el bit | WiFi, 3G |
| **QAM** (Quadrature Amplitude Modulation) | Combina amplitud y fase | ADSL, WiFi (64-QAM, 256-QAM, 1024-QAM) |

**Relación entre baudios y tasa binaria:**
```
R (bps) = Baudios × log₂(M)
```
donde M es el número de estados de la señal.

**Ejemplo**: 16-QAM → cada símbolo porta 4 bits → a 1000 baudios = 4000 bps.

**WiFi 6 (802.11ax)** usa **1024-QAM** (10 bits por símbolo).

### 2.2 Multiplexación

La **multiplexación** permite compartir un canal de comunicación entre varias señales simultáneamente. Es lo que hace posible que miles de conversaciones telefónicas o millones de conexiones a Internet compartan la misma infraestructura física.

| Técnica | Sigla | Descripción |
|---------|-------|-------------|
| **Multiplexación por División de Frecuencia** | **FDM/MDF** | Cada señal usa una frecuencia diferente |
| **Multiplexación por División de Tiempo** (síncrona) | **TDM/MDT** | Cada señal usa un intervalo de tiempo fijo |
| **TDM asíncrona** (estadística) | **STDM** | Los intervalos se asignan bajo demanda |
| **Acceso Múltiple por División de Tiempo** | **TDMA** | Varios usuarios en ranuras de tiempo (2G/GSM) |
| **Multiplexación por División Ortogonal de Frecuencia** | **OFDM** | Subportadoras ortogonales; usado en WiFi, ADSL, 4G/5G |
| **OFDM con Acceso Múltiple** | **OFDMA** | OFDM compartido entre usuarios; WiFi 6, 5G |

### 2.3 Modos de comunicación

| Modo | Descripción | Ejemplo |
|------|-------------|---------|
| **Simplex** | Un solo sentido | Televisión, radio |
| **Half-duplex** | Ambos sentidos, no simultáneo | Walkie-talkie, Ethernet half-duplex |
| **Full-duplex** | Ambos sentidos simultáneamente | Teléfono, Ethernet gigabit |

### 2.4 Modos de transmisión

| Modo | Descripción |
|------|-------------|
| **Asíncrona** | Cada carácter va flanqueado por bits de inicio y parada |
| **Síncrona orientada a carácter** | Bloques de datos con caracteres de control (SYN) |
| **Síncrona orientada al bit** | Tramas de bits con flags de inicio y fin (HDLC) |
| **Serie** | Los bits van uno tras otro por un solo canal |
| **Paralelo** | Varios bits simultáneos por múltiples líneas |
| **Banda base** | La señal digital ocupa todo el ancho de banda del medio |
| **Banda ancha** | Varias señales moduladas comparten el medio |

---

## 3. Medios de Transmisión

Los medios de transmisión determinan qué velocidades son posibles, qué distancias se pueden cubrir y qué interferencias hay que gestionar. Se clasifican en:
- **Guiados**: la señal viaja confinada en un conductor físico (cable).
- **No guiados**: la señal se propaga libremente por el espacio (radio, infrarrojos).

### 3.1 Par trenzado

Dos conductores de cobre trenzados entre sí. El trenzado es la clave: las interferencias electromagnéticas externas afectan por igual a los dos hilos, y al restar las señales en el receptor, el ruido se cancela. Cuantas más vueltas por metro, mayor inmunidad al ruido.

#### Tipos según apantallamiento

| Tipo | Sigla | Descripción |
|------|-------|-------------|
| Sin apantallar | **UTP** | Unshielded Twisted Pair |
| Lámina global | **FTP** | Foiled Twisted Pair (una lámina exterior) |
| Par apantallado | **STP** | Shielded Twisted Pair (cada par apantallado) |
| Combinado | **SFTP / S/FTP** | Lámina exterior + cada par apantallado |

> **Trampa de examen**: SBTP y BTP **no existen**. Los tipos válidos son UTP, FTP, STP y SFTP/S/FTP. Si aparecen SBTP o BTP en una respuesta, es la incorrecta.

#### Categorías de par trenzado

| Categoría | Velocidad máx. | Ancho de banda | Aplicación |
|-----------|---------------|----------------|-----------|
| Cat 1 | — | 1 MHz | Telefonía analógica |
| Cat 2 | 4 Mbps | 4 MHz | Token Ring |
| Cat 3 | 10 Mbps | 16 MHz | Ethernet 10BASE-T |
| Cat 4 | 16 Mbps | 20 MHz | Token Ring 16 Mbps |
| **Cat 5** | **100 Mbps** | **100 MHz** | Fast Ethernet |
| **Cat 5e** | **1.000 Mbps** | **100 MHz** | Gigabit Ethernet |
| **Cat 6** | **1.000 Mbps** | **250 MHz** | Gigabit Ethernet |
| **Cat 6A** | **10.000 Mbps** | **500 MHz** | 10GBASE-T (hasta 100 m) |
| Cat 7 | 10.000 Mbps | 600 MHz | 10 Gigabit Ethernet |
| **Cat 8** | **40.000 Mbps** | **2.000 MHz** | 40 Gigabit Ethernet |

#### Conector RJ-45 y normas de cableado

| Norma | Orden de colores (pin 1 a 8) |
|-------|------------------------------|
| **T568A** | Blanco-verde, Verde, Blanco-naranja, Azul, Blanco-azul, Naranja, Blanco-marrón, Marrón |
| **T568B** | Blanco-naranja, Naranja, Blanco-verde, Azul, Blanco-azul, Verde, Blanco-marrón, Marrón |

| Tipo de cable | Uso | Norma extremo A / B |
|--------------|-----|---------------------|
| **Directo** | PC-Switch, PC-Hub | T568B / T568B |
| **Cruzado** | PC-PC, Switch-Switch | T568A / T568B |
| **Consola** | PC-Router (puerto consola) | Inversión total |

### 3.2 Cable coaxial

Estructura concéntrica (como un tubo dentro de otro tubo): conductor central → dieléctrico → malla metálica (apantallamiento) → aislante exterior. El apantallamiento es excelente, lo que le da buena inmunidad al ruido y permite transmitir a mayor distancia que el par trenzado de su época. Hoy casi no se usa en redes LAN (sustituido por par trenzado de categoría alta y fibra), pero sigue siendo relevante en TV por cable y antenas.

| Tipo | Nombre | Impedancia | Longitud máx. | Estándar | Conector |
|------|--------|-----------|---------------|----------|---------|
| **Coaxial fino** | Thinnet, RG-58 | 50 Ω | 185 m | 10BASE2 | BNC |
| **Coaxial grueso** | Thicknet | 50 Ω | 500 m | 10BASE5 | N (vampiro) |

### 3.3 Fibra óptica

Transmite información mediante **pulsos de luz** en un filamento de vidrio o plástico. Es el medio guiado de mayor rendimiento: inmune a interferencias electromagnéticas, sin pérdidas eléctricas, capaz de cubrir miles de kilómetros con atenuación mínima. El backbone de Internet funciona sobre fibra óptica.

#### Estructura

```
[Núcleo (vidrio/plástico)] ← índice de refracción n₁ (mayor)
[Revestimiento (cladding)] ← índice de refracción n₂ (menor, n₂ < n₁)
[Cubierta exterior (coating/jacket)]
```

La diferencia de índices de refracción provoca la **reflexión interna total**, manteniendo la luz confinada en el núcleo.

#### Ventanas de transmisión

| Ventana | Longitud de onda | Atenuación |
|---------|-----------------|-----------|
| 1.ª | 850 nm | Mayor (~3 dB/km) |
| 2.ª | 1.300 nm | Media (~0,5 dB/km) |
| 3.ª | 1.550 nm | Menor (~0,2 dB/km) |

#### Tipos de fibra óptica

| Tipo | Diámetro núcleo | Propagación | Fuente de luz | Uso |
|------|----------------|-------------|---------------|-----|
| **Monomodo (SMF)** | 8-10 µm | Un solo modo de luz | Láser (ILD) | Larga distancia, alta velocidad |
| **Multimodo escalonado** | 50-62,5 µm | Múltiples modos, el haz rebota en las paredes | LED | Distancias cortas |
| **Multimodo gradual** | 50-62,5 µm | Múltiples modos con índice gradual | LED | Distancias medias |

#### Fuentes de luz

| Fuente | Tipo | Ventana | Tipo de fibra |
|--------|------|---------|---------------|
| **LED** | Emisor de luz de baja potencia | 1.ª ventana (850 nm) | Multimodo |
| **ILD/Láser** | Emisor de alta potencia y coherencia | 2.ª y 3.ª ventana | Monomodo |

#### Conectores de fibra óptica

- **ST**: bayoneta, uso en redes LAN.
- **SC**: cuadrado, fácil conexión push-pull.
- **LC**: pequeño formato, alta densidad.
- **FC**: rosca, entornos con vibraciones.

### 3.4 Cableado estructurado

Sistema de cableado planificado que sigue normas internacionales para instalaciones de telecomunicaciones en edificios. El objetivo es que el cableado sea independiente de las aplicaciones: el mismo cable que hoy se usa para Ethernet podría usarse mañana para otro protocolo, sin tener que repasar el edificio.

**Normas principales:**
- **ISO/IEC 11801**: norma internacional.
- **EN 50173**: norma europea.
- **TIA-568**: norma americana.

**Jerarquía del cableado:**
```
Área de trabajo (estaciones)
    ↓ Cableado horizontal (máx. 90 m)
Armario de planta (TR - Telecommunications Room)
    ↓ Cableado vertical / backbone
Sala de equipos (ER - Equipment Room)
    ↓ Backbone de campus
Instalación de entrada
```

**Clases de cableado:** A (voz), B, C (Cat 3), D (Cat 5), E (Cat 6), F (Cat 7).

### 3.5 Transmisión por radio (no guiada)

#### Espectro electromagnético en comunicaciones

| Banda | Frecuencia | Uso |
|-------|-----------|-----|
| VLF | 3-30 kHz | Comunicaciones militares submarinas |
| LF | 30-300 kHz | Radionavegación |
| MF | 300 kHz-3 MHz | Radio AM |
| HF | 3-30 MHz | Radio de onda corta |
| VHF | 30-300 MHz | Radio FM (87,5-108 MHz), DAB (195-223 MHz), TDT (470-790 MHz) |
| UHF | 300 MHz-3 GHz | Telefonía móvil, WiFi, Bluetooth |
| SHF/EHF | 3-300 GHz | Satélite, WiMAX, 5G |

#### Tipos de antenas

- **Yagi**: directiva, para TV terrestre.
- **Parabólica**: muy directiva, satélite y microondas.
- **Panel**: sectorial, cobertura de 60-120°.
- **Monopolar/Omnidireccional**: 360°, puntos de acceso WiFi.

#### Propiedades de la propagación

- **Absorción**: la señal pierde energía al atravesar obstáculos.
- **Reflexión**: rebote en superficies metálicas.
- **Refracción**: cambio de dirección al pasar entre medios.
- **Difracción**: la señal rodea obstáculos.

---

## 4. Equipos Terminales

La distinción DTE/DCE es conceptualmente importante: el DTE es el dispositivo del usuario que quiere comunicarse; el DCE es el "traductor" que adapta esa señal al medio de transmisión. Un PC (DTE) se conecta a un módem (DCE) que a su vez se conecta a la red telefónica.

### 4.1 DTE (Data Terminal Equipment)

Dispositivos que generan o consumen datos. En la práctica, todo lo que el usuario opera directamente.

- **Terminales no inteligentes**: teclado + pantalla, sin capacidad de proceso local.
- **Terminales inteligentes**: PCs y ordenadores.
- **Periféricos de entrada**: teclado, ratón, escáner, cámara.
- **Periféricos de salida**: monitor, impresora.
- **Periféricos mixtos (entrada y salida)**: memoria USB, disco duro externo, pantalla táctil.

### 4.2 DCE (Data Circuit-terminating Equipment)

Dispositivos que adaptan la señal del DTE al medio de transmisión.

- **Módem externo**: se conecta al PC por puerto serie o USB.
- **Módem interno**: tarjeta de expansión.
- **Módem ADSL (banda ancha)**: acceso a Internet por línea telefónica.
- **Router-módem**: combina módem y router.
- **Multiplexores**: combinan varias señales en un canal.

### 4.3 Interfaces DTE-DCE

| Interfaz | Norma | Conector | Velocidad |
|---------|-------|----------|----------|
| **RS-232 / V.24** | EIA-232 | DB9, DB25, RJ11 | Hasta 20 kbps |
| **V.92** | ITU-T | RJ11 | 48 kbps subida, 56 kbps bajada |
| **X.21** | CCITT | DB15 | 600-6.400 bps |

**Señales principales de RS-232 (V.24):**

| Señal | Nombre | Dirección |
|-------|--------|-----------|
| **TXD** | Transmitted Data | DTE → DCE |
| **RXD** | Received Data | DCE → DTE |
| **RTS** | Request To Send | DTE → DCE |
| **CTS** | Clear To Send | DCE → DTE |
| **DTR** | Data Terminal Ready | DTE → DCE |
| **DSR** | Data Set Ready | DCE → DTE |

### 4.4 Periféricos, almacenamiento y conectores

Aunque los periféricos tienen su tema propio, los exámenes de este bloque incluyen preguntas sobre hardware de puesto de trabajo: buses de comunicación, almacenamiento local, pantallas y tipos de zócalos de CPU.

#### Bus USB y protocolos de transferencia

| Versión | Nombre comercial | Velocidad máx. |
|---------|-----------------|----------------|
| USB 1.1 | Full Speed | 12 Mbps |
| USB 2.0 | High Speed | 480 Mbps |
| **USB 3.0 / 3.1 Gen 1 / 3.2 Gen 1** | **SuperSpeed** | **5 Gbps** |
| USB 3.1 Gen 2 / 3.2 Gen 2 | SuperSpeed+ | 10 Gbps |
| USB 3.2 Gen 2×2 | SuperSpeed+ | 20 Gbps |
| USB4 | — | 40 Gbps |

**Protocolos de transferencia USB:**
- **USB Mass Storage (UMS)**: el ordenador monta el dispositivo como unidad de disco; el dispositivo pierde el control de su sistema de archivos mientras está conectado. Riesgo de corrupción si se desconecta durante una transferencia.
- **MTP (Media Transfer Protocol)**: el dispositivo retiene el control de su sistema de archivos; el ordenador solicita archivos mediante el protocolo. Permite acceso simultáneo desde el ordenador y el propio dispositivo. Es más seguro y el estándar en dispositivos Android modernos.
- **PTP (Picture Transfer Protocol)**: precursor de MTP, diseñado para cámaras digitales y transferencia de imágenes.

**Sistema de impresión CUPS (Linux):**
- **CUPS** (Common Unix Printing System): sistema de impresión estándar en Linux y macOS.
- Cada impresora tiene un fichero PPD (PostScript Printer Description) en `/etc/cups/ppd/<nombre>.ppd`.
- Ejemplo: la impresora `PRINTER1` → `/etc/cups/ppd/PRINTER1.ppd`.
- La configuración principal está en `/etc/cups/cupsd.conf` y la lista de impresoras en `/etc/cups/printers.conf`.

#### Tipos de zócalos de CPU

| Tipo | Descripción | Sustituible |
|------|-------------|-------------|
| **LGA** (Land Grid Array) | Pines en el zócalo de la placa; contactos planos en el CPU (Intel desktop) | Sí |
| **PGA** (Pin Grid Array) | Pines en el CPU; agujeros en el zócalo (AMD AM4 y anteriores) | Sí (ZIF) |
| **BGA** (Ball Grid Array) | Bolas de soldadura en el CPU; soldado directamente a la placa | **No** (soldado) |

> **Para el examen**: BGA = soldado, NO sustituible. Es el formato estándar en portátiles, smartphones y dispositivos embebidos.

#### Conectores de vídeo

Conectores diseñados para transmitir señal de vídeo digital:
- **HDMI**: el más común en televisores y monitores; transmite vídeo y audio por un solo cable.
- **DisplayPort**: usado principalmente en monitores de PC; soporta muy altas resoluciones y frecuencias.
- **DVI**: Digital Visual Interface; en desuso pero aún presente en monitores y GPUs antiguas.
- **VGA**: analógico; obsoleto pero todavía presente en proyectores y equipos legacy.

> **Trampa de examen**: **DIMM** (Dual Inline Memory Module) es un módulo de memoria RAM, NO un conector de vídeo.

#### Tecnologías de pantalla

| Tecnología | Retroiluminación | Negros | Tiempo de respuesta | Multitouch |
|-----------|-----------------|--------|---------------------|------------|
| **LCD/LED** | Sí (backlight LEDs) | No perfectos (hay fuga de luz) | ~1-10 ms | Depende de la capa táctil |
| **OLED** | **No** (cada píxel emite su propia luz) | Perfectos (píxel apagado = negro absoluto) | ~0,1 µs | Sí (con capa táctil capacitiva) |

> **Para el examen**: OLED **no requiere retroiluminación**. Es precisamente su mayor ventaja estructural frente a LCD.

#### Pantallas táctiles

| Tipo | Funcionamiento | Ventajas | Desventajas |
|------|---------------|---------|-------------|
| **Resistiva** | Presión mecánica sobre capas flexibles | Funciona con guantes, lápices, cualquier objeto | Sin multitouch; menos durable; menor precisión |
| **Capacitiva** | Perturbación del campo eléctrico del vidrio conductor | Multitouch, más durable, alta precisión | No funciona con guantes normales |

> En entornos de atención al público (ventanillas, quioscos), las pantallas **capacitivas** son preferibles por su mayor durabilidad y soporte multitouch. Las **resistivas** son mejores en entornos industriales o con guantes.

#### Almacenamiento y RAID

**Tipos de almacenamiento según partes móviles:**
- **HDD (Hard Disk Drive)**: tiene platos magnéticos giratorios y cabezales mecánicos. Puede fallar por desgaste mecánico, golpes o vibraciones. Sus partes móviles son su punto débil.
- **SSD (Solid State Drive)**: memoria flash NAND sin partes móviles. Más rápido, silencioso y resistente a golpes.
- **Memoria USB / Tarjetas SD**: también flash NAND sin partes móviles.

**RAID (Redundant Array of Independent Disks):**

| Nivel | Técnica | Paridad | Mín. discos | Tolerancia a fallos |
|-------|---------|---------|-------------|---------------------|
| **RAID 0** | Striping | **No** | 2 | Ninguna (si falla 1, se pierden todos los datos) |
| **RAID 1** | Mirroring | No | 2 | 1 disco |
| **RAID 3** | Striping bytes + paridad dedicada | Sí (1 disco dedicado) | 3 | 1 disco |
| **RAID 4** | Striping bloques + paridad dedicada | Sí (1 disco dedicado) | 3 | 1 disco |
| **RAID 5** | Striping + paridad distribuida | **Sí (distribuida entre todos)** | 3 | 1 disco |
| **RAID 6** | Striping + doble paridad distribuida | **Sí (doble, distribuida)** | 4 | 2 discos simultáneos |

> **Para el examen**: RAID 0 mejora la velocidad **sin paridad**. RAID 5 y RAID 6 distribuyen la paridad entre **todos** los discos (no hay un disco dedicado). RAID 3 y 4 tienen un disco dedicado exclusivamente a paridad.

---

## 5. Equipos de Interconexión y Conmutación

Estos equipos son los que construyen la red: conectan segmentos, filtran tráfico, encaminan paquetes entre redes distintas. La clave para el examen es saber en qué capa OSI opera cada uno y qué hace con el tráfico.

### 5.1 Tabla comparativa de equipos

| Equipo | Capa OSI | Función principal | Dominio |
|--------|----------|------------------|---------|
| **Repetidor** | Física (1) | Regenera la señal eléctrica | Colisión ampliado |
| **Hub** | Física (1) | Concentra conexiones, emite por todos los puertos | Colisión compartido |
| **Bridge** | Enlace (2) | Filtra tramas por dirección MAC | Separa dominios de colisión |
| **Switch** | Enlace (2) | Conmuta tramas, aprende MACs, crea canales exclusivos | Elimina colisiones |
| **Router** | Red (3) | Enruta paquetes entre redes distintas | Separa dominios de broadcast |
| **Gateway** | Aplicación (7) | Conecta redes con protocolos diferentes | Traduce entre protocolos |

### 5.2 Hub (Concentrador)

- Opera en la **capa física**.
- Puede ser **pasivo** (sin amplificación) o **activo** (amplifica la señal).
- Todo el tráfico se envía por **todos los puertos** simultáneamente.
- Todos los dispositivos comparten el mismo **dominio de colisión**.
- Hoy en día está prácticamente sustituido por switches.

### 5.3 Bridge (Puente)

- Opera en las capas **física y de enlace**.
- **Filtra el tráfico** entre segmentos de red basándose en las direcciones MAC.
- Aprende qué dispositivos están en cada segmento y solo reenvía tramas cuando es necesario.
- **Separa dominios de colisión** pero no dominios de broadcast.

### 5.4 Switch (Conmutador)

- Opera en la **capa de enlace**.
- Mantiene una **tabla de direcciones MAC** para reenviar tramas solo al puerto destino.
- Crea **canales exclusivos** entre origen y destino (full-duplex posible).
- **Elimina colisiones** en cada puerto.
- Las **VLANs** permiten segmentar lógicamente la red en un switch.

### 5.5 Router (Encaminador)

- Opera en la **capa de red**.
- Mantiene una **tabla de enrutamiento** para decidir el mejor camino.
- **Separa dominios de broadcast**.
- Conecta redes con **diferentes tecnologías** (LAN-WAN).

**Algoritmos de enrutamiento:**

| Tipo | Descripción | Protocolo |
|------|-------------|-----------|
| **Estático** | Rutas configuradas manualmente | — |
| **Vector de distancias** | Comparte la tabla completa con vecinos | **RIP** (métrica: saltos, máx. 15, actualizaciones c/30 s) |
| **Estado de enlace** | Comparte el estado de enlaces con toda la red | **OSPF**, IS-IS |

### 5.6 Gateway (Compuerta)

- Puede operar en **todas las capas** del modelo OSI.
- **Traduce entre protocolos diferentes** (ej. de IPv4 a IPv6, de TCP/IP a SNA).
- Usa protocolos como **ARP**, **IGRP**, **RIP** para comunicarse con otras redes.

### 5.7 VLAN (Virtual LAN)

- Redes lógicas independientes creadas sobre una única infraestructura física.
- **Características:**
  - Hosts en VLANs diferentes no se comunican entre sí sin un router.
  - Cada VLAN tiene un nombre y un número (1-1023).
  - Se configuran en el switch por puerto, MAC o protocolo.

| Tipo de VLAN | Descripción |
|-------------|-------------|
| **Predeterminada** | Todos los puertos en el mismo dominio de difusión |
| **Nativa** | Para puertos troncales (tagged + untagged) |
| **De datos/usuario** | Transporta tráfico de usuarios |
| **De administración** | Gestión del switch (HTTP, Telnet, SSH, SNMP) |

---

## 6. Redes de Comunicaciones

### 6.1 Clasificación según la propiedad

Las redes se pueden clasificar por quién las gestiona: el operador de telecomunicaciones (redes públicas) o el propio usuario o empresa (redes privadas). Esta distinción es importante porque afecta a quién es responsable del mantenimiento, la seguridad y los costes.

#### Redes públicas

Gestionadas por operadores de telecomunicaciones, accesibles mediante abono. El usuario paga por usar la infraestructura, pero no la posee.

#### 6.1.1 RTB (Red Telefónica Básica / STDP)

- Par de cobre (bucle de abonado).
- Velocidad máxima: **56 kbps** (con módem V.92).
- Punto de Terminación de Red (**PTR**): límite entre red pública y red del usuario.

#### 6.1.2 RDSI (Red Digital de Servicios Integrados / ISDN)

Transmisión digital completa de voz y datos a través de la red telefónica.

**Grupos funcionales RDSI:**

| Equipo | Función |
|--------|---------|
| **TE1** | Terminal nativo RDSI |
| **TE2** | Terminal analógico (necesita TA) |
| **TA** | Adaptador de terminal (TE2 → interfaz RDSI) |
| **NT1** | Terminación de red de capa 1 |
| **NT2** | Terminación de red de capa 2 (centralita, PBX) |
| **LT** | Terminación de línea (en la central) |
| **ET** | Terminación de intercambio (en la central) |

**Puntos de referencia:**

| Punto | Ubicación |
|-------|-----------|
| **S** | Entre TE1 y NT2 / entre TA y NT2 |
| **R** | Entre TE2 y TA |
| **T** | Entre NT2 y NT1 |
| **U** | Bucle de abonado (NT1 a la central) |
| **V** | Entre LT y ET |

**Canales RDSI:**

| Canal | Descripción |
|-------|-------------|
| **Canal B** | 64 kbps, transporte de voz y datos |
| **Canal D** | 16/64 kbps, señalización |
| **Canal H** | Datos a alta velocidad (H0=384 kbps, H11=1.536 kbps, H12=1.920 kbps) |

**Modos de acceso:**

| Modo | Estructura | Velocidad total |
|------|-----------|----------------|
| **BRI** (Basic Rate Interface) | 2B + D (D=16 kbps) | 144 kbps |
| **PRI** (Primary Rate Interface) | 30B + D (D=64 kbps) | 1.984 kbps |

**Servicios RDSI:**
- **Portadores**: telefonía básica (4 kHz), datos digitales (64 kbps).
- **Teleservicios**: telefonía básica, telefonía 7 kHz, videotex, fax, correo electrónico.
- **Servicios de difusión**: TV y radio.
- **Servicios suplementarios**: identificación de llamada, llamada en espera, llamada a tres, desvío, retención, marcación abreviada, extensiones directas.

#### 6.1.3 ADSL y familia xDSL

**ADSL** (Asymmetric DSL): transmisión asimétrica de voz, datos y vídeo sobre el bucle de abonado de la RTB, en el rango de 300 Hz a 1,104 MHz.

**Componentes de una red ADSL:**
- **ATU-R**: módem ADSL del usuario.
- **ATU-C** (DSLAM): módem ADSL de la central. Cada entrada conecta un ATU-C.
- **Splitter**: separa la señal de voz (POTS) de los datos ADSL.
- **Microfiltro**: filtra la señal ADSL del teléfono del usuario.

**Canales ADSL:**
- **Downstream** (bajada): 138 kHz – 1,104 MHz, hasta 9 Mbps a menos de 3 km.
- **Upstream** (subida): 25 kHz – 138 kHz, 16-640 kbps.
- **POTS** (voz): 300 Hz – 4 kHz.

**Modulación ADSL:** CAP (Carrierless Amplitude Phase) o **DMT** (Discrete Multitone Modulation, estándar).

**Familia xDSL:**

| Tipo | Descripción | Velocidad | Infraestructura |
|------|-------------|-----------|----------------|
| **UDSL / ADSL Lite** | Universal DSL | 0,5-1 Mbps | Par de cobre |
| **ADSL** | Asimétrico | Hasta 9 Mbps bajada | Par de cobre |
| **ADSL2** | Mejorado con Trellis | 12 Mbps bajada, 2 Mbps subida (< 5 km) | Par de cobre |
| **ADSL2+** | Doble ancho de banda (2.208 MHz) | 20 Mbps bajada, 2 Mbps subida (< 1,5 km) | Par de cobre |
| **HDSL** | Simétrico | 2 × 2 Mbps simétrico | Dos pares de cobre |
| **SHDSL** | Similar a HDSL | Similar a HDSL | Un par de cobre |
| **SDSL** | Simétrico par único | Equivalente a un canal HDSL | Un par de cobre |
| **VDSL** | Muy alta velocidad | Hasta 50 Mbps (< 500 m) | Par de cobre |

#### 6.1.4 Redes privadas

Gestionadas por el propio usuario o empresa. La organización tiene control total sobre la infraestructura, la seguridad y las políticas de acceso.

**Tipología de redes privadas:**
- LAN 802.x alámbrica o inalámbrica.
- LAN centralizada o descentralizada (peer-to-peer o cliente-servidor).
- Topologías: anillo, bus, estrella, malla.
- VPN (Virtual Private Network).
- MAN de ámbito empresarial o universitario.

**Prestaciones:**
- Compartir recursos (archivos, aplicaciones, hardware).
- Comunicación interpersonal (email, chat).
- Acceso a bases de datos.
- Teletrabajo, telemedicina.

### 6.2 Clasificación según el ámbito de implantación

| Tipo | Ámbito | Velocidad típica |
|------|--------|-----------------|
| **PAN** (Personal Area Network) | < 10 m | Bluetooth (2,4 GHz) |
| **LAN** (Local Area Network) | Edificio / planta | 10 Mbps – varios Gbps |
| **MAN** (Metropolitan Area Network) | Ciudad / municipio | 10 Mbps – 10 Gbps |
| **WAN** (Wide Area Network) | País / continente / global | Variable |

#### LAN (Local Area Network)

- Ámbito local (edificio, planta).
- **Arquitecturas:** Peer-to-Peer (hogar) y Cliente-Servidor (empresa).
- **Velocidades:** 10-100 Mbps estándar; hasta varios Gbps en casos especiales.
- **Seguridad:** contraseña, filtrado MAC, firewall, antimalware.
- **SAN** (Storage Area Network): conecta múltiples unidades de almacenamiento a la LAN.

#### MAN (Metropolitan Area Network)

- Cubre municipios o grupos de municipios.
- **Medios:** fibra óptica (anillo FDDI / 802.5), par trenzado MAN BUCLE, Wi-Fi, WiMAX.
- **Tecnologías:** ATM, Frame Relay, xDSL, WDM, E1/T1, PPP.
- **Velocidades:** cobre 10-20 Mbps, fibra 100 Mbps – 10 Gbps.

#### WAN (Wide Area Network)

**Protocolos WAN:**

| Nivel | Protocolo | Descripción |
|-------|-----------|-------------|
| Físico | **X.21** | Conmutación de circuitos |
| Físico | **G.703** | Interfaces físicas ITU-T |
| Físico | **HSSI** | Serie de alta velocidad (hasta 52 Mbps) |
| Enlace | **ATM** | Conmutación de celdas (53 bytes fijas) |
| Enlace | **HDLC** | Punto a punto y multipunto |
| Enlace | **LAPB** | Para X.25 y Frame Relay |
| Red | **TCP/IP** | Protocolo de Internet |
| Red | **X.25** | Conmutación de paquetes |
| Red | **PPP** | Enlace de datos multi-protocolo |
| Red | **PPTP** | VPN sobre redes públicas |

**Tipos de redes WAN:**

| Tipo | Características | Ejemplos |
|------|----------------|---------|
| **Orientada a la conexión** | Establece circuito antes de transmitir; garantiza orden | X.25, Frame Relay, ATM, red telefónica |
| **No orientada a la conexión** | Cada paquete va independiente; sin garantía de orden | Internet (TCP/IP), correo electrónico, HTTP |

**Tipos de servidores en Internet:**

| Servidor | Función |
|---------|---------|
| **Proxy** | Intermediario; caché, filtrado, anonimato |
| **DNS** | Resolución de nombres de dominio a IP |
| **Web** | Publica páginas web (Apache, IIS, Nginx) |
| **Base de datos** | Almacena y gestiona datos |
| **Correo** | Envío (SMTP) y recepción (POP3/IMAP) de correo |
| **Clúster** | Alta disponibilidad y escalabilidad |

---

## 7. Redes de Conmutación

La **conmutación** es la técnica que permite que millones de dispositivos compartan la misma infraestructura de red sin necesitar un cable directo entre cada par de ellos. Sin conmutación, necesitarías N×(N-1)/2 cables para conectar N dispositivos. Con conmutación, basta con N cables a un nodo central.

**Taxonomía de redes de conmutación:**
```
Redes de comunicación
├── Conmutadas
│   ├── Conmutación de circuitos (red telefónica básica)
│   ├── Conmutación de paquetes
│   │   ├── Orientadas a la conexión (Frame Relay, X.25, ATM)
│   │   └── No orientadas a la conexión (Internet)
│   └── Conmutación de mensajes (fax, telégrafo)
└── Por difusión
    ├── Ethernet
    ├── Radiopaquetes
    └── Redes con satélites
```

### 7.1 Conmutación de circuitos

Se establece un **camino físico dedicado** entre los terminales antes de comenzar la transmisión.

**Fases:**
1. Los terminales inician la llamada con la dirección de destino.
2. Los equipos de conmutación establecen el camino físico.
3. La conexión se mantiene activa hasta que un terminal la finaliza.

| Ventajas | Desventajas |
|---------|-------------|
| Transmisión en tiempo real sin retardos | Retraso en el establecimiento de la conexión |
| Velocidad máxima garantizada durante toda la sesión | No aprovecha caminos alternativos más eficientes |
| Transparente para los terminales | Baja tolerancia a fallos (si falla un nodo, se pierde la conexión) |

#### La red telefónica conmutada

**Jerarquía de centrales (de mayor a menor nivel):**

| Central | Tipo | Descripción |
|---------|------|-------------|
| **CT** (Central Terciaria/Nodal) | Más alto orden (solo 6 en España) | Conecta centrales secundarias y otras terciarias |
| **CN** (Central Nodal) | Terciaria | Conecta centrales secundarias de una región |
| **CS** (Central Secundaria) | Segundo nivel | Conecta centrales primarias |
| **CP** (Central Primaria) | Tercer nivel | Conecta centrales locales |
| **CL** (Central Local) | Base | Centraliza grupos de abonados |

**Tipos de centrales locales:**
- **CT** (Central Terminal): áreas rurales.
- **CUO** (Central Urbana Ordinaria): conecta abonados urbanos a una Central Primaria.
- **CUNO** (Central Urbana No Ordinaria): se conecta a una Central Tandem.
- **Centrales Tandem**: en áreas densamente pobladas, sin conexión directa a abonados.

**Conexión entre centrales y abonados:**
- **Bucle de abonado**: conecta el terminal con la central local.
  - **Bucle local de pares de cobre**: telefonía y datos a baja velocidad.
  - **Bucle local multiservicio**: fibra óptica o cable coaxial para TV, telefonía y datos.
  - **Bucle local vía radio**: antenas con varios anchos de banda.

**Señalización en la red telefónica:**
- **De abonado** (CAS): tonos de marcación, descolgado/colgado.
- **De troncal** (CCS): señalización en canal común para todos los circuitos.
  - De línea, de registro, de tarificación.

### 7.2 Conmutación de mensajes

El más antiguo de los métodos de conmutación (origen en el **telégrafo** y el télex). Funciona como un sistema de mensajería interna de oficina: el mensaje completo va pasando de nodo en nodo.

- Método **Store and Forward**: el mensaje completo se almacena en cada nodo intermedio hasta que puede reenviarlo al siguiente. No se necesita un camino reservado.
- **Ventaja**: garantiza la entrega del mensaje completo y no desperdicias capacidad de red si no hay tráfico.
- **Desventaja**: muy lento para mensajes largos (el mensaje entero debe llegar a cada nodo antes de reenviarlo). Inviable para comunicación en tiempo real.

### 7.3 Conmutación de paquetes

La información se divide en **paquetes** que se envían independientemente por la red. Es la base de Internet. La gran ventaja sobre la conmutación de mensajes es que no hay que esperar a que llegue el mensaje completo a cada nodo: en cuanto llega el primer paquete, se reenvía. Además, si un camino está congestionado, los paquetes pueden ir por rutas alternativas.

#### El datagrama

- Unidad mínima de información en redes de conmutación de paquetes.
- Estructura: **cabecera** (control, direcciones) + **datos**.
- Cada paquete puede tomar rutas diferentes.
- Los paquetes se reensamblan en el destino.

**Retardos en conmutación de paquetes:**

| Retardo | Descripción | Fórmula |
|---------|-------------|---------|
| **T_esp** | Tiempo en cola del router | Depende del tráfico |
| **T_proc** | Procesamiento en routers | Depende del hardware |
| **T_prop** | Propagación en el medio | T = Distancia / Velocidad |
| **T_tx** | Transmisión del paquete | T = Longitud / Velocidad de enlace |
| **RTT** | Round Trip Time | Suma de todos los retardos de ida y vuelta |

#### Circuitos virtuales

La conmutación de paquetes puede ser sobre **circuitos virtuales** (todos los paquetes siguen la misma ruta predefinida):

| Tipo | Descripción |
|------|-------------|
| **PVC** (Permanent Virtual Circuit) | Circuito siempre activo entre dos puntos |
| **SVC** (Switched Virtual Circuit) | Circuito temporal, se crea y destruye según necesidad |

#### 7.3.1 Red X.25

- Red de conmutación de paquetes para terminales síncronos y asíncronos.
- Utilizada en España como **Iberpac** (primera red de datos de Telefónica).
- Requiere **PAD** (Packet Assembly-Disassembly) para terminales asíncronos.

**Capas del protocolo X.25:**
1. **Capa Física**: interfaces X.21, X.21bis (equivalente RS-232).
2. **Capa de Enlace**: protocolo **LAPB** (variante de HDLC).
3. **Capa de Paquete X.25**: gestiona circuitos virtuales (6 formatos de paquetes: transporte de datos, control de llamadas virtuales, control de flujo, reinicio, etc.).

#### 7.3.2 Frame Relay

Protocolo de conmutación de tramas para redes WAN.

**Características principales:**
- Opera en los niveles **físico y de enlace** del modelo OSI.
- Tramas de longitud variable (hasta **9 kB**), mayor flexibilidad que X.25.
- Conexiones virtuales: **PVC** y **SVC**.
- Identificación mediante **DLCI** (Data Link Connection Identifier).
- Más económico que otras tecnologías WAN.
- No adecuado para tiempo real (audio, vídeo).

**Gestión de congestión:**
- **BECN** (Backward Explicit Congestion Notification): indica congestión en el sentido de retorno.
- **FECN** (Forward Explicit Congestion Notification): indica congestión en el sentido de avance.

**Componentes Frame Relay:**
- **FRAD** (Frame Relay Assembler/Disassembler): ensambla y desensambla tramas de otros protocolos.
- **LAPF** (Link Access Procedure for Frame Relay): control de enlace.
- **LAPD** (Link Access Procedure for D-channel): comunicación de usuario.

#### 7.3.3 ATM (Modo de Transferencia Asíncrono)

Tecnología de conmutación de **celdas de tamaño fijo** para alta velocidad.

**Características:**
- Celdas de **53 octetos** (5 cabecera + 48 datos).
- Velocidades desde 155,52 Mbps (STM-1) hasta 9.953,28 Mbps (STM-64).
- Transmisión mediante tecnologías ópticas: **SDH** (Synchronous Digital Hierarchy) y **SONET**.

**Arquitectura ATM (capas):**

| Capa | Función |
|------|---------|
| **Capa Física** | Conversión al medio físico (subcapas TC y PMD) |
| **Capa ATM** | Transmisión de celdas en conexiones lógicas |
| **Capa de Adaptación ATM (AAL)** | Adapta protocolos externos (subcapas CS y SAR) |

**Planos ATM:**
- **Plano de Usuario**: transfiere datos de usuario.
- **Plano de Control**: gestiona conexiones y llamadas.
- **Plano de Gestión**: coordina todos los aspectos.

**Conexiones ATM:**
- **VCC** (Virtual Channel Connection): rutas básicas entre dos puntos.
- **VPC** (Virtual Path Connection): agrupa varios VCC con mismo origen y destino.

**Estructura de la celda ATM (UNI):**

| Campo | Tamaño | Descripción |
|-------|--------|-------------|
| GFC | 4 bits | Control de flujo genérico |
| VPI | 8 bits | Identificador de camino virtual |
| VCI | 16 bits | Identificador de canal virtual |
| PT | 3 bits | Tipo de carga útil |
| CLP | 1 bit | Prioridad de pérdida (1 = primera en descartarse) |
| HEC | 8 bits | Control de errores en la cabecera |
| Datos | 48 bytes | Carga útil |

**Tipos de conmutadores ATM:**
- Por División de Tiempo.
- Por División de Espacio (matriz de conmutación).
- Matriz Totalmente Conectada.
- Batcher-Banyan (Matriz Delta).

#### 7.3.4 MPLS (Conmutación de Etiquetas Multiprotocolo)

Mecanismo de transporte entre la capa de enlace y la capa de red.

**Elementos MPLS:**

| Elemento | Nombre completo | Función |
|---------|----------------|---------|
| **LER** | Label Edge Router | Inicia/termina el túnel MPLS; añade/elimina etiquetas |
| **LSR** | Label Switching Router | Conmuta paquetes por etiqueta (no por IP) |
| **LSP** | Label Switched Path | Camino unidireccional a través de la red MPLS |
| **LDP** | Label Distribution Protocol | Distribuye etiquetas entre routers |
| **FEC** | Forwarding Equivalence Class | Grupo de paquetes con el mismo tratamiento de reenvío |

> **Para el examen:** FEC = grupo de paquetes que siguen el mismo camino; LSR = router interno; LER = router de borde.

---

## 8. Redes de Difusión

Las redes de difusión (**broadcast**) transmiten información desde un nodo emisor a múltiples receptores simultáneamente, sin necesidad de retransmisión nodo a nodo.

**Características:**
- Transmisión unidireccional desde un punto central.
- El mensaje es recibido por todos los nodos conectados.

**Ejemplos:**
- Televisión y radio (ondas electromagnéticas, cable o Internet).
- Streaming broadcast (Netflix, YouTube).
- Redes LAN (modo broadcast).

### 8.1 Difusión en redes IPv4

| Modalidad | Descripción | Dirección |
|-----------|-------------|-----------|
| **Difusión amplia limitada** (Limited Broadcast) | Solo dentro de la red física local | 255.255.255.255 |
| **Multidifusión** (Multicast) | A grupos específicos de nodos | 224.0.0.0 – 239.255.255.255 |

**Ejemplo**: en la red 192.168.11.0/24, la dirección de broadcast es 192.168.11.255.

### 8.2 Difusión en redes IPv6

IPv6 **elimina el concepto de broadcast** y usa técnicas más eficientes:

| Tipo | Descripción |
|------|-------------|
| **Multicast** | Envío a un grupo de nodos predefinidos; direcciones comienzan por FF00 |
| **Anycast** | Envío al nodo más cercano de un grupo; útil para balanceo de carga |

### 8.3 Aplicaciones prácticas de la difusión

- **Descubrimiento de servicios**: Bonjour, mDNS.
- **Videoconferencia y streaming**: transmisión simultánea a múltiples receptores (H.323 para videoconferencias sobre IP).
- **Uso malintencionado**: ataques DoS por inundación (flooding), usando la IP víctima como origen.

### 8.4 Multidifusión en Internet

| Protocolo/Técnica | Descripción |
|------------------|-------------|
| **IEEE 802.1aq (SPB)** | Shortest Path Bridging; mejora enrutamiento multicast en LAN |
| **IP Multicast** | Envío eficiente a grupos específicos (clase D) |
| **IRC** | Chat en tiempo real basado en texto |
| **NNTP** | Protocolo para grupos de noticias Usenet |
| **Peercasting** | Distribución peer-to-peer de contenido multimedia |
| **Flooding** | Envío a todas las interfaces; puede provocar múltiples copias |

---

## 9. Comunicaciones Móviles y Redes Inalámbricas

### 9.1 Clasificación de redes inalámbricas

| Tipo | Estándar | Alcance | Tecnología |
|------|----------|---------|-----------|
| **WPAN** | IEEE 802.15 | < 10 m | Bluetooth, ZigBee, IrDA |
| **WLAN** | IEEE 802.11 | < 100 m | WiFi |
| **WMAN** | IEEE 802.16 | < 50 km | WiMAX |
| **WWAN** | — | Global | 2G/3G/4G/5G |

### 9.2 Regulación del espectro en España

El **CNAF** (Cuadro Nacional de Atribución de Frecuencias) regula el uso del espectro radioeléctrico en España.

**PIRE** (Potencia Isótropa Radiada Equivalente): potencia total radiada por la antena.

| Banda | Límite PIRE |
|-------|------------|
| 2,4 GHz (802.11b/g) | 100 mW (20 dBm) |
| 5,15-5,25 GHz | Solo interiores |
| 5,15-5,36 GHz (802.11a) | 200 mW |
| 5,47-5,725 GHz (802.11n) | 1 W |

### 9.3 La red WiFi (IEEE 802.11)

#### Componentes

- **Punto de acceso (AP)**: emisor/receptor de radio; actúa como enlace entre red cableada e inalámbrica.
- **Antenas**: irradian y reciben la señal electromagnética.
- **Terminal WiFi**: tarjeta inalámbrica (PCI, PCMCIA, USB).

#### Tipología

| Modo | Descripción |
|------|-------------|
| **Ad-hoc / Peer-to-Peer** | Sin infraestructura fija; los dispositivos se conectan directamente |
| **Mesh** | APs en diferentes canales que se comunican entre sí |
| **Infraestructura** | APs conectados a red cableada; mayor eficiencia |

#### Capas del modelo 802.11

- **Capa física:**
  - Subcapa **PMD** (Physical Medium Dependent): modulación, DSSS (Direct Sequence Spread Spectrum).
  - Subcapa **PLCP** (Physical Layer Convergent Procedure): preámbulo y cabecera.
- **Capa de enlace:**
  - Subcapa **MAC**: CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance).
    - **DCF** (Distributed Coordination Function): modo básico con CSMA/CA.
    - **NAV** (Network Allocation Vector): reserva del medio.
    - **RTS/CTS**: resolución del problema del terminal oculto.
  - Subcapa **LLC**: común a todos los estándares 802.

#### Versiones WiFi 802.11

| Estándar | Nombre | Frecuencia | Velocidad máx. | Modulación |
|---------|--------|-----------|----------------|-----------|
| 802.11 (legacy) | Legado | 2,4 GHz | 2 Mbps | FHSS/DSSS |
| **802.11b** | Wi-Fi 1 | 2,4 GHz | **11 Mbps** | QPSK-CCK |
| **802.11a** | Wi-Fi 2 | **5 GHz** | **54 Mbps** | OFDM |
| **802.11g** | Wi-Fi 3 | 2,4 GHz | **54 Mbps** | OFDM |
| **802.11n** | **Wi-Fi 4** | 2,4 y 5 GHz | **hasta 150 Mbps** | MIMO, 64-QAM |
| **802.11ac** | **Wi-Fi 5** | **5 GHz** | **hasta 3,5 Gbps** | 256-QAM, 4 streams |
| **802.11ax** | **Wi-Fi 6** | **2,4 y 5 GHz** | **hasta 9,6 Gbps** | OFDMA, **1024-QAM**, 8 streams |

> **Para el examen:** WiFi 6 (802.11ax) opera en **2,4 GHz Y 5 GHz** (NO solo en 5 GHz). Usa OFDMA y modulación 1024-QAM.

**Otros estándares 802.11 que conviene conocer:**
- **802.11e**: mejoras de QoS (prioridad de voz sobre datos).
- **802.11f**: roaming entre APs de diferentes fabricantes.
- **802.11i**: mejoras de seguridad (base de WPA2).
- **802.11r**: Fast Roaming (transición rápida entre APs).
- **802.11s**: Mesh Networking.
- **802.11ah** (Wi-Fi HaLow): baja potencia y mayor alcance para IoT.

#### Limitaciones tecnológicas WiFi

- **Alcance**: reducido por paredes e interferencias.
- **Ancho de banda variable**: 11 Mbps a 1 Mbps según condiciones.
- **Seguridad**: contraseñas y filtrado MAC.
- **Movilidad**: requiere reconexión manual al cambiar de AP.

#### Seguridad en redes WiFi

**Sistemas de cifrado (de mejor a peor):**
1. WPA2 + AES (recomendado)
2. WPA + AES
3. WPA + TKIP/AES
4. WPA + TKIP
5. WEP (obsoleto, inseguro)
6. Red abierta (sin seguridad)

| Protocolo | Descripción | Estado |
|-----------|-------------|--------|
| **WEP** | Clave estática compartida | Obsoleto, inseguro |
| **WPA** | TKIP (clave dinámica por paquete) | Vulnerable, superado |
| **WPA2** | AES, claves únicas por sesión; estándar desde 2006 | Recomendado |
| **WPA3** | Autenticación robusta, cifrado individualizado (2018) | Más seguro que WPA2 |
| **IPSec** | Cifrado a nivel de red; usado en VPNs | Muy seguro en corporativo |

**Sistemas de autenticación WiFi:**
- **Abierta**: cualquier dispositivo accede.
- **Clave compartida**: clave WEP fija.
- **Por dirección MAC**: lista de control de acceso; vulnerable a suplantación.

#### Configuración de un router WiFi

Parámetros configurables:
- Nombre de red (SSID), clave de seguridad.
- Filtrado MAC, tipo de protocolo de transporte (TCP/UDP).
- Rango DHCP, servidores DNS.
- Configuración IPv6.

### 9.4 Tecnología WiMAX (IEEE 802.16)

**WiMAX** (Worldwide Interoperability for Microwave Access): estándar inalámbrico de banda ancha metropolitana.

**Características:**
- Mayor alcance, ancho de banda y potencia que WiFi.
- Evolución de LMDS y MMDS.
- Frecuencias: 2,5 a 5,8 GHz (NLOS, sin línea de visión), hasta 70 km.
- Velocidad teórica: hasta 134 Mbps.
- Soporte de movilidad hasta 120 km/h (802.16e).
- Modulaciones: BPSK, QPSK, 16-QAM, 64-QAM.
- Multiplexación: OFDM/OFDMA.

**Capas del modelo 802.16:**

| Capa | Descripción |
|------|-------------|
| **CS** (Convergencia de Servicios) | Transforma datos en tramas para la capa MAC |
| **MAC CPS** | Acceso al sistema, gestión de ancho de banda, QoS |
| **Seguridad** | Autenticación, cifrado (AES), intercambio de claves |
| **PHY** (Física) | OFDM, NLOS, frecuencias 2,5-5,8 GHz |

**Estándares WiMAX:**

| Estándar | Descripción |
|---------|-------------|
| 802.16 | Banda 10-66 GHz (LOS) |
| 802.16a | Banda 2-11 GHz (NLOS) |
| 802.16e | WiMAX móvil (hasta 120 km/h) |
| 802.16f | Mesh WiMAX y roaming |

**Clases de servicio WiMAX (QoS):**
- **Unsolicited Grant-Real Time**: voz y vídeo en tiempo real.
- **Real Time Polling**: vídeo no interactivo.
- **Variable BitRate-Non Real Time**: descarga de archivos grandes.
- **Variable Bit Rate-Best Effort**: navegación web.

**Autenticación WiMAX:**
- **RADIUS**: autenticación y autorización de usuarios.
- Certificados digitales X.509.
- Firmas digitales para autenticación de mensajes.
- Renovación periódica de claves.

### 9.5 Otros sistemas inalámbricos

| Tecnología | Estándar | Descripción |
|-----------|----------|-------------|
| **Bluetooth** | IEEE 802.15.1 | WPAN a 2,4 GHz, voz y datos a corto alcance |
| **ZigBee** | IEEE 802.15.4 | Sensores y domótica, baja potencia |
| **Home RF** | — | Interconexión inalámbrica doméstica a 2,4 GHz |
| **HiperLAN2** | ETSI | WLAN corto alcance, alta velocidad y QoS |
| **IrDA** | IrDA | Infrarrojos, corta distancia |
| **DECT** | ETSI | Telefonía inalámbrica digital (1.880-1.990 MHz) |
| **WLL** | — | Bucle local inalámbrico (parte de DECT) |
| **GPRS** | — | Datos sobre GSM (WAP, SMS, MMS) |
| **CDPD** | — | Datos digitales en terminales TDMA |

### 9.6 Evolución de la telefonía móvil

| Generación | Año | Tecnología | Velocidad | Características |
|-----------|-----|-----------|-----------|----------------|
| **1G** | 1979 | AMPS, TACS | — | Analógica, solo voz |
| **2G** | 1991 | **GSM** | 9,6 kbps | Digital, SMS, roaming |
| **2,5G** | — | **GPRS**, EDGE | 171 kbps | Datos mejorados |
| **3G** | 1998 | **UMTS** | 384 kbps | Internet móvil |
| **3,5G** | — | HSPA | 14 Mbps | Mayor velocidad |
| **4G** | 2008 | **LTE** | 100 Mbps | Todo IP |
| **4G LTE** | 2009 | LTE Advanced | **1 Gbps** | MIMO, mayor velocidad |
| **5G** | 2019+ | NR | **hasta 10 Gbps** | IoT, baja latencia, densidad |

#### Tecnología 5G

**Objetivos:**
- Velocidad: hasta **10 Gbps** (10-100 veces más rápido que 4G).
- Latencia: **1 milisegundo** (4G: ~50 ms en condiciones reales).
- Conexiones: 100 veces más dispositivos por área que 4G.
- Disponibilidad: 99,999% del tiempo.
- Consumo de energía: reducción del 90%.
- IoT: hasta 1 millón de dispositivos por km².
- Frecuencia en España: banda 42 (3,5 GHz), asignada en subasta de 2021.

**Aplicaciones 5G:**
- Vehículos autónomos (reaccionan 250 veces más rápido que personas).
- Telecirugía y realidad virtual.
- IoT a escala masiva.

---

## Resumen para el Examen

### Datos clave de velocidades y estándares

| Tecnología/Norma | Velocidad | Nota clave |
|-----------------|-----------|-----------|
| RTB (módem V.92) | 56 kbps bajada, 48 kbps subida | Par de cobre, bucle abonado |
| RDSI BRI | 144 kbps (2B+D) | 2 canales B de 64 kbps + 1D de 16 kbps |
| RDSI PRI | 1.984 kbps (30B+D) | 30 canales B + 1D de 64 kbps |
| ADSL | Hasta 9 Mbps bajada | Asimétrico, bucle de abonado |
| ADSL2+ | 20 Mbps bajada / 2 Mbps subida | Distancia < 1,5 km |
| VDSL | 50 Mbps | Distancia < 500 m |
| Cat 5e | 1.000 Mbps / 100 MHz | Gigabit Ethernet |
| Cat 6A | 10.000 Mbps / 500 MHz | 10GBASE-T hasta 100 m |
| 802.3an | 10GBASE-T | Cat 6a, hasta 100 m |
| WiFi 4 (802.11n) | 150 Mbps | MIMO, 2,4 y 5 GHz |
| WiFi 5 (802.11ac) | 3,5 Gbps | 5 GHz, 256-QAM |
| WiFi 6 (802.11ax) | 9,6 Gbps | 2,4 Y 5 GHz, OFDMA, 1024-QAM |
| 4G LTE Advanced | 1 Gbps | — |
| 5G | hasta 10 Gbps | Latencia 1 ms |
| ATM STM-1 | 155,52 Mbps | — |
| ATM STM-64 | 9.953,28 Mbps | — |
| Coaxial fino (10BASE2) | 10 Mbps | Hasta 185 m, BNC |
| Coaxial grueso (10BASE5) | 10 Mbps | Hasta 500 m |

### Conceptos críticos para no confundir

| Concepto | Correcto | Incorrecto/Trampa |
|---------|---------|------------------|
| **Tipos de par trenzado válidos** | UTP, FTP, STP, SFTP | SBTP, BTP (no existen) |
| **WiFi 6 frecuencia** | 2,4 GHz Y 5 GHz | Solo 5 GHz (falso) |
| **Fibra multimodo escalonada** | El haz rebota en las paredes (reflexión interna total) | Confundir con fibra monomodo |
| **Fibra óptica estructura** | Núcleo (n₁) + revestimiento (n₂ < n₁) + cubierta exterior | Sin mencionar diferencia de índices |
| **MPLS FEC** | Grupo de paquetes con el mismo tratamiento de reenvío | No es el camino (eso es LSP) |
| **H.323** | Protocolo de videoconferencia sobre redes IP | No confundir con SIP |
| **RDSI BRI** | 2B+D → 144 kbps | No confundir con PRI (30B+D) |
| **ADSL vs ADSL2+** | ADSL hasta 9 Mbps; ADSL2+ hasta 20 Mbps bajada | Confundir velocidades o distancias |
| **Hub vs Switch** | Hub = dominio de colisión compartido; Switch = por puerto | Confundir los dominios de colisión y broadcast |
| **Router** | Separa dominios de broadcast | No separa dominios de colisión (eso lo hace el switch) |
| **Conmutación de circuitos** | Camino dedicado antes de transmitir; ineficiente si no hay datos | No confundir con conmutación de paquetes |
| **NRZ-L** | 1 = nivel alto, 0 = nivel bajo | No confundir con AMI (usa tres niveles: +V, 0, -V) |

---

## Referencias

- **PDF del tema**: Bloque 4 Tema 6 (2023) — Adrián Gil Gamboa / Flou Oposiciones. Contenido completo de comunicaciones, medios de transmisión, redes de conmutación y difusión, comunicaciones móviles e inalámbricas.
- **IEEE 802.11**: Estándar para redes WiFi — [https://www.ieee.org](https://www.ieee.org)
- **ITU-T**: Recomendaciones para telecomunicaciones — [https://www.itu.int](https://www.itu.int)
- **IETF RFC**: Protocolos de Internet — [https://www.ietf.org](https://www.ietf.org)
- **TIA-568**: Cableado estructurado — Telecommunications Industry Association
- **ISO/IEC 11801**: Cableado genérico para edificios de clientes
- **Wi-Fi Alliance**: Certificación y estándares WiFi — [https://www.wi-fi.org](https://www.wi-fi.org)
- **CNAF**: Cuadro Nacional de Atribución de Frecuencias — Ministerio de Asuntos Económicos y Transformación Digital (España)
