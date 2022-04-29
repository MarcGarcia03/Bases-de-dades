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

Crearem una base de dades per proves

`CREATE DATABASE <NomDB>`

![ScreenShot](imgs/DBprova.png)

I una taula amb camps NOT NULL, ja que el motor CSV no suporta camps Nulls

`CREATE TABLE <NomTaula> (<camp1> <parametre1>... NOT NULL, <camp2> <parametre1>... NOT NULL)`

![ScreenShot](imgs/taulaProva.png)

Introduirem dades en la taula

`INSERT INTO <NomTaula> VALUES(<camp1>,'<camp2>'),(<camp1>,'<camp2>');`

![ScreenShot](imgs/insertProva.png)

I mirarem la taula

`SELECT * FROM <NomTaula>;`

![ScreenShot](imgs/selectProva.png)

Ara anirem a buscar l'arxiu CSV i l'obrirem

L'arxiu CSV esta situat a la seguent ruta: `/var/lib/mysql/<NomDB>/<NomTaula>.CSV`

![ScreenShot](imgs/nanoCSV.png)

![ScreenShot](imgs/CSV.png)

## STORAGE ENGINE MyRocks

Al final del primer apartat posem el MyRocks per defecte, si no ho fem els seguents pasos no funcionaran

Ara en la Base de dades que hem creat anteriorment afegirem 3 taule i afegirem dades a les 3 taules

`USE <NomDB>`

`CREATE TABLE <NomTaula> (<camp1> <parametre1>..., ...)`

![ScreenShot](imgs/crearTaules.png)

`INSERT INTO <NomTaula> VALUES(<camp1>,'<camp2>'),...;`

![ScreenShot](imgs/insertMyRocks.png)

Ara anirem a buscar els fitxers de dades

`cd /var/lib/mysql/<NomDB>`

`du -sh *`

El numero de l'esquerre son els Kb que pesen els arxius

![ScreenShot](imgs/pesFitxers.png)

Per veure la compressio per defecte, executarem la seguent sentencia

`SELECT * FROM information_schema.rocksdb_cf_options WHERE option_type LIKE '%ompression%' AND cf_name='default';`

![ScreenShot](imgs/veureCompressioPerDefecte.png)

Per deshabilitar la compresio dels fitxers de les taules, anirem al fitxer `my.cnf`, i afegirem el seguent

```
rocksdb_default_cf_options="write_buffer_size=256m;target_file_size_base=32m;max_bytes_for_level_base=512m;max_write_buffer_number=4;level0_file_num_compaction_trigger=4;level0_slowdown_writes_trigger=20;level0_stop_writes_trigger=30;max_write_buffer_number=4;block_based_table_factory={cache_index_and_filter_blocks=1;filter_policy=bloomfilter:10:false;whole_key_filtering=0};level_compaction_dynamic_level_bytes=true;optimize_filters_for_hits=true;memtable_prefix_bloom_size_ratio=0.05;prefix_extractor=capped:12;compaction_pri=kMinOverlappingRatio;compression=kLZ4Compression;bottommost_compression=kLZ4Compression;compression_opts=-14:4:0"
```

![ScreenShot](imgs/deshabilitarCompresio.png)

[OPCIONAL] Si volem cambiar a la compressio Zlib, anirem al fitxer `my.cnf`, i afegirem el seguent

```
rocksdb_default_cf_options="write_buffer_size=256m;target_file_size_base=32m;max_bytes_for_level_base=512m;max_write_buffer_number=4;level0_file_num_compaction_trigger=4;level0_slowdown_writes_trigger=20;level0_stop_writes_trigger=30;max_write_buffer_number=4;block_based_table_factory={cache_index_and_filter_blocks=1;filter_policy=bloomfilter:10:false;whole_key_filtering=0};level_compaction_dynamic_level_bytes=true;optimize_filters_for_hits=true;memtable_prefix_bloom_size_ratio=0.05;prefix_extractor=capped:12;compaction_pri=kMinOverlappingRatio;compression=kZlibCompression;bottommost_compression=kZlibCompression;compression_opts=-14:4:0"
```

I reiniciarem el servei de Percona

`systemctl restart mysqld`

![ScreenShot](imgs/reiniciarPercona.png)

I per comprovar que aixo ha funcionat, tornarem a mirar el tipus de compressio estem utilitzant

`SELECT * FROM information_schema.rocksdb_cf_options WHERE option_type LIKE '%ompression%' AND cf_name='default';`

![ScreenShot](imgs/compressioCambiada.png)

## INNODB

### Desactivar l'opció que ve per defecte de innodb_file_per_table

Anirem al my.cnf i afegirem el seguent:

`innodb_file_per_table="OFF"`

![ScreenShot](imgs/FilePerTableInno.png)

Guardem els canvis i reiniciarem el servei de mysql

`systemctl restart mysqld`

![ScreenShot](imgs/reiniciarMysql.png)

Entrarem al Mysql i executarem el seguent, per verificar que el que hem fet funciona:

`SHOW VARIABLES LIKE '%file_per_table%';`

![ScreenShot](imgs/VerificarFilePerTable.png)

### Permisos directori /datadir

Per veure el PATH del datadir anirem a l'arxiu /etc/my.cnf, i buscarem el parametre "datadir"

![ScreenShot](imgs/mydatadir.png)

Per veure els permisos del directori datadir executarem el seguent

`ls <PATH>/.. -asil | grep mysql`

![ScreenShot](imgs/datadir.png)
