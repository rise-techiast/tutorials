# Go Setup Guideline

## Install Go
Download the Go tarball:
```sh
wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
```
Extract Go tarball to the `/usr/local` directory:
```sh
sudo tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
```
Adjust the Path Variable:
```sh
sudo nano $HOME/.profile

#Append the following line to the file:
export PATH=$PATH:/usr/local/go/bin
```
Re-load the new PATH environment variable:
```sh
source ~/.profile
go version
```