# Automatic Installation

```bash
source <(curl -s https://itrocket.net/api/testnet/mantra/autoinstall/)
```

### Create wallet <a href="#create-wallet" id="create-wallet"></a>

```bash
# to create a new wallet, use the following command. don’t forget to save the mnemonic
mantrachaind keys add $WALLET

# to restore exexuting wallet, use the following command
mantrachaind keys add $WALLET --recover

# save wallet and validator address
WALLET_ADDRESS=$(mantrachaind keys show $WALLET -a)
VALOPER_ADDRESS=$(mantrachaind keys show $WALLET --bech val -a)
echo "export WALLET_ADDRESS="$WALLET_ADDRESS >> $HOME/.bash_profile
echo "export VALOPER_ADDRESS="$VALOPER_ADDRESS >> $HOME/.bash_profile
source $HOME/.bash_profile

# check sync status, once your node is fully synced, the output from above will print "false"
mantrachaind status 2>&1 | jq .SyncInfo

# before creating a validator, you need to fund your wallet and check balance
mantrachaind query bank balances $WALLET_ADDRESS
```

### Create validator <a href="#create-validator" id="create-validator"></a>

Moniker Identity DetailsAmount.

```bash
mantrachaind tx staking create-validator \
--amount 1000000uaum \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(mantrachaind tendermint show-validator) \
--moniker "test" \
--identity "" \
--details " YOUR NODE " \
--chain-id mantrachain-testnet-1 \
--gas auto --gas-adjustment 1.5 --fees 50uaum \
-y
```

### Delete node <a href="#delete" id="delete"></a>

```bash
sudo systemctl stop mantrachaind
sudo systemctl disable mantrachaind
sudo rm -rf /etc/systemd/system/mantrachaind.service
sudo rm $(which mantrachaind)
sudo rm -rf $HOME/.mantrachain
sed -i "/MANTRA_/d" $HOME/.bash_profile
```
