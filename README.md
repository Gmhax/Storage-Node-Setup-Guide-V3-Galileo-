# Storage-Node-Setup-Guide-V3-Galileo-
This guide will help you set up a Storage Node for OG Labs. For official documentation, check here: https://docs.0g.ai/run-a-node/storage-node


# Requirements#
- Memory: 32 GB RAM
- CPU: 8 Cores
- Disk: 500GB / 1TB NVME SSD
- Bandwidth: 100 Mbps (Download / Upload)

# VPS 3 needed you can purchase here: https://contabo.com/en/vps/cloud-vps-8c/?addons=1854&image=ubuntu.332&qty=1&contract=12&storage-type=ssd-storage-extension-vps-8-cores

- Kung may Vps 3 kana at merun pang 16GB+ ram pa at merun 6GB+ CPU goods payan e-run sa 0g Storage,

# First request faucet: https://faucet.0g.ai/


## INSTALLATION

## 1. Install necessary packages
```bash
sudo apt-get update
sudo apt-get install clang cmake build-essential openssl pkg-config libssl-dev jq
```

## 2. Install go
```bash
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version
```

## 3. Install rustup
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```bash
. "$HOME/.cargo/env"
```

## 4. Download and Install 0G Storage Node
```bash
git clone -b v1.0.0 https://github.com/0glabs/0g-storage-node.git
```
```bash
cd $HOME/0g-storage-node
git stash
git fetch --all --tags
git checkout v1.0.0
git submodule update --init
cargo build --release
```

## 5. Set config
```bash
rm -rf $HOME/0g-storage-node/run/config.toml
curl -o $HOME/0g-storage-node/run/config.toml https://cdn.bangcode.id/0g/v3_config.toml
```

## nano storage-node
```bash
nano $HOME/0g-storage-node/run/config.toml
```
- Check network_boot_nodes and input this:
```
["/ip4/47.251.117.133/udp/1234/p2p/16Uiu2HAmTVDGNhkHD98zDnJxQWu3i1FL1aFYeh9wiQTNu4pDCgps","/ip4/47.76.61.226/udp/1234/p2p/16Uiu2HAm2k6ua2mGgvZ8rTMV8GhpW71aVzkQWy7D37TTDuLCpgmX","/ip4/47.251.79.83/udp/1234/p2p/16Uiu2HAkvJYQABP1MdvfWfUZUzGLx1sBSDZ2AT92EFKcMCCPVawV", "/ip4/47.238.87.44/udp/1234/p2p/16Uiu2HAmFGsLoajQdEds6tJqsLX7Dg8bYd2HWR4SbpJUut4QXqCj", "/ip4/47.251.78.104/udp/1234/p2p/16Uiu2HAmSe9UWdHrqkn2mKh99b9DwYZZcea6krfidtU3e5tiHiwN", "/ip4/47.76.30.235/udp/1234/p2p/16Uiu2HAm5tCqwGtXJemZqBhJ9JoQxdDgkWYavfCziaqaAYkGDSfU"]
```
- Locate the miner_key and enter your "private key". also remove the # to activate (the text will turn green).

## 6. Create service
```bash
sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

## 7.Start service
```bash
sudo systemctl daemon-reload && sudo systemctl enable zgs && sudo systemctl start zgs
```


## Command to check ur 0g-storage node
- Check Full Logs
```bash
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)
```
- Check Blocks and Peers
```bash
source <(curl -s https://astrostake.xyz/check_block.sh)
```


## Done ma bro!!










