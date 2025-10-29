#ACTIVIDAD 3. TEMA1 

Ejercicios 

&nbsp;&nbsp;&nbsp;**1. Crea un directorio llamado "dir1" y otro llamado "dir2">**
   
Creamos los dos directorios en **/var/www/** que es el directorio por defecto donde Apache  

guarda los sitios web  

**sudo mkdir /var/www/dir1 /var/www/dir2**

<br> 

&nbsp;&nbsp;&nbsp;**2. Explica qué diferencia existe entre ambos y muestra su equivalencia con la
directiva Require:** 

**Ambas están ya en desuso.** 

**<Directory /var/www/example1>**

&nbsp;&nbsp;&nbsp;&nbsp;**Order Deny,Allow**

&nbsp;&nbsp;&nbsp;&nbsp;**Deny from All**  

&nbsp;&nbsp;&nbsp;&nbsp;**Allow from 192.168.1.100** 

**&lt;/Directory&gt**



En este primer caso, el orden es : primero se deniega a todos y luego se permite acceso al  

recurso a la IP especificada. Concluimos diciendo que el único que tiene acceso al recurso es  

la ip especificada ya que la última orden que se ejecuta es **Allow**.  

La **directiva** equivalente con **Require**: 

<Directory /var/www/example1>  

&nbsp;&nbsp;&nbsp;&nbsp;Require ip 192.168.1.100  

&lt;/Directory&gt; 


**<Directory /var/www/example1>** 

&nbsp;&nbsp;&nbsp;&nbsp;**Order Allow,Deny** 

&nbsp;&nbsp;&nbsp;&nbsp;**Deny from All**

&nbsp;&nbsp;&nbsp;&nbsp;**Allow from 192.168.1.100** 

**&lt;/Directory&gt;** 

En este segundo caso: primero se permite acceso al recurso a la IP especificada y luego se  

deniega a todos. Por lo que concluimos diciendo que este recurso está denegado para todos ya  

que la orden **Denny** es la última que se ejecuta..  

La directiva equivalente con Require: 

&lt;Directory&gt;  
   
&nbsp;&nbsp;&nbsp;&nbsp;Require all denied  

&lt;/Directory&gt;  

<br>

&nbsp;&nbsp;&nbsp;**3. Para dir1** 

**a. Perrmite el acceso de las peticiones provenientes de 10.3.0.100** 

**b. Permite el acceso desde "marisma.intranet"** 

**c. Permite el acceso desde cualquier subdominio de "marisma.intranet"** 

**d. Permite el acceso de las peticiones provenientes de "10.3.0.100" con máscara "255.255.0.0"** 

<Directory /var/www/dir1> 

&nbsp;&nbsp;&nbsp;&nbsp;Require ip 10.3.0.100  

&nbsp;&nbsp;&nbsp;&nbsp;Require host marisma.intranet  

&nbsp;&nbsp;&nbsp;&nbsp;Require host .marisma.intranet  

&nbsp;&nbsp;&nbsp;&nbsp;Requiere ip 10.3.0.100/16  

&lt;/Directory&gt; 
<br>  


&nbsp;&nbsp;&nbsp;**4. Modifica la configuración de forma que el acceso a dir1:** 

**Se permita a "marisma.intranet" y no se permita desde 10.3.0.101"** 

&nbsp;&nbsp;&nbsp;<Directory /var/www/dir1> 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;RequireAll&gt; 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Require host marisma.intranet 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Require not ip 10.3.0.101 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/RequireAll&gt;


&nbsp;&nbsp;&nbsp;&lt;/Directory&gt;  
<br>
&nbsp;&nbsp;&nbsp;**5. Modifica la configuración de forma que el acceso a dir2:** 

**Se permita a "10.3.0.100/8" y no a "marisma.intranet"** 

&nbsp;&nbsp;&nbsp;<Directory /var/www/dir2>  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;RequireAll&gt;  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Require ip 10.3.0.100/8  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Require not host marisma.intranet  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/requireAll&gt;  

&nbsp;&nbsp;&nbsp;&lt;/Directory&gt; 
