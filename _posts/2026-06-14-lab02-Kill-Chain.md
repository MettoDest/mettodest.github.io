---
title: "Lab 02 - Kill Chain 1"
date: 2026-06-14 00:18:00
categories: [lab, setup]
tags: [metasploitable, virtualbox, kali, kill Chain]
---
# 1 RECONOCIMIENTO
## 1.1 Configuración del Segmento de Red

.En nuestra VM Kali Linux ejecutamos el siguiente comando para visualizar la dirección IP asignada para la interfaz virtual de red Ethernet.

![configuración del segmento de red](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781415781/ip_kali_ydi8vu.png)

Como vemos en la Imagen, la IP que se nos asigno fue la 192.168.1.4/24
Como no es la IP que necesitamos para que tengo comunicacion con la red, lo procedemos a cambiar usando los siguientes comandos:

. sudo dhclient -r (Para liberar la IP asignada)

. sudo dhclient (Para pedir que nos asignen una IP de nuevo)

Usando esos dos comando, mi IP nueva es 10.0.2.10/24

Ahora conoceremos la IP del metasplotable ejecutando ipconfig.
![IP del metasplotable](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781415507/ip_meta_wyvthy.png)

## 1.2 Escaneo Inicial

Para realizar este escaneo usaremos el comando sudo nmap -sn 10.0.2.0/24

![Escaneo de Red](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781415892/escaneo_de_red_dvkuqs.png)

En esta imagen podemos identificar 4 IPs
  . 10.0.2.1 : Router virtual
  . 10.0.2.2 : Servidor DNS
  . 10.0.2.3 : Nuestro Metasplotable
  . 10.0.2.10: mi propia direccion (kali)

## 1.3 Escaneo Agresivo

Una vez descubierto la direccion IP de nuestra Metasplotable, procedemos a realizar un escaneo de manera agresiva usando el comando sudo nmap -A -p- 10.0.2.3

![Resultado del escaneo agresivo](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781417233/escaneo_de_red_agresivo_z2po3d.png)

## 1.4 Confirmacion de Firewall Activo

Ahora haremos un reconocimiento del Firewall para ver si esta activado o no

![Resultado del Reconocimiento de Firewall](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781442544/reconocimiento_de_Firewall_s7hltc.png)

Usamos el traceroute para hacer el reconocimiento, si bien no es una confirmación directa, ese comando ofrece pistas muy claras para reconocer el firewall.










