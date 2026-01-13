Access and Host Configuration
=============================

To use any SCION application, you must have access to a SCION network and have the SCION endhost stack installed on your host.
Depending on the SCION network you are targeting, refer to the corresponding section below.

For the following instructions, we assume that the SCION daemon runs on the default address.
If it does not, specify the address of the SCION daemon to which you want to attach the application by setting the ``SCION_DAEMON_ADDRESS`` environment variable, e.g.:

.. code-block:: console

  export SCION_DAEMON_ADDRESS=127.0.0.1:30255

SCION production network
------------------------
The SCION production network is a global real-world network that provides secure and reliable communication.

In order to access the SCION production network, your network provider must provide SCION connectivity for your host, e.g.:

- You are a customer of an ISP that provides SCION connectivity.
- Your university or research institution is part of the SCIERA ISD (see `SCIERA docs <https://sciera.readthedocs.io/en/latest/index.html>`_) or another SCION ISD.

Setting up the SCION endhost stack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can install and configure the SCION endhost stack for **Linux, macOS, or Windows** using the `SCION orchestrator for endhosts <https://github.com/netsys-lab/scion-orchestrator>`_.

Alternatively, you can use the `SCION endhost bootstrapper <https://github.com/netsec-ethz/bootstrapper>`_ to fetch the SCION configuration.
On Debian systems, the bootstrapper will also install the SCION endhost stack for you.
For other platforms, you will need to manually install the SCION endhost stack. Please refer to the following sections for more information.

.. note::
  Both tools require your network provider to enable a `SCION bootstrapper service <https://github.com/netsys-lab/bootstrap-server>`_.
  If it is not available, follow the alternative host installation instructions below.

.. warning::
  The SCION endhost stack is not officially supported on Windows, but it can be built and run with some limitations.
  Mainly, the dispatcher is not supported on Windows, but you can run SCION applications in environments that do not require the dispatcher.
  This is applicable if your network provider runs SCION version > 0.11.0, available from the `Releases <https://github.com/scionproto/scion/releases>`_.

Alternative Linux Host Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If your network provider does not provide a SCION bootstrapper service, you can manually install the Debian packages (see `SCION Installation <https://docs.scion.org/en/latest/manuals/install.html#installation>`_).
Otherwise, you can also follow the instructions to build the SCION endhost stack from source (see `SCION Build <https://docs.scion.org/en/latest/dev/build.html#build>`_).

Additionally, you will require a valid configuration file from your network provider located at ``/etc/scion/topology.json``.
Depending on the applications you want to use, you may also need the TRC files under ``/etc/scion/certs/``.

Alternative macOS Host Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::
  Homebrew support will be available soon.

For now, you can build the SCION endhost stack from source (see `SCION Build <https://docs.scion.org/en/latest/dev/build.html#build>`_).
Depending on where you compile the binaries, you may need to specify ``GOOS=darwin`` and ``GOARCH=amd64`` (or your target architecture).

Alternative Windows Host Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can build the SCION endhost stack from source (see `SCION Build <https://docs.scion.org/en/latest/dev/build.html#build>`_).
Depending on where you compile the binaries, you may need to specify ``GOOS=windows`` and ``GOARCH=amd64`` (or your target architecture).


SCIONLab network
----------------
The SCIONLab network is a global testbed (not production) that runs as SCION as an overlay network protocol. 
It is used for experimental purposes, although one can deploy real applications on it. 
It is free to use and open to everyone, but one cannot expect the same level of reliability, performance and security as the SCION production network.

In order to access the SCIONLab network, you must have a SCIONLab account and have set up a SCIONLab node (see https://docs.scionlab.org/).

The SCIONLab node already comes with a SCION endhost stack, meaning that you can run SCION applications directly on the node.
Otherwise, you can follow the previous instructions to install the SCION endhost stack on your host and connect to the SCIONLab node.


Local SCION network for development
-----------------------------------
To set up a local SCION network for development, you must have a development environment set up (see https://docs.scion.org/en/latest/dev/setup.html).

If you have a running development environment, you can run the SCION applications on your host.
You need to specify the address of the SCION daemon to which you want to attach the application to, using the ``SCION_DAEMON_ADDRESS`` environment variable.
The SCION daemon addresses for the different ASes can be found in their corresponding ``sd.toml`` configuration files in the ``gen/ASx`` directories.
