# Description

This role configures [LivePeer](https://livepeer.org/), an open-source service:

>Open Source Video Infrastructure Services, Built On The Ethereum Blockchain.

# Introduction

A __LivePeer__ node - which runs as a [docker container](https://hub.docker.com/r/statusteam/livepeer/) - exposes 3 ports:

* __CLI__ - Command line tool access, management API (`7935`)
* __HTTP__ - For streaming video to web and other media players (`8080`)
* __RTMP__ - For receiving media stream to transcode and broadcast (`1935`)

In general the workflow is as follows:

1. A source of media(video+audio) like OBS or else is set up.
2. The source streams to the __RTMP__ TCP port.
3. The LivePeer node runs transcoding if necessary.
4. The LivePeer node broadcasts the stream via the __HTTP__ port.

The __HTTP__ port is exposed via __HTTPS__ using Nginx using CloudFlare certificates.

# Services

This role deploys two services:

* [__LivePeer__ node](https://github.com/livepeer/go-livepeer) described above.
* [__LivePeer__ JS player](https://github.com/livepeer/livepeerjs/tree/master/packages/player) and website.

The proxy that exposes the JS player defaults to ports `80` and `443` in Nginx, you can see that in [`templates/nginx_proxy.conf.j2`](templates/nginx_proxy.conf.j2).

# Requirements

Due to being part of Status infra this role assumes availability of certain things:

* Docker for running containers
* Nginx full installation
* The `iptables-persistent` module
