# Tema 5: Conceptos de Seguridad de los Sistemas de Información

> Este tema cubre los conceptos fundamentales de seguridad de los sistemas de información: principios CIA, tipos de amenazas y vulnerabilidades, el Plan de Seguridad Informática (PSI), la administración de la seguridad (ENS, SGSI, ciclo PDCA), seguridad física y lógica, técnicas criptográficas, protocolos seguros y la infraestructura de un CPD. En la oposición del Bloque IV es uno de los temas con mayor densidad normativa: 46 preguntas del banco incluyen legislación clave (ENS, LOPDGDD, Ley 36/2015) junto a conceptos técnicos de cifrado (simétrico, asimétrico, firma digital, IPsec, TLS). El estudiante debe dominar tanto la parte técnica como los detalles normativos con fechas, siglas y versiones exactas.

---

## 1. Conceptos de seguridad de los SI

### 1.1 ¿Por qué es importante la seguridad informática?

La seguridad de los sistemas de información (SI) persigue proteger la información y los recursos que la gestionan frente a accesos no autorizados, alteraciones, destrucción o uso indebido. Resulta crítica porque:

- Los sistemas de información son el núcleo de las organizaciones modernas.
- Los ataques provocan pérdidas económicas, de reputación y de confianza ciudadana.
- La normativa —especialmente en la Administración Pública— impone obligaciones legales.

### 1.2 Principios básicos: el modelo CITAD

El **Esquema Nacional de Seguridad (ENS)**, aprobado por el **Real Decreto 311/2022** (que derogó el RD 3/2010), define cinco **dimensiones de seguridad** que conforman el acrónimo **CITAD**:

| Dimensión | Código ENS | Descripción |
|---|---|---|
| **Confidencialidad** | [C] | Solo acceden a la información quienes están autorizados |
| **Integridad** | [I] | La información no ha sido alterada de forma no autorizada |
| **Trazabilidad** | [T] | Se puede seguir el rastro de quién hizo qué y cuándo |
| **Autenticidad** | [A] | El origen y la identidad de la información están verificados |
| **Disponibilidad** | [D] | Los usuarios autorizados acceden cuando lo necesitan |

> **Ojo en el examen:** la dimensión "Información [I]" del ENS corresponde a **Integridad**, no a "Información" como concepto genérico. El examen ha preguntado explícitamente cuál de las opciones es **incorrecta** al definir las dimensiones.

### 1.3 Términos y definiciones clave

| Término | Definición |
|---|---|
| **Activo** | Cualquier recurso con valor para la organización (información, hardware, software, personas) |
| **Amenaza** | Evento potencial que puede causar daño a un activo |
| **Vulnerabilidad** | Debilidad en un activo que puede ser explotada por una amenaza |
| **Riesgo** | Probabilidad de que una amenaza explote una vulnerabilidad y cause un impacto |
| **Impacto** | Consecuencia negativa de la materialización de una amenaza |
| **Contramedida** | Acción o mecanismo que reduce el riesgo |

### 1.4 Análisis de riesgo

El **análisis de riesgo** es el proceso sistemático para identificar, evaluar y tratar los riesgos. Sus fases son:

1. Identificar activos y su valor.
2. Identificar amenazas y vulnerabilidades.
3. Calcular el riesgo (probabilidad × impacto).
4. Seleccionar contramedidas (aceptar, transferir, mitigar o eliminar el riesgo).

### 1.5 Bienes informáticos críticos

Son los activos cuya indisponibilidad o compromiso tendría un impacto grave:

- **Datos e información**: bases de datos, ficheros de configuración, copias de seguridad.
- **Software**: sistemas operativos, aplicaciones críticas.
- **Hardware**: servidores, equipos de red, dispositivos de almacenamiento.
- **Servicios**: red eléctrica, telecomunicaciones, servicios en la nube.
- **Personas**: administradores, usuarios clave.

### 1.6 Marco normativo

| Norma | Contenido |
|---|---|
| **Constitución Española, art. 18** | Derecho a la intimidad y al secreto de las comunicaciones |
| **Constitución Española, art. 105.b** | Acceso de los ciudadanos a los registros y archivos administrativos |
| **RGPD / GDPR** | Reglamento (UE) 2016/679 de protección de datos personales |
| **LOPDGDD (LO 3/2018)** | Ley Orgánica de Protección de Datos y Garantía de Derechos Digitales |
| **LOPD (LO 15/1999)** | Derogada por la LOPDGDD; regulaba el tratamiento de datos de carácter personal |
| **ENS — RD 311/2022** | Esquema Nacional de Seguridad; aplicable a las AAPP y a sus proveedores |
| **Ley 36/2015 de Seguridad Nacional** | Marco de la política de seguridad nacional; el ENS es uno de sus instrumentos |
| **LO 7/2021** | Protección de datos tratados por autoridades competentes en materia penal |

> **Nota examen:** El art. 87 de la LOPDGDD regula el derecho de los trabajadores a la intimidad frente al uso de dispositivos digitales en el ámbito laboral.

---

## 2. Amenazas y vulnerabilidades

### 2.1 Clasificación de amenazas (ISO 27001)

La norma **ISO 27001** clasifica las amenazas en tres grandes categorías:

| Categoría | Ejemplos |
|---|---|
| **Naturales** | Terremotos, inundaciones, rayos, tormentas |
| **Del entorno** | Cortes eléctricos, contaminación, fallos de suministro |
| **Humanas (accidentales)** | Errores de usuarios, borrado accidental, configuraciones incorrectas |
| **Humanas (deliberadas)** | Ataques internos, espionaje, sabotaje, fraude |

### 2.2 Vulnerabilidades (ISO 27001)

Las vulnerabilidades pueden clasificarse según su naturaleza:

- **Físicas**: instalaciones mal protegidas, falta de control de acceso físico.
- **Organizativas**: falta de políticas, procedimientos mal definidos.
- **Humanas**: falta de formación, ingeniería social.
- **Técnicas / lógicas**: software desactualizado, configuraciones erróneas, contraseñas débiles.

### 2.3 Malware: clasificación y tipos

#### 2.3.1 Virus informáticos

Un **virus** es un programa que se replica insertando copias de sí mismo en otros ficheros o programas. Se clasifica:

**Por destino de infección:**
- **Virus de archivos**: infectan ficheros ejecutables (.exe, .com).
- **Virus de FAT / sector de arranque**: atacan la tabla de asignación de archivos o el MBR.
- **Virus de script**: escritos en lenguajes de script (VBScript, JavaScript).
- **Virus de macro**: se insertan en macros de documentos Office.

**Por modo de ataque:**
- **Acción directa**: se ejecutan en el momento de la infección.
- **Residentes en memoria**: permanecen en RAM y actúan en segundo plano.
- **Polimórficos**: cambian su código para evadir la detección antivirus.

#### 2.3.2 Otros programas maliciosos (malware)

