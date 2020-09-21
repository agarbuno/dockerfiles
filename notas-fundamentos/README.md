# Instrucciones para generar contenedor con las notas de Fundamentos.  

Necesitas tener instalado el [`Docker Desktop`](https://docs.docker.com/desktop/) si tu
sistema operativo es `MacOS` o `Windows`; o alternativamente, [`Docker
Engine`](https://docs.docker.com/engine/), si estás corriendo alguna
distribución de `Linux`.

## Generar imagen equivalente a [`Docker Hub`](https://hub.docker.com/repository/docker/agarbuno/notas-fundamentos)

Clona este repositorio en tu máquina y posicionáte en el directorio resultante para Dockerfile de las notas del curso. 
Es decir, 

```{bash}
mkdir -p ~/ruta/donde/va/el/repo
cd ~/ruta/donde/va/el/repo
git clone https://github.com/agarbuno/dockerfiles.git
cd dockerfiles/notas-fundamentos
```

Ahora genera la imagen en tu máquina con 
```{bash}
docker build --tag notas-fundamentos .
```
**Nota.** El procedimiento puede tardar un poco. Usualmente 20 minutos, dependiendo de tu conexión a internet. La razón es que 
se descarga el repositorio de las notas y también todas las librerías de `R` que utilizan en ellas. 

Unas vez generada la imagen en tu computadora puedes iniciarla con 
```{bash}
docker run -d -p 8787:8787 -e PASSWORD=<escribeunacontraseña> -m 4g agarbuno/notas-fundamentos
```
En general se necesita correr la imagen con 4Gb de memoria (`-m 4g`).  La opción
`-d` es opcional, y sirve para dejar corriendo el contenedor en el *background*
de tu sesión de terminal. Cuidado con esta opción por que si no tienes cuidado
podrías estar levantando múltiples contenedores en tu máquina.

Esta imagen leventará una sesión de `Rstudio Server` a la cual podrás acceder en
tu navegador de internet de preferencia por medio de
```{bash}
localhost:8787/
```
el cual te pedirá un usuario y contraseña. La contraseña es la misma que deberás
de cambiar en las líneas de arriba: *<escribeunacontraseña>*. El usuario es, por
*default*, `rstudio`.

Una vez en la pantalla de `Rstudio` puedes empezar con cargar las notas como un
proyecto.

## Genera la imagen con tu fork

Las instrucciones son similares, pero tendrás que modificar el `Dockerfile` para que apunte al 
repositorio de `Github` correspondiente. Esto lo lograrás cambiando la variable de ambiente 
```{bash}
ENV GITHUB_HOST tu_usuario
```
