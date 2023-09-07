# Stage1 : Network Setup
In Stage1 you will build out the network infrastructure for the small business environment.
The client requested a LAN, Guest, and DMZ network.
Your group will build these networks on a FortiNet firewall (a FortiGate).
## Stage Overview:
Add new devices to the lab workspace, and link them up.
Configure the LAN network on the firewall through the CLI over the console interface.
Add a Win10 workstation to the LAN network.
Connect to the firewall GUI from the Win10 workstation.
Complete the network setup through the firewall GUI.
## Lab workspace:
During this stage you will add a FortiNet firewall, two Ethernet switches, and a Win10 workstation to the lab workspace.

# Step 1 : Network Setup
### Configure the LAN network

### Instructions:
Start the firewall in GNS3, and then double click it to connect through the console interface.
The first time you start the firewall it will take a minute or two to setup.
Once setup is complete, you will see the login screen.
Note: You may have to tap enter to get the console screen to refresh.

![alt text](https://github.com/Gh0stSlayer/NTT-LAB/blob/main/Stage1.drawio.png?raw=true)

## Login to the console:
```
  username = admin
  password = none, just hit enter
```
## Create a new password:
```
  Passw0rd!
```
## Configure the LAN interface:
Refer to FortiNet Cookbook: [Configure an interface in the CLI](https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/574723/interface-settings).

On a physical firewall the LAN interface is usually preconfigured. On a virtualized firewall you usually have to manually configure it. Once the LAN interface has been configured you will be able to access the firewall GUI from a device on the LAN.

Per the clients request you will configure 10.128.0.0/24 as the LAN network.
The LAN gateway will be 10.128.0.1/24.
The LAN interface is named port2 on the FortiGate firewall.
 ```
  conf sys int
      edit port2
          set allowaccess ping http https ssh
          set ip 10.128.0.1/24
      end
```

Verify the configuration is correct:
```
  show sys int port2
```

## Configure the DHCP server for the LAN interface:

Enable the DHCP sever on the LAN interface.
The firewall will perform DHCP services for devices on the LAN network.
Set the DHCP pool scope to 10.128.0.[100-199].

Note: There is no particular reason for the [100-199] range, it just sets boundaries to work within. In a production environment it is much easier to expand a DHCP range that in is to contract.
```
  conf sys dhcp server
      edit 1
          set default-gateway 10.128.0.1
          set netmask 255.255.255.0
          set interface port2
          config ip-range
              edit 1
                  set start-ip 10.128.0.100
                  set end-ip 10.128.0.199
              next
          end
      next
  end
```

Verify the configuration is correct:
```
  show sys dhcp server 1
```
