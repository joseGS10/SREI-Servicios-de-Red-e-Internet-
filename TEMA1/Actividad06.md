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




