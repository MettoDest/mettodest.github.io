---
title: "Lab 01 - Instalación de Metasploitable 3"
date: 2026-05-26 21:50:00
categories: [lab, setup]
tags: [metasploitable, virtualbox, kali]
---

## Creación de la VM Metasploitable 3

Metasploitable es una máquina virtual vulnerable diseñada para practicar pruebas de penetración y ciberseguridad en entornos controlados.

En un laboratorio, sirve para simular un sistema real con fallas de seguridad conocidas, permitiendo que los estudiantes o analistas practiquen técnicas de reconocimiento, escaneo, explotación y evaluación de vulnerabilidades de forma segura.

Se utiliza comúnmente junto con herramientas como Metasploit Framework, Nmap y distribuciones como Kali Linux para entrenar en hacking ético, análisis de redes y seguridad informática.

Diferencia clave:

. ova → VM preconfigurada lista para importar (Kali)

. iso → imagen de disco que requiere crear la VM manualmente (Metasploitable)

![Creación de la VM en VirtualBox](https://res.cloudinary.com/dopcqb8wn/image/upload/f_auto,q_auto/imagen_1_wfolda)

![ISO montado en la VM](https://res.cloudinary.com/dopcqb8wn/image/upload/v1780238384/imagen_2_kwm77e.png)


## Configuración de red

El uso de NAT Network facilita la simulación de entornos reales de red, permitiendo que sistemas como Kali Linux, Windows o servidores virtualizados intercambien tráfico entre ellos, realicen pruebas de conectividad, escaneo y monitoreo, sin exponer directamente las máquinas virtuales a la red local de producción.

