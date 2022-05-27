
# “Desplegando Shiny Apps de manera local en Windows”

## Herramientas a utilizar:

[DesktopDeployR](https://github.com/wleepang/DesktopDeployR)  
[R Portable](https://sourceforge.net/projects/rportable/)

## Pasos a seguir:

### Paso 1:

Clonar repositorio de DesktopDeployR

    git clone https://github.com/wleepang/DesktopDeployR.git MyApplication

### Paso 2:

Instalar R portable, el cual se debe descargar desde:

    https://sourceforge.net/projects/rportable/ 

Esto se debe instalar en la carpeta “//dist” de nuestra carpeta de
DesktopDeployR. Por ejemplo, usando el git clone anterior,
“/MyApplication/dist”.

### Paso 3:

Pegar ui.R, server.R y demás archivos asociados al proyecto en la
carpeta app/shiny dentro de MyApplication.

### Paso 4:

Comprobar que los paquetes necesarios se encuentren en app/packages.txt,
uno por renglón.

### Paso 5:

Cambiar el atributo “home” de “r_exec” en el archivo /app/config.cfg, y
sustituirlo por:

    "dist\R-Portable\App\R-Portable\bin\"

La cual sería la ruta del R portable previamente instalado.

### Paso 6:

Comprobar que el nombre del archivo .bat sea el mismo que la carpeta, en
este caso MyApplication. De no ser así, cambiar el nombre, en este caso
“MyApplication”.

### Paso 7:

Ejecutar el .bat e instalar con un navegador basado en Chromium. (Edge,
Chrome, Brave, etc)

<br></br>

## Problemas conocidos y su solución:

Al desplegarse la aplicación, su puerto es aleatorio, lo que provoca que
el link entre Chromium y la aplicación en shiny sea temporal tras el
primer arranque, esto se soluciona asignando un puerto en el archivo
app/app.r, con el código: options(shiny.port = 8231)

![](puerto.png)

Es necesario correr el archivo batch de la aplicación (“MyApplication”
en el ejemplo) cada vez que arranca el sistema operativo, debido a que
tras el primer arranque, el motor de R portable deja de ejecutarse, una
vez apagado el equipo. Esto provoca que R Portable no se despliegue en
arranques posteriores a la primera ejecución del archivo batch. Esto se
soluciona creando un acceso directo al archivo batch y pegándolo en la
carpeta:

    C:\Users\<Usuario>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

Para obtener su ruta, también se puede acceder usando la ventana
Run/Ejecutar, con el comando:

    shell:startup

![](shell.png)

Esto lo que consigue es que el acceso directo se ejecute cada vez que el
sistema arranque, desplegando así el archivo batch, junto al motor de R
portable.

Una vez solucionados los problemas anteriores, cada vez que arranca el
sistema, se va a desplegar el navegador con el primer arranque del motor
de R y la aplicación en cuestión, para evitar que el navegador se
despliegue en cada arranque, hay que cambiar el atributo launch.browser
en el archivo app/app.r, tal y como se muestra en la Imagen 1, el valor
por defecto de este atributo es TRUE.
