# INSTALACIÓ MYSQL PERCONA

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

Per comprovar que el mysql esta fucionant farem el seguent:

`systemctl status mysql`

I ens a d'apareixer el missatge "active (running)" com a la imatge

![ScreenShot](imgs/systemctl_mysql.png)

A continuació hem de mirar quina es la contraseña que s'ha generat de forma aleatoria per l'usuari Root:

`cat /var/log/mysqld.log |grep generated`

![ScreenShot](imgs/veure_contra.png)

Ara entrarem al mysql amb l'usuari Root i la contrasenya que ens ha generat el percona:

`mysql -u root -p`

![ScreenShot](imgs/login_root.png)


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
