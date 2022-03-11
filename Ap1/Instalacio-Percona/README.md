# INSTALACIÓ PERCONA

## INSTALACIÓ

Instalarem el repositori de percona amb la seguent comanda:

`yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm`

Al executar la comanda anterior ens explica el que instalara i ens preguntara si estem d'acord amb la instalacio, li indiquem que si amb: "s"

![ScreenShot](imgs/percona.png)

Habilitarem el repositori:

`percona-release setup ps80`

Ens tornara a pregurtar si estem segurs li indiquem que si aquest cop amb: "y" com es veu a la imatge

![ScreenShot](imgs/habilitar_repositori_percona.png)

I finalment instalem el percona

`yum install percona-server-server`

Ens fara varies perguntes i li contestarem "s" en tots els casos

![ScreenShot](imgs/instalar_percona.png)

## POSTINSTALACIÓ

Per comprovar l'estat del mysql farem el seguent:

`systemctl status mysql`

I ens a d'apareixer el missatge "active (running)" com a la imatge

![ScreenShot](imgs/systemctl_mysql.png)

Realitzarem la securització del percona per fer mes segura la base de dades:

`mysql_secure_installation`

El primer cop que introduim el password nou ens dona un error,(perque el percona ja porta un component instalat), i ens fara una pregunta, li contestarem: y
A continuació introduirem la nova contrasenya per a root (2 cops):

![ScreenShot](imgs/percona_securitzacio.png)

Ara ens preguntara si volem utilitzar la contrasenya que acabem d'introduir, i si volem eliminar els usuaris anonims, es recomenable dir que si per millorar la seguretat de la base de dades

![ScreenShot](imgs/percona_securitzacio1.png)

A continuació ens pregunta si volem deshabilitar el login del root de forma remota, i si volem esborrar la base de dades de proves, es recomenable dir que si

![ScreenShot](imgs/percona_securitzacio2.png)

Finalment ens preguntara si volem recarregar els permisos de les taules, es recomenable dir que si

![ScreenShot](imgs/percona_securitzacio3.png)

Ara entrarem al mysql amb l'usuari Root i la contrasenya que hem canviat anteriorment:

`mysql -u root -p`

![ScreenShot](imgs/login_root.png)

Per finalitzar les proves, ens connectarem al Percona desde el Workbench, per fer-ho haurem de realitzar els seguents passos:

Afegirem una regla al firewall per habilitar la conexio desde fora:
![ScreenShot](imgs/regla_firewall.png)

### COMANDES PERCONA

Apagar el servei de Percona:
`systemctl stop mysql`

Iniciar el servei de Percona:
`systemctl start mysql`

Veure l'estat del servei de Percona:
`systemctl status mysql`

### FITXER DE CONFIGURACIÓ

El fitxer de configuració de Percona es:
`/etc/my.cnf`
