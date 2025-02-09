.. _docker:

Docker
======

From https://www.docker.com/what-docker:

    Docker is the world’s leading software container platform. Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers. Operators use Docker to run and manage apps side-by-side in isolated containers to get better compute density. Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux, Windows Server, and Linux-on-mainframe apps.

Download
--------

Our ISO image includes the Docker engine and all of our Docker images.

Security
--------

To prevent tampering, our Docker images are signed using GPG keys. :ref:`soup` verifies GPG signatures any time Docker images are updated.

Elastic
-------

To maintain a high level of stability, reliability, and support, our Elastic Docker images are based on the Docker images provided by Elastic.co.

Images
------

After installation, you can see all Docker images with the following command:

::

    sudo docker images
    
Logs
----

If a service is not writing its logs to ``/opt/so/log``, then you may need to check the Docker logs for more detail. For example, to check the Docker logs for :ref:`kibana`:

::

    sudo docker logs so-kibana
    
Registry
--------

The manager node runs a Docker registry. From https://docs.docker.com/registry/recipes/mirror/:

    If you have multiple instances of Docker running in your environment (e.g., multiple physical or virtual machines, all running the Docker daemon), each time one of them requires an image that it doesn’t have it will go out to the internet and fetch it from the public Docker registry. By running a local registry mirror, you can keep most of the redundant image fetch traffic on your local network.

If you see errors relating to ``so-dockerregistry`` (Docker Registry), then please take a look at the following discussions to see if your symptoms match and if their solutions may help you:

https://github.com/Security-Onion-Solutions/securityonion/discussions/12078

https://github.com/Security-Onion-Solutions/securityonion/discussions/12635

Networking and Bridging
-----------------------

By default, Docker configures its network bridge with an IP address of ``172.17.0.1``. This works fine for networks that aren't already using the ``172.17.0.0/16`` range. If you are using this range in your network, then you can change the Docker range during installation.

.. image:: images/16_setup_docker_range.png
  :target: _images/16_setup_docker_range.png
    
Containers
----------

Our Docker containers all belong to a common Docker bridge network, called ``sobridge``. Each container is also aliased, so that communication can occur between the different docker containers using said alias. For example, communication to the ``so-elasticsearch`` container would occur through an alias of ``elasticsearch``.

You may come across interfaces in ``ifconfig`` with the format ``veth*``. These are the external interfaces for each of the Docker containers. These interfaces correspond to internal Docker container interfaces (within the Docker container itself).

To identify which external interface belongs to which container, we can do something like the following:

From the host, type:

::

   sudo docker exec so-elasticsearch cat /sys/class/net/eth0/iflink

This should provide you with a value with which you can grep the host ``net`` class ``ifindex(es)``:

| **Example:**
| ``grep 25 /sys/class/net/veth*/ifindex | cut -d'/' -f5``

You should then receive some output similar to the following:

``vethc5ff027``

where ``vethc5ff027`` is the external interface of ``eth0`` within the ``so-elasticsearch`` container.

VMware Tools
------------

If you have VMware Tools installed and you suspend and then resume, the Docker interfaces will no longer have IP addresses and the Elastic stack will no longer be able to communicate. One workaround is to remove ``/etc/vmware-tools/scripts/vmware/network`` to prevent VMware suspend/resume from modifying your network configuration.

More Information
----------------

.. note::

    For more information about Docker, please see https://www.docker.com/what-docker.
