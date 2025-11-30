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

<img width="658" height="296" alt="image" src="https://github.com/user-attachments/assets/36bb283a-998e-4f4b-9f55-864698c9d78d" /> 

Salvamos y salimos. 

Para comprobar que responden hacemos ping a cada dominio. 

<img width="755" height="101" alt="image" src="https://github.com/user-attachments/assets/bc570d56-3b4e-42ef-a5d4-e5bd256c401b" /> 

<img width="754" height="105" alt="image" src="https://github.com/user-attachments/assets/f041401c-452a-4ed8-a412-2cb06c0cca8e" /> 

Para tener más de un dominio alojado en un mismo servidor es necesario crear hosts virtuales. 

Cuando instalamos Apache se crea un Virtual Host por defecto en /var/www/html y este sería suficiente si fuésemos a trabajar con un único sitio web. Este Virtual Host atenderá las solicitudes de clientes que no coincidan con los Virtual Host que crearemos a continuación. 

**Creación de un VirtualHost para centro.intranet.** 

Esto permite a Apache saber que carpeta servir cuando le llegue la petición centro.intranet. 

**sudo mkdir /var/www/centro.intranet** 

Ahora, mediante la variable $USER, asignamos la propiedad del directorio para hacer referencia al usuario que lo ha creado mediante el siguiente comando.  

**sudo chown -R $USER:$USER /var/www/centro.intranet** 

Damos permisos sobre la carpeta centro.intranet 

**chmod -R 755 /var/www/centro.intranet** 

Cada Virtual  Host que se cree debe tener su propio archivo de configuración asociado y para ello ejecutamos y programamos así: 

**sudo nano /etc/apache2/sites-available/centro.intranet.conf** 

<img width="940" height="379" alt="image" src="https://github.com/user-attachments/assets/2e1c00e2-01b9-4dab-80d1-c4ccd66201bd" /> 

Guardamos y cerramos. (Ctrl + o – Enter –Ctrl + x) 

Ahora, activamos el sitio y recargamos Apache. 

**sudo a2ensite centro.intranet** 

**sudo systemctl reload apache2** 

<img width="548" height="98" alt="image" src="https://github.com/user-attachments/assets/306d5d4f-d22f-4269-b7c2-18d267824bfa" /> 

Ahora, probamos que podemos acceder desde el navegador. 

<img width="691" height="242" alt="image" src="https://github.com/user-attachments/assets/6bd0f9c8-a222-4689-9d0d-a5075fd5c01d" /> 

Acceso desde línea de comando **curl -I http:/centro.intranet** 

<img width="545" height="120" alt="image" src="https://github.com/user-attachments/assets/f0bc0cef-f588-4ef5-b342-db4b54a4703b" /> 

### Creación de un Virtual Host para departamentos.centro.intranet. 

Esto permite a Apache saber que carpeta servir cuando le llegue la petición departamentos.centro.intranet. 

**sudo mkdir /var/www/departamentos.centro.intranet** 

Ahora, mediante la variable $USER, asignamos la propiedad del directorio para hacer referencia al usuario que lo ha creado mediante el siguiente comando.  

**sudo chown -R $USER:$USER /var/www/departamentos.centro.intranet** 

Damos permisos sobre la carpeta departamentos.centro.intranet 

**chmod -R 755 /var/www/departamentos.centro.intranet** 

Este VirtualHost también necesita tener su propio archivo de configuración asociado y para ello ejecutamos y programamos así: 

**sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf** 

<img width="956" height="271" alt="image" src="https://github.com/user-attachments/assets/294010a9-c0cf-4a58-9922-d0aab79d7933" /> 

Guardamos y cerramos. (Ctrl + o – Enter –Ctrl + x) 

Ahora, activamos el sitio y recargamos Apache. 

**sudo a2ensite departamentos.centro.intranet** 

**sudo systemctl reload apache2** 

<img width="642" height="118" alt="image" src="https://github.com/user-attachments/assets/4b0499bf-e144-467d-a7f6-343061dc6f2e" /> 

Ahora, probamos que podemos acceder desde el navegador. 

<img width="734" height="263" alt="image" src="https://github.com/user-attachments/assets/925602d2-58c3-408d-9869-57edbcca0c59" /> 

Acceso desde línea de comando **curl -I http:/departamentos.entro.intranet** 

<img width="657" height="122" alt="image" src="https://github.com/user-attachments/assets/99cb8ae0-0f4b-4894-85b0-0f70f59f7599" /> 

## PASO 4. Activación de los módulos necesarios para ejecutar PHP y acceder a MySql. 

**PHP** es el componente que procesa el código para mostrar contenido dinámico al usuario final.
Para instalarlo ejecutamos el siguiente comando: 

**sudo apt install php libapache2-mod-php** 

Una vez instalado comprobamos la versión con el siguiente comando: **php -v** 

<img width="683" height="138" alt="image" src="https://github.com/user-attachments/assets/84676ab0-a0a9-497b-94a3-a4799f1a16a1" /> 

Vamos a probar si el servidor es capaz de procesar PHP. Para ello, vamos a crear un pequeño archivo info.php el cual colgaremos dentro del DocumentRoot del sitio centro.intranet 

**sudo nano /var/www/centro.intranet/info.php** 

<img width="940" height="161" alt="image" src="https://github.com/user-attachments/assets/3bc92c3f-c7a3-4c5a-aec8-17607a8fb281" /> 

Ahora, desde el navegador vamos a acceder a dicho archivo. 

<img width="940" height="345" alt="image" src="https://github.com/user-attachments/assets/9b223a71-05d4-498e-9c2e-f3abaac6e2da" /> 

Esta página muestra información sobre nuestro servidor. Vemos que funciona. Se aconseja su eliminación una vez probado por contener información confidencial sobre nuestro entorno PHP. 

**sudo rm /var/www/centro.intranet/info.php** 

Para la instalación de **MySql** ejecutamos el siguiente comando **sudo apt install mysql-server** 

A continuación, se recomienda ejecutar una secuencia de comandos de seguridad. 

**sudo mysql_secure_installation**.	Tras finalizar intentamos iniciar en la consola de MySql con **sudo mysql** 

<img width="788" height="321" alt="image" src="https://github.com/user-attachments/assets/f35857bc-e745-43dc-9766-dac8d4c5362c" /> 

Salimos de la consola de MySql tecleando exit. 



