| Tipo | Descripción | Clave para el examen |
|---|---|---|
| **Gusano** (Worm) | Se propaga por la red sin necesitar un huésped; consume ancho de banda | No necesita fichero huésped |
| **Troyano** | Aparenta ser legítimo pero ejecuta acciones ocultas maliciosas | No se replica; actúa por engaño |
| **Adware** | Muestra publicidad no deseada; suele llegar empaquetado con software gratuito | Puede ser autónomo o acompañar a un troyano |
| **Dropper** | Instala otro malware en el sistema | Primer eslabón del ataque |
| **Keylogger** | Registra las pulsaciones del teclado para capturar credenciales | Captura contraseñas |
| **Rootkit** | Oculta su presencia y la de otros malware en el sistema | Muy difícil de detectar |
| **Spyware** | Recopila información del usuario sin su conocimiento | Viola la privacidad |
| **Rogue** | Simula ser un antivirus para engañar al usuario y cobrarle | Falsa alarma de virus |
| **Riskware** | Software legítimo que puede usarse con fines maliciosos (p.ej. RDP) | Uso indebido de herramienta legítima |
| **Exploit** | Código que aprovecha una vulnerabilidad conocida (CVE) | Aprovecha un fallo concreto |
| **APT** (Advanced Persistent Threat) | Ataque sofisticado y prolongado, generalmente patrocinado por estados | Persistente y sigiloso |
| **Ransomware** | Cifra los datos y pide rescate para recuperarlos | Pago en criptomonedas |
| **Botnet** | Red de equipos infectados controlados remotamente (zombies) | C&C (Command and Control) |

> **Distinción examen:** Un **exploit** aprovecha una vulnerabilidad técnica (fallo de software). Un **rootkit** oculta la presencia del atacante. Un **spyware** roba información. Son categorías distintas aunque pueden combinarse.

#### 2.3.3 Procedimientos de engaño (ingeniería social)

| Técnica | Descripción |
|---|---|
| **Hoax** | Bulo o alarma falsa distribuida por correo/redes; no es malware técnico |
| **Phishing** | Correo fraudulento que imita a una entidad legítima para robar credenciales |
| **Pharming** | Redirige el DNS para llevar al usuario a un sitio falso aunque escriba la URL correcta |
| **Smishing** | Phishing por SMS |
| **Vishing** | Phishing por llamada de voz |
| **Spoofing** | Suplantación de identidad (IP, correo, web, DNS) |
| **Scam** | Estafa económica online |

> **Ojo:** El **Man-in-the-Middle** (MITM) NO es una técnica de ingeniería social; es un ataque técnico de intercepción de comunicaciones.

### 2.4 Ataques remotos

| Ataque | Descripción |
|---|---|
| **Inyección de código** (SQL, XSS) | Inserción de código malicioso en aplicaciones para ejecutarse en el servidor |
| **Escaneo de puertos** | Reconocimiento de servicios activos en un sistema |
| **DoS / DDoS** | Denegación de servicio: saturar un sistema con tráfico para dejarlo inaccesible |
| **Escuchas de red** (Sniffing) | Captura del tráfico de red para obtener información sensible |
| **Fuerza bruta** | Prueba sistemática de combinaciones de credenciales |
| **Elevación de privilegios** | El atacante aumenta sus permisos más allá de los asignados |
| **Envenenamiento DNS** | Corrompe la caché DNS para redirigir tráfico |
| **Desincronización TCP** | Rompe la conexión TCP entre dos hosts |
| **Relay SMB** | Retransmisión de credenciales NTLM para autenticarse en otro sistema |
| **Ataque Smurf (ICMP)** | DoS amplificado mediante broadcast de paquetes ICMP |

---

## 3. El Plan de Seguridad Informática (PSI)

### 3.1 ¿Qué es el PSI?

El **Plan de Seguridad Informática (PSI)** es el documento maestro que recoge las políticas, procedimientos y medidas que una organización adopta para proteger sus sistemas de información. Piensa en él como la "constitución" de la seguridad en una organización: todo lo demás se deriva de ahí. Es un requisito del ENS para todas las Administraciones Públicas.

### 3.2 Condicionantes del Sistema de Seguridad de la Información (SSI)

El SSI debe garantizar las siguientes propiedades:

- **Confidencialidad**: solo acceden los autorizados.
- **Integridad**: la información no se modifica sin autorización.
- **Mínimo privilegio**: se asignan los permisos estrictamente necesarios y nada más.
- **Consistencia**: el sistema es coherente en todos sus niveles.
- **No repudio (imposibilidad de rechazo)**: las acciones realizadas no pueden ser negadas a posteriori.
- **Disponibilidad**: los recursos están accesibles cuando se necesitan.
- **Autenticidad**: los usuarios y la información son quienes dicen ser.

### 3.3 Estructura del PSI

El PSI se organiza en los siguientes apartados:

1. **Alcance**: qué sistemas y recursos cubre el plan.
2. **Políticas de seguridad**: directrices generales de alto nivel.
3. **Caracterización del SI**: descripción de los sistemas e infraestructuras.
4. **Estructura funcional**: organización de responsabilidades de seguridad.
5. **Clasificación y control de bienes**: inventario y valoración de activos.
6. **Control de cambios**: proceso de gestión de cambios en el SI.
7. **Análisis de riesgos**: identificación y evaluación de riesgos.
8. **Procedimientos y medidas de seguridad**: controles técnicos y organizativos.
9. **Gestión de incidencias**: cómo detectar, responder y registrar incidentes.
10. **Anexos**: documentación complementaria.

### 3.4 Categorías del ENS

El ENS clasifica los sistemas en tres **categorías** según el impacto potencial de un incidente de seguridad:

| Categoría | Impacto | Descripción |
|---|---|---|
| **BÁSICA** | Bajo | Daño limitado; puede causar perjuicios menores |
| **MEDIA** | Significativo | Daño considerable a intereses individuales u organizativos |
| **ALTA** | Grave o muy grave | Daño grave; puede afectar a la seguridad nacional o a derechos fundamentales |

> **Dato examen:** Un sistema es de categoría **MEDIA** cuando un incidente de seguridad puede causar un **daño significativo**.

### 3.5 Niveles de seguridad del ENS

Cada dimensión (C, I, T, A, D) se evalúa en tres **niveles**:

- **BAJO**: medidas básicas de seguridad.
- **MEDIO**: medidas reforzadas.
- **ALTO**: medidas más estrictas; mayor control y auditoría.

La **categoría del sistema** viene determinada por la dimensión con el nivel más alto.

### 3.6 Medidas de seguridad del ENS

Las medidas del ENS se agrupan en **tres marcos**:

| Marco | Código | Descripción |
|---|---|---|
| **Marco Organizativo** | org | Política de seguridad, normativa, procedimientos, plan de adecuación |
| **Marco Operacional** | op | Planificación, control de acceso, explotación, servicios externos, continuidad |
| **Medidas de Protección** | mp | Instalaciones, personal, equipamiento, comunicaciones, soportes, aplicaciones |

---

## 4. Administración de la seguridad de sistemas de información

### 4.1 Marco normativo: Orden PRE/2740/2007

La **Orden PRE/2740/2007** regula las funciones del **administrador de seguridad** en la Administración General del Estado. Sus cometidos principales son:

- Establecer y mantener las medidas de seguridad definidas en el PSI.
- Controlar el acceso a los sistemas y gestionar altas, bajas y modificaciones de usuarios.
- Supervisar el correcto funcionamiento de los controles de seguridad.
- Elaborar informes periódicos sobre el estado de la seguridad.
- Gestionar y registrar los incidentes de seguridad.
- Impulsar la formación y concienciación en seguridad.

