#ACTIVIDAD 3. TEMA1 

Ejercicios 

**1. Crea un directorio llamado "dir1" y otro llamado "dir2"**
   
Creamos los dos directorios en /var/www/ que es el directorio por defecto donde Apache  

guarda los sitios web  

**sudo mkdir /var/www/dir1 /var/www/dir2**

**2. Explica qué diferencia existe entre ambos y muestra su equivalencia con la
directiva Require:** 

**Ambas están ya en desuso.** 

<Directory /var/www/example1>  

Order Deny,Allow  

Deny from All  

Allow from 192.168.1.100  
&lt;/Directory&gt; 


En este primer caso, el orden es : primero se deniega a todos y luego se permite acceso al  

recurso a la IP especificada. Concluimos diciendo que el único que tiene acceso al recurso es  

la ip especificada ya que la última orden que se ejecuta es Allow.  

La **directiva** equivalente con **Require**: 

<Directory /var/www/example1>  

Require ip 192.168.1.100  

</Directory> 

**<Directory /var/www/example1>** 

Order Allow,Deny 

Deny from All 

Allow from 192.168.1.100 

</Directory> 

En este segundo caso: primero se permite acceso al recurso a la IP especificada y luego se  

deniega a todos. Por lo que concluimos diciendo que este recurso está denegado para todos ya  

que la orden **Denny** es la última que se ejecuta..  

La directiva equivalente con Require: 

<Directory>  
   
Require all denied  

</Directory>  
</Directory> 





