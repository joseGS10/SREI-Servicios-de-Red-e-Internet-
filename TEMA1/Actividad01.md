# Convirtiendo Linux en un Servidor Web. Instalación de Apache, MySql y PHP en Ubuntu 24.04
## PASO 1 Instalación de Apache y actualización del firewall
**Apache** es uno de los mejores servidores web para alojar sitios web.  
El 1er comando que debemos ejecutar es **sudo apt update** el cual actualiza los paquetes disponibles en el repositorio. Este comando siempre se debería ejecutar antes de instalar cualquier paquete.  
A continuación, ejecutamos el comando **sudo apt install apache2**, el cual instalará el software de Apache.   

<img width="940" height="571" alt="image" src="https://github.com/user-attachments/assets/d20b7a50-b2b6-4ab9-81bc-98c5db1ca125" />  

Tras confirmar que, **sí** deseamos continuar, se concluye instalando el paquete apache2  
Seguidamente, se debe configurar el firewall para permitir tráfico HTTP y HTTPS.  
Como no tenemos certificado TSL/SSL para permitir tráfico HTTPS en el servidor, lo configuraremos para permitir el tráfico únicamente por el puerto 80 (tráfico web no cifrado)  
Y para ello ejecutamos el comando **sudo ufw allow in “Apache”** en cual abre únicamente el puerto 80(tráfico web normal no cifrado) . De esta forma ya se permite tráfico en el puerto 80 a través del firewall.  

<img width="665" height="151" alt="image" src="https://github.com/user-attachments/assets/54dd1302-78ca-4476-a162-9f71c6246ef1" />  

Ahora, comprobamos que el **servidor Apache funciona**.  
Para ello, tenemos 2 formas:  
* nos vamos al navegador y ponemos **http://localtost**  
* o *curl http://localhost**    
y tiene que aparecer la página de bienvenida de  Apache como vemos en la siguiente imagen:

<img width="941" height="593" alt="image" src="https://github.com/user-attachments/assets/b9f14387-5714-4308-b4f5-fd9bd6d3fbed" />  

También podemos averiguar nuestra IP mediante el comando **hostname -I** y a continuación en el navegador ponemos **http://nuestra_ip** y debería salirnos la pagina de bienvenida de Apache  

