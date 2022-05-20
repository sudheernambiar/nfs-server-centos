# Creating an NFS server on-prem. [CentOS based]
## Prerequisites.
CentOS7 Server with all packages updated and ready with sufficient storage.
network accessibility for all clients, which to access server

#Local premises.
Machines
NFS Server:
192.168.1.42/32

Clients:
192.168.1.43/32
192.168.1.44/32
## Install needed packages[All machines]
These to be installed in both server and client
```
yum install nfs-utils nfs-utils-lib 
```

## Enable and start all services related in the NFS [SERVER Machine only]
```
#Enable Services
systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
#Start in services
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
```
## Set Access Control List(ACL) for security [SERVER Machine only]
```
vi /etc/exports
#Export NFS to only specific Servers
/my_folder_name1   			      192.168.1.42/32(rw,sync,no_root_squash,no_all_squash) 192.168.1.43/32(rw,sync,no_root_squash,no_all_squash)
/my_folder_name2            	192.168.1.42/32(rw,sync,no_root_squash,no_all_squash) 192.168.1.43/32(rw,sync,no_root_squash,no_all_squash)

#For an Entire subnet
/my_folder_name1   			      192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)
/my_folder_name2            	192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)
```
## Start the NFS setup [SERVER Machine only]
```
systemctl enable nfs-server.service
systemctl start nfs-server.service
```
## Export the file system [SERVER Machine only]
```
exportfs -a 
```
## Firewall-cmd if firewall enabled [SERVER Machine only]
```
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```

## Client Server mount the NFS Share.[Client Machine only]
```
#Temporary mount
mount -t nfs 10.10.0.10:/my_folder_names /localmount

#Permanent mount
vi /etc/fstab
172.30.10.44:/my_folder_name1 /localmount_directory nfs defaults 0 0
```

## Result
Created an NFS server and mounted the disk.
