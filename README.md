Configure Postfix
=========


[![Build Status](https://github.com/Rheinwerk/ansible-role-update_postfix_config/actions/workflows/ci.yml/badge.svg)](https://github.com/Rheinwerk/ansible-role-update_postfix_config/actions/workflows/ci.yml)

Requirements
------------

Upstream mailserver.


Role Variables
--------------

Every variable has a default (`defaults/main.yml`); set only the ones that differ.
`meta/argument_specs.yml` and `tasks/assert.yml` check the values before anything is
written; a wrong type, an unknown key inside `_postfix_relay`, `_postfix_tagging_transport`
or `_sasl`, or the `_postfix` dict of v0.x fails the run at the start.

| Variable | Default | Description |
| --- | --- | --- |
| `_postfix_domain` | `"{{ SITE.email.domain }}"` | Mail domain; `myhostname` and `/etc/mailname` become `<hostname>.<domain>`. |
| `_postfix_default_aliases` | `"{{ SITE.email.default_aliases }}"` | Aliases every host gets, `{<alias>: [<recipients>]}`; they win over `_postfix_aliases` with the same name. |
| `_postfix_default_alias_recipients` | `["{{ SITE.email.default_infrastructure_alert_address }}"]` | Recipients of every alias with an empty list. |
| `_postfix_aliases` | `{}` | Host-specific aliases, same format; every value is a list (`[]` for the default recipients). |
| `_postfix_relay` | `host: "{{ SITE.email.relay.host }}"`, `port: "{{ SITE.email.relay.port \| default(25) }}"` | Smarthost (`relayhost = [<host>]:<port>`); `{}` delivers directly and needs `_postfix_tagging_transport: {}` too. On EC2 the SASL and TLS settings stay on. |
| `_postfix_tagging_transport` | `recipient_regexp_filter: "/infrastructure.*@centerdevice.com/"` | Mail to matching recipients goes through the `taggingsmtp` transport, which puts `[<CD_ENV.iaas.name>.<account>.<env>]` into the subject; needs `_postfix_relay`, `{}` disables it. |
| `_postfix_message_size_limit` | `null` | `message_size_limit` in bytes; `null` keeps the Postfix default (10240000). |
| `_sasl` | `username: abc`, `password: def` | SMTP credentials outside EC2, `{}` disables SASL there. |
| `CD_ENV` | `iaas` with `name`, `account`, `env` set to `unset` | Environment for the subject tag. |

The defaults read the CenterDevice consumer contract (`SITE.email`); a play without it sets
the variables itself. On EC2 instances the role ignores `_sasl` and decrypts the SES
credentials from `/etc/postfix/ses_credentials.yml` with KMS instead, once per run and only
when `_postfix_relay` is set.

Dependencies
------------

None.


Example Playbook
----------------

    - hosts: servers
      vars:
        _postfix_aliases:
          haproxy: [root]
        _postfix_message_size_limit: 40000000
      roles:
        - { role: update_postfix_config, tags: ['postfix'] }

License
-------

Please see LICENSE.

Author Information
------------------

Original author is [Daniel Schneller](https://github.com/dschneller) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.