### 4.2 El ciclo PDCA y el SGSI

La gestión de la seguridad no es algo que se hace una vez y se olvida: es un proceso continuo. Por eso sigue el **ciclo de mejora continua PDCA** (también llamado ciclo de Deming):

```
Planificar (Plan) → Hacer (Do) → Verificar (Check) → Actuar (Act)
       ↑___________________________________________________|
```

| Fase | Actividades |
|---|---|
| **Planificar** | Definir alcance, política, análisis de riesgos, plan de tratamiento |
| **Hacer** | Implantar controles, formar al personal, gestionar operaciones |
| **Verificar** | Auditar, revisar indicadores, evaluar eficacia de controles |
| **Actuar** | Mejorar controles, actualizar el PSI, aplicar lecciones aprendidas |

El **Sistema de Gestión de la Seguridad de la Información (SGSI)** según la norma **ISO 27001** se estructura en 9 fases que siguen este ciclo.

### 4.3 Tipos de medidas de seguridad

| Tipo | Descripción |
|---|---|
| **Administrativas** | Políticas, normas, procedimientos, contratos |
| **Legales** | Cumplimiento normativo (ENS, LOPDGDD, etc.) |
| **Educativas** | Formación y concienciación del personal |
| **Operacionales** | Procedimientos del día a día (copias de seguridad, gestión de parches) |
| **De recuperación** | Planes de continuidad y de recuperación ante desastres |
| **Físicas** | Control de acceso físico, vigilancia, climatización |
| **Técnicas / Lógicas** | Cortafuegos, cifrado, antivirus, control de acceso lógico |

### 4.4 Auditorías de seguridad en el ENS

El ENS exige **auditorías regulares** para verificar el cumplimiento:

- Los sistemas de **categoría MEDIA y ALTA** deben auditarse **al menos cada dos años**.
- Las auditorías pueden ser internas o externas.
- Los resultados se elevan al responsable del sistema y al responsable de seguridad.

---

## 5. Seguridad física

### 5.1 Amenazas principales a la seguridad física

- Desastres naturales: terremotos, inundaciones, rayos, incendios.
- Fallos de suministro eléctrico: cortes, sobretensiones, picos, pulsos electromagnéticos (EMP).
- Acceso físico no autorizado: robo de equipos, sabotaje, vandalismo.
- Factores ambientales: temperatura, humedad, polvo.

### 5.2 Acondicionamiento de locales

Los recintos que albergan equipos TI deben cumplir requisitos como:

- Acceso restringido: solo personal autorizado.
- Control de temperatura y humedad mediante sistemas de aire acondicionado dedicados.
- Suelo elevado (falso suelo) para el paso de cableado y refrigeración.
- Protección contra incendios: detectores de humo, extintores de CO₂ o gas inerte (no agua).
- Protección contra inundaciones: sensores de agua, impermeabilización.

### 5.3 Energía eléctrica

| Medida | Descripción |
|---|---|
| **SAI / UPS** (Sistema de Alimentación Ininterrumpida) | Proporciona energía de batería durante cortes de suministro para un apagado controlado |
| **Generador diésel** | Proporciona energía durante cortes prolongados |
| **PIA** (Pequeño Interruptor Automático) | Protege los circuitos contra sobrecargas y sobreintensidades de corriente |
| **Diferencial** | Protege contra corrientes de fuga a tierra (electrocución) |
| **Filtros de línea / SAI online** | Protegen contra sobretensiones y picos de corriente |

> El SAI es clave para garantizar la **disponibilidad** de los sistemas durante cortes de luz.

### 5.4 Protección contra incendios

- Detectores de humo y temperatura.
- Sistemas de extinción automática: gas inerte (argón, FM-200), CO₂ o Halon (obsoleto).
- **No usar agua** en salas de informática.
- Compartimentación de zonas (puertas cortafuego).
- Planes de evacuación y simulacros.

### 5.5 Protección de soportes de información

#### Etiquetado
Todos los soportes (discos, cintas, pendrives) deben etiquetarse indicando:
- Clasificación de la información que contienen.
- Fecha de creación y de caducidad.
- Responsable.

#### Transporte
- Los soportes con información sensible deben transportarse con medidas de protección apropiadas (cifrado, custodia, seguimiento).

#### Borrado y destrucción segura
- **Borrado seguro**: sobreescritura múltiple de datos (métodos DoD 5220.22-M, Gutmann).
- **Desmagnetización** (degaussing): para cintas magnéticas.
- **Destrucción física**: triturado, fundido o incineración, para garantizar la irrecuperabilidad.

#### Registro de entrada/salida de equipamiento
- Todo movimiento de equipos debe registrarse: quién, qué, cuándo y hacia dónde.

---

## 6. Seguridad lógica

### 6.1 Control de acceso y autorizaciones

El acceso a los sistemas se controla mediante tres mecanismos:

1. **Identificación**: el usuario declara su identidad (nombre de usuario, DNI...).
2. **Autenticación**: el sistema verifica esa identidad.
3. **Autorización**: el sistema determina qué puede hacer el usuario autenticado.

#### Factores de autenticación

| Factor | Tipo | Ejemplos |
|---|---|---|
| **Algo que sabes** | Conocimiento | Contraseña, PIN, pregunta secreta |
| **Algo que tienes** | Posesión | Tarjeta, token físico (OTP), DNIe |
| **Algo que eres** | Inherencia | Huella dactilar, iris, reconocimiento facial |

La **autenticación multifactor (MFA)** combina dos o más factores distintos y es el pilar del modelo **Zero Trust**.

#### Buenas prácticas de contraseñas

- Longitud mínima de 12 caracteres.
- Combinación de letras (mayúsculas y minúsculas), números y símbolos.
- No reutilizar contraseñas anteriores.
- Cambio periódico (aunque las tendencias modernas priorizan la longitud y el gestor de contraseñas).
- No compartir contraseñas.

#### Token físico / OTP

Los **tokens físicos** generan contraseñas de un solo uso (**OTP**, One-Time Password) válidas por un tiempo limitado (TOTP) o para un solo uso (HOTP). Son el segundo factor más común en la Administración.

### 6.2 Modelos de control de acceso

| Modelo | Nombre | Descripción |
|---|---|---|
| **DAC** | Discrecional (Discretionary Access Control) | El propietario del recurso decide quién accede |
| **MAC** | Obligatorio (Mandatory Access Control) | El sistema impone el control según etiquetas de clasificación |
| **RBAC** | Basado en roles (Role-Based Access Control) | Los permisos se asignan a roles y los usuarios heredan los del rol |
| **ABAC** | Basado en atributos (Attribute-Based Access Control) | Los permisos dependen de atributos del usuario, del recurso y del contexto |

> **Dato examen:** El **ABAC** es el modelo más flexible: permite definir políticas basadas en atributos como la hora del día, la ubicación, el departamento o el nivel de clasificación del documento.

### 6.3 Clasificación de la información

La información se clasifica según su sensibilidad para aplicar controles proporcionales:

| Nivel | Descripción |
|---|---|
| **Pública** | Sin restricciones de acceso |
| **Interna** | Para uso exclusivo de la organización |
| **Confidencial** | Acceso restringido a personal autorizado |
| **Secreta / Reservada** | Máxima protección; su divulgación causaría daño grave |

