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

The dict *may* also include the following, optional variables:
* `project_domain_name`: OpenStack project domain name. Defaults to "Default".
* `region_name`: OpenStack region name. Defaults to "RegionOne".
* `user_domain_name`: OpenStack user domain name. Defaults to "Default".
* `service_type`: OpenStack monitoring service type. Defaults to "monitoring".
* `endpoint_type`: OpenStack monitoring endpoint type. Defaults to "public".

`monasca_rsyslog_packages_install`: Flag to define whether package dependencies
for creating a python virtualenv should be installed in the host OS.
defaults to `True`.

`monasca_rsyslog_rhel_packages`: List of dependency package names to enable virtualenv
support, and enable the building of some dependencies within a virtualenv by pip suitable
for an EPEL-enabled CentOS distribution.

`monasca_rsyslog_debian_packages`: List of dependency package names to enable virtualenv
support, and enable the building of some dependencies within a virtualenv by pip suitable
for a Debian distribution.

`monasca_rsyslog_custom_template_folder`: Folder, relative to the playbook which runs
this role containing custom rsyslog templates. For example, this may be used to ingest
system logs which aren't managed by rsyslog.

Dependencies
------------

This role installs the `monasca-rsyslog` output driver developed by Steve Simpson from
https://github.com/stackhpc/monasca-rsyslog


Example Playbook
----------------

The following playbook connects an rsyslog deployment with an output plugin for Monasca:

    ---
    - name: Deploy driver for Monasca-rsyslog
      hosts: all
      roles:
	- role: stackhpc.monasca-rsyslog
	  monasca_rsyslog_api_auth:
	    auth_url: "http://openstack-keystone:5000"
	    project: "monasca"
	    username: "monasca-agent"
	    password: "{{ vault_encypted_monasca_agent_password }}"
	  monasca_rsyslog_venv: "/usr/libexec/monasca-rsyslog"

Author Information
------------------

- Stig Telfer (<stig@stackhpc.com>)
