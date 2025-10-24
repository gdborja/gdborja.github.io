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
