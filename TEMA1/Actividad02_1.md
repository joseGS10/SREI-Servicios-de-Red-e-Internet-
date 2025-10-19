# ACTIVIDAD 2_1.TEMA1. CONFIGURACIÓN BÁSICA DE APACHE. <br>
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

<img width="940" height="526" alt="image" src="https://github.com/user-attachments/assets/b4ac6123-eb2f-4f22-8f12-5082a4355b2c" /> <br><br>

**2. Añadir el dominio “marisma.intranet” en el fichero “hosts”**  <br>

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

<img width="1059" height="298" alt="image" src="https://github.com/user-attachments/assets/55f82d7f-5095-4259-add7-fb2650320b11" />   <br><br>

**3. Cambia la directiva “ServerTokens” para mostrar el nombre del producto.** <br>

**ServerTokens** configura la cabecera de respuesta de HTTP Apache., devuelve el nombre del servidor y esto se puede comprobar en las herramientas de desarrollador del navegador. 

Abrimos el archivo **apache2.conf** para editarlo: 

**sudo nano /etc/apache2/apache2.conf** y le colocamos al final del directorio la directiva **ServerTokens Prod** 


<img width="841" height="567" alt="image" src="https://github.com/user-attachments/assets/511cc7e6-d945-47a3-80d8-202eb71eac23" /> 

**¿Cómo comprobamos que funciona?** 

Podemos hacerlo de 2 maneras. 

    1- En las herramientas del desarrollador del navegador /Red / recargamos la página / seleccionamos la petición / y en Cabeceras aparece **Server: Apache**, nos aparece solo el nombre del servidor, ni versión, ni sistema operativo….  
    

<img width="940" height="499" alt="image" src="https://github.com/user-attachments/assets/35cd469c-7958-4b33-bfc0-13f6e464802e" /> 
     2- **curl -I localhost** -> nos da información de la cabecera. 

<img width="859" height="402" alt="image" src="https://github.com/user-attachments/assets/5db20719-3a5b-4d84-b275-57b87c87028f" /> <br><br>
**4. Comprueba si se visualiza el pie de página en las páginas generadas por Apache (por ejemplo, en las páginas de error). Cambia el valor de la directiva y comprueba que funciona correctamente.** <br>
Primero intentamos acceder a una página que no contenga nuestro servidor:, por ej: **localhost/nadena** 

y me responde lo siguiente: 

<img width="806" height="171" alt="image" src="https://github.com/user-attachments/assets/d243296b-38b2-4b9c-b986-82971d4fe5fb" /> 

Esta página de error tiene su **pie de página** que aparece en las páginas que dan mensajes de error. El hecho de que aparezca el pie de página quiere decir que la directiva **ServerSignature** esta activa **on**. 

Entonces vamos a hacer que desaparezca el pie de página mediante la colocación de la **directiva ServerSignature off**. Para ello, accedemos al fichero de configuración **apache2.conf** 

**sudo nano /etc/apache2/apache2.conf** 

<img width="843" height="442" alt="image" src="https://github.com/user-attachments/assets/aacfc6f3-1f29-4f62-9388-4ebcc246c033" /> 


Comprobamos que ya no aparezca el pie de página asociado al error. 

<img width="843" height="208" alt="image" src="https://github.com/user-attachments/assets/6c0625ef-fd94-4991-96a1-df8f39974cd4" /> <br><br>

**5. Crea un directorio “prueba” y otro directorio “prueba2”. Incluye un par de páginas en cada una de ellas(en el virtual host por defecto)** 

**Nosotros tenemos en este momento 2 dominios, el que viene por defecto en Apache y que se encuentra deshabilitado de la actividad1 y por otro lado tenemos my_domain que es un virtual host que creamos y habilitamos en la actividad1. Nosotros podemos tener tantos virtual host creados y habilitados como queramos porque podemos dar servicio de hospedaje a más de un sitio web.** 

Accedemos al Document Root por defecto,  **/var/www/html** 

**Hay que decir que esto funcionara si no tenemos creado un virtual host , pero como si lo tenemos creado y habilitado de la actividad1 habría que deshabilitar el virtual host creado y habilitar el virtual host por defecto** 

deshabilito el virtual host que cree **“my_domain”** con **sudo a2dissite my_domain** 

<img width="481" height="134" alt="image" src="https://github.com/user-attachments/assets/70c413df-00f5-4634-af1b-17a369b4bf0f" /> 

habilito el virtual host por defecto con **sudo a2ensite 000-default** 


<img width="442" height="98" alt="image" src="https://github.com/user-attachments/assets/a8e90c60-1b86-46a4-8814-d4d7b8b0e425" /> 

Una vez realizados los cambios pertinentes( deshabilitado virtual host que creamos y habilitado virtual host por defecto), pasamos a crear los directorios. 

