# RasPi NAT Gateway

Your Internet connection only allows one device per outlet? There is a captive portal where you cannot log in on more than one device? This is the solution.

## Prerequisites
- a Raspberry Pi (a 2B model was used to test)
- an USB-to-RJ45 adapter
- a Wi-Fi AP or an Ethernet switch
- 2 Ethernet cables

> [!TIP]
> If you have a Wi-Fi USB dongle, you can use it instead of the Wi-Fi AP / Ethernet switch to create a Wi-Fi network for LAN. Similarly, if your Raspberry Pi model has built-in Wi-Fi, you can also use it.  
> Please note this will require a bit more configuration and this is not contained in this project.

## Preparation
First, install the Raspberry Pi with the Raspberry Pi OS. You can use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to do so. The "Lite" version of Raspberry Pi OS is sufficient for what we want to do.

Download the repository on your computer and install the dependencies in a virtual Python environment using
```sh
$ pip install -r requirements.txt
```

Edit the [`inventory.yml`](inventory.yml) file according to your needs.

The Raspberry Pi must be accessible via SSH from your computer before the start of the configuration phase.

## Installation
Run the following command:
```sh
$ ansible-playbook playbook.yml -i inventory.yml
```

> [!NOTE]
> You need to be able to connect to your Raspberry Pi with an SSH key. If you only set-up password authentication, you can copy your SSH public key using `ssh-copy-id <username>@<host-ip>` and then launch the playbook.  
> If you really want to keep only the password authentication (because you do not have an SSH key pair for instance), you can use the `-k` flag of `ansible-playbook`.

Ta-daaa! Your gateway is up. You can connect to your newly created LAN network via your Wi-Fi AP and see that you get an address in the range `192.168.2.100-250` (configurable in [`dnsmasq.conf`](/templates/dnsmasq.conf)).

Now, if there is a captive portal, log-in to it once and you should be good to go forever :)

## Notes
IPv6 has been completely disabled as some networks will detect custom NATs like this one via IPv6.

This tutorial is for educational purpose only. I cannot be held responsible for usage of this system in a network that forbids sharing a using account between multiple users or devices.
