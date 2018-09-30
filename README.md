## [LND](https://github.com/LightningNetwork/lnd) Docker Images

See [comodal/lnd-docker](https://hub.docker.com/r/comodal/lnd-docker/tags/) on Docker Hub for available images.

### Example Bitcoin Testnet Neutrino LND

```sh
> docker run -d\
 --name lnd-bitcoin-testnet\
 -p 9735:9735\
 -v lnd-bitcoin-testnet-data:/home/lnd/.lnd/data/\
 -v lnd-bitcoin-testnet-logs:/home/lnd/.lnd/logs/\
 comodal/lnd-docker:stretch-latest\
  --bitcoin.active\
  --bitcoin.testnet\
  --bitcoin.node=neutrino\
  --neutrino.connect=faucet.lightning.community\
  --lnddir=/home/lnd/.lnd\
  --datadir=/home/lnd/.lnd/data\
  --tlscertpath=/home/lnd/.lnd/data/tls.cert\
  --tlskeypath=/home/lnd/.lnd/data/tls.key\
  --logdir=/home/lnd/.lnd/logs
```

### Example CLI Usage

```sh
> docker exec -it lnd-bitcoin-testnet bash -l
>
> lncli\
 --macaroonpath=/home/lnd/.lnd/data/chain/bitcoin/testnet/admin.macaroon\
 --lnddir=/home/lnd/.lnd\
 --tlscertpath=/home/lnd/.lnd/data/tls.cert\
 --chain=bitcoin\
 --network=testnet\
 --help
```

You can create an alias

```sh
$ alias lncli="lncli --macaroonpath=/home/lnd/.lnd/data/chain/bitcoin/testnet/admin.macaroon --lnddir=/home/lnd/.lnd --tlscertpath=/home/lnd/.lnd/data/tls.cert --chain=bitcoin --network=testnet"
```
Now you can use lncli without parameters

### Create wallet on testnet
```sh
$ lncli create
```

## Get a new address, when synchronised (lncli getinfo show **"synced_to_chain": false**):
```sh
$ lncli newaddress np2wkh
{
    "address": "2N1cVnQE53bwuV9nCa84FbML66vwaDhCagf"
}
```

## Visit the faucet to get free tentnet BTC:
http://faucet.lightning.community

Copy the faucet node address and open a connection:
```
$ lncli connect 0270685ca81a8e4d4d01beec5781f4cc924684072ae52c507f8ebe9daf0caaab7b@159.203.125.125
```
Go back to faucet website and open a channel by providing:
* Target node: your identity_pubkey from **lncli getinfo** command.
* Channel ammount: 100000
* Initial balance: 50000 
After 3 confirmations this transaction will be mined. Check it on 
https://testnet.smartbit.com.au/tx/{your tx id from faucet}
Meanwhile observer on command line:
```sh
$ lncli pendingchannels
```
