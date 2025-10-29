**ACTIVIDAD 2.2_TEMA1. Trabajado con Scripts.** 

Crea un script para cada uno de los siguientes problemas 

**1. Crea un script que añada un puerto de escucha en el fichero de configuración de 
Apache. El puerto se recibirá como parámetro en la llamada y se comprobará que no 
esté ya presente en el fichero de configuración.** 


Para ello, vamos a editar un script de nombre **addport.sh** que guardaremos en nuestro directorio de trabajo. Pero, antes de nada, para evitar sorpresas, cuando estemos desarrollando scripts que tengan que modificar ficheros de configuración, lo primero que deberíamos de hacer es una copia de seguridad de ellos. 

Para este script tenemos que trabajar sobre el fichero **ports.conf** por lo que creamos una copia de este. 

**sudo cp /etc/apache2/ports.conf ports.bak** 

<img width="942" height="341" alt="image" src="https://github.com/user-attachments/assets/392fd27d-6d19-4831-be3f-391b77694e26" /> 

A continuación, editamos el script 

**sudo nano addport.sh** 
<img width="940" height="349" alt="image" src="https://github.com/user-attachments/assets/2366135f-30a9-4b50-b8ca-fd7ec7970e63" /> 

La opciones de grep usadas: 

**-i**   se usa para que en la búsqueda no distinga entre mayúsculas y minúsculas, y saque todas las coincidencias por igual. 

**-q** provoca una salida silenciosa de los resultados del parámetro, es decir, no muestra información alguna del resultado del parámetro; sin embargo, sí usa el valor que devuelve para su posterior uso como puede ser en un condicional. (**0 indica éxito** en la operación  y **1 que no se encontró nada**). 

 Guardamos y salimos. 
 
Le damos permisos de ejecución al script **sudo chmod + x addport.sh** 

Y ahora ponemos a prueba el script con los siguientes ordenes : 

(Para poder hacer modificaciones sobre el archivo de configuración **ports.conf** hay que hacerlo como **root**, por lo que el script habrá que ejecutarlo como sudo) 

1)	**sudo ./addport.sh 88**
   
<img width="940" height="75" alt="image" src="https://github.com/user-attachments/assets/e88e12c1-6bba-4ed9-bf97-83cb4fc65c4f" />  

Abrimos el fichero **/etc/apache2/ports.conf** para comprobar que se ha insertado el puerto 88 
<img width="940" height="392" alt="image" src="https://github.com/user-attachments/assets/08716aaf-d18f-4ca6-935e-14fb23823097" /> 

2)	**sudo ./addport.sh 80**
<img width="940" height="75" alt="image" src="https://github.com/user-attachments/assets/3224e1e9-2e31-49b3-8b24-bb060f90b9b2" />
Nos indica que el puerto ya existe, está configurado, luego no lo inserta.

3)	**sudo ./addport.sh	(sin indicarle ningún parámetro)**
<img width="940" height="67" alt="image" src="https://github.com/user-attachments/assets/cbaf4b33-6fc8-4d27-ab96-56897d478c74" />

Al no proporcionar en la llamada al script ningún parámetro nos está mandando un mensaje con dicho problema. 


**2. Crea un script que añada un nombre de dominio y una ip al fichero hosts. Debemos 
comprobar que no existe dicho dominio en el fichero hosts.** 

Al igual que antes, antes de nada, creamos una copia de seguridad del fichero sobre el que vamos a trabajar. 

**sudo cp /etc/hosts /etc/hosts_bak** 
<img width="940" height="214" alt="image" src="https://github.com/user-attachments/assets/c16ec665-5e0b-4eca-b28a-6f66205bdae2" />  

Editamos un script con el nombre adddomainip.sh el cual debe recoger 2 parámetros: el **nombre de dominio** y la **ip** 

**sudo nano adddomainip.sh** 
<img width="940" height="458" alt="image" src="https://github.com/user-attachments/assets/cee2fd16-5264-44f7-90f4-5f8906124793" /> 

Le damos permisos de ejecución: **sudo chmod + x adddomainip.sh** 


Hacemos las comprobaciones pertinentes para ver que el script funciona: 
<img width="940" height="97" alt="image" src="https://github.com/user-attachments/assets/f79fdc67-b144-41df-b7b8-1498d42b4d3c" /> 

<img width="940" height="85" alt="image" src="https://github.com/user-attachments/assets/1051a84a-edb7-4b9b-b24d-0cbf1cc3bbee" /> 

<img width="940" height="54" alt="image" src="https://github.com/user-attachments/assets/0e609734-503e-4e86-8935-32ef7878e52e" /> 

<img width="940" height="61" alt="image" src="https://github.com/user-attachments/assets/9c3568a7-e1ee-4793-b57c-3e59ebb7fb3c" /> 

El contenido del **fichero hosts** después de estas operaciones.  

<img width="940" height="352" alt="image" src="https://github.com/user-attachments/assets/20b50de6-44aa-4888-b986-82a4adf38fa4" /> 


**3. Crea un script que nos permita crear una página web con un título, una cabecera y 
un mensaje.** 

**sudo nano crear_pagina.sh** 

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/24573068-0b20-40d9-8faa-b8f28b9504f8" /> 

Le damos permiso de ejecución. 

**sudo chmod + x  crear_pagina.sh** 

Y ahora, comprobamos su funcionamiento. 
<img width="940" height="94" alt="image" src="https://github.com/user-attachments/assets/07ffe2f4-ed32-4dbe-b728-6d02fcabd42e" /> 

<img width="940" height="84" alt="image" src="https://github.com/user-attachments/assets/3d8a1f14-cab3-40d8-aeb8-2b61bf89c399" /> 

<img width="940" height="74" alt="image" src="https://github.com/user-attachments/assets/65641752-e740-49f1-bf3d-ec62e5ebde7a" /> 
Finalmente, comprobamos que se ha creado la página web. 

<img width="940" height="418" alt="image" src="https://github.com/user-attachments/assets/2ac9a2a1-bfba-4400-8ebd-84351978b696" /> 

**sudo cp web.html public_html/** 


<img width="940" height="321" alt="image" src="https://github.com/user-attachments/assets/ff00e403-5c00-4aad-a600-820b168f0383" /> 
