### 6.4 Copias de seguridad (Backup)

Dos métricas clave que salen mucho en el examen:

| Concepto | Definición |
|---|---|
| **RPO** (Recovery Point Objective) | Cuánto tiempo atrás podemos recuperar datos; máxima pérdida de datos tolerable |
| **RTO** (Recovery Time Objective) | Cuánto tiempo podemos tardar en restaurar el servicio |
| **Backup completo** | Copia de todos los datos; ocupa más espacio pero la restauración es más rápida |
| **Backup incremental** | Solo copia los cambios desde la última copia (completa o incremental); es la más rápida de hacer pero la más lenta de restaurar |
| **Backup diferencial** | Solo copia los cambios desde la última copia completa; equilibrio entre velocidad de backup y de restauración |

La regla **3-2-1**: 3 copias, en 2 medios distintos, 1 fuera de las instalaciones (offsite). Es el mínimo razonable para cualquier organización.

### 6.5 Protección de aplicaciones informáticas

#### Principios NIST para el desarrollo seguro

- Mínimo privilegio.
- Defensa en profundidad.
- Fallar de forma segura (fail-safe defaults).
- Economía de mecanismos (simplicidad).
- Separación de privilegios.

#### Pruebas de seguridad

- **Análisis estático de código** (SAST): sin ejecutar el programa.
- **Análisis dinámico** (DAST): analizando la aplicación en ejecución.
- **Pruebas de penetración** (pentest): simulación de un ataque real.

#### WAF (Web Application Firewall)

Dispositivo o software que filtra el tráfico HTTP/S hacia aplicaciones web, protegiéndolas de ataques como SQL Injection, XSS, etc.

#### OWASP Top 10

La **OWASP** (Open Web Application Security Project) publica periódicamente el **Top 10** de vulnerabilidades más críticas en aplicaciones web:

1. Control de acceso defectuoso
2. Fallos criptográficos
3. Inyección (SQL, NoSQL, OS, LDAP)
4. Diseño inseguro
5. Configuración de seguridad incorrecta
6. Componentes vulnerables y desactualizados
7. Fallos de identificación y autenticación
8. Fallos de integridad del software y de los datos
9. Fallos en el registro y monitoreo de seguridad
10. Falsificación de solicitudes del lado del servidor (SSRF)

### 6.6 Prevención de fugas de datos (DLP)

**DLP (Data Loss Prevention)** son las soluciones que detectan y previenen el movimiento no autorizado de información sensible. Supervisan:

- **Datos en reposo**: ficheros almacenados en servidores, equipos o nube.
- **Datos en uso**: accesos y modificaciones en tiempo real.
- **Datos en tránsito**: tráfico de red, correo electrónico, transferencias.

### 6.7 Zero Trust

El modelo **Zero Trust** ("nunca confiar, siempre verificar") asume que ningún usuario ni dispositivo es de confianza por defecto, incluso dentro de la red interna. Sus principios son:

- **MFA obligatorio** para todos los accesos, independientemente de la red.
- **Mínimo privilegio**: solo los permisos estrictamente necesarios.
- **Microsegmentación**: dividir la red en zonas pequeñas para limitar el movimiento lateral.
- **Monitoreo continuo**: registrar y analizar toda la actividad.

> **Dato examen:** Zero Trust exige MFA **incluso cuando el usuario está dentro de la red corporativa**.

### 6.8 Herramientas de seguridad

| Herramienta | Descripción |
|---|---|
| **Antivirus / EDR** | Detecta y elimina malware; los EDR añaden capacidades de respuesta y forense |
| **Firewall / NGFW** | Filtra el tráfico de red según reglas; los NGFW inspeccionan el contenido |
| **IDS / IPS** | Detecta (IDS) o bloquea (IPS) intrusiones en la red |
| **SIEM** | Centraliza logs y genera alertas de seguridad (Security Information and Event Management) |
| **WAF** | Protege aplicaciones web |
| **DLP** | Previene fugas de datos |
| **Proxy** | Intermediario entre el usuario e Internet; permite filtrado de contenidos |
| **Gestor de contraseñas** | Almacena credenciales de forma segura |

---

## 7. Técnicas criptográficas y protocolos seguros

### 7.1 ¿Por qué cifrar?

El cifrado protege la información garantizando:

- **Confidencialidad**: solo el destinatario puede leer el mensaje.
- **Integridad**: cualquier alteración es detectable.
- **Autenticidad**: el origen del mensaje es verificable.
- **No repudio**: el emisor no puede negar haber enviado el mensaje.

### 7.2 Información que debe cifrarse

- Datos personales y sensibles (LOPDGDD, ENS).
- Comunicaciones entre sistemas (APIs, correo, acceso remoto).
- Soportes extraíbles (pendrives, discos externos).
- Información almacenada en la nube.
- Contraseñas (con funciones hash, no cifrado reversible).

### 7.3 Herramientas criptográficas utilizadas en la Administración

| Herramienta | Uso |
|---|---|
| **VPN** | Cifrado de comunicaciones entre redes |
| **SSL/TLS** | Cifrado de comunicaciones web (HTTPS) |
| **WPA2/WPA3** | Cifrado de redes inalámbricas |
| **SSH** | Acceso remoto seguro a sistemas |
| **IPsec** | Cifrado a nivel de red |
| **SFTP** | Transferencia segura de ficheros |
| **PGP/GPG** | Cifrado y firma de correo electrónico |
| **S/MIME** | Estándar para correo seguro con certificados X.509 |

### 7.4 Configuración de privacidad

- Usar protocolos seguros y versiones actualizadas (TLS 1.2+, no SSL 2.0/3.0).
- Deshabilitar protocolos y algoritmos obsoletos.
- Gestionar correctamente los certificados digitales.
- Renovar y revocar certificados cuando sea necesario.

---

## 8. Cifrado de datos y firma digital

### 8.1 Fundamentos de la criptografía

La **criptografía** transforma información legible (**texto en claro** o **plaintext**) en información ilegible (**texto cifrado** o **ciphertext**) mediante un **algoritmo** y una **clave**.

**Tipos de ataques al cifrado:**
- **Criptoanálisis**: se basa en el conocimiento del algoritmo y el análisis matemático.
- **Fuerza bruta**: prueba todas las combinaciones posibles de clave hasta dar con la correcta.

### 8.2 Cifrado de datos según su estado

| Estado | Técnica | Ejemplo |
|---|---|---|
| **En movimiento** (tránsito) | Cifrado de canal | TLS, VPN, SSH |
| **En reposo** (almacenado) | FDE, EFS, HSM | BitLocker (FDE), EFS de Windows, HSM para claves |
| **En uso** (procesando) | Tecnologías avanzadas | Cifrado homomórfico, HSM distribuido, protección contra raspado de RAM |

- **FDE (Full Disk Encryption)**: cifra todo el disco duro (p.ej., BitLocker, VeraCrypt).
- **EFS (Encrypting File System)**: cifrado a nivel de fichero integrado en NTFS de Windows.
- **HSM (Hardware Security Module)**: dispositivo físico que almacena y gestiona claves criptográficas de forma segura.
- **Cifrado homomórfico**: permite operar sobre datos cifrados sin descifrarlos (tecnología emergente).

### 8.3 Algoritmos simétricos (clave única)

