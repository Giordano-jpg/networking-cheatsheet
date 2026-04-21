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
```bash
sudo nmcli radio wifi off
```
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

## WiFi Connection

With password:
```bash
sudo nmcli device wifi connect "switch_68B4E50100P" password "WIFI_PASSWORD"
```

* Open network:
```bash
sudo nmcli device wifi connect "switch_68B4E50100P"
```

## WiFi Disconnection

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




 






