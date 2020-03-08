# Register a new SSH key to GitHub Account
This is a step-by-step guideline to register a new SSH key to your GitHub account.

## Generate a new SSH key
Open terminal and type in the below command:
```sh
ssh-keygen -t rsa -b 4096 -C "$YOUR_GITHUB_EMAIL_ADDRESS"
```
You will be prompted for a response as of below, just hit "Enter" and move to the next step:
```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/tuantran/.ssh/id_rsa):
```
Enter a secure passphrase, which will be used for later authentication:
```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```
At the end of the process, you will receive a sample below result:
```
Your identification has been saved in /Users/tuantran/.ssh/id_rsa.
Your public key has been saved in /Users/tuantran/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:wyBhrepbm1zHbaX/VQMnQNHKKE4US0DHXTG+OsSfABk tuantran@Tuans-MacBook-Pro.local
The key's randomart image is:
+---[RSA 2048]----+
|    ++oEo oB+    |
|   . .+o+.. o.   |
|    ..o+  o..o . |
|    .. =o. o. +  |
|   .  o S+ ..  ..|
|  .    o.o+o.   o|
| .  . . oo+o    .|
|  .o + . ...   . |
|  ..+       ...  |
+----[SHA256]-----+
```
## Add SSH key to `ssh-agent`
SSH agent is a protocol that automatically handles signing of any authentication request from your remote servers. Basically, it caches your private keys and requires you to enter the passphrase once only; which reduces the amount of time you need to enter passphrase for each authentication attempt.

Start `ssh-agent` in the background:
```sh
eval "$(ssh-agent -s)"
```
Add your generated SSH private key to the ssh-agent:
```sh
ssh-add -K ~/.ssh/id_rsa
```
For Mac users, you need to modify `~/.ssh/config` to automatically load keys into the ssh-agent:
```sh
nano ~/.ssh/config
```
Paste in the below details:
```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```
## Add SSH key to your GitHub account
1. Navigate to this url https://github.com/settings/ssh/new, or you can access from the UI following the below steps:
   1. In the upper-right corner of any page, click your profile photo, then click `Settings`.
   1. In the user settings sidebar, click `SSH and GPG keys`.
   1. Click `New SSH key or Add SSH key`.
 
1. In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".
1. Paste your key into the "Key" field.
1. Click Add SSH key.
1. If prompted, confirm your GitHub password.

## Test your SSH connection
Enter the following command:
```sh
ssh -T git@github.com
```
You will receive the below response then type "yes":
```
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
```
After that, the below message indicates that you have successfully authenticated:
```
Hi tuan-tl! You've successfully authenticated, but GitHub does not provide shell access.
```

## References
* _Generating a new SSH key and adding it to the ssh-agent_, GitHub Help, viewed 17 Feb 2020, ><https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>>.
* _Adding a new SSH key to your GitHub account_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account>>.
* _Testing your SSH connection_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/authenticating-to-github/testing-your-ssh-connection>>.