La idea es simple: emisor y receptor comparten la misma clave. El reto está en cómo intercambiar esa clave de forma segura.

**Características:**
- Utilizan una **única clave compartida** para cifrar y descifrar.
- Más **rápidos** que los asimétricos.
- Principal uso: **datos en reposo** y cifrado de grandes volúmenes.
- **Problema principal**: distribución segura de la clave compartida.
- La información es indescifrable incluso conociendo el algoritmo (seguridad del algoritmo + secreto de la clave).

**Procedimiento:**
```
Emisor: Mensaje en claro → [cifrado con clave compartida] → Criptograma
                                                                    ↓ (red)
Receptor: Criptograma → [descifrado con la misma clave] → Mensaje en claro
```

| Algoritmo | Bits de bloque | Año | Estado |
|---|---|---|---|
| **DES** | 64 | 1977 | Obsoleto (roto en 1998 con Deep Crack); algoritmo Feistel |
| **3DES** | 64 | 1999 | En desuso (sustituido por AES); algoritmo Feistel |
| **AES** | 128 | 2001 | Estándar actual; algoritmo Rijndael; vida útil estimada 20-30 años |
| **IDEA** | 64 | 1991 | Usado en PGP |
| **RC4, RC5, Blowfish, Twofish, CAST, SAFER** | Variable | — | Alternativas; ChaCha20 es moderno y rápido |

**Ataques al cifrado simétrico:**
- **Criptoanálisis**: basado en el algoritmo y texto conocido.
- **Fuerza bruta**: prueba todas las combinaciones posibles.

### 8.4 Algoritmos asimétricos (clave doble)

Aquí la magia está en que no hace falta compartir ningún secreto de antemano: cada persona tiene una clave pública (que todo el mundo puede ver) y una clave privada (que nunca sale de su poder).

**Características:**
- Cada usuario tiene un **par de claves matemáticamente relacionadas**:
  - **Clave privada**: secreta; nunca se comparte.
  - **Clave pública**: se distribuye libremente.
- Lo que cifra una clave, solo lo descifra la otra.
- Más **lentos** que los simétricos.
- Principal uso: **datos en movimiento**, firma digital y distribución de claves.
- Requieren **Autoridades de Certificación (CA)** para validar la autenticidad de las claves públicas.

**Dos usos principales:**

**A) Cifrado para confidencialidad:**
```
Ana → cifra con la clave PÚBLICA de David → David descifra con su clave PRIVADA
→ Solo David puede leer el mensaje (CONFIDENCIALIDAD)
```

**B) Firma digital para autenticidad:**
```
David → cifra/firma con su clave PRIVADA → Ana descifra con la clave PÚBLICA de David
→ Solo David pudo haber firmado (AUTENTICIDAD / NO REPUDIO)
```

**Algoritmos asimétricos principales:**

| Algoritmo | Características | Uso |
|---|---|---|
| **RSA** | Basado en factorización de números primos grandes; claves de 512-4096 bits | SSH, TLS, firma digital |
| **ECC** (Elliptic Curve) | Seguridad equivalente con claves más cortas | TLS, dispositivos móviles |
| **DSA / ECDSA** | Específico para firma digital | Firma de código, certificados |
| **DH / ECDH** | Intercambio de claves (no cifra directamente) | Establecimiento de sesión TLS |

### 8.5 Funciones hash (resumen)

Una **función hash** transforma un mensaje de longitud arbitraria en una cadena de longitud fija (el **hash** o **digest**). Características:

- **Unidireccional**: no se puede obtener el mensaje original a partir del hash.
- **Determinista**: el mismo mensaje siempre produce el mismo hash.
- **Resistente a colisiones**: dos mensajes distintos no deben producir el mismo hash.

| Algoritmo | Longitud del hash | Estado |
|---|---|---|
| **MD4** | 128 bits | Obsoleto |
| **MD5** | 128 bits | Comprometido (colisiones conocidas); no usar para seguridad |
| **SHA-1** | 160 bits | Obsoleto en certificados (desde 2017) |
| **SHA-256** | 256 bits | Estándar actual; parte de la familia SHA-2 |
| **SHA-3** | Variable | Basado en Keccak; más moderno |
| **RIPEMD-160** | 160 bits | Alternativa a SHA-1 |

> **Dato examen:** **MD5** genera un hash de **128 bits**. SHA-1 genera 160 bits. SHA-256 genera 256 bits.

### 8.6 Firma digital

La firma digital proporciona **autenticidad**, **integridad** y **no repudio** de los documentos electrónicos.

**Garantías de la firma digital:**
- Verificación por una organización homologada y confiable.
- Origen verificado del documento.
- Integridad del mensaje (cualquier cambio invalida la firma).
- Protección contra manipulación.
- Verificación universal (cualquiera puede verificarla con la clave pública).

**Proceso de firma digital:**

```
FIRMA:
1. Mensaje → [función hash] → Resumen (hash)
2. Resumen → [cifrado con clave PRIVADA del emisor] → Firma digital
3. Se envía: Mensaje + Firma digital

VERIFICACIÓN:
4. Receptor: Firma digital → [descifrado con clave PÚBLICA del emisor] → Hash recibido
5. Receptor: Mensaje recibido → [función hash] → Hash calculado
6. Si Hash recibido == Hash calculado → FIRMA VÁLIDA
```

> **Dato examen:** La firma digital usa la **clave privada del emisor** para firmar. El receptor verifica con la **clave pública del emisor**.

#### Tipos de firma electrónica (Reglamento eIDAS)

| Tipo | Requisitos | Validez jurídica |
|---|---|---|
| **Firma electrónica simple** | Datos electrónicos vinculados al firmante | Limitada |
| **Firma electrónica avanzada** | Vinculada al firmante, permite detectar cambios, usa datos exclusivos del firmante | Mayor valor probatorio |
| **Firma electrónica cualificada** | Avanzada + creada con dispositivo cualificado + certificado cualificado de AC | Equivalente a la firma manuscrita |

> **Dato examen:** Solo la **firma electrónica cualificada** tiene la misma validez jurídica que la firma manuscrita según el Reglamento eIDAS.

### 8.7 Infraestructura de Clave Pública (PKI)

Una **PKI (Public Key Infrastructure)** es el conjunto de protocolos, servicios y estándares para crear, gestionar, distribuir y revocar certificados digitales. Se basa en el modelo de **Terceras Partes Confiables**.

**Entidades de una PKI:**

| Entidad | Función |
|---|---|
| **Autoridad de Certificación (CA)** | Emite y gestiona certificados digitales |
| **Autoridad de Registro (RA)** | Verifica la identidad del solicitante antes de emitir el certificado |
| **Autoridad de Validación (VA)** | Verifica si un certificado está vigente o revocado |
| **CRL (Certificate Revocation List)** | Lista de certificados revocados antes de su fecha de caducidad |
| **OCSP** | Protocolo para comprobar el estado de un certificado en tiempo real |

**CAs en España:**
- **FNMT** (Fábrica Nacional de Moneda y Timbre): emite certificados para ciudadanos y funcionarios.
- **DGP** (Dirección General de Policía): actúa como CA del **DNI electrónico (DNIe)**.
- **Agencia de Certificación Electrónica** y **Verisign** (internacional).

### 8.8 El DNI electrónico (DNIe)

