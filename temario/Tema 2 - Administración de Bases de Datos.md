# Tema 2: Administración de Bases de Datos

> Este tema tiene **82 preguntas** en el banco de examen — de los más cargados del Bloque IV. El reparto es bastante uniforme, pero hay subtemas que aparecen una y otra vez: **SQL** (clasificar sentencias DDL/DML/DCL/TCL, comportamiento de JOINs y funciones de agregado), **normalización** (formas normales, dependencias parciales y transitivas), **tipos de backup** (RPO/RTO, incremental vs. diferencial), **modelos de nube NIST** (IaaS/PaaS/SaaS) y **virtualización** (hipervisores tipo 1 y 2, VDI). No es un tema que se estudie de golpe: conviene repartirlo y volver a él.

---

## 1. El Administrador de Bases de Datos (DBA)

### 1.1 Definición y diferencia con el administrador de datos

El **DBA** (*Database Administrator*) es el profesional técnico responsable de la creación, gestión y mantenimiento de las bases de datos en una organización. Pero ojo: en el examen suele aparecer la distinción entre el DBA y el **administrador de datos**, que son perfiles diferentes y complementarios:

| Rol | Enfoque | Alcance |
|---|---|---|
| **Administrador de datos** | Estrategia, calidad y valor del dato para el negocio | Toda la organización |
| **DBA** | Creación y gestión técnica de la base de datos; implementa los controles técnicos definidos por el administrador de datos | Una BBDD específica y los sistemas relacionados |

La característica más importante del DBA no es solo el conocimiento técnico puro, sino el dominio de las **políticas y normas de la empresa** y la capacidad de aplicarlas en el momento adecuado. Saber cuándo actuar y cómo importa tanto como saber el SQL.

La función del DBA varía mucho según el tamaño del entorno:
- En una **BBDD personal**: el propio usuario actúa como administrador.
- En **grupos de trabajo**: una o dos personas asumen la función a tiempo parcial.
- En **entornos organizacionales**: la función es compleja y a tiempo completo, y suele dividirse entre varios especialistas.

### 1.2 Funciones del DBA

El DBA no trabaja en solitario: colabora con equipos de programadores, administradores de sistemas y otros técnicos. Sus responsabilidades abarcan desde lo más técnico hasta lo más operativo:

- Creación y gestión de bases de datos.
- Implementación de controles técnicos relacionados con TI.
- Gestión de infraestructuras tecnológicas y protocolos de red.
- Optimización del código de programación.
- Garantizar un procesamiento eficiente de la información.
- Gestión de interfaces para el manejo de datos (gestión de cambios, gestión por objetivos).
- Coordinación de tecnologías de almacenamiento.
- Implementación de copias de seguridad y centros de respaldo.
- Integración de sistemas para el funcionamiento eficiente del negocio.
- Pruebas de software y hardware.

### 1.3 Responsabilidades del DBA

El DBA es responsable primordialmente de:

1. **Administrar la estructura de la base de datos**: participar en el diseño inicial, implementación y control, incluyendo la elección del DBMS. Gestionar las solicitudes de modificación de esquema, evaluando el impacto en toda la comunidad de usuarios antes de implementarlas.

2. **Administrar la actividad de los datos**: establecer estándares, guías y procedimientos de control para que los usuarios trabajen de forma cooperativa. La estandarización abarca todos los aspectos, desde la captura de información hasta su procesamiento y presentación. El DBA utiliza alternativas para minimizar problemas de acceso concurrente.

3. **Administrar el SGBD**: compilar y analizar estadísticas de rendimiento, identificar áreas problemáticas y responder a quejas sobre tiempo de respuesta, precisión y facilidad de uso. El DBA monitoriza continuamente las actividades de los usuarios y rastrea tasas de error.

4. **Establecer el diccionario de datos**: registrar los estándares de estructura disponibles para todos los usuarios involucrados.

5. **Asegurar la confiabilidad de la BBDD**: gestionar la documentación de todas las modificaciones, formatos de prueba y resultados. La documentación es fundamental en situaciones de crisis.

6. **Confirmar la seguridad de la BBDD**: gestionar los permisos de acceso.

---

## 2. Integridad de Datos

La **integridad de datos** se refiere a la precisión y totalidad de la información en una base de datos. Cada vez que se ejecuta un `INSERT`, `DELETE` o `UPDATE`, existe el riesgo de dejar los datos en un estado inconsistente. Por eso el DBMS incorpora mecanismos para preservarla automáticamente.

### 2.1 Tipos de restricciones de integridad

Hay tres niveles de integridad, y cada uno actúa sobre un ámbito distinto: la columna, la fila o la relación entre tablas.

| Tipo | Descripción | Mecanismo SQL |
|---|---|---|
| **Integridad de dominio** | Los datos almacenados en una columna deben cumplir ciertas condiciones (tipo, rango, NOT NULL) | `CHECK`, `NOT NULL`, tipos de datos |
| **Integridad de entidad** | La clave primaria de una tabla debe contener valores únicos y no nulos para cada fila | `PRIMARY KEY` |
| **Integridad referencial** | Mantiene la coherencia entre tablas relacionadas (padre/hijo): las claves foráneas deben referenciar claves primarias existentes | `FOREIGN KEY ... REFERENCES` |

---

## 3. Sistemas de Gestión de BBDD (SGBD / DBMS)

Un **SGBD** (*Sistema de Gestión de Bases de Datos*) es el software que permite definir, crear, mantener y controlar el acceso a las bases de datos. El SGBD actúa como intermediario entre las aplicaciones y los datos reales: ninguna aplicación accede directamente a los archivos en disco, todo pasa por él.

### 3.1 Arquitectura ANSI-SPARC

