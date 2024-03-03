## Installation

  

**1. Preparations — Install Build Tools**

  

```bash

sudo apt update && sudo apt upgrade -y

sudo apt -qy install curl git jq lz4 build-essential

```

**2. Install Docker & Docker Compose**

```bash
sudo apt install docker.io

docker --version

sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

**3. Clone The Starter Repository**

```bash
# Clone locally:
git clone https://github.com/ritual-net/infernet-container-starter

# Navigate to the repository
cd infernet-container-starter
```

**4. Run the hello-world container**
```bash
project=hello-world make deploy-container

ctr+C

docker ps
```

**5. Change The Configuration**

***Ensuring your wallet has around $5~10 of ETH on the Basechain for transactions.***
```bash
goto: https://dashboard.alchemy.com/apps
```

![enter image description here](https://raw.githubusercontent.com/kennode048/ritual/main/1.png)

![enter image description here](https://raw.githubusercontent.com/kennode048/ritual/main/2.png)

![enter image description here](https://raw.githubusercontent.com/kennode048/ritual/main/3.png)

```bash
nano ~/infernet-container-starter/deploy/config.json
```
Replace the `rpc_url` field with the Alchemy app's HTTPS URL.

Change the `private_key` field to your Ethereum wallet's private key, begins with `0x`.

Change the `coordinator_address`: `0x8D871Ef2826ac9001fB2e33fDD6379b6aaBF449c`.

Pressing `CTRL + X`, then `Y` to confirm the changes, and `Enter` to exit.

**6. Initialize The New Configuration**
```bash
docker restart anvil-node  
docker restart hello-world  
docker restart deploy-node-1  
docker restart deploy-fluentbit-1  
docker restart deploy-redis-1
```
Check that `rpc_url` and `public wallet address` are the same as the previous config:
```bash
docker logs deploy-node-1 2>&1 | grep -a "chain.rpc"
docker logs deploy-node-1 2>&1 | grep -a "chain.wallet"
```
**7. install Foundry And Dependencies**
```bash
# Change to home directory:  
cd  
  
# Create a new folder:  
mkdir foundry  
  
# Navigate into it:  
cd foundry  
  
# Execute the script from Foundry's official source  
curl -L https://foundry.paradigm.xyz | bash  
  
# Update your shell configuration by sourcing the .bashrc file  
source ~/.bashrc  
  
# Run foundryup to ensure Foundry is fully installed:  
foundryup

# Navigate to the contracts directory:  
cd ~/infernet-container-starter/projects/hello-world/contracts  
  
# Install the forge-std library:  
forge install --no-commit foundry-rs/forge-std  
  
# Install the infernet-sdk:  
forge install --no-commit ritual-net/infernet-sdk  
  
# Go to the directory three levels up:  
cd ../../../
```
**8. Deploy A Consumer Contract**
```bash 
# Navigate to the repository:  
cd ~/infernet-container-starter  
  
# Deploy the SaysGM contract:  
project=hello-world make deploy-contracts
```
**9. Call The Contract**
```bash
project=hello-world make call-contract
```
**10. On-Chain Registration**
```bash
goto: https://basescan.org/address/0x8d871ef2826ac9001fb2e33fdd6379b6aabf449c#writeContract
```
First, make sure you’re using the same Web3 wallet (like MetaMask) connected to your _config.json_ file. For registration, input your wallet’s public address into the “node (address)” field of the “registerNode” function and confirm the transaction. **After waiting for 1 hour**, proceed to activate your node using the “activateNode” function. This process is important for integrating your node with the network.
![enter image description here](https://raw.githubusercontent.com/kennode048/ritual/main/5.png)
```bash
goto: https://basescan.org/address/{your wallet address}
```
Check your wallet transactions with 2 method `Activate Node` and `Register Node` is successfully.

**11. Check The Docker Logs**
```bash
docker logs -f deploy-node-1
```
![enter image description here](https://raw.githubusercontent.com/kennode048/ritual/main/4.png)


Congratulations! 🎉 You have successfully created an on-chain subscription request!
