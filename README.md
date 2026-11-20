🐘 Manual Maestro de PostgreSQL 18

# 🐘 Manual Maestro de PostgreSQL 18
### De cero absoluto a tu primera base de datos respaldada, segura y optimizada

> **Nivel:** Principiante absoluto · **Entorno del curso:** Windows + pgAdmin 4 + psql · **Duración estimada de práctica:** 4 a 6 horas
> **Fuente:** transcripción del curso "CURSO DE POSTGRESQL" (12 módulos, 3 h 53 min), depurada, corregida y reorganizada en 11 fases ejecutables.

---

## 📌 Cómo usar este manual

1. **Lee la Fase completa antes de tocar el teclado.** Luego ejecuta paso por paso.
2. **Escribe el código a mano.** Copiar y pegar funciona, pero escribir fija el aprendizaje.
3. **No avances con errores pendientes.** Cada fase termina con una verificación ("✅ Checkpoint").
4. **Guarda tus scripts** en un archivo llamado `consultas.sql`, tal como hace el instructor.

**Leyenda de iconos**

| Icono | Significado |
|---|---|
| 💡 | **Pro Tip**: consejo profesional que ahorra horas |
| ⚠️ | **Alerta**: acción que puede dañar datos |
| ✅ | **Checkpoint**: verificación obligatoria antes de continuar |
| 🖼️ | **Recurso visual**: descripción de imagen + prompt listo para generarla |
| 📝 | **Nota del Editor**: vacío o ambigüedad detectada en la transcripción |

> 📝 **Nota del Editor:** las imágenes de este manual son **marcadores de posición**. Cada bloque `prompt` está listo para pegarse en un generador de imágenes en el siguiente paso. Las maquetas de interfaz son ilustrativas: los textos y la posición exacta de los botones pueden variar ligeramente respecto a tu versión instalada.

---

## 1. Resumen ejecutivo: ¿qué vas a aprender?

Al terminar este manual serás capaz de **instalar, consultar, programar, proteger y respaldar** una base de datos PostgreSQL real, sin ayuda externa.

**Competencias concretas que adquirirás:**

- Instalar PostgreSQL 18 en Windows y verificar que el servicio esté corriendo.
- Conectarte con dos clientes distintos: `psql` (consola) y pgAdmin 4 (interfaz gráfica).
- Diseñar tablas con tipos de datos correctos, claves primarias y claves foráneas.
- Dominar el CRUD completo: `INSERT`, `SELECT`, `UPDATE`, `DELETE`.
- Filtrar, ordenar, paginar y renombrar resultados con `WHERE`, `ORDER BY`, `LIMIT`, `OFFSET` y `AS`.
- Resumir datos con `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING` y `DISTINCT`.
- Combinar tablas con los cuatro `JOIN` y modelar relaciones 1:1, 1:N y N:N.
- Acelerar consultas con índices y **leer un plan de ejecución** con `EXPLAIN ANALYZE`.
- Proteger la integridad con transacciones (`BEGIN`, `COMMIT`, `ROLLBACK`) y entender ACID + MVCC.
- Automatizar reglas con funciones, procedimientos y triggers en PL/pgSQL.
- Crear roles, otorgar y revocar permisos con `GRANT` / `REVOKE`.
- Hacer un backup y restaurarlo después de eliminar la base de datos.

**Mapa de rutas del manual**

| Fase | Módulos del video | Resultado tangible |
|---|---|---|
| 1 · Preparación | 1–2 | PostgreSQL instalado y servicio verificado |
| 2 · Herramientas | 3 | pgAdmin conectado + BD `prueba` creada |
| 3 · Fundamentos | 4 | Tablas con PK y FK funcionando |
| 4 · SQL básico | 5 | BD `tienda_online` con CRUD completo |
| 5 · Consultas avanzadas | 6 | Reportes de ventas con agregados |
| 6 · Relaciones y JOINs | 7 | BD `relaciones` con los 3 modelos |
| 7 · Índices y rendimiento | 8 | Índice creado + plan interpretado |
| 8 · Transacciones | 9 | Transferencias seguras con ACID |
| 9 · Programación | 10 | Función, procedimiento y trigger activos |
| 10 · Seguridad | 11 | Rol `alumno` con permisos controlados |
| 11 · Respaldos | 12 | Backup creado, BD borrada y restaurada |

🖼️ **[Imagen: Infografía horizontal tipo "roadmap" con 11 estaciones numeradas conectadas por una línea, cada una con un icono (descarga, enchufe, tabla, lápiz, gráfico, enlace, rayo, escudo, engranaje, candado, cofre) y el nombre de la fase.]**

```prompt
Infografía vectorial plana y premium, formato 16:9, fondo blanco con detalles azul profundo (#1B3A5C) y acento celeste (#2E86DE). Ruta horizontal tipo "roadmap" con 11 círculos numerados del 1 al 11 unidos por una línea continua; debajo de cada círculo un icono lineal simple y una etiqueta en español: 1 Instalación, 2 Herramientas, 3 Fundamentos, 4 SQL básico, 5 Agregados, 6 JOINs, 7 Índices, 8 Transacciones, 9 Programación, 10 Seguridad, 11 Respaldos. Título superior: "Ruta de aprendizaje PostgreSQL 18". Tipografía sans-serif limpia, mucho espacio en blanco, sin fotografía, estilo manual corporativo.
```

---

## 2. Requisitos previos

**Hardware y sistema**

- PC con **Windows** (el curso se dicta en Windows; Linux, macOS y Docker son alternativas válidas).
- Permisos de **administrador** para instalar software y abrir el panel de Servicios.
- Espacio libre en disco para el instalador (**≈ 354 MB**) más los datos que crearás.
- Conexión a internet para descargar el instalador desde el sitio oficial.

**Software que obtendrás durante la Fase 1**

- PostgreSQL 18 (motor de base de datos).
- pgAdmin 4 (interfaz gráfica).
- Command Line Tools (`psql`, `pg_dump`, `pg_restore`, `initdb`).
- Stack Builder (opcional).

**Conocimientos necesarios**

- Manejo básico de Windows: menú Inicio, buscador, clic derecho, Explorador de archivos.
- Escribir texto y usar el teclado (tabulador, Enter, comillas simples y dobles).
- **Ningún conocimiento previo de SQL.** Todo se explica desde cero.

**Materiales de trabajo**

- Un archivo `consultas.sql` para guardar tus scripts (Bloc de notas o el editor de pgAdmin).
- Una contraseña que **debes recordar**: será la del superusuario `postgres`.
- Un cuaderno o nota para anotar: contraseña, puerto `5432` y nombres de tus bases de datos.

> 💡 **Pro Tip:** anota la contraseña del superusuario en un gestor de contraseñas antes de instalar. El video no muestra ningún mecanismo de recuperación, y perderla complica mucho el acceso.

> 📝 **Nota del Editor:** la transcripción menciona un aviso del instalador relacionado con **Visual C++**, pero no detalla qué hacer si aparece. Si el instalador solicita ese componente, acepta la instalación de los redistribuibles y verifica el requisito en la documentación oficial antes de continuar.

---

## 3. Glosario de términos

### 3.1 Conceptos de base de datos

| Término | Explicación en lenguaje simple |
|---|---|
| **RDBMS** | Sistema que gestiona bases de datos relacionales. PostgreSQL es uno. |
| **SQL** | *Structured Query Language*. El idioma con el que le pides cosas a la base de datos. |
| **Tabla** | La "hoja de Excel" donde vive un tipo de cosa: clientes, pedidos, productos. |
| **Columna (campo)** | Un atributo: `nombre`, `email`, `edad`. Cada columna tiene un tipo fijo. |
| **Fila (registro / tupla)** | Un caso concreto: el cliente Juan Pérez con todos sus datos. |
| **Clave primaria (PK)** | La columna que identifica una fila sin repetirse y sin quedar vacía. Es el DNI del registro. |
| **Clave foránea (FK)** | Columna que apunta a la PK de otra tabla. Evita datos "huérfanos". |
| **Integridad referencial** | Regla que impide registrar un pedido de un cliente que no existe. |
| **Tabla intermedia (pivote)** | Tabla puente que permite relaciones muchos a muchos. |
| **Índice** | Como el índice de un libro: te lleva directo a la página sin leerlo todo. |
| **Transacción** | Paquete de operaciones que se ejecutan todas o ninguna. |
| **ACID** | Cuatro garantías: Atomicidad, Consistencia, Aislamiento, Durabilidad. |
| **MVCC** | Control de concurrencia multiversión: cada transacción ve su propia "foto" de los datos. |
| **Lock (bloqueo)** | Reserva temporal de una fila para que nadie más la modifique a la vez. |
| **Backup** | Foto completa de la base de datos en un momento dado. |
| **Restore** | Reconstruir la base de datos a partir de esa foto. |

### 3.2 Palabras clave de SQL

| Palabra | Para qué sirve |
|---|---|
| `CREATE` / `DROP` / `ALTER` | Crear / eliminar / modificar estructuras. |
| `INSERT INTO ... VALUES` | Agregar filas nuevas. |
| `SELECT ... FROM` | Leer datos. |
| `UPDATE ... SET` | Modificar datos existentes. |
| `DELETE FROM` | Eliminar filas. |
| `WHERE` | Filtrar filas por condición. |
| `ORDER BY ... ASC/DESC` | Ordenar de menor a mayor o al revés. |
| `LIMIT` / `OFFSET` | Cuántas filas mostrar / cuántas saltar. |
| `AS` | Renombrar una columna **solo en el resultado** (alias). |
| `GROUP BY` / `HAVING` | Agrupar filas / filtrar esos grupos. |
| `DISTINCT` | Quitar duplicados del resultado. |
| `JOIN` | Combinar tablas relacionadas. |
| `SERIAL` | Entero autoincremental (1, 2, 3…). |
| `GENERATED ALWAYS AS IDENTITY` | Forma moderna del autoincremental. |
| `NOT NULL` / `UNIQUE` / `CHECK` | Obligatorio / sin repetidos / debe cumplir una regla. |
| `DEFAULT` | Valor automático si no envías ninguno. |
| `CONSTRAINT` | Nombre explícito de una regla de la tabla. |
| `BEGIN` / `COMMIT` / `ROLLBACK` | Iniciar / confirmar / deshacer una transacción. |
| `GRANT` / `REVOKE` | Dar / quitar permisos. |
| `EXPLAIN ANALYZE` | Mostrar el plan y medir el tiempo real de una consulta. |

### 3.3 Componentes y herramientas

| Término | Qué es |
|---|---|
| **postmaster** | Proceso padre: escucha conexiones, autentica y crea un backend por cliente. |
| **backend** | Proceso que atiende a un cliente: parsea, optimiza y ejecuta su consulta. |
| **Shared buffers** | Memoria compartida donde PostgreSQL cachea datos. |
| **psql** | Consola oficial. El puente entre tú y el servidor. |
| **pgAdmin 4** | Interfaz gráfica oficial, gratuita. |
| **Stack Builder** | Utilidad para instalar extras (drivers, extensiones geográficas). Opcional. |
| **PL/pgSQL** | Lenguaje procedural de PostgreSQL: variables, `IF`, bucles. |
| **Función** | Bloque reutilizable que **siempre devuelve** un valor. Se llama con `SELECT`. |
| **Procedimiento** | Bloque que ejecuta acciones y **no devuelve** valor. Se llama con `CALL`. |
| **Trigger** | Disparador automático ante `INSERT`, `UPDATE` o `DELETE`. |
| **Rol** | Identidad de acceso: puede ser usuario (con login) o grupo (sin login). |
| **postgresql.conf** | Archivo de configuración del servidor (puerto, memoria, logs). |
| **pg_hba.conf** | Archivo que define quién se conecta, desde dónde y cómo se autentica. |
| **JSONB** | JSON en formato binario, más rápido de consultar que `JSON`. |
| **ENUM** | Tipo personalizado con lista fija de valores (ej.: estados). |

---

## Fase 1 · Preparación del entorno (Módulos 1–2)

### 1.1 Entiende qué vas a instalar (2 minutos de teoría)

PostgreSQL es un sistema de gestión de bases de datos **relacional y objeto-relacional**, de **código abierto**, que almacena y consulta datos de forma estructurada.

**Sus cuatro superpoderes:**

- **Cumple SQL estándar** de forma estricta y completa, y admite extensiones avanzadas.
- **Es extensible**: puedes crear tus propios tipos de datos, funciones, operadores e incluso indexadores.
- **Es estable y confiable**, apto para aplicaciones críticas.
- **Es gratuito siempre**: no existen ediciones *enterprise* de pago. Todas las funciones están en la versión comunitaria.

**Línea de tiempo mínima**

| Año | Hito |
|---|---|
| 1986 | Nace el proyecto **POSTGRES** en la Universidad de California (Berkeley), liderado por Michael Stonebraker, como evolución de Ingres. |
| 1986–1994 | Desarrollo enfocado en el modelo objeto-relacional y reglas avanzadas. |
| 1995 | Se agrega soporte SQL y se renombra **Postgres95**. |
| 1996 | Se renombra **PostgreSQL** y se lanza la versión 6.0. Comienza el desarrollo abierto fuera de la universidad. |
| Actualidad | Versión **18**, con replicación, paralelismo en consultas, soporte JSON y extensiones vectoriales para IA. |

**Comparativa rápida (según el curso)**

| Criterio | PostgreSQL | MySQL / MariaDB | SQL Server / Oracle |
|---|---|---|---|
| Licencia | Código abierto | Código abierto | Propietarias (con opciones gratuitas y de pago) |
| Cumplimiento SQL | Estricto y completo | Más flexible | Bueno, con extensiones propias |
| Tipos avanzados | JSON indexado, arrays, geometría, tipos custom | Básicos + JSON | Avanzados, pero menos extensibles |
| Consultas complejas | Excelente | Bueno | Muy bueno / Excelente |
| Costo | Gratuito siempre | Gratuito comunitario | Licencias de uso |
| Escenario ideal | Apps complejas y analíticas | Webs simples y rápidas | Entornos enterprise con soporte corporativo |

**Casos de uso citados en el curso:** Instagram, Netflix, Uber, Apple, Microsoft, Spotify y Discord, además de banca, e-commerce y plataformas cloud.

> 📝 **Nota del Editor:** estos casos de uso se mencionan de forma general en el video, sin detalle técnico verificable. Tómalos como referencia de adopción, no como especificación de arquitectura.

**Arquitectura en una frase:** PostgreSQL usa un modelo **cliente-servidor basado en procesos** (no en hilos). El `postmaster` recibe tu conexión, crea un `backend` para ti, y ese backend trabaja con la memoria compartida y los archivos en disco. Gracias a **MVCC**, muchos usuarios leen a la vez sin bloquearse.

🖼️ **[Imagen: Diagrama de arquitectura cliente-servidor de PostgreSQL con postmaster, tres backends, shared buffers, procesos de fondo y archivos en disco.]**