El **DNI electrónico** incorpora un chip con:
- Certificado de identidad (permite identificarse electrónicamente).
- Certificado de firma (permite firmar documentos digitalmente).
- La **clave privada** del titular, que nunca abandona el chip.

La **Dirección General de la Policía (DGP)** actúa como **Autoridad de Certificación** del DNIe.

### 8.9 Formatos de firma digital

| Formato | Base | Características |
|---|---|---|
| **CAdES** (CMS Avanzado) | Binario | Para archivos grandes; la información se guarda en binario; no se puede leer después de firmar |
| **XAdES** (XML Avanzado) | XML | Produce un archivo XML con la información de la firma; más grande que CAdES |
| **PAdES** (PDF Avanzado) | PDF | Para documentos PDF; el receptor puede verificar la firma sin herramientas externas |
| **OOXML** | Office Open XML | Formato de Microsoft Office |
| **ODF** | OpenDocument | Formato de OpenOffice/LibreOffice |

La aplicación **AutoFirma** (del Ministerio) permite configurar el formato y firma documentos con el DNIe o certificado software.

---

## 9. Protocolos seguros

### 9.1 ¿Qué es un protocolo de seguridad?

Un **protocolo de seguridad** es un conjunto de reglas que especifica cómo se comunican y protegen los datos entre sistemas informáticos. Sin estos protocolos, cualquier comunicación por la red sería como enviar una postal: todo el mundo puede leerla. Proporciona servicios como: autenticación, confidencialidad, establecimiento de claves, e-voting y compartición de información clasificada.

### 9.2 Seguridad inalámbrica: WEP y WPA

| Protocolo | Estándar | Cifrado | Estado |
|---|---|---|---|
| **WEP** (Wired Equivalent Privacy) | 802.11 | RC4 con IV de 24 bits | Muy inseguro; obsoleto; susceptible a múltiples ataques |
| **WPA** (Wi-Fi Protected Access) | 802.11 | RC4 + TKIP (o AES) | Mejora WEP; implementa TKIP con IV de 48 bits |
| **WPA2** | 802.11i | AES (Rijndael) | Estándar actual seguro |
| **WPA3** | 802.11 | AES-GCMP + SAE | Más moderno; protege mejor frente a ataques de diccionario |

**WPA puede operar en dos modos:**
- **PSK (Pre-Shared Key)**: clave compartida (entornos domésticos).
- **Enterprise**: autenticación mediante servidor **RADIUS** que distribuye claves únicas a cada usuario.

### 9.3 IPsec

**IPsec** es un conjunto de protocolos que opera en la **capa de red** (capa 3 del modelo OSI) para autenticar y cifrar paquetes IP. Es transparente para las aplicaciones.

**Protocolos de IPsec:**

| Protocolo | Función | Confidencialidad |
|---|---|---|
| **AH** (Authentication Header) | Integridad y autenticación de datos e IP header | No (sin cifrado) |
| **ESP** (Encapsulating Security Payload) | Integridad, autenticación y confidencialidad | Sí |

**Modos de operación de ESP:**

| Modo | Descripción | Uso típico |
|---|---|---|
| **Transporte** | Cifra solo el payload; conserva el IP header original | Acceso remoto de administradores |
| **Túnel** | Cifra todo el paquete original y añade nuevo IP header | VPN entre sedes (gateway a gateway) |

**Características técnicas de IPsec:**
- Usa **ISAKMP/IKE** para establecer las **Asociaciones de Seguridad (SA)**.
- Emplea UDP puerto 500 para establecer la SA.
- Soporta IPv4 e IPv6.
- Puede implementarse en firewall/router sin necesidad de configurar los hosts internos.

**Servicios de IPsec:**
- Confidencialidad del mensaje.
- Protección del análisis de tráfico (modo túnel oculta IPs origen/destino).
- Integridad del mensaje mediante MAC (Message Authentication Code).

### 9.4 VPN (Red Privada Virtual)

Una **VPN** crea una red privada segura sobre una red pública (Internet). Establece **túneles cifrados** entre los extremos.

**Funcionamiento:**
1. El dispositivo se conecta al servidor VPN.
2. Todo el tráfico viaja cifrado hasta el servidor VPN.
3. El dispositivo actúa como si estuviera en la red local de la VPN.
4. Permite acceder a recursos internos desde cualquier lugar.

**En VPN se combina cifrado simétrico y asimétrico:**
- La **clave de sesión** (AES, simétrica) cifra los datos por eficiencia.
- La **clave pública** del servidor cifra la clave de sesión para transmitirla de forma segura.
- El servidor descifra la clave de sesión con su **clave privada**.

### 9.5 SSL y TLS

**SSL (Secure Sockets Layer)** y su sucesor **TLS (Transport Layer Security)** operan en la **capa de transporte** (sobre TCP) y protegen las comunicaciones web (HTTPS).

| Aspecto | SSL | TLS |
|---|---|---|
| Estado | Obsoleto (vulnerable a ataques como POODLE) | Estándar actual (TLS 1.2, TLS 1.3) |
| Puerto | 443 (HTTPS) | 443 (HTTPS) |
| RFC | — | RFC 2246 (TLS 1.0) |

**Protocolo Handshake TLS:**
1. Negociación de parámetros criptográficos.
2. Establecimiento de la clave de sesión.
3. Autenticación del servidor (y opcionalmente del cliente) mediante certificado X.509.

**Proceso SSL (establecimiento de conexión):**
```
Alice → [versión SSL, preferencias, nonce RA] → Bob
Alice ← [versión SSL, elecciones, nonce RB] ← Bob
Alice ← [cadena certificados X.509] ← Bob
Alice → [E_B(clave premaster)] → Bob  (cifrada con clave pública de Bob)
Alice → [Change cipher] → Bob
Alice ↔ [comunicación cifrada simétrica con clave de sesión derivada] ↔ Bob
```

### 9.6 PGP (Pretty Good Privacy)

**PGP** añade seguridad y autenticación al **correo electrónico** a nivel de aplicación. Combina cifrado simétrico y asimétrico, y utiliza una **red de confianza** (Web of Trust) en lugar de CAs centralizadas.

---

## 10. Infraestructura física de un CPD

### 10.1 ¿Qué es un CPD?

Un **Centro de Procesamiento de Datos (CPD)** es la instalación física que alberga los sistemas informáticos y de comunicaciones de una organización. Su seguridad física es crítica para garantizar la disponibilidad de los servicios.

### 10.2 Elección del emplazamiento

A la hora de elegir la ubicación de un CPD se deben considerar:

| Factor | Descripción |
|---|---|
| **Clima** | Afecta a la refrigeración y eficiencia energética |
| **Características geológicas** | Probabilidad de terremotos, tipo de suelo, proximidad a ríos o montañas |
| **Infraestructura disponible** | Redes eléctricas, telecomunicaciones, accesos por carretera |
| **Tipo de edificio** | Extenso pero con el menor número de plantas posible (facilita el cableado) |

### 10.3 Requisitos básicos del edificio

- **Seguridad y protección**: primera barrera de defensa; acceso exclusivo para personal autorizado.
- **Zona de carga y descarga**: área exterior designada.
- **Flexibilidad**: edificio inteligente (inmótica: automatización de instalaciones industriales y de oficinas) que permita expansión sin interrumpir operaciones.
- **Normativa**: cumplir con normativas habitacionales y de seguridad.
- **Control de luz y aire**: minimizar luz natural; control estricto de temperatura y humedad.
- **Medidas contra incendios**: extintores adecuados y al menos una ruta de evacuación.

