#ACTIVIDAD 3. TEMA1 

Ejercicios 

**1. Crea un directorio llamado "dir1" y otro llamado "dir2"**
   
Creamos los dos directorios en **/var/www/** que es el directorio por defecto donde Apache  

guarda los sitios web  

**sudo mkdir /var/www/dir1 /var/www/dir2**

<br> 

**2. Explica qué diferencia existe entre ambos y muestra su equivalencia con la
directiva Require:** 

**Ambas están ya en desuso.** 

**<Directory /var/www/example1>**

**Order Deny,Allow**

**Deny from All**  

**Allow from 192.168.1.100** 

**&lt;/Directory&gt**



En este primer caso, el orden es : primero se deniega a todos y luego se permite acceso al  

recurso a la IP especificada. Concluimos diciendo que el único que tiene acceso al recurso es  

la ip especificada ya que la última orden que se ejecuta es **Allow**.  

La **directiva** equivalente con **Require**: 

<Directory /var/www/example1>  

Require ip 192.168.1.100  

&lt;/Directory&gt; 


**<Directory /var/www/example1>** 

**Order Allow,Deny** 

**Deny from All**

**Allow from 192.168.1.100** 

**&lt;/Directory&gt;** 

En este segundo caso: primero se permite acceso al recurso a la IP especificada y luego se  

deniega a todos. Por lo que concluimos diciendo que este recurso está denegado para todos ya  

que la orden **Denny** es la última que se ejecuta..  

La directiva equivalente con Require: 

&lt;Directory&gt;  
   
Require all denied  

&lt;/Directory&gt;  

<br>

**3. Para dir1** 

**a. Perrmite el acceso de las peticiones provenientes de 10.3.0.100** 

**b. Permite el acceso desde "marisma.intranet"** 

**c. Permite el acceso desde cualquier subdominio de "marisma.intranet"** 

**d. Permite el acceso de las peticiones provenientes de "10.3.0.100" con máscara "255.255.0.0"** 

<Directory /var/www/dir1> 

&nbsp;Require ip 10.3.0.100  

Require host marisma.intranet  

Require host .marisma.intranet  

Requiere ip 10.3.0.100/16  

&lt;/Directory&gt; 
<br> 






