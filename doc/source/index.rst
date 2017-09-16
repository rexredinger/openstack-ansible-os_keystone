===================================
Keystone role for OpenStack-Ansible
===================================

.. toctree::
   :maxdepth: 2

   configure-keystone.rst
   configure-federation.rst
   configure-federation-wrapper.rst
   configure-federation-sp.rst
   configure-federation-idp.rst
   configure-federation-mapping.rst

To clone or view the source code for this repository, visit the role repository
for `os_keystone <https://github.com/openstack/openstack-ansible-os_keystone>`_.

Default variables
~~~~~~~~~~~~~~~~~

.. literalinclude:: ../../defaults/main.yml
   :language: yaml
   :start-after: under the License.


Required variables
~~~~~~~~~~~~~~~~~~

This list is not exhaustive at present. See role internals for further
details.

.. code-block:: yaml

    # hostname or IP of load balancer providing external network
    # access to Keystone
    external_lb_vip_address: 10.100.100.102

    # hostname or IP of load balancer providing internal network
    # access to Keystone
    internal_lb_vip_address: 10.100.100.102

    # password used by the keystone service to interact with Galera
    keystone_container_mysql_password: "YourPassword"

    keystone_auth_admin_password: "SuperSecretePassword"
    keystone_service_password: "secrete"
    keystone_rabbitmq_password: "secrete"
    keystone_container_mysql_password: "SuperSecrete"

Example playbook
~~~~~~~~~~~~~~~~

.. literalinclude:: ../../examples/playbook.yml
   :language: yaml

External Restart Hooks
~~~~~~~~~~~~~~~~~~~~~~

When the role performs a restart of the service, it will notify an Ansible
handler named ``Manage LB``, which is a noop within this role. In the
playbook, other roles may be loaded before and after this role which will
implement Ansible handler listeners for ``Manage LB``, allowing external roles
to manage the load balancer endpoints responsible for sending traffic to the
servers being restarted by marking them in maintenance or active mode,
draining sessions, etc. For an example implementation, please reference the
`ansible-haproxy-endpoints role <https://github.com/Logan2211/ansible-haproxy-endpoints>`_
used by the openstack-ansible project.

Tags
~~~~

This role supports two tags: ``keystone-install`` and ``keystone-config``

The ``keystone-install`` tag can be used to install and upgrade.

The ``keystone-config`` tag can be used to maintain configuration of the
service.
