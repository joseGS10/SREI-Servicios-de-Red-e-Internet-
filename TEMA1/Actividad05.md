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





