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

## PASO 5. Instalción y configuración de WordPress en centro.intranet  y configurar Python para departamentos.centro.intranet. 

•	Instala y configura wordpress 

•	Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python 

•	Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente. 

•	Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación 

•	Instala y configura awstat. 

WordPress es un gestor de contenido hecho en PHP y que requiere de una base de datos como MySql para guardar información. 

Llegados a este punto recopilamos y podemos decir que tenemos todo lo necesario para que WordPress funcione: Apache, PHP y MySql. 

**Paso 5.1 Creación de una base de datos y un usuario para wordpress.** 

Enteramos en la base de datos **sudo mysql** 

Dentro de mysql ejecutamos las siguientes ordenes: 

**CREATE DATABASE wordpress_db;** 

**CREATE USER ‘wordpress_user@localhost’ IDENTIFIED BY ‘user’;** 

GRANT ALL PRIVILEGES ON wordpress_db.* TO ‘wordpress_user@localhost’;

**FLUSH PRIVILEGES;** 

**EXIT;** 

<img width="940" height="545" alt="image" src="https://github.com/user-attachments/assets/9749278c-c744-44b9-bd4b-a7bbbd04d11c" /> 

<img width="940" height="257" alt="image" src="https://github.com/user-attachments/assets/a0180c40-3eb2-43b6-926b-636d22297d32" /> 

Una vez creada vamos a comprobar que existe. 

<img width="277" height="242" alt="image" src="https://github.com/user-attachments/assets/687fe13e-9bb0-46eb-9362-f078005d59ac" /> 

Dicho gestor de contenidos debemos instalarlo en el dominio **centro.intranet** ubicado en **/var/www/centro.intranet.** 

**PPaso 5.2 Descargar WordPress.** 

En principio lo descargamos en el directorio /tmp y para ello ejecutamos 

**cd /tmp** 

**wget https://es.wordpress.org/latest-es_ES.tar.gz** 


<img width="784" height="278" alt="image" src="https://github.com/user-attachments/assets/5d422bd3-61ab-474b-afaf-fa7b4495aeeb" /> 

Podemos ver que se trata de un archivo comprimido por lo que pasamos a descomprimirlo. 

**tar -xvzf latest-es_ES.tar.gz** guardando el contenido en una carpeta llamada wordpress/ 


<img width="603" height="605" alt="image" src="https://github.com/user-attachments/assets/1e0f2a0b-dbda-4842-b3d1-cb0d0a71c130" /> 

Ahora lo movemos a la carpeta donde se instalará. 

