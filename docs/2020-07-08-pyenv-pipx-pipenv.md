# pyenv, pipx and pipenv on 64 bit Ubuntu 20.04

How to install pyenv, pipx and pipenv on 64 bit Ubuntu 20.04.

## Install pyenv

```shell
# Update package info
$ sudo apt update
# Install prerequisites - https://github.com/pyenv/pyenv/wiki/Common-build-problems#prerequisites
$ sudo apt-get install build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git curl
# Install pyenv
$ curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
# Once complete, add the following to the shell rc (.bashrc, .zshrc etc.)
$ vi ~/.bashrc
export PATH="/$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
# Restart the shell
$ exec $SHELL
# Verify pyenv
$ pyenv --version
pyenv 1.2.20
```

### Install Python version with pyenv
```shell
# Install Python 3.8.5 with pyenv
$ pyenv install 3.8.5
Downloading Python-3.8.5.tar.xz...
-> https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tar.xz
Installing Python-3.8.5...
# Check available versions
$ pyenv versions
  3.8.5
# Check version before enabling 3.8.5
$ python3 --version
Python 3.8.2
# Set 3.8.5 as the global version in pyenv  
$ pyenv global 3.8.5 
# Verify the version has changed
$ python --version
Python 3.8.5
```

## Install pipx
```shell
# Install pipx via pip (controlled by pyenv)
$ pip install pipx
$ pipx --version
0.15.4.0
# Add "$HOME/.local/bin" to $PATH
pipx ensurepath
```

## Install pipenv
```shell
$ pipx list
nothing has been installed with pipx
$ pipx install pipenv
  installed package pipenv 2020.6.2, Python 3.8.5
  These apps are now globally available
    - pipenv
    - pipenv-resolver
done!
$ pipenv --version
pipenv, version 2020.6.2
```

## Resources
https://github.com/pyenv/pyenv-installer#installation--update--uninstallation
https://github.com/pyenv/pyenv
https://github.com/pipxproject/pipx
https://pipenv.pypa.io/en/latest/

