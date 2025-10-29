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

</Directory>  

