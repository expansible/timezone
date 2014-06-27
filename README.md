timezone
========

Set system timezone on almost any OS supported by Ansible.

[![Build Status](https://travis-ci.org/expansible/etckeeper.svg?branch=develop)](https://travis-ci.org/expansible/timezone) 

Requirements
------------

* Ansible 1.4 or later.

Role Variables
--------------

There are two parameter variables (with defaults in the role tasks file):

* **timezone** - *string* (default is **GMT**)

  This should be set to the desired timezone.
  On most systems, "Olson-style" timezone names are preferred
  (e.g. "Americas/New_York" or "Europe/Berlin"),
  but some systems (e.g. AIX < 6.1) only support POSIX-style
  (e.g. "EST5EDT" or "PST8PDT,M3.2.0/2:00:00,M11.1.0/2:00:00")
  timezones.
  The complex POSIX-style timezones are not always fully supported
  (e.g. on systems that expect to create links to a file in
  ``/usr/share/zoneinfo/`` with the same name as the timezone).

* **timezone_localtime_mode** - *string* (default is **auto**)

  On many Linux systems, ``/etc/localtime`` is either a copy or a
  symbolic link to a timezone data file in ``/usr/share/zoneinfo/``.
  Using a symbolic link can cause complications if ``/usr/`` is a
  separate filesystem,
  and a copy requires extra work to apply updated timezone information
  (e.g. changed start or end of daylight savings time).
  The default ('auto') preserves the type of the existing file,
  but setting this to 'copy' or 'symlink' forces the specified type.

There are additional configuration variables that are used to
parameterize the different ways that timezone information is
configured and managed on different systems.
They aren't needed unless you have an unrecognized operating system;
in that case you should consult the source file ``defaults/main.yml``
for information on their use.

Example Playbook
-------------------------

This role is really very simple and straightforward;
you can specify the desired timezone and/or the type of file
``/etc/localtime`` should be as parameters to the role,
or by setting the appropriate variables elsewhere.

Here is an example playbook that uses the timezone role:

    ---
    # Set the timezone to UTC
    - hosts: all
      roles:
      - { role: expansible.timezone, timezone: 'UTC' }

    # Set the timezone, replacing /etc/localtime symlink with a copy
    - hosts: all
      roles:
      - { role: expansible.timezone, timezone_localtime_mode: 'copy' }

License
-------

MIT (Expat) - see LICENSE file for details

Author Information
------------------
Sergey Korolev (<korolev.srg@gmail.com>)
Alexander Dupuy (<alex.dupuy@mac.com>)
