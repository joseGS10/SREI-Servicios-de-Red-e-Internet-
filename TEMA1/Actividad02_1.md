# ACTIVIDAD 2.TEMA1. CONFIGURACIÓN BÁSICA DE APACHE.
Poner en marcha el servidor y realizar los siguientes cambios en los archivos de configuración:  
**sudo systemctl start apache2** -> lo pone en marcha  
**sudo systemctl status apache2**  o **sudo service apache2 status** -> lo comprueba  
**sudo service stop apache2** -> lo para  
**sudo service restart apache2** -> lo reinicia  
Hay 2 archivos de configuración  :  
**apache2.conf** : que es el archivo de configuración general    
**Ports.conf** : que configura los puertos de escucha  
Ambos se encuentran en **/var/apache2**  
Todos los cambios que se van a realizar en estos ficheros son a través de **directivas**. Una directiva no es más que un nombre con una serie de argumentos (como una función).  
Para conocer las diferentes directivas ponemos en el buscador **“Apache docs”** seleccionamos la última versión, y pinchamos en **“Directivas de configuración en tiempo de ejecución”**  y aparece todas las directivas.  

**1. Apache utilizará el puerto 81 además del 80.** 

La directiva que tiene que ver con los puertos de escucha, se llama “listen [IP address]”. 

Para ello nos vamos al fichero de configuración ports.conf ubicado en /etc/apache2, lo editamos con **sudo nano /etc/apache2/ports.conf** 

……dentro escribimos una nueva línea Listen 81 


<img width="1034" height="500" alt="image" src="https://github.com/user-attachments/assets/3e7c6c39-4d0e-4a9d-87ba-1a865c596406" /> 

Y ahora, como cada vez que se realiza cualquier modificación en un archivo de configuración, hay que comprobar que la sintaxis escrita está bien y para ello ejecutamos el comando apachectl -t 


<img width="941" height="84" alt="image" src="https://github.com/user-attachments/assets/d2e60136-2c2d-4888-8f03-f852b7e05042" /> 

