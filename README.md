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

## WiFi ON/OFF

Turn WiFi off:
Desactiva el WiFi a nivel de NetworkManager (alto nivel). Es equivalente a apagar el WiFi desde la interfaz gráfica.
```bash
sudo nmcli radio wifi off
```
Desactiva la interfaz a nivel kernel (bajo nivel).
NetworkManager pierde control sobre el dispositivo y puede generar estados inconsistentes.
> Nivel kernel: `sudo ip link set wlan0 down`

Turn WiFi on:
```bash
sudo nmcli radio wifi on
```
> Nivel kernel: `sudo ip link set wlan0 up`

## Scan WiFi Networks

List available networks (SSID, signal strength, security):
```bash
nmcli device wifi list
```

Show device status:
```bash
nmcli device status
```

## WiFi Connection

With password:
```bash
sudo nmcli device wifi connect "SSID" password "WIFI_PASSWORD"
```
> Esto crea una conexión persistente con `autoconnect=yes` (se reconectará automáticamente en futuros inicios)

Temporary connection (recommended for hotspots):
```bash
nmcli device wifi connect "SSID" password "WIFI_PASSWORD" --temporary
```

With password and no autoconection cada vez que inicia el ordenador o solo lo pongo como no autonecction ns :
```bash
nmcli device wifi connect "SSID" password "WIFI_PASSWORD" \  connection.autoconnect no
```

Disable autoconnect (alternative):
```bash
nmcli device wifi connect "SSID" password "WIFI_PASSWORD" connection.autoconnect no
```

Open network:
```bash
sudo nmcli device wifi connect "SSID"
```

## WiFi Disconnection

Disconnect current device:
```bash
sudo nmcli device disconnect wlp4s0
```
> Corta la conexión activa del dispositivo WiFi. Si la red WiFi se había configardo con `autoconnect=yes`, al reiniciar se reconectará automáticamente.

Disconnect a specific connection.
```bash
sudo nmcli connection down id "SSID"
```
> Desconecta solo esa red, sin apagar el WiFi. 

Delete saved connection:
```bash
nmcli connection delete "SSID"
```
> Elimina el perfil guardado (evita reconexiones automáticas y problemas futuros).

# HTTP / Web Access

Get HTML page:
curl http://192.168.0.1/index.html
> To extract the JSON content, change the extension from `.html` to `.json`
> `--http0.9`: Force HTTP/0.9 if the server only responds using that old protocol.
> `--interface wlp4s0`: Use the WiFi interface explicitly (can fix issues when the request fails or behaves differently over another interface).

To extract/download content shared on a web page:

1. Display the page content to find the file name or identifier (it may be JSON, JS, HTML, etc.):
```bash
curl --interface wlp4s0 http://192.168.0.1/data.json
```

2. Once the file name is identified, download it:

Image:
```bash
curl --interface wlp4s0 http://192.168.0.1/img/FILE_NAME.jpg -o image.jpg
```

Video:
```bash
curl --interface wlp4s0 http://192.168.0.1/img/FILE_NAME.mp4 -o video.mp4
```




 






