# Networking Cheatsheet
Basic networking commands and practical use cases with nmcli and curl, including downloading shared resources from local devices.

* NetworkManager (`nmcli`): **High-level** tool used to manage network connections. It interacts with NetworkManager and can be used via CLI or GUI equivalents.

* `ip link`: **Low-level** kernel tool used to enable/disable network interfaces directly. It bypasses NetworkManager and directly changes the interface state at the kernel level.

# Ethernet

**Disconnect** Ethernet:
```bash
sudo nmcli device disconnect enp3s0
```
> Kernel level: `sudo ip link set enp3s0 down`


**Reconnect** Ethernet:
```bash
sudo nmcli device connect enp3s0
```
> Kernel level: `sudo ip link set enp3s0 up`

# WiFi Management

## WiFi ON/OFF

Turn WiFi **OFF**:
```bash
sudo nmcli radio wifi off
```
> Kernel level: `sudo ip link set wlp4s0 down`

Turn WiFi **ON**:
```bash
sudo nmcli radio wifi on
```
> Kernel level: `sudo ip link set wlp4s0 up`

## Scan WiFi Networks

List **available networks** (SSID, signal strength, security):
```bash
nmcli device wifi list
```

Show device **status**:
```bash
nmcli device status
```

## WiFi Connection

With password (**autoconnect enabled**):
```bash
sudo nmcli device wifi connect "SSID" password "WIFI_PASSWORD"
```
> This creates a persistent connection with `autoconnect=yes` (it will automatically reconnect on future startups)

**Temporary** connection (useful for public or temporary WiFi networks such as hotspots)
```bash
nmcli device wifi connect "SSID" password "WIFI_PASSWORD" --temporary
```
> Creates a non-persistent connection that is not saved after disconnection.

With password (**autoconnect disabled**):
```bash
nmcli device wifi connect "SSID" password "WIFI_PASSWORD" connection.autoconnect no
```

**Open network**:
```bash
sudo nmcli device wifi connect "SSID"
```

## WiFi Disconnection

**Disconnect** current device:
```bash
sudo nmcli device disconnect wlp4s0
```
> Terminates the active connection on the WiFi interface. If the network was configured with `autoconnect=yes`, it will automatically try to reconnect on the next system startup.

Disconnect a **specific connection**:
```bash
sudo nmcli connection down id "SSID"
```
> Deactivates only that specific connection profile without disabling WiFi. 

**Delete** a saved connection:
```bash
nmcli connection delete "SSID"
```
> Removes the saved connection profile, preventing automatic reconnection and avoiding potential conflicts.

# HTTP / Web Access

Get HTML page:
```bash
curl http://192.168.0.1/index.html
```
> To retrieve JSON data (if available), replace `.html` with `.json`.
> 
> `--http0.9`: Force HTTP/0.9 if the server only supports this legacy protocol.
> 
> `--interface <interface>`: Forces curl to use a specific network interface (**useful when multiple interfaces are present or routing issues occur**).

To extract/download content shared on a web page:

1. Inspect the page content to identify file names or resource identifiers (e.g., JSON, JS, HTML, etc.):
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




 






