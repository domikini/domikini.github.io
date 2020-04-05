## Reference: https://www.snbforums.com/threads/letsencrypt-cert-stopped-updating.62715/

Connect to your router via ssh, delete your /jffs/.le folder and execute service restart_letsencrypt command. Allow a minute for the task to complete.

Looks like the same problem described in this thread: Lets Encrypt not updating, or? (https://www.snbforums.com/threads/lets-encrypt-not-updating-or.59524/)
