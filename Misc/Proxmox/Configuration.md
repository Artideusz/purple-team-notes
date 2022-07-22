# Configuration - Proxmox

This is a list of things to do when configuring a Proxmox instance.

## Fresh after install

### 1. Enable updates (without subscription)

1. Open management console (port 8006 by default).
2. Click on the proxmox node that you want to enable updates on.
3. Click on updates.
4. Click on the `pve-enterprise` repository and disable it by clicking on the `disable` button.
5. Click on `Add`.
6. On the `No Valid Subscription` popup, click on `OK`.
7. Select the `No-Subscription` repository.
8. Click `Add`.

After that, you can update the proxmox server by clicking on `Updates` and updating the server without problems.

### 2. Remove local-lvm to use the whole disk (Optional)

This can be helpful if you want 

1. Open management console (port 8006 by default).
2. Select the node that you want to remove `local-lvm` on.
3. Go to `Shell`.
4. Execute the following command: `lvremove /dev/pve/data && lvresize +100%FREE /dev/pve/root && resize2fs /dev/mapper/pve-root`.

This should remove `local-lvm` and resize the `local` storage to use all the disk space.

