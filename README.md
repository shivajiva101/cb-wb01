# cb-wb01
Creality Box WB01 OpenWrt firmware and Octoprint installation. This project is based on the work of figgyc and Ihrapsa and is just a placeholder until OpenWrt is in active development/maintenance again.

**Specifications:**

 From https://github.com/figgyc/openwrt/tree/wb01

- **SoC**: MediaTek MT7688AN @ 580 MHz  
- **Flash**: BoyaMicro BY25Q128AS (16 MiB, SPI NOR)  
- **RAM**: 128 MiB DDR2 (Winbond W971GG6SB-25)  
- **Peripheral**: Genesys Logic GL850G 2 port USB 2.0 hub  
- **I/O**: 1x 10/100 Ethernet port, microSD SD-XC Class 10 slot, 4x LEDs, 2x USB 2.0 ports, micro USB input (for power only), reset button  
- **FCC ID**: 2AXH6CREALITY-BOX  
- **UART**: test pads: (square on silkscreen) 3V3, TX, RX, GND; default baudrate: 57600 (https://hackaday.com/wp-content/uploads/2020/12/wifibox_serialport.png?resize=250)

## Firmware Instructions

<details>
  <summary>Expand steps!</summary>
  
  #### Flash Openwrt using the following guide
  
  ### If you're box is currently on stock firmware:
  
  Options:  
  **A. Standard option**
  
  1. Use `cxsw_update.tar.bz2`  
  2. Copy it to the root of a FAT32 formatted microSD card.  
  3. Turn on the device, wait for it to start, then insert the card. The stock firmware reads the `install.sh` script from this archive and flashes the new OpenWrt image.  
  4. Connct to the AP `OpenWrt` -> `ssh` and continue Wi-fi internet setup.
  
  **B. Through the Stock firmware UI interface (link)**
  
  **C. Using the `Recovery process`** see below  
  
  ### If your box is already on OpenWrt and has the luci web UI reachable:
  
  Options:  
  **A. Flashing another Openwrt binary:** Access the luci web UI -> Go to System -> Upgrade -> Uncheck the box that says `Save configs` -> Upload the SYSUPGRADE bin -> Flash  
  **B. Resetting the box** By holding the reset button for about 6 seconds the box will freshly reset the current OpenWrt firmware.  
  **C. Using the `Recovery process`** see below  
  
  
  ## Recovery process  
  If the box is either on stock or Openwrt but unreachable (semi bricked)❗With the recovery process you can restore stock firmware or install/recover Openwrt firmware regardless of what's already on the box.
  
  **Recovering to Openwrt**  
  1. Rename the SYSUPGRADE bin to `root_uImage`  
  2. Put it on a fat32 formatted USB stick (not uSD card)  
  3. With the box powered off, plug the USB stick in the box  
  4. Press and hold the reset button.  
  5. While holding the reset button power on the box and keep it pressed for about 6-10sec  
  6. Green LED should start flashing while the box installs the firmware  
  7. Wait a few minutes until you see it on the network (`OctoWrt` WiFi AP )  
  
  **Restoring to Stock**  
  1. Download and extract the `root_uImage` file from the `cxsw_update.tar.bz2` ([link](https://www.crealitycloud.com/software-firmware/box/creality-box)) 
  2. Put it on a fat32 formatted USB stick (not uSD card)  
  3. With the box powered off, plug the USB stick in the box  
  4. Press and hold the reset button.  
  5. While holding the reset button power on the box and keep it pressed for about 6-10sec  
  6. Green LED should start flashing while the box installs the firmware  
  7. Wait a few minutes until you see the Creality AP on the network
     
  Once flashed setup internet access on the box (either Wi-Fi client or wired connection)
  </details> 
</details>

------------------
## Scripted Install:

<details>
  <summary>Expand steps!</summary>
   <details>
    <summary>Expand Internet setup!</summary>
   
  - Make sure you've flahsed/sysupgraded latest `.bin` file from latest release.
  - Connect to the `OpenWrt` access point
  - Access LuCi web interface and log in on by visiting `http://192.168.1.1:81`
  - _(**optional** but recommended)_ Secure wifi access by adding a password to the `OctoWrt` access point: `Wireless` -> Under wireless overview `EDIT` the `OpenWrt` interface -> `Wireless Security` -> Choose an encryption -> set a password -> `Save` -> `Save & Apply`
  - _(**optional** but recommended)_ Add a password: `System` -> `Administration` -> `Router Password`
  - ❗If your home network subnet is on 1 (192.168.1.x), in order to avoid any ip conflicts, change the static ip of the box LAN from 192.168.1.1 to something like 192.168.3.1. To do that access the luci webinterface -> `Network` -> `Interfaces` and edit the static ip -> `Save` -> press the down arow on the Save&Apply button -> `Apply Unchecked`. You can now access the luci web interface on the new ip and continue configuring Client setup. 
  - Connect as a client to your Internet router: `Network` -> `Wireless` -> `SCAN` -> `Join Network` -> check `Lock to BSSID` -> `Save` -> `Dropdown arrow -> Apply Unchecked` -> `Apply Unchecked`
  - Connect back to your main internet router and find the new box's ip inside the `DHCP` list.
  - ❗  Access the terminal tab (`Services` -> `Terminal`) ❗ If terminal tab is not working go to `Config` tab and change `Interface` to the interface you are connecting through the box (your wireless router SSID for example) -> `Save & Apply`.
  - Proceed with step 2

  </details>
  
  #### 1. Execute format script:
  ```
  cd /tmp
  wget https://github.com/shivajiva101/cb-wb01/raw/main/scripts/1_format.sh
  chmod +x 1_format.sh
  ./1_format.sh
  ```
  #### 2. Execute install script:
  ```
  cd /tmp
  wget https://github.com/shivajiva101/cb-wb01/raw/main/scripts/2_install.sh
  chmod +x 2_install.sh
  ./2_install.sh
  ```
    
  #### 3. Access Octoprint UI on port 5000
  
  ```
  http://creality-box-ip:5000
  ```
  
  When prompted use the following **server commands**:

    - Restart OctoPrint : `/etc/init.d/octoprint restart`  
    - Restart system : `reboot`  
    - Shutdown system : `poweroff`  

  For **webcam** support:  
  
  `/etc/config/mjpg-streamer` is the configuration file. Modify that to change resolution, fps, user, pass etc.  
  
  Inside OctoPrint snapshot and stream fields add the following:  
  - Stream URL: `http://your-box-ip:8080/?action=stream`  
  - Snapshot URL: `http://your-box-ip:8080/?action=snapshot` 
  - ffmpeg binary path: `/usr/bin/ffmpeg`
  
  
</details>
 
  
## Manual Install:

<details>
  <summary>Expand steps!</summary>

## ⤵️ Preparing:

<details>
  <summary>Expand steps!</summary>
  
* **OpenWrt**: Make sure you have OpenWrt firmware flashed. Follow the guide above -> Once flashed setup Wi-Fi client or wired connection for internet access on the box

* **Format**: execute [this](https://github.com/shivajiva101/cb-wb01/raw/main/scripts/1_format.sh) script. Make sure you have a microsd plugged in!
  
  ```
  cd ~
  wget https://github.com/shivajiva101/cb-wb01/raw/main/scripts/1_format.sh
  chmod +x 1_format.sh
  ./1_format.sh
  ```
   Then reboot when instructed!
 
* **Swap**: 

  ```
  opkg update && opkg install swap-utils zram-swap
  ```
  ```
  dd if=/dev/zero of=/overlay/swap.page bs=1M count=512;
  mkswap /overlay/swap.page;
  swapon /overlay/swap.page;
  mount -o remount,size=256M /tmp;
  ```
  ```
  rm /etc/rc.local;
  cat << "EOF" > /etc/rc.local
  # Put your custom commands here that should be executed once
  # the system init finished. By default this file does nothing.
  ###activate the swap file on the SD card  
  swapon /overlay/swap.page  
  ###expand /tmp space  
  mount -o remount,size=256M /tmp
  exit 0
  EOF
  ```
  
</details>

## ⤵️ Installing:

<details>
  <summary>Expand steps!</summary>
  
#### 1. Update the package feeds

```
rm /etc/opkg/distfeeds.conf;
wget https://github.com/shivajiva101/cb-wb01/raw/main/config/distfeeds.conf -P /etc/opkg
```
  
#### 2. Install OpenWrt dependencies:

```
opkg update
opkg install gcc make unzip htop wget-ssl git-http
opkg install v4l-utils mjpg-streamer-input-uvc mjpg-streamer-output-http mjpg-streamer-www ffmpeg
```
By default mjpg-streamer comes with username=openwrt and password=openwrt. If you don't want them do:

```
uci delete mjpg-streamer.core.username
uci delete mjpg-streamer.core.password
```

------------------------------

* **Python 3**:

⚠️ _It is recommended to use the python 3 approach since python 2 got deprecated since January 1st, 2020. However, if you want older versions of Octoprint, python 2 approach might be the only way._

Install python 3 packages
```
opkg install python3 python3-pip python3-dev python3-psutil python3-netifaces python3-pillow
pip install --upgrade setuptools
pip install --upgrade pip
pip install virtualenv
virtualenv venv
```
 
--------------------

#### 3. Install Octoprint:

```
git clone https://github.com/shivajiva101/OctoPrint.git src
cd src
../venv/bin/pip install .
```

#### 4. Create octoprint service:
  
  <details>
    <summary> Expand </summary>
  
  ```
  cat << "EOF" > /etc/init.d/octoprint
  #!/bin/sh /etc/rc.common
  # Copyright (C) 2009-2014 OpenWrt.org
  # Put this inside /etc/init.d/

  START=91
  STOP=10
  USE_PROCD=1


  start_service() {
      procd_open_instance
      procd_set_param command /root/venv/bin/octoprint serve --iknowwhatimdoing
      procd_set_param respawn
      procd_set_param stdout 1
      procd_set_param stderr 1
      procd_close_instance
  }
  EOF
  ```
  </details>
  
#### 5. Make it executable:

```
chmod +x /etc/init.d/octoprint
```
#### 6. Enable the service:

```
service octoprint enable
``` 

#### 7. Reboot and wait a while

```
reboot
```

▶️ _**Note!**_  
_Booting on the last versions takes a while (~5 minutes). Once booted however, everything works as expected. If you care that much about this you can install older versions (v1.0.0 for example) that are much lighter but are not plugin enabled. Only Temps, Control, Webcam and Gcode preview._
  
#### 8. First setup
  
<details>
  <summary> Expand steps </summary>
  
Access Octoprint UI on port 5000
  
```
http://creality-box-ip:5000
```
  
When prompted use thefollowing **server commands**:

  - Restart OctoPrint : `/etc/init.d/octoprint restart`  
  - Restart system : `reboot`  
  - Shutdown system : `poweroff`  

For **webcam** support:  
  
  `/etc/config/mjpg-streamer` is the configuration file. Modify that to change resolution, fps, user, pass etc.  
  Inside OctoPrint snapshot and stream fields add the following:
  - Stream URL: `http://your-box-ip:8080/?action=stream`  
  - Snapshot URL: `http://your-box-ip:8080/?action=snapshot` 
  
  If your webcam is not showing, unplug and replug it.  
  If you don't want webcam authentication you can comment or delete the user and password lines inside `mjpg-streamer` config file. Make sure to restart it after that:  `/etc/init.d/mjpg-streamer restart`
  
  </details>
  
  #### 9. Timelapse plugin setup
   
  
  <details>
    <summary> Expand steps </summary>
    
    In octoprint settings set the ffmpeg binary path as:
    
    ```
    /usr/bin/ffmpeg
    ```
    
   </details>
  
</details>

</details>

-------------------------

## 🔝 Credits:

<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488057-52b688f7-25d5-46e1-9ac8-bb5309384d98.png">  **[Irhapsa](https://github.com/ihrapsa)** for [OctoWrt](https://github.com/ihrapsa/OctoWrt) and [KlipperWrt](https://github.com/ihrapsa/KlipperWrt)   
<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488057-52b688f7-25d5-46e1-9ac8-bb5309384d98.png">  **George** a.k.a [figgyc](https://github.com/figgyc) for porting OpenWrt to this device   
<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488418-c703c383-1835-49a0-aa41-eadee0671ab7.png">  **Gina and co.** for creating and developing [OctoPrint](https://github.com/OctoPrint/OctoPrint)  