## PASO 2 Creación de un host virtual para un mi sitio web.
Esto es necesario cuando se quiere alojar más de un dominio en un mismo servidor.  
Cuando instalamos Apache , se nos crea un virtual host por efecto en /var/www/html y este es suficiente si trabajamos con un único sitio web. Pero si vamos a guardar varios sitios web en deseable crear un virtual host en /var/www/my_domain para cada sitio web.  
Dejaremos /var/www/html establecido como directorio predeterminado que se presentará si una solicitud de cliente no coincide con ningún otro sitio.  
Paso a crear el directorio para my_domain con el siguiente comando.  
sudo mkdir /var/www/my_domain  
<img width="633" height="161" alt="image" src="https://github.com/user-attachments/assets/29112940-9ccf-424c-9536-6153a07b2157" />
Ahora, mediante la variable $USER, asignamos la propiedad del directorio para hacer referencia al usuario que lo ha creado  mediante el siguiente comando.  
sudo chown -R $USER;$USER /var/www/my_domain  
<img width="642" height="61" alt="image" src="https://github.com/user-attachments/assets/da50bcd5-c18a-4ace-a8b7-4aa4b6d8866b" />
Cada virtual host que yo cree debe tener su archivo de configuración.  
Ahora, con nano, abrimos un archivo de configuración de nombre my_domain.conf en el directorio sites-available de Apache.  
sudo nano /etc/apache2/sites-available/my_domain.conf  
De esta forma entramos en el editor nano y generamos el siguiente script:  
<img width="631" height="220" alt="image" src="https://github.com/user-attachments/assets/894df99a-7cea-4b4f-80e8-7dad01efac43" />
(el * sirve para indicar en caso de que haya varias direcciones de red, en cuál de ellas debe atender las peticiones). Este virtual host servirá por el puerto 80 y al poner * puede escuchar cualquier petición venga de donde venga.  
Con esta configuración de VirtualHost, le decimos a Apache que proporcione my_domain usando /var/www/my_domain como directorio root web.  
Para habilitar este nuevo host virtual usamos a2ensite(aplicación que trae Apache)  
en -> significa enable  	 
sudo a2ensite my_domain  
<img width="940" height="223" alt="image" src="https://github.com/user-attachments/assets/7658e79b-eaa5-400e-94e8-70591490fa14" />
Ahora vamos a deshabilitar el sitio web(virtual host) que crea por defecto Apache con el fin de que cuando se realicen búsquedas vaya directamente al sitio que nosotros hemos creado(nuestro virtualhost). Para ello ejecutamos: sudo a2dissite 000-default  
<img width="542" height="141" alt="image" src="https://github.com/user-attachments/assets/4ac81e00-7f2f-457d-aebe-059b41084795" />
En otro momento, volveremos a habilitarlo.  
Para asegurarnos de que el archivo de configuración que acabamos de crear no contenga errores de sintaxis, debemos ejecutar el siguiente comando: sudo apache2ctl configtest ya que si Apache detecta cualquier error en el archivo de configuración no arrancará y además no nos dirá por qué.  
<img width="940" height="118" alt="image" src="https://github.com/user-attachments/assets/c0dd2419-cbae-4ca6-98bb-9194c9cc6c8b" />
Por último, tenemos que volver a recargar Apache para que todos estos cambios surtan efecto mediante el comando:  
sudo systemctl reload apache2  
Otra forma de hacer que los cambios surtan efecto es parando Apache y volvirendolo a poner en marcha.  
<img width="428" height="65" alt="image" src="https://github.com/user-attachments/assets/f2075c4d-8e61-480d-94f0-b1382beb1009" />
<img width="428" height="47" alt="image" src="https://github.com/user-attachments/assets/2fcb7023-8a38-47cb-aff4-65124a404bc1" />
Y para comprobar que vuelve a funcionar:  
curl http://localhost  
<img width="436" height="114" alt="image" src="https://github.com/user-attachments/assets/1868a9f3-0709-41f3-b016-52fb14119bb0" />
O también desde el navegador poniendo http://localhost.  
Ahora, nuestro nuevo sitio web está activo, el que crea por defecto Apache está deshabilitado, pero el directorio root web /var/my_domain está aún vacío. Entonces, para probar que nuestro virtual Host funciona vamos a crear dentro del directorio my_domain un index.html que nos muestre un mensaje cuando accedemos a nuestro localhost.  
Para ello, nano /var/www/my_domain/index.html, y escribimos lo siguiente:  
<h1> Bienvenido al Servidor Apache de Linux </h1>  
<p> Parece que todo funciona </p>  
<i> Sigue avanzando…… </i>  
<img width="940" height="602" alt="image" src="https://github.com/user-attachments/assets/fbfd6184-564b-445e-90fe-89adc091da37" />
Ctrl + o, Enter, Ctrl + x.  
Ahora, desde el navegador o línea de comandos, mediante el nombre de dominio(no dado de alta aún) o la IP de nuestro servidor(o localhost), tenemos que poder visualizar el mensaje del archivo index.html.  
(hay que decir que como el nombre de dominio no lo hemos dado de alta aún no podremos usarlo en este caso todavía)  
Desde el navegador ejecutamos: http://localhost  
<img width="940" height="268" alt="image" src="https://github.com/user-attachments/assets/71910b53-7840-4f12-838b-71caabc22a4c" />
Desde línea de comandos: curl http://localhost  
<img width="940" height="168" alt="image" src="https://github.com/user-attachments/assets/471ce191-8b7b-4937-ad1f-129b37bc7d86" />
Como podemos ver funciona. Dejaremos este archivo index.html como pagina de destino temporal de nuestro sitio hasta que se configure un archivo index.php que lo sustituya. Cuando lo hagamos, tendremos que eliminar el archivo index.html del root de documentos, o cambiarle en nombre, ya que tendría precedencia sobre un archivo index.php por defecto.  
Una última nota.  
¿Qué es DirectoryIndex?. Es el nombre de la página que voy a servir por defecto. En un principio, hemos creado un index.html que tendría prioridad sobre un index.php.   
Esta situación de prioridad del .html sobre el .php es útil para establecer páginas de mantenimiento de aplicaciones PHP, dado que se puede crear un archivo index.html temporal que contenga un mensaje informaivo para los visitantes. Una vez finalizado el mantenimiento, el archivo index.html se eliminará del root de documentos o se renombrará para volver a mostrar la página habitual de la aplicación(el index.php)  
Para programar este comportamiento, tendremos que editar el archivo /etc/apache2/mods-enabled/dir.conf y modificar el orden en el que el archivo index.php se enumera en la directiva DirectoryIndex.  
sudo nano /etc/apache2/mods-enabled/dir.conf  
<img width="940" height="184" alt="image" src="https://github.com/user-attachments/assets/30ec2486-b408-4290-9cdd-f4650b72b71b" />
En el archivo podemos ver como index.html aparece en primer lugar, luego tendrá preferencia sobre index.php que está más retrasado. Pues nosotros podemos modificar ese orden según nos interese que responda index.php o index.html ante una petición a nuestro sitio. Cuando se realiza una petición de una pagina se irá accediendo a los archivo en el orden en que aparecen. Si uno no esta salta al siguiente y así funciona.  
Después de guardar y cerrar el archivo, tendremos que volver a cargar Apache para que los cambios surtan efecto mediante el siguiente comando:  sudo systemctl reload apache2  

