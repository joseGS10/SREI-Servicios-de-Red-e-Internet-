# Práctica Servidores web
1º trimestre

Vamos a instalar un servidor web interno para un instituto. Se Pide:

•	Instalación del servidor web apache. Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en Python

•	Activar los módulos necesarios para ejecutar php y acceder a mysql

•	Instala y configura wordpress

•	Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python

•	Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.

•	Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación

•	Instala y configura awstat.

•	Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.


## PASO 1. Creación de una MV en Proxmox con el sistema operativo Ubuntu Desktop 25.10.
Dicha MV la crearemos en el nodo marisma000 y llevará por nombre “Practica-Servidor-Web-Instituto-JB-SRI”( SID: 402). Su configuración hardware: 

•	2 núcleos 

•	8 GB de RAM 

•	50 GB de disco duro 

Utilizaremos la versión Ubuntu  desktop 25.10. 

Usuario: Jose	 

Nombre equipo: Ubuntu25.10 

Contraseña: 1234 

## PASO 2. Instalación del Servidor Web APACHE.
Antes de nada, vamos a actualizar el sistema sobre el que vamos a trabajar (actualizaciones software y seguridad). 

sudo apt update (actualiza la lista de programas disponibles) 

sudo apt upgrade (descarga e instala versiones más nuevas de los programas ya instalados) 

Pasamos a instalar Apache.			**sudo apt install apache2** 


Verificamos su estado.			**sudo systemctl status apache2**

<img width="940" height="381" alt="image" src="https://github.com/user-attachments/assets/416b7d7c-43e8-47b2-bff8-532cb228ff98" /> 

Verificación desde navegador		**http:/localhost** 

<img width="940" height="551" alt="image" src="https://github.com/user-attachments/assets/d3cc5148-b196-4624-88c9-77f0d182b13a" /> 

Verificación desde línea de comandos	**curl -I http:/localhost** 

<img width="940" height="331" alt="image" src="https://github.com/user-attachments/assets/395490c7-6a50-4de7-a4e8-314fc880aca5" /> 

Configuración del firewall para permitir tráfico HTTP por el puerto 80. 

**sudo ufw allow in “Apache”** 

## PASO 3. Creación de los dominios centro.intranet y departamentos.centro.intranet  

Creación de los dominios centro.intranet y departamentos.centro.intranet 

Usaremos ambos dominios mediante los archivos .hosts 

Este archivo se encuentra en /etc/hosts y tendremos que editarlo con los nombres de los nuevos dominios apuntando a 127.0.0.1 ya que el servidor web y el navegador están en la misma máquina, es decir, los dominios internos apuntarán a la propia máquina.
**sudo nano /etc/hosts**



