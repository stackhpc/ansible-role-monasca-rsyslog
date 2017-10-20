Monasca Rsyslog Connector
=========================

This role connects a local deployment of rsyslog to a Monasca log API.

Requirements
------------

The Monasca log APIs should be accessible from the target host.  Client
credentials for Monasca logging should be supplied as playbook parameters.
These get written into a `clouds.yaml` file.

Role Variables
--------------

`monasca_rsyslog_venv`: Path to a virtualenv installation of the Monasca rsyslog connector.
It defaults to `/usr/libexec/monasca-rsyslog`

`monasca_rsyslog_api_endpoint`: Monasca log api endpoint,
of the form `http://monasca-log-api:5607/v3.0`

`monasca_rsyslog_api_auth`: OpenStack authentication credentials.  For
example, a dict of the form:
* `auth_url`: OpenStack Keystone endpoint, for example http://keystone:5000/
* `project`: OpenStack tenant/project.
* `username`: OpenStack username.
* `password`: OpenStack password.

`monasca_rsyslog_packages_install`: Flag to define whether package dependencies
for creating a python virtualenv should be installed in the host OS.
defaults to `True`.

`monasca_rsyslog_packages`: List of dependency package names to enable virtualenv
support, and enable the building of some dependencies within a virtualenv by pip.
The default is a suitable list of package names for an EPEL-enabled CentOS distribution.

Dependencies
------------

*WRITEME*


Example Playbook
----------------

*WRITEME*

The following playbook generates a guest image and uploads it to OpenStack:

    ---
    - name: Generate guest image and upload
      hosts: openstack
      roles:
        - role: stackhpc.os-image
          os_images_auth:
            auth_url:     "{{ lookup('env','OS_AUTH_URL') }}"
            username:     "{{ lookup('env','OS_USERNAME') }}"
            password:     "{{ lookup('env','OS_PASSWORD') }}"
            project_name: "{{ lookup('env','OS_TENANT_NAME') }}"
          os_images_list:
          - name: FedoraCore
            elements:
              - fedora
              - selinux-permissive
              - alaska-extras
            env:
              DIB_ALASKA_DELETE_REPO: "y"
              DIB_ALASKA_PKGLIST: "pam-python pam-keystone"

Author Information
------------------

- Stig Telfer (<stig@stackhpc.com>)
