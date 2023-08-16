<div align="center">
 
# Lava Network

</div>

<div align="center">

[![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/Megumiiiiii)

 <img align="top" src="https://komarev.com/ghpvc/?username=Megumiiiiii&color=ff69b4&style=plastic&label=Visitors" height='35'/>

</div>

#

<div align="center">
  
## ${\color{violet} COPAS \space SERTAIN \space SUMBER \space SU}$

### ${\color{violet} DIKIRA \space BIKIN \space GINIAN \space GAK \space PERLU \space USAHA \space APA}$ 

</div>

## Official Links

- [Discord](https://discord.gg/4RsKJEMD)
- [Twitter](https://twitter.com/lavanetxyz)
- [Website](https://www.lavanet.xyz/)
- [Docs](https://docs.lavanet.xyz/)


### Update Sistem
```
sudo apt update -y; sudo apt upgrade -y
```

### Install alat & bahan
```
sudo apt install -y unzip logrotate git jq sed wget curl coreutils systemd
```

### Install GO
```
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.19.10.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)
```

### Clone Repo
```
git clone https://github.com/lavanet/lava-config.git
cd lava-config/testnet-2
```

```
source setup_config/setup_config.sh
```

### Setup
```
echo "Lava config file path: $lava_config_folder"
mkdir -p $lavad_home_folder
mkdir -p $lava_config_folder
cp default_lavad_config_files/* $lava_config_folder
```

### Set file genesis
```
cp genesis_json/genesis.json $lava_config_folder/genesis.json
```

# NEXT!

### Set node
```
lavad_binary_path="$HOME/go/bin/"
mkdir -p $lavad_binary_path
```

### Download genesis binary
```
wget https://lava-binary-upgrades.s3.amazonaws.com/testnet-2/genesis/lavad
chmod +x lavad
```

### Verify path
```
cp lavad /usr/local/bin
```

### Cek apakah berhasil terpasang
```
lavad --help
```
JIka muncul list command = aman

### Create Service
```
echo "[Unit]
Description=Lava Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which lavad) start --home=$lavad_home_folder --p2p.seeds $seed_node
Restart=always
RestartSec=180
LimitNOFILE=infinity
LimitNPROC=infinity
[Install]
WantedBy=multi-user.target" >lavad.service
sudo mv lavad.service /lib/systemd/system/lavad.service
```

## Start Service
```
sudo systemctl daemon-reload
sudo systemctl enable lavad.service
sudo systemctl restart systemd-journald
sudo systemctl start lavad
```

### Cek logs
```
sudo journalctl -u lavad -f
```
![Screenshot_99](https://github.com/Megumiiiiii/lava-testnet-2/assets/98658943/e2c5eacb-a323-431c-acf2-1984cdfcb155)

= GUD

### Cek status sync
```
lavad status | jq .SyncInfo.catching_up
```
Tunggu false

# NEXT


### Create wallet
```
lavad keys add wallet
```
backup pharse, copas address

### Req Faucet
- Ke Discord channel #faucet
- Request dengan command `$request addressLavaMu`

# Setelah SYNC

### Create Validator
```
lavad tx staking create-validator \
    --amount="50000000ulava" \
    --pubkey=$(lavad tendermint show-validator --home "$HOME/.lava/") \
    --moniker="NamaMu" \
    --chain-id=lava-testnet-2 \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="10000" \
    --details "BEBASSUKAHATI" \
    --identity "IdentityMu" \
    --website "websiteMu" \
    --gas="auto" \
    --gas-adjustment "1.5" \
    --gas-prices="0.05ulava" \
    --home="$HOME/.lava/" \
    --from=wallet
```

## SUDAH

### Jika Ingin membaca docs lebih lanjut
[Docs Official](https://docs.lavanet.xyz/validator-manual)

#

<div id="header" align="center">
  <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMzNmZTIxZmE3ZmY3MzRiMDcwNDJhYTQ5ZmNlY2YxMWE1OWIyYmVkNSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/mVBlqOD4ra9jQiI3cC/giphy.gif" height="125" width="420"/>
</div>
