## Actividad4_TEMA1 Expresiones Regulares  

**Escribe las expresiones regulares para los siguientes supuestos:** 

Directorios en /www/ cuyo nombre consista en tres dígitos.  

\/www\/\d{3}  

Ficheros: *.gif, *.jpeg, *.jpg, *.png  

.+\.(gif | jpe?g | png )  

Escribe una directiva para redireccionar todos los GIF a ficheros JPEG en otro servidor 

RedirectMatch "(.*)\.gif$" "$1.jpg"  

Cadenas que correspondan con números enteros y decimales   

[+-]?\d*(\.?\d+)?  

Números de teléfono en el formato Americano: 123-123-1234  

\d{3}-?\d{3}-?\d{4}  

Palabras  

\b[a-zA-Z]+\b  

Códigos hexadecimales de color de 24 o 32 bits  

(#|0x)?(?:[0-9A-F]{2}){3,4}  

Palabras de 4 letras    

\b[a-zA-Z]{4}\b  

Número entero sin signo   

\b\d+\b  (así me detecta números independientes que no forman parte de una palabra)  

Número entero con signo   

[+-]?\d+ -------------       

Números reales   

[+-]?[0-9]+ 
[-+]?([0-9]*\.[0-9]+)|[0-9]+ ------  [+-]?\d*(\.\d+)?  

Número reales con exponente  

[-+]?([0-9]*\.[0-9]+)|[0-9]+([eE][+-]?[0-9]+)?  

Email ------- [+-]?\d*(\.\d+)?([eE][+-]?\d  

\b[a-zA-Z0-9\._%\+\-]+@[a-zA-Z0-9\.\-]+\.[a-zA-Z]{2,}\b  

Números del 0 a 255  

^([01][0-9][0-9])|2[0-4][0-9]|25[0-5])$ ---     
\b[w\.-]+@[\w\.-]+\.\w{2,4}\b
