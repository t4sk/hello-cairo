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
