# epson-l360-scanner-driver
Image Scan! for Linux
=====================

Copyright (C) 2015  SEIKO EPSON CORPORATION

For the impatient
-----------------

Change to the folder where you downloaded this scanner driver bundle,
extract it and install all components with

::

   tar xaf iscan-bundle-1.0.0.x64.deb.tar.gz
   cd iscan-bundle-1.0.0.x64.deb
   ./install.sh

in a terminal window.

You will be asked for your password to acquire the privileges needed
to install software on your system.  This works the same way as with
your regular software installation procedure.

What is all this?
-----------------

This scanner driver bundle contains all components of Image Scan! for
Linux that are needed for the scanner you selected and a convenience
``install.sh`` script.  The script can be used to install everything
with a single command.

If you are curious about what the script does, run it as follows

::

   ./install.sh --dry-run

That will display the command(s) it will run to install everything.
For a simple help message, try the ``--help`` option.

Non-free components
-------------------

The components in the ``plugins/`` folder are non-free, in the sense
that they do not give you all the freedoms normally associated with
`Free Software`_.

.. _Free Software: https://en.wikipedia.org/wiki/Free_software

With the exception of the "network" plugin (only included with the
generic iscan-bundle) these plugins are needed for the scanner you
selected.  The "network" plugin, however, is optional.  If you only
connect your scanner via USB (or SCSI) you do not need it.  To tell
the ``install.sh`` script that you do not want it installed, use

::

   ./install.sh --without-network

The "network" plugin is installed by default because almost all recent
EPSON scanners and all-in-ones support access via a (wireless) network
connection.

To be completely honest, the GUI of Image Scan! for Linux contains a
non-free image processing module as well.  It will not work without
this module.

Troubleshooting
---------------

While the ``install.sh`` script is expected to work as intended in the
vast majority of scenarios it might not work as intended for you.  If
that is the case, please pay attention to the error messages in the
output that is produced.  That should help you find a workaround by
yourself or in the list below.

No permission/privileges to install
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is likely to give messages that include phrases such as ``are you
root?``, ``Root privileges are required`` and ``you need to be root``
(possibly in your language).  The ``install.sh`` script assumes that
you can use ``sudo`` to obtain ``root`` status.  If that is not the
case the install will fail.  You can try

::

   su -c './install.sh'

and provide the ``root`` password in this case.  If you want to use
the ``--without-network`` option, make sure to put it inside the
quotes.

Note, however, that many distributions disable ``root`` logins these
days and instead set up the first user account created during initial
installation so that it can obtain ``root`` status via ``sudo``.  If
that is not your account, get the owner of that account to install the
bundle.

Package conflicts
~~~~~~~~~~~~~~~~~

There is a small chance that the scanner bundle requires other
software to be installed that conflicts with software you have already
installed.  Before you can install the scanner bundle, you will need
to resolve any such conflicts.

The straightforward way is simply to remove any conflicting software
that is already installed.  Your package manager may suggest ways to
resolve the conflict that are less drastic, though.

Package manager or ``apt-get`` command not found
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``install.sh`` script assumes that you have one of the common
higher level package managers for RPM or Debian packages installed.
It also assumes that the binary packages in the bundle are in the
package format that your distribution expects.

In the unlikely case that no supported package manager is available
you can still install manually using the low-level package managers
``rpm`` or ``dpkg``.  First run

::

   ./install.sh --dry-run

and note the list of ``.rpm`` or ``.deb`` files that shows.  You can
use the ``--without-network`` option with the above command if you
wish.  Next, try to install those packages with one of

::

   rpm --install $list_of_rpm_files
   dpkg --install $list_of_deb_files

This will likely fail due to missing dependencies.  In case you used
``rpm``, then install all of the required packages that are mentioned
in the error output using your regular software installation method
and run the same command again.  This time it ought to succeed.

After a failed ``dpkg``, first remove the failed packages.

::

   dpkg --remove $list_of_packages

Note this requires the package names listed at the end of the error
message, *not* the package files you used to install.  Once removed,
you can use your regular software installation method to install any
missing dependencies and run the ``dpkg --install`` command again.

Of course, all ``rpm``, ``dpkg`` and ``apt-get`` invocations require
``root`` privileges.  Use ``sudo`` or ``su -c`` to obtain them.

``getopt`` command not found
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Very unlikely to happen but if it does, install ``util-linux`` and try
again.
