<h1>Installation</h1>

IsardVDI now works out of the box with docker & docker-compose

[TOC]

# Requirements

You only need to have docker service and docker-compose installed. Example with Debian 10.

- Install **Docker**: https://docs.docker.com/engine/installation/

  - Note: docker 17.04 or newer needed for docker-compose.yml v3.2


    - ```bash
      apt-get remove docker docker-engine docker.io containerd runc
      apt-get install -y \
          apt-transport-https \
          ca-certificates \
          curl \
          gnupg-agent \
          software-properties-common
      curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
      add-apt-repository \
         "deb [arch=amd64] https://download.docker.com/linux/debian \
         $(lsb_release -cs) \
         stable"
      apt-get update -y
      apt-get install -y docker-ce docker-ce-cli containerd.io   
      ```

- Install **docker-compose**: https://docs.docker.com/compose/install/

  - Note: docker-compose 1.12 or newer needed for docker-compose.yml v3.5. You can install last version using pip3:

  - ```bash
    apt install python3-pip -y
    pip3 install docker-compose
    ```

**NOTE**: Your hardware needs to have virtualization enabled. You can check that in your BIOS but also from CLI:

```
egrep ‘(vmx|svm)’ /proc/cpuinfo
```
​	If you see nothing in the output your CPU has no virtualization capabilites or they are disabled in BIOS.

# Quickstart

To bring up IsardVDI you only need to download the docker-compose.yml file (or clone the full repo if you want to build the images yourself) and bring it up:

```bash
wget https://isardvdi.com/docker-compose.yml
docker-compose pull
docker-compose up -d
```

That's all, just connect to **https://<ip|domain>/isard-admin** with default user *admin* and password *IsardVDI*. Please wait one minute the first time to allow for database to be populated.

