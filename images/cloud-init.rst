==========
Cloud-init
==========

`Cloud-init <https://cloudinit.readthedocs.io/en/latest/>`_ is the industry standard
multi-distribution method for cross-platform cloud instance initialization.

It's supported across all major public cloud providers, provisioning systems for private
cloud infrastructure, and bare-metal installations.

During boot, cloud-init identifies the cloud it's running on and initialises the system
accordingly. Cloud instances will automatically provision during first boot with networking,
storage, SSH keys, packages and other system aspects already configured.

Cloud-init is powerful and users can send their own cloud-user data to cloud-init to
customized configuration. See the `examples here <https://cloudinit.readthedocs.io/en/latest/reference/examples.html>`_.

..  seealso::

    - :doc:`index`
