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

## 2. TTPs Clave (Attack Patterns) [Basado en MITRE ATT&CK y análisis]

* T1560.003: Archive via Custom Method - Lazarus utiliza métodos personalizados para archivar datos robados antes de exfiltrarlos, usando formatos y técnicas propias para comprimir y empaquetar información sensible .

* T1049: System Network Connections Discovery - El grupo realiza descubrimiento de conexiones de red activas para mapear el entorno, identificar sistemas accesibles y planificar movimiento lateral dentro de la red comprometida .

* T1585.002: Email Accounts - Lazarus crea cuentas de correo electrónico falsas o compromete cuentas legítimas para utilizarlas en campañas de spearphishing, estableciendo así canales de entrega de malware .

* T1021.002: SMB/Windows Admin Shares - Utilizan recursos compartidos administrativos (ADMIN,C, IPC$) para movimiento lateral y ejecución remota de comandos, aprovechando credenciales robadas para acceder a otros sistemas .

* T1190: Exploit Public-Facing Application - La técnica que usarás en tu examen. Lazarus la usa para obtener acceso inicial mediante la explotación de vulnerabilidades en aplicaciones expuestas a internet. Ejemplo: Explotación de ElasticSearch (CVE-2014-3120) .
