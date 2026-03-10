# BLOQUE 1.2 – Procesos principales del sistema
## 1. Objetivo
Describir los procesos que intervienen en el flujo de datos entre:
- ERP (Odoo 18)
- Proceso ETL (Apache Hop)
- Almacén de datos (PostgreSQL – esquema staging)
Además, identificar las herramientas que permiten monitorizar el funcionamiento del sistema y detectar incidencias.
---
# 2. Arquitectura del flujo de datos
## 2.1 Esquema general del flujo
Usuarios ERP │ ▼ Odoo 18 (contenedor Docker) │ ▼ PostgreSQL transaccional (contenedor Docker) │ ▼ Apache Hop ETL (ejecutándose en el host Windows) │ ▼ PostgreSQL – esquema staging (almacén de datos)
Descripción breve del flujo:
- Los usuarios interactúan con el sistema ERP Odoo.
- Las operaciones se almacenan en PostgreSQL transaccional.
- Apache Hop ejecuta procesos ETL que extraen datos desde PostgreSQL.
- Los datos transformados se cargan en el esquema **staging** del almacén de datos.
---
# 3. Procesos del sistema ERP
## 3.1 Proceso de gestión en Odoo
**Sistema:** Odoo 18
**Ubicación:** Contenedor Docker
Descripción del proceso:
Odoo gestiona las operaciones del negocio, como:
- gestión de clientes
- registro de ventas
- gestión de productos
- pedidos y facturación
Estas operaciones generan datos que se almacenan en la base de datos PostgreSQL utilizada por el ERP.
---
## 3.2 Base de datos transaccional
**Sistema gestor:** PostgreSQL
**Ubicación:** Contenedor Docker
Descripción del proceso:
La base de datos PostgreSQL almacena toda la información generada por el ERP, incluyendo:
- clientes
- productos
- pedidos
- facturas
- transacciones comerciales
Esta base de datos actúa como **fuente de datos para el proceso ETL**.
---
# 4. Proceso ETL
## 4.1 Herramienta utilizada
Apache Hop ejecutándose en el **host Windows**.
El proceso ETL se encarga de:
- **Extract:** extraer datos desde PostgreSQL del ERP
- **Transform:** limpiar y transformar datos
- **Load:** cargar los datos en el almacén de datos
---
## 4.2 Transformaciones realizadas
Ejemplos de transformaciones que pueden realizarse en el pipeline:
- limpieza de espacios
- sustitución de valores nulos
- conversión de tipos de datos
- filtrado de registros
- estandarización de formatos
Estas transformaciones permiten preparar los datos para su análisis posterior.

---
# 5. Almacén de datos
## 5.1 Base de datos del almacén
Sistema gestor: PostgreSQL
El almacén de datos utiliza un esquema denominado:
staging
Este esquema almacena los datos procedentes del ERP después de pasar por el proceso ETL.

---
## 5.2 Función del esquema staging
El esquema staging sirve como:
- zona temporal de almacenamiento
- área de preparación de datos
- separación entre sistema transaccional y sistema analítico
Esto evita que los procesos analíticos afecten al rendimiento del ERP.

---
# 6. Herramientas de monitorización del sistema
## 6.1 Monitorización del sistema Windows
Herramientas utilizadas:
- Administrador de tareas
- Monitor de recursos
- PowerShell
Parámetros observados:
- uso de CPU
- memoria disponible
- procesos activos
- uso de disco
Estas herramientas permiten detectar saturación del sistema o procesos que consumen demasiados recursos.

---
## 6.2 Monitorización de contenedores Docker
Herramientas utilizadas:
docker ps docker stats docker logs
Información obtenida:
- estado de los contenedores
- consumo de CPU
- consumo de memoria
- mensajes de error en los logs
Permite verificar el funcionamiento de:
- Odoo
- PostgreSQL

---
## 6.3 Monitorización de PostgreSQL
PostgreSQL permite analizar la actividad de la base de datos mediante consultas internas.
Se pueden revisar aspectos como:
- conexiones activas
- consultas en ejecución
- estado de las sesiones
Esto permite detectar problemas como:
- exceso de conexiones
- consultas lentas
- bloqueos en la base de datos
---
## 6.4 Monitorización del proceso ETL
Apache Hop permite analizar la ejecución de los pipelines ETL mediante:
- logs de ejecución
- estado de los pasos del pipeline
- mensajes de error
Esto permite identificar incidencias durante la extracción, transformación o carga de datos.
---
# 7. Resumen de procesos del sistema
| Componente | Función | Tecnología |
|-------------|---------|-----------|
| ERP | Gestión de operaciones del negocio | Odoo 18 |
| Base de datos transaccional | Almacenamiento de datos operativos | PostgreSQL |
| Proceso ETL | Extracción, transformación y carga de datos | Apache Hop |
| Almacén de datos | Almacenamiento para análisis | PostgreSQL (staging) |
---
# 8. Conclusión
El sistema está compuesto por un ERP Odoo que almacena los datos en PostgreSQL, un proceso ETL ejecutado mediante Apache Hop que extrae y transforma dichos datos, y un almacén de datos basado en PostgreSQL donde se almacenan los datos preparados para su análisis.
La monitorización del sistema se realiza mediante herramientas del sistema operativo, Docker, PostgreSQL y los logs del proceso ETL, lo que permite detectar incidencias y problemas de rendimiento.