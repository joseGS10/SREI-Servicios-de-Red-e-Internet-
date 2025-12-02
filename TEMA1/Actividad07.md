
## ACTIVIDAD_7_TEMA1. Autenticación. 

**Ejercicio 1.**  Crea cinco usuarios: usuario1, usuario2, usuario3, usuario4, usuario5. 

Los usuarios creados se irán guardando en /etc/apache2/.htpasswd 
<img width="940" height="123" alt="image" src="https://github.com/user-attachments/assets/358d6b38-42fd-4f7a-b73f-ac8f07dab187" /> 
(la opción -c se utiliza solo para crear el primer usuario ya que sirve para crear a la vez el fichero que contendrá la información de los usuarios registrados). 

Se nos solicitará la clave de cada usuario 2 veces. 

A continuación, creamos los demás usuarios. 
<img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/53434b96-3b47-4ab5-b16a-635641ccee62" />  

**Ejercicio 2.** Crea dos grupos de usuarios, el primero formado por usuario1 y usuario2; y el segundo por el resto de usuarios. 

Apache usa un archivo de texto simple para los grupos. 

**sudo nano /etc/apache2/groups** 
<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/2f023361-1fe8-4732-8c52-3138f4514ba1" />  

**Ejercicio 3.** Crea un directorio llamado privado1 que permita el acceso a todos los usuarios. 

Creamos el directorio privado1 

**sudo mkdir /var/www/html/privado1** 

En el fichero de configuración apache2.conf metemos la siguiente directiva. 

**sudo nano /etc/apache2/apache2.conf** 
<img width="940" height="290" alt="image" src="https://github.com/user-attachments/assets/a822f471-8039-417d-97d3-4c34a1fef68a" /> 
<img width="940" height="92" alt="image" src="https://github.com/user-attachments/assets/34088e9f-94ae-4622-801c-973d4f96c212" /> 
Comprobamos que se puede acceder con todos los usuarios. 

Creamos contenido para privado1 
<img width="940" height="86" alt="image" src="https://github.com/user-attachments/assets/1f07d9c7-000e-4d18-bf59-964e3ea0e1d6" /> 
<img width="730" height="198" alt="image" src="https://github.com/user-attachments/assets/f7f3d5e1-b811-4404-96ef-d55cd1cd19d1" /> 

Conseguimos entrar con los cinco usuarios previa identificación.  

**Ejercicio 4.** Crea un directorio llamado privado2 que permita el acceso sólo a los usuarios del grupo1. 

**sudo mkdir /var/www/html/privado2**

Ahora, tenemos que hacer que a la carpeta privado2 solo accedan usuarios del grupo1 

**sudo nano /etc/apache2/apache2.conf** 
<img width="940" height="298" alt="image" src="https://github.com/user-attachments/assets/4ef9bd20-b6fa-4cbf-a042-03ed9cc80915" /> 
<img width="940" height="53" alt="image" src="https://github.com/user-attachments/assets/9fc4c699-44b9-420d-944f-2a87a65946a9" /> 
Me da un problema referente al modulo de grupos. Voy a habilitarlo. 
<img width="940" height="215" alt="image" src="https://github.com/user-attachments/assets/f2e66650-6df7-4c0e-bed2-bfac1cd0e0d1" /> 
Solucionado. 

Comprobemos que podemos acceder con los usuarios autorizados y no con el resto. 

Creamos contenido para privado2 
<img width="940" height="98" alt="image" src="https://github.com/user-attachments/assets/5bbca16d-7672-4aab-8963-09ab407b0382" /> 
<img width="940" height="460" alt="image" src="https://github.com/user-attachments/assets/755a2061-a296-4e6d-a8fc-7347bbda1c8e" /> 
<img width="940" height="146" alt="image" src="https://github.com/user-attachments/assets/fdb6f45a-d78e-46d7-9d10-9f889273ecd5" /> 
Con usuario1 y usuario2 entra sin problema previo logueo. 
<img width="940" height="462" alt="image" src="https://github.com/user-attachments/assets/5dd64c08-822a-4dc6-8a03-732edc924c5c" /> 
Con los usuarios 3,4 y 5 no me permite la entrada y se queda preguntando la password continuamente. Si le doy a Cancelar me da el siguiente mensaje. 
<img width="940" height="165" alt="image" src="https://github.com/user-attachments/assets/80a5ec28-b8ed-44f9-9bb6-b22fafe6fbe8" />  
**Ejercicio 5.** En el directorio privado2 haz que sólo sea accesible desde el localhost, y estudia cómo se comporta la autorización si ponemos: satisfy any, satisfy all 

**Caso 1: Satisfy All.** Aquí para poder entrar debes cumplir la condición de la IP y además la contraseña. 
<img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/e2b8729f-a29f-4751-a2f2-52e177cbcf5f" /> 
**Caso 2: Satisfy Any.** Aquí, si cumples la IP pasas, si no, te requiero la contraseña 
<img width="940" height="388" alt="image" src="https://github.com/user-attachments/assets/d21d7f5e-caba-43a4-a805-d4f19402cccb" /> 
 



























