# **Práctica Servidores web**
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
