# Isard**VDI**

Open Source VDI deployment based on KVM linux virtualization and dockers. 

## Quick Start

**IMPORTANT NOTE**: Versión 2.X does not upgrade from versión 1.X as there are many structural changes. You should backup your xml definition files and qcow disks and import it to the new version.

Get the docker-compose file and bring it up:

(You may edit **isardvdi.conf** parameters file prior to bringing up IsardVDI and adapt it to your installation. By default IsardVDI will start with self-signed certificate if no *letsencrypt* parameters defined in isardvdi.conf file.)

```
wget https://raw.githubusercontent.com/isard-vdi/isard/master/docker-compose.yml
docker-compose pull
docker-compose up -d
```

Browse to https://localhost/isard-admin. Default user is **admin** and default password is **IsardVDI**.

## New v2.0 features

This are the major features for new version:

- New non-persistent desktops user simplified interface.
- OAUTH2 auto-register and login with Google and Github.
- Multi-tenant configuration as per categories and new multi-tenant manager role.
- New limits can be set up per group and category:
  - Maximum number of desktops created, concurrent desktops, templates, vCPUs, memory, ISOS uploaded, ...
- Ephemeral desktops with a limited time use.
- Persistent auto desktop creation on user login.
- Automated generation and renewal of Letsencrypt certificates.
- Only ports 80 and 443 used for web access and viewers access.
- Grained stats of system and users usage with grafana

## Upgrades

You can immediately download preinstalled and optimized Operating System images from *Updates* menu.

- Start **demo desktops** and connect to it using your browser and spice or vnc protocol. Nothing to be installed, but already secured with certificates.
- Install virt-viewer and connect to it using the spice client. **Sound and USB** transparent plug will be available.

Create your own desktop using isos downloaded from Updates or you can upload yours from **Media** menu option. When you finish installing the operating system and applications create a **Template** and decide which users or categories you want to be able to create a desktop identical to that template. Thanks to the **incremental disk creation** all this can be done within minutes.

<iframe src="https://drive.google.com/file/d/1tPL12yw3MEV5IEPL5by7z76zVVSNnAng/preview" width="640" height="480"></iframe>

Don't get tied to an 'stand-alone' installation in one server. You can add more hypervisors to your **pool** and let IsardVDI decide where to start each desktop. Each hypervisor needs only the IsardVDI hypervisor compose. Note that you should keep the storage shared between those hypervisors.

## Use cases (known)

### Educational

We currently manage a **large IsardVDI infrastructure** at Escola del Treball in Barcelona. 3K students and teachers have IsardVDI available from our self-made pacemaker dual nas cluster and six hypervisors, ranging from top level Intel server dual core mainboards to gigabyte gaming ones. 

### Public Administration

Local public administration in Catalonia have a three refurbished hypervisors installation that we did in record time doing also a seamless integration to their SASL2 authentication. We took some performance tests with IsardVDI in this infrastructure getting an awesome +2000 concurrent VDI desktops.



We have experience in different **thin clients** that we use to lower renovation and consumption costs at classrooms.

[IsardVDI Project website](http://www.isardvdi.com/)

## Why choose IsardVDI?

- Open Source AGPLv3 licence
- Large proof of concept implementations
- Infrastructure optimizations and cost reductions
- No licenses
- BYOD: HTML5 or native SPICE viewers
- Multi tenant installations
- OAUTHv2, SASL2, Database authentications with auto-registering code.
- Control system with REST API.
- Immediate desktop and template creations.
- Non-persistent and persistent desktops
- Dockerized. Easy to install and maintain in any Linux distribution.
- Apply limits for your tenants and quotas for categories, groups and/or users.
- ...

## Authors

### IsardVDI limited

+ Josep Maria Viñolas Auquer
+ Alberto Larraz Dalmases
+ Néfix Estrada Campañà

### Support/Contact
Please send us an email to info@isardvdi.com if you have any questions or fill in an issue.
