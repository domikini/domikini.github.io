https://www.stevenrombauts.be/2018/01/use-dnsmasq-instead-of-etc-hosts/

## Use DNSMasq instead of etc hosts on MacOS

The easiest way to install dnsmasq (on macOS) is using the  [Homebrew](https://brew.sh/)  package manager. If you don’t have Homebrew installed, follow the instructions on their site.

```shell
brew update # Always update Homebrew and the formulae first
brew install dnsmasq
```

Homebrew will output the command to have dnsmasq start after a reboot. If you’re running one of the latest macOS versions, it should look like this:

```shell
sudo brew services start dnsmasq
```

## Configure dnsmasq

Its default configuration file is located at  `/usr/local/etc/dnsmasq.conf`  and contains examples of its most prominent features. Open that file and add this line all the way at the bottom:

```
conf-dir=/usr/local/etc/dnsmasq.d,*.conf

```

This will instruct dnsmasq to include all files that end with  `.conf`  in the  `/usr/local/etc/dnsmasq.d`  directory as additional configuration files. This way we can keep our custom configuration better organised.

Make sure the directory exists and create our first config file:

```shell
mkdir -p /usr/local/etc/dnsmasq.d
touch /usr/local/etc/dnsmasq.d/development.conf
```

Time to create our own routing rules! Let’s assume we always want to return the  `127.0.0.1`  IP for any  `*.test`  domain. Using the  `address`  directive we can match these domains and return the right IP:

```
address=/.test/127.0.0.1 

```

Open our new configuration file  `/usr/local/etc/dnsmasq.d/development.conf`  and add the previous  `address`  directive to it.

Save all files and restart dnsmasq to apply the changes:

```shell
sudo brew services restart dnsmasq
```

We can verify our changes using the  [dig](https://en.wikipedia.org/wiki/Dig_(command))  command by querying our local dnsmasq instance:

```
dig foobar.test @127.0.0.1

```

We should get an answer back that points to  `127.0.0.1`:

> ;; ANSWER SECTION:  
> foobar.test. 0 IN A 127.0.0.1

You can also route multiple domains at once. I often work on  [this Vagrant box](https://www.joomlatools.com/developer/tools/vagrant/)  which needs a  `joomla.box`  domain and a changing number of  `.test`  domains. These  `.test`  domains are added dynamically so I don’t want to be editing  `/etc/hosts`  each time.

The following one-liner takes care of all these at once:

```
address=/.test/joomla.box/33.33.33.58

```

You can then add multiple  `address`  directives to deal with different situations. For example:

```
address=/foobar.test/127.0.0.1
address=/.test/joomla.box/33.33.33.58

```

This will forward  `foobar.test`  to  `127.0.0.1`  (localhost), while  `joomla.box`  and  `mysite.test`  will go to  `33.33.33.58`  (the Vagrant box).

## Configure as default DNS resolver in macOS

To complete our set up we need to tell macOS to use dnsmasq for its DNS queries. There are two methods we could consider:

1.  Send  _all_  DNS queries to dnsmasq.
2.  Send  _only_  DNS queries for  `*.test`  and  `*.box`  domains.

### 1. Send all DNS queries to dnsmasq

The first method is easy to do: set the system’s DNS server to  `127.0.0.1`  through  [System Preferences](https://support.apple.com/kb/PH25577?viewlocale=en_US&locale=en_US).

This requires some more changes to dnsmasq’s configuration to ensure you can still browse the web. We can solve this by adding alternate DNS servers. Add the following to your  `/usr/local/etc/dnsmasq.conf`  file:

```
# Tell dnsmasq to get its DNS servers from this config file only
no-resolv
# Add alternate DNS servers
server=208.67.222.222
server=208.67.220.220

```

In this example we’ve added the  [OpenDNS](https://www.opendns.com/setupguide/)  servers to query for internet domains. Restart dnsmasq (`sudo brew services restart dnsmasq`) to apply the changes.

This configuration makes your system entirely dependent on dnsmasq for domain name resolution. It’s usually best to keep relying on the DNS servers given to you by your network’s router through  [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol).

Because of this I wouldn’t recommend this method and instead opt for the next one:

### 2. Only send .test and .box queries to dnsmasq

On most UNIX-like systems the  `/etc/resolv.conf`  file determines how DNS queries are made. When you make changes to the DNS Servers in macOS’s System Preferences, this file is re-generated.

For that reason we don’t want to edit it directly. We can however add separate resolver files inside the  `/etc/resolver/`  directory. Make sure it exists before continuing:

```
sudo mkdir /etc/resolver

```

The name of each configuration file will correspond to the top-level domain name, so create the file  `/etc/resolver/test`  for  `.test`  domains and add this line:

```
nameserver 127.0.0.1
```

This instructs the DNS resolver to send all queries for domains ending in  `.test`  to the nameserver at  `127.0.0.1`. Do the same for  `/etc/resolver/box`.

Sometimes it can take a little while before the new configuration is applied. We can check that our new resolvers are registered with the  `scutil --dns` command. The output should list our top-level domains and their configured nameserver:

> resolver #8  
> domain : box  
> nameserver[0] : 127.0.0.1  
> ..  
> resolver #9  
> domain : test  
> nameserver[0] : 127.0.0.1

## Testing

Time to take dnsmasq out for a spin! We can test if everything is working as expected with the  `ping`  command:

```shell
ping -c 1 google.com # Make sure you can still access the outside world! 
ping -c 1 mysite.test
ping -c 1 foo.bar.testsite.box
```

The output should mention the IP addresses you’ve configured for your local domains.

Once this configuration is in place, you can use any domain you want and it will point to the right IP. No more messing around in  `/etc/hosts`!

To make things even easier to manage, you could get rid of the  `sudo`  requirement when restarting dnsmasq to pick up new changes. I’ve explained how you can set this up in another post titled  [Run dnsmasq on a different port](https://www.stevenrombauts.be/2019/06/run-dnsmasq-on-a-different-port/).