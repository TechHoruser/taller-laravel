# Guía Instalación Laravel 5.6

## Creación de entorno de desarrollo con VirtualBox y Vagrant

Para trabajar en entornos homogeneos usaremos un entorno virtualizado gracias a vagrant y la *box* oficial facilitada por Laravel. En primer lugar necesitaremos tener un sofware de virtualización, en este caso usaremos VirtualBox que podremos descargar en su [página oficial](https://www.virtualbox.org/wiki/Downloads), no sin antes revisar las versión que vamos a descargar, ya que necesitaremos compatibilidad con **vagrant**, para eso revisar el contenido de este otro enlace [Compatibilidad Vagrant](https://www.vagrantup.com/docs/virtualbox/). 

Yo personalmente en mi sistema unix uso las versiones:
 - Virtualbox: Versión 5.2.6
 - Vagrant: Vagrant 2.0.1

Posteriormente instalar Vagrant descangándolo también de su [página oficial](https://www.vagrantup.com/downloads.html).

A continuación ejecutar el siguiente comando para descargar *box* eliguiendo la opción de VirtualBox al ejecutar: 

```bash
vagrant box add laravel/homestead
```

Una vez finalizada la descarga podremos instalar de manera cómoda la máquina virtual que nos facilita laravel. Para ello ejecutar en la terminal:

```bash
git clone https://github.com/laravel/homestead.git ~/Homestead
cd ~/Homestead
git checkout v7.3.0
```

###### Mac / Linux
```bash
bash init.sh
```

###### Windows
```bash
init.bat
```

La configuración por defecto en *Homestead.yaml* ya incluye una carpeta compartida:

```yaml
folders:
    - map: ~/code
     to: /home/vagrant/code
```

Para que la sincronización de la carpeta se lleve a cabo correctamente debemos instalar el plugin de [VirtualBox Guest Additions](https://github.com/dotless-de/vagrant-vbguest#vagrant-vbguest), si estamos siguiendo el tutorial solo debemos ejecutar:

```bash
vagrant plugin install vagrant-vbguest
```

###### Si no queremos el proyecto en *~/code* introducir el path completo donde se encontrá la carpeta con el proyecto. En mi caso */home/fran/proyect-laravel*.

Y también un virtualhost:
```yaml
sites:
    - map: homestead.test
      to: /home/vagrant/code/Laravel/public
```
Ahora debemos incluir ese host en nuestro fichero de hosts de la siguiente forma:


###### Mac / Linux (fichero): */etc/hosts*
###### Windows (fichero): *C:\Windows\System32\drivers\etc\hosts*

```bash
192.168.10.10  homestead.test
```

Ya podemos levantar la máquina virtual, para ello ejecutamos:

```bash
vagrant up
```

#### Posibles Errores
* En caso de ver el error *Check your Homestead.yaml file, the path to your private key does not exist.*, es debido a que el sistema no tiene una clave rsa creada, usar el comando ssh-keygen.
* En caso de ver, un error como el siguiente, se debe a que no esta creada la carpeta donde vamos a generar el proyecto, como vimos anteriormente por defecto espera ver la carpeta *~/code*, tenemos dos opciones crear dicha carpeta o editar el fichero *Homestead.yaml*:
```
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
    homestead-7: Unable to mount one of your folders. Please check your folders in Homestead.yaml
```
* Posible error de **permisos generados con el comando *Artisan*** (habitual usuarios linux). Incluir *type: "nfs"* en Homestead.yaml e instalar en caso de no tener nfs server *sudo apt-get install nfs-kernel-server*.




## Instalación Laravel
Accedemos por ssh a la máquina virtual:

```bash
vagrant ssh
```

Una vez conectado ir a la carpeta donde vamos a alojar el proyecto, en nuestro caso lo alojaremos en la carpeta compartida si recordamos de los pasos anteriores:

```bash
cd /home/vagrant/code
```

Una vez aquí ejecutamos la instalación de Laravel via composer de la siguiente forma:

```bash
composer create-project --prefer-dist laravel/laravel .
```

Comprobar que conectamos al proyecto en la url **http://homestead.test/**. Deberíamos de ver lo siguiente:

![Laravel](https://raw.githubusercontent.com/mortalswat/taller-laravel/master/imgs/laravel.png)

Es aconsejable realizar una pequeña prueba para ver que disponemos de sincronización de las carpetas (Entre la máquina anfitriona y la virtualizada). Para ello abrimos el fichero *./resources/views/welcome.blade.php* y editamos la linea 82 del fichero sustituyendo *Laravel* por *Hola Laravel*. Posteriormente refrescamos el navegador y podremos ver que ha cambiado el texto. Quedando algo así:

![Hola Laravel](https://raw.githubusercontent.com/mortalswat/taller-laravel/master/imgs/hola-laravel.png)