## PASO 3 Instalación de PHP.
PHP es el componente que procesa el código para mostrar contenido dinámico al usuario final.  
Para instalarlo ejecutamos el siguiente comando:  
sudo apt install php libapache2-mod-php  
Una vez instalado comprobamos la versión con el siguiente comando:  
php -v  
<img width="563" height="117" alt="image" src="https://github.com/user-attachments/assets/2b138095-e915-4b97-bf1a-eb0a77fc6d81" />

## PASO 4 Probar el procesamiento de un archivo PHP en el servidor web.
A continuación, vamos a crear una página php y vamos a ver el funcionamiento, vamos a verificar que Apache puede gestionar solicitudes y procesar solicitudes de archivos PHP.  
1ro creamos un nuevo archivo llamado info.php (podría haber sido index.php) en nuestra carpeta root web.  
nano /var/www/my_domain/info.php  
<img width="940" height="195" alt="image" src="https://github.com/user-attachments/assets/22eb621c-409d-419e-aa98-6093288647fd" />
Una vez programado, guardamos y salimos de nano.  
Para probar este archivo, nos vamos al navegador web y accedemos a nuestra ip(localhost) del servidor seguido del nombre del archivo.  
http://localhost/info.php	y si nos muestra la siguiente pantalla quiere decir que toda va funcionando.  
<img width="940" height="767" alt="image" src="https://github.com/user-attachments/assets/9a7e6b28-0cc8-4cb8-ba9a-ad399493ec38" />
Esta página muestra información básica sobre nuestro servidor desde la perspectiva de PHP. Se aconseja una vez probado el funcionamiento, eliminar el archivo creado info.php ya que contiene información confidencial sobre nuestro entorno PHP y servidor de Ubuntu.  
Para ello, ejecutamos sudo rm  /var/www/my_domain/info.php  

## PASO 5 Instalación de MySQL
Ahora que tenemos un servidor web funcional, tenemos que instalar un sistema de base de datos para poder almacenar y gestionar los datos de un sitio. MySQL es un sistema de administración de bases de datos popular que se utiliza en entornos PHP.  
Lo instalamos con el siguiente comando:   
sudo apt install mysql-server  
Una vez instalado se recomienda ejecutar una secuencia de comandos de seguridad que viene preinstalada en MySQL la cual eliminará algunos ajustes predeterminados poco seguros y se bloqueará el acceso a un sistema de base de datos. La secuencia es la siguiente:  
sudo mysql_secure_installation   nos pregunta si queremos configurar el VALIDATE PASSWORD PLUGIN (la dejaremos deshabilitada ya que no supone peligro alguno).  
<img width="711" height="294" alt="image" src="https://github.com/user-attachments/assets/e782d8cf-4726-4633-98e7-8c492a270c2f" />
Nosotros le indicamos que no queremos activar clave alguna.   
<img width="940" height="264" alt="image" src="https://github.com/user-attachments/assets/12236bf1-1d96-49f4-b861-1a001a665f0d" />
Independientemente de que elijamos instalar el VALIDATE PASSWORD PLUGIN o no, el servidor nos pedirá una contraseña para el usuario root de MySQL que no tiene nada que ver con el root del sistema. El usuario root de base de datos es un usuario administrativo con privilegios completos sobre el sistema de base de datos.  
Para el resto de las preguntas contestaremos con sí , y Enter en cada mensaje. Con ello, eliminaremos algunos usuarios anónimos y la base de datos de prueba, se deshabilitarán las credenciales de inicio de sesión remoto de root y se cargarán estas nuevas reglas para que MySQL aplique de inmediato los cambios que realizó.  
<img width="940" height="778" alt="image" src="https://github.com/user-attachments/assets/b973f73a-5aa2-4a62-8cb8-cc2fbae6d4e9" />
Tras finalizar, comprobamos que se puede iniciar en la consola de MySQL escribiendo lo siguiente:  
sudo mysql  
<img width="940" height="356" alt="image" src="https://github.com/user-attachments/assets/005ebe7f-2114-4cf1-96a1-ad07011229c7" />
Para salir de la consola de MySQL tecleamos exit  
























