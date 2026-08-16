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