### 10.4 Requisitos de las instalaciones del CPD

#### Instalación eléctrica

- **Cuadros de mando**: accesibles, etiquetados, con materiales antiestáticos.
- **SAI**: para energía de respaldo durante cortes.
- **Conexión a tierra**: para todas las masas metálicas; protege contra descargas.
- **Enchufes con conexión a masa**: evitan acumulación de cargas eléctricas.
- **Generadores / baterías**: para cortes prolongados.

#### Instalación de agua

- **Prevención de fugas**: evitar canalizaciones de agua dentro de la sala; sensores de fuga.
- **Cableado impermeable**: todo el cableado de suministro de agua debe ser impermeable.
- **Depósitos estancos**: si existen depósitos en el entorno, deben estar perfectamente sellados.

#### Instalación de aire y protección contra incendios

- **Disipación del calor**: sistemas de aire acondicionado para mantener temperatura y humedad adecuadas.
- **Mecanismo de corte automático** del aire acondicionado para prevenir incendios.
- **Materiales resistentes al fuego**.
- **Circuito eléctrico que se desactive automáticamente en caso de incendio**.
- **Alarmas, detectores de humo y pulsadores de pánico** estratégicamente situados.
- **Recintos de protección combinada**: áreas seguras frente a fuego e inundaciones.

### 10.5 Normativa aplicable a los CPD

| Normativa | Contenido |
|---|---|
| **Norma Básica de la Edificación** | Requisitos mínimos de seguridad, habitabilidad, accesibilidad y eficiencia energética |
| **Normas Tecnológicas de la Edificación** | Instalaciones de climatización, electricidad, redes de datos y seguridad física |
| **Ordenanzas Municipales** | Regulaciones locales de construcción y funcionamiento |
| **Reglamentos Electrotécnicos** | Disposiciones técnicas sobre instalaciones eléctricas; prevención de cortocircuitos e incendios |
| **Verificación de Productos y Suministros Industriales** | Certificación de materiales y equipos utilizados |

---

## 11. Sistemas de gestión de incidencias

### 11.1 Definición de incidente de seguridad

Un **incidente de seguridad** es cualquier evento, accidental o intencional, que afecte o ponga en peligro las tecnologías de la información o los procesos relacionados. Incluye:

- Accesos no autorizados a sistemas o datos.
- Interrupciones del servicio.
- Uso no autorizado de sistemas.
- Suplantación de identidad.
- Cambios no autorizados en sistemas o datos.

### 11.2 Elementos clave de la gestión de incidentes

| Elemento | Descripción |
|---|---|
| **Detección y respuesta** | Mecanismos eficaces para detectar, identificar y responder; alertas tempranas; respuesta coordinada |
| **Acciones a realizar** | Derivadas del análisis de riesgos; qué hacer, quién, cómo y con qué recursos |
| **Procedimientos detallados** | Evaluación del incidente, a quién reportar, documentación de evidencias, acciones post-incidente |
| **Registro de incidentes** | Se conserva un mínimo de **5 años**; sirve como criterio de medición del SGSI |

### 11.3 Ejemplo de procedimiento: interrupción de comunicaciones

1. Informar a la Dirección de la situación.
2. Identificar si la interrupción es por factores externos o internos.
3. Reportar al proveedor de servicio si el problema está en la línea de comunicación.
4. Restablecer la operación y determinar las causas; definir acciones para evitar recurrencia.
5. Anotar las acciones y resultados en el registro de incidencias.
6. Reportar el incidente al **INCIBE** (Instituto Nacional de Ciberseguridad).

### 11.4 INCIBE y CCN-CERT

- **INCIBE**: organismo que gestiona la ciberseguridad para ciudadanos, empresas y sector privado en España. Punto de reporte de incidentes.
- **CCN-CERT**: Centro Criptológico Nacional; gestiona incidentes en la Administración Pública española. Depende del CNI.

---

## 12. Control remoto en puestos de usuario

### 12.1 Riesgos del acceso remoto

El acceso remoto supone una **fuente de numerosos riesgos** porque no se dispone del mismo nivel de control que en las instalaciones físicas. Los objetivos a preservar son: **integridad, confidencialidad, autenticidad y trazabilidad**.

### 12.2 Medidas de seguridad en el acceso remoto

| Medida | Descripción |
|---|---|
| **Identificación y autenticación robusta** | MFA para evitar suplantación de identidad; mecanismo robusto de verificación |
| **VPN** | Canal seguro cifrado entre el dispositivo remoto y la red corporativa |
| **Definición de acciones permitidas** | Qué aplicaciones usar, qué datos son accesibles, bajo qué condiciones se pueden almacenar datos en dispositivos externos |
| **Políticas de sesión** | Cierre automático de sesiones inactivas; tiempo máximo de sesión |
| **BYOD (Bring Your Own Device)** | Es **preferible** que el equipo sea propiedad y esté configurado por el organismo, no por el usuario |
| **Filtros y cortafuegos** | En servidor y cliente para limitar acciones desde el acceso remoto; segmentación de red; DLP |
| **Registros de actividad (logs)** | Activar y analizar regularmente el cumplimiento de la política; implementar SIEM |

### 12.3 SIEM (Security Information and Event Management)

El **SIEM** es una solución que centraliza y correlaciona eventos de seguridad procedentes de múltiples fuentes (firewalls, servidores, IDS, etc.) para:

- Detectar patrones de ataque.
- Generar alertas automáticas.
- Facilitar la respuesta a incidentes.
- Proporcionar evidencias para auditorías.

---

## Resumen para el examen

### Lo más importante del ENS (RD 311/2022)

```
ENS = RD 311/2022 (deroga RD 3/2010)
Dimensiones: C-I-T-A-D (Confidencialidad, Integridad, Trazabilidad, Autenticidad, Disponibilidad)
Categorías: BÁSICA / MEDIA / ALTA
Niveles: BAJO / MEDIO / ALTO
Medidas: Marco Organizativo (org) + Marco Operacional (op) + Medidas de Protección (mp)
Auditorías: cada 2 años para categoría MEDIA y ALTA
```

### Criptografía: tabla comparativa

| Aspecto | Simétrica | Asimétrica |
|---|---|---|
| Claves | Una sola (compartida) | Par (pública + privada) |
| Velocidad | Rápida | Lenta |
| Uso principal | Datos en reposo, grandes volúmenes | Datos en tránsito, firma digital |
| Problema | Distribución segura de la clave | Tiempos de cálculo elevados |
| Algoritmos | AES, DES, 3DES, ChaCha20 | RSA, ECC, DSA |

### Firma digital: regla mnemotécnica

- **Firmar** = cifrar con tu **CLAVE PRIVADA** (solo tú puedes haberlo firmado).
- **Verificar** = descifrar con la **CLAVE PÚBLICA del emisor** (cualquiera puede verificar).
- **Confidencialidad** = cifrar con la **CLAVE PÚBLICA del destinatario**.
- **Descifrar** = usar tu **CLAVE PRIVADA**.

### Malware: distinciones clave

