### Reset data
```
lavad tendermint unsafe-reset-all
```

### Download Genesis File terbaru
```
cd /.lava/config
rm genesis.json
wget https://raw.githubusercontent.com/lavanet/lava-config/main/testnet-2/genesis_json/genesis.json
```

### Download lavad terbaru
```
cd ~/lava-config/testnet-2
rm lavad
wget -O lavad https://github.com/lavanet/lava/releases/download/v0.21.1.2/lavad-v0.21.1.2-linux-amd64; chmod +x lavad
rm /usr/local/bin/lavad
cp lavad /usr/local/bin
```
### Restart
```
sudo systemctl stop lavad
sudo systemctl restart lavad
```

### Set Statesync agar cepat sync
- Sauce [Nodes Guru](https://lava.explorers.guru/state-sync)

- Set Variable
```
daemon_name="lavad"
statesync_rpc="https://rpc-1.lava.nodes.guru"
rpc_servers="https:\/\/rpc-1.lava.nodes.guru:443,https:\/\/rpc-1.lava.nodes.guru:443"
statesync_homedir="$HOME/.lava"
height_offset=2000
```

- Dapatkan data blocks
```
latest_height=$(curl -s $statesync_rpc/block | jq -r .result.block.header.height)
height=$((latest_height - $height_offset))
hash=$(curl -s "$statesync_rpc/block?height=$height" | jq -r .result.block_id.hash)
```

- Peer
```
peer_status=$(curl -s "$statesync_rpc"/status | jq .result)
peer_node_id=$(echo $peer_status | jq .node_info.id | sed 's/"//g')
striped_rpc=$(echo $statesync_rpc | sed 's/https\?:\/\///')
peer_port=$(echo $peer_status | jq .node_info.listen_addr | sed 's/"//g' | awk -F'[/:]' '{print $NF}')
peer_id="${peer_node_id}@${striped_rpc}:${peer_port}"
```

- Setup
```
sed -i '/\[statesync\]/{:a;n;/enable/s/false/true/;Ta;}' $statesync_homedir/config/config.toml
sed -i.bak -e "s/rpc_servers *=.*/rpc_servers = \"$rpc_servers\"/g" $statesync_homedir/config/config.toml
sed -i.bak -e "s/trust_height *=.*/trust_height = \"$height\"/g" $statesync_homedir/config/config.toml
sed -i.bak -e "s/trust_hash *=.*/trust_hash = \"$hash\"/g" $statesync_homedir/config/config.toml
sed -i "s/^persistent_peers *=.*/persistent_peers = \"$peer_id\"/;" $statesync_homedir/config/config.toml
```

- Clear database
```
sudo systemctl stop $daemon_name
unsafe_reset_response=$($daemon_name unsafe-reset-all --home $statesync_homedir 2>&1)
if echo "$unsafe_reset_response" | grep -q -E "Error|error"; then
    $daemon_name tendermint unsafe-reset-all --home $statesync_homedir --keep-addr-book
fi
```

- Restart service
```
sudo systemctl restart $daemon_name
sudo journalctl -u $daemon_name -f -o cat
```
![Screenshot_19](https://github.com/Megumiiiiii/lava-testnet-2/assets/98658943/a930a070-5d8f-481d-94bc-19f4402ee206)

