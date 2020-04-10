1. Download "docker-credential-pass". https://github.com/docker/docker-credential-helpers/releases/

2. Unpack tar -xvf docker-credential-pass-v0.6.3-amd64.tar.gz

3. Copy the unpacked file to /usr/bin directory.

4. Set execution permission `chmod +x /usr/bin/docker-credential-pass` 

5. Check that docker-credential-pass work. To do this, run command docker-credential-pass. You should see: "Usage: docker-credential-pass <store|get|erase|list|version>".

6. Install gpg and pass. `sudo yum install gpg pass`

7. gpg --generate-key. Enter your name, mail, etc. You will get gpg-id like "5BB54DF1XXXXXXXXF87XXXXXXXXXXXXXX945A". Copy it to clipboard. To show GPG key id: `gpg --list-secret-keys --keyid-format LONG`

    If entropy is needed. Install rng-tools in Ubuntu or rng-utils in Fedora.
    
    Edit /etc/default/rng-tools and add the line HRNGDEVICE=/dev/urandom.
    
    Now, start the rng-tools daemon: /etc/init.d/rng-tools start

8. `pass init (paste from clipboard)`

9. `pass insert docker-credential-helpers/docker-pass-initialized-check` and set the next password "pass is initialized" (without quotes).

10. `pass show docker-credential-helpers/docker-pass-initialized-check`. You should see pass is initialized.

11. `docker-credential-pass list`. You should see {} or another data. You shouldn`t see error like "pass store is uninitialized".

12. `vim ~/.docker/config.json`. Set in root node the next line "credsStore": "pass" save ctrl+o.

13. after docker login and etc.

14. export GPG_TTY=$(tty)
