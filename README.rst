Umap2
=====

Umap2 is the second revision of NCC Group's
python based USB host security assessment tool.

This revision will have all the features that
were supported in the first revision:

- *umap2emulate* - USB device emulation
- *umap2scan* - USB host scanning for device support
- *umap2detect* - USB host OS detection (no implemented yet)
- *umap2fuzz* - USB host fuzzing

In this revision there will be some additional
features:

- USB host fuzzing uses kitty as fuzzing engine
- Umap2 not only contains executable scripts,
  but is also installed as a package
  and may be used as a library

Umap2 is developed by NCC Group and Cisco SAS team.

Warning: umap2 is still an experimental,
alpha stage tool.
The APIs, executable names, etc. are likely to be changed
in the near future.
Use at your own risk.

Installation
------------

Since this is a very early version,
Umap2 is not yet available from pypi,
instead, use pip to install it:

::

    $ pip install git+https://github.com/nccgroup/umap2.git#egg=umap2

"Soft" Dependencies
-------------------

Umap2's dependencies are listed in **setup.py** and will be installed with umap2,
however, there are couple of things that you might want to do to add support
for some devices:

Mass Storage
~~~~~~~~~~~~

1. Requires a disk image called **stick.img** in the running directory

MTP
~~~

1. Requires a folder/file called **mtp_fs** in the current directory.
2. Requires the python package pymtpdevice. This package is not on pypi
   at the moment, but can be downloaded and installed from here:
   https://github.com/BinyaminSharet/Mtp

Usage
-----

Device Emulation
~~~~~~~~~~~~~~~~

Umap2's basic functionallity is emulating a USB device.
You can emulate one of the existing devices
(use **umap2list** to see the available devices):

::

    $ umap2emulate -P fd:/dev/ttyUSB0 -C mass_storage

or emulate your own device:

::

    $ umap2emulate -P fd:/dev/ttyUSB0 -C ~/my_mass_storage.py

A detailed guide to add your device will be added soon,
in the meantime, you can take a look at umap2 devices
under *umap2/dev/*

Device Support Scanning
~~~~~~~~~~~~~~~~~~~~~~~

Umap2 can attempt to detect what types of USB devices
are supported by the host.
It is done by emulating each device that is implemented in Umap2
for a short period of time,
and checking whether a device-specific message was sent.

::

    $ umap2scan -P fd:/dev/ttyUSB0

Fuzzing
~~~~~~~

Fuzzing with Umap2 is composed of three steps,
which might be unified into a single script in the future.

1. Find out what is the order of messages
   for the host you want to fuzz and the
   USB device that you emulate:

   ::

        $ umap2stages -P fd:/dev/ttyUSB0 -C keyboard -s keyboard.stages

2. Start the kitty fuzzer in a separate shell,
   and provide it with the stages generated in step 1.

   ::

        $ umap2kitty -s keyboard.stages

3. Start the umap2 keyboard emulation in fuzz mode

   ::

        $ umap2fuzz -P fd:/dev/ttyUSB0 -C keyboard

After stage 3 is performed, the fuzzing session will begin.

Host OS Detection
~~~~~~~~~~~~~~~~~

TBD

