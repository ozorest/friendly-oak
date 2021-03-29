+++
title = "[Fedora 32] How to solve docker internal network issue"
description = ""
tags = [
    "fedora",
    "docker",
    "firewalld",
]
date = "2020-06-11"
categories = [
    "Fedora",
]
menu = "main"
+++

Recently I upgraded to Fedora 32 with a fresh install and started to face an issue using docker-compose and docker in general, containers weren't able to talk each other.

After some googling I found that default backend for firewalld was changed [from iptables to nftables](https://fedoraproject.org/wiki/Changes/firewalld_default_to_nftables).

I tried to do the proposed fixes for Docker described in the link above, but without success, so the way to solve the issue for me was put back iptables as firewalld backend.

With those commands below, I was able to solve the issue.

```
sudo sed -i 's/FirewallBackend=nftables/FirewallBackend=iptables/g' /etc/firewalld/firewalld.conf

sudo systemctl restart firewalld docker
```

That's all folks, thanks in advance and stay in tune.