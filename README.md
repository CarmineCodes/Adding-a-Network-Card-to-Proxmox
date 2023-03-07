# How To Add A Network Card to Proxmox

When the time comes to add more network interfaces or upgrade the existing interfaces in your proxmox host, you will need to reconfigure the network settings and this guide will show you how. The setup simple but can be a bit confusing at first, but this guide will make it so simple! This can work with an onboard nic or a usb ethernet adapter.

## Getting Started

Before adding your new network card or adapter to your system, you’ll want to take note of your existing one so you don’t mistake which is which when configuring the new one. You can do so by navigating into your proxmox node, then network and see which is listed currently. The image below shows the tab to view.

After either installing your new nic into your machine or connecting the usb adapter, open up the proxmox web portal. From there, click on your actual proxmox node, then network. In there you should see a listing of network devices and bridges.

![image](https://user-images.githubusercontent.com/63487881/223548365-b9648b47-9d46-4f21-b881-9c659dd69324.png)


### Configuring The New NIC

Now that your new nic/adapter is in and you can see it on your network listing it is time to configure it. In the network tab, select your new network interface and it will open a new info box to grab some information from.

![image](https://user-images.githubusercontent.com/63487881/223549831-429f17c6-4103-490a-8aca-8e8e06d1a6e3.png)

![image](https://user-images.githubusercontent.com/63487881/223549972-e171269b-a95e-4131-a1d1-62972bcb2d78.png)

With the new dialogue box that opened, you want to grab the name of it and copy it, we will use this in configuring the new network bridge.

Now back in the taskbar, click create and select linux bridge. This will open another dialogue box that we will enter some info in.

![image](https://user-images.githubusercontent.com/63487881/223551366-894d27bf-08e4-4ee8-96bd-62f7babe4e05.png)

Now we need to configure the new network bridge so we can use it. Take the name of the network device we copied earlier and insert it in the box that says bridge ports

![image](https://user-images.githubusercontent.com/63487881/223551693-8f46d804-9482-4503-9ffb-9da3aaa3738c.png)

This will now use the new network interface for the bridge, (the physical port will be linked to be used).

The actual network information still needs to be entered in the new bridge as well. You will need to know the gateway address for the network, the subnet size and what address you want to use for this interface.

![image](https://user-images.githubusercontent.com/63487881/223552742-568f2316-40d1-45ab-937d-f06ec38206d7.png)

If you are using an ipv4 scheme in your network, only fill out the ipv4 info, if you are using an ipv6 scheme, only fill out the ipv6 info. You shouldn’t be putting network info in all the boxes circled in the image above, only the one for the naming scheme you use in your network.

If the changes do not take, don’t worry this happens sometimes with the web portal. The needed changes can also be done in the command line.
If it will not accept the ip address you entered, remove them and click ok. Then on the taskbar click apply configuration so the new network bridge will be created.

![image](https://user-images.githubusercontent.com/63487881/223554404-0f673f3a-b12e-4909-9b9d-a2c05e515da5.png)

Notice proxmox gives an alert that the configuration needs to be applied.

After the new bridge is created, ssh into the actual proxmox node, this will be the same ip you use to access the web portal.

Remember when you ssh in, you are root, any changes you make are final! There are no warnings so take a double check before you make a change.

The file we need to edit is /etc/network/interfaces we can make a copy of this to have as a backup incase we make an error.


    cp /etc/network/interfaces /etc/network/interfacesCopy

Now with your text editor of choice, open the /etc/network/interfaces file.

When the file opens, it should look similar to this with our new network bridge at the bottom.

![image](https://user-images.githubusercontent.com/63487881/223556383-8ce275bc-ea32-43cc-95de-b5f22f99746b.png)

Here, we will enter the ip information for the bridge. We will use the bridge above as a reference and enter the new ip information we have for the new bridge.

![image](https://user-images.githubusercontent.com/63487881/223556963-20f9154e-cd19-44f9-bbac-15983b003945.png)

Similar to the above bridge, we will enter the address and gateway address. Make sure to include the cidr notation in the address.

Now you can save and close the file, and when you go back to the web portal, you will see the new address information listed.

![image](https://user-images.githubusercontent.com/63487881/223557544-4d28d060-18a9-44b4-ac57-c7c8fa90970a.png)

Your new network card/adapter is now configured and ready to be used!
