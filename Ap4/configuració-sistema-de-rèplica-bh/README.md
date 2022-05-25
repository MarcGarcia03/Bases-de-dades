# Configuració d'un sistema de rèplica

Preparem 2 maquines (master i slave) amb el Percona instalat i IPs fixes

Per canviar la IP executarem el seguent

`nmtui`

I se'ns obrira un "entorn grafic"

Fem enter

![ScreenShot](imgs/ip1.png)

Seleccionem la connexió

![ScreenShot](imgs/ip2.png)

Anem a "Automático" dins l'apartat de IPv4

![ScreenShot](imgs/ip3.png)

I seleccionem "Manual"

![ScreenShot](imgs/ip4.png)

Afegim la IP, i anem a "Aceptar"

![ScreenShot](imgs/ip5.png)

Anem a "Atras"

![ScreenShot](imgs/ip6.png)

I anem a "Salir"

![ScreenShot](imgs/ip7.png)

Per aplicar els canvis farem el seguent

`ifdown <nom tarjeta xarxa>`

`ifup <nom tarjeta xarxa>`

![ScreenShot](imgs/ip8.png)

I executem `nmcli` per verificar que funciona

![ScreenShot](imgs/ip9.png)

I farem el mateix proces a la maquina "Slave"

## CONFIGURACIÓ MASTER

Comprovar el "server-id" esta en 1

![ScreenShot](imgs/serverId.png)

Farem un FLUSH dels LOGS

`mysql> FLUSH LOGS;`

![ScreenShot](imgs/flushLogs.png)

Crearem un backup de la BBDD sakila (importada previament)

`mysqldump --user=root --password=<pwd> --master-data=2 sakila > /<path on es fará el backup>/backup.sql`

![ScreenShot](imgs/crearBackup.png)

Ara mirarem els valors de "MASTER_LOG_FILE" i "MASTER_LOG_POS" dins del backup

`nano /<pathBackup>`

Farem Ctrl + W i buscarem "CHANGE MASTER TO"

I segudament trobarem els valors de "MASTER_LOG_FILE" i "MASTER_LOG_POS"

![ScreenShot](imgs/valors.png)

Ara crearem l'usuari "slave" a la maquina master

`CREATE USER 'slave'@'IP-SLAVE' IDENTIFIED WITH mysql_native_password BY '<pwd>';`

I li assigem permisos de replicació

`GRANT REPLICATION SLAVE ON *.* TO 'slave'@'IP-SLAVE';`

![ScreenShot](imgs/slaveUser.png)

## CONFIGURACIÓ SLAVE

Parem el servei de Mysql

![ScreenShot](imgs/pararMysql.png)

Anirem al my.cnf i deshabilitarem el binlog i assignarem un server-id diferent al del master

Per deshabilitar el binlog descomentarem el parametre "disable_log_bin"

I per modificar el server_id, afegim el parametre i li assignem el numero que volguem

![ScreenShot](imgs/mycnfSlave.png)

Iniciem el servei de Mysql

![ScreenShot](imgs/startMysql.png)

Comprovem els canvis

`SHOW VARIABLES LIKE '%log_bin%';`

![ScreenShot](imgs/verificarBinlog.png)

`SHOW VARIABLES LIKE '%server_id%';`

![ScreenShot](imgs/verificarServerId.png)

[OPCIONAL]Ara anirem al seguent archiu

`/var/lib/mysql/auto.cnf`

I el modificarem, ja que si esten en un entorn virtual, i hem duplicat maquines tindrem IDs repetits

![ScreenShot](imgs/.png)

Ara canviarem el Master de la maquina slave

```
CHANGE MASTER TO
-> MASTER_HOST = '<ip-servidor-master>',
-> MASTER_USER = 'slave',
-> MASTER_PASSWORD = '<pwd>',
-> MASTER_PORT = 3306,
-> MASTER_LOG_FILE = '<valor trobat anteriorment>',
-> MASTER_LOG_POS = <valor trobat anteriorment>,
-> MASTER_CONNECT_RETRY = 10;
```

![ScreenShot](imgs/changeMaster.png)

I iniciem l'slave

`START SLAVE;`

![ScreenShot](imgs/startSlave.png)

Executant la seguent comanda, hauriem de trobar el seguent misatge, per asegurarnos de que tot ha sortit bé

`SHOW SLAVE STATUS\G;`

![ScreenShot](imgs/statusSlave.png)

Ara crearem una taula en el Master dins de sakila, i hauriem de veure aquesta taula a l'slave´

```
CREATE TABLE slave (
    prova INT
);
```

![ScreenShot](imgs/createSlave.png)

Mirem quines taules te sakila a l'slave

`SHOW TABLES sakila;`

![ScreenShot](imgs/mirarTaulaSlave.png)
