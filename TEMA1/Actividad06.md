## ACTIVIDAD_6_TEMA1

**Configuración Vitual Host de Apache en Ubuntu.** 

En esta práctica vamos a llevar a cabo la creación y configuración de 2 sitios(dominios) en un mismo servidor. 


**Paso1.** Crear la estructura del directorio 

Vamos a crear dos sitios: 

- ejemplo.com
  
- pruebas.com
  
Los crearemos en el directorio principal en el que Apache busca el contenido a mostrar (/var/www) 


**sudo mkdir /var/www/ejmplo.com** 

**sudo mkdir /var/www/pruebas.com** 


**Paso2.** Otorgamos permisos. 


Para que puedan acceder a dichos sitios nuestro usuario regular y pueda modificar los archivos del sitio, se necesita cambiar el propietario haciendo: 


**sudo chown -R $USER:$USER /var/www/ejemplo.com** 

**sudo chown -R $USER:$USER /var/www/pruebas.com** 


la variable **$USER** tomará el valor del usuario con el que actualmente estas identificado y pasa a ser propietario de los directorios(sitios). 


Además , sobre esos fichero debemos dar los permisos de lectura para que los archivos y directorios para que las páginas puedan ser desplegadas. 


**sudo chmod -R 755 /var/www** 


De esta forma, el servidor tiene ahora los permisos necesarios para mostrar el contenido y el usuario sra capaz de crear contenido en los directorios a medida que sea necesario. 



**Paso3.** Crear una página de prueba para cada virtual host. 


Crearemos un index.html en cada sitio. 


**sudo nano /var/www/ejemplo.com/index.html** 

<img width="1004" height="235" alt="image" src="https://github.com/user-attachments/assets/1c44d8aa-ab4e-4abf-9067-9c2e0b64e149" /> 
**sudo nano /var/www/pruebas.com/index.html** 
<img width="1004" height="249" alt="image" src="https://github.com/user-attachments/assets/49d76fa6-7c07-4437-bd53-47b8a99a6904" /> 

**Paso4.** Crear nuevos archivos Virtual Host 

Los archivos virtual host son archivos que contienen información y configuración específica para el dominio y le indican al servidor Apache cómo responden a las peticiones de varios dominios. 

Aunque Apache contiene un virtual host por defecto “000-default-conf”,  lo deseable y nosotros lo haremos así, es crear un virtual host para cada sitio o dominio. 

**Archivo Virtual Host para ejemplo.com** 

**sudo nano /etc/apache2/sites-available/ejemplo.com.conf** 
<img width="1004" height="448" alt="image" src="https://github.com/user-attachments/assets/2e7289f2-4226-4cd2-979d-a6f601ca6a54" /> 
**Archivo Virtual Host para pruebas.com** 

**sudo nano /etc/apache2/sites-available/pruebas.com.conf** 
<img width="1004" height="273" alt="image" src="https://github.com/user-attachments/assets/a90d1e9a-58c1-4a7b-8665-d803a806fc7d" /> 
**Paso5.**  Habilitar los nuevos archivos Virtual Host 

**sudo a2ensite ejemplo.com.conf** 

**sudo a2ensite.pruebas.com.conf** 
<img width="1004" height="178" alt="image" src="https://github.com/user-attachments/assets/d7be444b-f616-46e9-a4d7-9f2d7e123c48" /> 

Ahora, se reinicia Apache 

**sudo service apache2 restart** 

**Paso 6.** Configuración archivos locales. 

En nuestro caso estamos trabajando en local, por lo que necesitamos modificar el archivo hosts para que los sitios que hemos creado respondan cuando accedamos a ellos. 

**sudo nano /etc/hosts** 

<img width="973" height="444" alt="image" src="https://github.com/user-attachments/assets/b87fa296-dd03-456d-a638-cb0b43263c37" /> 
Guardamos y salimos. **( Ctrl + o), Intro, (Ctrl + x)** 


**Paso 7.** Comprobación de que nuestros sitios están activos y funcionando. 

Comprobación mediante el navegador. 
<img width="1004" height="182" alt="image" src="https://github.com/user-attachments/assets/5c881d44-75cb-486f-840b-afae51b22396" /> 
<img width="1004" height="191" alt="image" src="https://github.com/user-attachments/assets/df1e28b0-0dae-4b8c-a7b2-a3c54fb9ca1a" /> 
También podemos hacer comprobaciones desde línea de comandos 
<img width="1004" height="272" alt="image" src="https://github.com/user-attachments/assets/426d0c1e-da3c-4931-bcc0-873a1d027914" /> 
<img width="992" height="275" alt="image" src="https://github.com/user-attachments/assets/d6f1bb13-4bfd-41a9-96a3-2b88bd3eb3df" /> 













