==========
Networking
==========

Networking is a core function in the In Binero cloud platform. It features a complete suite of virtual
networking functions ranging from routing, security groups (firewall) and high speed internet access to
VPNs (Virtual Private Networks) and load balancers.

The two main methods to connect to the internet is to either assign a :doc:`directly attached IP <directly-attached-ips>`
to your instance or setup a :doc:`router <router/index>` and connect a :doc:`subnet <subnet/index>`
to the router and your instance, then use a :doc:`floating IP <floating-ips>` to your instance (which is
the recommended approach).

The general design of networking in the platform is that :doc:`instances </compute/index>` use :doc:`ports <ports>` to
connect to :doc:`networks <network/index>`.

Networks in turn, have :doc:`subnets <subnet/index>` that assigns IP addresses to :doc:`ports <ports>` used by
:doc:`instances </compute/index>`.

:doc:`Floating IP addresses <floating-ips>` are publicly routed IP addresses mapped (1:1) to a port to either
publish services on the internet or provide servers with access to the internet. 

See the sections in this article to get a good understanding of the networking possibilities
in the platform.

If you are looking to get started with networking, we recommend our :doc:`getting started guide <../getting-started/launching-an-instance>`
that will guide you through setting up a versatile solution by using a single subnet behind a router. 

.. toctree::
  :caption: Available services
  :maxdepth: 2

  network/index
  subnet/index
  ports
  security-groups/index
  router/index
  floating-ips
  directly-attached-ips
  mtu
  load-balancer/index
  reaching-your-instances
  client-vpn/index
  site-to-site-vpn/index
  custom-firewall-router
  reverse-dns-ptr
  regions-and-availability-zones
  network-api
