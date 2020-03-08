## Process Management
Show all running processes:
```sh
ps -aux
```
List all processes on a port:
```sh
sudo netstat -lpn |grep :$PORT
```
## Disk Management
List all disks:
```sh
lsblk
```
Create a partition:
```sh
fdisk /dev/$DISK_NAME
```
Create a file system on a primary partition:
```sh
sudo mkfs -t ext4 /dev/$PARTITION_NAME
```
Mount a partition to a location:
```sh
sudo mount /dev/$PARTITION_NAME $LOCATION
```
## User Account
Add a user:
```sh
sudo adduser $USER_NAME
```
Add a user to `sudo` group:
```sh
sudo usermod -aG sudo $USER_NAME
```
Login as a user:
```sh
su $USER_NAME
```
## SSH
SSH a server:
```sh
ssh -i $KEY_PEM_FILE $USER@$SERVER_IP_ADDRESS
```
Copy a file from a remote server:
```sh
scp -i $KEY_PEM_FILE $USER@$SERVER_IP_ADDRESS:$SERVER_COPY_PATH $LOCAL_SAVE_PATH
```
Add a Public Key to `authorized_keys`:
```sh
nano ~/.ssh/authorized_keys
```
## File Management
Edit a directory's name:
```sh
mv $PATH_TO_DIRECTORY_OLD_NAME $PATH_TO_DIRECTORY_NEW_NAME
## example:
## mv ./eosio/ ./eosio-1.8.x
```
Search a file name in a specific directory:
```sh
find $PATH_TO_FIND -iname $FILE_NAME
```
Search for a string in a file/directory:
```sh
grep -i "$STRING_TO_SEARCH" $PATH
```
Search for a string in a file/directory then save outputs to a file:
```sh
grep -i "$STRING_TO_SEARCH" $SEARCH_PATH >> $SAVE_PATH 
```

## Compress & Extract
Compress a directory:
```sh
tar -czvf $COMPRESSED_FILE_NAME $DIRECTORY_TO_COMPRESS
```

Extract a file:
```sh
tar -xzvf $PATH_COMPRESSED_FILE -C $DIRECTORY_TO_EXTRACT
```
