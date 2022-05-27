# REALITZAR UN BACKUP I RESTAURAR-LO

Instalem el Percona Xtrabackup

`yum install percona-xtrabackup-80`

![ScreenShot](imgs/install.png)

Creem la carpeta `/backups` i li donem permisos

![ScreenShot](imgs/carpeta.png)

Creem el backup

`xtrabackup --backup --datadir=/var/lib/mysql/ --target-dir=/<target-path> --user=<usuari> --password=<password>`

![ScreenShot](imgs/backup.png)
