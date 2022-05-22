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
