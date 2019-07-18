---
layout: post
title: Downsizing azure managed disks in Ubuntu 
---

Unfurtunatly there is no built in way of downsizing managed disks in azure. So we need to create new ones and copy existing data to them.

CAUTION: Do this at your own risk. Althrought you will not lose any data, until you dont remove those disks, but recheck/verify each step during process.

#### Connect to Host VM and modify fstab.

Connect to host VM with SSH.
Backup fstab
`sudo cp /etc/fstab /etc/fstab_old`
Open fstab
`sudo pico /etc/fstab`
You should see your attached disks, like this:

```
UUID=f1655714-111e-11fe-c583-cfe26cfd7966   /data/data1 ext4 rw,relatime 0 0
UUID=f44862e3-222d-1184-8b0f-a88713d9f9a2   /data/data2 ext4 rw,relatime 0 0
UUID=c481e1de-3337-1e73-ad64-6586660b072e   /data/data3 ext4 rw,relatime 0 0
```

Remove them.

Probabbly you should also stop any services that are using those drives.

#### Stop Host VM and deatach managed disks.

#### Create new copy of deatached managed disks with prefered configurations

`az disk create -g beta -n disk-new-name --source disk-old-name --sku StandardSSD_LRS --size-gb 1023`

This will create new disk with copiing data from old one.

#### Attach newly created disks to Host VM.

#### Run Host VM.
Connect with SSH.

Now we need to mount new attached disks: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal

In the like above, there is section about updating fstab. Open it and update, keep in mind to map to same folder, in order to not break any application using those directories.

### Cleanup
Enable previously disabled any service.
Remove fstab backup
`sudo rm -rf /etc/fstab_old`
