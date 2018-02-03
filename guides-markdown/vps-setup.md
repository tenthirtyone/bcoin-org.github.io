# Full Node VPS Setup

```post-author
Alex Sherbuck
```

```post-description
This guide provides installation instructions for a Bcoin full node running on a Digital Ocean of Amazon AWS Virtual Private Server.
```

## Introduction

Running a full node requires your computer always be online and connected to the Bitcoin network. For most users, a VPS is an elegant 24/7 full node solution.

## Digital Ocean

1. Create an account at digitalocean.com
2. Choose 'Create' -> 'Droplet' from the account dashboard
3. Choose 'Ubuntu 16.04' for OS Distribution
4. Choose '4 GB 2 CPU $20/month' for Droplet Size
5. Add Block Storage, 500GB $50/month.

### A Note on hardware requirements
These hardware requirements are for a full node. When run in SPV node, you will not need the additional block storage. To run a full node you must maintain a complete history of all Bitcoin transactions. Without the full history the node is unable to validate transactions. If it cannot validate transactions, it cannot validate blocks or mine transactions to blocks.

SPV Nodes use something called a bloom filter to maintain a smaller set of records. For example, if your Bitcoin address were `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa` your SPV node will ask its network peers for all transactions with an address that starts with `1A1z`. The returned data set will contain extra transactions irrelevant to your wallet. But its storage requirements will be far smaller than a full node. Consequently, SPV Nodes can only verify their own transactions.

6. Choose a data center region.
7. Add a new ssh key

### Setting Up SSH
If you already have an ssh key you can copy your pubkey here. If you do not then follow these steps:

`ssh-keygen -t rsa -C "your_email@example.com"`

Accept the defaults.

On Windows your keys are located:

`cd %userprofile%/.ssh`

On Linux/Mac:

`cd ~/.ssh`

Copy the contents of the `id_rsa.pub` file and paste them into Digital Ocean's form.

`ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCsYKEA5LZCDyMF+ZbrPWeVIYso0ZzpIZx9L7R+CGqMPo0mrSlYeeaPbP1btM/Wis4a81EaTM7Y5kkKqZ4XB/LnRWp415XVl5QdtGF2l5tgiy2ootVxEwdrH0lXyGFHEpOwHU6MYdYCd+bgpQwa291Q4bOUJhGxNZ07L/rMtZfWhWL
+YL+JpSajg/uonu+4YKuFETggGLIuK+piTD9dvjiaThwKtqiCh2dnqdHztRYk+OehJUcof3tFl9kSRUmh9MVI7pDaOxCJWRaU1dsn9YaUwRkIyOwESHqBdCE9ZDU4FzNItRh2dYY4ukGv2iRqZoTrjcB8UGJepI65aINKNvdj email@nomail.com`

8. Choose a hostname for the droplet and click create.

Digital Ocean will provision your server. Now is a good time to grab coffee.


## Amazon AWS

1. Create an account at https://aws.amazon.com
2. Launch a new instance from the console https://aws.amazon.com/console
3. Choose 'Ubuntu 16.04' and at least 2CPU 4GiB for hardware.
4. Continue to add storage. Add 500GiB.

### A Note on hardware requirements
These hardware requirements are for a full node. When run in SPV node, you will not need the additional block storage. To run a full node you must maintain a complete history of all Bitcoin transactions. Without the full history the node is unable to validate transactions. If it cannot validate transactions, it cannot validate blocks or mine transactions to blocks.

SPV Nodes use something called a bloom filter to maintain a smaller set of records. For example, if your Bitcoin address were `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa` your SPV node will ask its network peers for all transactions with an address that starts with `1A1z`. The returned data set will contain extra transactions irrelevant to your wallet. But its storage requirements will be far smaller than a full node. Consequently, SPV Nodes can only verify their own transactions.

5. Review and Launch
6. Amazon will prompt you for an ssh keypair, download a new keypair.
7. Launch Instances.
8. View Instances

Amazon will provision your server. Now is a good time to grab coffee.


## Connecting to your server

#### Digital Ocean

`ssh Your.Server.Ip.Here`

E.g.

`ssh 178.62.124.90`

Accept the RSA key, and you will be at the command line


#### Amazon

In the same directory as the private key you downloaded,

```chmod 400 test.pem
ssh -i "test.pem" yourname@yourinstance.amazonaws.com
```

E.g.

`ssh -i "test.pem" ubuntu@ec2-18-219-26-103.us-east-2.compute.amazonaws.com`

Accept the RSA key, and you will be at the command line

`ubuntu@ip-172-31-7-194:~$ `

## Setting up the environment
The VPS is setup, from here the intructions will be the same regardless of VPS provider.

1. Install NVM
2. Install Node
4. Install build essential
5. Install python

```
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
$ source ~/.bashrc
$ nvm install 9.2.1
$ sudo apt-get install build-essential
$ sudo apt-get install python
```
## Install Bcoin

```
$ git clone git://github.com/bcoin-org/bcoin.git
$ cd bcoin
$ npm install
$ node docs/Examples/fullnode
```

You should begin to see blocks syncing as soon as the network peers pickup.