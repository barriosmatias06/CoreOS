# CoreOS
Instalación Fedora CoreOS
https://docs.fedoraproject.org/en-US/fedora-coreos/bare-metal/


Descargar ISO > LINK
Desde Windows mediante Rufus >> seleccionar la imagen en modo DD.
Eso genera un pendrive booteable con #bash

Antes de instalar FCOS, debe tener un archivo de configuración de Ignition y alojarlo en algún lugar (por ejemplo, usando python3 -m http.server). Si no tiene uno, consulte Producción de un archivo de encendido . 
#Instalar butane para transpilar ignition.bu > ignition.ign
#Ej ArchLinux > git clone https://aur.archlinux.org/butane.git
Una vez instalado , ejecute Butane en la configuración de Butane:
butane --pretty --strict example.bu > example.ign

Cuando esté en etapa bash ejecutar el siguiente comando:

#Example 
sudo coreos-installer install /dev/sda --ignition-url http://192.168.231:8000/ignition.ign -–insecure-ignition
Instalar paquetes sudo rpm-ostree install (paquete)

Instalar docker-compose 
root: sudo rpm-ostree install docker-compose

–: 
	Carpeta para Compartir archivos al svr:
	ej: sftp://${serverip}/var/container
	Usuario para los chicos. (accesos) : usuariopepe
	
##Fedora Core OS ignition.bu > ignition.ign

variant: fcos
version: 1.4.0
passwd:
  users:
    - name: coreos
      password_hash: $y$j9T$JK55R4.mv9QRB4M/IIgjy1$6XqMG2yE2BF1v7vSPISQ/uBreT
      #pwroot
    - name: usuario1
      groups:
        - "sudo"
        - "docker"
      ssh_authorized_keys:
        - ssh-ed25519 Dsne1.33fg17rbv0$j/JZNIii0FV.X2fLA77QV1bRYTXn5Pi $[keyssh}
    - name: usuario2
      groups:
        - "sudo"
        - "docker"
      ssh_authorized_keys:
        - ssh-ed25519 Dsne1.33fg17rbv0$j/JZNIii0FV.X2fLA77QV1bRYTXn5Pi ${key-ssh}
    - name: usuariogenerico
      groups:
        - "docker"
      password_hash: $y$j9T$M.HlEADsne1.33fg17rbv0$j/JZNIii0FV.X2fLA77QV1bRYTXn5PijGPAfaDGHmD/
      #pwusuariogenerico
      
systemd:
  units:
    - name: serial-getty@ttyS0.service
      dropins:
      - name: autologin-core.conf
        contents: |
          [Service]
          # Override Execstart in main unit
          ExecStart=
          # Add new Execstart with `-` prefix to ignore failure`
          ExecStart=-/usr/sbin/agetty --autologin core --noclear %I $TERM
storage:
  files:
    - path: /etc/hostname
      mode: 0644
      contents:
        inline: |
          dockersrv
    - path: /etc/ssh/sshd_config.d/20-enable-passwords.conf
      mode: 0644
      contents:
        inline: |
          # Fedora CoreOS disables SSH password login by default.
          # Enable it.
          # This file must sort before 40-disable-passwords.conf.
          PasswordAuthentication yes


