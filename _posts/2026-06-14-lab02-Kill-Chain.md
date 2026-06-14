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

. `sudo dhclient -r (Para liberar la IP asignada)`

. `sudo dhclient (Para pedir que nos asignen una IP de nuevo)`

Usando esos dos comando, __mi IP nueva es 10.0.2.10/24__

Ahora conoceremos la IP del metasplotable ejecutando ipconfig.
![IP del metasplotable](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781415507/ip_meta_wyvthy.png)

## 1.2 Escaneo Inicial

Para realizar este escaneo usaremos el comando `sudo nmap -sn 10.0.2.0/24 `

![Escaneo de Red](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781415892/escaneo_de_red_dvkuqs.png)

En esta imagen podemos identificar 4 IPs
  * 10.0.2.1 : Router virtual
  * 10.0.2.2 : Servidor DNS
  * 10.0.2.3 : Nuestro Metasplotable
  * 10.0.2.10: mi propia direccion (kali)

## 1.3 Escaneo Agresivo

Una vez descubierto la direccion IP de nuestra Metasplotable, procedemos a realizar un escaneo de manera agresiva usando el comando `sudo nmap -A -p- 10.0.2.3 `

![Resultado del escaneo agresivo](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781417233/escaneo_de_red_agresivo_z2po3d.png)

## 1.4 Confirmacion de Firewall Activo

Ahora haremos un reconocimiento del Firewall para ver si esta activado o no

![Resultado del Reconocimiento de Firewall](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781442544/reconocimiento_de_Firewall_s7hltc.png)

Usamos el traceroute para hacer el reconocimiento, si bien no es una confirmación directa, ese comando ofrece pistas muy claras para reconocer el firewall.

# 2 Enumeración de servicios

Hasta este punto ya hemos identificado la máquina objetivo y confirmado la presencia de un firewall activado, ahora revisaremos mas a detalle para descubrir los servicios especificos y las versiones.

## 2.1 Escaneo Detallado de Servicios y Versiones con Nmap

Usaremos el comando `sudo nmap -sV -p 22 10.0.2.3`

![Resultado del Escaneo Detallado](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781443440/Escaneo_detallado_de_servicios_u0ms8v.png)

Analizando el escaneo vemos:

* El servicio detectado es __OpenSSH 7.1__ (protocolo 2.0).
* El host responde muy rápido (latencia de 0.0017 s).
* La MAC address indica que es una __máquina virtual de VirtualBox__
 
En conclusión, este escaneo demuestra que __el host está activo y accesible por SSH__, solo que bloquea ciertos tipos de paquetes (como los que usa `traceroute`).

## 2.2 Explotación de la Vulnerabilidad

El escaneo muestra un servidor __OpenSSH 7.1__ corriendo en una máquina VirtualBox. Esta versión es antigua (2015) y, aunque el servidor en sí no tiene vulnerabilidades críticas directas, presenta un riesgo importante para los clientes que se conecten a él. La vulnerabilidad clave es __CVE-2016-0777/CVE-2016-0778__ (roaming), que permite a un servidor malicioso extraer fragmentos de memoria del cliente, pudiendo robar __claves privadas SSH__ u otras credenciales.

Para explotarlo, un atacante necesitaría controlar este servidor o realizar un ataque Man-in-the-Middle. En la práctica, si alguien se conecta desde un cliente OpenSSH vulnerable (versiones 5.4 a 7.1) a esta máquina, podría comprometer sus claves privadas. Esto convierte al host no en el objetivo final, sino en un __trampolín para atacar a los administradores o usuarios__ que lo gestionan.

En términos académicos, esta vulnerabilidad se clasifica como una __falta de control de tasa de intentos__ (lack of rate limiting) o __ausencia de mecanismos de bloqueo en autenticación fallida__. Esto significa que, debido a la configuración predeterminada, el servicio permite intentos ilimitados de autenticación, lo cual expone al sistema a riesgos de ataques de fuerza bruta y ataques de diccionario. En tales ataques, el atacante puede automatizar múltiples combinaciones de nombres de usuario y contraseñas sin ser bloqueado, lo que incrementa las posibilidades de comprometer una cuenta mediante la obtención de credenciales válidas.

# Etapa 3: Acceso

Si bien ya sabemos que podemos hacer varios intentos para obtener acceso incial al sistema a través del servicio SSH.

## 3.1 Descarga de diccionarios

Descargaremos 2 diccionarios uno para los Usernames y otro para los Passwords.

* `top-usernames-shortlist.txt` para Usernames
* `Rockyou.txt` para Passwords

![Diccionario que usaremos](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781447965/diccionarios_wq5rmm.png)

## 3.2 Enumeración de Usuarios con Metasploit

Para realizar esta acción, vamos a correr un scanner de enumeración con la herramienta Metasploit.

```
msfconsole -q
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 10.0.2.3
set USER_FIlE /home/user/Downloads/top-usernames-shortlist.txt
run
```

Corriendo ese comando tenemos este resultado

![Resultado con el nombre de usuario](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781448946/Username_vuk1ep.png)

En el resultado obtenido nos indica que el usuario que reconocio es `vagrant`, por lo que ahora es cuestión de encontrar la contraseña de ese usuario, para poder tener acceso vía SSH a la máqina target.

## 3.3 Ataque de Fuerza Bruta en SSHH

Para esta acción usaremos el siguiente comando, considerando que seguimos dentro del `msfconsole -q`

```
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 10.0.2.3
set USERNAME vagrant
set PASS_FIlE /home/user/Downloads/rockyou.txt
set VERBOSE true
run
```






