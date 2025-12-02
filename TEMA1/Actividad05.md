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










