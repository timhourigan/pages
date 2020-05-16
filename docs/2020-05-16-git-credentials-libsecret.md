# Git credentials with libsecret

How to setup libsecret as a git credentials helper on Ubuntu 20.04.

```shell
# Install necessary packages
$ sudo apt-get install libsecret-1-dev 

# Make
$ sudo make --directory=/usr/share/doc/git/contrib/credential/libsecret
# `git-credential-libsecret` executable now exists
$ ls  /usr/share/doc/git/contrib/credential/libsecret/
git-credential-libsecret  git-credential-libsecret.c  git-credential-libsecret.o  Makefile

# Setup credential store in git
git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret

# Additional information
# Ubuntu version
$ cat /etc/lsb-release 
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04 LTS"
# Git version
$ git --version
git version 2.25.1
```

## Related Information

How to setup a Personal Access Token in Github:

https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line


## Sources
https://stackoverflow.com/a/40312117
https://wiki.gnome.org/Projects/Libsecret