La [arquitectura ANSI-SPARC](https://es.wikipedia.org/wiki/Arquitectura_ANSI-SPARC), propuesta en 1975, establece una separación en **tres niveles de abstracción** para aislar a los usuarios de los detalles físicos del almacenamiento. La idea clave es que si cambias la forma en que los datos se guardan en disco (nivel interno), las aplicaciones no tienen que saber nada de ese cambio (el nivel externo queda intacto):

```
┌─────────────────────────────────────────┐
│      NIVEL EXTERNO (Vistas de usuario)  │  ← Distintas vistas para cada usuario/app
├─────────────────────────────────────────┤
│      NIVEL CONCEPTUAL (Lógico)          │  ← Estructura completa: entidades, relaciones, restricciones
├─────────────────────────────────────────┤
│      NIVEL INTERNO (Físico)             │  ← Cómo se almacenan los datos en disco: bloques, índices
└─────────────────────────────────────────┘
```

| Nivel | También llamado | Qué describe | Quién lo usa |
|---|---|---|---|
| **Externo** | Vista de usuario | Parte de la BBDD relevante para cada usuario o aplicación | Usuarios finales, aplicaciones |
| **Conceptual** | Lógico | Estructura completa: tablas, atributos, relaciones, restricciones (independiente del almacenamiento físico) | DBA, diseñadores |
| **Interno** | Físico | Cómo se almacenan físicamente los datos: archivos, índices, bloques en disco | Motor DBMS |

> **Pregunta de examen frecuente:** "En el nivel lógico, se da una definición de las estructuras de datos que constituyen la base de datos."

### 3.2 Principales SGBD del mercado

#### Microsoft SQL Server
- Desarrollado por **Microsoft**. Utiliza el lenguaje **Transact-SQL (T-SQL)**, una implementación del estándar ANSI SQL.
- Compatible con transacciones y procedimientos almacenados. Ofrece interfaz gráfica de administración.
- Puede configurarse para admitir múltiples instancias en un solo servidor físico.

#### MySQL
- Sistema relacional creado originalmente por **MySQL AB**. Sun Microsystems adquirió MySQL AB en 2008 y, posteriormente, **Oracle Corporation** adquirió Sun en 2010 — de ahí que hoy MySQL pertenezca a Oracle.
- Conocido por ser una **base de datos rápida en lectura**, pero puede presentar problemas de integridad en entornos de alta concurrencia en escritura.
- Existe versión **Community** (gratuita) y versiones **Enterprise**.

#### Oracle Database
- Líder histórico en el mercado empresarial. Potente y versátil, pero su principal desventaja es el **alto coste** (licencias y configuración son caras).
- Ofrece funciones avanzadas: **PL/SQL** (lenguaje procedural con triggers y procedimientos almacenados), sólida integridad referencial declarativa y soporte para múltiples plataformas.
- La versión **Oracle 19c** es la versión Long Term Support de referencia. Existe también una edición gratuita llamada **Express Edition**.
- Las dos estructuras de memoria básicas de una instancia Oracle son la **SGA** (*System Global Area*, compartida por todos los procesos) y la **PGA** (*Program Global Area*, privada de cada proceso de servidor). Este dato aparece en el examen.

> **Dato de examen:** Los **Redo Log Files** de Oracle registran todos los cambios realizados sobre la base de datos y protegen contra pérdida de datos ante fallos eléctricos o de disco. Son el mecanismo de recuperación ante fallos de instancia.

#### PostgreSQL
- Originado en 1986 en la Universidad de Berkeley como proyecto POSTGRES (sucesor de Ingres). En 1996 adoptó el nombre PostgreSQL para reflejar su soporte de SQL.
- **Ventajas**: open source, altamente ampliable, muy conforme al estándar SQL, soporta tipos de datos complejos (geoespaciales, JSON), búsqueda de texto completo, multiplataforma.
- **Desventajas**: no disponible por defecto en todos los hostings, documentación principalmente en inglés, puede tener velocidad de lectura inferior a MySQL en cargas muy simples.

#### Arquitectura lógica de Oracle (tablespaces)

El diagrama del PDF ilustra la jerarquía de almacenamiento lógico de Oracle:

```
Base de Datos
  └── Tablespace (unidad lógica de almacenamiento; el tablespace SYSTEM es el diccionario de datos)
        └── Segmento (conjunto de extensiones de un objeto: tabla, índice, etc.)
              └── Extensión (conjunto de bloques contiguos)
                    └── Bloque lógico (unidad mínima de lectura/escritura)
```

### 3.3 Bases de datos en la nube (BBDD Cloud)

Una base de datos en la nube es un **servicio** que se crea y accede a través de una plataforma cloud: tiene las mismas funciones que una BBDD tradicional, pero sin gestionar hardware propio. El proveedor se encarga de la infraestructura, las actualizaciones y la alta disponibilidad.

**Ventajas del cloud para BBDD:**
- Menos dependencia del hardware (el proveedor lo gestiona)
- Escalabilidad rápida (ampliar o reducir capacidad sin parar el servicio)
- Pago por uso (sin inversión inicial en hardware)
- Acceso a la tecnología más reciente sin migraciones costosas
- Alta disponibilidad y redundancia gestionadas por el proveedor

**Proveedores principales:**

| Proveedor | Servicio relacional | Servicio NoSQL |
|---|---|---|
| **Amazon AWS** | Amazon RDS (Oracle, SQL Server, MySQL, PostgreSQL...) | Amazon DynamoDB |
| **Oracle** | Oracle Autonomous Database (Cloud) | — |
| **Microsoft** | Azure SQL Database | Azure Cosmos DB |

---

## 4. SQL: El Lenguaje de Bases de Datos

**SQL** (*Structured Query Language*) es el estándar ISO/IEC 9075 para interactuar con bases de datos relacionales. Una de las preguntas más típicas del examen es clasificar sentencias SQL en su sublenguaje correcto, así que memoriza bien esta tabla:

| Sublenguaje | Significado | Sentencias principales |
|---|---|---|
| **DDL** | Data Definition Language | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** | Data Manipulation Language | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** | Data Control Language | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

### 4.1 DDL — Definición de estructuras

El DDL define y modifica la estructura de la base de datos (tablas, índices, vistas...). Las sentencias DDL **no pueden deshacerse con ROLLBACK** en la mayoría de SGBD (incluido Oracle), porque hacen un COMMIT implícito.

```sql
-- Crear una tabla
CREATE TABLE empleados (
    id         NUMBER PRIMARY KEY,
    nombre     VARCHAR2(100) NOT NULL,
    salario    NUMBER(10,2),
    id_depto   NUMBER REFERENCES departamentos(id)
);

-- Modificar estructura (añadir columna)
ALTER TABLE empleados ADD fecha_alta DATE;

-- Eliminar tabla y su estructura (irreversible)
DROP TABLE empleados;

-- Vaciar tabla conservando la estructura (más rápido que DELETE)
TRUNCATE TABLE empleados;
```

**Diferencias clave entre DELETE, TRUNCATE y DROP** — pregunta casi segura en el examen:

| Operación | Tipo | Elimina datos | Elimina estructura | Rollback posible |
|---|---|---|---|---|
| `DELETE` | DML | Filas (con WHERE opcional) | No | Sí |
| `TRUNCATE` | DDL | Todas las filas | No | No (en Oracle) |
| `DROP` | DDL | Todas las filas | Sí (tabla completa) | No |

### 4.2 DML — Manipulación de datos

El DML opera sobre los datos (no sobre la estructura). A diferencia del DDL, los cambios DML pueden deshacerse con `ROLLBACK` mientras no se haya ejecutado un `COMMIT`.

```sql
-- Consulta básica con DISTINCT (elimina duplicados)
SELECT DISTINCT departamento FROM empleados;

-- Funciones de agregado: COUNT sobre columna excluye NULL, incluye duplicados
SELECT COUNT(salario), AVG(salario), MAX(salario) FROM empleados;

-- COUNT(*) cuenta todas las filas incluyendo NULL
SELECT COUNT(*) FROM empleados;

-- JOINs
SELECT e.nombre, d.nombre
FROM empleados e
INNER JOIN departamentos d ON e.id_depto = d.id;   -- Solo filas con coincidencia en ambas tablas

-- LEFT OUTER JOIN: TODAS las filas de la tabla izquierda (con NULL donde no haya coincidencia a la derecha)
SELECT e.nombre, d.nombre
FROM empleados e
LEFT JOIN departamentos d ON e.id_depto = d.id;

-- FULL JOIN: unión completa de ambas tablas
SELECT e.nombre, d.nombre
FROM empleados e
FULL JOIN departamentos d ON e.id_depto = d.id;
```

> **Atención al examen:** `LEFT OUTER JOIN` devuelve **TODAS** las filas de la tabla izquierda (incluyendo las que tienen coincidencia). No excluye la intersección.

### 4.3 DCL — Control de acceso

El DCL gestiona los permisos. `GRANT` los otorga y `REVOKE` los retira. Con `WITH GRANT OPTION`, el usuario receptor puede a su vez ceder ese permiso a otros.

```sql
-- Otorgar privilegios
GRANT SELECT, INSERT ON empleados TO usuario1;
GRANT SELECT ON empleados TO usuario1 WITH GRANT OPTION;

-- Revocar privilegios
REVOKE INSERT ON empleados FROM usuario1;
```

### 4.4 TCL — Control de transacciones

El TCL gestiona las transacciones. Una transacción es una unidad de trabajo que puede confirmarse (`COMMIT`) o deshacerse (`ROLLBACK`). Los `SAVEPOINT` permiten volver a un punto intermedio sin deshacer toda la transacción.

```sql
-- Confirmar cambios permanentemente
COMMIT;

-- Deshacer todos los cambios desde el último COMMIT
ROLLBACK;

-- Establecer punto intermedio
SAVEPOINT sp1;
ROLLBACK TO SAVEPOINT sp1;  -- Deshace hasta sp1, sin deshacer toda la transacción
```

### 4.5 Vistas (VIEWS)

Una **vista** es una tabla virtual basada en el resultado de una consulta `SELECT` almacenada. Permite:
- **Restringir el acceso**: diferentes usuarios ven solo ciertas filas o columnas.
- Simplificar consultas complejas.
- No almacena datos físicamente (excepto las **vistas materializadas**, que sí almacenan el resultado).

En Oracle existen dos tipos de objetos de vista (`OBJECT_TYPE`): `VIEW` y `MATERIALIZED VIEW`.

### 4.6 Secuencias en Oracle

Las **secuencias** generan valores numéricos únicos, muy usadas para claves primarias:

```sql
CREATE SEQUENCE SQ1 START WITH 1 INCREMENT BY 1 NOCACHE NOCYCLE;

-- Obtener el siguiente valor (incrementa el contador)
SELECT SQ1.NEXTVAL FROM DUAL;

-- Obtener el valor actual (sin incrementar)
SELECT SQ1.CURRVAL FROM DUAL;
```

> **Nota:** La pseudocolumna correcta es `NEXTVAL`, no `NEXTVALUE` ni `NEXT`.

---

## 5. Modelo Relacional y Normalización

### 5.1 Terminología del modelo relacional

El modelo relacional tiene su propia jerga. El examen usa indistintamente los términos técnicos y los coloquiales, así que conviene conocer ambos:

| Término técnico | Equivalente coloquial | Significado |
|---|---|---|
| **Relación** | Tabla | La estructura que almacena datos |
| **Tupla** | Fila / registro | Una entrada en la tabla |
| **Atributo** | Columna / campo | Una propiedad de la entidad |
| **Dominio** | — | Conjunto de valores válidos para un atributo |
| **Grado** | — | Número de atributos (columnas) de una relación |
| **Cardinalidad** | — | Número de tuplas (filas) de una relación |
| **Clave primaria (PK)** | — | Atributo(s) que identifican unívocamente cada fila |
| **Clave ajena / foránea (FK)** | — | Atributo que referencia la PK de otra tabla |
| **Clave candidata** | — | Superclave minimal (cualquier atributo o conjunto que podría ser PK) |

> **Truco:** Grado = columnas (piensa en "grado de libertad"); Cardinalidad = filas (piensa en "cuántas hay").

El **esquema relacional** completo debe especificar: nombre de relaciones, atributos, dominios, **claves primarias y claves ajenas**.

### 5.2 Características de las BBDD relacionales

- **Escasa redundancia de datos** (gracias a la normalización)
- **Procesamiento orientado a conjuntos** (las operaciones actúan sobre conjuntos de filas)
- **Lenguaje de consultas homogéneo** (SQL es el estándar ISO)
- Alta consistencia mediante propiedades **ACID**

### 5.3 Propiedades ACID

| Propiedad | Significado |
|---|---|
| **Atomicidad** | La transacción se ejecuta completa o no se ejecuta |
| **Consistencia** | La BBDD pasa de un estado válido a otro estado válido |
| **Aislamiento** | Las transacciones concurrentes no interfieren entre sí |
| **Durabilidad** | Los cambios confirmados (COMMIT) persisten aunque haya fallos |

### 5.4 Álgebra relacional

| Operación | Símbolo | Trabaja con | Equivalente SQL |
|---|---|---|---|
| **Selección** | σ (sigma) | Filas | `WHERE` |
| **Proyección** | π (pi) | Columnas (elimina duplicados) | `SELECT DISTINCT col1, col2` |
| **Unión** | ∪ | Dos relaciones | `UNION` |
| **Intersección** | ∩ | Dos relaciones | `INTERSECT` |
| **Diferencia** | − | Dos relaciones | `MINUS` / `EXCEPT` |
| **Producto cartesiano** | × | Dos relaciones | `CROSS JOIN` |
| **Join** | ⋈ | Dos relaciones | `JOIN ... ON` |

> La **proyección** extrae columnas y elimina duplicados. La **selección** filtra filas.

### 5.5 Normalización

La **normalización** es el proceso de organizar las tablas para eliminar redundancias y evitar anomalías (problemas al insertar, actualizar o borrar datos). La idea es que cada dato esté en un solo sitio. Como efecto secundario puede aumentar el número de tablas y requerir más JOINs, lo que a veces reduce el rendimiento de lectura. Para mejorar el rendimiento se usa la **desnormalización** (introducir redundancia controlada).

> **Importante:** El objetivo de la normalización es eliminar redundancias y garantizar consistencia. **No** es mejorar el rendimiento ni reducir el número de tablas — de hecho, normalmente aumenta el número de tablas.

| Forma Normal | Requisito clave | Qué elimina |
|---|---|---|
| **1FN** | Valores atómicos (indivisibles); sin grupos repetitivos; cada fila única | Columnas multivaluadas o repetidas |
| **2FN** | En 1FN + todos los atributos no clave dependen **completamente** de la clave primaria | Dependencias parciales (solo relevante si la clave es compuesta) |
| **3FN** | En 2FN + ningún atributo no clave depende de otro atributo no clave | Dependencias transitivas |
| **FNBC** | Todo determinante en una dependencia funcional debe ser superclave | Anomalías que 3FN no cubre |

**Ejemplo de violación de 3FN (dependencia transitiva):**
```
EMPLEADO(id_empleado, nombre, id_departamento, nombre_departamento)
  id_empleado → id_departamento  (directa, OK)
  id_departamento → nombre_departamento  (TRANSITIVA: viola 3FN — nombre_dpto
                                          depende de id_dpto, no de id_empleado)

Solución: separar en EMPLEADO(id, nombre, id_dpto) + DEPARTAMENTO(id_dpto, nombre_dpto)
```

### 5.6 Modelo Entidad-Relación (ER)

El modelo ER, propuesto por Peter Chen en 1976, es la herramienta de diseño conceptual por excelencia: permite modelar la realidad antes de trasladarla a tablas. Sus componentes:

- **Entidad**: objeto del mundo real distinguible, por ejemplo "Empleado" o "Departamento" (se representa como rectángulo).
- **Relación/interrelación**: asociación entre dos o más entidades, por ejemplo "trabaja en" (se representa como rombo).
- **Atributo**: propiedad que describe una entidad o relación, por ejemplo "nombre" o "fecha de alta".
- **Cardinalidad**: cuántas instancias de una entidad se relacionan con cuántas de otra (1:1, 1:N, M:N).
- **Entidad débil**: entidad que no tiene identidad propia y depende de la existencia de otra entidad para existir (representada con rectángulo de doble trazo); su relación de identificación usa rombo de doble trazo.

### 5.7 Modelos de bases de datos históricos

| Modelo | Estructura | Característica | Ejemplo |
|---|---|---|---|
| **Jerárquico** | Árbol | Cada hijo tiene exactamente un padre | IMS de IBM |
| **En red (PLEX)** | Grafo/PLEX | Un hijo puede tener varios padres; permite M:N directamente | IDMS (CODASYL) |
| **Relacional** | Tablas | Relaciones mediante claves; consultas con SQL | Oracle, MySQL, PostgreSQL |
| **OO (orientado a objetos)** | Objetos | Herencia, encapsulamiento, polimorfismo | db4o, ObjectDB |
| **NoSQL** | Varios | Flexible, distribuido, escalable horizontalmente | MongoDB, Cassandra |

---

## 6. NoSQL

Las bases de datos **NoSQL** (*Not only SQL*) surgieron en los años 2000 para dar respuesta a los requisitos de escalabilidad y flexibilidad que los sistemas relacionales no podían satisfacer fácilmente en aplicaciones web de gran escala. Empresas como Google, Amazon y Facebook necesitaban almacenar y procesar cantidades de datos que ningún SGBD relacional tradicional podía gestionar de forma eficiente, y diseñaron sus propias soluciones (Bigtable, Dynamo...) que luego inspiraron los productos actuales.

### 6.1 Tipos de bases de datos NoSQL

| Tipo | Descripción | Ejemplos |
|---|---|---|
| **Documental** | Almacena documentos JSON/BSON | MongoDB, CouchDB, DynamoDB |
| **Clave-valor** | Par clave-valor simple | Redis, DynamoDB, Riak |
| **Columnas anchas** | Tablas con columnas dinámicas (wide-column) | Cassandra, HBase, BigTable |
| **Grafos** | Nodos y relaciones | Neo4j, Amazon Neptune |
| **Series temporales** | Datos con marca de tiempo | InfluxDB, TimescaleDB |

> **Atención:** MariaDB es una base de datos **relacional** (fork de MySQL), **no es NoSQL**.

### 6.2 SQL vs. NoSQL

| Característica | SQL (Relacional) | NoSQL |
|---|---|---|
| Esquema | Fijo y predefinido | Flexible o sin esquema |
| Escalado | Vertical (principalmente) | Horizontal (sharding) |
| Transacciones | ACID | BASE (Basically Available, Soft state, Eventual consistency) |
| Integridad referencial | Alta | Baja (la gestiona la aplicación) |
| Normalización | Alta | Desnormalización frecuente |
| Consultas complejas | Excelentes (SQL) | Limitadas (específicas de cada BBDD) |
| Casos de uso | ERP, CRM, sistemas transaccionales | Big Data, redes sociales, IoT, tiempo real |

La **principal ventaja de NoSQL** es la **mayor escalabilidad horizontal** (añadir más servidores). La integridad referencial y la normalización son fortalezas del modelo relacional, no de NoSQL.

---

## 7. Tecnologías de Almacenamiento

### 7.1 Racks

Un **rack** (armario bastidor) es un soporte metálico estándar de 19 pulgadas utilizado en centros de datos para alojar equipos electrónicos, informáticos y de comunicaciones: SAIs, enrutadores, switches, paneles de parcheo, servidores, matrices de discos, etc. Los equipos se montan deslizándolos sobre raíles horizontales y se miden en unidades de rack (**U**), donde 1U = 4,445 cm de altura.

### 7.2 DAS, SAN y NAS

Los tres enfoques principales de almacenamiento en entornos empresariales se diferencian principalmente en dónde reside el sistema de archivos y cómo acceden a él los servidores:

| Tecnología | Nombre completo | Descripción | Protocolo |
|---|---|---|---|
| **DAS** | Direct Attached Storage | Conexión punto a punto entre servidor y almacenamiento. El almacenamiento es local al sistema de archivos del servidor | SCSI, SATA, SAS |
| **SAN** | Storage Area Network | Red especializada para almacenamiento. Permite que varios servidores accedan a varios dispositivos de almacenamiento. El almacenamiento es remoto pero el sistema de archivos reside en el servidor | Fibre Channel, iSCSI (FC/GbE) |
| **NAS** | Network-Attached Storage | Las aplicaciones hacen solicitudes de datos al sistema de archivos de forma remota (el sistema de archivos está en el NAS) | SMB/CIFS, NFS |

El diagrama del PDF ilustra que en **DAS** el flujo va directamente de aplicación a disco; en **SAN** pasa por una red FC/GbE; en **NAS** la aplicación accede a un sistema de ficheros remoto a través de la red.

> **SAN es más costosa que NAS** por su tecnología avanzada. Una SAN puede alcanzar capacidades de cientos o miles de terabytes y permite compartir datos entre servidores sin afectar el rendimiento.

### 7.3 Estructura de una SAN

Una SAN tiene **tres capas**:
- **Capa Host**: servidores y dispositivos que quieren acceder al almacenamiento.
- **Capa de Fibra/Conexión**: cables y switches (SAN hubs o SAN switches) que conectan hosts y almacenamiento.
- **Capa de Almacenamiento**: discos y cintas donde se guardan los datos.

**Características de una SAN:**
- **Latencia baja**: rápida transmisión de datos.
- **Conectividad múltiple**: varios servidores se conectan a la misma unidad de almacenamiento.
- **Distancia**: hasta 10 km sin repetidores (fibra óptica).
- **Alta velocidad**: desde 1 Gbps hasta 4 y 8 Gbps.
- **Redundancia**: conexiones redundantes, tolerancia a fallos.
- **Seguridad**: zonificación y presentación (zoning/masking).
- Tipos de red: **Red Fibre Channel** (especializada, muy rápida) o **Red IP** (iSCSI sobre Ethernet).

Para conectarse a una SAN mediante **red IP** se utiliza una **tarjeta iSCSI HBA** (no una FC HBA, que es para Fibre Channel, ni una tarjeta Ethernet genérica).

### 7.4 Protocolos de acceso al almacenamiento

- **A nivel de bloque**: Fibre Channel, SAS, FICON, iSCSI
- **A nivel de archivo**: NFS, CIFS/SMB

---

## 8. Virtualización

### 8.1 Definición

La **virtualización** es la creación de versiones virtuales de recursos tecnológicos (hardware, SO, almacenamiento, redes) mediante software. Existe desde la década de 1960 (IBM lo usaba en mainframes), pero con la popularización de los servidores x86 y el cloud computing se ha convertido en el pilar de la infraestructura moderna.

La pieza clave es el **Hypervisor** o **VMM** (*Virtual Machine Monitor*): una capa de software que se interpone entre el hardware físico y los sistemas operativos de las máquinas virtuales. El hipervisor reparte dinámicamente CPU, memoria, red y almacenamiento entre las VMs, haciendo que cada una crea que tiene su propio hardware exclusivo.

**Principales proveedores**: Citrix, VMware, Microsoft.

### 8.2 Tipos de hipervisores

Los hipervisores se clasifican en dos tipos según dónde se instalan. Esta distinción es muy frecuente en el examen:

| Tipo | También llamado | Descripción | Ejemplos |
|---|---|---|---|
| **Tipo 1** | Bare-metal | Se instala directamente sobre el hardware, sin SO intermedio. Más eficiente y seguro porque no hay capas adicionales | VMware ESXi, Microsoft Hyper-V, Xen, KVM |
| **Tipo 2** | Hosted | Se instala como aplicación sobre un SO convencional (Windows, macOS, Linux). Más fácil de usar pero menos eficiente | VirtualBox, VMware Workstation, Parallels |

> **Regla rápida:** Tipo 1 = producción empresarial. Tipo 2 = entornos de desarrollo o uso personal.

Un **hipervisor** permite que un servidor host preste soporte a varias VMs mediante el uso compartido virtual de sus recursos (CPU, memoria, almacenamiento, red).

### 8.3 Tipos de virtualización

La virtualización no es solo de servidores. Hay varios ámbitos donde se aplica:

- **Virtualización de servidor**: el servidor físico se divide en múltiples VMs mediante un hipervisor. Es el uso más habitual.
- **Virtualización de almacenamiento**: combina varios dispositivos de almacenamiento físico para presentarlos como un único recurso lógico (ver sección SDS).
- **Virtualización a nivel de sistema operativo**: múltiples instancias aisladas comparten el mismo kernel del SO subyacente (por ejemplo, contenedores Docker).
- **Virtualización de aplicación**: las aplicaciones se ejecutan dentro de entornos aislados del SO del host, reduciendo conflictos (por ejemplo, App-V de Microsoft).
- **Infraestructura de escritorio virtual (VDI)**: los escritorios se ejecutan en el datacenter y los usuarios acceden remotamente (ver sección 8.5).

### 8.4 Virtualización del almacenamiento — SDS

La **virtualización del almacenamiento** o **SDS** (*Software Defined Storage*) permite independizarse de los elementos de hardware y evitar depender de un único fabricante. Mantiene un registro (*metadatos* en una tabla de asignaciones) de las asignaciones de almacenamiento virtualizado.

**Soluciones hardware:**
- **IBM SVC** (*SAN Volume Controller*): usa dos servidores IBM con Linux propio para virtualizar el almacenamiento; gestiona cabinas V7000 y V3700 conectadas por fibra.
- **HP HPE StoreVirtual VSA**: se puede instalar en servidores VMware o Hyper-V.

**Soluciones software:**
- **VMware vSAN**: integrado en el hipervisor ESXi. Utiliza los discos locales (SSD, NVMe) de cada host ESXi del clúster para crear un datastore distribuido. Requiere al menos un disco SSD por servidor. Se licencia por socket. Limitación: diseñado exclusivamente para entornos VMware. Proporciona alta disponibilidad con al menos tres hosts.
- **DataCore**: funciona sobre Windows; compatible con FC, FCoE, iSCSI, SAS, SATA, SCSI, IDE, USB. Configuración clásica: dos nodos en modo espejo.

> En VMware, un **datastore** es un espacio de almacenamiento lógico (no un dispositivo físico) para almacenar máquinas virtuales (discos .vmdk, configuración .vmx, snapshots, etc.).

### 8.5 VDI — Infraestructura de Escritorio Virtual

La **VDI** (*Virtual Desktop Infrastructure*) usa máquinas virtuales para gestionar y proporcionar escritorios virtuales a usuarios finales. Un hipervisor divide los servidores en VMs que alojan escritorios accesibles remotamente.

**Diagrama del PDF**: los dispositivos de usuario (User devices) se conectan a través de un **Connection Broker** al hipervisor, que asigna VMs a cada usuario.

| Característica | VDI (on-premises) | DaaS (Desktop as a Service, cloud) |
|---|---|---|
| Infraestructura | Propia organización | Proveedor cloud |
| Control datos | Total | Limitado |
| Gestión técnica | Requiere personal TIC | Externalizada al proveedor |
| Escalabilidad | Limitada por hardware | Alta (bajo demanda) |
| Soberanía tecnológica | Alta (datos bajo control propio) | Menor (datos en cloud externo) |
| Coste | CapEx (inversión inicial) | OpEx (pago por uso) |

**VDI puede ser:**
- **Persistente**: permite personalización continua del escritorio.
- **No persistente**: escritorios genéricos, se resetean al cerrar sesión.

**Ventajas de VDI**: movilidad, acceso remoto, ahorro de costes de hardware, mayor seguridad y gestión centralizada.

**Cuándo usar VDI**: cuando se gestionan datos clasificados o sensibles con exigencias de soberanía tecnológica (ENS, RGPD). VDI on-premises mantiene el control total sobre el ciclo de vida del dato.

### 8.6 Productos de virtualización

#### VMware
- Filial de EMC Corporation (Dell Inc.). Software de virtualización para x86.
- Productos: **VMware Workstation**, **VMware Server** (gratuito), **VMware Player** (gratuito), **VMware ESXi** (hipervisor tipo 1).
- Compatible con Windows, Linux y macOS (con VMware Fusion en Intel).
- VMware tiende a virtualizar la plataforma x86 ejecutando muchas instrucciones directamente sobre el hardware físico.

#### Microsoft Hyper-V
- Programa de virtualización de Microsoft basado en hipervisor para sistemas **64 bits** (AMD-V o Intel VT).
- Versión preliminar incluida en Windows Server 2008; versión final lanzada el **26 de junio de 2008**.

#### Microsoft Virtual PC
- Software gestor de virtualización desarrollado por Connectix, adquirido por Microsoft.
- Su función es emular, a través de virtualización, un hardware donde ejecutar un SO específico.
- Incluye "**Virtual Machine Additions**" para facilitar el intercambio de archivos entre el sistema anfitrión e invitado.
- Virtual PC traduce instrucciones en llamadas al SO físico (diferencia con VMware, que ejecuta más instrucciones directamente en hardware).

#### Citrix
- Citrix Systems, Inc. (fundada en 1989): corporación multinacional de virtualización de servidores, SaaS y cloud.
- **Citrix Delivery Center** incluye:
  - **XenDesktop**: cliente ligero para estaciones de trabajo virtuales, acceso remoto a aplicaciones corporativas.
  - **Citrix XenApp**: entrega de aplicaciones Windows bajo demanda virtualizadas en el datacenter.
  - **XenServer**: monitor de VMs compatible con varias arquitecturas (gratuito desde abril de 2009).
  - **NetScaler**: balanceo de cargas avanzado y gestión del tráfico.
- **Citrix Receiver**: usa el protocolo propio **HDX** (*High-Definition Experience*) para una experiencia optimizada en redes limitadas.
- **Citrix OpenCloud**: nubes híbridas para cargas de trabajo empresariales.

> **Protocolo de Citrix**: HDX (High-Definition Experience). No confundir con RDP (Microsoft) ni con Blast Extreme (VMware Horizon).

#### KVM vs. KALI Linux
- **KVM** (*Kernel-based Virtual Machine*): módulo de virtualización del kernel Linux. Es un hipervisor tipo 1. No confundir con KALI Linux, que es una distribución Linux para ciberseguridad (pentesting), no una tecnología de virtualización.

#### Formato OVF/OVA
- **OVF** (*Open Virtualization Format*): estándar abierto (DMTF) para el **empaquetado, distribución e intercambio de máquinas virtuales** entre diferentes hipervisores (VMware, VirtualBox, Hyper-V, KVM).
- **OVA**: un único archivo TAR que empaqueta todos los componentes OVF.

---

## 9. Sistemas de Backup: Políticas, Guardado, Monitorización y Recuperación

### 9.1 Importancia del backup

El **backup** (copia de seguridad) es la red de seguridad ante cualquier desastre. Los datos pueden perderse por motivos muy variados: fallos en discos duros, ransomware, errores humanos, bugs en aplicaciones, intrusiones, incendios, robos o desastres naturales. Sin una política de backup bien definida, cualquiera de estos eventos puede ser catastrófico.

La **velocidad de recuperación** (no solo la capacidad de recuperar, sino cuán rápido) es un criterio clave al evaluar un sistema de backup — de ahí la importancia de RTO y RPO.

### 9.2 Tipos (niveles) de backup

Los tres tipos de backup se distinguen por **qué se copia** y tienen implicaciones directas sobre el tiempo y el espacio necesarios para guardar y restaurar:

| Tipo | Descripción | Ventaja | Inconveniente |
|---|---|---|---|
| **Completo** (*Full*) | Guarda todos los datos en un único soporte | Restauración rápida (todo en un soporte) | Mayor espacio y tiempo de guardado |
| **Incremental** | Solo copia los datos modificados desde la **última copia** (completa o incremental anterior) | Menor espacio y tiempo de guardado | Restauración lenta: necesita el backup completo + TODOS los incrementales en orden |
| **Diferencial** | Copia todos los datos modificados desde la **última copia completa** (no desde el último diferencial) | Restauración más rápida que incremental (solo completo + último diferencial) | El tamaño de cada diferencial crece con el tiempo |

**Ejemplo práctico (semana con backup completo el domingo):**
- **Diferencial** del miércoles contiene: cambios del lunes + martes + miércoles (todo desde el completo).
- **Incremental** del miércoles contiene: solo los cambios del miércoles.
- Para restaurar al miércoles con incremental necesitas: completo + incr. lunes + incr. martes + incr. miércoles.
- Para restaurar al miércoles con diferencial necesitas: completo + diferencial del miércoles.

### 9.3 Políticas de backup

Al establecer una política de backup deben abordarse:

- **Definición del tipo de datos**: identificar qué se respalda.
- **Nivel de importancia**: prioridad de cada tipo de dato.
- **Elección de niveles de backup**: completo, incremental, diferencial.
- **Ámbito de restauración y disponibilidad**: tiempo para restaurar a un estado específico.
- **Tecnologías y plataformas**: qué herramientas y entornos participan.
- **Consideración de recursos**: velocidad de red, hardware disponible.
- **Entornos de trabajo**: preproducción, desarrollo, producción.
- **Tiempo de recuperación**: RTO y RPO.
- **Periodicidad**: la frecuencia de las copias es un aspecto crítico.

Las políticas deben:
- Formar parte del **plan de continuidad de negocio**.
- Cumplir el **RGPD** (*Reglamento General de Protección de Datos*).

**Componentes de la política:**
- **Inicio del proceso**: cómo se lanza el backup.
- **Intervención de hardware, software y factor humano**: quién y qué participa.
- **Almacenamiento de datos**: dónde se guardan los respaldos.
- **Plan de recuperación**: cómo se recuperan los datos en caso necesario.
- **Ámbito de aplicación**: tipo de datos y niveles de seguridad.
- **Frecuencia de copias**: la recuperación se realiza desde el último respaldo, por lo que su frecuencia es crítica.
- **Soporte de copias**: se recomienda el cloud (AWS, etc.) por fiabilidad y seguridad.

### 9.4 Guardado de backup

- Decidir cuánto tiempo se conserva cada copia.
- Las copias deben guardarse en **ubicaciones diferentes** (no en el mismo lugar que el sistema original).
- Los **servicios de backup en la nube** ofrecen soluciones rentables y seguras: mayor velocidad de recuperación ante desastres, transferencia fiable, escalabilidad, mejor accesibilidad y posibilidad de monitorización.

### 9.5 Monitorización

La monitorización y validación de las copias de seguridad son cruciales. Dos objetivos clave:
- Contar con **un único punto de revisión** del estado de todas las copias.
- **Generar informes automáticos**.

Herramienta destacada: **Nagios**, sistema de monitorización de redes de código abierto que vigila hardware y software, emite alertas ante comportamiento no deseado. Características: monitorización de servicios de red, recursos hardware, independencia de SO, monitorización remota mediante SSL/SSH, soporte de plugins personalizados.

### 9.6 Recuperación: RTO y RPO

RTO y RPO son los dos indicadores que definen cuán exigente es una política de recuperación. Confundirlos es un error clásico de examen:

| Concepto | Nombre completo | Definición en castellano simple |
|---|---|---|
| **RPO** | Recovery Point Objective | ¿Cuántos datos puedo perder? Es el máximo período de datos que la organización puede permitirse perder. Si el RPO es de 4 horas, un backup cada 4 horas es suficiente. Para banca o e-commerce, el RPO es prácticamente cero. |
| **RTO** | Recovery Time Objective | ¿Cuánto tiempo puedo estar caído? Es el tiempo máximo tolerable desde que ocurre el desastre hasta que el sistema vuelve a estar operativo. |

> **Truco:** RPO mira hacia atrás (¿cuánto pasado puedo perder?). RTO mira hacia adelante (¿cuánto futuro puedo perder recuperando?).

Ambos valores determinan directamente el coste de la infraestructura de backup: cuanto más bajos sean, más cara es la solución necesaria.

---

## 10. Máquinas Virtuales: Backup de Sistemas Físicos y Virtuales

En entornos de virtualización, **VMware e Hyper-V** son los líderes del mercado. La capacidad de ejecutar varios SO en un único servidor físico maximiza el aprovechamiento del hardware y centraliza la gestión, pero introduce un riesgo: **si el hardware físico falla, todas las VMs que corren sobre él caen a la vez**.

Por eso, cuando todos los sistemas comparten el mismo hardware, las copias de seguridad son aún más cruciales que en entornos físicos tradicionales. VMware y Hyper-V ofrecen mecanismos para tomar **instantáneas** (*snapshots*) de las VMs, pero atención: **las snapshots no son un sustituto del backup** y no garantizan la consistencia de la información (especialmente si la VM tiene una BBDD activa en el momento de la snapshot).

**Entorno de virtualización recomendado:**
- Host de virtualización (proporciona la potencia de procesado)
- Sistema de almacenamiento (cabina de discos o SAN para los discos virtuales)
- Sistema de backup dedicado (por ejemplo, un servidor NAS con software como Nakivo para almacenar las copias)
- Opcionalmente: segundo NAS en ubicación diferente para replicar las copias (protección ante desastres físicos)

---

## 11. Cloud Computing

### 11.1 Modelos de servicio (NIST SP 800-145)

El [NIST](https://csrc.nist.gov/publications/detail/sp/800-145/final) (*National Institute of Standards and Technology*) es la referencia oficial para la definición de cloud computing. Su publicación SP 800-145 establece **tres modelos de servicio** y cinco características esenciales. Los modelos de servicio son la parte que más aparece en el examen:

> **Clave para el examen:** IaaS, PaaS y SaaS son **modelos de servicio**. Nube pública, privada, híbrida y comunitaria son **modelos de despliegue**. No mezclarlos.

| Modelo | Nombre | Descripción | El cliente gestiona | El proveedor gestiona | Ejemplos |
|---|---|---|---|---|---|
| **IaaS** | Infrastructure as a Service | Infraestructura virtualizada (servidores, red, almacenamiento) | SO, middleware, aplicaciones | Hardware físico, red, virtualización | AWS EC2, Azure VMs, Google Compute Engine |
| **PaaS** | Platform as a Service | Plataforma para desarrollar y desplegar aplicaciones | Aplicaciones y datos | SO, middleware, runtime, hardware | Google App Engine, Heroku, Azure App Service |
| **SaaS** | Software as a Service | Aplicaciones completas accesibles vía navegador | Solo configuración de usuario | Todo lo demás | Microsoft 365, Gmail, Salesforce |

**Responsabilidades por modelo:**

| Componente | IaaS | PaaS | SaaS |
|---|---|---|---|
| Aplicación | **Cliente** | **Cliente** | Proveedor |
| Middleware/Runtime | **Cliente** | Proveedor | Proveedor |
| Sistema Operativo | **Cliente** | Proveedor | Proveedor |
| Infraestructura física | Proveedor | Proveedor | Proveedor |

> **Nubes públicas, privadas e híbridas** son **modelos de despliegue**, no modelos de servicio.

### 11.2 Características esenciales del cloud (NIST)

1. **Autoservicio bajo demanda**: aprovisionamiento automático sin intervención del proveedor.
2. **Acceso amplio a la red**: desde cualquier dispositivo y ubicación.
3. **Agrupación de recursos**: recursos físicos compartidos entre múltiples clientes (multitenancy).
4. **Elasticidad rápida**: recursos escalables rápidamente según la demanda.
5. **Servicio medido**: monitorización y facturación del uso (pago por uso).

### 11.3 Modelos de despliegue

| Modelo | Descripción |
|---|---|
| **Nube pública** | Recursos compartidos del proveedor (AWS, Azure, GCP) |
| **Nube privada** | Recursos dedicados a una organización |
| **Nube híbrida** | Combinación de pública y privada |
| **Nube comunitaria** | Compartida por varias organizaciones con intereses comunes |

---

## 12. Acceso a Bases de Datos: Interfaces y Protocolos

Las aplicaciones necesitan una capa de acceso estándar para conectarse a las bases de datos. Hay varias APIs con nombres parecidos que se prestan a confusión — precisamente por eso suelen aparecer en el examen:

| Interfaz | Tipo | Descripción |
|---|---|---|
| **JDBC** | API Java | Conecta y ejecuta sentencias SQL en bases de datos relacionales desde Java |
| **ODBC** | API estándar (Microsoft/ISO) | Acceso a diferentes fuentes de datos desde múltiples lenguajes; muy extendido en entornos Windows |
| **OLE DB** | API Microsoft (COM) | Sucesor de ODBC para el ecosistema Windows; acceso a datos relacionales y no relacionales |
| **JNDI** | API Java | Accede a servicios de nombres y directorios (LDAP, DNS); **NO** es un interfaz de BBDD — es para localizar recursos, no para consultarlos |
| **PDBC** | — | No existe como estándar reconocido — si lo ves en una pregunta de examen, es el distractor |

**Pool de conexiones**: en vez de abrir y cerrar una conexión a la BBDD por cada petición (lo cual es costoso), el pool mantiene un conjunto de conexiones activas y las reutiliza. Es especialmente útil en **aplicaciones web** con muchos usuarios concurrentes. Las conexiones tienen un ciclo de vida gestionado (idle timeout, validación periódica) y no se mantienen abiertas indefinidamente.

---

## Resumen para el examen

Los subtemas más frecuentes en el banco de 82 preguntas son:

1. **SQL — DDL/DML/DCL/TCL**: clasificar sentencias (`ALTER` es DDL; `GRANT` es DCL; `ROLLBACK` es TCL). Diferencias entre `DELETE`, `TRUNCATE` y `DROP`. Comportamiento de `DISTINCT`, `COUNT`, `LEFT JOIN`.

2. **Normalización**: definiciones precisas de 1FN, 2FN, 3FN y FNBC. La 2FN elimina dependencias parciales; la 3FN elimina dependencias transitivas. La normalización puede reducir el rendimiento de lectura.

3. **Tipos de backup**: Full vs. Incremental vs. Diferencial. Incremental: necesita completo + TODOS los incrementales. Diferencial: solo completo + último diferencial. RPO = cuántos datos puedes perder. RTO = cuánto tardas en recuperarte.

4. **Modelos de nube (NIST)**: SaaS/PaaS/IaaS (son modelos de **servicio**, no de despliegue). En IaaS el cliente gestiona SO, middleware y aplicaciones. En SaaS el proveedor gestiona todo.

5. **Virtualización e hipervisores**: Tipo 1 (bare-metal: ESXi, Hyper-V, KVM) vs. Tipo 2 (hosted: VirtualBox, VMware Workstation). Hipervisor = permite que un host soporte varias VMs compartiendo sus recursos.

6. **VDI vs. DaaS**: VDI on-premises para datos sensibles/soberanía tecnológica; DaaS para elasticidad y externalización.

7. **SAN/NAS/DAS**: SAN usa Fibre Channel o iSCSI; NAS usa NFS/SMB. Para conectarse a una SAN vía IP: tarjeta iSCSI HBA. SAN tiene tres capas: Host, Fibra/Conexión, Almacenamiento.

8. **Arquitectura ANSI-SPARC**: nivel externo (vistas de usuario), conceptual/lógico (estructura completa) e interno/físico (almacenamiento). El nivel lógico define las estructuras de datos.

9. **Oracle**: SGA + PGA (estructuras de memoria). Redo Log Files = protegen ante pérdida de integridad por fallos eléctricos/disco. Secuencias: `NEXTVAL` (no `NEXTVALUE`). Tablespace > Segmento > Extensión > Bloque.

10. **NoSQL**: MongoDB (documental), Cassandra (columnas), Neo4j (grafos), HBase (columnas), DynamoDB (clave-valor/documental). **MariaDB es SQL relacional, no NoSQL**. Ventaja principal de NoSQL: escalabilidad horizontal.

11. **Modelo relacional**: Grado = número de columnas. Tupla = fila. Proyección = extrae columnas (elimina duplicados). Selección = filtra filas. Clave ajena = relaciona dos entidades. Entidades débiles dependen de la existencia de otra entidad.

12. **DBA**: DBA ≠ Administrador de datos (el DBA es técnico; el administrador de datos es estratégico). Responsabilidades del DBA: estructura, actividad de datos, SGBD, diccionario de datos, confiabilidad, seguridad.

13. **Almacenamiento VMware**: un **datastore** es un contenedor lógico para VMs (no es hardware físico). **OVF/OVA** es el estándar de empaquetado de VMs (no un paquete ofimático ni formato GIS). **vSAN** usa almacenamiento local de los hosts ESXi (no necesita cabina externa).

14. **Citrix**: protocolo propio **HDX** (no RDP ni Blast Extreme). Productos: XenDesktop, XenApp, XenServer, NetScaler.

15. **Bonding Linux modo 1** (active-backup): solo tolerancia a fallos, **no mejora el rendimiento** (solo usa una interfaz a la vez).

---

## Preguntas frecuentes en exámenes

### Subtemas con mayor presencia en el banco de preguntas

#### 1. SQL (DDL/DML/DCL/TCL) — Alta frecuencia

Las preguntas de SQL son las más frecuentes. Se piden clasificaciones de sentencias y comportamientos específicos:

- *"¿Cuál comando SQL pertenece al DDL?"* → `ALTER` (no `GRANT` que es DCL, no `INSERT` que es DML).
- *"¿Qué hace DISTINCT?"* → Elimina filas duplicadas en el resultado (no ordena, no filtra por PK).
- *"¿Qué hace COUNT(columna)?"* → Incluye duplicados, **excluye valores NULL**.
- *"¿Qué sentencia deshace los cambios de una transacción?"* → `ROLLBACK` (no COMMIT, no SAVEPOINT).
- *"¿LEFT OUTER JOIN excluye la intersección?"* → **Falso**: LEFT JOIN devuelve TODAS las filas de la tabla izquierda, incluyendo las que tienen coincidencia.

#### 2. Normalización — Alta frecuencia

- *"Objetivo de la normalización"* → Eliminar redundancias y garantizar consistencia (**no** mejorar rendimiento, **no** reducir el número de tablas).
- *"Entidad en 3FN"* → En 2FN + sin dependencias transitivas de atributos no clave sobre la clave primaria.
- *"En qué FN: campo que no depende totalmente de la clave se mueve a otra tabla"* → **2FN**.

#### 3. Tipos de backup y RPO/RTO — Alta frecuencia

- *"¿Cuántos soportes necesitas para restaurar con backup incremental?"* → El completo + todos los incrementales.
- *"¿Cuántos soportes con diferencial?"* → El completo + el último diferencial.
- *"¿Qué es el RPO?"* → Cuánto tiempo puede estar una empresa sin datos (máximo período de datos perdibles).
- *"¿Qué es el RTO?"* → Tiempo máximo para recuperar el sistema.

#### 4. Cloud Computing (IaaS/PaaS/SaaS) — Alta frecuencia

- *"Tres modelos de servicio cloud"* → SaaS, PaaS, IaaS (no HaaS; las nubes pública/privada/híbrida son **modelos de despliegue**).
- *"En SaaS, ¿quién gestiona todo?"* → El proveedor; el cliente solo usa aplicaciones.
- *"En IaaS, ¿qué gestiona el cliente?"* → SO, middleware y aplicaciones.
- *"Característica esencial del cloud"* → Autoservicio bajo demanda (no colocation, no dedicación exclusiva de recursos).

#### 5. Virtualización — Frecuencia media-alta

- *"¿Qué es un hipervisor?"* → Software que permite que un host soporte varias VMs compartiendo recursos (CPU, memoria).
- *"¿Qué es un datastore en VMware?"* → Espacio lógico para almacenar VMs (no hardware físico, no disco virtual).
- *"¿Cuál no es tecnología de virtualización?"* → KALI Linux (es una distro de ciberseguridad).
- *"¿Qué es OVF?"* → Estándar de empaquetado de VMs.
- *"¿Cuándo usar VDI y no DaaS?"* → Cuando hay datos sensibles o exigencias de soberanía tecnológica.

#### 6. SAN/NAS/DAS — Frecuencia media

- *"Para conectarse a una SAN vía IP, se usa..."* → Tarjeta iSCSI HBA.
- *"SAN es más costosa que..."* → NAS.
- *"En NAS, el protocolo de acceso es..."* → NFS o CIFS/SMB.

---

## Referencias

- [Arquitectura ANSI-SPARC (Wikipedia ES)](https://es.wikipedia.org/wiki/Arquitectura_ANSI-SPARC) — Los tres niveles de abstracción del SGBD
- [NIST SP 800-145: The NIST Definition of Cloud Computing](https://csrc.nist.gov/publications/detail/sp/800-145/final) — Definición oficial de IaaS, PaaS, SaaS y los modelos de despliegue
- [Oracle Database 19c Documentation](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/) — Arquitectura de memoria (SGA/PGA), tablespaces, redo logs, secuencias
- [PostgreSQL Documentation](https://www.postgresql.org/docs/current/) — Tipos de datos SQL, constraints, funciones de agregado
- [DMTF OVF Standard](https://www.dmtf.org/standards/ovf) — Estándar de empaquetado de máquinas virtuales
- [VMware vSAN Documentation](https://docs.vmware.com/en/VMware-vSAN/index.html) — Almacenamiento definido por software integrado en ESXi
- [Modelo de normalización de BBDD (IBM)](https://www.ibm.com/think/topics/database-normalization) — 1FN, 2FN, 3FN, FNBC
- [Tipos de backup: Full, Differential, Incremental](https://www.veeam.com/blog/backup-types.html) — Comparativa práctica de estrategias de backup
- [Nagios Monitoring](https://www.nagios.org/) — Sistema de monitorización de código abierto
- [MongoDB NoSQL Explained](https://www.mongodb.com/nosql-explained) — Tipos de bases de datos NoSQL y comparativa con SQL
- [Apache Cassandra](https://cassandra.apache.org/) — Base de datos NoSQL de columnas anchas
- [Citrix Virtual Apps and Desktops](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/) — VDI y virtualización de aplicaciones con protocolo HDX
- [Linux Bonding Modes](https://www.kernel.org/doc/Documentation/networking/bonding.txt) — Modos de agregación de interfaces de red en Linux
- [Esquema Nacional de Seguridad (ENS)](https://www.ccn-cert.cni.es/es/ens/esquema-nacional-de-seguridad.html) — Marco de seguridad para AAPP españolas
