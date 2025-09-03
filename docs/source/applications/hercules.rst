Hercules
***********

Hercules enables high-speed file transfer over the SCION network.

The file transfer system, *Hercules*, has integrated multipath capabilities and congestion control, 
enabling efficient bulk transfer in high-speed local and inter-domain networks. 
To achieve high-performance network throughput, Hercules employs Linux’s express data path
(XDP) socket type to bypass the traditional in-kernel network stack and achieve high transfer rates
from user space. Furthermore, Hercules uses PCC for congestion control to achieve consistently
high performance. 
More technical details and design decision can be found in the `Hercules paper <https://netsec.ethz.ch/publications/papers/gartner-hercules-2023.pdf>`_.
Note however, that Hercules development is ongoing and therefore the current version differs from the one in the paper.

Hercules is open-souce and its repository can be found at: https://github.com/netsec-ethz/hercules.
The build process is documented there.


Setup and Configuration
=========================
In this secion we will show how Hercules can be configured. We show two potential deployments.

1. Stand Alone Deployment: We control two machines (in possibly different ASes) that want to exchange data using Hercules.
2. SCIERA: We want to exchange data using Hercules as part of the `SCIERA <https://sciera.readthedocs.io/en/latest/>`_ network.


Stand Alone Deployment
------------------------
We assume we have two machines in different ASes that want to exchange data. Our example configurations will refer to such a deployment as shown in the figure below. Both machines have SCION connectivity.

.. image:: hercules_setup.svg

Hercules constists of two separate processes (`hercules-monitor` and `hercules-server`) that both read from a shared config file. 
Before starting the processes we need to populate the configuration file `hercules.conf`. For Hercules host `A` a simple configuration could look like this.

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

When Hercules is running on both hosts we can finally start a transfer.
We start a transfer from host `A` to host `B` by using the Hercules command line tool (``hcp``) on host `A`.

.. code-block:: bash

    # move to the directory where the hcp tool resides
    cd ./hercules/hcp

    # start the transfer
    ./hcp localhost:8000 /home/ubuntu/__source_filename__ 1-ff00:2:2,10.0.2.2:10000 /home/user/__destination_filename__



SCIERA
------------------------

Configuring a hercules host and starting a transfer works the same in the SCIERA network as described for a stand alone deployment above.
However, the SCIERA network already contains multiple Hercules hosts that can be used for transferring files. 
If you are part of the SCIERA network this might make transferring files over SCION even easier.
Feel free to contact us, if you are interested in this.