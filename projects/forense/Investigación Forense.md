## Índice

- [1. Introducción](#introduccion) 
- [2. Bloque 1 - Análisis forense de un sistema informático](#bloque-1-analisis-forense-de-un-sistema-informatico) 
- [3. Bloque 2 – Informe Pericial](#bloque-2-informe-pericial) 
  - [3.1 Introducción](#bloque-2-introduccion) 
  - [3.2 Objeto y alcance](#bloque-2-objeto-y-alcance) 
  - [3.3 Metodología](#bloque-2-metodologia) 
  - [3.4 Resultados y evidencias](#bloque-2-resultados-y-evidencias) 
  - [3.5 Conclusiones periciales](#bloque-2-conclusiones-periciales) 
  - [3.6 Anexo](#bloque-2-anexo) 
- [4. Conclusión](#conclusion) 

---

<!-- Secciones: pega aquí el contenido correspondiente a cada apartado -->
<h2 id="introduccion">1. Introducción</h2>

La presente actividad se estructura en dos bloques cuyo objetivo es abordar de forma integral un supuesto caso de fraude con tarjetas bancarias en el que ha sido intervenido un equipo informático. En el primer bloque se llevará a cabo el análisis forense de la imagen digital, mientras que en el segundo se redactará un informe pericial ajustado a la norma UNE 197010 y a las mejores prácticas de la pericia forense. 

<h2 id="bloque-1-analisis-forense-de-un-sistema-informatico">2. Bloque 1 - Análisis forense de un sistema informático</h2>

El primer paso consiste en comprobar la integridad de la imagen recibida y así verificar que se ha respetado la cadena de custodia. Dado que la imagen está dividida en fragmentos de 655080 KB cada uno, crearemos con FTK Imager un único archivo que reúna todos esos ficheros. De este modo, podremos calcular los valores de resumen (hashes) correspondientes y compararlos con los proporcionados. 
<p align="center">
    <img src="img/1.png" alt="comprobar integridad" width="400px">
</p>
Seleccionamos el formato de imagen resultante.
<p align="center">
    <img src="img/2.png" alt="Selección de formato" width="400px">
</p>
Nombre de la imagen y ruta de destino.
<p align="center">
    <img src="img/3.png" alt="Nombre y ruta de la imagen resultante" width="400px">
</p>
Calculamos los valores hash MD5 y SHA-1 para garantizar la integridad de la imagen recibida. Aunque 
el nombre del archivo resultante en FTK Imager no coincida exactamente con el de la imagen 
original, la herramienta fusiona de forma transparente todos los fragmentos en un único volumen 
RAW idéntico al inicial.
<p align="center">
    <img src="img/4.png" alt="Comprobar hash de evidencias" width="400px">
</p>
A continuación, mostramos el tamaño de la partición a analizar. En el volumen Ann_HD.E01, 
hacemos clic derecho sobre el archivo, seleccionamos Propiedades y comprobamos que su tamaño 
es de 10462593024 Bytes (aproximadamente 10,46 GB)
<p align="center">
    <img src="img/5.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
Para conocer el sistema y la versión del SO instalado navegamos hasta 
Windows\System32\config\SOFTWARE y extraemos la clave de Registro SOFTWARE que 
posteriormente analizaremos con la herramienta Register Explorer.
<p align="center">
    <img src="img/6.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
Mostramos las propiedades del fichero.
<p align="center">
    <img src="img/7.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
Analizando la clave de registro Software navegamos al directorio Microsoft\Windows 
NT\CurrentVersion y nos fijamos en los valores ProductName y CurrentVersion, demostrando que el 
Sistema operativo es Windows con la versión Windows 7 Professional 
<p align="center">
    <img src="img/8.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
En este mismo directorio podemos apreciar el nombre de usuario Ann y ninguna organización 
registrada. 
<p align="center">
    <img src="img/9.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
Además el ProductID asociado al sistema.
<p align="center">
    <img src="img/10.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Debido a la ausencia de la clave CSDVersion, y a que tanto CurrentBuild como CurrentBuildNumber 
tienen valor 7600, podemos concluir que el sistema no tiene instalado ningún Service Pack. 
<p align="center">
    <img src="img/11.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
El campo InstallDate almacena la fecha y hora de instalación en formato timestamp Unix (segundos 
desde el 1 de enero de 1970, UTC); convertido a la hora local de Europa/Madrid, corresponde al 2 de 
septiembre de 2015 a las 12:17:43 CEST. 
<p align="center">
    <img src="img/12.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Por otra parte, para determinar la fecha y hora del último apagado, extraemos la clave de registro 
SYSTEM, que presenta las siguientes propiedades.
<p align="center">
    <img src="img/13.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
<p align="center">
    <img src="img/14.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Analizamos la clave con Register Explorer y en primer lugar revisamos el directorio SYSTEM\Select 
para identificar cuál ControlSet fue utilizado por el sistema en su último arranque exitoso apreciando 
que fue controlSet001, ya que el valor current es 1.
<p align="center">
    <img src="img/15.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Navegamos al directorio ControlSet001\Control\Windows y apreciamos el valor ShutdownTime en 
Hexadecimal que convertido sabemos que la hora del último apagado fue 9 de abril de 2024 a las 
13:27:57 (hora local Europa/Madrid)
<p align="center">
    <img src="img/16.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Para identificar los usuarios definidos en el sistema (excluyendo los creados por defecto) y 
determinar la fecha y hora de su último inicio de sesión, accedemos a la clave de registro SAM 
ubicada en el directorio Windows\System32\config, la extraemos y analizamos con Register 
Explorer. 
<p align="center">
    <img src="img/17.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Mostramos las propiedades de este.
<p align="center">
    <img src="img/18.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
En el directorio Domains\Account\Users se identifican dos cuentas de usuario personalizadas: Tom y 
Anna, ambas con permisos de administrador. Analizando los datos de cada cuenta, se determina que: 
• El último inicio de sesión de Tom fue el 21 de octubre de 2015 a las 09:03:02. 
• El último inicio de sesión de Anna tuvo lugar el 21 de octubre de 2015 a las 13:54:18
<p align="center">
    <img src="img/19.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
A continuación, procedemos a localizar archivos eliminados en el sistema. Para ello, analizamos el 
contenido del directorio $Recycle.Bin, correspondiente a la Papelera de reciclaje de Windows. 
Dentro de este directorio identificamos una subcarpeta asociada al SID del usuario Ann 
$Recycle.Bin\S-1-5-21-2589184436-4231671082-1653910475-1000

<p align="center">
    <img src="img/20.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
El contenido del fichero corresponde a una lista de posibles datos sensibles organizada en bloques, 
cada uno compuesto por el tipo de tarjeta "Visa", un número de tarjeta de 16 dígitos que cumple con 
el formato típico de tarjetas Visa (comenzando por el dígito 4) y un nombre completo de persona. 
Identifico que el fichero es relevante para la causa investigada. 
<p align="center">
    <img src="img/21.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Del mismo modo, localizamos el fichero $IQC0MZN.ods con la ruta al fichero 
C:\Users\Ann\Desktop\Pendientes.ods en su interior. 
<p align="center">
    <img src="img/22.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Si lo extraemos y lo abrimos con LibreOfice podemos apreciar el siguiente contenido en su interior. 
<p align="center">
    <img src="img/23.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Además de los ficheros previamente identificados, al examinar la galería de imágenes, 
específicamente en el directorio Users/Ann/Pictures/Fotos/Otras fotos, se localizó una fotografía 
denominada Playa.png con el siguiente contenido en su interior. 
<p align="center">
    <img src="img/24.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Del mismo modo, analizando los metadatos de la imagen 20150907_162718.jpg incluye coordenadas 
GPS (latitud 41.611493 y longitud 2.081493) que sitúan la ubicación aproximada en Granollers 
Barcelona. 
<p align="center">
    <img src="img/25.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
En el mismo lugar también se sacaron las fotografías 20150907_162819.jpg y 20150907_162746.jpg 
todas vinculas al dispositivo móvil SAMSUNG SM-G350 
<p align="center">
    <img src="img/26.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
En el directorio Users/Ann/Downloads/se localizaron varios archivos comprimidos. 
• ListadoNumeraciones.zip
<p align="center">
    <img src="img/27.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
El cual estaba comprimido con contraseña. 
<p align="center">
    <img src="img/28.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Para determinar si el archivo fue abierto recientemente, encontrar datos útiles en la caché y obtener 
posibles indicios sobre la contraseña utilizada, examino el directorio de archivos temporales del 
sistema, ubicado en /Users/Ann/AppData/Roaming/. Encuentro un el directorio dclogs, y dentro el 
fichero 2015-09-07-2.dc, el cual contiene parte de una conversación de Skype del usuario annetom22 
y la sentencia "Introducir contraseña: Kapo[<-][<-][<-][<-][<-]KaPoe[<-]w581!KaPow581! " y más 
abajo:: Documentos (19:41:09) 
[F2][<-]Listado n[<-][<-][<-][<-][<-][<-][<-][<-][<-]Listado numeracions[<-]es lo que me da a pensar 
que se trata de la contraseña para descomprimir el fichero.
<p align="center">
    <img src="img/29.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Después de varios intentos, determino que la contraseña es KaPow581! y al descomprimir el .zip 
encontramos el ejecutable LlistatNumeracions.exe 
<p align="center">
    <img src="img/30.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
El cual analizándolo con VirusTotal nos reporta que es un fichero malicioso.
<p align="center">
    <img src="img/31.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Al principio del fichero 2015-09-07-2.dc encontramos la sentencia "Enter password for 
C:\Users\Ann\MyHome (12:47:16) 
safePlace 
SafePlace 
" 
<p align="center">
    <img src="img/32.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Navegamos al directorio y observamos que los resultados del análisis de MyHome muestran que este 
encriptado. 
<p align="center">
    <img src="img/33.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Además, en las propiedades observamos que tiene un tamaño sospechoso. 
<p align="center">
    <img src="img/34.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
En el directorio Roaming, como vimos anteriormente, hemos encontrado una carpeta llamada 
TrueCrypt. TrueCrypt es una aplicación de cifrado de discos y contenedores de archivos que permite 
crear volúmenes cifrados para almacenar datos de forma segura; por tanto, es probable que 
MyHome sea en realidad un contenedor cifrado con TrueCrypt. 
Por lo tanto, en una máquina virtual nos descargamos la herramienta TrueCrypt, extraemos el 
contenedor de archivos MyHome y tratamos de montarlo en la máquina virtual.
<p align="center">
    <img src="img/35.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Usamos la contraseña correcta "SafePlace" 
<p align="center">
    <img src="img/36.png" alt="Comprobar tamaño de la partición" width="400px">
</p> 
Comprobamos que se monta el volumen y podemos acceder a los ficheros de su interior. 
<p align="center">
    <img src="img/37.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
El contenido del fichero pwd.txt.txt vemos las credenciales de ambos usuarios del sistema operativo, 
una contraseña para la herramienta S-Tools, un programa de esteganografía para Windows que 
permite ocultar datos dentro de archivos, credenciales de Tom que posiblemente sean para descifrar 
el volumen MyHome, credenciales de correos tanto de Tom como Ann y credenciales de inicio de 
sesión a skype de Ann. 
<p align="center">
    <img src="img/38.png" alt="Comprobar tamaño de la partición" width="400px">
</p>
<h2 id="bloque-2-informe-pericial">3. Bloque 2 – Informe Pericial</h2>

Resumen del Bloque 2... 

<h3 id="bloque-2-introduccion">3.1 Introducción</h3>

Contenido subsección 3.1... 

<h3 id="bloque-2-objeto-y-alcance">3.2 Objeto y alcance</h3>

Contenido subsección 3.2... 

<h3 id="bloque-2-metodologia">3.3 Metodología</h3>

Contenido subsección 3.3... 

<h3 id="bloque-2-resultados-y-evidencias">3.4 Resultados y evidencias</h3>

Contenido subsección 3.4... 

<h3 id="bloque-2-conclusiones-periciales">3.5 Conclusiones periciales</h3>

Contenido subsección 3.5... 

<h3 id="bloque-2-anexo">3.6 Anexo</h3>

Contenido subsección 3.6... 

<h2 id="conclusion">4. Conclusión</h2>

Contenido de la Conclusión... 
