# REALITZAR UN BACKUP I RESTAURAR-LO

Instalem el Percona Xtrabackup

`yum install percona-xtrabackup-80`

![ScreenShot](imgs/install.png)

Creem la carpeta `/backups` i li donem permisos

![ScreenShot](imgs/carpeta.png)

Creem el backup

`xtrabackup --backup --datadir=/var/lib/mysql/ --target-dir=/<target-path> --user=<usuari> --password=<password>`

![ScreenShot](imgs/backup.png)

Preparem el backup

`xtrabackup --prepare --target-dir=/<targetBackup>/`

![ScreenShot](imgs/prepare.png)

Parem el servei de mysql

![ScreenShot](imgs/stop.png)

Ara renombrem la carpeta `/var/lib/mysql/`

![ScreenShot](imgs/mv.png)

I restaurarem el backup que acabem de crear

`rsync -avrP /<pathBackup>/* /<pathDesti>`

![ScreenShot](imgs/restore.png)

canviem els permisos dels fitxers que ha creat el xtrabackup a mysql

![ScreenShot](imgs/chown.png)

I aixequem el servei de mysql

![ScreenShot](imgs/start.png)
