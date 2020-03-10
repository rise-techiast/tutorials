# Yarn Setup Guideline

## Install Yarn
Import the repository’s GPG key: 
```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```
Update the package list:
```sh
sudo apt update
```
Install Yarn:
```sh
## If you already have Node.js installed then use this command:
sudo apt install --no-install-recommends yarn

## If you haven't, use this command which will install recommended Node.js:
sudo apt install yarn
```
Verify Yarn:
```sh
yarn --version
```

## Basic Usage
#### Create a new Project
```sh
yarn init $PROJECT_NAME
```
#### Install all project dependencies
```sh
yarn install
```
#### Run a project
```sh
yarn run
```
#### Add dependency
```sh
yarn add $DEPENDENCY_NAME
```
#### Upgrade dependency
```sh
yarn upgrade $DEPENDENCY_NAME
yarn upgrade $DEPENDENCY_NAME@$VERSION_OR_TAG
```
#### Remove dependency
```sh
yarn remove $DEPENDENCY_NAME
```

## Troubleshooting
#### No such file or directory: 'install'
```
ERROR: [Errno 2] No such file or directory: 'install'
```
Resolution:
```sh
sudo apt remove cmdtest
sudo apt remove yarn
sudo npm install -g yarn
```