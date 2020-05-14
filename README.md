# Presentación "Prácticas virtuales para optimizar las reales"

Materiales y enlaces relacionados con el PechaKucha "Prácticas virtuales para optimizar las reales" presentado en el I Congreso Virtual de [Apuntes FP Informática](https://apuntesfpinformatica.es).

* [1. Vídeo](#1-vídeo)
* [2. Diapositivas](#2-diapositivas)
* [3. Archivo H5P de la actividad](#3-archivo-h5p-de-la-actividad)
* [4. Cómo utilizar la extensión de Branching Scenario](#4-cómo-utilizar-la-extensión-de-branching-scenario)
	* [4.1. Preparación del entorno](#41-preparación-del-entorno)
	* [4.2. Descarga e instalación de Branching Scenario modificado](#42-descarga-e-instalación-de-branching-scenario-modificado)
	* [4.3. Uso](#43-uso)
	* [4.4. Ejemplo: Conexión con Learning Locker](#44-ejemplo-conexión-con-learning-locker)
* [5. Más información](#5-más-información)


## 1. Vídeo

[https://youtube.com/watch?v=0siJGtKph3s](https://youtube.com/watch?v=0siJGtKph3s)

## 2. Diapositivas

Se pueden consultar las diapositivas presentadas durante el vídeo [aquí](https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/Diapositivas.pdf).

## 3. Archivo H5P de la actividad

La actividad ha sido editada eliminando las opciones derivadas de la extensión (entorno en 360 grados y sentencias xAPI modificadas) para que pueda probarse en cualquier sistema que tenga instalado el plugin de H5P. Solamente hay que marcar la opción "Subir" en el HUB H5P del sistema y subir el archivo que se puede descargar [aquí](https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/practica-configura-tu-router-domestico.h5p).

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/inicio-practica.png" width="350"/></p> 

## 4. Cómo utilizar la extensión de Branching Scenario

Si se pretende crear una actividad en Branching Scenario que pueda conectarse con un LRS y quede registrado el nodo de contenido recorrido y el tiempo permanecido en cada uno de ellos por el usuario, es necesario utilizar la versión modificada del mismo. Se ha solicitado un pull request al desarrollo principal, pero mientras no sea aprobado, hay que hacer una instalación manual siguiendo las instrucciones a continuación.

### 4.1. Preparación del entorno

Es necesario contar con un sistema Drupal 7 instalado, junto con los plugins de H5P, Libraries y Tin Can API. En la configuración del módulo H5P, hay que activar las opciones:

* Enable development mode
* Enable library development directory (For programmers only)

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/config-h5p.png" width="700"/></p> 

Además, en el HUB de H5P hay que instalar los tipos de contenido "Course Presentation", "Interactive Video" e "Image Hotspots". Pero no debería instalarse "Branching Scenario", ya que se instalará una versión modificada y podrían crearse conflictos.

### 4.2. Descarga e instalación de Branching Scenario modificado

En una terminal del servidor en el que esté instalado Drupal 7, debemos acceder a la carpeta de instalación de Drupal y a la siguiente ruta dentro de ella: sites/default/files/h5p/development.

Una vez ahí, ejecutamos los siguientes comandos:
```
git clone https://github.com/DavidLMS/h5p-branching-scenario
git clone https://github.com/h5p/h5p-branching-question
git clone https://github.com/h5p/h5p-editor-branching-scenario
git clone https://github.com/h5p/h5p-editor-branching-question
git clone https://github.com/h5p/h5p-material-design-icons.git
mv h5p-branching-scenario H5P.BranchingScenario
mv h5p-branching-question H5P.BranchingQuestion
mv h5p-editor-branching-scenario H5PEditor.BranchingScenario
mv h5p-editor-branching-question H5PEditor.BranchingQuestion
```

Posteriormente, ejecutamos los siguientes:
```
cd H5PEditor.BranchingScenario
sudo npm install
sudo npm audit fix
sudo npm run-script build
cd ../H5P.BranchingScenario
sudo npm install
sudo npm audit fix
sudo npm run-script build
```

Si después de ejecutar alguno de los dos comandos "sudo npm install" obtenemos un error, puede intentar ejecutarse así:
```
sudo npm install -g --unsafe-perm=true — allow-root
```

### 4.3. Uso

Una vez realizado lo anterior, la versión de Branching Scenario que aparecerá en el HUB de H5P será la modificada y podremos usarla para crear la actividad. Tendremos que conectar nuestro LRS preferido en la configuración del módulo Tin Can API para poder revisar los registros de uso de la misma.

### 4.4. Ejemplo: Conexión con Learning Locker

Si se quiere realizar la conexión de la xAPI con un sistema Learning Locker, primero crearemos un almacén (Store) en Settings->Stores de Learning Locker.

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/storelearninglocker.png" width="450"/></p> 

Después, en Settings->Clients creamos un nuevo cliente. Copiamos los datos "Key" y "Secret", además del "xAPI Endpoint" y pasamos a configurar el plugin Tin Can Api de Drupal.

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/config-learning-locker.png" width="450"/></p> 

Ahí será donde pongamos el Endpoint, así como el "User", que se corresponde con "Key", y "Password", que se corresponde con "Secret". Solamente nos queda guardar la configuración.

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/config-tin-can-api.png" width="450"/></p> 

Así, cada vez que un usuario avance en un nodo de contenido de la actividad creada en Branching Scenario, aparecerá en Learning Locker una sentencia tipo "progressed" con el identificador del nodo y el tiempo permanecido en el mismo.

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/learninglocker.png" width="450"/></p> 

 <p align="center"> <img src="https://github.com/DavidLMS/PresentacionPracticasVirtualesParaOptimizarLasReales/raw/master/images/json.png" width="450"/></p> 

El número de identificación del nodo es el que está definido en el archivo .json de la actividad dentro de la carpeta content. Para acceder al mismo, solamente tenemos que descargarnos nuestra actividad, cambiar la extensión .h5p por .zip y descomprimirlo.

Estos datos pueden exportarse en .csv para automatizar su análisis.

## 5. Más información

El proceso de desarrollo de la extensión realizada en Branching Scenario y la creación de la actividad están detallados en el [Trabajo de Fin de Máster](https://rodin.uca.es/xmlui/handle/10498/21831).

Los resultados derivados de la aplicación de la actividad en una clase se pueden encontrar en el [artículo de investigación](https://ieeexplore.ieee.org/document/8970117).