| Si preguntan por... | La respuesta es... |
|---|---|
| Captura pulsaciones del teclado | **Keylogger** |
| Oculta presencia del atacante | **Rootkit** |
| Se propaga solo por la red | **Gusano** |
| Pide rescate por los datos | **Ransomware** |
| Finge ser un antivirus | **Rogue** |
| Aprovecha un fallo concreto de software | **Exploit** |
| Bulo / alarma falsa sin código malicioso | **Hoax** |
| Redirige el DNS aunque la URL sea correcta | **Pharming** |

### Protocolos: capa OSI

| Protocolo | Capa OSI | Puerto/Notas |
|---|---|---|
| **IPsec** | Red (capa 3) | UDP 500; modos transporte y túnel |
| **SSL/TLS** | Transporte (capa 4) | Puerto 443; HTTPS |
| **WEP/WPA** | Enlace (capa 2) | Redes Wi-Fi 802.11 |
| **PGP** | Aplicación (capa 7) | Correo electrónico |
| **SSH** | Aplicación (capa 7) | Puerto 22; acceso remoto seguro |

### Normativa: fechas y datos clave

| Dato | Valor |
|---|---|
| ENS vigente | RD 311/2022 |
| Antigua LOPD | LO 15/1999 (derogada) |
| LOPDGDD | LO 3/2018 |
| Ley Seguridad Nacional | Ley 36/2015 |
| Accesibilidad web | RD 1112/2018 (WCAG 2.1) |
| Registro incidentes ENS | Mínimo 5 años |
| MD5 | 128 bits |
| SHA-1 | 160 bits |
| SHA-256 | 256 bits |
| AES (año estándar) | 2001; 128 bits de bloque; Rijndael |
| DES (roto en) | 1998 (Deep Crack) |

---

## Preguntas frecuentes en exámenes

A continuación se recogen los temas que aparecen con mayor frecuencia en el banco de preguntas del examen, con las respuestas correctas y las claves para no equivocarse.

**1. Dimensiones del ENS**
- Las dimensiones son: **C**onfidencialidad, **I**ntegridad, **T**razabilidad, **A**utenticidad, **D**isponibilidad.
- La opción "[I] Información" es **incorrecta**; la I corresponde a **Integridad**.

**2. Firma digital con clave asimétrica**
- La firma se realiza con la **clave privada del emisor**.
- El receptor verifica con la **clave pública del emisor**.
- Mnemotecnia: "David firma con su clave privada; cualquiera puede verificar con su clave pública".

**3. Objeto del ENS (RD 311/2022)**
- Establecer la política de seguridad en la utilización de medios electrónicos en el ámbito de la Administración Pública.
- Es de aplicación a las AAPP y a sus proveedores que traten sistemas o datos del sector público.

**4. Criptografía simétrica vs asimétrica: la frase incorrecta**
- **Incorrecta**: "La criptografía asimétrica es más rápida que la simétrica". Es al revés: la simétrica es más rápida.
- **Incorrecta**: "En criptografía simétrica se usan dos claves diferentes". La simétrica usa una sola clave.

**5. Dimensión integridad [I]**
- Garantiza que la información **no ha sido alterada** de forma no autorizada.

**6. Función hash MD5**
- MD5 genera un hash de **128 bits**.
- No es adecuado para aplicaciones de seguridad actuales por sus colisiones conocidas.

**7. Control de acceso ABAC**
- Permite definir políticas basadas en **atributos del usuario, del recurso y del contexto** (hora, ubicación, departamento, etc.).
- Es el modelo más flexible.

**8. Zero Trust**
- Exige **MFA incluso dentro de la red corporativa**.
- Principio: "nunca confiar, siempre verificar".

**9. OWASP Top 10**
- Lista de las 10 vulnerabilidades más críticas en aplicaciones web.
- Incluye: inyección SQL, fallos de autenticación, exposición de datos sensibles, etc.

**10. Ransomware: respuesta correcta**
- Ante un ataque de ransomware: **no pagar el rescate**, aislar los sistemas afectados, restaurar desde backup, notificar a las autoridades (INCIBE/CCN-CERT).

**11. Phishing vs Pharming**
- **Phishing**: engaño por correo/web falsa; el usuario hace clic en un enlace malicioso.
- **Pharming**: manipulación del DNS; el usuario escribe la URL correcta pero acaba en una web falsa.

**12. Man-in-the-Middle**
- Es un ataque **técnico de intercepción**, NO es ingeniería social.

**13. Firma cualificada (eIDAS)**
- Solo la **firma electrónica cualificada** equivale jurídicamente a la firma manuscrita.

**14. Nivel MEDIO del ENS**
- Un sistema es de categoría MEDIA cuando un incidente de seguridad puede causar **daño significativo**.

**15. DNI electrónico**
- La **Dirección General de la Policía (DGP)** actúa como Autoridad de Certificación del DNIe.

**16. Categorías del ENS y su relación con el daño**
- BÁSICA: daño limitado.
- MEDIA: daño significativo.
- ALTA: daño grave o muy grave.

**17. Gestión de la configuración segura (ENS)**
- Implica mantener los sistemas actualizados, con configuraciones reforzadas (hardening), eliminando servicios y cuentas innecesarios.

**18. SICRES 4.0**
- Estándar para el intercambio de registros entre Administraciones Públicas (Sistema de Información Común de Registros de Entrada y Salida).

---

## Referencias

| Fuente | URL |
|---|---|
| **ENS — Real Decreto 311/2022** | [https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191](https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191) |
| **CCN-CERT — Guías de Seguridad (Series CCN-STIC)** | [https://www.ccn-cert.cni.es/es/guias/guias-series-ccn-stic.html](https://www.ccn-cert.cni.es/es/guias/guias-series-ccn-stic.html) |
| **INCIBE** | [https://www.incibe.es](https://www.incibe.es) |
| **LOPDGDD (LO 3/2018)** | [https://www.boe.es/buscar/act.php?id=BOE-A-2018-16673](https://www.boe.es/buscar/act.php?id=BOE-A-2018-16673) |
| **Reglamento eIDAS (UE) 910/2014** | [https://eur-lex.europa.eu/legal-content/ES/TXT/?uri=celex:32014R0910](https://eur-lex.europa.eu/legal-content/ES/TXT/?uri=celex:32014R0910) |
| **ISO/IEC 27001:2022** | [https://www.iso.org/standard/27001](https://www.iso.org/standard/27001) |
| **OWASP Top 10** | [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/) |
| **AutoFirma (Ministerio)** | [https://firmaelectronica.gob.es/Home/Descargas.html](https://firmaelectronica.gob.es/Home/Descargas.html) |
| **DNI electrónico** | [https://www.dnielectronico.es](https://www.dnielectronico.es) |
| **FNMT — Certificados digitales** | [https://www.sede.fnmt.gob.es](https://www.sede.fnmt.gob.es) |
| **RD 1112/2018 — Accesibilidad web** | [https://www.boe.es/buscar/act.php?id=BOE-A-2018-12699](https://www.boe.es/buscar/act.php?id=BOE-A-2018-12699) |
| **Ley 36/2015 de Seguridad Nacional** | [https://www.boe.es/buscar/act.php?id=BOE-A-2015-10389](https://www.boe.es/buscar/act.php?id=BOE-A-2015-10389) |
