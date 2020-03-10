# AWS CLI Setup Guideline

## Install pip
Follow this [guideline](./python.md) to install pip.

If you already have pip and a supported version of Python, you can skip this step and move forward.

## Install AWS CLI
Edit profile script:
```sh
sudo nano ~/.profile
```
Add the following export command:
```
export PATH=~/.local/bin:$PATH
```
Reload the profile:
```sh
source ~/.profile
```
Install AWS CLI packages:
```sh
## For pip
pip install awscli --upgrade --user

## For pip3
pip3 install awscli --upgrade --user
```
Check installed AWS CLI version:
```sh
aws --version
```
## Configure AWS CLI
For general usage, a profile should be configured using the below command:
```sh
aws configure
```
Below is a sample input details, which you can obtain from your AWS Management Console:
```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```
## References
* _Install the AWS CLI version 1_, AWS Command Line Interface User Guide, viewed 17 Feb 2020, <<https://docs.amazonaws.cn/en_us/cli/latest/userguide/install-macos.html>>.