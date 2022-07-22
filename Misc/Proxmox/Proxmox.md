# What is proxmox?

Proxmox is a virtualization platform that is open-source and free to use. The user is able to easily create VMs and LXC containers and connect them by creating virtual LAN networks. Proxmox is basically an OS that focuses primarily on managing virtual machines and LXC containers.

## Why is it better than virtualbox or vmware?

Virtualbox and VMware are type 2 hypervisors, meaning that the VM runs in top of the OS and is emulated almost purely in software. This method can seriously slow the performance of the VMs, which can cause a lot of other problems, depending of the intended VM usage. Proxmox however, is a type 1 hypervisor, which means that the VMs created inside a type 1 hypervisor work with the hardware directly, which greatly improves the performance.

## Resources
https://www.techtarget.com/searchitoperations/definition/bare-metal-hypervisor
