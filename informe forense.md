# 03 – Informe Técnico y Ejecutivo Completo

## 📌 1. Informe Ejecutivo

El día **13 de noviembre de 2025**, el CSIRT detectó actividad anómala en el equipo **PC‑IT‑03** del departamento de IT. El análisis forense reveló que el sistema fue comprometido mediante la explotación del **CVE‑2018‑8174 (Double Kill)**, una vulnerabilidad crítica que permite la ejecución remota de código a través de VBScript.

El atacante utilizó un **dropper en VBScript**, ejecutado mediante `wscript.exe`, que descargó y ejecutó dos payloads maliciosos:

* `QkryuzzwVu.exe`
* `KzcmVNSNkYkueQf.exe`

Ambos intentaron conectarse a un servidor C2 en la IP **10.28.5.1** utilizando los puertos **8081** y **53**, sin éxito (estado SYN_SENT).

### **Impacto**

* Ejecución remota de código en el equipo.
* Riesgo de descarga de malware adicional.
* No se detectó movimiento lateral ni persistencia.
* No se observaron daños en servicios del sistema.

### **Recomendaciones**

1. Instalar todas las actualizaciones pendientes, especialmente el parche para CVE‑2018‑8174.
2. Restringir o deshabilitar VBScript en sistemas Windows.
3. Bloquear `wscript.exe` mediante AppLocker.
4. Desplegar un EDR con detección de scripting malicioso.
5. Realizar formación contra phishing.

---

## 📌 2. Informe Técnico Detallado

### 2.1 Contexto

El equipo afectado ejecutaba **Windows 7 SP1 x64**. Se realizó adquisición live de memoria, disco y logs del sistema.

**Evidencias recolectadas:**

* `memdump.mem`
* `disco.E01`
* Listados: tasklist, netstat, netscan, etc.

### 2.2 Análisis de Memoria

Se identificaron dos procesos sospechosos:

* `QkryuzzwVu.exe` (PID 944)
* `KzcmVNSNkYkueQf.exe` (PID 2960)

Ambos fueron ejecutados por **wscript.exe**, indicador de dropper en VBScript.

### 2.3 Análisis de Red

Intentos de conexión saliente:

* `QkryuzzwVu.exe` → 10.28.5.1:8081 (SYN_SENT)
* `KzcmVNSNkYkueQf.exe` → 10.28.5.1:53 (SYN_SENT)

### 2.4 Vector de Compromiso

El comportamiento del sistema confirma la explotación del:

# **CVE‑2018‑8174 – "Double Kill" (VBScript RCE)**

### 2.5 Herramienta de Hacking

El atacante utilizó:

# **Un dropper en VBScript (VBS) ejecutado mediante `wscript.exe`**

No se emplearon frameworks como Metasploit, Cobalt Strike o Sliver.

### 2.6 Evaluación del Daño

* No hay persistencia detectada.
* No hay exfiltración registrada.
* Malware no logró comunicación con el C2.

---

## 📌 3. Conclusiones

El atacante explotó CVE‑2018‑8174 mediante un documento malicioso que ejecutó un VBScript. Este descargó y ejecutó dos payloads que intentaron comunicarse sin éxito con un servidor C2. La respuesta temprana evitó daños mayores.

---

## 📌 4. Anexo de Evidencias

### A1 – `QkryuzzwVu.exe`

* Tipo: Payload malicioso
* Estado: Solo en memoria
* PID: 944
* Padre: `wscript.exe`
* Conexión: 10.28.5.1:8081 (SYN_SENT)
* Hash: No disponible

### A2 – `KzcmVNSNkYkueQf.exe`

* Tipo: Payload malicioso
* PID: 2960
* Padre: `wscript.exe`
* Conexión: 10.28.5.1:53 (SYN_SENT)

### A3 – Proceso padre `wscript.exe`

* PIDs: 2816 / 2824
* Función: Ejecución del dropper VBS

### A4 – Integridad del disco

* Archivo: `disco.E01`
* MD5: 77caee16ef4f58421e5686572656bb07
* SHA1: b8fd3876617625d3f47018203ca15c1bbd1ae9c8

---

**Fin del informe**
