# Mock Server for Sonoff OTA

## Introduction
This guide will help you fix OTA error by setting up a local Apache server using XAMPP or any other tool to spoof the address for Sonoff OTA updates. By utilizing OpenWRT, Pihole, or Adguard, you can redirect requests from the official server to your local server.

For more details, check the [GitHub issue](https://github.com/itead/Sonoff_Devices_DIY_Tools/issues/45).

---


## Setting Up Apache Server with XAMPP

1. Install XAMPP
2. Clone the Repository
3. Move Files to htdocs Folder
4. Start Apache Server
5. Open your web browser and visit [http://127.0.0.1/](http://127.0.0.1/) to confirm that the Apache server is up and running.

---

## Setting Up Spoofing on OpenWRT

Navigate to your OpenWRT interface:

1. Go to **Network**.
2. Select **DHCP and DNS**.
3. Under **Addresses**, add a new entry: `/apid.coolkit.cn/192.168.1.101`.
   (Replace `192.168.1.101` with the IP address of your local server)

- IP: `52.57.118.192` (Optional)
- Domain: `ec2-52-57-118-192.eu-central-1.compute.amazonaws.com`(Optional)
Ensure Spoofing is Working:

1. Confirm the spoofing is effective by opening your browser.
2. Visit http://apid.coolkit.cn/v2/d/otaflash.
3. Check if you receive a 422 error as a response.

If the spoofing is successful and you get the expected 422 error, you can proceed with the next steps.

---

## Setting Up Spoofing on AdGuard

1. Install and Start AdGuard Docker

2. Go to the **Filter** section and then navigate to **DNS Rewrites**.

3. Add the following domain:
   - Domain: `apid.coolkit.cn`

4. Set the IP address to your mock server, where you have hosted Docker or Apache server.

   ### Example:
   - Domain: `apid.coolkit.cn`
   - IP Address: `<your_mock_server_ip>`

5. Save and Enable Protection

6. **Configure your AdGuard machine IP as the DNS address in your router.**

---

## OTA Flash Scripts

There are two OTA scripts available:

1. [OTA Script](https://github.com/njh/sonoff-ota-flash-cli)
2. [OTA Script V2](https://github.com/IBims1NicerTobi/sonoff-ota-flash-cli-devid-fix)

---

## Flashing Sonoff Devices

To flash Sonoff devices using the OTA script, follow these steps:

1. **Prepare the Device:**
   - Put the device into compatible pairing mode.
   - Press and hold the onboard button for 5 seconds, release, and repeat for an additional 5 seconds.
   - Connect to the new AP "ITEAD-XXX" with the password `12345678`.
   - Open http://10.10.7.1/ to configure WiFi credentials.

2. **Run the OTA Script:**
   - Clone the respective OTA script repository.
   - Execute the script using the following command:

```bash
./sonoff-ota-flash.sh -i 192.168.1.111
```

Alternatively, you can run it directly without manual input

```bash
curl -O https://raw.githubusercontent.com/mahipat99/sonoff-ota/main/sonoff-ota-flash-unlock.sh && chmod +x sonoff-ota-flash-unlock.sh && ./sonoff-ota-flash-unlock.sh -i 192.168.1.87 
```

If you have unlocked it through any other method, you can use this command, which skips the OTA unlock process

```bash
curl -O https://raw.githubusercontent.com/mahipat99/sonoff-ota/main/sonoff-ota-flash.sh && chmod +x sonoff-ota-flash.sh && ./sonoff-ota-flash.sh -i 192.168.1.87 
```
Ensure you replace `192.168.1.111` with the actual IP address of your Sonoff device.

Feel free to refer to the script repositories for more detailed instructions.
