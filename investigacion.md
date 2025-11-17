# 02 – Investigación del Incidente

## 1. Introducción

Este documento responde a las preguntas del Proyecto 2 relativas al análisis del incidente, identificando el CVE explotado, los procesos implicados y la herramienta de hacking utilizada.

---

## 2. ¿Qué CVE fue explotado?

Tras el análisis de memoria y de los procesos del sistema comprometido, se ha determinado que el atacante aprovechó la vulnerabilidad:

# **CVE-2018-8174 — "Double Kill" (VBScript RCE)**

### Justificación:

* Se observó la ejecución de **wscript.exe** como proceso padre de los ejecutables maliciosos.
* La explotación de este CVE se basa en la ejecución de código mediante **VBScript**, normalmente cargado desde documentos maliciosos.
* La máquina afectada ejecuta **Windows 7 SP1**, una versión vulnerable.
* Los payloads se ejecutaron mediante scripting, sin modificación de servicios del sistema, lo cual es típico de este exploit.

---

## 3. ¿Cuál es el nombre del proceso exacto tras el compromiso?

Durante el análisis se identificaron **dos procesos maliciosos** ejecutados en memoria:

### 🔹 `QkryuzzwVu.exe`

* PID: 944
* Proceso padre: `wscript.exe`
* Actividad: intento de conexión al C2 por el puerto 8081

### 🔹 `KzcmVNSNkYkueQf.exe`

* PID: 2960
* Proceso padre: `wscript.exe`
* Actividad: intento de conexión al C2 por el puerto 53

### ✔ Proceso malicioso principal: **`QkryuzzwVu.exe`**

Este fue el primer ejecutable lanzado tras la explotación y el que realiza la conexión más significativa al C2.

---

## 4. ¿Qué herramienta de hacking se utilizó una vez comprometida la máquina?

No se identificó el uso de herramientas de explotación comunes (Metasploit, Cobalt Strike, Sliver). El análisis reveló que la herramienta utilizada fue:

# **Un dropper malicioso en VBScript (VBS) ejecutado mediante `wscript.exe`**

### Funciones observadas del dropper:

* Descarga de dos payloads ejecutables.
* Ejecución automática de los EXE con nombres aleatorios.
* Intentos de conexión a un servidor C2.

### Indicadores clave:

* `wscript.exe` aparece como proceso padre de ambos ejecutables.
* No se detectan DLLs ni patrones de frameworks típicos.
* Los EXEs usan nombres generados aleatoriamente (típico de malware personalizado).

---

## 5. Conclusión

El incidente fue originado por la explotación del **CVE-2018-8174**, permitiendo la ejecución de un dropper en VBScript que lanzó dos payloads maliciosos. El proceso principal involucrado fue `QkryuzzwVu.exe`, y la herramienta empleada corresponde a un script malicioso VBS diseñado para descargar y ejecutar malware personalizado.

---

**Fin del documento**
