Bitcoind for Docker
===================

Docker image that runs a bitcoind node in a container for easy deployment.


Requirements
------------

* Physical machine, cloud instance, or VPS that supports Docker (i.e. [Vultr](http://bit.ly/vultrbitcoind), [Digital Ocean](https://bit.ly/dobitcoind), KVM or XEN based VMs) running Ubuntu 14.04 or later (*not OpenVZ containers!*)
* At least 40 GB to store the block chain files
* At least 1 GB RAM + 2 GB swap file

Recommended and tested on [Vultr 1024 MB RAM/320 GB disk instance @ $8/mo](http://bit.ly/vultrbitcoind).  Vultr also *accepts Bitcoin payments*!  May run on the 512 MB instance, but took *forever* (1+ week) to initialize due to swap and disk thrashing.


Really Fast Quick Start
-----------------------

One liner for Ubuntu 14.04 LTS machines with JSON-RPC enabled on localhost and adds upstart init script:

    curl https://raw.githubusercontent.com/kylemanna/docker-bitcoind/master/bootstrap-host.sh | sh -s trusty


Quick Start
-----------

1. Run a Bitcoin node and use the data volume:

        docker run -v /var/data/bitcoind:/bitcoin --name=bitcoind-node -d \
          -p 8333:8333 \
          -p 8332:8332 \
          -p 6881:6881 \
          -p 6882:6882 \
        kylemanna/bitcoind

2. Verify that the container is running and waiting for bitcoind node to
   catch up with network

        $ docker ps
        CONTAINER ID        IMAGE                       COMMAND                CREATED             STATUS              PORTS                                              NAMES
        b595e548bfa8        kylemanna/bitcoind:latest   "bitcoind -rpcallowi   6 minutes ago       Up 6 minutes        0.0.0.0:6881->6881/tcp, 0.0.0.0:6882->6882/tcp, 0.0.0.0:8332->8332/tcp, 0.0.0.0:8333->8333/tcp   bitcoind-node

3. You can then access the daemon's output thanks to the [docker logs command]( https://docs.docker.com/reference/commandline/cli/#logs)

        docker logs -f bitcoind-node

4. Install optional init script for upstart provided @ `upstart.init`.


Enable JSON-RPC
---------------

The following Docker run line will create a container with JSON-RPC enabled and will only allow Docker host to access the JSON RPC port 8332.

    $ docker run --volumes-from=bitcoind-data --name=bitcoind-node -d -p 8333:8333 -p 127.0.0.1:8332:8332 kylemanna/bitcoind bitcoind -disablewallet -rpcallowip=*


Documentation
-------------

* Additional documentation in the [docs folder](docs).


Todo
----

- [ ] Review possiblity of bootstraping blockchain via BitTorrent