```prompt
Diagrama técnico vectorial plano, formato 16:9, fondo blanco, paleta azul profundo y celeste con gris claro. A la izquierda, tres iconos de cliente etiquetados "psql", "pgAdmin", "Aplicación / Backend". Al centro, un recuadro grande titulado "Servidor PostgreSQL" que contiene: arriba una caja "postmaster (proceso padre)", debajo tres cajas "backend 1", "backend 2", "backend 3", a la derecha una caja "Memoria compartida (shared buffers)" y otra "Procesos de fondo". A la derecha del servidor, un icono de disco con la etiqueta "Almacenamiento: tablas en archivos físicos". Flechas etiquetadas "conexión" de clientes al postmaster y "lee / escribe" de backends a memoria y disco. Nota inferior: "MVCC: lecturas concurrentes sin bloqueos". Tipografía sans-serif, estilo manual corporativo, sin fotografía.
```

### 1.2 Descarga el instalador oficial

1. **Abre** tu navegador web.
2. **Entra** al sitio oficial de PostgreSQL.
3. **Haz clic** en la sección **Downloads** (Descargas).
4. **Selecciona** tu sistema operativo: **Windows**.
5. **Localiza** la versión más reciente disponible: **18**.
6. **Haz clic** en **"Download the installer"**.
7. **Elige** la fila correspondiente a **Windows x86-64**.
8. **Haz clic** en el enlace de descarga y **espera**. El archivo pesa ≈ 354 MB.
9. **Verifica** en tu carpeta de Descargas que el archivo `.exe` esté completo.

✅ **Checkpoint:** tienes un instalador `.exe` de PostgreSQL 18 para Windows x86-64 en tu equipo.

> 📝 **Nota del Editor:** la transcripción no dicta la URL exacta ni el nombre del archivo. Descarga siempre desde el dominio oficial del proyecto y verifica la versión antes de ejecutar.

### 1.3 Ejecuta el instalador paso a paso

1. **Haz doble clic** en el archivo descargado.
2. **Acepta** el aviso de permisos de Windows si aparece.
3. **Atiende** el aviso relacionado con Visual C++ si se presenta (ver Nota del Editor en Requisitos).
4. **Pulsa** **Next** en la pantalla de bienvenida.
5. **Deja** el directorio de instalación por defecto. **Pulsa Next**.
6. **Revisa** la pantalla de componentes. **Deja los cuatro marcados** y pulsa **Next**.

| Componente | Qué es | ¿Lo necesitas? |
|---|---|---|
| **PostgreSQL Server** | El motor de la base de datos. | **Obligatorio.** Sin esto no hay PostgreSQL. |
| **pgAdmin 4** | Cliente gráfico para crear BD, consultar, ver tablas, roles y backups. | **Muy recomendado** si estás aprendiendo. |
| **Stack Builder** | Instalador de extras: extensiones, drivers, datos geográficos. | Opcional. |
| **Command Line Tools** | `psql`, `pg_dump`, `pg_restore`, etc. | **Muy recomendado** para consola y scripts. |

🖼️ **[Imagen: Maqueta del asistente de instalación de PostgreSQL en la pantalla "Select Components", con las cuatro casillas marcadas y una flecha roja señalando el botón Next.]**

```prompt
Ilustración estilo captura de pantalla de un asistente de instalación de Windows, formato 4:3, ventana gris clara con título "Setup - PostgreSQL 18". Encabezado "Select Components" y subtítulo "Select the components you want to install". Lista de cuatro casillas de verificación todas marcadas: "PostgreSQL Server", "pgAdmin 4", "Stack Builder", "Command Line Tools". A la derecha de cada nombre, una pequeña etiqueta explicativa en español. Botones inferiores "Back", "Next" y "Cancel"; una flecha roja gruesa señala el botón "Next". Panel lateral izquierdo azul con el texto "Installation Wizard". Estilo limpio y realista de interfaz, sin logos oficiales.
```

7. **Deja** el directorio de datos por defecto. **Pulsa Next**.
8. **Escribe** la contraseña del superusuario `postgres`. **Repítela** en el campo de confirmación.
9. **Anótala** en tu gestor de contraseñas. **Pulsa Next**.
10. **Deja** el puerto por defecto **5432**. **Pulsa Next**.
11. **Deja** la configuración regional (locale) por defecto. **Pulsa Next**.
12. **Pulsa Next** en el resumen de instalación.
13. **Espera** a que la barra de progreso termine.
14. **Decide** si deseas abrir Stack Builder al finalizar. **Pulsa Finish**.

> ⚠️ **Alerta:** esta contraseña pertenece al **superusuario** de la base de datos. Tiene poder total. No la compartas ni la reutilices en otros servicios.

> 💡 **Pro Tip:** si el puerto 5432 ya está ocupado por otra instalación de PostgreSQL, elige otro puerto libre y **anótalo**. Lo necesitarás en cada conexión.

### 1.4 Cierra Stack Builder (no lo necesitas aún)

1. **Observa** la ventana de Stack Builder si se abrió.
2. **Entiende** su propósito: instalar complementos opcionales (datos geográficos, drivers, extensiones especiales).
3. **Selecciona** tu versión de PostgreSQL solo si vas a instalar un extra.
4. **Pulsa Cancel** y confirma con **Sí** si no instalarás nada extra.

✅ **Checkpoint:** PostgreSQL funciona perfectamente sin Stack Builder. Cerrarlo no rompe nada.

### 1.5 Verifica que el servicio esté en ejecución

Este paso evita el 80 % de los errores de conexión.

1. **Pulsa** la tecla **Windows** o haz clic en el menú Inicio.
2. **Escribe** la palabra `Servicios`.
3. **Pulsa Enter** para abrir el panel de servicios de Windows.
4. **Desplázate** por la lista hasta encontrar el servicio de PostgreSQL (ejemplo del video: **postgresql-x64-18**).
5. **Mira** la columna **Estado**. Debe decir **En ejecución**.
6. **Si está detenido**: haz clic derecho sobre el servicio y elige **Iniciar**.
7. **Mira** la columna **Tipo de inicio**. Debe decir **Automático**.
8. **Opcional**: si no quieres que arranque con Windows, haz clic derecho → **Propiedades** → cambia el tipo de inicio.

🖼️ **[Imagen: Panel Servicios de Windows con la fila "postgresql-x64-18" resaltada, Estado = En ejecución y Tipo de inicio = Automático.]**

```prompt
Ilustración estilo captura de pantalla del panel "Servicios" de Windows, formato 16:9, tabla con columnas "Nombre", "Descripción", "Estado", "Tipo de inicio". Varias filas de servicios genéricos en gris y una fila resaltada con fondo celeste claro: nombre "postgresql-x64-18", descripción "PostgreSQL Server 18", Estado "En ejecución", Tipo de inicio "Automático". Un recuadro rojo delgado rodea la fila resaltada y una flecha roja apunta a la celda "En ejecución". Nota lateral en español: "Si está detenido: clic derecho > Iniciar". Interfaz clara y realista, tipografía del sistema, sin logos oficiales.
```

✅ **Checkpoint:** servicio **En ejecución** + inicio **Automático**. Ya puedes conectarte.

> 💡 **Pro Tip:** "En ejecución" significa que el motor trabaja en segundo plano aunque no abras pgAdmin ni la consola, e incluso después de reiniciar el PC.

### 1.6 Reconoce los directorios importantes

1. **Abre** el Explorador de archivos.
2. **Navega** a `C:\Program Files\PostgreSQL\`.
3. **Entra** en la carpeta de tu versión (`18`, o `16`/`17` según lo instalado).
4. **Identifica** las carpetas de la tabla siguiente.

| Carpeta / archivo | Contenido | ¿Se toca? |
|---|---|---|
| `bin` | Ejecutables: `postgres.exe` (motor), `psql.exe` (consola), `pg_dump` (backup), `pg_restore` (restaurar), `initdb` (crear clúster). | No |
| `data` | **El corazón**: datos, configuración y logs. | Con extremo cuidado |
| `data\postgresql.conf` | Configuración del servidor: puerto, memoria, logs, conexiones. | Solo si sabes qué haces |
| `data\pg_hba.conf` | Control de acceso: quién, desde dónde, a qué BD, con qué método. | Solo si sabes qué haces |
| `data\base` | Datos reales de las bases de datos, organizados por IDs internos. | **Nunca** |
| `lib` | Librerías internas y desarrollo avanzado. | No |
| `share` | Archivos SQL, plantillas y extensiones. | Rara vez |

> ⚠️ **Alerta:** dentro de `data` no borres ni muevas nada manualmente. Los nombres internos son IDs, no nombres legibles.

> 💡 **Pro Tip (regla de oro del instructor):** antes de editar cualquier archivo de configuración: **1)** detén el servicio, **2)** copia el archivo y pégalo en la misma carpeta como respaldo, **3)** edita el original. Si algo se rompe, borra el original dañado y renombra la copia con el nombre original.

📝 **Nota del Editor:** las rutas mostradas son de Windows. En Linux y macOS la estructura de directorios cambia considerablemente; verifica las rutas de tu plataforma en la documentación oficial.

### 1.7 Tu primer acceso con psql

`psql` **no es la base de datos**: es el puente entre tú y el servidor. Sirve para conectarte, crear y borrar bases de datos, crear tablas, ejecutar consultas, administrar usuarios y permisos, y probar consultas rápido. Es liviano y muy usado en producción.

1. **Abre** el menú Inicio de Windows.
2. **Escribe** `psql`.
3. **Pulsa Enter** sobre la aplicación **SQL Shell (psql)**. Se abre una ventana de terminal.
4. **Server [localhost]:** escribe `localhost` o pulsa **Enter** para aceptar el valor entre corchetes.
5. **Database [postgres]:** escribe `postgres` o pulsa **Enter**.
6. **Port [5432]:** pulsa **Enter**.
7. **Username [postgres]:** pulsa **Enter**.
8. **Password:** escribe la contraseña que definiste en la instalación y pulsa **Enter**.
9. **Observa** el prompt: `postgres=#`. Ese símbolo significa que **ya estás dentro del servidor**.

> 💡 **Pro Tip:** al escribir la contraseña **no aparece nada en pantalla**, ni asteriscos. No es un fallo: se oculta para que nadie la vea. Escribe con normalidad y pulsa Enter.

