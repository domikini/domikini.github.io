## Reference: https://www.snbforums.com/threads/asus-letsencrypt-workaround-if-yours-is-broken.61965/

For those of you having issues renewing certs using the built-in Let's Encrypt functionality (because MOST ISP's block Port 80, therefore you will not be able to create or renew your SSL cert via the built-in ASUSWRT feature for Let's Encrypt), there is a basic/simple workaround that does not involve the use of TLS -APLN-01 challenge.

In your router's System Log, when you attempt to create or renew a Let's Encrypt certificate, blocked port 80 will cause the following error message to show up:
Feb 20 08:59:42 kernel: <your-domain-name>:Verify error:Fetching http://<your-domain-name>/.well-known/acme-challenge/<random-characters>: Connection refused
Feb 20 08:59:42 kernel: [Thu Feb 20 08:59:42 EST 2020]

This error will prevent renewal of an existing Let's Encrypt SSL cert, or the creation of a new Let's Encrypt SSL cert.

There are a couple of steps for this workaround, and those steps may not be suitable for everyone. If you're using a DDNS provider such as No-ip, it may not be free either (even though Letsencrypt is free). No-ip charges $25 annually for their "enhanced services"...if you upgrade to use their enhanced services, you'll be able to create and edit TXT records for any of their domains (including the dynamic domains) using their web UI. There may be DDNS providers that allow their users to edit TXT records for dynamic domains, but I'm not sure of any that do so without charging some sort of fee, off the top of my head.

Basically, in order to do this workaround, you need to be able to manage and/or update a TXT record for your domain name. Any of you that own domains and manage them through a domain name service provider should be able to do this. The 2nd (and final) piece of the puzzle is to have access to something running Linux (with Internet access) where you can install certbot and run ONE command. Installing certbot is very easy if you have a Linux system available (Raspberry Pi, running Raspbian OS for example). Your ASUS Router is running a Linux variant, so you may be able to install certbot and use it on your ASUS router rather than needing another system...you will need to be able to get your cert files however when the process is done.

Assuming you can do those TWO (2) things, here is the workaround:

Using certbot in your Linux environment, run the following command:
certbot -d <your-domain-name-goes-here> --manual --preferred-challenges dns certonly

This command will ensure that certbot only runs the DNS-01 challenge to verify your domain, before it can issue a SSL cert. This bypasses the port 80 HTTP-01 challenge. You will be prompted a couple of times - once to confirm it's ok to log your IP (type Y and hit return/enter), and a second prompt that will instruct you to copy/paste a randomly-generated string to copy and paste into a TXT record for your domain name. At this point you will copy and paste that text into the TXT record for your domain through your DNS host/provider, ensure that it's applied for your DNS, then hit enter within certbot. Certbot will generate a bunch of key files (".pem" extension) which will be stored in /etc/letsencrypt/live/<your-domain-name> on your Linux system.

You'll need TWO (2) of the files it generates - (1) Your private key named privatekey.pem and (2) your certificate file named fullchain.pem. These cert files must be manually imported into your ASUS router.

Within ASUSWRT, you will need to manually import BOTH of those files - this is found in your WAN > DDNS settings. Select Import Your Own Certificate, Choose to UPLOAD, and you'll be prompted for both cert files. Click the Apply button and voila, you're done. The router admin page should load in virtually any browser from any location without throwing security errors when you visit via the https://<your-domain>:8443 link.

Let's Encrypt certs expire every 3 months, so you'll need to renew your cert via Certbot before it expires, then manually import into your ASUS router. Until ASUS incorporates something to get around the port 80 issues, or Let's Encrypt changes their SSL cert generation policies, the built-in Let's Encrypt feature will not work properly for anyone with an ISP prohibiting port 80 inbound.

Of course, there are other ways to go about obtaining your own SSL cert files without going through any of the steps above, but it'll likely cost $$$. The least expensive method of obtaining a SSL certificate is to first (1) own your own domain name, then (2) obtain your SSL certificate from Let's Encrypt (they expire every 90 days but are free). The other method (more complex, but likely no cost) is to install another ACME client (on a Linux system) that can fetch certs from Let's Encrypt via TLS-APLN-01 challenge, which would not require the use of port 80. There's one called Lego on Github that supports TLS-APLN-01.

** You could also redirect port 80 to another port (such as 8443, your router's WAN UI) via your DNS provider and skip pretty much all the steps mentioned above, and use the built-in Let's Encrypt feature of your ASUS router.
