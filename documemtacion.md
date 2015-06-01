**Primera Clase - Segundo Modulo.
Davis Mendoza

- Debian y aprender a realizar administración de servidores.
- Documentar todo lo que hagamos.
- Automatización.
- Practicas en clase 70%.
- Examen 15%.
- Documentación MD (Mark Down) 15%.
- Virtualizacion por Hardware. KVM.
- FHS estandar de jerarquia de sistema de archivos FHS**

# Gnu / Linux (Titulo)

## Comandos Básicos (Sub-Titulo)

**listar archivos**

    ls -l


**listar archivos, ordenar por fecha de modificacion**

    ls -lahtr  


**crear directorios**


    mkdir nombre de la carpeta

**crear carpeta y subcarpetas**

    mkdir -p /nombre_carpeta/subcarpetas
    
    mkdir -p  /tmp/{c1,c2,c3,c4,c5,c6}

    . (punto) Directorio Actual
    
    mkdir ./hola/
    
    mkdir ∼hola/ (∼ es el home)

**Crear archivo vacio**


    touch nombre_archivo

**Para editar archivos**

    nano, vi, vim, less

    ls -l > nombre_archivo (obtener todo el comando y almacenarlo en un archivo.)
    
    >> es para concatenar la informacion 
    
    echo «Hola Mundo» >> (concatenara al final del archivo los demas archivos que vayamos operando.)
    
    tail -F hola.txt (muestra en tiempo real los logs de los sistemas.)
    
    grep nombre_a_buscar archivo
    
    grep -n nombre_a_buscar archivo  (buscara la palabra y nos dira en que linea esta).
    
    grep -nR palabra directorio (Busca recursivamente la palabra dentro del directorio).
    
    grep -nR --color aa . (Busca en el directorio actual y resalta la palabra con otro color).
    
    find directorio -name nombre_archivo_carpeta (busqueda de un archivo) 
    
    find directorio -name \*la\*
    
    man find (muestra el manual de uso del comando find)
    
    whereis nano (te dice donde esta almacenado los archivos de configuración de nano, solo para comandos de la maquina).
    
    rm -r nombre_archivo_carpeta/ (-r recursivo, rm para borrar).
    
    sudo mkdir /opt/aaa (tengo permisos de administrador para poder crear carpetas)
    
    cd Documentos/ (para ingresar a carpetas)
    
    cd .. (para salir de la carpeta).
    cd ../../../ (para salir de 3 carpetas).
    cd ∼(para ingresar directamente a la carpeta home).
    
    apt-get install nombre_paquete
    aptitude install nombre_paquete (para instalar paquetes).


## Virtualizacion



### Virtualizacion completa - KVM
### Virtualizacion Ligera - LXC, Docket.

y se lo guarda como introduccion.md



# LVM  #

**Instalar LVM**

    sudo aptitude install lvm2 

**Crear volúmenes físicos:**

    sudo pvcreate /dev/sda1

Esto que hemos hecho no es dividir en particiones un disco físico, sino más bien es establecer estos volúmenes
físicos para ser usados por LVM.

**Crear un grupo de volúmenes:**

    sudo vgcreate data /dev/sda1
    
Este es el punto donde deﬁnimos que dispositivos físicos están en cuales grupos, esto es muy importante pues luego los grupos se vuelven clave para el uso de lvm.

**Crear un volumen lógico:**


    sudo lvcreate --name programas --size 5G data

Ahora deﬁnimos un volumen lógico el cual esta dentro de un grupo que ya hemos creado con anterioridad,
este nuevo volumen lógico puede crecer dentro del espacio del grupo, aunque inicialmente le deﬁnimos un
tamaño de 5G (5GB).

**Crear el sistema de archivos para el volumen lógico:**


    sudo mkfs.ext3 /dev/data/programas

Aunque el volumen lógico está ya creado, de forma similar que una partición normal, no contiene aún un
formato y por eso le creamos uno. 

**Montar el volumen lógico:**

Primero debemos tener un punto de montaje, por lo cual crearé un carpeta en mi directorio personal.

    sudo mkdir /home/miusuario/programas
    
Ahora el comando normal de montaje en Linux.

    mount /dev/data/programas /home/miusuario/programas

**Conﬁgurar /etc/fstab:**

Para que el volumen lógico esté accesible sin necesidad de usar la cuenta de administrador root creamos una
nueva línea en el archivo de conﬁguración fstab sudo gedit /etc/fstab se debe cambiar gedit por el editor
favorito de cada cual. En fstab crear un linea similar a la siguiente, agregando las opciones preferidas de cada
quien:


    /dev/data/programas /home/miusuario/programas xfs defaults,users,noatime 0 0

# Permisos de Carpetas #

**Usuario propietario y grupo propietario de un archivo**

En Unix todos los archivos pertenecen obligatoriamente a un usuario y a un  grupo.  

Cuando  un  usuario  crea  un  nuevo  archivo,  el  propietario  del  archivo  será  el  usuario  que  lo  ha
creado y el grupo del archivo será el grupo principal de dicho usuario.


**Tipos de permisos**

