# 03 – Informe forense

**Fecha del incidente:** 13/11/2019
**Sistema afectado:** FORENSE-06 — Departamento IT
**Sistema operativo:** Windows 7 SP1 x64
**Analista:** CSIRT Interno

## 1. INFORME EJECUTIVO

El día 13 de noviembre de 2019 se detectó actividad anómala en el equipo FORENSE-06. El análisis forense determinó que el sistema fue comprometido mediante la explotación del **CVE-2018-8174 (Double Kill)**, vulnerabilidad crítica que permite ejecución remota de código a través de VBScript.

Tras la explotación, el atacante ejecutó un **dropper en VBScript** que lanzó dos payloads:

* `QkryuzzwVu.exe`
* `KzcmVNSNkYkueQf.exe`

Ambos intentaron comunicarse con un **servidor C2** en la IP `10.28.5.1`, sin lograr conexión efectiva.

Se detectó también la presencia de **KMSPico**, herramienta de activación ilegal asociada a malware, aumentando el riesgo de exposición previa.

El análisis evaluó la posible explotación de **EternalBlue (CVE-2017-0144)**, debido a que Windows 7 SP1 es vulnerable. No se encontraron evidencias de uso exitoso, aunque el riesgo era significativo.

### Impacto del incidente

* Ejecución remota de código.
* Ejecución de malware en memoria.
* Intentos de comunicación con C2.
* Riesgo elevado por presencia de software crackeado.
* No se identificó movimiento lateral, persistencia ni exfiltración.

### Recomendaciones principales

1. Aplicar los parches para **CVE-2018-8174** y **CVE-2017-0144**.
2. Eliminar **KMSPico** y realizar reinstalación limpia del sistema.
3. Deshabilitar **VBScript** y `wscript.exe` mediante AppLocker.
4. Implementar **EDR con heurística** para scripting y explotación.
5. Reforzar segmentación de red y auditorías periódicas.
6. Capacitación sobre documentos maliciosos.

## 2. INFORME TÉCNICO DETALLADO

### 2.1. Contexto

El equipo comprometido ejecutaba Windows 7 SP1 x64 con múltiples parches faltantes, incluyendo CVE-2018-8174 y CVE-2017-0144.

**Evidencias adquiridas:**

* Volcado de memoria: `memdump.mem`
* Imagen de disco: `disco.E01`
* Listados: `tasklist`, `netstat`, `evtlogs`, `netscan`

### 2.2. Vector de Compromiso Confirmado: CVE-2018-8174

**Descripción:**
Vulnerabilidad que permite ejecución remota de código mediante **VBScript** incrustado en documentos maliciosos (generalmente Word), explotado a través de `wscript.exe`.

**Indicadores observados:**

* Ejecución de `wscript.exe` sin interacción del usuario.
* Lanzamiento de payloads directamente desde memoria.
* Procesos huérfanos con comportamiento de dropper.

### 2.3. Procesos Maliciosos Identificados

**QkryuzzwVu.exe**

* PID: 944
* Padre: wscript.exe
* Actividad: intento de conexión a C2 (10.28.5.1)
* Estado: SYN_SENT
* Localización: memoria (no en disco)

**KzcmVNSNkYkueQf.exe**

* PID: 2960
* Padre: wscript.exe
* Actividad: intento de conexión a C2 (10.28.5.1)
* Estado: SYN_SENT

**Proceso ejecutor:** `wscript.exe` (PIDs 2816 y 2824)

* Rol: ejecución del dropper VBS responsable de lanzar ambos payloads.

### 2.4. Análisis de Conectividad

| Proceso             | Puerto | IP C2     | Estado   |
| ------------------- | ------ | --------- | -------- |
| QkryuzzwVu.exe      | 8081   | 10.28.5.1 | SYN_SENT |
| KzcmVNSNkYkueQf.exe | 53     | 10.28.5.1 | SYN_SENT |

> Sin conexión establecida, evitando ejecución adicional de órdenes remotas.

### 2.5. Posible Uso de EternalBlue (CVE-2017-0144)

**Análisis exploratorio:** no indicio de explotación.

* Sistema corría Windows 7 SP1
* SMBv1 habilitado
* MS17-010 no instalado

**Resultado:**

* No se encontraron procesos relacionados con `lsass.exe` inyectados.
* No hubo creación de servicios sospechosos.
* No se identificaron anomalías en el log 4624 tipo 3.

> Conclusión: EternalBlue no fue utilizado, aunque el riesgo era crítico.

### 2.6. Evidencia de KMSPico

* Carpeta: `C:\Program Files\KMSpico\`
* Ejecutable: `AutoPico.exe`
* Tareas programadas asociadas

**Relevancia:** crack con malware embebido, puerta de entrada previa y riesgo persistente.

### 2.7. Evaluación del Daño

| Componente           | Resultado                           |
| -------------------- | ----------------------------------- |
| Persistencia         | No detectada                        |
| Movimiento lateral   | No observado                        |
| Exfiltración         | No evidenciada                      |
| Conexión C2          | No establecida                      |
| Integridad del disco | Comprometida por software crackeado |
| Nivel de riesgo      | Alto                                |

## 3. CONCLUSIONES

* Compromiso mediante CVE-2018-8174, ejecución de dropper en VBScript y despliegue de dos payloads.
* Intentos de comunicación con C2 no concretados.
* KMSPico debilitó la seguridad y pudo facilitar la intrusión.
* No se evidenció explotación de EternalBlue.
* Contención rápida gracias a detección temprana.

## 4. ANEXO DE EVIDENCIAS

**A1 — QkryuzzwVu.exe**

* Tipo: Payload malicioso en memoria
* PID: 944
* Conexión: 8081 → 10.28.5.1
* Hash: N/A (no en disco)

**A2 — KzcmVNSNkYkueQf.exe**

* Tipo: Malware
* PID: 2960
* Conexión: 53 → 10.28.5.1

**A3 — wscript.exe**

* Proceso ejecutor del dropper VBS
* PIDs: 2816, 2824

**A4 — Disco (E01)**

* MD5: 77caee16ef4f58421e5686572656bb07
* SHA1: b8fd3876617625d3f47018203ca15c1bbd1ae9c8
