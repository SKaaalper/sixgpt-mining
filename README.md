# sixgpt-mining

Before start:
You must have logged into [https://sixgpt.xyz](https://sixgpt.xyz) with your wallet & email before running the miner
Make sure the wallet associated with your vana private key has enough $VANA balance on the desired network (at least 0.1) 

Claim Faucet here: [VANA faucet](https://faucet.vana.org/moksha)

## SixGPT Official Account:
- [Twitter](https://x.com/sixgpt)
- [Discord](https://discord.com/invite/4WVt25pbpG)
- [Website](https://sixgpt.xyz/)

**SixGPT** is a decentralized synthetic data generation platform built on the [Vana network Official Twitter](https://x.com/withvana).

The SixGPT Miner is a software package which allows users to contribute data they generate for wikipedia question-answer based tasks to the network for rewards. In the future, we will support other tasks such as chatbot conversations, image captioning, etc.

### Install Docker:

```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo chmod +x /usr/local/bin/docker-compose
```

### Docker version check:
```
docker --version
```

## Install sixgpt

1. Create folders
```
mkdir sixgpt && cd sixgpt
```

2. Set Variables - Replace `your_private_key`
```
nano .env
```
```
export VANA_PRIVATE_KEY=your_private_key
export VANA_NETWORK=moksha
```
- To save: `CTRL` + `X` + `Y` + `ENTER`

3. Create `docker-compose.yml`
```
nano docker-compose.yml
```

- enter the following
```
version: '3.8'

services:
  ollama:
    image: ollama/ollama:0.3.12
    ports:
      - "11435:11434"
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped
 
  sixgpt3:
    image: sixgpt/miner:latest
    ports:
      - "2015:2000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=${VANA_PRIVATE_KEY}
      - VANA_NETWORK=${VANA_NETWORK}
    restart: always

volumes:
  ollama:
```
- To save: `CTRL` + `X` + `Y` + `ENTER`

## Start miner
```
docker compose up -d
```

## To check `Logs`
```
docker compose logs -fn 100
```

![Banner](https://github.com/SKaaalper/sixgpt-mining/blob/main/six.png)