En los Sistemas Unix, la gestión de los permisos que los usuarios y los grupos de usuarios tienen sobre los
archivos y las carpetas, se realiza mediante un sencillo esquema de tres tipos de permisos que son:

- Permiso de lectura
- Permiso de escritura
- Permiso de ejecución

**Permiso de lectura**

Cuando un usuario tiene permiso de lectura de un archivo significa que puede leerlo o visualizarlo, bien sea
con  una  aplicación  o  mediante  comandos.  

Cuando un usuario tiene permiso de lectura de una carpeta, significa que puede visualizar el contenido de la
carpeta,  es  decir,  puede ver  los  archivos  y  carpetas  que contiene,  bien sea con el  comando 'ls'  o con un explorador de archivos como Konqueror. Si el usuario no tiene permiso de lectura sobre la carpeta, no podrá ver lo que contiene.

El permiso de lectura se simboliza con la letra 'r' del inglés 'read'.

**Permiso de escritura**

Cuando un usuario tiene permiso de escritura sobre un archivo significa que puede modificar su contenido, e
incluso borrarlo. 

También le da derecho a cambiar los permisos del archivo mediante el comando chmod así como  cambiar  su  propietario  y  el  grupo  propietario  mediante  el  comando  chown.  

Si  el  usuario  no  tiene
permiso de escritura, no podrá modificar el contenido del archivo.


Cuando un usuario tiene permiso de escritura sobre una carpeta, significa que puede modificar el contenido
de la carpeta, es decir, puede crear y eliminar archivos y otras carpetas dentro de ella. 

Si el usuario no tiene
permiso de escritura sobre la carpeta, no podrá crear ni eliminar archivos ni carpetas dentro de ella.

El permiso de escritura se simboliza con la letra 'w' del inglés 'write'.


**Permiso de ejecución**

Cuando un usuario tiene permiso de ejecución de un archivo significa que puede ejecutarlo. 

Si el usuario no dispone de permiso de ejecución, no podrá ejecutarlo aunque sea una aplicación.

Los únicos  archivos ejecutables  son las  aplicaciones y los archivos  de comandos (scripts).  

Si  tratamos de ejecutar un archivo no ejecutable, dará errores.

Cuando un usuario  tiene permiso de ejecución sobre una carpeta,  significa  que puede entrar  en ella,  bien
sea con el  comando 'cd'  o con un explorador  de archivos  como Konqueror.  

Si  no dispone del  permiso de ejecución significa que no puede ir a dicha carpeta.

El permiso de ejecución se simboliza con la letra 'x' del inglés 'eXecute'.


# SUDO #

**INSTALAMOS SUDO**

Instalamos sudo en una consola o terminal logueado como administrador o root :


    apt-get install sudo

**CONFIGURAMOS SUDO**

Una vez terminada la instalación de sudo vamos a configurarlo editando el archivo de
configuración **"/etc/sudoers"**. 

Es donde vamos a indicarle el/los usuario/s que vamos a
autorizar, que aplicaciones vamos a permitir utilizar a cada usuario y como lanzarlas. 

Aunque tenemos muchas mas opciones que podemos configurar,  me voy a centrar en las necesarias para poder autorizar un usuario a realizar tareas administrativas. 

Simplemente vamos a añadir una linea con una normativa especifica para un usuario, pero si es necesario
puedes añadir tantas lineas como necesites con diferentes normativas para cada usuario.

**Breve explicación**

Antes de añadir la linea con la normativa de uso para el usuario, vamos a hablar básicamente de su estructura para que cada uno pueda personalizar la
configuración según sus necesidades. 

Podemos dividirla en 4 campos :

1. "usuario_autorizado" : Usuario al que vamos a autorizar el uso de sudo.

2. "host" : IP o HOST desde el que queremos autorizar el uso de sudo.

3. "privilegios_de_que_usuario" : Nombre del usuario del cual vamos a usar sus privilegios para lanzar el comando.

4. "comando" : Aquí le indicarnos el/los comando/s que queremos autorizar y como lanzarlos.

**Continuamos con lo practico**

Tenemos que editar el archivo de configuración "sudoers". Yo uso nano. 

En una consola o terminal logueado como administrador o root :


    nano /etc/sudoers

Buscamos la linea

    root ALL=(ALL) ALL

Y justo en la linea siguiente es donde tenemos que añadir la nueva linea de la que hemos estado hablando un poco mas arriba. 

Yo voy a usar la opción mas común, que es la que os
va a servir a todos. 

Voy a autorizar a mi usuario para poder realizar cualquier tarea administrativa verificando la identidad con contraseña. 

La linea a añadir seria algo como

    (sustituye "JOSE" por el nombre de tu usuario) : JOSE ALL=(ALL:ALL) ALL

Una vez realizados los cambios necesarios procedemos a guardarlos. 

    Control+X para salir.

Nos pregunta si queremos guardar los cambios, "S" para confirmar.

Nos pide verificar el nombre de archivo, ENTER para confirmar.

Y ya tenemos sudo instalado y configurado. Podemos cerrar la consola de root.

