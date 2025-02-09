.. _network:

Network Visibility
==================

When you log into :ref:`soc`, you may see alerts from :ref:`suricata` or :ref:`idh`, protocol metadata logs from :ref:`zeek` or :ref:`suricata`, file analysis logs from :ref:`strelka`, or full packet capture from :ref:`stenographer` or :ref:`suricata`. How is that data generated and stored? This section covers the various processes that Security Onion uses to analyze and log network traffic.

.. image:: images/diagrams/sniffing.png
   :target: _images/sniffing.png

.. toctree::
   :maxdepth: 2

   af-packet
   bpf
   stenographer
   suricata
   zeek
   strelka
   idh
