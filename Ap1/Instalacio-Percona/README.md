# INSTALACIÓ MYSQL PERCONA

### INSTALACIÓ

Instalarem el repositori de percona amb la seguent comanda:

`yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm`

Al executar la comanda anterior ens explica el que instalara i ens preguntara si estem d'acord amb la instalacio, li indiquem que si amb: "s"

![ScreenShot](https://github.com/MarcGarcia03/Bases-de-dades/blob/main/Ap1/Instalacio-Percona/imgs/2022-03-04_18-57.png)


Habilitarem el repositori:

`percona-release setup ps80`

Ens tornara a pregurtar si estem segurs li indiquem que si aquest cop amb: "y" com es veu a la imatge

![ScreenShot](https://github.com/MarcGarcia03/Bases-de-dades/blob/main/Ap1/Instalacio-Percona/imgs/2022-03-04_19-18.png)

I finalment instalem el percona

`yum install percona-server-server`

Ens fara varies perguntes i li contestarem "s" en tots els casos

![ScreenShot](https://github.com/MarcGarcia03/Bases-de-dades/blob/main/Ap1/Instalacio-Percona/imgs/2022-03-04_19-20.png)


### POSTINSTALACIÓ
