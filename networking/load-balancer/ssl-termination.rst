=======================================
SSL/TLS termination using Load Balancer
=======================================

General concept
---------------

When using SSL/TLS (which is standard today) to provide encryption for visitors to a web
application, managing the SSL/TLS certificates on web servers might be cumbersome.

When using a load balancer, an alternative is to publish the certificates on the load
balancer instead, as a single point of ingress into the system. The traffic, that is
the request the load balancer sends to a server, can be unencrypted as it never leaves
the platforms internal network. Encryption is then managed on the load balancer through
the platform and all instances managing the backend traffic can just run HTTP on port 80.

.. note::

   It's recommended to always encrypt your network traffic even if it's internal to
   your application, but to get you started using unencrypted traffic can help you
   get started faster.

Configuration
-------------

To setup HTTPS in the load balancer, you first need to have a certificate stored in our
security storage. More information is available in our
:doc:`/secret-store/create-cert-for-loadbalancing` article.

Once you have the certificate stored, you are able to follow either
:doc:`launching-a-loadbalancer/openstack-horizon` or
:doc:`launching-a-loadbalancer/openstack-terminal-client` to install
the load balancer.

Remember to select **TERMINATED_HTTPS** as protocol of the
:doc:`listener <general-concept/listeners>` as doing so will (in OpenStack Horizon)
enable the ``SSL Certificates`` tab to appear at the end, under which you are able
to select the certificate you've added.

.. note::

   This feature is only available in :doc:`/getting-started/managing-your-cloud/openstack-horizon`
   or :doc:`/getting-started/managing-your-cloud/openstack-terminal-client`.

Headers
-------

When you do termination of SSL/TLS in the load balancer, its important to remember that
the web application that receives the request will consider it to be un-encrypted.

It will also consider the request made out to whatever IP address that the instance that
receives the request (from the load balancer) is listening on since the request is *proxied*
(as opposed to *routed*).

To give your web application more information, there are a few headers that your load balancer
can insert:

- ``x-forwarded-port`` will be 443 and can be used to check that the traffic is indeed
  arriving on the encrypted port (if you also allow pure http traffic to the same instances).

- ``x-forwarded-proto`` works the same was as above but will give the protocol.

- ``x-forwarded-for`` will tell you to what IP, the visitor made the request. If you have
  several load balancers fronting the same instances, this will let you know through which
  load balancer the request was made.

The headers are available from the **Listener details** tab when setting up the load balancer
(or by editing the listener).

We recommend adding the headers when setting up a load balancer as the performance impact
is negligible and the information is useful for your application.

Requiring HTTPS
---------------

Web applications that have HTTPS should force the user to use the secure protocol.

If a user is not required to use HTTPS, its possible that they inadvertently send their
credentials over the (unencrypted) HTTP protocol.

Today most browsers will warn but this will be a bad user experience (as the warning will not
mean much to most users - and even if it does, suggest that security of the site is lacking).

To solve the problem you could add a second listener that listens on TCP port 80 (HTTP). On the
listener, its then possible to create :doc:`layer7-policies/index` that could (among other
things) redirect.

Two main ways to add the redirection to HTTPS. The first method would add a new prefix to a
path and would thus take paths into consideration.

This is the recommended approach as it would always send the visitor to the correct page (provided
the path exists). The downside is, its only configurable using the
:doc:`/getting-started/managing-your-cloud/openstack-terminal-client`.

The second method would ensure that all requests made via HTTP redirects to your home page. If your
web application is using HTTPS for linking, the user would afterwards remain in the HTTPS realm.

Create a listener
^^^^^^^^^^^^^^^^^

The first step is to setup a new listener.

.. note::

   The cloud management portal cannot create just a listener but will create an entire pool as
   well.

   We do not recommend using it for this task but if you want to use it, navigate to your load
   balancer in the menu, click the **Listeners** tab and then press the **+** plus sign, after
   which you should be able to follow our :doc:`guide <launching-a-loadbalancer/cloud-management-portal>`.

Documentation for creating a listener by using the
:doc:`/getting-started/managing-your-cloud/openstack-terminal-client` is available
:doc:`here <launching-a-loadbalancer/openstack-terminal-client>`.

To add just a HTTP listener (as opposed to an entire load balancer with pools and health
checking) using OpenStack Horizon.

- Under **Project**, click **Network** and then **Load balancers** in the sidebar menu.

- Press the name of the load balancer to which you want to add the rule.

- Press the listener tab.

- Press **+ Create listener**.

- Name your listener. We recommend calling it ``NAME_listener_http`` to differentiate it
  from other listeners. Optionally provide a description.

- Select HTTP as the load balancer protocol.

- Ensure that you set the port to 80 (if its automatically set to 81, you already have
  a listener that listens on port 80 and should instead use that one).

- Under the **Pool details** tab, select **No** under **Create pool** section.

- Press **Create listener**.

.. important::

   When setting up the policy in the next step, remember that you would need a policy
   for both www.example.com and example.com (assuming you use both).

Create a path aware redirect policy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The recommended way to require HTTPS is to use a redirect prefix. To setup this, use
the OpenStack Terminal Client according to below. If you would rather use OpenStack
Horizon or the cloud management portal, see below for a less accurate way to redirect.

- Run this command: ``openstack loadbalancer listener list``. Save the name of the listener.

- Run this command: ``openstack loadbalancer l7policy create --action REDIRECT_PREFIX --redirect-prefix https://[YOUR_DOMAIN] --name redirect_to_https [LISTENER NAME]`` replacing
  the domain and the listener name from the previous step.

- Run this command: ``openstack loadbalancer l7rule create --compare-type STARTS_WITH --type PATH --value / redirect_to_https``

.. note::

   Don't add a trailing slash on your domain as that will add an extra slash
   in the path.

Create a redirect to the first page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you prefer to stay in the GUI, the following method will allow you to setup similar functionality
using OpenStack Horizon, using this method, for example http://www.example.com/subfolder/index.html
would redirect to https://example.com.

- Under **Project**, click **Network** and then **Load balancers** in the sidebar menu.

- Press the name of the load balancer to which you want to add the rule.

- Press the listener tab.

- Press the name of your HTTP listener.

- Press **L7 Policies** tab and then **+ Create L7 policy**

- Name your policy to for example ``redirect_https`` and optionally give it a description.

- Under **Action** select ``REDIRECT_TO_URL``.

- Under **Redirection URL** type ``https://yourdomain.com`` (replace yourdomain.com with your domain).

- Under **Position**, type ``1``.

- Press the name of your new policy and then the tab **L7 Rules** and then **+ Create L7 Rule**.

- Under **Type**, select ``HOST_NAME``.

- Under **Compare type**, select ``CONTAINS``.

- Enter your domain (same as the one you want to do redirects for) in the value field.

- Press **Create L7 Rule**

..  seealso::

    - :doc:`general-concept/index`
