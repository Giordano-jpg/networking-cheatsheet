# Networking Cheatsheet
Basic networking commands and practical use cases with nmcli and curl, including downloading shared resources from local devices.

# Ethernet

Disconnect Ethernet:
```bash
sudo nmcli device disconnect enp3s0
```
Con `nmcli` es lo mismo que abrir la interfaz gráfica de NetworkManager y pulsar “Desconectar” en Ethernet.
Con `ip link` se desactiva la interfaz a nivel del kernel; la tarjeta queda completamente deshabilitada
> Nivel kernel: `sudo ip link set enp3s0 down`


Reconnect Ethernet:
```bash
sudo nmcli device connect enp3s0
```
> Nivel kernel: `sudo ip link set enp3s0 up`

# WiFi Management/Control

Turn WiFi off:
```bash
sudo nmcli radio wifi off
```
> Nivel kernel: `sudo ip link set wlan0 down`

Turn WiFi on:
```bash
sudo nmcli radio wifi on
```
> Nivel kernel: `sudo ip link set wlan0 up`

Scan WiFi networks o Lista redes: SSID, señal, seguridad
```bash
nmcli device wifi list
```

* Connect to WiFi:
With password:
```bash
sudo nmcli device wifi connect "switch_68B4E50100P" password "WIFI_PASSWORD"
```

* Open network:
```bash
sudo nmcli device wifi connect "switch_68B4E50100P"
```

Disconnect WiFi

Disconnect current network (recommended):
```bash
sudo nmcli device disconnect wlp4s0
```

Disconnect saved connection:
```bash
sudo nmcli connection down id "switch_68B4E50100P"
```

# HTTP / Web Access

Get HTML page:
curl http://192.168.0.1/index.html
> Para extraer el contenido JSON cambiar la extension `.html` → `.json`
> `--http0.9`: Si el servidor solo responde con HTTP 0.9
> `--interface wlp4s0`: Especificar el uso de la interfaz WIFI 




 






