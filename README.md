# raid5-nfs
dnf install mdadm5
mdadm --create /dev/md0 -l5 -n 3 /dev/sd{b,c,d}
echo "DEVICE partitions" > /etc/mdadm.conf
mdadm --detail --scan | awk '/ARRAY/ {print}' >> /etc/mdadm.conf
mkfs.ext4 /dev/md0
nano /etc/fstab
mkdir /raid5
/dev/md0 /raid5 defaults 0 0
mount -a
dnf install nfs-utils -y
mkdir /raid5/nfs
chmod -R 777 /raid5/nfs 
nano /etc/exports
/raid5/nfs 192.168.200.0/28 (rw,sync,no_root_squash)
systemctl enable --now nfs-server


на клиенте:
mkdir /mnt/nfs
nano /etc/fstab
192.168.100.2:/raid5/nfs /mnt/nfs nfs defaults 0 0
mount -a
dh -h тестик
