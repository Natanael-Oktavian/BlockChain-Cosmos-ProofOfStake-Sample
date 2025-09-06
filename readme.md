# natancoin
**natancoin** is a blockchain built using Cosmos SDK and Tendermint and created with [Ignite CLI](https://ignite.com/cli).

## Get started

```
ignite chain serve
```

`serve` command installs dependencies, builds, initializes, and starts your blockchain in development.

### Configure

Your blockchain in development can be configured with `config.yml`. To learn more, see the [Ignite CLI docs](https://docs.ignite.com).

### Web Frontend

Additionally, Ignite CLI offers a frontend scaffolding feature (based on Vue) to help you quickly build a web frontend for your blockchain:

Use: `ignite scaffold vue`
This command can be run within your scaffolded blockchain project.


For more information see the [monorepo for Ignite front-end development](https://github.com/ignite/web).

## Release
To release a new version of your blockchain, create and push a new tag with `v` prefix. A new draft release with the configured targets will be created.

```
git tag v0.1
git push origin v0.1
```

After a draft release is created, make your final changes from the release page and publish it.

### Install
To install the latest version of your blockchain node's binary, execute the following command on your machine:

```
curl https://get.ignite.com/username/natancoin@latest! | sudo bash
```
`username/natancoin` should match the `username` and `repo_name` of the Github repository to which the source code was pushed. Learn more about [the install process](https://github.com/ignite/installer).

###
Prerequisites:
- Install go, cosmos sdk, ignite cli

ignite scaffold chain natancoin --address-prefix natancoin

cd natancoin    

ignite chain build

natancoind init mynode --chain-id natancoin-1

natancoind keys add alice --keyring-backend test

- address: natancoin14eztkklkglmgvqpu0uhycxmyzve99x66dpeqrv
  name: alice
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A8XaV/LuAg61SFnoAmng8gI4OeRFUTDABLTY4hJoTq4h"}'
  type: local


natancoind keys add bob --keyring-backend test

- address: natancoin1cg37zemv3dezdg4geju68xwx7ly0yfwte2nz7w
  name: bob
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A0WEHvX+VF3BEOdOq/toIarVuuorbs4B5pkKqK6eQ+t/"}'
  type: local


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

shaft goose lottery seat differ night neither speak retire rural ostrich couch side east general talent stock cluster timber pretty chest kiss nasty future


natancoind genesis add-genesis-account $(natancoind keys show alice -a --keyring-backend test) 2000000000000unatancoin

natancoind genesis add-genesis-account $(natancoind keys show bob -a --keyring-backend test) 500000000000unatancoin

natancoind genesis gentx alice 100000000000unatancoin --chain-id natancoin-1 --keyring-backend test

natancoind genesis collect-gentxs

natancoind genesis validate-genesis

Set minimum-gas-prices = "2000unatancoin" in .natancoin/app.toml

Change all “stake” to “unatancoin” in .natancoint/genesis.json

natancoind start   

SMOKE TEST

Check account list and address : 

natancoind keys list --keyring-backend test

natancoind keys show alice -a --keyring-backend test

natancoind keys show bob -a --keyring-backend test

Check balance : 

natancoind query bank balances $(natancoind keys show alice -a --keyring-backend test) -> 1.9 M natancoin (100K used for validators/stake)

natancoind query bank balances $(natancoind keys show bob -a --keyring-backend test) -> 500 K natancoint

- Send 1K natancoin from bob to Alice (400 Natancoin fee)

 natancoind tx bank send \
  natancoin1cg37zemv3dezdg4geju68xwx7ly0yfwte2nz7w \
  natancoin14eztkklkglmgvqpu0uhycxmyzve99x66dpeqrv \
  1000000000unatancoin \
  --fees 400000000unatancoin \
  --keyring-backend test \
  --chain-id natancoin-1 \
  -y

Check balance after tx : 

natancoind query bank balances $(natancoind keys show alice -a --keyring-backend test) -> 1.901.000 natancoin (1K received from bob)

natancoind query bank balances $(natancoind keys show bob -a --keyring-backend test) -> 498600 natancoin (burn 1.4 K (1K sent and 400 fee))


Check Alice validator balance

natancoind keys show alice --bech val -a --keyring-backend test

natancoinvaloper14eztkklkglmgvqpu0uhycxmyzve99x6649ec6h

Query validator info

natancoind query staking validator natancoinvaloper14eztkklkglmgvqpu0uhycxmyzve99x6649ec6h -o json

"tokens": "100000000000",
 "delegator_shares": "100000000000.000000000000000000"

Query Alice’s self-delegation

ALICE=$(natancoind keys show alice -a --keyring-backend test)

VAL=$(natancoind keys show alice --bech val -a --keyring-backend test)

natancoind query staking delegation $ALICE $VAL

Query rewards

natancoind query distribution rewards $ALICE $VAL


Delegate token (Bonding)

natancoind tx staking delegate $VAL 5000000unatancoin \
  --from bob \
  --fees 400000000unatancoin \
  --gas auto --gas-adjustment 1.4 \
  --yes --keyring-backend test \
  --chain-id natancoin-1

Check delegation status

BOB=$(natancoind keys show bob -a --keyring-backend test)
natancoind query staking delegation $BOB $VAL

Bob coin is now 498.195 Natancoin

Withdraw reward

natancoind tx distribution withdraw-rewards $VAL --from bob \
  --fees 400000000unatancoin \
  --commission=false --yes --keyring-backend test\
  --chain-id natancoin-1


Reset the state

natancoind tendermint unsafe-reset-all --home ~/.natancoin


Check the balance after reset

natancoind query bank balances $(natancoind keys show alice -a --keyring-backend test) -> 1.9 M natancoin (100K used for validators/stake)

natancoind query bank balances $(natancoind keys show bob -a --keyring-backend test) -> 500 K natancoint


## Learn more

- [Ignite CLI](https://ignite.com/cli)
- [Tutorials](https://docs.ignite.com/guide)
- [Ignite CLI docs](https://docs.ignite.com)
- [Cosmos SDK docs](https://docs.cosmos.network)
- [Developer Chat](https://discord.com/invite/ignitecli)
