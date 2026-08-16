---
title: "Examen Final — Lazarus Group"
date: 2026-08-15 18:38
categories: [Examen, Threat Intelligence]
tags: [examen-final]
---

# Acto 1 — La inteligencia dirige (OpenCTI del curso)

Basándonos en OpenCTI, el perfil de Lazarus Group quedaría así:
## 1. Intrusion Set:

* Identidad: Lazarus Group.
* Alias: Guardians of Peace, ZINC, HIDDEN COBRA, NICKEL ACADEMY.
* Motivación: Geopolítica y financiera. Es un grupo patrocinado por el estado de Corea del Norte (Reconnaissance General Bureau - RGB).

![ALIAS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1786839017/ALIAS_pafqbj.png)

![MOTIVACION](https://res.cloudinary.com/dopcqb8wn/image/upload/v1786839055/MOTIVACION_ycdbwj.png)

## 2. TTPs Clave (Attack Patterns) [Basado en OpenCTI y análisis]

* T1560.003: Archive via Custom Method - Lazarus utiliza métodos personalizados para archivar datos robados antes de exfiltrarlos, usando formatos y técnicas propias para comprimir y empaquetar información sensible .

* T1049: System Network Connections Discovery - El grupo realiza descubrimiento de conexiones de red activas para mapear el entorno, identificar sistemas accesibles y planificar movimiento lateral dentro de la red comprometida .

* T1585.002: Email Accounts - Lazarus crea cuentas de correo electrónico falsas o compromete cuentas legítimas para utilizarlas en campañas de spearphishing, estableciendo así canales de entrega de malware .

* T1021.002: SMB/Windows Admin Shares - Utilizan recursos compartidos administrativos (ADMIN,C, IPC$) para movimiento lateral y ejecución remota de comandos, aprovechando credenciales robadas para acceder a otros sistemas .

* T1190: Exploit Public-Facing Application - La técnica que usarás en tu examen. Lazarus la usa para obtener acceso inicial mediante la explotación de vulnerabilidades en aplicaciones expuestas a internet. Ejemplo: Explotación de ElasticSearch (CVE-2014-3120) .

![FUENTE](https://res.cloudinary.com/dopcqb8wn/image/upload/v1786840038/TECNICAS_iavrzt.png)

## 3. IoCs / Indicators Asociados [Basado en OpenCTI y análisis complementario]

* Dominios de C2: Asociados a RATANKBA y Bankshot para comunicaciones HTTPS
* Archivos maliciosos: DLLs y ejecutables de los malware Bankshot, RATANKBA, Cryptoistic
* Protocolos: TCP (Cryptoistic), HTTPS (RATANKBA), SMB (T1021.002)
* Actividad de red: Conexiones a recursos compartidos administrativos (T1049, T1021.002)

## 4. Malware / Herramientas del Grupo [Basado en OpenCTI del curso + análisis complementario]

* Bankshot - Malware utilizado por Lazarus para exfiltración de datos y movimiento lateral, identificado en las relaciones del OpenCTI como "USES → MALWARE → Bankshot" . Se trata de un troyano que contiene la capacidad de consultar información del sistema como hora, zona horaria y versión de la máquina, y ha sido observado en campañas del grupo contra instituciones financieras .

* RATANKBA (aka QUICKRIDE) - Backdoor identificado en el OpenCTI (relación "USES → MALWARE → RATANKBA") que establece persistencia usando la carpeta Startup y se comunica con su servidor C2 mediante HTTPS con una cadena User-Agent estática . Es capaz de recopilar información del sistema, descargar y ejecutar ejecutables, y auto-desinstalarse. Fue utilizado contra bancos en Polonia, México, Uruguay, Reino Unido y Chile . También se han documentado variantes en PowerShell (PowerRatankba) utilizadas en la campaña FastCash 2.0 .

* Cryptoistic - Backdoor identificado en las relaciones del OpenCTI como "USES → MALWARE → Cryptoistic", desarrollado en Swift que utiliza TCP para comunicaciones C2. Se enfoca en objetivos financieros y de criptomonedas, alineándose con la motivación principal del grupo .

* Responder - Herramienta documentada en el OpenCTI (relación "USES → TOOL → Responder") utilizada para poisoning de protocolos LLMNR, NBT-NS y WPAD, capturando hashes de autenticación en redes locales. Esta herramienta permite a Lazarus cosechar credenciales para movimiento lateral y elevación de privilegios en entornos comprometidos .

## 5. Análisis de Inteligencia y Justificaciones:

### Niveles de Inteligencia:

* Táctico: Bankshot, RATANKBA, Cryptoistic, Responder (herramientas y malware específicos)
* Operacional: T1560.003, T1049, T1585.002, T1021.002 (TTPs documentadas)
* Estratégico: Patrón de relaciones USES que muestra un grupo organizado con múltiples capacidades

### TLP Sugerido:

* TLP:AMBER: Bankshot, RATANKBA, Cryptoistic (malware específico con firmas detectables)
* TLP:GREEN: TTPs documentadas y Responder (herramienta pública)
* Política TLP: El equipo de Inteligencia de Amenazas (TI) o el CISO deben fijar la política final

### Modelo Diamante:

* Adversario: Lazarus Group
* Capacidad: Bankshot, RATANKBA, Cryptoistic, Responder, TTPs documentadas
* Infraestructura: Servidores C2 (HTTPS para RATANKBA, TCP para Cryptoistic)
* Víctima: Bancos, instituciones financieras, entidades de criptomonedas

# Acto 2 — Plan de ataque mapeado a ATT&CK

## Análisis de Superficie de Ataque en Metasploitable3

Basándome en el perfil de Lazarus construido con los datos del OpenCTI, identifico qué TTPs pueden ser reproducidos en el laboratorio y cuáles no.

### Tabla de TTPs Reproducibles

| Táctica | Técnica ATT&CK | Servicio/objetivo en Metasploitable3 | Herramienta |
| :---: | :---: | :---: | :---: |
| Initial Access | T1190 - Exploit Public-Facing Application | ElasticSearch (9200) - CVE-2014-3120 | exploits/multi/elasticsearch/script_mvel_rce.rb |
| Discovery | T1049 - System Network Connections Discovery | Sistema objetivo tras RCE | Meterpreter netstat, arp, route |
| Lateral Movement | T1021.002 - SMB/Windows Admin Shares | Recursos compartidos ADMIN,C en Metasploitable3 | Meterpreter psexec, smb_login |
| Exfiltration | T1560.003 - Archive via Custom Method | Archivos sensibles en Metasploitable3 | Meterpreter download, compresión manual |
| Collection | T1005 - Data from Local System | Archivos, hashes, flags en Metasploitable3 | Meterpreter search, hashdump, download |
| Credential Access | T1003.001 - OS Credential Dumping: LSASS Memory | Metasploitable3 | hashdump, mimikatz via Meterpreter |
| Execution | T1059.001 - PowerShell | Sistema objetivo tras RCE | Meterpreter powershell_execute |

### TTPs de Lazarus NO Reproducibles y Justificación

| Técnica ATT&CK | Razón de Exclusión |
| :---: | :---: |
| T1566.001 - Spearphishing Attachment | El laboratorio no tiene un cliente de correo electrónico ni usuarios humanos objetivos para enviar correos maliciosos. Esta técnica requiere interacción social y un entorno de correo configurado. |
| T1547.001 - Registry Run Keys / Startup Folder | Aunque técnicamente posible, el laboratorio es efímero y no se requiere persistencia para demostrar el objetivo del examen. |
| T1547.005 - Security Support Provider (SSP) | Requiere persistencia avanzada y manipulación de DLLs del sistema; excede el alcance del examen y podría afectar la estabilidad del laboratorio compartido. |
| T1486 - Data Encrypted for Impact (Ransomware) | Cifrar datos en el laboratorio podría afectar la disponibilidad del servicio para otros estudiantes y va en contra de las políticas de uso del entorno compartido. |
| T1585.002 - Email Accounts | No hay infraestructura de correo electrónico en el laboratorio para crear o comprometer cuentas de email. |