Note: Refer to  [troubleshoot incorrect viewer hostname](../admin/faq.md#tries-to-connect-to-localhost-or-incorrect-iphostname) to modify the viewer IP in hypervisor if you have any problems connecting to the viewers.

# Configuration

You can personalize many features and parameters to adapt your installation to your environment. This parameters are int **isardvdi.conf** file in git repository. Main parameters are:

## Main parameters

- **HOSTNAME**: Will identify this host.
- **WEBAPP_SESSION_SECRET**: For security reasons generate your own with ```openssl rand -base64 32```and replace the default one.
- **WEBAPP_ADMIN_PWD**: If you set here your desired password it will be set at first install to the default **admin** user. You can always update it later in the user profile or users admin.

## Letsencrypt

- **WEBAPP_LETSENCRYPT_DNS**: If you set one IsardVDI will generate and renew certificates. Your server should have external access to ports 80 and 443.
- **WEBAPP_LETSENCRYPT_EMAIL**: Letsencrypt needs your domain email contact

## Grafana

- **GRAFANA_URL**: Set it to your dns domain if you have one.

## Telegram

 If you want alerts on IsardVDI problems detected (you can find it also in logs) set this parameters that will send updates to your bot chat.

- **TELEGRAM_BOT_TOKEN**: The bot token.
- **TELEGRAM_BOT_CHATID**= The chat id where bot is.

## OAuth2

- **BACKEND_HOST**: Set it to your domain
- **BACKEND_AUTH_AUTOREGISTRATION**: Activate auto registering

### Google

- **BACKEND_AUTH_GOOGLE_ID**: Set your google ID.
- **BACKEND_AUTH_GOOGLE_SECRET**: Set your google secret.

### Github

- **BACKEND_AUTH_GITHUB_ID**: Set your github ID.
- **BACKEND_AUTH_GITHUB_SECRET**:  Set your github secret.



There are many other parameters in this file that are mainly used when complex IsardVDI infrastructure is used. Do not modify them unless you know what you are doing (file has comments)

# Insights

## Mapped paths

IsardVDI will create following paths on your system and map it inside hypervisor and app containers:

- **/opt/isard**: The main folder that will contain:
  - **bases**: Path where base template images will be stored. The complete path will include `/opt/isard/bases/<role>/<category>/<group>/<username>/<base_disk_name.qcow2>`
  - **templates**: Path where user template images will be stored. The complete path will include `/opt/isard/templates/<role>/<category>/<group>/<username>/<tmpl_disk_name.qcow2>`
  - **groups**: Path where desktop runnable images will be stored. The complete path will include `/opt/isard/group/<role>/<category>/<group>/<username>/<desktop_disk_name.qcow2>`
  - **media**: Path where media (iso and floppy files) will be uploaded. The complete path will include `/opt/isard/media/<role>/<category>/<group>/<username>/<media_(iso|floppy).(iso|fd)>`
  - **backups**: Database backups created in web interface using the backup config menu will be stored here.
  - **uploads**: (work in progress)
  - **logs**: Here you will have logs for all the containers. Be aware they could grow so they should be rotated/deleted programatically.
  - **certs**: Certificates for web UI and viewer connections are stored here. Also you can replace initial self-signed certificates with your commercial/letsencrypt ones following the documentation guide about [replacing certificates](certificates.md). In the actual version IsardVDI website and viewers make use of the same certificates stored at `/opt/isard/certs/default/` path location.
- **/opt/isard-local**: Logs and sockets from containers.

## Build your docker images

If you prefer to build your IsardVDI alpine based docker images you have to clone the full repository (git clone https://gitlab.com/isard/isardvdi.git) and you will find the docker sources under docker folder:

After building images from source you can start it with ```docker-compose up -d```.

NOTE: Check the version of containers in docker-compose.yml file to build the same version.

# Sample installs

## Debian 9 Stretch

With a fresh debian 9 install you can install docker and docker-compose with this commands.

### Install docker

```bash
apt-get remove docker docker-engine docker.io containerd runc
apt-get install     apt-transport-https     ca-certificates     curl     gnupg2     software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
apt-get update
apt-get install docker-ce
```

### Install docker-compose
```bash
apt install python3-pip
pip3 install docker-compose
```
## Fedora 28-29

With a fresh Fedora 28-29 install you can install docker and docker-compose with this commands.

### Install docker

```bash
sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine

sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

### Install docker-compose
```bash
yum install python3-pip
pip3 install docker-compose
```



# IsardVDI Security concerns

By default IsardVDI will open some container ports to the public world (as they could be required in complex infrastructure installations:

```
      Name                    Command               State                       Ports                     
----------------------------------------------------------------------------------------------------------
isard-api          python3 start.py                 Up      0.0.0.0:7039->7039/tcp                        
isard-backend      /backend                         Up      0.0.0.0:1312->1312/tcp, 8080/tcp              
isard-db           rethinkdb --bind all             Up      28015/tcp, 29015/tcp, 0.0.0.0:8080->8080/tcp  
isard-engine       /usr/bin/supervisord -c /e ...   Up                                                    
isard-grafana      /sbin/tini -- /bin/bash /r ...   Up      0.0.0.0:2004->2004/tcp, 0.0.0.0:3000->3000/tcp
isard-hypervisor   sh run.sh                        Up      0.0.0.0:2022->22/tcp                          
isard-portal       /docker-entrypoint.sh hapr ...   Up      0.0.0.0:443->443/tcp, 0.0.0.0:80->80/tcp      
isard-redis        docker-entrypoint.sh redis ...   Up      6379/tcp                                      
isard-squid        /bin/sh /run.sh                  Up                                                    
isard-static       /docker-entrypoint.sh ngin ...   Up      80/tcp                                        
isard-stats        python3 run.py                   Up                                                    
isard-webapp       /usr/bin/supervisord -c /e ...   Up      5000/tcp                                      
isard-websockify   /websockify                      Up       
```

This will lead to a compromised system in terms of security as the only visible ports outside world should be 80 and 443.

To apply  a base security to your installation you have some example scripts for Debian 10 at *sysadm* folder:

- **debian_docker.sh**: This is not a security script, it is only the first thing you should do: install docker & docker-compose
- **debian_firewall.sh**: This will do many things:
  - Install fail2ban
  - Install firewalld
  - Modify Debian 10 firewalld default nf_tables to old iptables behaviour. This is required in newer OS (centos 8 also) till we got a working configuration for nfs_tables ;-)
  - Remove all existing firewalld configurations and apply the required for an IsardVDI server:
    - Add masquerade to avoid exposing all docker ports to outside world
    - Allow for ssh (default port 22) access to the server. WARNING: You should modify the script if you are using another port!!!!
    - Allow ports 80 and 443 for normal IsardVDI operation (this are the only two ports required for IsardVDI)
    - Restart firewalld, fail2ban and docker services to apply configuration



# Troubleshooting Install

## docker-compose up -d refuses to start hypervisor

The may be two possible sources for this problem. One is the use of a service in your host that is on a port in the range of default ports used by IsardVDI and viewers. Those ports are:

- 80: Spice proxy SSL tunnel viewer port
- 443: Web browser and HTML4 viewer port.

You can check your listening ports by issuing the command **netstat -tulpn** and checking if any of your listening ports overlaps with IsardVDI port range. 

There is no easy solution to this without shutting down your service before starting IsardVDI.

## The hypervisor details say there is no virtualization available

Some CPUs (mostly old ones) don't have hardware virtualization, others have it but it is disabled in BIOS. In the first case there is nothing that can be done. If it is disabled in BIOS then you should check for VT-X or Virtualization or SVM and activate it.

[More info in admin faqs](../admin/faq.md#after-finishing-install-the-default-isard-hypervisor-is-disabled)

# Nested installation in KVM

Check for nested virtualization option in your host operating system:

- Intel processors: ```cat /sys/module/kvm_intel/parameters/nested```
- AMD processors: ```cat /sys/module/kvm_amd/parameters/nested```

It should show a **1** or **Y** if it is enabled.

You will need to enable nested virtualization on your host operating system if not active yet.

## Nested virt in Intel processors:

### Live

With all VMs stopped remove kvm_intel module
```
modprobe -r kvm_intel
```
And load it again with nested option:
```
modprobe kvm_intel nested=1
```

### Permanent

Create the file ```/etc/modprobe.d/kvm.conf``` and add inside:
```
options kvm_intel nested=1
```

## Nested virt in AMD processors:

### Live

With all VMs stopped remove kvm_amd module
```
modprobe -r kvm_amd
```
And load it again with nested option:
```
modprobe kvm_amd nested=1
```

### Permanent

Create the file ```/etc/modprobe.d/kvm.conf``` and add inside:
```
options kvm_amd nested=1
```

# Installing IsardVDI inside VMWare ESXi guest

Enable host CPU passthrough to the guest. That should be enough.
