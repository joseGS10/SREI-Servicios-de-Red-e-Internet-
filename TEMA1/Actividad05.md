## ACTIVIDAD 5. Reescritura.

**Nota:** Si no utilizas ficheros .htaccess, incluye las directivas de reescritura dentro de la directiva “directory” correspondiente, tal como se muestra a continuación: 

<Directory /var/www/html> 

    	Options FollowSymLinks 
      
    	RewriteEngine On 
      
    	RewriteBase / 
      
    	RewriteRule ^([a-z]+)/([0-9]+)/([0-9]+)$ operacion.php?op=$1&op1=$2&op> 
      
</Directory> 

Ejercicio1: **Reescribir URL** 

Partimos del fichero operacion.php el cual tenemos que crear y ubicar en el DocumentRoot. 

Para ello, nos movemos al DocumentRoot. 

**cd /var/www/html** 

Y ahora creamos el fichero **operación.php** con 

**sudo nano operación.php** 

<img width="940" height="417" alt="image" src="https://github.com/user-attachments/assets/b6543044-bc2d-435a-a381-8847db268549" /> 

A continuación, realizamos una serie de pruebas para ver que funciona de forma correcta. 

Llamamos a operacion.php con los siguientes valores marcados en la URL. 

<img width="940" height="204" alt="image" src="https://github.com/user-attachments/assets/7ff17a15-2700-4cfd-a5e3-f9e626bc23f0" /> 
Y funciona. 

En este ejercicio, queremos poder modificar la forma en que llamamos al fichero operación.php. La forma normal de llamarlo es escribiendo en la URL del navegador  http://localhost/operacion.php?op=suma&op1=6&op2=8  y queremos reescribir la URL de forma que podamos llamarlo en vez de como php como html de la siguiente forma http://localhost/operacion.html?op=suma&op1=6&op2=8 

Para ello: 

1.	Activamos el módulo mod_rewrite.
   
    **udo a2enmod rewrite** 

	**sudo systemctl restart apache2**

<img width="941" height="126" alt="image" src="https://github.com/user-attachments/assets/6dcd48e4-9c7a-4914-af78-057cbe02135a" /> 


2.	Creamos la directiva Directory siguiente: 

<Directory /var/www/html> 

	RewriteEngine On 
    
	RewriteRule  ^operación\.html$ operación.php 
    
</Directory> 


Para ello, **sudo nano /etc/apache2/sites-available/000-default.conf** 

<img width="943" height="472" alt="image" src="https://github.com/user-attachments/assets/979c9d1a-064c-48d7-947e-a19a6a7b5b7d" /> 










