# UFW Firewall configuraiton commands examples
1. sudo ufw route allow in on eth0 from 192.168.8.0/24 out on wlan0 to 192.168.0.195 port 443
2. sudo ufw status numbered
3. sudo ufw delete 9
4. sudo ufw route allow in on eth0 out on wlan0 to any from any

# To drop ICMP ping on forwarding and signal in
Edit /etc/ufw/before.rules
Change ACCEPT to DROP on the sections "ok icmp code for INPUT" ok icmp code for FORWARD"