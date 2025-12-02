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

**sudo apache2ctl configtest**  -> comprobamos sintaxis OK?	 

**sudo systemctl restart apache2**   -> reiniciamos Apache para que coja los cambios 

Por último, probamos que funciona dicha reescritura. Para ello desde el navegador  y va OK. 

<img width="941" height="166" alt="image" src="https://github.com/user-attachments/assets/444a1a2a-5444-4391-a4a5-abff2a1ffb6a" /> 


Ejercicio 2.**Cambiar la extensión de los ficheros** 

Si queremos usar la extensión do en vez de html podriamos usar este .htacces 

	Options FollowSymLinks 
	
	RewriteEngine On 
	
	RewriteRule ^(.+)\.do$ $1.html [nc] 
	

Pero el acceder a la misma página con dos URL distintas está penalizado. Para evitarlo se hace una redirección 

	RewriteRule ^(.+)\.do$ $1.html [r,nc] 
	

No estoy utilizando .htacces 

Entramos en sudo nano /etc/apache2/sites-available/000-default.conf 

<img width="940" height="575" alt="image" src="https://github.com/user-attachments/assets/1ada8ddc-1e39-41f6-8d76-2bd362e9bb40" /> 

comprobamos sintaxis y reiniciamos. 

<img width="940" height="74" alt="image" src="https://github.com/user-attachments/assets/21087bce-f6cc-4902-bcb9-1430022437db" /> 

Ejercicio 3. **Crear URL amigables.** 

El fichero del punto 1 operación.php se podría llamar de otra forma también: 

	http://localhost/suma/8/6 
	
Para ello, tendremos que crear lo siguiente en 

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/1b17ffb1-0e1a-4a96-8b95-0dc57e872f2b" /> 

comprobamos sintaxis 

**apachectl -t** 

Hay que reiniciar para que os cambios tengan efecto. 

**sudo systemctl restart apache2** 


Ejercicio 4. **Acortar URL.** 

Para ello vamos a crear la siguiente regla de reescritura 

**RewriteRule ^buscar/ (.+) http://www.google.es/search?q=$1** 

de esta forma cuando yo escribo en el buscador buscar/ (lo que sea) → busca dentro de google (ese, lo que sea). 

<img width="940" height="302" alt="image" src="https://github.com/user-attachments/assets/261d71be-2266-43d6-8b0a-b47b8d690912" /> 

Guardamos, comprobamos sintaxis y reiniciamos.	 










