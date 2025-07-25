===============
Create snapshot
===============

A snapshot is a point-in-time snapshot of the state of an instance or
a volume.

You can create a snapshot of a single volume or create a snapshot of an
instance, when creating a snapshot of an instance, snapshots get created
for each volume attached to the instance. Metadata about the instance gets
saved as an image.

This part will focus on creating a snapshot of an instance. This will
allow you to create a new instance from the snapshot.

.. note::

   Creating a new instance from the image created from a snapshot will
   reuse some parameters from the original instance but not for example
   instance ports (and thus not any IP addressing).

.. important::

   For data integrity reasons, we recommend shutting of the instance before taking
   a snapshot.

   Incomplete writes might otherwise impact data integrity and in result in a
   inconsistent snapshot with corrupt data resulting in at worst data loss.

We recommend only having snapshots for limited periods of time and recommend that you
regularly :doc:`delete snapshots <delete-snapshot>`.

.. caution::

   Snapshots is **NOT** backups and are does not provide any more data safety or
   guarantee than the underlying storage used for the volume does.

Cloud management portal
-----------------------

To create a snapshot using the :doc:`/getting-started/managing-your-cloud/cloud-management-portal`

- Press **Compute** and then **Instances** in the sidebar menu.

- Press the instance that you want to resize.

- Press the **Snapshots** tab. 

- Press **Create snapshot** button.

- Name your snapshot. 

- Press **Create**

Your snapshot is available in the **Snapshot** tab of the instance and if you press
**Storage** and then **Snapshots** in the main menu. In the latter case, there will
be one snapshot per volume of the instance you created the snapshots from (only one
if the instance only had a single volume).

OpenStack Horizon
-----------------

To create a snapshot using :doc:`/getting-started/managing-your-cloud/openstack-horizon`

- Under **Project**, click **Compute** and then **Instances** in the sidebar menu.

- In the dropdown menu to the far right of the line corresponding to the instance you
  want to snapshot, press **Create snapshot** (which is either the default option if the
  instance is running, of in the dropdown if it's not running).

- Name your snapshot. 

- Press **Create Snapshot**

Your snapshot is available in the **Images** menu (under main menu **Compute**) as well
as if you press **Volumes** and then **Snapshots** in the main menu. In the latter case,
there will be one snapshot per volume of the instance you created the snapshots from (only
one if the instance only had a single volume).

OpenStack Terminal Client
-------------------------

To create a snapshot using the :doc:`/getting-started/managing-your-cloud/openstack-terminal-client`

- Run this command: ``openstack server list``, note the name of the instance you want to
  snapshot.

- Run this command: ``openstack server image create --name [SNAPSHOT_NAME] [INSTANCE_NAME]``, replacing
  the items in angle brackets with a descriptive name (this is optional, if omitted the instance name
  will be used) and and the name of the instance you want to snapshot.

- Run this command to verify: ``openstack image list --private``

..  seealso::

    - :doc:`index`
    - :doc:`../persistent-block-storage/index`
