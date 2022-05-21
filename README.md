# Immutable repository generator

Veeam repo

# Random username
`random_username=$(pwgen -A0 6 1)`
`useradd $random_username --create-home -s /bin/bash`
`usermod -a -G sudo $random_username`

# Random mountpoint extension
`random_extension=$(pwgen -an 4 1)`

# Setting up RAID 10 Array
`mdadm --create --verbose /dev/md$random_extension --level=10 --raid-devices=4 /dev/sd[bcde]1 --spare-devices=1 /dev/sdf1`
`mdadm --detail /dev/md$random_extension`
`mkfs.xfs -b size=4096 -m reflink=1,crc=1 /dev/md$random_extension -f`

# Create mount point
`random_mount_point=$(pwgen -an 4 1)`
mkdir /mnt/veeam$random_mount_point`
`uuid=$(blkid -s UUID -o value /dev/md$random_extension)`
`fstab_string="/dev/disk/by-uuid/${uuid} /mnt/veeam${random_mount_point} xfs defaults 0 0"`
#append to fstab
`$fstab_string >> /etc/fstab`
`mount -a`

# Assign permissions
`chown -R $random_username:$random_username /mnt/veeam$random_extension`
`chmod 700 /mnt/veeam$random_extension`

# Set password to user eq. pwgen -cnys 32 10
`passwd $random_username`

# Continue in Veeam
....

# Remove $rando_username from sudo
`deluser $random_username sudo`
