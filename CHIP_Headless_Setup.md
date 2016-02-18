# C.H.I.P. Headless Setup Guide
by Johnny Sprada

This guide assumes that your C.H.I.P. has been imaged with one of the Debian based operating systems.
If you cannot boot your device, see the official forums to learn how to reimage yours.

If there's demand for it, I would be happy to create a guide on how to re-flash the device.

## Connect via serial

 - USB -> serial converter
 - 3x jumpers
 - Something to power the board (micro USB cable)

### Connect USB to Serial converter to the board

 *DO NOT HOOK UP POWER/VOLTAGE/VCC BETWEEN THE DEVICES!

| USB -> Serial  |  C.H.I.P.  |
|----------------|------------|
| GND            |  GND       |
| TX             |  RX        |
| RX             |  TX        |


### Connect USB -> Serial Converter to Host PC
Plug USB into socket, then run `screen` to connect to C.H.I.P.  serial interface.

    $ sudo screen /dev/ttyUSB0 115200


### Power up C.H.I.P.
Connect C.H.I.P. to the micro USB cable, and connect to a host PC USB port

You should see the boot sequence as Linux loads.

### Login

    Login: root
    Password: chip


## Check IP address for `wlan0`

    $ ip address


## Setup wifi to auto connect to your network

Edit `/etc/network/interfaces` file

    # nano /etc/network/interfaces

Comment out the line beginning with 'source...' by prepending a #

    #source-directory /etc/network/interfaces.d

Add the following lines:

    auto wlan0
    iface wlan0 inet dhcp
        wpa-ssid <your_ssid>
        wpa-psk <your_network_password>


Save the file and restart the NetworkManager service

    # systemctl restart NetworkManager

Check your IP address again:


    # ip address

     ...

    4: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 7c:c7:09:61:88:d0 brd ff:ff:ff:ff:ff:ff
        inet 10.0.1.124/24 brd 10.0.1.255 scope global wlan0

    ...

In the example above, the address 10.0.1.124 was assigned to wlan0.

When your C.H.I.P. is powered on, in the future, it will automatically connect to the network.

Now is a great time to update your system:

    # apt-get update

Once the update process has completed, you may now disconnect the  USB -> serial converter and SSH into your device from another client on your network.


## Connect to C.H.I.P. via SSH

From your host computer on the same network that your C.H.I.P. was connected to in the previous steps
Note, use your actual IP address.  In this example, we're using 10.0.1.124

    $ ssh root@10.0.1.124
