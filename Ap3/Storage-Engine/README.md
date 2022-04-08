# STORAGE ENGINE

## STORAGE ENGINES QUE PODEM UTILITZAR I MODIFICAR EL MOTOR DEFAULT

Per veure els motors d'emmagatzematge que podem utilitzar, executarem la seguent sentencia

`SHOW ENGINES;`

Per veure quins motors podem utilitzar ens fixarem en l'apartat support en la captura, tot el que posi "YES" vol dir que el prodrem utilitzar

![ScreenShot](imgs/showEngines.png)

Amb la comanda anterior també podem saber quine el el Storage Engine per defecte

En el cas de Percona el Storage Engine per defecte es: "InnoDB"

![ScreenShot](imgs/default.png)

Per canviar el motor per defecte ho podrem fer de 2 maneres:

* ### TEMPORAL

Executarem la seguent sentencia

`SET default_storage_engine="<NOM_MOTOR>"`

![ScreenShot](imgs/setMemory.png)

Per verificar que ha funcionat tornarem a executar `SHOW ENGINES;`

![ScreenShot](imgs/nouDefault.png)

* ### PERMANENT

Anirem al my.cnf i afegirem el parametre `default_storage_engine`

`default_storage_engine="<NOM_MOTOR>"`

![ScreenShot](imgs/mycnfDefaultEngine.png)

I reiniciarem el servei de Percona

`systemctl restart mysqld`

![ScreenShot](imgs/serveiMySQL.png)

Per verificar que ha funcionat entrarem al mysql i tornarem a executar `SHOW ENGINES;`

![ScreenShot](imgs/nouDefault2.png)

## INSTALAR I ACTIVAR MyRocks

Per instalar MyRocks executarem la seguent comanda

`sudo yum install percona-server-rocksdb -y`

![ScreenShot](imgs/MyRocks.png)

Per activar MyRocks executarem la seguent comanda

`ps-admin --enable-rocksdb -u root -p<PswUsuari>`

![ScreenShot](imgs/activarMyRocks.png)

Posarem MyRocks com a motor per defecte

![ScreenShot](imgs/defaultMyRocks.png)

Finalment reiniciarem el servei de Percona i comprovarem que tot ha funcionat

`systemctl restart mysqld`

![ScreenShot](imgs/reiniciarPercona.png)

`SHOW ENGINES;`

![ScreenShot](imgs/comprovaMyRocks.png)

## COM UTILITZAR EL STORAGE ENGINE CSV
