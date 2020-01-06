https://wiki.archlinux.org/index.php/GnuPG

# Introduction
GPG agent can be setup to be used as SSH agent. Current SSH keys can be imported and used by the GPG agent. The SSH key files is stored at ~/.gnupg/ folder in Linux systems or in C:\Users\<UserName>\AppData\Roaming\gnupg in Windows.

On Windows you can check the store location with PowerShell:
`echo $env:AppData\gnupg`

Once the keys are stored with GPG agent it can be removed from the default location.

Once the gpg agent is setup, save current SSH keys by typing `ssh-add`. The GPG agent will look for existing keys in ~/.ssh folder.

## To setup GPG agent: 
1. Install GnuPG

On Fedora/RHEL:
`sudo yum install -y gnupg2 pinentry-curses pcsc-lite pcsc-lite-libs gnupg2-smime`

On Windows:
Download and install from https://www.gpg4win.org/download.html

I prefer using MobaXterm on Windows.

On MacOS:
Download and install [Homebrew](https://brew.sh/) and install the following packages:

`brew install gnupg yubikey-personalization hopenpgp-tools ykman pinentry-mac`


## Configure GPG agent
### On Linux and MacOS systems:

To launch gpg-agent for use by SSH, use the `gpg-connect-agent /bye` or `gpgconf --launch gpg-agent` commands.

Add these to the shell rc file:

    export GPG_TTY="$(tty)"
     export SSH_AUTH_SOCK="/run/user/$UID/gnupg/S.gpg-agent.ssh"
    gpg-connect-agent updatestartuptty /bye > /dev/null

On modern systems, gpgconf --list-dirs agent-ssh-socket will automatically set SSH_AUTH_SOCK to the correct value and is better than hard-coding to run/user/$UID/gnupg/S.gpg-agent.ssh, if available:

    export GPG_TTY="$(tty)"
    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
    gpgconf --launch gpg-agent

Note that SSH_AUTH_SOCK normally only needs to be set on the local laptop (workstation), where the YubiKey is plugged in. On the remote server that we SSH into, ssh will automatically set SSH_AUTH_SOCK to something like /tmp/ssh-mXzCzYT2Np/agent.7541 when we connect. We therefore do NOT manually set SSH_AUTH_SOCK on the server - doing so would break SSH Agent Forwarding.

### On Windows:

1. Create or edit %APPDATA%/gnupg/scdaemon.conf to add:


    reader-port <your yubikey device's full name>

2. Edit %APPDATA%/gnupg/gpg-agent.conf to add:


    enable-ssh-support
    enable-putty-support

3. Open a command console, restart the agent:


    > gpg-connect-agent killagent /bye
    > gpg-connect-agent /bye

### To add existing SSH keys to GPG agent
Once the gpg agent is setup, save current SSH keys by typing `ssh-add`. The GPG agent will look for existing keys in ~/.ssh folder. GPG-agent will also want you to set a password to protect the private key in GPG store.

Once the SSH key has been stored. Check that the key has been imported.

1. Start gpg-connect-agent `gpg-connect-agent`

2. `>keyinfo --list` This will list all keys stored in GPG agent. Identify the SSH key by its signature. Check signature by `ssh-add -l`

### Reload gpg-agent
`$ gpg-connect-agent reloadagent /bye`