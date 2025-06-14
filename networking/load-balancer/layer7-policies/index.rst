===============
Layer7 Policies
===============

General concept
---------------

When using either HTTP or HTTPS as :doc:`listener protocol <../general-concept/listeners>`, you
can use various features of the HTTP protocol directly in the load balancer.

This is enabled via Layer 7 Policies. A policy can execute some (basic) functionality:

- Reject, will result in HTTP/1.1 403 Forbidden reply.

- Redirect to pool, will send traffic to another listeners pool. This is a good thing if you
  already have a pool setup (from another listener) of the same servers and just want to add
  another listener (for example when adding HTTP protocol to an already configured HTTPS
  listener). Another really useful use case could be if you want to run a separate pool for
  your static content. You can then opt to send for example traffic to "www.example.com/js" or
  "/images" to separate backends that are optimised (maybe via caching) for static content.

- Redirect to URL, this will redirect (using a header: location) the request to another
  URL. Useful when for example requiring HTTPS.

The policies will only triggered once you setup a corresponding Layer 7 Rule on them. The rule
decides when the policy should trigger. Various *types* of triggers are available:

- Hostname (for example ``example.com``)

- Patch (for example ``/images``)

- File type (for example ``.jpeg``)

- Header (for example ``x-forwarded-for``)

- Cookie (for example ``phpsessid``)

The types take keys when relevant (when specifying for example cookie, you could specify the
cookie ``auth_token``). The type can be compared by certain methods:

- ``REGEX`` (regular expressions)

- ``EQUALS_TO`` (value is exactly compared)

- ``STARTS_WITH`` or ``ENDS_WITH``

- ``CONTAINS`` (value contains)

Finally, a value is specified, for example when comparing hostname, ``example.com`` could be
specified.

The entire evaluation can also be negated, for example when looking for a non logged in
user, you might redirect to login when **not** finding a cookie.

If the Layer7 rule is met, the Layer7 policy will trigger.

.. tip::

   The official OpenStack support pages have some good examples of Layer 7 policies being used.

   The `CLI documentation <https://docs.openstack.org/python-octaviaclient/latest/cli/index.html#l7policy>`__ is
   somewhat comprehensive, for some use cases see the
   `layer 7 cookbook <https://docs.openstack.org/octavia/queens/user/guides/l7-cookbook.html>`__.

Creating policies
-----------------

To create a Layer 7 policy in Binero cloud, you have three main options as outlined in the links
below. Each option have its pros and cons:

- :doc:`The cloud management portal <cloud-management-portal>` is recommended and will get
  a user with limited prior knowledge from A to B quickly. The trade-off is that advanced
  features are not always available.

- :doc:`OpenStack Horizon <openstack-horizon>` is the web interface included in OpenStack. Some
  advanced features might only have a GUI implementation here.

- :doc:`The OpenStack terminal client <openstack-terminal-client>` is a command line implementation
  giving terminal oriented users a quick way to access the cloud. The learning curve is steeper than
  the GUI implementation but the workflow will be efficient.

.. toctree::
  :caption: Available services
  :maxdepth: 2

  cloud-management-portal
  openstack-horizon
  openstack-terminal-client
