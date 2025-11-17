sudo nano /etc/netplan/50-cloud-init.yaml

-> It shows something like the below, update the wifi details here, "auth" and "key-management: psk", with username and password. Only then , new wifi is registered

network:
  version: 2
  renderer: networkd
  wifis:
    wlp3s0:
      dhcp4: true
      access-points:
        "SpectrumWiFi5G":
             auth:
                key-management: "psk"
                password: "MySuperSecretPW123"

ctrl + o , ctrl + x -> to save and exit.

-> Once saved, run "sudo netplan apply" to apply the new file changes.
-> run "hostname -I" to see if the new details are updated or not.

