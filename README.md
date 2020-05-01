# DEVOPS P3 VAGRANT

## Installation

MacOS:

```sh
$ brew cask install virtualbox
$ brew cask install vagrant
```

Ubuntu / Debian:

```sh
$ sudo apt install virtualbox
$ sudo apt update
$ curl -O https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb
$ sudo apt install ./vagrant_2.2.6_x86_64.deb
```

Verify Installation:

```sh
$ vagrant --version
```

## Usage
Website resources are automatically cloned from `https://github.com/adyassine/p3-devops-docker` into `p3-devops.com`
```sh
$ vagrant up
$ vagrant ssh
$ curl localhost:8080
$ ssh root@localhost -p 8022 # pwd : root
```


[![asciicast](https://asciinema.org/a/226229.png)](https://asciinema.org/a/QIpBwip7jgSJMgWi4sq7bUIvO)