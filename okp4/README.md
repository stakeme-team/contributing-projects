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
| RPC       | http://okp4.stakeme.pro:29657      |
| Peer      | a6fc531f7274aa6615fa33198496ea69b2023a0f@okp4.stakeme.pro:29656      |

## Download addrbook
Our system update every 12 hours
```sh
ADDRBOOK_NAME=$(curl -s http://okp4.stakeme.pro:8080/public/ | egrep -o ">okp4_addrbook.*\.json" | tr -d ">")
curl -s http://okp4.stakeme.pro:8080/public/$ADDRBOOK_NAME > $HOME/.okp4d/config/addrbook.json
```

## Snapshot
Update every 6 hours
```sh
sudo systemctl stop okp4d
cp $HOME/.okp4d/data/priv_validator_state.json $HOME/.okp4d/priv_validator_state.json.backup
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book
rm -rf $HOME/.okp4d/data
SNAPSHOT_NAME=$(curl -s http://okp4.stakeme.pro:8080/public/ | egrep -o ">okp4_snapshot.*\.tar.lz4" | tr -d ">")
curl -s http://okp4.stakeme.pro:8080/public/$SNAPSHOT_NAME | lz4 -dc - | tar -xf - -C $HOME/.okp4d
mv $HOME/.okp4d/priv_validator_state.json.backup $HOME/.okp4d/data/priv_validator_state.json
sudo systemctl restart okp4d
```

**Automate** install node: https://t.me/stakeme_bot