Para ello, nos movemos al directorio del **Document Root.** 

**cd /var/www/html** 

<img width="945" height="129" alt="image" src="https://github.com/user-attachments/assets/3e278969-2c17-4f95-b28e-1b8a3172c2f1" /> 


creo 2 directorios 

**sudo mkdir prueba prueba2** 

Ahora, creamos un **index.html** en cada uno de los 2 directorios recién creados. 

**sudo nano prueba/index.html**

<img width="1334" height="287" alt="image" src="https://github.com/user-attachments/assets/104e5710-2301-4c08-b1d8-57584d0a71b4" /> 

Lo guardamos y salimos. Ctrl + O, Intro, Ctrl +x 

Ahora, hacemos una copia de ese **index.html* en el directorio **prueba2** 

**sudo cp prueba/index.html prueba2** 

A continuación, editamos el **index.html** del **directorio prueba2** y le modificamos el párrafo indicando que este fichero es de **prueba2** 

**sudo nano prueba2/index.html** 

<img width="1309" height="269" alt="image" src="https://github.com/user-attachments/assets/f8c592f0-36be-4661-a41a-f468830a8a62" /> 

Ahora, vamos a comprobar que todo esto funciona. 

<img width="655" height="158" alt="image" src="https://github.com/user-attachments/assets/3ac37e30-866a-4d59-b971-f2ccfa7df5e4" /> 


<img width="927" height="235" alt="image" src="https://github.com/user-attachments/assets/de29b0e4-1f6f-4cea-a97b-7f6ddd16c646" /> 

Si lo comprobamos con **curl localhost/prueba** me sale lo siguiente. 

<img width="1330" height="314" alt="image" src="https://github.com/user-attachments/assets/8b5c92ed-faca-4f47-ac19-47e6a5c16c47" /> 

(He de comentar que estas comprobaciones me daban negativo y ello era debido a que tenía creado un virtual host y tenía deshabilitado el virtual host por defecto donde había creado estos directorios.). Tener en cuenta para futuros ejercicios. 

Este apartado decía que creáramos un par de páginas en cada directorio y solo hemos creado **index.html** en cada directorio. Vamos a crear **pagina.html** en cada directorio diferenciando su contenido para saber a qué directorio pertenece cada uno. <br><br>

**6. Redirecciona el contenido de la carpeta “prueba” hacia “prueba2”** <br>
Nos vamos a la directiva **Redirect** de la documentación de ayuda de Apache Docs. 

Esta directiva funciona así:  **Redirect  “/prueba”  “/prueba2”** , ya que se encuentran en el mismo host. 

Para hacer efectivo esta redirección, tenemos que acceder al archivo de configuración **/etc/apache2/apache2.conf** e incluir la directiva **Redirect  “/prueba”  “/prueba2”** 

**sudo nano /etc/apache2/apache2.conf** 

<img width="918" height="544" alt="image" src="https://github.com/user-attachments/assets/3c193e15-2329-4b3c-bba5-ecc12b683668" /> 


Y ahora probamos que funciona la redirección. 

**curl localhost/prueba** 

<img width="944" height="218" alt="image" src="https://github.com/user-attachments/assets/b062d302-48db-440b-a85e-6132545919e6" /> 


Si escribimos en el buscador **localhost/prueba** automáticamente nos redirecciona a **prueba2** 

<img width="884" height="215" alt="image" src="https://github.com/user-attachments/assets/0922e6de-7031-4361-ad40-8437bd86eb97" /> <br><br>


 **7. Es posible redireccionar tan solo una página en lugar de toda la carpeta. Pruébalo.** <br>
Para hacer efectivo esta redirección, tenemos que acceder al archivo de configuración **/etc/apache2/apache2.conf** e incluir la directiva **Redirect  “/prueba/pagina.html” “/prueba2”**. De esta forma estamos redireccionando solo la página pagina.html de prueba a prueba2 

**sudo nano /etc/apache2/apache2.conf** 

<img width="813" height="503" alt="image" src="https://github.com/user-attachments/assets/6d37eb78-ef66-49e4-a774-a65754a22b5c" /> 

**¿Como lo comprobamos?** 

**curl localhost/prueba/pagina.html** 
<img width="681" height="185" alt="image" src="https://github.com/user-attachments/assets/175fac4b-ab03-4dbe-b085-899ab2c90269" /> 
o también poniendo en el navegador **localhost/prueba/pagina.html** automáticamente me redirige a prueba2. 

<img width="620" height="140" alt="image" src="https://github.com/user-attachments/assets/c38ef8ac-4edc-4c14-9838-1622e1c5163f" /> <br><br>


**8. Usa la directiva userdir** <br>
Por defecto, este módulo se encuentra deshabilitado en el directorio **/etc/apache2/mods-available**. Para poder hacer uso de él, debemos activarlo. 

