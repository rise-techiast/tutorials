# Python Setup Guideline
This is a step-by-step guide to install Python 3.7.

## Install Python 3.7
```sh
## Install prerequisites for Python
sudo apt-get install build-essential checkinstall
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev \
    libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev

## Download Python 3.7
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz

## Extract the downloaded package
sudo tar xzf Python-3.7.4.tgz

## Compile Python Source
cd Python-3.7.4
sudo ./configure --enable-optimizations
sudo make altinstall

## Check Python Version
python3.7 -V
```
## Install pip (optional)
```sh
python3.7 -m pip install pip
```
## Install virtualenv (optional)
```sh
## Install virtualenv using pip
pip3.7 install virtualenv
python3.7 -m virtualenv $Env
```
Create a new virtual environment:
```sh
python3.7 -m venv env
```
Activate the virtual environment:
```sh
source env/bin/activate
```
Activate the virtual environment:
```sh
deactivate
```

## Troubleshooting
#### Unsupported locale setting:
```
locale.Error: unsupported locale setting
```
Resolution:
```sh
export LC_ALL=C
```