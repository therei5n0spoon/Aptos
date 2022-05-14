# AptosIncentivizedTestNet-Guide
How do I join the Incentive Testnet?

## <a name='System Requirements'></a>System requirements
**CPU**: 4 cores
**Memory**: 8GiB RAM

## Step 1
**First, let's set the screen and variables.**
```
sudo apt install screen
```
```
screen -S aptos
```
```
echo "export WORKSPACE=testnet" >> $HOME/.bash_profile
```
```
echo "export PUBLIC_IP=$(curl -s ifconfig.me)" >> $HOME/.bash_profile
```
```
source $HOME/.bash_profile
```

## Step 2
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt-get install jq unzip -y
```

## Step 3
```
sudo apt-get install ca-certificates curl gnupg lsb-release -y
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```
```
mkdir -p ~/.docker/cli-plugins/
```
```
curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
```
```
chmod +x ~/.docker/cli-plugins/docker-compose
```
```
sudo chown $USER /var/run/docker.sock
```
## Step 4
```
wget -qO aptos-cli.zip https://github.com/aptos-labs/aptos-core/releases/download/aptos-cli-v0.1.1/aptos-cli-0.1.1-Ubuntu-x86_64.zip
```
```
unzip aptos-cli.zip -d /usr/local/bin
```
```
chmod +x /usr/local/bin/aptos
```
```
rm aptos-cli.zip
```
## Step 5
```
mkdir ~/$WORKSPACE && cd ~/$WORKSPACE
```
```
wget -qO docker-compose.yaml https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/docker-compose.yaml
```
```
wget -qO validator.yaml https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/validator.yaml
```
```
wget -qO fullnode.yaml https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/fullnode.yaml
```

## Step 6
```
aptos genesis generate-keys --output-dir ~/$WORKSPACE
```
```
aptos genesis set-validator-configuration \
  --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
  --username thereisnospoon \
  --validator-host $PUBLIC_IP:6180 \
  --full-node-host $PUBLIC_IP:6182
```
## Step 7
```
mkdir keys
```
```
aptos key generate --output-file keys/root
```
## Step 8
```
tee layout.yaml > /dev/null <<EOF
---
root_key: "0x5243ca72b0766d9e9cbf2debf6153443b01a1e0e6d086c7ea206eaf6f8043956"
users:
  - thereisnospoon
chain_id: 23
EOF
```
## Step 9
```
wget -qO framework.zip https://github.com/aptos-labs/aptos-core/releases/download/aptos-framework-v0.1.0/framework.zip
```
```
unzip framework.zip
```
```
rm framework.zip
```
```
aptos genesis generate-genesis --local-repository-dir ~/$WORKSPACE --output-dir ~/$WORKSPACE
```

 # Final Step
 ```
 docker compose up -d
 ```
 
 ## **Please check your node**
  ```
 https://aptos-node.info/
  ```
 
