# Manejo de Contraseñas


Utilizamos [Teampass](https://github.com/nilsteampassnet/TeamPass) para el manejo de contraseñas a nivel de empresa. Como colaborador, si necesita acceder a este sistema, solicite acceso a los encargados.

Como parte de los accesos que se le brindarían están la configuración del VPN y el usuario y contraseña inicial en la herramienta.

El tipo de vpn en uso es OpenVPN, por ello, se ocupa un software para utilizarlo. En el caso de Linux, este es `openvpn` y se encuentra en los repositorios de la mayoría de distros. En el caso de MAC, puede usarse Tunnelblick, Viscosity o algún otro. Una vez instalado el software, procedemos a conectarnos al VPN. A continuación las instrucciones para cada sistema operativo.

## Linux

Al final del archivo .ovpn provisto, agregar las siguientes líneas:
```
script-security 2
up /etc/openvpn/update-resolv-conf
down /etc/openvpn/update-resolv-conf
```
Lo anterior, para asegurarse que el sistema utilice los nuevos ajustes de DNS provistos por el VPN.

Luego, desde terminal, `sudo openvpn archivo.ovpn` y listo.

## Mac
Estas instrucciones dependen del software en uso, pero lo que hay que hacer es darle a dicho software el archivo de configuración (con extensión ovpn) provisto.


Una vez conectado al vpn, podrá acceder a http://teampass.manati.vpn e iniciar sesión con las credenciales iniciales que se le dieron. A continuación se le solicitará que cambie su contraseña para poder seguir utilizando el sistema. 