# Configuració d'un sistema de rèplica amb Blackhole

Preparem 1 maquina mes apart de les del apartat anterior amb el Percona instalat i IPs fixa

## CONFIGURACIÓ BLACKHOLE

Modifiquem el server id i decomentem el parametre "disable_log_bin" en el my.cnf

![ScreenShot](imgs/serverId.png)

Modifiquem el auto.cnf

![ScreenShot](imgs/autocnf.png)

Reiniciem el servei de Mysql

![ScreenShot](imgs/reiniciemMysql.png)

Ara crearem l'usuari "slave"

`CREATE USER 'slave'@'IP-SLAVE' IDENTIFIED WITH mysql_native_password BY '<pwd>';`

I li assigem permisos de replicació

`GRANT REPLICATION SLAVE ON *.* TO 'slave'@'IP-SLAVE';`

![ScreenShot](imgs/slaveUser.png)

Canviarem el Master de la maquina slave

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

## CONFIGURACIÓ MASTER

Crearem l'usuari "blackhole"

`CREATE USER 'blackhole'@'IP-SLAVE' IDENTIFIED WITH mysql_native_password BY '<pwd>';`

I li assigem permisos de replicació

`GRANT REPLICATION SLAVE ON *.* TO 'slave'@'IP-SLAVE';`

## CONFIGURACIÓ SLAVE

Canviarem el Master de la maquina slave

```
CHANGE MASTER TO
-> MASTER_HOST = '<ip-servidor-blackhole>',
-> MASTER_USER = 'blackhole',
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
