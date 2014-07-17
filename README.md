# Vagrant Debian Proxy/Cache

This project is a simple Vagrant configuration to run a local Debian package proxy/cache. The active component is [Squid deb proxy](https://launchpad.net/squid-deb-proxy). I've found this very handy when creating/destroying many local VMs. The apt-get update process can get very slow depending on your network connection. With a local package cache, the speeds for subsequent updates are much faster.

See this article for more details and background: [Ubuntu deb proxy and cache: squid-deb-proxy and apt-cacher-ng](http://www.garron.me/en/blog/ubuntu-deb-proxy-cache.html).

## Prerequisites

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
1. Install [Vagrant](https://www.vagrantup.com/downloads)
1. Check that both are installed and reachable from a command line:

        $ vagrant --version
        Vagrant 1.6.0
        $ VBoxManage --version
        4.3.12r93733

## Installation

1. Clone this repository

        git clone https://github.com/bcantoni/vagrant-deb-proxy.git
        cd vagrant-deb-proxy

1. The default static IP is 10.211.54.100. Edit the `CFG_IP` value if you want something different.

1. Start the cache VM

        vagrant up

1. In the provisioning script for your other VMs, include these steps (adjusting IP address if you've changed it):

        # install and configure for local debian proxy (if present)
        apt-get install squid-deb-proxy-client -y
        echo 'Acquire::http::Proxy "http://10.211.54.100:8000/";' | sudo tee /etc/apt/apt.conf.d/30autoproxy

1. Now start your other VMs as normal. They should start using this VM as a proxy/cache during any `apt-get` commands.

## Notes:

* On the proxy VM, you can tail the log `/var/log/squid-deb-proxy/access.log` to make sure everything is working with cache hits/misses. The cached package files will be stored under `/var/cache/squid-deb-proxy`.
