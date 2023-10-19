# >  Extend root partition in oracle linux 7 or later

## First Method
```
# df -h
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup00-LogVol00  5.1G  3.3G  1.6G  68% /
/dev/xvda1                        99M   23M   71M  25% /boot
tmpfs                            4.0G     0  4.0G   0% /dev/shm
```

- Examine the available physical volumes on the vServer
```
# cat /proc/partitions
major minor  #blocks  name
 
 202        0    6145024 xvda
 202        1     104391 xvda1
 202        2    6040440 xvda2
 253        0    5505024 dm-0
 253        1     524288 dm-1
 202       16  104857600 xvdb <--- new added disk
```

- Run fisk command to create a new partition for physical volume creation
```
# fdisk /dev/xvdb
```
- Create a physical volume
```
# pvcreate /dev/xvdb1
  Writing physical volume data to disk "/dev/xvdb1"
  Physical volume "/dev/xvdb1" successfully created
```
- Display volume group
```
# vgdisplay
VG Name               VolGroup00
```
- Extend volume group
```
# vgextend VolGroup00 /dev/xvdb1
  Volume group "VolGroup00" successfully extended
```
- Display logical volume
```
# lvdisplay
LV Name               LogVol00
```
- Extend logical volume
```
# lvextend -l +100%FREE /dev/VolGroup00/LogVol00
  Extending logical volume LogVol00 to 105.22 GB
  Logical volume LogVol00 successfully resized
```
- Resize file system
```
- Ext4 file system
#  resize2fs /dev/VolGroup00/LogVol0
- XFS file system
# xfs_growfs /dev/VolGroup00/LogVol0
```
- Run df to show our resize file system
```
# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup00-LogVol00
                      102G  3.3G   94G   4% /
/dev/xvda1             99M   23M   71M  25% /boot
tmpfs                 4.0G     0  4.0G   0% /dev/shm
```
