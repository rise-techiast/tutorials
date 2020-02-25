# Build a Private EOS Blockchain
This is a step-by-step guideline to setup a private EOS blockchain.

Note: if you want to automate the below procedure, you can use this python script alternatively.

## Summary
We are going to follow the below procedure to create a private EOS blockchain:
1. [Setup a Wallet](#1-setup-a-wallet): using `keosd`, a key management command line tool for storing private keys and signing transactions. For development purposes, we will store private keys of all required accounts in the same wallet.
2. [Build the Genesis Node](#2-build-the-genesis-node): the initial block producer node that will be used to deploy, initiate and configure EOS system smart contracts for our blockchain. 
3. [Transition from single Genesis Producer to Multiple Producers](#3-transition-from-single-genesis-producer-to-multiple-producers): delegate the block production responsibility from our genesis node to multiple nodes within the same blockchain network. 
4. [Resign eosio account and system accounts](#4-resign-eosio-account-and-system-accounts): set the keys of system smart contract accounts to null so that our blockchain can function in a decentralized manner based on the consensus rules.

## Prerequisites
For this guideline, please make sure you have installed the below dependencies with the corresponding version:
* [EOSIO v1.8](https://techiast.com/eosio-v1-8/)
* [EOSIO.cdt v1.6](https://techiast.com/eosio-v1-6/)
* [EOSIO contracts v1.8](https://techiast.com/eosio-contracts-v1-8/)

## 1. Setup a wallet
Create a new wallet
```sh
cleos wallet create --to-console
```

```
Creating wallet: default
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5KC8otxqtEsVUUYdy6nqPuW63v5z8Nwwg3Wtje3HP5CCNzZhjMP"
```
Open the wallet
```sh
cleos wallet open
```
Unlock the wallet
```sh
cleos wallet unlock
```
Paste the above generated password and press enter.

Generate a pair of Public Key and Private Key
```sh
cleos create key --to-console
```

```
Private key: 5JhtdjUdCEAtvsM3oxwt88UTD6uviPJQcJYG5EjVtB4pSavX2du
Public key: EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
```

Import Private Key to the wallet
```sh
cleos wallet import --private-key
```
Enter the above generated Private Key.
## 2. Build the Genesis node
### Create the genesis node
Clone master branch
```sh
git clone https://github.com/tuan-tl/eos-node
cd eos-node
```
Build the scripts and directory
```sh
./build.sh
```
Input based on the following details or your own configurations:
```
1) Block Producer
Genesis Key (the above generated Public Key): EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
Producer Name: eosio
Public Key (the above generated Public Key): EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
Private Key (the above generated Private Key): 5JhtdjUdCEAtvsM3oxwt88UTD6uviPJQcJYG5EjVtB4pSavX2du
HTTP Request Endpoint: 127.0.0.1:8888
P2P Listen Endpoint: 127.0.0.1:9010
P2P Peering Address: localhost:9011,localhost:9012,localhost:9013
```
### Start the genesis node
Start a genesis node by executing `genesis_start.sh`
```sh
cd node
./genesis_start.sh
```
Inspect `nodeos.log` file to see block producing status
```sh
tail -f ./blockchain/nodeos.log
```
### Create system accounts
Here is a list of system accounts to be generated
```
eosio.bpay
eosio.msig
eosio.names
eosio.ram
eosio.ramfee
eosio.saving
eosio.stake
eosio.token
eosio.vpay
eosio.rex
```
Create keys for ```eosio.bpay``` account
```sh
cleos create key --to-console
```

Import generated Private Key to the wallet
```sh
cleos wallet import --private-key
```

Create ```eosio.bpay``` account
```sh
cleos create account eosio eosio.bpay EOS84BLRbGbFahNJEpnnJHYCoW9QPbQEk2iHsHGGS6qcVUq9HhutG
```
Repeat the above steps to the rest accounts.
### Build eosio.contracts
Clone the repository
```sh
cd ~
git clone https://github.com/EOSIO/eosio.contracts
cd ./eosio.contracts/
./build.sh
```
Install the ```eosio.token``` contract
```sh
cleos set contract eosio.token $CONTRACTS_DIRECTORY/eosio.contracts/build/contracts/eosio.token/
```
Install the ```eosio.msig``` contract
```sh
cleos set contract eosio.msig $CONTRACTS_DIRECTORY/eosio.contracts/build/contracts/eosio.msig/
```
### Create and allocate the SYS currency
Create the SYS currency with a maximum value of 10 billion tokens. Then issue one billion tokens. Replace SYS with your specific currency designation.

Create SYS currency
```sh
cleos push action eosio.token create '[ "eosio", "10000000000.0000 SYS" ]' -p eosio.token@active
```
Allocate SYS currency to ```eosio``` account
```sh
cleos push action eosio.token issue '[ "eosio", "1000000000.0000 SYS", "memo" ]' -p eosio@active
```
### Activate ```eosio.system``` contract
```sh
cleos set contract eosio $CONTRACTS_DIRECTORY/eosio.contracts/build/contracts/eosio.system/
```

## 3. Transition from single genesis producer to multiple producers
### Delegate `eosio` account's privilege to smart contract accounts
Make ```eosio.msig``` a privileged account
```sh
cleos push action eosio setpriv '["eosio.msig", 1]' -p eosio@active
```
Initialize system account
```sh
cleos push action eosio init '["0", "4,SYS"]' -p eosio@active
```
### Create staked accounts
We are going to create the following staked accounts
```
accountnum11 has 8KB of RAM, 100000000 SYS on CPU and NET.
accountnum12 has 8KB of RAM, 100000000 SYS on CPU and NET.
accountnum13 has 8KB of RAM, 100000000 SYS on CPU and NET.
```
Create a staked account with initial resources and public key
```sh
cleos create key --to-console
```
Import generated Private Key to the wallet
```sh
cleos wallet import --private-key
```
Create ```accountnum11``` account using the above generated Public Key
```sh
cleos system newaccount eosio --transfer accountnum11 $EOS_PUB_KEY_OF_accountnum11 --stake-net "100000000.0000 SYS" --stake-cpu "100000000.0000 SYS" --buy-ram-kbytes 8192
```
Register the new account as a producer using the above generated Public Key
```sh
cleos system regproducer accountnum11 $EOS_PUB_KEY_OF_accountnum11 https://accountnum11.com $EOS_PUB_KEY_OF_accountnum11
```
Repeat the above steps for `accountnum12` and `accountnum13`

### List producers
Run this command to list producers
```sh
cleos system listproducers
```
If everything has been setup properly, the following result should show up:
```
Producer        Producer key                    Scaled votes
accountnum11    $EOS_PUB_KEY_OF_accountnum11    0.0000
accountnum12    $EOS_PUB_KEY_OF_accountnum12    0.0000
accountnum13    $EOS_PUB_KEY_OF_accountnum13    0.0000
```
### Set up and start producers
Set up a new producer using the previously created accountnum11 account
```sh
mkdir accountnum11
git clone https://github.com/tuan-tl/eos-node
cd eos-node
./build.sh
```
Input based on the following details
```
Genesis Key (the Public Key of genesis node): EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
Producer Name: accountnum11
Public Key (the Public Key of accountnum11): $EOS_PUB_KEY_OF_accountnum11
Private Key (the Private Key of accountnum11): $EOS_PRIV_KEY_OF_accountnum11
HTTP Request Endpoint: 127.0.0.1:8011
P2P Listen Endpoint: 127.0.0.1:9011
P2P Peering Address: localhost:9010,localhost:9012,localhost:9013
```
Start the producer node by executing the following commands
```sh
cd node
./genesis_start.sh

## To see block production status:
tail -f blockchain/nodeos.log
```
Repeat the above steps for `accountnum12` using the below inputs
```
1) Block Producer
Genesis Key (the Public Key of genesis node): EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
Producer Name: accountnum12
Public Key (the Public Key of accountnum12): $EOS_PUB_KEY_OF_accountnum12
Private Key (the Private Key of accountnum12): $EOS_PRIV_KEY_OF_accountnum12
HTTP Request Endpoint: 127.0.0.1:8011
P2P Listen Endpoint: 127.0.0.1:9012
P2P Peering Address: localhost:9010,localhost:9011,localhost:9013
```
Repeat the above steps for `accountnum13` using the below inputs
```
1) Block Producer
Genesis Key (the Public Key of genesis node): EOS6nSXgwnwge4rHrwmixycjFPC8AayjcRXTv8R3SVEgtfw8woYQi
Producer Name: accountnum13
Public Key (the Public Key of accountnum13): $EOS_PUB_KEY_OF_accountnum13
Private Key (the Private Key of accountnum13): $EOS_PRIV_KEY_OF_accountnum13
HTTP Request Endpoint: 127.0.0.1:8013
P2P Listen Endpoint: 127.0.0.1:9013
P2P Peering Address: localhost:9010,localhost:9011,localhost:9012
```
### Vote for each of the block producers to get started in block production
```sh
cleos system voteproducer prods accountnum11 accountnum11 accountnum12 accountnum13
```
## 4. Resign eosio account and system accounts
Set the keys of the `eosio.*` accounts to null
```sh
cleos push action eosio updateauth '{"account": "eosio", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": "active"}}]}}' -p eosio@owner
cleos push action eosio updateauth '{"account": "eosio", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": "active"}}]}}' -p eosio@active
```
Resign system accounts by running the following commands
```sh
cleos push action eosio updateauth '{"account": "eosio.bpay", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.bpay@owner
cleos push action eosio updateauth '{"account": "eosio.bpay", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.bpay@active

cleos push action eosio updateauth '{"account": "eosio.msig", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.msig@owner
cleos push action eosio updateauth '{"account": "eosio.msig", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.msig@active

cleos push action eosio updateauth '{"account": "eosio.names", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.names@owner
cleos push action eosio updateauth '{"account": "eosio.names", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.names@active

cleos push action eosio updateauth '{"account": "eosio.ram", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.ram@owner
cleos push action eosio updateauth '{"account": "eosio.ram", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.ram@active

cleos push action eosio updateauth '{"account": "eosio.ramfee", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.ramfee@owner
cleos push action eosio updateauth '{"account": "eosio.ramfee", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.ramfee@active

cleos push action eosio updateauth '{"account": "eosio.saving", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.saving@owner
cleos push action eosio updateauth '{"account": "eosio.saving", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.saving@active

cleos push action eosio updateauth '{"account": "eosio.stake", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.stake@owner
cleos push action eosio updateauth '{"account": "eosio.stake", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.stake@active

cleos push action eosio updateauth '{"account": "eosio.token", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.token@owner
cleos push action eosio updateauth '{"account": "eosio.token", "permission": "active", "parent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.token@active

cleos push action eosio updateauth '{"account": "eosio.vpay", "permission": "owner", "parent": "", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.vpay@owner
cleos push action eosio updateauth '{"account": "eosio.vpay", "permission": "active", "pa;l./////////////;;;;-rent": "owner", "auth": {"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": "active"}}]}}' -p eosio.vpay@active
```
## References
* _BIOS Boot Sequence_, EOS Developers, viewed 25th Feb 2020, <<https://developers.eos.io/welcome/latest/tutorials/bios-boot-sequence>>.