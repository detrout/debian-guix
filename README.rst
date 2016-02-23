guix for Debian
---------------

The Guix daemon requires a reasonable amount of setup before it'll work
correctly so I thought I'd package it.

However since Guix includes several bootstrap binaries it's unlikely that this
package can go into Debian figuring out how to build the bootstrap binaries
from source using Debian tools.

Usage
-----

To use this repository checkout it out <Guix Root>/debian.  You'll also need
to a copy of the source tarball in <Guix Root>/..

To build you need to do something like the following:

.. code:: bash

    mkdir guix
    cd guix
    git clone https://github.com/detrout/debian-guix.git debian
    uscan --download-current-version
    tar xavf ../guix-$(dpkg-parsechangelog -S Version | cut -f 1 -d -).orig.tar.gz --strip-components=1
    sudo apt-get install build-essential dh-autoreconf dh-systemd autotools-dev graphviz guile-2.0-dev guile-json help2man libgcrypt20-dev libsqlite3-dev libbz2-dev texinfo
    dpkg-buildpackage
    sudo dpkg -i ../guix_0.9.0-1_amd64.deb ../emacs-guix_0.9.0-1_all.deb