Para habilitarlo ejecutamos  **sudo a2enmod userdir** 

<img width="427" height="110" alt="image" src="https://github.com/user-attachments/assets/bc4b28ea-b768-427e-9a34-05c8f8098ba9" /> 

Una vez habilitado hay que reiniciar apache para que los cambio surtan efecto: 

**sudo systemctl restart apache** 

para visualizar que dicho modulo queda habilitado **cd /etc/apache2/mods-enabled** 

<img width="911" height="145" alt="image" src="https://github.com/user-attachments/assets/a68adb55-f6b9-4b02-ae67-c4caae4748a9" /> 

El **userdir** sirve para que el administrador del sitio web de permiso a los usuarios del sistema Linux, del servidor, para que ellos mismos puedan poner su propio contenido en su directorio de usuario; recordar que Apache sirve contenido desde el DocumentRoot y esto es una excepción para que el administrador del servidor no tenga que estar creando virtual host a estos usuarios cada vez que se lo pidan y de esta forma los usuarios puedan usar su directorio de trabajo para almacenar contenido. 

**¿Cómo lo hacemos?** 

**Userdir “public_html” , esto indica que el directorio que deben  usar los usuarios para almacenar lo que ellos quieran es el directorio “public_html” que se deberá crear cada usuario en su directorio personal.** 

1º tenemos que asegurarnos que el módulo **userdir** está habilitado. (**Ok**) 

2º Configurar la directiva **userdir** en el fichero **/etc/apache2/mods-available/userdir.conf** 

<img width="1072" height="230" alt="image" src="https://github.com/user-attachments/assets/3012cca1-1080-4686-ad53-c1feb648af23" /> 

3º Pero para que termine de funcionar , a Apache le tengo que informar que se permite la lectura  desde ese directorio ya que estamos leyendo de un directorio que no es el Document Root. Y para esto existe la **directiva Directory**. 

**sudo nano /etc/apache2/mods-available/userdir.conf** 

<img width="1172" height="601" alt="image" src="https://github.com/user-attachments/assets/563edaf3-bc4a-49dc-9dcd-0a942465a745" /> 

Todo lo que el usuario incluya en su directorio **“public_html“* podrá ser visto desde Apache. 

    • Creamos el  directorio public_html  dentro de nuestro directorio de trabajo **mkdir  ~/public_html** 
    
    • Creamos un fichero index.html dentro del directorio **public_html** con el siguiente contenido: 
    
    sudo nano  ~/public_html/index.html
      

<img width="1026" height="157" alt="image" src="https://github.com/user-attachments/assets/a61ce45d-c2f9-4396-9233-18e36501bdd2" /> 
    • A continuación, hay que dar permisos 755 sobre nuestro directorio de trabajo y sobre **public_html**  para que Apache pueda entrar hasta el directorio **html_public** donde el usuario colgará el contenido que él quiera. Para ello: 
    
                •  chmod 755 ~ 
                
                •  chmod 755 ~/public_html
                

Ahora para acudir a mi sitio web, ponemos **localhost /~nombre usuario** y con esto nos busca la página index.html que tenga dentro de **“public_html”** de ese usuario. 

<img width="741" height="178" alt="image" src="https://github.com/user-attachments/assets/14225db0-88a9-43d6-9c3d-476cf0628897" /> <br><br>
**9. Usa la directiva alias para redireccionar a una carpeta dentro del directorio de usuario.** 
Un alias es un nombre alternativo que se utilizará para llamar a otra cosa. 

Para probar un alias, vamos a crear en el fichero **apache2.conf** una entrada que cuando pongamos /imágenes acceda directamente al directorio imágenes de nuestra cuenta de usuario /home/jose 

**udo nano /etc/apache2/apache.conf** 


<img width="993" height="531" alt="image" src="https://github.com/user-attachments/assets/6fe032c4-f96a-4ed4-821e-6f06814c05d7" /> <br><br>

**10. ¿Para qué sirve la directiva Options y dónde aparece? Comprueba si apache indexa los directorios. Si es así. ¿Cómo lo desactivamos?** <br>

Esta directiva sirve para indicar comportamientos(opciones de configuración) dentro de un determinado directorio. Entre las opciones que se utilizan:      
-	**FollowSymLinks**: indica que permite enlaces simbólicos.
  
-	**Indexes**: cuando se activa indexes en un directorio, mostrará un listado de todos los archivos y subdirectorios dentro de ese directorio si no encuentra un archivo de índice (como index.html o index.php). Hay que tener cuidado con el uso de esta opción ya que si no encuentra lo que busca lista el contenido del directorio.Esta opción no se suele activar.
  
Se usan de la siguiente forma y se suelen colocar en **etc/apache2/apache2.conf**: 

<Directory /web/docs> 

Options Indexes FollowSymLinks 
    
</Directory> 




