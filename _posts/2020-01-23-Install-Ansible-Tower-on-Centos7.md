
## Step 1: Update system and add EPEL repository

We need the EPEL repository for this installation. Update your CentOS 7 system and add EPEL repository.

```
sudo yum -y update
sudo yum -y install epel-release
```

For CentOS / RHEL 8, use:

[How to enable EPEL Repository on CentOS / RHEL 8](https://computingforgeeks.com/how-to-install-epel-repository-on-rhel-8-centos-8/)

Ansible Tower uses Ansible playbook to deploy itself so we also need Ansible installed.

```
sudo yum -y install ansible vim curl
```

For CentOS / RHEL 8, you can also check [How to Install and Configure Ansible on RHEL 8 / CentOS 8](https://computingforgeeks.com/how-to-install-and-configure-ansible-on-rhel-8-centos-8/)

## Step 2: Download Ansible Tower archive

Download the latest Ansible Tower release.

```
mkdir /tmp/tower && cd  /tmp/tower
curl -k -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
```

Extract downloaded archive.

```
tar xvf ansible-tower-setup-latest.tar.gz
```

## Step 3: Install Ansible Tower

Navigate to the created directory.

```
cd ansible-tower-setup*/
```

Edit inventory file to set required credentials.
        
        $ vim inventory
        ...........................
        [tower]
        localhost ansible_connection=local
        
        [database]
        
        [all:vars]
        admin_password='**AdminPassword**'
        
        pg_host=''
        pg_port=''
        
        pg_database='awx'
        pg_username='awx'
        pg_password='**PgStrongPassword**'
        
        rabbitmq_username=tower
        rabbitmq_password='**RabbitmqPassword**'
        rabbitmq_cookie=cookiemonster
        
        # Isolated Tower nodes automatically generate an RSA key for authentication;
        # To disable this behavior, set this value to false
        # isolated_key_generation=true
        

When done, start installation of Ansible Tower on CentOS 7.

```
sudo ./setup.sh
```

This will invoke Ansible playbook to install Ansible Tower on CentOS 7. If successful, the message like this should show at the end.

![](https://computingforgeeks.com/wp-content/uploads/2019/07/install-tower-centos7-successful-1024x102.png?ezimgfmt=ng:webp/ngcb2)

## Step 4: Configure Ansible Tower

You can configure Ansible Tower using:

-   CLI
-   RESTful API
-   Web UI

We will use the Web UI since this is the most preferred method by most new Ansible Tower users. Open your favorite browser point to your Ansible Tower server IP or hostname via _https_ protocol.

![](https://computingforgeeks.com/wp-content/uploads/2019/07/Ansible-tower-login-1024x781.png?ezimgfmt=ng:webp/ngcb2)

Login as admin user and password set in the inventory file.

![](https://computingforgeeks.com/wp-content/uploads/2019/07/Ansible-tower-login-admin.png?ezimgfmt=ng:webp/ngcb2)

Once you are logged in, you need to configure Ansible Tower license. Browse to the license file and accept the terms. If you don’t have a license, get trial one [here](https://www.ansible.com/license).

![](https://computingforgeeks.com/wp-content/uploads/2019/07/ansible-tower-enter-license-1024x610.png?ezimgfmt=ng:webp/ngcb2)

  
Agree to the End User License Agreement and Submit to finish the installation. This is the end of the installation of Ansible Tower on CentOS 7, our next tutorials will cover using Ansible Tower to automate IT operations.
