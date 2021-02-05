paulrentschler.postfix
======================

[![MIT licensed][mit-badge]][mit-link]

Installs and configures Postfix on Ubuntu Linux.


Requirements
------------

None.


Role Variables
--------------

The following variables are available with defaults defined in `defaults/main.yml`:

Specify the hostname of the server if it is other than what Ansible knows.

    postfix_myhostname: "{{ ansible_nodename }}"

Specify the domain of the server (not defined by default).

    postfix_mydomain: ""

Specify the inet_interfaces that Postfix listens on.

    postfix_inet_interfaces: "$myhostname, localhost"

Specify the domains that this Postfix instance represents.

    postfix_mydestination: "$myhostname, localhost.$mydomain, localhost, $mydomain"

Specify the local recipient map(s) to use.

    postfix_local_recipient_maps: "unix:passwd.byname $alias_maps $transport_maps"

Specify the "trusted" SMTP clients that are allowed to relay mail through this Postfix instannce.

    postfix_mynetworks: "127.0.0.0/8, [::ffff:127.0.0.0]/104, [::1]/128"

Specify the host that email from this Postfix instance should be passed through.

    postfix_relayhost: ""

Specify the transport maps to be used (not defined by default).

    postfix_transport_maps: regexp:/etc/postfix/transportregex

Specify who should receive mail sent to the `root` user.

    postfix_roots_mail: root

Specify the group that should get permissions on configuration files.

    postfix_admin_group: root

Define email aliases. Each entry in the list should be a dictionary consisting of:

* `name` -- the alias to match and translate into `email`
* `email` -- the email address mail should directed to

    postfix_aliases: []

Specify additional external delivery methods that go into master.cf.

    postfix_additional_external_delivery_methods: []


### TLS settings

Set to "yes" to enable the use of TLS encryption when sending messages.

    postfix_smtpd_use_tls: no

Specify the SSL/TLS certificate used for sending encrypted messages.

    postfix_tls_cert: "/etc/ssl/certs/ssl-cert-snakeoil.pem"

Specify the SSL/TLS certificate's private key.

    postfix_tls_key: "/etc/ssl/private/ssl-cert-snakeoil.key"


### SASL settings

Set to "yes" to enable SASL authentication for sending messages.

    postfix_smtp_sasl_auth_enable: no

Specify the SSL/TLS Certificate Authority (SA) certificate file.

    postfix_smtp_tls_cafile: ""

Specify the SASL username to use.

    postfix_smtp_sasl_user: "{{ ansible_ssh_user }}"

Specify the SASL password to use.

    postfix_smtp_sasl_password: ""


Dependencies
------------

None.


Example Playbook
----------------

Simple version using all the defaults:

    - hosts: all
      roles:
        - paulrentschler.postfix


Custom version specifying settings:

    - hosts: all
      roles:
        - { role: paulrentschler.postfix,
            postfix_myhostname: mail.example.com,
            postfix_relayhost: mail.provider.net,
            postfix_roots_mail: sysadmin@example.com,
            postfix_admin_group: admin,
            }


License
-------

[MIT][mit-link]


Author Information
------------------

Created by Paul Rentschler in 2021.


[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://github.com/paulrentschler/ansible-role-postfix/blob/master/LICENSE
