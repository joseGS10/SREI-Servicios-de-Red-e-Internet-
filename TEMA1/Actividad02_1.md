# ACTIVIDAD 2.TEMA1. CONFIGURACIÓN BÁSICA DE APACHE. <br>
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

Para conocer las diferentes directivas ponemos en el buscador **“Apache docs”** seleccionamos la última versión, y pinchamos en **“Directivas de configuración en tiempo de ejecución”**  y aparece todas las directivas.  <br><br>

**1. Apache utilizará el puerto 81 además del 80.** <br>

La directiva que tiene que ver con los puertos de escucha, se llama “listen [IP address]”. 

Para ello nos vamos al fichero de configuración ports.conf ubicado en /etc/apache2, lo editamos con **sudo nano /etc/apache2/ports.conf** 

……dentro escribimos una nueva línea Listen 81 


<img width="1034" height="500" alt="image" src="https://github.com/user-attachments/assets/3e7c6c39-4d0e-4a9d-87ba-1a865c596406" /> 

Y ahora, como cada vez que se realiza cualquier modificación en un archivo de configuración, hay que comprobar que la sintaxis escrita está bien y para ello ejecutamos el comando **apachectl -t** 

<img width="941" height="84" alt="image" src="https://github.com/user-attachments/assets/d2e60136-2c2d-4888-8f03-f852b7e05042" /> 
Ahora, tenemos que recargar Apache con : **sudo systemctl** restart apache2 ya que hemos realizado una modificación en uno de sus archivos de configuración. 

<img width="581" height="61" alt="image" src="https://github.com/user-attachments/assets/f053a2f9-c12d-4983-8745-60895f0a016c"  /> 

Y, para terminar, comprobamos que funciona abriendo el navegador y escribiendo la dirección **localhost:81** 

<img width="940" height="526" alt="image" src="https://github.com/user-attachments/assets/b4ac6123-eb2f-4f22-8f12-5082a4355b2c" /> 

**2. Añadir el dominio “marisma.intranet” en el fichero “hosts”** 
El fichero hosts se encuentra en /etc.  

Lo editamos con nano de la siguiente forma:  **sudo nano /etc/hosts** 


<img width="798" height="353" alt="image" src="https://github.com/user-attachments/assets/10a3102a-6d8f-4e96-bc37-3cee96fe760d" /> 
le añadimos el dominio **“marisma.intranet”**  junto con la dirección localhost que es 127.0.0.1 y ahora guardamos y salimos de la edición.  

En este caso, tras modificar el fichero **“hosts”** no se requiere que reiniciemos Apache. Todo lo contrario que con las directivas que siempre que modifiquemos alguna hay que reiniciar Apache. 

He de decir que el nombre del dominio **“marisma.intrranet”** también se podía haber colocado en la primera línea del fichero a continuación de localhost separado por un espacio. 

**¿Cómo comprobamos que funciona?**
Si hacemos **ping a localhost** me va a responder con **127.0.0.1** 

<img width="871" height="183" alt="image" src="https://github.com/user-attachments/assets/bc02ccd5-faad-49b9-afa4-5ced53c16219" /> 

Si hacemos **ping a marisma.intranet** me responde también con *127.0.0.1* 

<img width="980" height="198" alt="image" src="https://github.com/user-attachments/assets/e6753692-7af9-4430-a986-0bafc24dbfa3" /> 


De igual manera, si en el navegador ponemos **http://marisma.intranet** nos responde. 

<img width="1059" height="298" alt="image" src="https://github.com/user-attachments/assets/55f82d7f-5095-4259-add7-fb2650320b11" />   

**3. Cambia la directiva “ServerTokens” para mostrar el nombre del producto.** 


