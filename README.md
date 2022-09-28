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
