# Aztec-Sequencer-Node

Langkah-langkah menjalankan Node Aztec-Sequencer dan mendapatkan Role Apprentice
---

### Minimal Spesifikasi
| RAM       | CPU         | Disk         |
|-----------|-------------|--------------|
| `8–16 GB` | `4–9 cores` | `100+ GB SSD` |
---

### 1. Install Dependecies
```sudo apt-get update && sudo apt-get upgrade -y```
```sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen  -y```
```source <(wget -O - https://raw.githubusercontent.com/frianowzki/installer/main/docker.sh)```
```sudo groupadd docker && sudo usermod -aG docker $(whoami) && newgrp docker```
