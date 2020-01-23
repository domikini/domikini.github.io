In order to use terminal to input pin for gpg-agent. Add the following to the .gpg-agent.conf file.

      1 enable-ssh-support
      2 default-cache-ttl 60
      3 max-cache-ttl 120
      4 pinentry-program /usr/bin/pinentry-curses
      
Restart gpg-agent with the command: `gpg-connect-agent updatestartuptty /bye`
