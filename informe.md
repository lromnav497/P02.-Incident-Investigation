# 01 – Informe de Adquisición de Evidencias

## 1. Introducción

Este documento detalla el proceso de recolección, preservación y almacenamiento de evidencias realizado sobre el equipo comprometido **PC-IT-03** del departamento de IT, siguiendo la metodología DFIR definida por la organización.

---

## 2. Metodología Aplicada

Estándares utilizados:

* ISO/IEC 27037 – Identificación y recolección de evidencias digitales.
* ISO/IEC 27042 – Análisis e interpretación.
* UNE 71506 – Cadena de custodia.
* NIST SP 800-86 – Integración forense en respuesta a incidentes.

Fases aplicadas:

1. Identificación del activo y situación inicial.
2. Recolección de evidencias.
3. Preservación mediante cálculo de hashes.
4. Documentación de cadena de custodia.
5. Almacenamiento seguro.

---

## 3. Recolección de Evidencias

El incidente fue live, por lo que la recolección se realizó directamente en el sistema afectado.

### 3.1 Pasos realizados

1. Se accedió al equipo con las credenciales proporcionadas.
2. Se documentó el estado del sistema (procesos, conexiones, sesiones activas).
3. Se realizó adquisición completa de memoria RAM.
4. Se realizó adquisición de la imagen del disco en formato **E01**.
5. Se generaron listados adicionales: `tasklist`, `netstat`, `volatility netscan`.
6. Se preservaron las evidencias mediante hashing (MD5 y SHA1).

### 3.2 Evidencias recolectadas

* **memdump.mem** – Volcado de memoria.
* **disco.E01** – Imagen del disco.
* **tasklistdump.txt** – Listado de procesos.
* **netscandump.txt** – Conexiones activas.
* **netscanlog.txt** – Resultado de Volatility netscan.
* **tasklistlog.txt** – Procesos en ejecución.

---

## 4. Cadena de Custodia

* **Descubrimiento:** 13/11/2025 10:14 — Técnico CSIRT A.
* **Recolección:** 13/11/2025 10:30 — Técnico CSIRT B.
* **Hashing:** 13/11/2025 10:44 — Técnico CSIRT B.
* **Custodia actual:** Repositorio seguro del CSIRT.

### Hashes de las evidencias

* **disco.E01**

  * MD5: 77caee16ef4f58421e5686572656bb07
  * SHA1: b8fd3876617625d3f47018203ca15c1bbd1ae9c8

*(El resto de hashes se añadirán si se requiere su extracción.)*

---

## 5. Almacenamiento

Las evidencias fueron almacenadas en:

* Repositorio seguro interno del CSIRT.
* Copia adicional en repositorio público solicitado para la entrega.

Formato:

* Memoria → RAW
* Disco → E01
* Logs → TXT

---

## 6. Conclusiones

La adquisición se realizó correctamente siguiendo los procedimientos establecidos. Las evidencias mantienen su integridad y se encuentran listas para su análisis posterior.

---

## 7. Enlace al repositorio

*(Añadir URL cuando el repositorio esté creado.)*
