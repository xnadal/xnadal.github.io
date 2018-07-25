# Instalacion de Ubuntu 16.04 LTS Xenial IFIC.  

* Instalacion normal. En ingles con teclado en castellano. Como ificadmin. Crear particiones:  
  /  
  swap (un poco mas de la RAM)  
  /var/cache/openafs (2500MB)  

* Quitar la lista de usuarios del indicator session para ificadmin:  
  `$ gsettings set com.canonical.indicator.session user-show-menu false`  

* Para desactivar la deteccion automatica de las impresoras en el fichero:   
  `/etc/cups/cups-browsed.conf`  
  la linea:      
  `BrowseProtocols none`  
  tiene que estar descomentada.  
    
   Quitar las que haya detectado.  
   Instalar las del IFIC (a mano o con el script [ific-printers-setup.sh](ific-printers-setup.sh)).  

* Actualizar e instalar los drivers de la grafica.  
  Software Updates > Additional Drivers  

* Quitar del greeter el listado de usuarios.  
  `# gedit /usr/share/lightdm/lightdm.conf.d/50-ific.conf`  

    `[SeatDefaults]`    
	`allow-guest=false`    
	`greeter-hide-users=true`   
	`greeter-show-manual-login=true`     
     
* Crear ahora los usuarios locales, si hacen falta.
     
* Bajar [ssh-setup.sh](ssh-setup.sh), [afs-setup.sh](afs-setup.sh) y [afsgetpw.deb](afsgetpw.deb). E instalarlos.   

* Cambiar el minimum\_uid en pam:  
  `# sed -i.bck 's/minimum_uid=1000/minimum_uid=5000/g' /etc/pam.d/common-{account,auth,password,session{,-noninteractive}}`  

* Activar ufw y abrir puertos 22 y 7001.  
  `# ufw limit ssh`  
  `# ufw allow afs3-callback`  
  `# ufw enable`  

* Instalar fail2ban y crear el jail.local:  
  `# apt -y install fail2ban`     
  `# gedit /etc/fail2ban/jail.local`  
   
   `[DEFAULT]`  
   `ignoreip = 127.0.0.0/8 147.156.52.0/23 147.156.160.0/22 147.156.42.0/24`  
   `bantime  = 604800`  
   `findtime = 300`  

   `[ssh]`  
   `enable   = true`   
   `maxretry = 3`  

* Crear /l/usuario  
  `# mkdir -p /l/usuario`  
  `# chown usuario.grupo /l/usuario`  
  `# chmod 750 /l/usuario`   

* Poner al usuario como administrador.  
  `# usermod -a -G adm,sudo usuario`  

* Instalar los paquetes usuales.  
  `# apt -y install emacs alpine thunderbird texlive-full lyx kile texmaker p7zip p7zip-full a2ps openjdk-8-jre icedtea-8-plugin flashplugin-installer vlc ddd gimp libdvdread4 tcsh gnome-session-flashback htop`

* Instalar los paquetes physics?  
  `# apt -y install science-highenergy-physics science-physics`

* **reboot**.  

## Configuracion con cada usuario de afs.

* Si el usuario no puede acceder con el entorno grafico, en un terminal (sin X) renombrar los directorios y ficheros de configuraciones anteriores ([save-old-config.sh](save-old-config.sh)):  
  .cache  
  .config  
  .gconf  
  .ICEauthority  
  .local  
  .nv  
  .Xauthority  
  .xsession-errors  
  
* Desde terminal (sin X) configurar el .cache en /l/usuario  
  `$ mkdir -p /l/usuario/.cache`  
  `$ rm -Rf ~/.cache`  
  `$ ln -s /l/usuario/.cache ~/.cache`  

* En el entorno grafico:  
  `$ gedit ~/.config/user-dirs.dirs`     
   anadir:  
  `XDG_CACHE_HOME="/l/usuario/.cache"`  
  
  El resto de variables tambien se pueden modificar, segun las preferencias del usuario. Pero antes de modificarlas se tienen que crear los directorios correspondientes en `/l/usuario`.

* Quitar la lista de usuarios del indicator session:  
   `$ gsettings set com.canonical.indicator.session user-show-menu false`  

* Configurar las caches de thunderbird y firefox como [antes](?file=configuracion_Ubuntu.mkd).
