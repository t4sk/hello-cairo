https://www.cairo-lang.org/

# Explorer

https://goerli.voyager.online/

# Install

```
# install pyenv

git clone https://github.com/pyenv/pyenv.git ~/.pyenv

echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

pyenv install 3.9.14
pyenv local 3.9.14

# install cairo
python -m venv ./venv
source ./venv/bin/activate

sudo apt install -y libgmp3-dev

pip3 install ecdsa fastecdsa sympy
pip3 install cairo-lang

# compile
cairo-compile test.cairo --output test_compiled.json

# execute
cairo-run \
  --program=test_compiled.json --print_output \
  --print_info --relocate_prints \
  --tracer
```

# Setup account

```
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount

starknet deploy_account
```

# Compile and deploy

```
starknet-compile contract.cairo \
    --output contract_compiled.json \
    --abi contract_abi.json

export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount

starknet declare --contract contract_compiled.json

# deploy contract
CLASS_HASH=0x68704d18de8ccf71da7c9761ee53efd44dcfcfd512eddfac9c396e7d175e234
starknet deploy --class_hash $CLASS_HASH

# interact with contract
CONTRACT_ADDR=0x058033a8754130de5ed44b74e956b3393dd04a333c1db061ba1b50c3cf640904
starknet invoke \
    --address $CONTRACT_ADDR \
    --abi contract_abi.json \
    --function increase_balance \
    --inputs 1234

# query
starknet call \
    --address $CONTRACT_ADDR \
    --abi contract_abi.json \
    --function get_balance

# transaction status
TX_HASH=0x691f959313c284f8d95599a6ead398d7d21511ab1d99c32de1e262f02810b35
starknet tx_status --hash $TX_HASH

```

### user auth example

```
starknet-compile user_auth.cairo \
    --output user_auth_compiled.json \
    --abi user_auth_abi.json

export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount

starknet deploy --contract user_auth_compiled.json --no_wallet

CONTRACT_ADDR=0x042438621596b7a4e388963205459facddb65f12e0764475f2727f5aaff3f5f1

starknet invoke \
    --address ${CONTRACT_ADDR} \
    --abi user_auth_abi.json \
    --function increase_balance \
    --inputs 4321

TX_HASH=0x21f2ddfe35c484e6985d1db4c0b03866c632d35141ff766297fa228b4772b2b

starknet tx_status --hash $TX_HASH

ACCOUNT=0x024c356efa03b4910b4645deb0a4401b5cbf7b5640ea8a1a161bcce099350ea7

starknet call \
    --address ${CONTRACT_ADDR} \
    --abi user_auth_abi.json \
    --function get_balance \
    --inputs ${ACCOUNT}
```

### constructor example

```shell
# compile
starknet-compile ownable.cairo \
    --output ownable_compiled.json \
    --abi ownable_abi.json

# upload code
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount

starknet declare --contract ownable_compiled.json

# deploy
starknet deploy --contract ownable_compiled.json --inputs 123 --no_wallet

```

# L1 L2

```
starknet-compile l1l2.cairo \
    --output l1l2_compiled.json \
    --abi l1l2_abi.json

starknet deploy --contract l1l2_compiled.json --no_wallet

CONTRACT_ADDR=0x02ccc2318b908ae1928cce871da57fdbcff4ce8bec5b4a0c3bff86b3f9e0f3bd

USERID="1"

# increase balance
starknet invoke \
    --address $CONTRACT_ADDR \
    --abi l1l2_abi.json \
    --function increase_balance \
    --inputs \
        $USERID \
        3333

# get balance
starknet call \
    --address $CONTRACT_ADDR \
    --abi l1l2_abi.json \
    --function get_balance \
    --inputs \
        $USERID

# withdraw
starknet invoke \
    --address $CONTRACT_ADDR \
    --abi l1l2_abi.json \
    --function withdraw \
    --inputs \
        $USERID \
        1000

# check tx status
TX_HASH=0x52526940df0fb89d702635fa48930431dc7ff8ff987c79bb5562c08470c7a39
starknet tx_status --hash $TX_HASH

# call withdraw on L1
contract address
https://goerli.etherscan.io/address/0x8359E4B0152ed5A731162D3c7B0D8D56edB165A0#writeContract

withdraw(
    $CONTRACT_ADDR,
    $USERID,
    1000
)

```