**sudo mv wordpress /var/www/centro.intranet/wordpress/** 

Entramos en la carpeta wordpress. 

Wordpress viene con un archivo de ejemplo: **wp-config-sample.php** 

Le hacemos una copia y lo modificamos con nuestros datos. 

**sudo cp wp-config-sample.php wp-config.php** 

**sudo nano wp-config.php** 

### Datos con los que creamos la base de datos anterior desde mysql 


<img width="466" height="325" alt="image" src="https://github.com/user-attachments/assets/3dd786fe-a9f8-4414-9c13-621c5cd1b771" /> 

Asignamos permisos  

**sudo chown -R $USER:$USER /var/www/centro.intranet/wordpress** 

WordPress requiere tener habilitado el módulo **Rewrite** 

**sudo a2enmod rewrite** 


<img width="739" height="128" alt="image" src="https://github.com/user-attachments/assets/78aae7af-68a6-491a-81b5-1ee13558064b" /> 

Pasamos a instalar **wordpress** desde el navegador. 

#### Me salta un mensaje de error que dice “Parece que tu instalación de PHP no cuenta con la extensión de MySql, necesaria para hacer funcionar WordPress. Por favor, comprueba que la extensión de PHP mysqli está instalada y habilitada” 

Para solucionarlo, paso a instalar las extensiones PHP para MySql. 

**sudo apt install php-mysql** 

Lo que instalará la extension **mysqli** entre otras. 

Y reiniciamos Apache. 

**sudo systemctl restart apache2** 

Volvemos al navegador para lanzar la instalación de WordPress y por fin salta. 


<img width="941" height="540" alt="image" src="https://github.com/user-attachments/assets/5967ed0f-056a-4b59-9ec5-04d16cb709c1" /> 

Una vez concluida la instalación me pide credenciales para entrar a WordPress. 


<img width="941" height="390" alt="image" src="https://github.com/user-attachments/assets/a9fef151-8c27-4f77-b6d4-ab5708f41f6e" /> 


<img width="943" height="514" alt="image" src="https://github.com/user-attachments/assets/87bc4691-2117-4a21-bc7e-180ddb1c41b9" /> 

Ahora cada vez que accedamos a **centro.intranet** nos saltará la página que tengamos creada en **wordpress** o por defecto saldrá una página de bienvenida si no hemos creado aún ninguna. 

En este punto, hemos accedido al archivo **centro.intranet.conf** y hemos modificado la línea del DocumentRoot para añadir la carpeta wordpres y de la directiva. 

<img width="940" height="384" alt="image" src="https://github.com/user-attachments/assets/4f719b14-8b01-4ef6-bce0-77d3fbdc1a67" /> 

Para acceder al panel de administración de WordPress hay que acceder desde el navegador con la dirección: 

**http://centro.intranet/wp-admin** 

## PASO 6. Hacer que el sitio departamentos.centro.intranet sirva una aplicación Python. 

**6.1 Activación del módulo mod_wsgi** 

Este módulo de Apache permite ejecutar aplicaciones web Python. 

Instalamos el módulo: 

**sudo apt update** 

**sudo apt install -y libapache2-mod-wsgi-py3 python3-venv python3-pip** 

Habilitamos el módulo y recargamos Apache: 

**sudo a2enmod wsgi** 

**sudo systemctl restart apache2** 


<img width="628" height="91" alt="image" src="https://github.com/user-attachments/assets/77ceaf2c-da59-411a-a0d6-d461e761d5e4" /> 

**6.2 Creación de una mini aplicación Python.** 

**sudo nano /var/www/departamentos.centro.intranet/app.py** 

<img width="940" height="238" alt="image" src="https://github.com/user-attachments/assets/bff75653-7620-4b60-9125-f1ca1ac48efe" /> 

Guardamos y salimos. **Application** es una función que WSGI necesita para ejecutar Python. 

**6.3 Creación del archivo WSGI que usará Apache.** 

**sudo nano /var/www/departamentos.centro.intranet/app.wsgi** 

<img width="940" height="201" alt="image" src="https://github.com/user-attachments/assets/9618333a-18d8-48e3-a2c6-1e79a9058833" /> 

Al fichero de configuración de este sitio hay que incluirle la siguiente línea 

**WSGIScriptAlias / /var/www/departamentos.centro.intraner/app.wsgi** 

**sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf** 

<img width="940" height="395" alt="image" src="https://github.com/user-attachments/assets/3119c060-9360-4b2c-a8f9-edd47d730306" /> 

Guardamos y salimos. 

Activamos el sitio y recargamos. 

<img width="728" height="106" alt="image" src="https://github.com/user-attachments/assets/1583c8a6-ac62-4735-802b-80326864e3ee" /> 

Ahora, vamos a probar la aplicación desde el navegador. 

<img width="599" height="196" alt="image" src="https://github.com/user-attachments/assets/286dc3fb-c7b5-4cd4-b21e-a73a6bd23560" /> 

**6.4 proteger el acceso a la aplicación python mediante autenticación** 

Apache tiene un archivo donde guarda usuarios y contraseñas. 

#### Creación del archivo que guarda los usuarios  

Con el siguiente comando guardamos la contraseña para el usuario José 

<img width="683" height="133" alt="image" src="https://github.com/user-attachments/assets/c57a1780-5b64-48e8-9cba-618c78a95fba" /> 

Ahora, le tenemos que decir a Apache que proteja esta carpeta(../departamentos.centro.intranet) y para ello accedemos al archivo de configuración del Virtual Host  y añadimos lo siguiente:

<img width="940" height="533" alt="image" src="https://github.com/user-attachments/assets/a411c295-197d-49c4-b9c2-47bb8072e221" /> 

Comprobamos sintaxis y reiniciamos apache 

<img width="508" height="99" alt="image" src="https://github.com/user-attachments/assets/721058d5-3cc8-43b8-84bc-4d9665bd6a29" /> 

Ahora vamos a probar que el sistema de autenticación funciona. Para ello, entramos desde el navegador en el sitio y me debe pedir credenciales para entrar. 

<img width="940" height="321" alt="image" src="https://github.com/user-attachments/assets/f8c481e2-ec7a-47c5-906d-c69249c0cdff" /> 

Si intentamos entrar con el siguiente comando me tiene que responder con ‘401 Uhauthorized’. 

<img width="940" height="167" alt="image" src="https://github.com/user-attachments/assets/01eadbe1-b1d5-4cb1-8f0e-4e7af31e722b" /> 

**6.5 Instalación de AWStats.** 

AWStats es un analizador de estadísticas de un servidor web sobre los usuarios que visitan un sitio web. 

**sudo apt install awstats** 

AWStats tiene un fichero de configuración específico para cada sitio.

Vamos a coger uno que viene por defecto en Apache y lo vamos a copiar y posteriormente modificarlo para nuestro sitio. 

Dentro de él, tenemos que configurar el dominio, configurar el alias y finalmente especificar la ubicación del log. 

**sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.departamentos.centro.intranet.conf** 

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/d05aef9b-a836-4e25-9869-1b59863862d7" /> 

Necesitamos activar CGI para poder ver AWStats desde el navegador 

<img width="573" height="280" alt="image" src="https://github.com/user-attachments/assets/4ef3fd14-0a36-421b-9ccb-846c29fa70a2" /> 

Ahora, configuramos Apache para acceder a AWStats desde el sitio. 

Para ello abrimos el fichero de configuración del sitio e incluimos las siguientes líneas: 

<img width="940" height="781" alt="image" src="https://github.com/user-attachments/assets/9d954907-c8fa-4950-bdce-b173680e0745" /> 

Tenemos que generar un primer informe antes de ver estadística alguna. 

<img width="940" height="256" alt="image" src="https://github.com/user-attachments/assets/d9086c92-68b0-4960-95ed-b14cab28b99e" /> 

Ahora, por último, visitamos el sitio desde el navegador, pero para ver las estadísticas sobre el tenemos que poner en el navegador lo siguiente:  

**http://departamentos.centro.intranet/awstats/awstats.pl?config=departamentos.centro.intranet** 

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/21a0b6bb-df75-45d2-9c79-3d682333e1f0" /> 

## PASO 7. Instalación del servidor nginx bajo el dominio “servidor2.centro.intranet”. Configuración para que sirva en el puerto 8080 y realización de los cambios necesarios para ejecutar php. Instala phpmyadmin. 

Este servidor web operará de forma independiente a Apache y escuchará por el puerto 8080. 

**sudo apt update** 

**sudo apt install -y nginx php-fpm php-mysql phpmyadmin** 

Me pregunta sobre que servidor quiero configurar phpMyAdmin. Como no sale nginx como opción le digo que ninguno. Posteriormente configuraremos manualmente phpMyAdmin con nginx. 

<img width="940" height="264" alt="image" src="https://github.com/user-attachments/assets/beba3c72-874f-4ec7-a0d2-4ebcda4a9263" /> 

<img width="940" height="238" alt="image" src="https://github.com/user-attachments/assets/5d63d436-37e6-4a04-b4b6-ce96aae8b060" /> 

<img width="940" height="177" alt="image" src="https://github.com/user-attachments/assets/7d524ac2-c0ab-4719-9616-b829db9b05eb" /> 

La contraseña introducida es: usuario 

Ahora, compruebo el estado de nginx y me da error. 

<img width="940" height="341" alt="image" src="https://github.com/user-attachments/assets/b1fbe19d-5904-4b55-9c21-219c7d7b7aeb" /> 

Posiblemente sea por incompatibilidad con Apache por usar el puerto 80. Lo veremos más adelante. 

Creamos la carpeta del nuevo dominio. 

**sudo mkdir -p /var/www/servidor2.centro.intranet** 

Creo un fichero PHP de prueba dentro del dominio. 

<img width="940" height="143" alt="image" src="https://github.com/user-attachments/assets/ba3eb8c0-b9ae-4605-85be-1e9805335fe2" /> 

Le damos permisos: 

<img width="940" height="91" alt="image" src="https://github.com/user-attachments/assets/02e88174-bcd3-4f24-a054-edd139c38697" /> 

Configuramos Nginx para el dominio y puerto de escucha 8080. 

Para ello, creamos el archivo de configuración del sitio. 

<img width="940" height="502" alt="image" src="https://github.com/user-attachments/assets/4a931940-44d9-4cad-b97f-4559d9a63033" /> 

Activamos el sitio mediante la creación de un enlace simbólico. 

<img width="940" height="43" alt="image" src="https://github.com/user-attachments/assets/e4de7033-74ef-408e-9d41-ebed9ec8c1bf" /> 

Probamos la configuración: 

<img width="934" height="84" alt="image" src="https://github.com/user-attachments/assets/fa98ca3a-f8c4-473c-bfe3-0df788936eaf" /> 

Recargamos y me falla. Miramos su estado y vemos que hay problemas ya que está intentando acceder por el puerto 80. 

<img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/b7b89de7-4468-4022-a198-ec6df6e049de" /> 

Abrimos y cambiamos donde ponga 80 ponemos 8080. 

<img width="940" height="180" alt="image" src="https://github.com/user-attachments/assets/49f4ac1e-aec9-4e1e-bcf4-d02304731e02" /> 

Reiniciamos y no se queja. 

<img width="592" height="44" alt="image" src="https://github.com/user-attachments/assets/88d8b072-c8a9-4039-9cc9-348321363dcd" /> 

Tenemos que incluir en el fichero hosts este nuevo dominio. 

<img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/9dd45e7b-9483-4d8c-890c-f8be5fc75312" /> 

Me esta dando problemas a la hora de que me muestre la página de php por defecto cuando accedo al dominio dándome la página de Apache. Por lo tanto, vamos a hacer una serie de ajustes. 

-	Desactivamos el sitio por defecto.
-	Activo mi sitio
-	Comprobamos que no haya errores de sintaxis.
-	Reiniciamos nginx.
  
<img width="940" height="199" alt="image" src="https://github.com/user-attachments/assets/921ac20c-c4b1-4992-a19b-d3531e4e5329" /> 


Probamos que el dominio funciona y que nginx sirve PHP a través del puerto 8080 mediante el navegador: 

<img width="940" height="411" alt="image" src="https://github.com/user-attachments/assets/066d0147-3628-43f0-a017-bb2d6a1badb8" /> 

#### Vamos a integrar phpMyAdmin con nginx. 

phpMwAdmin se instala por defecto en /usr/share/phpmyadmin. 

Tenemos que indicarle a nginx donde encontrarlo. Para ello, abrimos el archivo de configuración y añadimos una serie de líneas. 

<img width="940" height="668" alt="image" src="https://github.com/user-attachments/assets/1b59dfd6-4824-4ea2-bca3-86a85885d559" /> 

Comprobamos sintaxis y recargamos. 

<img width="888" height="66" alt="image" src="https://github.com/user-attachments/assets/d27db0d6-4a86-4abb-b86c-08d2e9209cc2" /> 

Comprobar phpmyadmin en el navegador. 

<img width="940" height="430" alt="image" src="https://github.com/user-attachments/assets/abfad921-2917-4775-913e-08c2c90a6460" /> 
























































































































