Hercules
***********

*Hercules* is a high-performance file transfer system built on SCION.

Hercules integrates multipath capabilities and congestion control to enable efficient, 
high-speed data transfer across both local and inter-domain network environments. 
To maximize throughput, Hercules leverages Linux's Express Data Path (XDP) sockets, 
bypassing the traditional in-kernal network stack and achieving line-rate transfers directly from user space. 
Furthermore, Hercules adopts Performance-oriented Congestion Control (PCC), 
enabling stable and consistent performance under dynamic network conditions.

More technical details and design decision can be found in the `Hercules paper <https://netsec.ethz.ch/publications/papers/gartner-hercules-2023.pdf>`_.
Please note, however, that Hercules is under active development, and the current version may differ from the one described in the paper.

Hercules is open source. The source code and build instructions can be found at: https://github.com/netsec-ethz/hercules


Setup and Configuration
=========================
In this secion we will show how Hercules can be configured. We show two potential deployments.

1. Stand Alone Deployment: Two machines (in possibly different ASes) exchange data using Hercules.
2. SCIERA: Hercules is used as part of the `SCIERA <https://sciera.readthedocs.io/en/latest/>`_ network.


Stand Alone Deployment
------------------------
We assume two machines, each in a different AS, both with SCION connectivity, want to exchange data. The setup is illustrated in the figure below.

.. image:: hercules_setup.svg

Hercules constists of two separate processes (`hercules-monitor` and `hercules-server`) that both read from a shared config file. 
Before starting the processes we need to populate the configuration file ``hercules.conf``. For Hercules host `A` a simple configuration could look as follows:

.. code-block:: TOML

    # SCION address the Hercules server should listen on
    ListenAddress = "1-ff00:1:1,10.0.1.1:10000"

    # Network interfaces to use for Hercules
    Interfaces = [
    "eth0",
    ]

    # Number of threads to use
    NumThreads = 4

    # Number of paths to use for a transfer
    DefaultNumPaths = 3

    # Destinations
    [[DestinationASes]]
    IA = "1-ff00:2:2"
    Payloadlen = 1200


The two Hercules processes can then be started.

.. code-block:: bash

    # move to the directory where Hercules is installed
    cd ./hercules

    # start the monitor process
    sudo ./hercules-monitor


.. code-block:: bash

    # move to the directory where Hercules is installed
    cd ./hercules

    # start the server process
    sudo ./hercules-server


The Hercules repository also contains ``.service`` files that allow to start Hercules with systemd.

Once Hercules is running on both hosts, you can initiate a transfer from Host A to Host B using the Hercules CLI tool:

.. code-block:: bash

    # move to the directory where the hcp tool resides
    cd ./hercules/hcp

    # start the transfer
    ./hcp localhost:8000 /home/user/__source_filename__ 1-ff00:2:2,10.0.2.2:10000 /home/user/__destination_filename__



SCIERA
------------------------

Running Hercules within the SCIERA network follows the same procedure as above. 
However, SCIERA already hosts multiple Hercules-enabled nodes that can be used for file transfers.

Contact us if you are interested in experiencing Hercules on SCIERA.
