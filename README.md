ansible-role-osbs-common
========================

Role for deploying OpenShift OSBS common components on top of an existing
[OpenShift](https://openshift.org) cluster where we do not have cluster admin.

- [OpenShift build service](https://github.com/projectatomic/osbs-client/),
service for building layered Docker images.

This role is part of
[ansible-osbs](https://github.com/projectatomic/ansible-osbs/)
playbook for deploying OpenShift build service. Please refer to that github
repository for [documentation](https://github.com/projectatomic/ansible-osbs/blob/master/README.md)
and [issue tracker](https://github.com/projectatomic/ansible-osbs/issues).

Role Variables
--------------

OpenShift authorization policy - which users should be assigned the view
(read-only), osbs-builder (read-write), and cluster-admin (admin) roles. In
default configuration, everyone has read/write access. The authentication is
handled by the proxy - if you are not using it the everyone connecting from the
outside belongs to the `system:unauthenticated` group.

Default setup:

    osbs_readonly_users: []
    osbs_readonly_groups: []
    osbs_readwrite_users: []
    osbs_readwrite_groups:
      - system:authenticated
      - system:unauthenticated
    osbs_admin_users: []
    osbs_admin_groups: []

Development with authenticating proxy:

    osbs_readonly_users: []
    osbs_readonly_groups: []
    osbs_readwrite_users: []
    osbs_readwrite_groups:
      - system:authenticated
    osbs_admin_users: []
    osbs_admin_groups: []

Example production configuration with only one user starting the builds:

    osbs_readonly_users: []
    osbs_readonly_groups:
      - system:authenticated
    osbs_readwrite_groups: []
    osbs_readwrite_users:
      - kojibuilder
    osbs_admin_users:
      - foo@EXAMPLE.COM
      - bar@EXAMPLE.COM
    osbs_admin_groups: []

Limit on the number of running pods. Undefine or set to -1 to remove limit.

    osbs_master_max_pods: 3

Deploy yum proxy from a docker image (disabled by default). Variable
`osbs_yum_proxy_name` sets the name of the deploymentconfig/service OpenShift
objects.

    osbs_yum_proxy_image: docker.io/vrutkovs/docker-squid
    osbs_yum_proxy_name: yum-proxy

Dependencies
------------

OpenShift is expected to be installed on the remote host. This can be
accomplished by the
[install-openshift](https://github.com/projectatomic/ansible-role-install-openshift)
role.

License
-------

BSD

Author Information
------------------

Martin Milata &lt;mmilata@redhat.com&gt;
