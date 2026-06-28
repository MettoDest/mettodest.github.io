---
title: "Tarea: Investigación de TTPs-OilRig(APT34)"
date: 2026-06-28 18:28:00
categories: [Investigacion, setup]
tags: [OilRig, Investigacion]
---







| TTP |  Descripción  | Ejecucion | Producto |
| :--- | :--- | :--- | :---: |
| T1592 |Es la recopilación de información técnica sobre los sistemas de la víctima (versión del SO, arquitectura, parches de seguridad instalados y software en ejecución). Permite al atacante elegir el exploit o la herramienta exacta que funcionará contra ese sistema específico, ahorrando tiempo y evitando fallos. |  |  |
| T1583 | Es la acción de adquirir o alquilar servidores, dominios web, VPS o cuentas en la nube para ser utilizados en el ataque. Brinda al atacante una plataforma externa para alojar sus herramientas, páginas de phishing y servidores de comando y control (C2), separando su identidad real de la infraestructura maliciosa. |  |  |
| T1566.002 | Es una técnica de ingeniería social donde el atacante envía un correo electrónico personalizado a un objetivo específico, conteniendo un enlace malicioso. Permite el acceso inicial a la red de la víctima de forma engañosa. Al hacer clic en el enlace, se descarga malware o se roban credenciales de acceso sin que el usuario se dé cuenta. |  |  |
| T1053.005 |  Es la creación o modificación de tareas programadas en el sistema operativo Windows para ejecutar un programa malicioso. Otorga persistencia al atacante, asegurando que el malware se reactive automáticamente cada cierto tiempo o cada vez que el equipo se reinicie, incluso si el usuario intenta cerrarlo. |  |  |
| T1078.002 | Es el uso de credenciales (usuario y contraseña) robadas de una cuenta real y activa dentro de un dominio de Active Directory. Otorga un acceso legítimo y privilegiado a la red corporativa, permitiendo al atacante moverse lateralmente por otros equipos sin levantar sospechas, ya que el sistema reconoce las credenciales como válidas. |  |  |
| T1068 | Es la explotación de una vulnerabilidad de seguridad en el sistema operativo o en un software instalado para obtener más permisos. Permite al atacante elevar sus privilegios desde un usuario estándar hasta un administrador o cuenta SYSTEM, dándole control total sobre la máquina comprometida. |  |  |
| T1082 | Es la recopilación de datos básicos del sistema infectado, como el nombre del equipo, la versión exacta de Windows, el idioma y la arquitectura. Permite al atacante entender el entorno en el que está operando, lo cual es fundamental para saber qué comandos usar, cómo comportarse y si el sistema es valioso para sus objetivos. |  |  |
| T1033 | Es la acción de identificar qué cuentas de usuario están conectadas actualmente al sistema o quiénes tienen sesión abierta en el dominio. Brinda al atacante información clave para decidir a qué usuario suplantar o atacar, y para saber si hay algún administrador observando el sistema en ese momento. |  |  |
| T1016 | Es la recopilación de la configuración de red del equipo, como direcciones IP, servidores DNS, máscaras de subred y puertas de enlace predeterminadas. Permite al atacante mapear la red interna desde la perspectiva de la víctima, sabiendo por dónde puede salir el tráfico, qué subredes existen y hacia dónde moverse. |  |  |
| T1087 | Es la enumeración y listado de todos los usuarios y grupos locales o del dominio de Active Directory que existen en el sistema. Permite al atacante identificar potenciales objetivos (como administradores o altos ejecutivos) cuyas cuentas podrá robar o suplantar para seguir avanzando en la red. |  |  |
| T1069 | Es la búsqueda y descubrimiento de los grupos de seguridad y sus niveles de acceso dentro del sistema (ejemplo: "Administradores", "Usuarios del dominio"). Permite saber quién tiene acceso a qué. Al conocer los grupos privilegiados, el atacante sabe a qué usuarios debe robarles las credenciales para obtener el máximo control posible. |  |  |
| T1021.001 | Es el uso del protocolo RDP (Escritorio Remoto) para conectarse de forma interactiva a otra máquina dentro de la misma red. Permite el movimiento lateral. Usando credenciales robadas, el atacante puede saltar a otros ordenadores y tomar el control gráfico de ellos, como si estuviera sentado frente a esa pantalla. |  |  |
| T1005 | Es la acción de buscar y recolectar datos sensibles y archivos almacenados directamente en el disco duro local de la máquina infectada (documentos, hojas de cálculo, imágenes, etc.). Permite el robo directo de información corporativa o personal que esté guardada en el equipo, cumpliendo el objetivo final de muchos ataques (el espionaje o la extorsión). |  |  |
| T1074.001 | Es el proceso de reunir, organizar y comprimir (en un solo archivo o carpeta) todos los datos robados dentro de la máquina víctima, antes de enviarlos al atacante. Permite al atacante preparar la exfiltración de una forma más rápida y silenciosa, reduciendo el tráfico de red al enviar un solo paquete en lugar de muchos archivos sueltos. |  |  |
| T1071.001 | Es el uso de los protocolos estándar de internet HTTP o HTTPS (puertos 80 y 443) como medio de comunicación entre el malware y el servidor del atacante. Permite que el malware envíe órdenes y reciba instrucciones sin ser detectado, ya que camufla el tráfico malicioso como si fuera una simple navegación web normal. |  |  |
| T1572 | ¿Qué es? Es la técnica de encapsular el tráfico malicioso (como comandos) dentro de otros protocolos de red que normalmente están permitidos, como las consultas DNS o el tráfico ICMP (ping). Permite evadir firewalls y sistemas de detección de intrusos (IDS), ya que el tráfico malicioso viaja dentro de un "túnel" que parece legítimo y suele estar autorizado a salir de la red. |  |  |
| T1048.003 | Es el envío de los datos robados hacia el exterior utilizando un protocolo diferente al que usa el malware para recibir órdenes, y a menudo sin cifrar o con ofuscación básica. Permite al atacante sacar la información de la empresa. Al usar un protocolo alternativo, el atacante intenta que la exfiltración pase desapercibida para los analistas de seguridad, quienes suelen enfocarse solo en el canal de comando principal. |  |  |
