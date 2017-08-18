Configure Postfix
=========


[![Build Status](https://travis-ci.org/Rheinwerk/ansible-role-postfix.svg?branch=master)](https://travis-ci.org/Rheinwerk/ansible-role-postfix)

Requirements
------------

Upstream mailserver.


Role Variables
--------------

There is one main variable that drives this role: `_postfix`. It is a map that contains all configuration and settings for this role. There is an additional optional variable `_sasl`, which, if defined, controls setting up sasl in the postfix configuration.
Please see `defaults/main.yml` for details.

Dependencies
------------

None.


Example Playbook
----------------

The general contract of this role is to take the variables map `_postfix` from `defaults/main.yml` as a template for your configuration and pass that configuration as a parameter to this role.

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      var:
        POSTFIX
          ...
      roles:
         - { role: postfix, tags: [ 'postfix' ], _postfix: "{{ POSTFIX }}" }

License
-------

Please see LICENSE.

Author Information
------------------

Original author is [Daniel Schneller](https://github.com/dschneller) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.

