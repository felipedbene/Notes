public:: true

- control1-192.168.2.63
  id:: 671efed0-d3b3-4ef1-a165-9922d406dae7
- worker1-192.168.2.221
- control2-192.168.2.24
- worker2-192.168.2.193
- control3-192.168.2.74
- worker3-192.168.2.158
- ubuntu-lb-192.168.2.26
-
- Arrumar ubuntu #clone
- ```
  sudo su
  chmod +w /etc/machine-id
  echo -n > /etc/machine-id
  rm /var/lib/dbus/machine-id
  chmod -w /etc/machine-id
  reboot
  ```