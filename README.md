# 📶 Pwnagotchi Smart Hotspot WiFi Switch  

This script **automatically switches between internal and external WiFi adapters** on your **Pwnagotchi**, depending on whether a USB WiFi adapter is plugged in, and **creates a hotspot on the onboard adapter**.

It runs in the background and detects WiFi adapter changes in real time, restarting the Pwnagotchi service with the correct WiFi interface. You are then able to connect to the hotspot with your device of choice and **control the pwnagotchi remotely without bluetooth**.

## 🔧 Features  
✅ **Hot-swappable** – Detects WiFi adapter changes on the go  
✅ **Automatic switching** – Enables/disables internal WiFi hotspot as needed  
✅ **Service-based** – Runs at boot for seamless operation  
✅ **No reboots needed** – Keeps your Pwnagotchi running smoothly  

## 🛠️ Installation  

1. **Install dependencies**
   ```bash
   sudo apt install hostapd
   sudo curl -o /usr/bin/lnxrouter https://raw.githubusercontent.com/garywill/linux-router/master/lnxrouter
   ```

1. **Download and copy the script** (`wifi_hotspot.sh`) to `/` and make it executable:  
   ```bash
   sudo curl -o /wifi_hotspot.sh https://raw.githubusercontent.com/merll002/pwnagotchi-auto-antenna/refs/heads/patch-1/wifi_hotspot.sh
   sudo chmod +x /wifi_hotspot.sh
   ```

2. **Create the service file** at `/etc/systemd/system/wifi_hotspot.sh.service`:  

   ```ini
   [Unit]
   Description=Wifi Switch Script
   After=network.target
   Before=pwnagotchi.service
   
   [Service]
   Type=simple
   ExecStart=/bin/bash /wifi_hotspot.sh
   Restart=always
   RestartSec=5
   User=root
   Group=root

   [Install]
   WantedBy=multi-user.target
   ```

3. **Enable and start the service**:  
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable wifi_hotspot.service
   sudo systemctl start wifi_hotspot.service
   ```

4. **Check if the service is running properly**:  
   ```bash
   sudo systemctl status wifi_hotspot.service
   ```

## 📝 Notes  
- You can modify `RestartSec=5` to change the polling interval for adapter detection.  

Now your Pwnagotchi will **automatically switch WiFi adapters without requiring a reboot!** 🚀
