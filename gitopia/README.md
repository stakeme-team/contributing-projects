# Gitopia

Ecosystem: **cosmos** </br>
Details: Gitopia is the next-generation Code Collaboration Platform fuelled by 
a decentralized network and interactive token economy.
It is designed to optimize the open-source software 
development process through collaboration, transparency, 
and incentivization.</br>

> Site: https://gitopia.com/ </br>
> Discord: https://discord.gg/WujRarhaFV </br>
> Github: https://gitopia.com/gitopia </br>
> Twitter: https://twitter.com/gitopiaDAO </br>
## Table projects
| Type      | Info      |
|-----------|-----------|
| Statesync | http://gitopia.stakeme.pro:28657 |
| RPC       | http://gitopia.stakeme.pro:28657       |
| Peer      | bb6f0d3c55a6834037d545159869388bc498a5c7@gitopia.stakeme.pro:28656      |

## Download addrbook
Our system update every 24 hours
```sh
ADDRBOOK_NAME=$(curl -s http://gitopia.stakeme.pro:8080/public/ | egrep -o ">gitopia_addrbook.*\.json" | tr -d ">")
curl -s http://gitopia.stakeme.pro:8080/public/$ADDRBOOK_NAME > $HOME/.gitopia/config/addrbook.json
```

## Statesync
```sh
sudo systemctl stop gitopiad
gitopiad tendermint unsafe-reset-all --home $HOME/.gitopiad --keep-addr-book

SNAP_RPC="http://gitopia.stakeme.pro:28657"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.gitopia/config/config.toml

sudo systemctl restart gitopiad
```

**Automate** install node: https://t.me/stakeme_bot
