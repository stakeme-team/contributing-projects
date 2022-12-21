![okp4](https://user-images.githubusercontent.com/79249177/208854327-9b8bbae8-72ac-4d21-ae7d-8a94a0051913.png)
# OKP4

Ecosystem: **cosmos** </br>
Details: The OKP4 project is one of is a domain-specific layer-1 dedicated to trust-minimized data sharing.</br>

> Site: https://okp4.network/ </br>
> Discord: https://discord.gg/okp4 </br>
> Github: https://github.com/okp4 </br>
> Twitter: https://twitter.com/OKP4_Protocol </br>
## Table projects
| Type      | Info     |
|-----------|----------|
| Statesync | http://okp4.stakeme.pro:29657 |
| RPC       | a6fc531f7274aa6615fa33198496ea69b2023a0f@okp4.stakeme.pro:29656     |
| Peer      | http://okp4.stakeme.pro:29657      |

## Download addrbook
Our system update every 12 hours
```sh
ADDRBOOK_NAME=$(curl -s http://okp4.stakeme.pro:8080/public/ | egrep -o ">okp4_addrbook.*\.json" | tr -d ">")
curl -s http://okp4.stakeme.pro:8080/public/$ADDRBOOK_NAME > $HOME/.okp4d/config/addrbook.json
```

## Statesync
```sh
sudo systemctl stop okp4d
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book

SNAP_RPC="http://okp4.stakeme.pro:29657"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.okp4d/config/config.toml
sudo systemctl restart okp4d
```

**Automate** install node: https://t.me/stakeme_bot
