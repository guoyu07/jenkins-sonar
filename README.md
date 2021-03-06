# Jenkins - Sonarqube

# Overview 

Este repositorio contiene código para la generación de un ambiente de Inspección Continua, basada en Jenkins
y Sonarqube

[Articulo Referencia](https://dzone.com/articles/dockerizing-jenkins-2-setup-and-using-it-along-wit?edition=309196&utm_source=Daily%20Digest&utm_medium=email&utm_campaign=dd%202017-07-19)

# Pre-Requisitos
----

* Docker Community Edition.



## Construir la imagen docker para el contenedor Jenkins
----

Se utilizarán 2 imagenes docker una para Jenkins y otra para Sonarqube.

## Paso a Paso

## Construir imagen *myjenkins*.
``` bash
docker build -t myjenkins . 
```
## Ejecutar contenedor de nombre myjenkins  basado en la imagen myjenkins:latest

```bash
docker run -p 8080:8080 -v `pwd`/downloads:/var/jenkins_home/downloads --rm --name myjenkins myjenkins:latest
```

## Ejecutar contenedor con volumenes de persistencia.

```bash
docker run -p 8080:8080  \ 
-v `pwd`/downloads:/var/jenkins_home/downloads \
-v `pwd`/jobs:/var/jenkins_home/jobs/ \
-v `pwd`/m2deps:/var/jenkins_home/.m2/repository/ \
--rm \
--name myjenkins myjenkins:latest
```

Los volumenes de persistencia son segmentos de storage dispuesto por el host para persistir la data en el exterior del contenedor. Para usar esta 
opción por primera vez, se debe contar con una copia de los volumenes de arranque (rodrigovaldes@gmail.com).


## Procedimiento de Backup de Volumen

Para hacer un backup del estado de los volumenes se debe usar el script/bash/backup-volume.sh. 

### Ejemplo

```bash
cyberlaptop:jenkins-sonar rvaldes$  ./scripts/bash/backup-volume.sh download `pwd`
tar -czvf download-18082017.tar.gz /Users/rvaldes/Containers/personal/jenkins-sonar
```

# Sonarqube

## Obtener la última version de la imagen oficial de Sonarqube.
 
````bash
docker pull sonarqube:latest
````

## Obtener la versión LTS 5.6.6
````bash
docker pull sonarqube:5.6.6
````

## Inciar el contenedor con volumen de Persistencia
  
 ````bash 
  docker run -d --name sonarqube \
  -p 9000:9000 \
  -p 9092:9092 \
  -v `pwd`/data/sonarqube:/opt/sonarqube \
  sonarqube:5.6.6
  ````
  ````bash
  docker run -d --name sonarqube \
    -p 9000:9000 \
    -p 9092:9092 \
    sonarqube:5.6.6
````
