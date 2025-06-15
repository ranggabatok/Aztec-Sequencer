# Aztec-Sequencer-Node

Langkah-langkah menjalankan Node Aztec-Sequencer dan mendapatkan Role Apprentice
---

### Minimal Spesifikasi
| RAM       | CPU         | Disk         |
|-----------|-------------|--------------|
| `8–16 GB` | `4–9 cores` | `100+ GB SSD` |
---

### 1. Install Dependecies
- Update Packages :
```
sudo apt-get update && sudo apt-get upgrade -y
```
- Install Packages :
```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```
- Install Docker :
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

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
```

### 2. Install Aztec Tools
```
bash -i <(curl -s https://install.aztec.network)
```
```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
```
```
source ~/.bashrc
```
- Tutup VPS nya lalu Buka lagi
---
### 3. Update Aztec
```
aztec-up latest
```
---
### 4. Enable Firewall & Open Ports
```
# Firewall
ufw allow 22
ufw allow ssh
ufw enable

# Sequencer
ufw allow 40400
ufw allow 8080
```
---
### 5. Run Sequencer-Node
- Bikin aztec Direktori :
```
mkdir aztec
```
- Masuk ke Direktori :
```
cd aztec
```
- Bikin .env :
```
nano .env
```
- Isi semua Data :
```
ETHEREUM_RPC_URL=https://sepolia-rpc.codeblocklabs.com
CONSENSUS_BEACON_URL=https://sepolia-consensus.codeblocklabs.com
VALIDATOR_PRIVATE_KEY=0xYourPrivateKey
COINBASE=0xYourAddress
P2P_IP=P2P_IP
```
- Ganti 0xYourPrivateKey dengan **PRIVATE KEY KITA**
- Ganti 0xYourAddress dengan **ADRESS KITA**

- Bikin docker-compose.yml :
```
nano docker-compose.yml
```
- Isi dengan kode ini :
```
services:
  aztec-node:
    container_name: aztec-sequencer
    network_mode: host 
    image: aztecprotocol/aztec:latest
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEY: ${VALIDATOR_PRIVATE_KEY}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: debug
    entrypoint: >
      sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network alpha-testnet --node --archiver --sequencer'
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8080:8080
    volumes:
      - /root/.aztec/alpha-testnet/data/:/data
```
- Run Node Docker :
```
docker compose up -d
```
- Cek Logs :
```
docker compose logs -fn 1000
```
---
### 6. Dapetin Role Apprentice
```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://ipvpsanda:8080 | jq -r ".result.proven.number"
```
- Ganti **ipvpsanda** dengan **IP VPS KITA**
- Nanti Muncul BLOCK, simpan dan masukan ke command di bawah ini
```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK_NUMBER","BLOCK_NUMBER"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```
- Ganti **BLOCK_NUMBER** dengaan Block yang udah simpan di atas tadi
- Simpan Proof nya
- Buka Discord Aztec
- Buka Channel operator | start-here
- Ketik /operator start
- Ganti Adress dengan Adress Kita
- Ganti Block Number dengan Block yang di simpan tadi
- Ganti Proof dengan Proof yang di simpan tadi
**Selamat kalian udah berhasil menjalankan Node Aztec-Sequencer dan dapat Role Apprentice**

### TERIMA KASIH UNTUK RPC GRATISNYA DARI CODEBLOCKLABS.COM
- https://t.me/codeblocklabs_chat