🖼️ **[Imagen: Terminal SQL Shell (psql) mostrando los cinco prompts de conexión y el prompt final postgres=# con una anotación sobre la contraseña invisible.]**

```prompt
Ilustración estilo captura de pantalla de una terminal de Windows con fondo negro y texto verde claro, formato 16:9. Líneas exactas en orden: "Server [localhost]:", "Database [postgres]:", "Port [5432]:", "Username [postgres]:", "Password for user postgres:", una línea en blanco y luego "postgres=#" con cursor de bloque. A la derecha, un globo de nota amarillo con texto en español: "La contraseña no se muestra al escribir. Es normal." Debajo del prompt, otra nota: "postgres=# significa que ya estás dentro del servidor". Estilo terminal realista, tipografía monoespaciada.
```

**Metacomandos básicos de psql** (empiezan con barra invertida `\`)

| Comando | Qué hace |
|---|---|
| `\h` | Ayuda general de comandos SQL. |
| `\?` | Ayuda de todos los metacomandos de psql. |
| `\l` | Lista las bases de datos disponibles. |
| `\dt` | Lista las tablas de la base de datos actual. |
| `\d` | Muestra estructuras / detalles de un objeto. |
| `\c` | Cambia o conecta a otra base de datos. |
| `\q` | Sale de psql. |

10. **Prueba** `\h` y pulsa Enter. Revisa la ayuda.
11. **Prueba** `\?` y pulsa Enter. Revisa los metacomandos.
12. **Prueba** `\l` y pulsa Enter. Verás bases de datos propias del sistema, aún no las tuyas.
13. **Prueba** crear una base de datos escribiendo:

```sql
CREATE DATABASE prueba;
```

14. **Pulsa Enter.** ⚠️ El punto y coma final es obligatorio en psql.
15. **Sal** con `\q` y pulsa Enter. Luego presiona una tecla para cerrar la ventana.

> 📝 **Nota del Editor:** en el video el instructor escribe este comando solo como demostración y luego crea la base de datos `prueba` desde pgAdmin. Si ya la creaste aquí, en la Fase 2 omite el paso de creación y continúa.

✅ **Checkpoint de Fase 1:** entraste y saliste de psql sin errores. Ya sabes que el servidor está vivo.

---

## Fase 2 · Elige tu herramienta y conéctate (Módulo 3)

### 2.1 Compara los clientes disponibles

Todos hacen lo mismo: **conectarse al servidor** que ya instalaste. Elige uno y domínalo.

| Herramienta | Tipo | Costo | Fuerte en | Consideración |
|---|---|---|---|---|
| **psql** | Consola | Gratis (incluido) | Rapidez, scripts, administración | Requiere soltura con la terminal |
| **pgAdmin 4** | Gráfica oficial | Gratis (incluido) | Servidores, roles, backups, aprendizaje | Algo pesada y lenta |
| **DBeaver** | Gráfica multibase | Community gratis / Pro de pago | Conectar a PostgreSQL, MySQL, Oracle, etc. | Muy completa y estable |
| **TablePlus** | Gráfica moderna | Versión gratis limitada | Interfaz rápida y cómoda a diario | Límites en la versión gratis |
| **DataGrip** | IDE de JetBrains | De pago | Autocompletado SQL inteligente, proyectos grandes | Curva de aprendizaje y costo |

> 💡 **Pro Tip:** este manual usa **pgAdmin 4** porque ya lo instalaste. Todo lo que hagas allí también funciona en psql: el SQL es idéntico.

### 2.2 Memoriza los cinco datos de conexión

Cualquier aplicación externa (backend, frontend, servidor remoto) necesita estos datos:

| Dato | Valor típico en local | Qué significa |
|---|---|---|
| **Host** | `localhost` | IP o dominio del servidor. En un servidor remoto, será su IP. |
| **Puerto** | `5432` | Puerta de entrada. Cambiable si está ocupado. |
| **Base de datos** | `postgres` (inicial) | Nombre de la BD a la que entras. |
| **Usuario** | `postgres` | Rol con el que te autenticas. |
| **Contraseña** | La que definiste | Secreto de autenticación. |

**Si necesitas conexiones externas**, el curso indica este procedimiento conceptual:

1. **Edita** `postgresql.conf` para habilitar la escucha de conexiones externas.
2. **Autoriza** quién puede conectarse (control de acceso en `pg_hba.conf`).
3. **Reinicia** el servicio de PostgreSQL.
4. **Abre** el puerto `5432` en el firewall, si tienes firewall activo.

> 📝 **Nota del Editor:** la transcripción explica el flujo pero **no dicta los parámetros exactos** (`listen_addresses`, líneas de `pg_hba.conf`, método de autenticación). El curso se desarrolla íntegramente en local con psql y pgAdmin. Antes de exponer tu servidor a la red, verifica la sintaxis y las implicaciones de seguridad en la documentación oficial.

### 2.3 Abre pgAdmin y conéctate al servidor

1. **Abre** el menú Inicio y **escribe** `pgAdmin`.
2. **Pulsa Enter** para abrir la herramienta.
3. **Observa** el panel izquierdo: el árbol de **Servers**. En el video aparecen dos versiones instaladas (12 y 18).
4. **Haz clic** en el servidor de tu versión **18** para expandirlo.
5. **Escribe** la contraseña del superusuario cuando la solicite.
6. **Confirma**. pgAdmin ya trae el resto de los datos por defecto.
7. **Explora** el panel derecho: **Dashboard**, propiedades, SQL, estadísticas, dependencias y procesos.

> 💡 **Pro Tip:** si aparece una ventana pidiendo datos que no reconoces, pulsa **Cancel** y conéctate desde el árbol de servidores. Algunos accesos (como abrir un Query Tool suelto) sí piden todos los datos de conexión.

### 2.4 Crea tu primera base de datos: `prueba`

1. **Ubica** el nodo **Databases** dentro de tu servidor 18.
2. **Haz clic derecho** sobre **Databases**.
3. **Selecciona** **Create → Database…**.
4. **Escribe** `prueba` en el campo **Database** (nombre).
5. **Verifica** que el **Owner** sea `postgres`.
6. **Haz clic** en **Save**.
7. **Observa** la nueva base de datos `prueba` en el árbol.
8. **Haz clic** sobre `prueba` y abre la pestaña **SQL** del panel derecho.
9. **Lee** el comando `CREATE DATABASE` que pgAdmin generó por ti.

> 💡 **Pro Tip:** pgAdmin es también un profesor: cada acción gráfica muestra su SQL equivalente. Copia ese SQL en tu archivo `consultas.sql` y aprende el comando mientras usas la interfaz.

🖼️ **[Imagen: pgAdmin 4 con el diálogo "Create - Database" abierto, nombre "prueba", owner "postgres" y el botón Save resaltado.]**

```prompt
Ilustración estilo captura de pantalla de pgAdmin 4, formato 16:9, tema claro. Panel izquierdo con árbol de objetos: "Servers > PostgreSQL 18 > Databases", con las bases "postgres" y "prueba". Al centro, un diálogo modal titulado "Create - Database" con pestañas "General", "Definition", "Security"; campo "Database" con el valor "prueba" y campo "Owner" con "postgres". Botones inferiores "Close", "Reset", "Save"; el botón "Save" resaltado con un recuadro rojo y una flecha roja. Nota lateral en español: "pgAdmin muestra el SQL generado en la pestaña SQL". Interfaz limpia y realista, sin logos oficiales.
```

### 2.5 Abre el Query Tool correcto y ajusta la vista

1. **Haz clic derecho** específicamente sobre la base de datos **`prueba`**.
2. **Selecciona** **Query Tool**. Se abre una pestaña de editor SQL.
3. **Verifica** en el título de la pestaña que estás conectado a `prueba` y no a otra base de datos.
4. **Amplía** la interfaz si el texto se ve pequeño: menú **View → Zoom In**, o **Ctrl + "+"**.
5. **Guarda** este hábito: una pestaña para escribir consultas y, más adelante, otra para ver las tablas.

> ⚠️ **Alerta:** el error más común del principiante es ejecutar el script en la base de datos equivocada. El Query Tool hereda la base de datos sobre la que hiciste clic derecho.

✅ **Checkpoint de Fase 2:** tienes pgAdmin conectado y un Query Tool abierto sobre `prueba`.

---

## Fase 3 · Fundamentos: tu primera tabla con reglas (Módulo 4)

### 3.1 Anatomía de una tabla

Piensa en una hoja de cálculo:

- **Tabla** = la hoja completa. Representa una entidad: `clientes`, `pedidos`.
- **Columna** = un atributo con tipo fijo: `nombre` (texto), `fecha_alta` (fecha), `salario` (número).
- **Fila / registro / tupla** = un caso concreto con valores para todas las columnas.

🖼️ **[Imagen: Tabla "clientes" ampliada con una columna resaltada en azul y una fila resaltada en verde, con etiquetas "Columna = atributo (tipo fijo)" y "Fila = registro o tupla".]**

```prompt
Infografía vectorial plana, formato 16:9, fondo blanco. Al centro, una tabla de datos titulada "clientes" con encabezados "id", "nombre", "email" y tres filas de ejemplo: (1, Juan Pérez, juan@test.com), (2, María Gómez, maria@test.com), (3, Ana López, ana@test.com). La columna "nombre" completa resaltada en azul celeste con una etiqueta superior: "Columna = atributo con tipo de dato fijo". La segunda fila completa resaltada en verde claro con etiqueta lateral: "Fila = registro o tupla". Encabezado de tabla en azul profundo con texto blanco. Etiquetas en español, tipografía sans-serif, estilo manual corporativo.
```

### 3.2 Crea la tabla `clientes`

1. **Ve** al Query Tool de la base de datos `prueba`.
2. **Escribe** el siguiente código. Usa **tabulador** para indentar y **mayúsculas** en las palabras clave.

```sql
CREATE TABLE clientes (
    id INTEGER,
    nombre VARCHAR(100),
    email VARCHAR(150)
);
```

3. **Revisa** que haya **coma** después de cada columna excepto la última.
4. **Ejecuta** con el botón ▶ (play) o la tecla **F5**.
5. **Lee** el mensaje de éxito en el panel inferior.

> ⚠️ **Error provocado en el video:** al olvidar la coma después de `id INTEGER`, PostgreSQL responde `syntax error at or near "nombre"`. **La coma separa definiciones de columna.** Corrige y vuelve a ejecutar.

6. **Haz clic derecho** sobre `prueba` y elige **Refresh** (o F5 en el árbol).
7. **Navega** a **Schemas → public → Tables**.
8. **Confirma** que aparece `clientes`.

✅ **Checkpoint:** la tabla `clientes` existe en el árbol de objetos.

### 3.3 Inserta tus primeras filas

1. **Escribe** en el editor:

```sql
INSERT INTO clientes (id, nombre, email)
VALUES
    (1, 'Juan Pérez', 'juan@test.com'),
    (2, 'María Gómez', 'maria@test.com');
```

2. **Selecciona** con el ratón solo el bloque que quieres ejecutar.
3. **Pulsa F5**.
4. **Verifica** el mensaje: se insertaron 2 filas.

> ⚠️ **Regla de oro de las comillas (error real del video):**
> - **Comillas simples `'...'`** → valores de texto: `'Juan Pérez'`.
> - **Comillas dobles `"..."`** → nombres de columnas o tablas.
> Si usas dobles para un valor, PostgreSQL creerá que buscas una columna y fallará.

### 3.4 Lee los datos

```sql
SELECT * FROM clientes;
```

1. **Ejecuta** el bloque.
2. **Observa** el panel de resultados: columnas `id`, `nombre`, `email` y las dos filas.

> 💡 **Pro Tip:** el asterisco `*` significa "todas las columnas". En producción, pide solo las columnas que necesitas: es más rápido y más seguro.

### 3.5 Elige el tipo de dato correcto

| Familia | Tipo | Cuándo usarlo |
|---|---|---|
| **Numéricos enteros** | `SMALLINT` | Números pequeños. Rango: −32 768 a 32 767. Ideal para edades. |
| | `INTEGER` | Enteros de uso general. |
| | `BIGINT` | Enteros muy grandes. |
| **Numéricos decimales** | `NUMERIC(p,s)` | Decimales **exactos**. Ej.: `NUMERIC(10,2)` para dinero. |
| | Flotantes (`REAL`, `DOUBLE PRECISION`) | Decimales aproximados, cálculos científicos. |
| **Secuenciales** | `SERIAL` | Entero autoincremental. |
| **Texto** | `CHAR(n)` | Longitud fija; rellena con espacios. |
| | `VARCHAR(n)` | Longitud variable hasta *n* caracteres. |
| | `TEXT` | Texto sin límite práctico. Ideal para textos largos. |
| **Fecha y hora** | `DATE`, `TIME`, `TIMESTAMP` | Fecha, hora, o fecha + hora. |
| **Lógico** | `BOOLEAN` | `TRUE`, `FALSE` o `NULL`. |
| **Identificador** | `UUID` | Identificador único universal. |
| **Documentos** | `JSON` | Texto con estructura. Consultas más lentas. |
| | `JSONB` | Formato binario. **Más rápido para consultar.** |
| **Colecciones** | `ARRAY` | Lista de valores del mismo tipo en una columna. |
| **Personalizado** | `ENUM` | Lista fija de valores permitidos (ej.: estados). |

### 3.6 Claves primarias: el DNI de cada fila

Una **clave primaria (PK)** identifica cada fila de forma única: **no se repite** y **no acepta NULL**.

**¿Por qué no usar el nombre o el email como identidad?** Porque los nombres se repiten (hay muchos Juan) y los emails pueden cambiar. Un `id` numérico es estable y simple.

**Opción A — la clásica con `SERIAL`:**

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```

1. **Ejecuta** el bloque.
2. **Comprueba** el resultado con:

```sql
SELECT * FROM usuarios;
```

3. **Observa** que la columna `id` aparece marcada como **PK integer**.

| Restricción | Traducción práctica |
|---|---|
| `SERIAL` | Entero que se incrementa solo: 1, 2, 3, 4… |
| `PRIMARY KEY` | Identidad única, no nula. |
| `NOT NULL` | El campo es obligatorio. |
| `UNIQUE` | No se permiten dos valores iguales (ej.: dos emails idénticos). |

**Opción B — la moderna con `IDENTITY`:**

```sql
CREATE TABLE productos (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nombre TEXT NOT NULL
);
```

1. **Ejecuta** el bloque.
2. **Entiende** la diferencia: `GENERATED ALWAYS AS IDENTITY` es la forma **moderna y estándar** de generar el valor automático. Reemplaza a `SERIAL` en versiones recientes.

### 3.7 Claves foráneas: el puente entre tablas

Una **clave foránea (FK)** apunta a la PK de **otra** tabla. Garantiza tres cosas:

- El valor referenciado **debe existir**.
- Se mantiene la **integridad referencial**.
- No quedan **datos huérfanos** (por ejemplo, un pago sin cliente).

**Analogía:** en la tabla `pagos` no guardas todos los datos del cliente. Guardas solo su `id`. Con ese `id` ya puedes recuperar su nombre y todo lo demás.

🖼️ **[Imagen: Diagrama entidad-relación con la tabla "usuarios" (id PK) conectada por una flecha a "pedidos" (usuario_id FK), con la etiqueta "REFERENCES usuarios(id)".]**

```prompt
Diagrama entidad-relación vectorial plano, formato 16:9, fondo blanco, paleta azul profundo y celeste. Dos recuadros de tabla: izquierda "usuarios" con filas "id — SERIAL — PK" (con icono de llave dorada), "nombre — TEXT — NOT NULL", "email — TEXT — UNIQUE"; derecha "pedidos" con filas "id — SERIAL — PK", "total — NUMERIC(10,2)", "usuario_id — INT — FK" (con icono de eslabón). Flecha gruesa desde "usuario_id" de pedidos hasta "id" de usuarios, etiquetada "FOREIGN KEY · REFERENCES usuarios(id)". Cardinalidad marcada "1" del lado usuarios y "N" del lado pedidos. Nota inferior en español: "Un usuario puede tener muchos pedidos. Cada pedido pertenece a un solo usuario." Estilo manual corporativo, sin fotografía.
```

**Construye la tabla `pedidos` con FK nombrada:**

```sql
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    total NUMERIC(10,2) NOT NULL,
    usuario_id INT NOT NULL,
    CONSTRAINT fk_usuario
        FOREIGN KEY (usuario_id)
        REFERENCES usuarios (id)
);
```

1. **Escribe** el código completo, incluida la coma después de `usuario_id INT NOT NULL`.
2. **Ejecuta** con F5.
3. **Refresca** el árbol: **Schemas → public → Tables**.
4. **Verifica** que existe `pedidos`.
5. **Comprueba** con `SELECT * FROM pedidos;`.

**Desglose de cada pieza:**

| Pieza | Función |
|---|---|
| `CONSTRAINT fk_usuario` | Da **nombre explícito** a la regla. Facilita leer errores y modificarla después. |
| `FOREIGN KEY (usuario_id)` | Declara qué columna de esta tabla será la referencia. |
| `REFERENCES usuarios (id)` | Indica la tabla y columna destino de la referencia. |
| `INT` en `usuario_id` | **Debe coincidir** con el tipo de la columna referenciada. Si allá es entero, aquí también. |

> ⚠️ **Error real del video:** faltó una coma antes de `CONSTRAINT` y PostgreSQL lanzó un error de sintaxis. Revisa las comas antes de ejecutar.

✅ **Checkpoint de Fase 3:** tienes `clientes`, `usuarios`, `productos` y `pedidos`, con PK y FK activas.

---

## Fase 4 · SQL básico: el CRUD completo (Módulo 5)

**SQL** = *Structured Query Language*. No es un lenguaje de programación completo como Python o Java: es un lenguaje **especializado** para consultar, modificar y administrar datos.

### 4.1 Crea la base de datos `tienda_online`

1. **Haz clic derecho** en **Databases** → **Create → Database…**.
2. **Escribe** el nombre `tienda_online`.
3. **Deja** por defecto owner, codificación, ordenación de textos y plantilla base.
4. **Pulsa Save** (o Create).
5. **Haz clic derecho** sobre `tienda_online` → **Query Tool**.

> 💡 **Pro Tip:** el diálogo permite configurar dueño, codificación de caracteres, ordenación y plantilla. Para aprender, los valores por defecto son correctos.

### 4.2 Crea la tabla `usuarios` con todos los adornos

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    edad SMALLINT,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

1. **Escribe** el bloque y **ejecútalo**.
2. **Entiende** cada decisión de diseño:

| Columna | Decisión | Razón |
|---|---|---|
| `id` | `SERIAL PRIMARY KEY` | Se numera solo y es único. |
| `nombre` | `VARCHAR(100) NOT NULL` | 100 caracteres sobran para nombres; es obligatorio. |
| `email` | `VARCHAR(150) UNIQUE NOT NULL` | Caben correos largos; no se permiten duplicados. |
| `edad` | `SMALLINT` | Rango −32 768 a 32 767: perfecto para edades. |
| `creado_en` | `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` | Registra fecha y hora automáticamente al insertar. |

### 4.3 Inserta una fila

```sql
INSERT INTO usuarios (nombre, email, edad)
VALUES ('Ana López', 'ana@mail.com', 35);
```

1. **Ejecuta** el bloque.
2. **Nota** que **no envías** `id` ni `creado_en`: se llenan solos.
3. **Respeta** el orden: los valores deben ir en el mismo orden que las columnas listadas.

### 4.4 Inserta varias filas de una vez

```sql
INSERT INTO usuarios (nombre, email, edad)
VALUES
    ('Ana López',   'ana2@mail.com',    35),
    ('Carlos Ruiz', 'carlos@gmail.com', 20),
    ('Pedro Sánchez','pedro@mail.com',  50);
```

1. **Ejecuta** el bloque.
2. **Observa** el mensaje: 3 filas insertadas.

> ⚠️ **Error real del video:** al repetir `ana@mail.com` (ya existente), PostgreSQL rechaza **todo** el `INSERT` por la restricción `UNIQUE`. Solución aplicada: cambiar el correo a `ana2@mail.com`.
> 📝 **Nota del Editor:** como el video ejecuta varias inserciones de prueba, **tus valores de `id` pueden diferir**. Antes de actualizar o borrar por `id`, ejecuta un `SELECT` y confirma el número real.

### 4.5 Lee datos con `SELECT`

```sql
-- 1) Todo
SELECT * FROM usuarios;

-- 2) Solo algunas columnas
SELECT nombre, email FROM usuarios;

-- 3) Con filtro
SELECT * FROM usuarios WHERE edad > 25;

-- 4) Ordenado de mayor a menor
SELECT nombre, edad FROM usuarios ORDER BY edad DESC;

-- 5) Limitado a 2 filas
SELECT * FROM usuarios LIMIT 2;
```

1. **Ejecuta** cada consulta por separado, seleccionándola antes de pulsar F5.
2. **Compara** los resultados: el filtro `edad > 25` devuelve Ana y Pedro; el orden `DESC` muestra 50, 35, 20.

### 4.6 Actualiza con `UPDATE`

```sql
UPDATE usuarios
SET email  = 'ana.nuevo@gmail.com',
    edad   = 29,
    nombre = 'Liset Torres'
WHERE id = 3;
```

1. **Verifica** primero con `SELECT * FROM usuarios;` qué `id` quieres cambiar.
2. **Ejecuta** el `UPDATE`.
3. **Confirma** con otro `SELECT`.

**Actualización masiva calculada:**

```sql
UPDATE usuarios
SET edad = edad + 1;
```

> ⚠️ **Alerta crítica:** sin `WHERE`, el `UPDATE` afecta **todas las filas de la tabla**. En el video se usa a propósito para sumar un año a todos. En producción, escribe siempre el `WHERE` primero.

> 💡 **Pro Tip:** antes de un `UPDATE` o `DELETE` masivo, ejecuta el mismo `WHERE` dentro de un `SELECT`. Si el `SELECT` devuelve exactamente las filas que quieres tocar, el `WHERE` es correcto.

### 4.7 Elimina con `DELETE`

```sql
-- Por identificador (lo más seguro)
DELETE FROM usuarios WHERE id = 3;

-- Por condición
DELETE FROM usuarios WHERE edad > 40;

-- ⚠️ TODA la tabla
DELETE FROM usuarios;
```

1. **Ejecuta** el borrado por `id`.
2. **Comprueba** con `SELECT` que la fila desapareció.
3. **Prueba** el borrado por condición y vuelve a comprobar.
4. **Lee** la advertencia antes de probar el borrado total.

> ⚠️ **Alerta:** `DELETE FROM usuarios;` sin `WHERE` vacía la tabla. Es una operación destructiva. Practica solo en bases de datos de prueba.

### 4.8 Crea la tabla `productos` de la tienda

```sql
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    precio NUMERIC(10,2) NOT NULL,
    categoria VARCHAR(50),
    stock INTEGER DEFAULT 0
);
```

1. **Ejecuta** el bloque en el Query Tool de `tienda_online`.
2. **Carga** los datos de práctica:

```sql
INSERT INTO productos (nombre, precio, categoria, stock)
VALUES
    ('Laptop Gamer',          1200.00, 'Electrónica', 15),
    ('Mouse Óptico',            20.50, 'Electrónica', 150),
    ('Camiseta de Algodón',     18.90, 'Ropa',        200),
    ('Auriculares Bluetooth',   89.99, 'Electrónica', 50),
    ('Pantalones',              45.00, 'Ropa',        80);
```

3. **Ejecuta** y **verifica** con `SELECT * FROM productos;`.

> ⚠️ **Error real del video:** se escribió mal el nombre de una columna (`stock`). PostgreSQL respondió que la columna no existe. **Los nombres deben coincidir carácter por carácter** con los definidos en `CREATE TABLE`.

### 4.9 Filtra con `WHERE`, `AND` y `OR`

```sql
-- Una condición
SELECT * FROM productos WHERE categoria = 'Electrónica';

-- Proyección + filtro numérico
SELECT nombre, precio FROM productos WHERE precio > 50.00;

-- AND: se deben cumplir AMBAS condiciones
SELECT * FROM productos
WHERE categoria = 'Ropa'
  AND precio > 20;

-- OR: basta con UNA condición
SELECT * FROM productos
WHERE categoria = 'Electrónica'
   OR precio >= 1000;
```

| Operador | Lógica | Analogía del video |
|---|---|---|
| `AND` | Verdadero solo si **ambas** condiciones son verdaderas | "Llueve **y** hace frío": deben pasar las dos. |
| `OR` | Verdadero si **al menos una** es verdadera | "Llueve **o** hace frío": con una basta. |
| `NOT` | Invierte el resultado | Negar un falso da verdadero. |

### 4.10 Ordena con `ORDER BY`

```sql
-- De menor a mayor
SELECT nombre, precio FROM productos ORDER BY precio ASC;

-- De mayor a menor
SELECT nombre, precio FROM productos ORDER BY precio DESC;

-- Doble criterio: categoría ascendente y, dentro de cada una, precio descendente
SELECT nombre, categoria, precio
FROM productos
ORDER BY categoria ASC, precio DESC;
```

1. **Ejecuta** cada variante.
2. **Observa** cómo el segundo criterio solo desempata dentro del primer grupo.
3. **No olvides** la coma entre criterios de ordenamiento.

### 4.11 Pagina con `LIMIT` y `OFFSET`

```sql
-- Los 3 productos más baratos
SELECT nombre, precio
FROM productos
ORDER BY precio ASC
LIMIT 3;

-- Los 3 siguientes (página 2)
SELECT nombre, precio
FROM productos
ORDER BY precio ASC
LIMIT 3 OFFSET 3;
```

| Cláusula | Qué hace | Resultado en el ejemplo |
|---|---|---|
| `LIMIT 3` | Devuelve como máximo 3 filas | Camiseta 18.90, Mouse 20.50, Pantalones 45.00 |
| `OFFSET 3` | **Salta** las 3 primeras filas | Auriculares 89.99 y Laptop 1200.00 |

> 💡 **Pro Tip:** la paginación de una tienda web se construye así: `LIMIT = tamaño de página`, `OFFSET = (página − 1) × tamaño de página`. Siempre combina con `ORDER BY`, o el orden será impredecible.

### 4.12 Renombra resultados con alias `AS`

```sql
SELECT nombre AS producto,
       precio AS costo,
       stock  AS cantidad_en_stock
FROM productos;
```

1. **Ejecuta** la consulta.
2. **Observa** los encabezados nuevos en el resultado.
3. **Comprueba** con `SELECT * FROM productos;` que **la tabla real no cambió**.

> 💡 **Pro Tip:** el alias existe **solo en el resultado**. Es ideal para reportes legibles. Usa `_` en lugar de espacios para evitar complicaciones al reutilizar el alias en `ORDER BY`.

✅ **Checkpoint de Fase 4:** tienes `tienda_online` con `usuarios` y `productos`, y dominas las cuatro operaciones CRUD.

---

## Fase 5 · Consultas con superpoderes (Módulo 6)

### 5.1 Operadores de comparación y lógicos

Un operador compara valores y devuelve un **booleano** (`TRUE` / `FALSE`). Con ese resultado, `WHERE` decide qué filas muestra.

| Operador | Significado | Ejemplo |
|---|---|---|
| `=` | Igual | `total = 100` |
| `>` `<` | Mayor / menor | `precio > 50` |
| `>=` `<=` | Mayor o igual / menor o igual | `precio >= 50.00` |
| `<>` | Diferente | `categoria <> 'Ropa'` |
| `AND` `OR` `NOT` | Y / O / Negación | `a AND b` |
| `BETWEEN x AND y` | Rango inclusivo | `precio BETWEEN 20 AND 100` |
| `IN (...)` | Pertenece a una lista | `categoria IN ('Electrónica','Ropa')` |
| `LIKE` | Patrón de texto, **sensible** a mayúsculas | `nombre LIKE 'A%'` |
| `ILIKE` | Patrón de texto, **insensible** a mayúsculas | `nombre ILIKE '%gamer%'` |

**El comodín `%`** representa "cualquier cantidad de caracteres":

- `'A%'` → empieza con A.
- `'%gamer%'` → contiene "gamer" en cualquier posición.

### 5.2 Crea la tabla `ventas`

```sql
CREATE TABLE ventas (
    id SERIAL PRIMARY KEY,
    producto_id INTEGER NOT NULL,
    cantidad INTEGER NOT NULL,
    fecha DATE DEFAULT CURRENT_DATE,
    total NUMERIC(10,2) NOT NULL
);
```

1. **Ejecuta** el bloque en `tienda_online`.
2. **Entiende** el diseño: `producto_id` guarda **qué** se vendió, `cantidad` **cuánto**, `fecha` **cuándo** (automática por defecto) y `total` el dinero de la venta.
3. **Carga** los datos de práctica:

```sql
INSERT INTO ventas (producto_id, cantidad, fecha, total)
VALUES
    (1,  2, '2026-01-20', 2400.00),
    (2, 10, '2026-01-21',  255.00),
    (3,  5, '2026-01-22',   94.00),
    (1,  1, '2026-01-23', 1200.00),
    (4,  3, '2026-01-24',  265.00),
    (3,  8, '2026-01-25', 3000.00),
    (1,  9, '2026-01-26',  151.00);
```

4. **Verifica** con `SELECT * FROM ventas;`.

> 📝 **Nota del Editor:** el instructor dicta estos importes "inventados" en vivo, por lo que **no siempre equivalen a precio × cantidad**. Se conservan tal como se usan en el video para que tus resultados coincidan con los del curso (ingresos totales = 7 365). En un sistema real, el total se calcularía a partir del precio y la cantidad.

### 5.3 Ejercita los filtros combinados

```sql
-- Comparación + AND + patrón de texto
SELECT nombre, precio, categoria
FROM productos
WHERE precio >= 50.00
  AND categoria = 'Electrónica'
   OR nombre ILIKE '%gamer%';

-- Rango
SELECT nombre, precio
FROM productos
WHERE precio BETWEEN 20.00 AND 100.00;

-- Lista de valores
SELECT nombre, categoria
FROM productos
WHERE categoria IN ('Electrónica', 'Ropa');

-- Texto que empieza con A, sin importar mayúsculas
SELECT nombre
FROM productos
WHERE nombre ILIKE 'A%';
```

1. **Ejecuta** una consulta a la vez.
2. **Anota** cuántas filas devuelve cada una.
3. **Experimenta**: cambia `AND` por `OR` y observa cómo cambia el resultado.

> 💡 **Pro Tip:** cuando mezclas `AND` y `OR`, usa **paréntesis** para dejar clara tu intención. Sin paréntesis, el orden de evaluación puede sorprenderte.

### 5.4 Funciones agregadas: de filas a resúmenes

Una función agregada toma **muchas filas** y devuelve **un solo valor**.

| Función | Qué calcula |
|---|---|
| `COUNT(*)` | Cuántas filas hay |
| `SUM(col)` | Suma de los valores numéricos |
| `AVG(col)` | Promedio |
| `MIN(col)` | Valor mínimo |
| `MAX(col)` | Valor máximo |

```sql
-- ¿Cuántos productos tengo?
SELECT COUNT(*) AS total_productos FROM productos;              -- 5

-- ¿Cuántos son de ropa?
SELECT COUNT(*) AS total_ropa
FROM productos
WHERE categoria = 'Ropa';                                       -- 2

-- ¿Cuánto dinero ingresó por ventas?
SELECT SUM(total) AS ingresos_totales FROM ventas;              -- 7365.00

-- ¿Cuál es el precio promedio?
SELECT AVG(precio) AS precio_promedio FROM productos;           -- ≈ 274.88

-- Extremos de precio
SELECT MIN(precio) AS precio_mas_bajo,
       MAX(precio) AS precio_mas_alto
FROM productos;                                                 -- 18.90 y 1200.00
```

> 💡 **Pro Tip del instructor:** antes de agregar, ejecuta `SELECT * FROM tabla;` y mira los datos crudos. Así sabrás si el resumen tiene sentido.

### 5.5 Agrupa con `GROUP BY`

`GROUP BY` junta las filas que comparten un valor y calcula un resumen **por cada grupo**.

```sql
SELECT producto_id,
       COUNT(*)      AS cantidad_ventas,
       SUM(cantidad) AS unidades_vendidas,
       SUM(total)    AS ingreso_por_producto
FROM ventas
GROUP BY producto_id
ORDER BY ingreso_por_producto DESC;
```

**Resultado esperado con los datos de práctica:**

| producto_id | cantidad_ventas | unidades_vendidas | ingreso_por_producto |
|---|---|---|---|
| 1 | 3 | 12 | 3751.00 |
| 3 | 2 | 13 | 3094.00 |
| 4 | 1 | 3 | 265.00 |
| 2 | 1 | 10 | 255.00 |

> ⚠️ **Error real del video:** el alias escrito en `ORDER BY` no coincidía exactamente con el definido en `SELECT`. **El nombre debe ser idéntico**, incluidos guiones bajos. Sin `GROUP BY`, verías productos repetidos sin resumir.

### 5.6 Filtra grupos con `HAVING`

`HAVING` filtra **grupos**, no filas. Siempre va **después** de `GROUP BY`.

```sql
SELECT producto_id,
       SUM(total) AS ingresos
FROM ventas
GROUP BY producto_id
HAVING SUM(total) > 1000
ORDER BY ingresos DESC;
```

**Resultado:** productos 1 (3751.00) y 3 (3094.00).

| Cláusula | Cuándo actúa | Sobre qué actúa |
|---|---|---|
| `WHERE` | Antes de agrupar | Filas individuales |
| `HAVING` | Después de agrupar | Grupos ya calculados |

### 5.7 Elimina duplicados con `DISTINCT`

```sql
SELECT DISTINCT categoria FROM productos;
```

1. **Ejecuta** la consulta.
2. **Observa**: solo dos filas, `Electrónica` y `Ropa`.
3. **Compara** con `SELECT categoria FROM productos;` (5 filas repetidas).

> 💡 **Pro Tip del instructor:** abre **dos Query Tools**: uno para escribir consultas y otro llamado "Tablas" para inspeccionar los datos crudos. Renombra las pestañas con clic derecho. Verás la causa y el efecto a la vez.

### 5.8 Subconsultas: una consulta dentro de otra

**Problema:** quieres el **nombre** del producto más caro. `MAX(precio)` te da el precio, pero no el nombre.

**Solución:** anidar consultas.

```sql
SELECT nombre, precio
FROM productos
WHERE precio = (SELECT MAX(precio) FROM productos);
```

1. **Ejecuta** la consulta.
2. **Resultado:** `Laptop Gamer`, `1200.00`.
3. **Entiende** el orden: primero se resuelve el paréntesis interno, luego el filtro externo.

> ⚠️ **Buena práctica:** el curso advierte que anidar muchas subconsultas no es recomendable. Úsalas cuando aporten claridad; con una o dos suele bastar.

### 5.9 Clasifica con `CASE WHEN`

`CASE` es el "if" de SQL: crea una columna cuyo valor depende de otra columna.

```sql
SELECT nombre,
       precio,
       CASE
           WHEN precio >= 1000 THEN 'Premium'
           WHEN precio >= 100  THEN 'Alto'
           WHEN precio >= 50   THEN 'Medio'
           ELSE 'Económico'
       END AS categoria_precio
FROM productos
ORDER BY precio DESC;
```

| Pieza | Función |
|---|---|
| `CASE` | Abre la lógica condicional |
| `WHEN ... THEN ...` | Condición y valor si se cumple |
| `ELSE` | Valor por defecto si nada se cumple |
| `END` | Cierra la lógica |
| `AS categoria_precio` | Nombra la columna calculada |

**Resultado esperado:** Laptop → Premium; Auriculares (89.99) → Medio; Pantalones, Mouse y Camiseta → Económico.

> 💡 **Pro Tip:** `CASE` evalúa en orden y se detiene en la **primera** condición verdadera. Ordena de la más restrictiva a la más amplia, como en el ejemplo.

🖼️ **[Imagen: Diagrama de flujo vertical de un CASE WHEN con cuatro rombos de decisión (≥1000, ≥100, ≥50, ELSE) y sus salidas Premium, Alto, Medio, Económico.]**

```prompt
Diagrama de flujo vectorial plano, formato vertical 3:4, fondo blanco, paleta azul profundo, celeste y gris. Arriba una caja "precio del producto". Debajo, tres rombos de decisión apilados con las etiquetas "¿precio >= 1000?", "¿precio >= 100?", "¿precio >= 50?", cada uno con flecha "Sí" hacia la derecha a una etiqueta de resultado ("Premium", "Alto", "Medio") y flecha "No" hacia abajo al siguiente rombo. Al final, una caja "ELSE" con flecha a "Económico". Título superior: "CASE WHEN: clasificar productos por precio". Nota inferior: "Se detiene en la primera condición verdadera". Tipografía sans-serif clara, estilo manual corporativo.
```

✅ **Checkpoint de Fase 5:** puedes producir un reporte de ventas por producto, filtrar los más rentables y clasificar productos por precio.

---

## Fase 6 · Relaciones y JOINs (Módulo 7)

### 6.1 Los tres tipos de relación

| Tipo | Significado | Ejemplo del curso | Cómo se implementa |
|---|---|---|---|
| **1:1** | Una fila de A se relaciona con exactamente una de B | Una persona tiene un pasaporte | FK + `UNIQUE` en la tabla hija |
| **1:N** | Una fila de A se relaciona con muchas de B; cada B pertenece a una A | Un cliente hace muchos pedidos | FK en la tabla "muchos" |
| **N:N** | Muchas filas de A con muchas de B y viceversa | Estudiantes ↔ Asignaturas | **Tabla intermedia** (pivote) con dos FK |

🖼️ **[Imagen: Tres diagramas ER lado a lado mostrando 1:1 (personas–pasaportes), 1:N (clientes–pedidos) y N:N (estudiantes–inscripciones–asignaturas).]**

```prompt
Infografía vectorial plana, formato 16:9, fondo blanco, paleta azul profundo y celeste. Tres paneles separados por líneas verticales suaves, cada uno con título en español. Panel 1 "Relación 1:1": cajas "personas (id PK)" y "pasaportes (id PK, persona_id FK UNIQUE)" unidas por una línea con "1" en ambos extremos. Panel 2 "Relación 1:N": cajas "clientes (id PK)" y "pedidos (id PK, cliente_id FK)" unidas por línea con "1" a la izquierda y "N" a la derecha. Panel 3 "Relación N:N": cajas "estudiantes (id PK)", "inscripciones (PK compuesta: estudiante_id + asignatura_id)" al centro y "asignaturas (id PK)", unidas con "N" y "N" en los extremos y la etiqueta "tabla intermedia o pivote" bajo la caja central. Iconos de llave para PK y eslabón para FK. Estilo manual corporativo, tipografía sans-serif.
```

### 6.2 Crea la base de datos `relaciones`

1. **Clic derecho** en **Databases** → **Create → Database…**.
2. **Escribe** `relaciones`. **Pulsa Save**.
3. **Clic derecho** sobre `relaciones` → **Query Tool**.
4. **Opcional:** abre un segundo Query Tool para inspeccionar datos.

### 6.3 Modela la relación 1:N

```sql
CREATE TABLE clientes (
    id SERIAL PRIMARY KEY
);

CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes (id)
);
```

1. **Ejecuta** primero `clientes` (la tabla padre).
2. **Ejecuta** después `pedidos` (la tabla hija).
3. **Entiende** el orden: no puedes referenciar una tabla que aún no existe.

### 6.4 Modela la relación 1:1

```sql
CREATE TABLE personas (
    id SERIAL PRIMARY KEY
);

CREATE TABLE pasaportes (
    id SERIAL PRIMARY KEY,
    persona_id INTEGER UNIQUE NOT NULL,
    FOREIGN KEY (persona_id) REFERENCES personas (id)
);
```

1. **Ejecuta** ambos bloques en orden.
2. **Clave del truco:** el `UNIQUE` sobre `persona_id` impide que la misma persona aparezca en dos pasaportes. **Eso convierte la relación en 1:1.**

### 6.5 Modela la relación N:N

```sql
CREATE TABLE estudiantes (
    id SERIAL PRIMARY KEY
);

CREATE TABLE asignaturas (
    id SERIAL PRIMARY KEY
);

CREATE TABLE inscripciones (
    estudiante_id INTEGER NOT NULL,
    asignatura_id INTEGER NOT NULL,
    PRIMARY KEY (estudiante_id, asignatura_id),
    FOREIGN KEY (estudiante_id) REFERENCES estudiantes (id),
    FOREIGN KEY (asignatura_id) REFERENCES asignaturas (id)
);
```

1. **Ejecuta** las tres sentencias en orden.
2. **Observa** la **clave primaria compuesta**: la pareja (estudiante, asignatura) no puede repetirse. Evita inscribir dos veces al mismo alumno en la misma materia.
3. **Revisa** las comas: la última definición **no lleva coma** antes del paréntesis de cierre.

> ⚠️ **Error real del video:** una coma sobrante al final de la última línea provocó un error de sintaxis. Regla: **coma entre definiciones, nunca después de la última**.

### 6.6 Carga datos para los JOIN

```sql
INSERT INTO clientes (id) VALUES (1), (2), (3);

INSERT INTO pedidos (cliente_id) VALUES (1), (2), (3), (1), (1), (3);
```

1. **Ejecuta** ambas sentencias.
2. **Verifica** con `SELECT * FROM clientes;` y `SELECT * FROM pedidos;`.
3. **Comprueba** la distribución: cliente 1 → pedidos 1, 4 y 5; cliente 2 → pedido 2; cliente 3 → pedidos 3 y 6.

> 📝 **Nota del Editor:** el video inserta los `id` de clientes de forma manual en una columna `SERIAL`. Verifica el comportamiento de la secuencia antes de mezclar inserciones manuales y automáticas en un mismo proyecto, para no generar choques de identificadores.

### 6.7 Qué es un JOIN y por qué lo necesitas

Un `JOIN` **combina datos de dos o más tablas** usando las relaciones que creaste con claves foráneas.

**Sin JOIN:** si consultas solo `pedidos`, ves `cliente_id = 1` pero no sabes cómo se llama ese cliente. **Con JOIN:** obtienes el nombre y el pedido en la misma fila.

| Tipo de JOIN | Qué devuelve | Qué hace si no hay coincidencia |
|---|---|---|
| `INNER JOIN` | Solo las filas con pareja en **ambas** tablas | Descarta la fila |
| `LEFT JOIN` | **Todas** las de la izquierda + coincidencias de la derecha | Rellena con `NULL` a la derecha |
| `RIGHT JOIN` | **Todas** las de la derecha + coincidencias de la izquierda | Rellena con `NULL` a la izquierda |
| `FULL JOIN` | **Todas** las de ambas tablas | Rellena con `NULL` en el lado faltante |

🖼️ **[Imagen: Cuatro diagramas de Venn en fila, uno por cada JOIN, con la zona resultante sombreada en azul y una mini tabla de resultado debajo de cada uno.]**

```prompt
Infografía vectorial plana, formato 16:9, fondo blanco, paleta azul profundo y celeste. Cuatro paneles iguales en fila, cada uno con dos círculos superpuestos etiquetados "clientes (izquierda)" y "pedidos (derecha)". Panel 1 "INNER JOIN": solo la intersección sombreada en azul. Panel 2 "LEFT JOIN": todo el círculo izquierdo sombreado, incluido el solapamiento. Panel 3 "RIGHT JOIN": todo el círculo derecho sombreado. Panel 4 "FULL JOIN": ambos círculos completamente sombreados. Debajo de cada diagrama, una mini tabla de 3 columnas con filas de ejemplo y celdas "NULL" en gris claro donde corresponda. Leyenda inferior: "NULL = no hay coincidencia en la otra tabla". Tipografía sans-serif, estilo manual corporativo, sin fotografía.
```

### 6.8 Ejecuta los cuatro JOIN

```sql
-- INNER JOIN: solo clientes con pedidos
SELECT clientes.id AS cliente_id,
       pedidos.id  AS pedido_id
FROM clientes
INNER JOIN pedidos ON clientes.id = pedidos.cliente_id;

-- LEFT JOIN: todos los clientes, tengan o no pedidos
SELECT clientes.id AS cliente_id,
       pedidos.id  AS pedido_id
FROM clientes
LEFT JOIN pedidos ON clientes.id = pedidos.cliente_id;

-- RIGHT JOIN: todos los pedidos
SELECT clientes.id AS cliente_id,
       pedidos.id  AS pedido_id
FROM clientes
RIGHT JOIN pedidos ON clientes.id = pedidos.cliente_id;

-- FULL JOIN: todo de ambas tablas
SELECT clientes.id AS cliente_id,
       pedidos.id  AS pedido_id
FROM clientes
FULL JOIN pedidos ON clientes.id = pedidos.cliente_id;
```

1. **Ejecuta** el `INNER JOIN` y **cuenta** las filas: 6.
2. **Cambia** solo la palabra `INNER` por `LEFT`, luego `RIGHT`, luego `FULL`.
3. **Compara** los resultados.

**Anatomía de la sentencia:**

| Pieza | Función |
|---|---|
| `FROM clientes` | Tabla principal (la de la izquierda) |
| `INNER JOIN pedidos` | Tabla que se combina (la de la derecha) |
| `ON clientes.id = pedidos.cliente_id` | **Condición de unión**: qué debe coincidir |
| `AS cliente_id` | Alias para distinguir dos columnas llamadas `id` |

> 📝 **Nota del Editor:** con estos datos todos los pedidos tienen cliente válido, por lo que los cuatro JOIN devuelven lo mismo. Es la consecuencia esperada de la integridad referencial: la FK **impide** pedidos huérfanos. Para ver `NULL` en acción necesitas una tabla sin restricción de FK, como el ejemplo de usuarios y gustos que muestra el video.

> 💡 **Pro Tip del curso:** el sitio de diagramas visuales de JOINs que usa el instructor (tablas de usuarios y "likes") es excelente para interiorizar los cuatro tipos. Reproduce mentalmente cada caso antes de escribir la consulta.

### 6.9 Buenas prácticas con claves foráneas

1. **Nombra siempre** tus restricciones: `CONSTRAINT fk_usuario ...`. Los mensajes de error serán legibles.
2. **Elige la acción adecuada** para cuando se borra el padre:

| Escenario | Comportamiento |
|---|---|
| Por defecto | Impide borrar el padre si tiene hijos. Protege la integridad. |
| Borrado en cascada | Al borrar el padre, se borran sus hijos. |
| `ON DELETE SET NULL` | Al borrar el padre, la FK del hijo queda en `NULL`. |

3. **Recuerda** que las relaciones pueden encadenar muchas tablas. Cuantas más dependencias, más cuidado al eliminar.

> 📝 **Nota del Editor:** la transcripción describe estas acciones verbalmente pero no dicta la sintaxis completa de `ON DELETE CASCADE`. Verifica la cláusula exacta en la documentación antes de aplicarla en datos reales.

✅ **Checkpoint de Fase 6:** modelaste los tres tipos de relación y ejecutaste los cuatro JOIN sin errores.

---

## Fase 7 · Índices y rendimiento (Módulo 8)

### 7.1 Qué es un índice (analogía del libro)

Sin índice, buscar algo en un libro de 1000 páginas implica **leer página por página**. Con el índice del libro, buscas la referencia y **saltas directo** a la página 999.

Un índice en PostgreSQL es una **estructura separada** de la tabla que guarda una copia ordenada de los valores de una o varias columnas, más un **puntero** a la fila real.

| Sin índice | Con índice |
|---|---|
| Lee toda la tabla de principio a fin | Busca en una estructura pequeña y ordenada |
| Lento en tablas grandes | Salta directo al dato |

🖼️ **[Imagen: Comparación lado a lado entre "Seq Scan" (libro leído página por página) e "Index Scan" (índice del libro que apunta a la página 999).]**

```prompt
Ilustración vectorial plana comparativa, formato 16:9, fondo blanco, paleta azul profundo, celeste y rojo suave. Panel izquierdo titulado "Sin índice · Seq Scan": libro abierto con una lupa recorriendo páginas numeradas 1, 2, 3 ... 1000, flechas rojas lentas y etiqueta "Lee fila por fila". Panel derecho titulado "Con índice · Index Scan": libro cerrado con su página de índice visible, una entrada resaltada "edad 40 ......... página 999" y una flecha verde directa hacia la página 999, etiqueta "Salta directo al dato". Franja inferior con dos columnas: "Ventajas: SELECT, ORDER BY y JOIN más rápidos" y "Costos: INSERT/UPDATE/DELETE más lentos + espacio extra en disco". Tipografía sans-serif, estilo manual corporativo.
```

### 7.2 El equilibrio del rendimiento

| ✅ Ventajas | ⚠️ Costos |
|---|---|
| `SELECT` con filtro casi instantáneo | **Escritura más lenta**: cada `INSERT`, `UPDATE` o `DELETE` también actualiza el índice |
| `ORDER BY` mucho más rápido | **Espacio en disco**: en tablas grandes el índice puede pesar casi como la tabla |
| `JOIN` encuentra coincidencias rápido | Muchos índices encarecen las grabaciones |

> 💡 **Pro Tip:** indexa lo que **consultas** con frecuencia, no todo. Cada índice extra es un impuesto que pagas en cada escritura.

### 7.3 Prepara el laboratorio

1. **Crea** una base de datos llamada `db_indices` (clic derecho en Databases → Create → Database…).
2. **Abre** un Query Tool sobre ella.
3. **Crea** la tabla:

```sql
CREATE TABLE personas (
    id SERIAL,
    nombre TEXT,
    edad INTEGER
);
```

4. **Carga** diez filas:

```sql
INSERT INTO personas (nombre, edad)
VALUES
    ('Ana',    20),
    ('Luis',   25),
    ('Carlos', 30),
    ('María',  35),
    ('Pedro',  40),
    ('Sofía',  45),
    ('Juan',   50),
    ('Lucía',  55),
    ('Miguel', 60),
    ('Elena',  65);
```

> 📝 **Nota del Editor:** el video dicta nueve nombres para diez filas. Se completa el décimo registro para que el plan de ejecución muestre las 10 filas que se ven en pantalla.

### 7.4 Mide el "antes" con `EXPLAIN ANALYZE`

```sql
EXPLAIN ANALYZE
SELECT * FROM personas WHERE edad = 40;
```

1. **Ejecuta** la sentencia completa.
2. **Localiza** la línea `Seq Scan on personas`.
3. **Traduce**: *sequential scan* = PostgreSQL leyó la tabla **fila por fila**.
4. **Anota** el tiempo total mostrado (en el video, del orden de 0.02–0.04 ms).

| Prefijo | Qué hace |
|---|---|
| `EXPLAIN` | **No ejecuta** la consulta. Solo muestra el plan que PostgreSQL planea usar. |
| `EXPLAIN ANALYZE` | **Sí ejecuta** la consulta y mide tiempos reales. |

### 7.5 Crea tu primer índice

```sql
CREATE INDEX idx_personas_edad ON personas (edad);
```

| Pieza | Función |
|---|---|
| `CREATE INDEX` | Orden de crear un índice |
| `idx_personas_edad` | Nombre libre. Convención: `idx_` + tabla + columna |
| `ON personas` | Tabla donde se crea |
| `(edad)` | Columna que se indexa |

1. **Ejecuta** la sentencia.
2. **Vuelve a ejecutar** el `EXPLAIN ANALYZE` del paso anterior.
3. **Compara** los tiempos.

### 7.6 Entiende por qué sigue apareciendo `Seq Scan`

Este es el momento más importante de la fase.

**Observación del video:** tras crear el índice, el plan **sigue diciendo `Seq Scan`** y los tiempos apenas cambian.

**Explicación:** el optimizador de PostgreSQL calcula qué es más barato. Con **muy pocas filas** (10 en este laboratorio), leer la tabla completa es **más rápido** que usar el índice. Con tablas de cientos, miles o millones de filas, el índice sí se activa.

> 💡 **Pro Tip:** que el índice no se use **no significa que no exista**. Si intentas crearlo otra vez, PostgreSQL te dirá que `idx_personas_edad` ya existe. Para probar índices de verdad, necesitas volumen de datos.

> ⚠️ **Alerta:** no midas rendimiento con 10 filas. Las diferencias de milisegundos que verás son ruido, no señal.

### 7.7 Tipos de índices

| Tipo | Ideal para | Notas |
|---|---|---|
| **B-tree** | Igualdad y rangos: `=`, `>`, `<`, `>=`, `<=`, `BETWEEN` | **Tipo por defecto.** Funciona con números, texto y fechas. El más usado. |
| **Hash** | Solo igualdad `=` | Antes era más rápido; hoy B-tree es casi igual de rápido y más versátil. Poco usado. |
| **GIN** | Datos compuestos: arrays, JSON/JSONB | Indexa cada elemento interno del valor compuesto. |
| **GiST** | Geometría, rangos, búsqueda avanzada de texto | Poco común en aplicaciones básicas. |
| **BRIN** | Tablas enormes con datos ordenados físicamente (por fecha o secuencia) | Ocupa muy poco espacio, pero es menos preciso que B-tree. |

**Crear un índice de tipo específico:**

```sql
CREATE INDEX idx_personas_edad_hash
ON personas USING HASH (edad);
```

1. **Ejecuta** la sentencia.
2. **Observa** la única diferencia: la cláusula `USING <tipo>`.
3. **Experimenta** cambiando el tipo según el dato que indexas.

> 📝 **Nota del Editor:** algunos tipos requieren tipos de dato compatibles (por ejemplo, GIN con arrays o JSONB). El video no detalla esas combinaciones; verifica los operadores soportados por cada tipo antes de aplicarlo.

### 7.8 Lee un plan de ejecución como un profesional

Esta tabla es tu diccionario para interpretar `EXPLAIN ANALYZE`:

| Elemento del plan | Qué significa | Cómo usarlo |
|---|---|---|
| `Seq Scan on personas` | Escaneo secuencial: leyó toda la tabla | Señal de posible índice faltante en tablas grandes |
| `cost=0.00..1.12` | **Estimación interna**, no milisegundos. Coste de arranque y coste total | PostgreSQL compara planes y elige el más barato |
| `rows=1` (estimado) | Filas que PostgreSQL **cree** que devolverá | Si la estimación es mala, puede elegir un mal plan |
| `width=40` | Tamaño promedio estimado de cada fila en **bytes** | Útil para optimización fina |
| `actual time=0.023..0.024` | **Tiempo real en ms**: primera fila / última fila | Lo que de verdad tardó |
| `rows=1` (real) | Filas realmente devueltas | Compáralo con el estimado |
| `loops=1` | Cuántas veces se ejecutó ese nodo | En JOINs puede ser mucho mayor |
| `Filter: (edad = 40)` | La condición evaluada fila por fila | Confirma que tu `WHERE` se aplicó |
| `Rows Removed by Filter: 9` | Leyó 10 filas, descartó 9, quedó 1 | **Métrica clave** del desperdicio de lectura |
| `Buffers: shared hit=1` | Los datos ya estaban en la **memoria compartida** | No hubo lectura de disco: por eso fue rápido |
| `Planning Time: 0.087 ms` | Tiempo de analizar la consulta y elegir plan | Bajo en consultas simples |
| `Execution Time: 0.040 ms` | Tiempo total de ejecución, incluido devolver el resultado | Tu número de referencia para comparar |

🖼️ **[Imagen: Captura anotada de la salida de EXPLAIN ANALYZE con cada línea señalada por una etiqueta explicativa en español.]**

```prompt
Ilustración estilo captura de pantalla de pgAdmin 4, formato 16:9, panel de resultados con texto monoespaciado mostrando un plan de ejecución: "Seq Scan on personas (cost=0.00..1.12 rows=1 width=40) (actual time=0.023..0.024 rows=1 loops=1)", "Filter: (edad = 40)", "Rows Removed by Filter: 9", "Buffers: shared hit=1", "Planning Time: 0.087 ms", "Execution Time: 0.040 ms". Cada línea conectada por una flecha fina a un globo de nota en español: "Leyó toda la tabla", "Estimación interna, NO milisegundos", "Tiempo real en milisegundos", "Descartó 9 de 10 filas", "Datos ya en memoria, sin disco", "Tiempo de elegir el plan", "Tiempo total de ejecución". Globos en amarillo claro, fondo blanco, estilo manual técnico corporativo.
```

✅ **Checkpoint de Fase 7:** sabes crear un índice, sabes por qué a veces no se usa y puedes explicar cada línea de un plan.

---

## Fase 8 · Transacciones y concurrencia (Módulo 9)

### 8.1 El problema que resuelven

Una **transacción** es un bloque de operaciones que PostgreSQL trata como **una sola unidad**: **o se ejecutan todas, o no se ejecuta ninguna**.

**Sin transacciones:** si actualizas 1000 registros y el proceso falla en el 500, la mitad queda modificada y la otra mitad no. Tus datos quedan inconsistentes.

**Con transacciones:** ese escenario imposible. O todo, o nada.

### 8.2 Prepara la tabla `cuentas`

```sql
CREATE TABLE cuentas (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    saldo INTEGER
);

INSERT INTO cuentas (nombre, saldo) VALUES ('Juan', 1000);
```

1. **Ejecuta** ambos bloques en la base de datos que prefieras para esta práctica.
2. **Verifica** con `SELECT * FROM cuentas;`.

### 8.3 `BEGIN` + `COMMIT`: confirmar cambios

```sql
BEGIN;

UPDATE cuentas
SET saldo = saldo - 200
WHERE nombre = 'Juan';

COMMIT;
```

| Comando | Qué hace |
|---|---|
| `BEGIN;` | Inicia la transacción. Desde aquí, los cambios **aún no son definitivos**. |
| `COMMIT;` | Confirma. Ahora los cambios son **permanentes** y visibles para las demás sesiones. |

1. **Ejecuta** el bloque completo.
2. **Verifica** el mensaje `COMMIT`.
3. **Abre** un segundo Query Tool y ejecuta `SELECT * FROM cuentas;`.
4. **Confirma** que el saldo es 800 en ambas sesiones.

### 8.4 `ROLLBACK`: el botón de deshacer

```sql
BEGIN;

UPDATE cuentas
SET saldo = saldo - 500
WHERE nombre = 'Juan';

-- Revisa antes de confirmar
SELECT * FROM cuentas;

ROLLBACK;
```

1. **Ejecuta** solo `BEGIN;` y el `UPDATE`.
2. **Consulta** en esta sesión: verás 300.
3. **Consulta** en la otra sesión: seguirá en 800. **El cambio aún no es público.**
4. **Ejecuta** `ROLLBACK;`.
5. **Vuelve a consultar**: el saldo regresó a 800.

> 💡 **Pro Tip:** `ROLLBACK` es el "Ctrl+Z" de la base de datos, pero **solo funciona dentro de una transacción abierta**. Después de `COMMIT` ya no hay vuelta atrás con este comando.

🖼️ **[Imagen: Diagrama de ciclo de vida de una transacción: BEGIN → operaciones → bifurcación COMMIT (persistente) o ROLLBACK (estado original).]**

```prompt
Diagrama de flujo vectorial plano, formato 16:9, fondo blanco, paleta azul profundo, verde y rojo suave. Inicio: caja redondeada "BEGIN — inicia la transacción". Flecha a caja "Operaciones SQL (INSERT / UPDATE / DELETE)" con nota lateral "Los cambios aún NO son definitivos. Otras sesiones no los ven." Luego un rombo de decisión "¿Todo salió bien?" con dos ramas: rama verde "Sí → COMMIT" que llega a una caja con icono de disco "Cambios permanentes y visibles para todos"; rama roja "No → ROLLBACK" que llega a una caja con icono de flecha circular "Se restaura el estado original". Franja inferior: "Después del COMMIT no existe ROLLBACK: los datos son duraderos." Tipografía sans-serif, estilo manual corporativo.
```

### 8.5 ACID: las cuatro garantías

| Letra | Nombre | Significado práctico |
|---|---|---|
| **A** | **Atomicidad** | Todo o nada. Si algo falla, no se guarda nada. |
| **C** | **Consistencia** | Las reglas de la base de datos nunca se rompen. |
| **I** | **Aislamiento** | Las transacciones no se molestan entre sí. Cada una ve su propio mundo. |
| **D** | **Durabilidad** | Tras el `COMMIT`, los cambios sobreviven a fallos y reinicios. |

**A — Atomicidad en acción**

```sql
BEGIN;
UPDATE cuentas SET saldo = saldo - 200 WHERE nombre = 'Juan';
-- ¿Me equivoqué? Deshago todo el bloque.
ROLLBACK;
```

**C — Consistencia en acción: crea una regla**

```sql
ALTER TABLE cuentas
ADD CONSTRAINT saldo_no_negativo CHECK (saldo >= 0);
```

1. **Ejecuta** el `ALTER TABLE`.
2. **Intenta** retirar más dinero del disponible.
3. **Observa** el error: la regla `CHECK` lo impide. La base de datos **nunca** permite saldos negativos.

> 💡 **Pro Tip:** `ALTER TABLE` modifica una tabla ya existente. `ADD CONSTRAINT` agrega una regla con nombre. Es la forma correcta de endurecer un diseño después de crearlo.

**I — Aislamiento en acción: dos sesiones**

1. **Abre** dos Query Tools.
2. **Renombra** las pestañas: `Sesión 1` y `Sesión 2`.
3. **En Sesión 1**, ejecuta:

```sql
BEGIN;
UPDATE cuentas SET saldo = 500 WHERE nombre = 'Juan';
SELECT * FROM cuentas;   -- aquí se ve 500
```

4. **En Sesión 2**, ejecuta `SELECT * FROM cuentas;` → verás **800**.
5. **Entiende** por qué: la transacción de la Sesión 1 no está confirmada. La Sesión 2 no debe trabajar sobre datos que podrían deshacerse.
6. **En Sesión 1**, ejecuta `ROLLBACK;`.
7. **Verifica** que ambas sesiones vuelven a ver 800.

**D — Durabilidad en acción**

1. **Ejecuta** una transacción con `COMMIT`.
2. **Intenta** `ROLLBACK;` después.
3. **Lee** el aviso: no hay transacción en curso.
4. **Conclusión:** para cambiar datos ya confirmados debes usar un nuevo `UPDATE`. Y si reinicias el servidor, los datos confirmados seguirán ahí.

🖼️ **[Imagen: Cuatro cuadrantes ACID con icono y una frase práctica en cada uno.]**

```prompt
Infografía vectorial plana de cuatro cuadrantes, formato 1:1, fondo blanco, paleta azul profundo y celeste con un acento distinto por cuadrante. Cuadrante superior izquierdo: letra grande "A", título "Atomicidad", icono de bloque indivisible, frase "Todo o nada: si algo falla, no se guarda nada". Superior derecho: "C", "Consistencia", icono de escudo con check, frase "Las reglas nunca se rompen (CHECK, NOT NULL, UNIQUE)". Inferior izquierdo: "I", "Aislamiento", icono de dos burbujas separadas, frase "Cada transacción ve su propio mundo". Inferior derecho: "D", "Durabilidad", icono de disco duro con candado, frase "Tras el COMMIT, los datos sobreviven a fallos y reinicios". Título superior: "ACID: las 4 garantías de una transacción". Tipografía sans-serif, estilo manual corporativo.
```

### 8.6 Locks: cuando dos quieren escribir a la vez

Cuando una transacción modifica una fila, PostgreSQL **bloquea esa fila**. Nadie más puede modificarla al mismo tiempo. Esto evita el caos de "¿cuál saldo es el válido?".

**Experimento guiado:**

1. **En Sesión 1**, ejecuta:

```sql
BEGIN;
UPDATE cuentas SET saldo = saldo + 100 WHERE nombre = 'Juan';
```

2. **Consulta** en Sesión 1: el saldo subió a 900 (aún sin confirmar).
3. **En Sesión 2**, ejecuta:

```sql
UPDATE cuentas SET saldo = saldo + 50 WHERE nombre = 'Juan';
```

4. **Observa** el mensaje: *waiting for the query to complete*. La Sesión 2 **queda en espera**.
5. **En Sesión 1**, ejecuta `COMMIT;`.
6. **Vuelve** a Sesión 2: su `UPDATE` se completa sola.
7. **Consulta** el saldo final: **950**. Ninguna actualización se perdió.

🖼️ **[Imagen: Línea de tiempo de dos sesiones mostrando el bloqueo de fila y la espera hasta el COMMIT.]**

```prompt
Diagrama de línea de tiempo vectorial plano, formato 16:9, fondo blanco, dos carriles horizontales etiquetados "Sesión 1" y "Sesión 2". Eje de tiempo de izquierda a derecha con marcas t1 a t5. En Sesión 1: bloque azul "BEGIN", bloque azul "UPDATE saldo + 100 (fila bloqueada)" con icono de candado, y al final bloque verde "COMMIT". En Sesión 2: bloque gris "UPDATE saldo + 50" seguido de una barra rayada amarilla etiquetada "En espera: waiting for the query to complete" que termina justo después del COMMIT de la Sesión 1, y luego bloque verde "Se completa". Debajo, una caja de resultado: "Saldo final: 950 — ninguna actualización se perdió". Nota lateral: "Un SELECT no bloquea un UPDATE, y un UPDATE no bloquea un SELECT (MVCC)". Tipografía sans-serif, estilo manual corporativo.
```

### 8.7 MVCC: leer sin bloquear

PostgreSQL usa **MVCC** (control de concurrencia multiversión): en lugar de sobrescribir, **crea versiones** de los datos. Cada transacción ve la versión que le corresponde.

**Consecuencia práctica que puedes comprobar:**

- Un `SELECT` **no bloquea** un `UPDATE`.
- Un `UPDATE` **no bloquea** un `SELECT`.
- Dos `UPDATE` sobre la **misma fila** sí se bloquean entre sí.

1. **Repite** el experimento anterior.
2. **Esta vez**, mientras la Sesión 1 tiene la transacción abierta, ejecuta un `SELECT` en la Sesión 2.
3. **Comprueba** que responde de inmediato, sin espera.

> 📝 **Nota del Editor:** el video menciona que puedes configurar el **nivel de aislamiento** de la transacción (aislamiento máximo, vista constante, lectura de cambios confirmados), pero no dicta los nombres SQL ni la sintaxis. Verifica los niveles disponibles y el valor por defecto en la documentación oficial antes de cambiarlos.

✅ **Checkpoint de Fase 8:** puedes ejecutar una operación segura con `BEGIN`/`COMMIT`, deshacerla con `ROLLBACK` y explicar ACID, locks y MVCC con tus palabras.

---

## Fase 9 · Funciones, procedimientos y triggers (Módulo 10)

### 9.1 Función vs. procedimiento

| Característica | Función | Procedimiento |
|---|---|---|
| ¿Devuelve valor? | **Sí, siempre** (`RETURN`) | **No** |
| ¿Cómo se ejecuta? | `SELECT mi_funcion(...)` | `CALL mi_procedimiento(...)` |
| ¿Recibe parámetros? | Opcional | Opcional |
| Uso ideal | Cálculos y lógica reutilizable | Procesos y operaciones masivas (`INSERT`, `UPDATE`, `DELETE`) |

> 💡 **Pro Tip:** si no devuelve nada, no es una función: es un procedimiento. Es la misma distinción que ya conoces de otros lenguajes de programación.

### 9.2 Tu primera función: sumar dos números

1. **Abre** un Query Tool sobre la base de datos `prueba`.
2. **Escribe**:

```sql
CREATE OR REPLACE FUNCTION sumar_dos_numeros(numero1 INTEGER, numero2 INTEGER)
RETURNS INTEGER
AS $$
BEGIN
    RETURN numero1 + numero2;
END;
$$ LANGUAGE plpgsql;
```

3. **Ejecuta** para crear la función.
4. **Llámala**:

```sql
SELECT sumar_dos_numeros(5, 7);   -- 12
```

5. **Verifica** en el árbol: **Schemas → public → Functions**.

**Anatomía de la función:**

| Pieza | Función |
|---|---|
| `CREATE OR REPLACE` | Si existe, la reemplaza; si no, la crea |
| `sumar_dos_numeros(...)` | Nombre y parámetros de entrada con su tipo |
| `RETURNS INTEGER` | Tipo del valor que devuelve (**con S final**) |
| `AS $$ ... $$` | Delimita el cuerpo. Es el equivalente a las llaves `{ }` de otros lenguajes |
| `BEGIN ... END;` | Bloque de instrucciones |
| `RETURN` | Devuelve el resultado |
| `LANGUAGE plpgsql` | Lenguaje procedural de PostgreSQL |

> ⚠️ **Errores reales del video, y sus soluciones:**
> 1. **Nombre de parámetro mal escrito** dentro del cuerpo (`numero1` vs `numero2`). PostgreSQL no lo permite corregir con `CREATE OR REPLACE` si cambias la firma.
> 2. **Solución aplicada:** eliminar la función indicando **los tipos de sus parámetros**:
> ```sql
> DROP FUNCTION sumar_dos_numeros(integer, integer);
> ```
> 3. Luego volver a crearla correctamente.
> 4. Otro error frecuente: escribir `RETURN` en lugar de `RETURNS` en la declaración del tipo de retorno.

### 9.3 Tu primer procedimiento

```sql
CREATE OR REPLACE PROCEDURE mostrar_mensaje()
LANGUAGE plpgsql
AS $$
BEGIN
    RAISE NOTICE 'Hola, este es un procedimiento';
END;
$$;
```

1. **Ejecuta** para crearlo.
2. **Llámalo con `CALL`**, no con `SELECT`:

```sql
CALL mostrar_mensaje();
```

3. **Busca** el mensaje en la pestaña de mensajes/notices de pgAdmin.

> 💡 **Pro Tip:** `RAISE NOTICE` muestra mensajes informativos. Es la herramienta de depuración más simple dentro de PL/pgSQL.

### 9.4 PL/pgSQL: variables y condicionales

**PL/pgSQL** es SQL + lógica de programación: variables, `IF`, bucles. Se usa dentro de funciones y procedimientos.

```sql
CREATE OR REPLACE FUNCTION verificar_numero(numero INTEGER)
RETURNS TEXT
AS $$
DECLARE
    resultado TEXT;
BEGIN
    IF numero > 0 THEN
        resultado := 'El número es positivo';
    ELSIF numero < 0 THEN
        resultado := 'El número es negativo';
    ELSE
        resultado := 'El número es cero';
    END IF;

    RETURN resultado;
END;
$$ LANGUAGE plpgsql;
```

1. **Crea** la función.
2. **Pruébala** con tres casos:

```sql
SELECT verificar_numero(-4);   -- El número es negativo
SELECT verificar_numero(10);   -- El número es positivo
SELECT verificar_numero(0);    -- El número es cero
```

| Pieza | Función |
|---|---|
| `DECLARE` | Zona donde declaras variables, antes del `BEGIN` |
| `resultado TEXT;` | Variable y su tipo |
| `:=` | Asignación de valor a la variable |
| `IF ... THEN ... ELSIF ... ELSE ... END IF;` | Condicional. **Se cierra con `END IF;`** |

> ⚠️ **Error real del video:** escribió `RETURN` en la línea del tipo de retorno. Recuerda: **`RETURNS`** para declarar, **`RETURN`** para devolver.

### 9.5 Triggers: reglas que se ejecutan solas

Un **trigger** se dispara automáticamente ante un `INSERT`, `UPDATE` o `DELETE`, y puede actuar **antes** (`BEFORE`) o **después** (`AFTER`). Automatiza reglas sin intervención humana.

**Objetivo del ejercicio:** impedir que nadie registre un producto con precio negativo.

**Paso 1 — Prepara el escenario**

1. **Crea** una base de datos llamada `trigger`.
2. **Abre** un Query Tool sobre ella.
3. **Crea** la tabla:

```sql
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    precio NUMERIC
);
```

**Paso 2 — Crea la función que hará la validación**

```sql
CREATE OR REPLACE FUNCTION validar_precio()
RETURNS TRIGGER
AS $$
BEGIN
    IF NEW.precio < 0 THEN
        RAISE EXCEPTION 'El precio no puede ser negativo';
    END IF;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

| Pieza | Función |
|---|---|
| `RETURNS TRIGGER` | Tipo especial: esta función la usará un trigger |
| `NEW` | La fila **que se intenta insertar** (o el nuevo valor en un `UPDATE`) |
| `RAISE EXCEPTION` | Lanza un error y **cancela** la operación |
| `RETURN NEW` | Permite que la inserción continúe si todo está bien |

**Paso 3 — Crea el trigger**

```sql
CREATE TRIGGER trigger_validar_precio
BEFORE INSERT ON productos
FOR EACH ROW
EXECUTE FUNCTION validar_precio();
```

| Pieza | Función |
|---|---|
| `trigger_validar_precio` | Nombre del trigger. **Usa guiones bajos, nunca espacios** |
| `BEFORE INSERT` | Se ejecuta **antes** de insertar |
| `ON productos` | Tabla vigilada |
| `FOR EACH ROW` | Se evalúa **por cada fila** afectada |
| `EXECUTE FUNCTION validar_precio()` | Función que se dispara |

**Paso 4 — Pruébalo**

```sql
-- Caso válido: se inserta
INSERT INTO productos (nombre, precio) VALUES ('Laptop Gamer', 1200);

-- Caso inválido: el trigger lo rechaza
INSERT INTO productos (nombre, precio) VALUES ('Celular', -500);
```

1. **Ejecuta** el primer `INSERT`. Debe funcionar.
2. **Ejecuta** el segundo. Debe fallar con el mensaje **"El precio no puede ser negativo"**.
3. **Verifica** con `SELECT * FROM productos;` que el celular **no existe**.
4. **Comprende** lo ocurrido: nadie llamó a la función con `SELECT`. **La llamó el trigger**, automáticamente, al detectar el `INSERT`.

🖼️ **[Imagen: Diagrama de flujo de un trigger BEFORE INSERT con dos caminos: precio válido → fila insertada; precio negativo → excepción y fila rechazada.]**

```prompt
Diagrama de flujo vectorial plano, formato 16:9, fondo blanco, paleta azul profundo, verde y rojo. Inicio: caja "INSERT INTO productos (nombre, precio)". Flecha a caja destacada en celeste "Trigger BEFORE INSERT · FOR EACH ROW" con icono de rayo. Flecha a caja "Función validar_precio() revisa NEW.precio". Luego rombo "¿NEW.precio < 0?" con dos ramas: rama roja "Sí" hacia caja con icono de X "RAISE EXCEPTION: El precio no puede ser negativo — la fila NO se inserta"; rama verde "No" hacia caja "RETURN NEW" y de ahí a caja con icono de base de datos "La fila se inserta correctamente". Nota inferior: "El trigger llama a la función automáticamente: nadie ejecuta SELECT". Tipografía sans-serif, estilo manual corporativo.
```

✅ **Checkpoint de Fase 9:** tienes una función con `SELECT`, un procedimiento con `CALL` y un trigger que rechaza datos inválidos.

---

## Fase 10 · Seguridad: roles y permisos (Módulo 11)

### 10.1 El concepto clave

PostgreSQL **no tiene usuarios independientes: tiene roles**.

- Un rol **con login** funciona como usuario.
- Un rol **sin login** funciona como grupo.
- Los **permisos** definen qué puede hacer ese rol y qué no.

| Comando | Acción |
|---|---|
| `CREATE ROLE` | Crea la identidad |
| `GRANT` | Otorga permisos |
| `REVOKE` | Quita permisos |

**Autenticación ≠ autorización:**
- **Autenticación**: saber **quién eres**.
- **Control de accesos**: saber **qué puedes hacer**, a nivel de rol, de tabla e incluso de columna.

> 📝 **Nota del Editor:** el audio menciona los métodos de autenticación de forma distorsionada (se oyen "MT5" y "PIR"). Corresponden a un método de **contraseña cifrada** y a **peer** (que usa los usuarios del sistema operativo). Verifica los métodos vigentes y recomendados en la documentación oficial de tu versión antes de configurar `pg_hba.conf`.

### 10.2 Crea un rol con login

1. **Abre** un Query Tool sobre la base de datos `trigger` (como superusuario `postgres`).
2. **Ejecuta**:

```sql
CREATE ROLE alumno LOGIN PASSWORD '1234';
```

| Pieza | Función |
|---|---|
| `alumno` | Nombre del rol |
| `LOGIN` | Le permite conectarse (es un "usuario") |
| `PASSWORD '1234'` | Su contraseña |

> ⚠️ **Alerta:** `1234` es una contraseña de laboratorio. En cualquier entorno real usa una contraseña fuerte y única.

### 10.3 Otorga permisos con `GRANT`

```sql
-- Permisos sobre la tabla
GRANT SELECT, INSERT ON productos TO alumno;

-- Permiso sobre la secuencia del id autoincremental
GRANT USAGE, SELECT ON SEQUENCE productos_id_seq TO alumno;
```

1. **Ejecuta** el primer `GRANT`.
2. **Ejecuta** el segundo. **No lo omitas.**

> 💡 **Pro Tip crítico:** si la tabla tiene un `id` autoincremental, el rol necesita permiso **también sobre la secuencia**. Sin él, el `INSERT` falla aunque tenga permiso sobre la tabla. Es uno de los fallos más desconcertantes para principiantes.

> 📝 **Nota del Editor:** el video menciona el permiso sobre la secuencia pero no dicta su nombre exacto. `productos_id_seq` es la convención habitual (`tabla_columna_seq`). **Confirma el nombre real** en pgAdmin (Schemas → public → Sequences) o con `\ds` en psql antes de ejecutar.

### 10.4 Conéctate como `alumno` y comprueba

1. **Abre** el menú Inicio y busca **psql**. Pulsa Enter.
2. **Server:** pulsa Enter para aceptar `localhost`.
3. **Database:** escribe `trigger` y pulsa Enter.
4. **Port:** pulsa Enter para aceptar `5432`.
5. **Username:** escribe `alumno` y pulsa Enter.
6. **Password:** escribe `1234` y pulsa Enter (no se verá nada).
7. **Comprueba** el prompt: `trigger=>`.
8. **Prueba la lectura**:

```sql
SELECT * FROM productos;
```

9. **Prueba la escritura**:

```sql
INSERT INTO productos (nombre, precio) VALUES ('Aspiradora Xiaomi', 4000);
```

10. **Verifica** con otro `SELECT` que la fila se insertó.

> 💡 **Pro Tip:** el prompt cambia de `=#` (superusuario) a `=>` (usuario normal). Es una pista visual inmediata de con qué privilegios estás trabajando.

### 10.5 Revoca permisos con `REVOKE`

1. **Vuelve** a pgAdmin, conectado como `postgres`.
2. **Ejecuta**:

```sql
REVOKE INSERT ON productos FROM alumno;
```

3. **Regresa** a la sesión de psql del rol `alumno`.
4. **Intenta** insertar de nuevo:

```sql
INSERT INTO productos (nombre, precio) VALUES ('Smartwatch', 2000);
```

5. **Observa** el error de permiso denegado. El `REVOKE` surtió efecto de inmediato.
6. **Confirma** que el `SELECT` sigue funcionando: solo se quitó la escritura.

🖼️ **[Imagen: Diagrama de roles y permisos con el rol "alumno" y dos tablas de permisos antes/después del REVOKE.]**

```prompt
Infografía vectorial plana, formato 16:9, fondo blanco, paleta azul profundo, verde y rojo. A la izquierda, icono de persona con etiqueta "ROL alumno (LOGIN)". Al centro, icono de tabla con etiqueta "productos" y debajo un icono de secuencia con etiqueta "productos_id_seq". Flechas verdes etiquetadas "GRANT SELECT, INSERT" y "GRANT USAGE, SELECT ON SEQUENCE". A la derecha, dos paneles comparativos: panel "Antes" con lista "SELECT ✅ / INSERT ✅" en verde; panel "Después del REVOKE INSERT" con lista "SELECT ✅ / INSERT ❌" donde la X es roja. Nota inferior en español: "Sin permiso en la secuencia, el INSERT falla aunque tengas permiso en la tabla." Tipografía sans-serif, estilo manual corporativo.
```

✅ **Checkpoint de Fase 10:** creaste un rol, le diste permisos, los probaste desde psql y se los quitaste. Entiendes la diferencia entre autenticación y autorización.

---

## Fase 11 · Backups y restauración (Módulo 12)

### 11.1 Qué es y para qué sirve

Un **backup** es una **foto completa** de tu base de datos en un momento determinado. Puede incluir:

- **Estructura**: tablas, columnas, índices, funciones, procedimientos, triggers.
- **Datos**: todos los registros.
- A veces **roles y permisos**.

**Cuatro razones para hacerlo:**

| Razón | Escenario |
|---|---|
| **Protección** | Error humano, virus o falla del servidor |
| **Cambios seguros** | Probar una actualización y poder volver atrás |
| **Migración** | Mover la base de datos a otro servidor |
| **Recuperación de desastres** | Se borró la base de datos o falló el disco |

**Restaurar** = tomar ese archivo y reconstruir la base de datos al estado exacto del momento del backup.

> ⚠️ **Alerta fundamental:** si respaldaste con 100 clientes y luego se agregaron 10 más, al restaurar recuperas **solo los 100**. Los 10 posteriores se pierden si no tenían respaldo.

> 💡 **Pro Tip:** los servidores modernos permiten **programar** backups (por ejemplo, cada madrugada a la 1:00). Y como ocupan espacio, define una política de retención: conserva uno o dos recientes y elimina los antiguos.

### 11.2 Configura el backup en pgAdmin

1. **Haz clic derecho** sobre la base de datos que quieres respaldar (en el video, `trigger`).
2. **Selecciona** **Backup…**.
3. **Completa** el diálogo según esta tabla:

| Campo | Qué define | Recomendación del curso |
|---|---|---|
| **Filename** | Ruta y nombre del archivo de respaldo | Usa el icono de carpeta para elegir ubicación y nombre |
| **Format** | `Custom`, `Tar` o `Plain` | **Custom** siempre: permite restauración parcial |
| **Compression ratio** | Nivel de compresión, de 0 a 9 | Mayor número = archivo más pequeño pero más lento de crear y restaurar. Vacío = valor por defecto |
| **Encoding** | Codificación de caracteres | **UTF8** si tus datos tienen acentos o caracteres especiales. Vacío = toma la de la BD |
| **Number of jobs** | Procesos paralelos para crear el backup | Útil en bases enormes. En bases pequeñas, déjalo vacío |
| **Role name** | Rol propietario de los objetos al restaurar | Debe existir en el servidor. Vacío = usa el rol actual |

**Comparativa de formatos:**

| Formato | Restauración parcial | Legible/editable | Uso |
|---|---|---|---|
| **Custom** | ✅ Sí (tablas y esquemas específicos) | No | **Recomendado**: máxima flexibilidad |
| **Tar** | ✅ Sí | No | Alternativa empaquetada |
| **Plain** | ❌ No | ✅ Sí, es un script SQL | Cuando quieres leer o editar el SQL |

> 📝 **Nota del Editor:** en el video la opción **Number of jobs** aparece deshabilitada. Verifica en la documentación qué formato habilita el respaldo en paralelo antes de intentar usarlo.

4. **Haz clic** en el icono de carpeta junto a **Filename**.
5. **Navega** a la carpeta destino.
6. **Escribe** el nombre del archivo, por ejemplo `trigger_backup`.
7. **Pulsa Guardar**. Verifica que la ruta quedó escrita en el campo.
8. **Selecciona** el formato **Custom**.
9. **Pulsa** el botón **Backup**.
10. **Espera** el mensaje de proceso iniciado y luego **proceso completado**.
11. **Verifica** en el Explorador de archivos que el archivo existe y tiene tamaño mayor a cero.

🖼️ **[Imagen: Diálogo Backup de pgAdmin con Filename, Format=Custom y el botón Backup resaltado, más una tabla lateral explicando cada campo.]**

```prompt
Ilustración estilo captura de pantalla de pgAdmin 4, formato 16:9, tema claro. Diálogo modal titulado "Backup - trigger" con pestañas "General", "Data Options", "Query Options", "Table Options". Campos visibles: "Filename" con valor "trigger_backup" y un icono de carpeta a la derecha; "Format" con lista desplegable mostrando "Custom" seleccionado; "Compression ratio" vacío; "Encoding" con "UTF8"; "Number of jobs" deshabilitado en gris; "Role name" vacío. Botones inferiores "Cancel" y "Backup", este último resaltado con recuadro rojo y flecha roja. A la derecha, panel de notas en español con viñetas: "Custom permite restauración parcial", "Compresión 0 a 9", "UTF8 para acentos", "El rol debe existir en el servidor". Interfaz limpia y realista, sin logos oficiales.
```

### 11.3 Simula el desastre: elimina la base de datos

> ⚠️ **Alerta:** este paso es **destructivo a propósito**. Ejecútalo solo sobre una base de datos de práctica cuyo backup ya verificaste.

1. **Haz clic derecho** sobre `trigger` → **Delete/Drop**.
2. **Confirma** la eliminación.
3. **Si aparece un error** de que la base está siendo usada por otros usuarios: **cierra las otras sesiones** (por ejemplo, la ventana de psql conectada como `alumno`) y reintenta.
4. **Refresca** el árbol y **confirma** que `trigger` desapareció.

### 11.4 Restaura desde el archivo

1. **Crea** una base de datos nueva **con el mismo nombre**: `trigger`. Estará completamente vacía: sin esquemas propios, sin tablas, sin triggers.
2. **Haz clic derecho** sobre la nueva `trigger` → **Restore…**.
3. **Selecciona** el archivo de backup que guardaste (`trigger_backup`).
4. **Pulsa Abrir** y luego el botón **Restore**.
5. **Espera** los mensajes de proceso iniciado y completado.
6. **Refresca** el árbol: **Schemas → public → Tables**.
7. **Verifica** que la tabla `productos` volvió.
8. **Abre** un Query Tool y ejecuta:

```sql
SELECT * FROM productos;
```

9. **Confirma** que los datos son exactamente los del momento del backup.

🖼️ **[Imagen: Flujo de tres pasos Backup → Drop → Restore con iconos de archivo, papelera y base de datos reconstruida.]**

```prompt
Infografía vectorial plana de proceso en tres pasos, formato 16:9, fondo blanco, paleta azul profundo, celeste, rojo y verde. Paso 1: icono de base de datos con una flecha hacia un icono de archivo etiquetado "trigger_backup (formato Custom)", título "1. BACKUP", subtítulo "Foto completa: estructura + datos". Paso 2: icono de papelera con una base de datos desvaneciéndose, título "2. DROP", subtítulo "Simulamos el desastre. Cierra otras sesiones antes de borrar." Paso 3: icono de archivo con flecha hacia una base de datos reconstruida, título "3. RESTORE", subtítulo "Crea una BD vacía con el mismo nombre y restaura". Flechas grandes entre pasos. Franja inferior de advertencia en amarillo: "Solo recuperas lo que había en el momento del backup. Los datos posteriores se pierden." Tipografía sans-serif, estilo manual corporativo.
```

✅ **Checkpoint de Fase 11:** respaldaste, borraste y restauraste tu base de datos con todos sus objetos. **Has completado el ciclo profesional completo.**

---

## Solución de problemas comunes (FAQ)

### Tabla de diagnóstico rápido

| Síntoma / mensaje | Causa probable | Solución |
|---|---|---|
| `syntax error at or near "nombre"` | Falta una **coma** entre definiciones de columna | Agrega la coma después de cada columna salvo la última |
| Error de sintaxis al final del `CREATE TABLE` | **Coma sobrante** después de la última definición | Elimina esa coma antes del `)` |
| `column "Juan Pérez" does not exist` | Usaste **comillas dobles** para un valor de texto | Cambia a comillas simples: `'Juan Pérez'` |
| `column "stock" of relation ... does not exist` | Nombre de columna mal escrito en el `INSERT` | Compara con el `CREATE TABLE`; los nombres deben coincidir exactamente |
| `duplicate key value violates unique constraint` | Insertaste un valor repetido en una columna `UNIQUE` | Usa un valor distinto (el video cambió `ana@mail.com` → `ana2@mail.com`) |
| `null value in column ... violates not-null constraint` | Omitiste un campo `NOT NULL` | Incluye el campo obligatorio en el `INSERT` |
| `relation "idx_personas_edad" already exists` | El índice ya fue creado | No lo vuelvas a crear; usa otro nombre o elimínalo antes |
| No puedo reemplazar una función | Cambiaste el nombre/tipo de los parámetros | `DROP FUNCTION nombre(tipo1, tipo2);` y créala de nuevo |
| Error en la declaración de retorno | Escribiste `RETURN` en vez de `RETURNS` | Usa `RETURNS <tipo>` al declarar; `RETURN` solo dentro del cuerpo |
| Error al crear un trigger con espacios en el nombre | Los identificadores no admiten espacios sin comillas | Usa guiones bajos: `trigger_validar_precio` |
| `permission denied for table productos` | El rol no tiene ese permiso (hubo `REVOKE`) | Otorga el permiso con `GRANT` desde el superusuario |
| `INSERT` falla con rol nuevo pese a tener permiso en la tabla | Falta permiso sobre la **secuencia** del `id` | `GRANT USAGE, SELECT ON SEQUENCE ... TO rol;` |
| `database "trigger" is being accessed by other users` | Hay sesiones abiertas (psql u otro Query Tool) | Cierra esas sesiones y reintenta el Drop |
| No puedo conectarme a PostgreSQL | El **servicio está detenido** | Panel Servicios → clic derecho → Iniciar |
| La contraseña "no se escribe" en psql | Es el comportamiento normal de seguridad | Escribe a ciegas y pulsa Enter |
| La tabla nueva no aparece en el árbol | pgAdmin no refresca solo | Clic derecho → **Refresh** (F5 sobre el árbol) |
| Creé la tabla en la base de datos equivocada | Abriste el Query Tool sobre otra BD | Clic derecho **sobre la BD correcta** → Query Tool |
| El índice existe pero el plan dice `Seq Scan` | La tabla tiene muy pocas filas | Es correcto: el optimizador prefiere leer 10 filas. Prueba con más volumen |
| `waiting for the query to complete` | Otra transacción tiene la fila **bloqueada** | Haz `COMMIT` o `ROLLBACK` en la sesión que bloquea |
| `there is no transaction in progress` al hacer `ROLLBACK` | Ya hiciste `COMMIT` o no abriste transacción | No hay nada que deshacer; usa un nuevo `UPDATE` |
| Error al retirar más saldo del disponible | La restricción `CHECK (saldo >= 0)` lo impide | Es el comportamiento deseado. Ajusta el importe |
| `El precio no puede ser negativo` | El trigger `BEFORE INSERT` rechazó la fila | Corrige el precio; la fila no se insertó |
| El alias no funciona en `ORDER BY` | El nombre no coincide exactamente con el del `SELECT` | Copia el alias idéntico, con guiones bajos |
| El puerto 5432 está ocupado | Otra instancia de PostgreSQL lo usa | Instala con otro puerto y anótalo para conectarte |
| `UPDATE`/`DELETE` afectó toda la tabla | Faltó la cláusula `WHERE` | Restaura desde backup. Prevéngalo: prueba el `WHERE` con un `SELECT` antes |

### Las cinco preguntas que todo principiante se hace

**1. ¿pgAdmin o psql? ¿Cuál debo usar?**
Ambos hablan con el mismo servidor. pgAdmin es más cómodo para aprender y para backups; psql es más rápido y es lo que encontrarás en servidores de producción. Aprende los dos: este manual los usa alternadamente.

**2. ¿Por qué mi `SELECT` no muestra la tabla que acabo de crear?**
Casi siempre es una de dos cosas: no refrescaste el árbol, o estás en otra base de datos. Revisa el título de la pestaña del Query Tool.

**3. ¿Cuándo uso `SERIAL` y cuándo `GENERATED ALWAYS AS IDENTITY`?**
Ambos generan el número automáticamente. `SERIAL` es la forma clásica; `IDENTITY` es la forma moderna y estándar, preferible en proyectos nuevos.

**4. ¿Puedo deshacer un `DELETE` que ya ejecuté?**
Solo si estaba dentro de una transacción sin confirmar (`ROLLBACK`) o si tienes un backup. Fuera de esos casos, no. Por eso la Fase 11 es tan importante.

**5. ¿Por qué mi consulta sigue lenta si creé un índice?**
Porque el optimizador decide. Con pocas filas, leer la tabla completa es más barato. Usa `EXPLAIN ANALYZE` para ver el plan real y prueba con volumen de datos representativo.

---

## Chuleta final (imprímela y tenla al lado)

**Metacomandos de psql**

```text
\h    ayuda SQL          \l    listar bases de datos
\?    ayuda metacomandos \dt   listar tablas
\c    cambiar de BD      \d    ver estructura
\q    salir
```

**Orden correcto de una consulta completa**

```sql
SELECT columnas, funcion_agregada(col) AS alias
FROM tabla
JOIN otra_tabla ON tabla.id = otra_tabla.tabla_id
WHERE condicion_de_filas
GROUP BY columna_agrupada
HAVING condicion_de_grupos
ORDER BY columna ASC|DESC
LIMIT n OFFSET m;
```

**Los 10 mandamientos del principiante en PostgreSQL**

1. Verifica que el **servicio esté en ejecución** antes de culpar a tu SQL.
2. **Comillas simples** para valores; dobles para identificadores.
3. **Coma entre definiciones**, nunca después de la última.
4. Escribe el `WHERE` **antes** de ejecutar un `UPDATE` o `DELETE`.
5. Prueba tu `WHERE` con un `SELECT` primero.
6. **Nombra** tus restricciones (`CONSTRAINT fk_...`).
7. Da permisos también sobre las **secuencias**.
8. Mide con `EXPLAIN ANALYZE`, no con intuición.
9. Envuelve operaciones críticas en `BEGIN` … `COMMIT`.
10. **Respalda antes de cambiar** cualquier cosa importante.

---

## 🎓 Cierre: tu siguiente paso

Has recorrido el ciclo completo: **instalar → conectar → modelar → consultar → relacionar → optimizar → proteger → programar → asegurar → respaldar**. Eso ya no es "saber un poco de SQL": es operar una base de datos con criterio.

**Plan de práctica sugerido para los próximos 7 días:**

1. **Día 1–2:** reconstruye `tienda_online` desde cero, **sin mirar el manual**. Solo la chuleta.
2. **Día 3:** agrega una tabla `categorias` y relaciona `productos` con ella mediante FK. Practica los cuatro JOIN.
3. **Día 4:** carga miles de filas y repite la Fase 7. Ahora sí verás cambiar el plan de ejecución.
4. **Día 5:** escribe un trigger `BEFORE UPDATE` que impida bajar el stock por debajo de cero.
5. **Día 6:** crea un rol de solo lectura y comprueba desde psql que no puede escribir.
6. **Día 7:** programa un backup y **restáuralo en una base de datos con otro nombre**. Un respaldo que nunca se restauró no es un respaldo: es una esperanza.

> 💡 **Pro Tip final del instructor:** el material del curso se apoya en un archivo de consultas, pero **escribir el código a mano es lo que realmente enseña**. Copiar te da resultados; escribir te da criterio.

**Bienvenido al mundo PostgreSQL. Ya sabes cuidar los datos.** 🐘
