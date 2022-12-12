![nibiru-contribute](https://user-images.githubusercontent.com/79249177/206979648-04a9a592-8c20-4851-b64e-0fbb5f0cda27.png)
# Nibiru

Ecosystem: **cosmos** </br>
Details: unlock leverage at scale for the Cosmos ecosystem</br>

> Site: https://nibiru.fi </br>
> Discord: https://discord.com/invite/sgPw8ZYfpQ </br>
> Github: https://github.com/NibiruChain </br>
> Twitter: https://twitter.com/NibiruChain </br>
## Table projects
| Type      | Info      |
|-----------|-----------|
| Statesync | http://nibiru.stakeme.pro:36657 |
| RPC       | http://nibiru.stakeme.pro:36657       |
| Peer      | ae357e14309640ca33cde597b37f0a91e63a32bd@nibiru.stakeme.pro:36656      |

## Download addrbook
Our system update every 12 hours
```sh
ADDRBOOK_NAME=$(curl -s http://nibiru.stakeme.pro:8080/public/ | egrep -o ">nibiru_addrbook.*\.json" | tr -d ">")
curl -s http://nibiru.stakeme.pro:8080/public/$ADDRBOOK_NAME > $HOME/.nibid/config/addrbook.json
```

## Statesync
```sh
sudo systemctl stop nibid
nibid tendermint unsafe-reset-all --home $HOME/.nibid --keep-addr-book

SNAP_RPC="http://nibiru.stakeme.pro:36657"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.nibid/config/config.toml
sudo systemctl restart nibid
```

**Automate** install node: https://t.me/stakeme_bot
