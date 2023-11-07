# cb-wb01
Creality Box WB01 OpenWrt firmware and Octoprint installation.

------------------
## Automatic Installation:

<details>
  <summary>Expand steps!</summary>

  #### 1. Flash Openwrt using the following guide
  
  ### If you're box is currently on stock firmware:
  
  Alternative Options:  
  **A. Standard option**
  
  1. Rename `*factory.bin` to `cxsw_update.tar.bz2`  
  2. Copy it to the root of a FAT32 formatted microSD card.  
  3. Turn on the device, wait for it to start, then insert the card. The stock firmware reads the `install.sh` script from this archive and flashes the new OpenWrt image.  
  4. Connct to the AP `OpenWrt` -> `ssh` and continue Wi-fi internet setup.
  
  **B. Through the Stock firmware UI interface (link)**
  
  **C. Using the `Recovery process`** see below  
  
  ### If your box is already on OpenWrt and has the luci web UI reachable:
  
  Alternative Options:  
  **A. Flashing another Openwrt binary:** Access the luci web UI -> Go to System -> Upgrade -> Uncheck the box that says `Save configs` -> Upload the SYSUPGRADE bin -> Flash  
  **B. Resetting the box** By holding the reset button for about 6 seconds the box will freshly reset the current OpenWrt firmware.  
  **C. Using the `Recovery process`** see below  
  
  
  ## Recovery process  
  If the box is either on stock or Openwrt but unreachable (semi bricked)❗With the recovery process you can restore stock firmware or install/recover Openwrt firmware regardless of what's already on the box.
  
  **Recovering to Openwrt**  
  1. Rename the SYSUPGRADE bin to `root_uImage`  
  2. Put it on a fat32 formatted USB stick (not uSD card)  
  3. With the box powerd off, plug the USB stick in the box  
  4. Press and hold the reset button.  
  5. While holding the reset button power on the box and keep it pressed for about 6-10sec  
  6. Green Led should start flashing while the box installs the firmware  
  7. Wait a few minutes until you see it on the network (`OctoWrt` WiFi AP )  
  
  **Restoring to Stock**  
  1. Extract the `root_uImage` file from the `cxsw_update.tar.bz2`   
  2 - 6. Same steps as above  
  7. You should see the creality AP
     
    Once flashed setup internet access on the box (either Wi-Fi client or wired connection)
    
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
  
  #### 2. Execute format script:
  ```
  cd /tmp
  wget https://github.com/shivajiva101/cb-wb01/raw/main/scripts/1_format.sh
  chmod +x 1_format.sh
  ./1_format.sh
  ```
  #### 3. Execute install script:
  ```
  cd /tmp
  wget https://github.com/shivajiva101/cb-wb01/raw/main/scripts/2_install.sh
  chmod +x 2_install.sh
  ./2_install.sh
  ```
    
  #### 4. Access Octoprint UI on port 5000
  
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
 


## 🔝 Credits:

<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488057-52b688f7-25d5-46e1-9ac8-bb5309384d98.png">  **[Irhapsa](https://github.com/ihrapsa)** for [OctoWrt](https://github.com/ihrapsa/OctoWrt) and [KlipperWrt](https://github.com/ihrapsa/KlipperWrt)   
<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488057-52b688f7-25d5-46e1-9ac8-bb5309384d98.png">  **George** a.k.a [figgyc](https://github.com/figgyc) for porting OpenWrt to this device   
<img width=20 align=center src="https://user-images.githubusercontent.com/40600040/128488418-c703c383-1835-49a0-aa41-eadee0671ab7.png">  **Gina and co.** for creating and developing [OctoPrint](https://github.com/OctoPrint/OctoPrint)  
