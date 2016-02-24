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
    tar xavf ../guix_$(dpkg-parsechangelog -S Version | cut -f 1 -d -).orig.tar.gz --strip-components=1
    sudo apt-get install build-essential dh-autoreconf dh-systemd autotools-dev graphviz guile-2.0-dev guile-json help2man libgcrypt20-dev libsqlite3-dev libbz2-dev texinfo
    dpkg-buildpackage
    sudo dpkg -i ../guix_0.9.0-1_amd64.deb ../emacs-guix_0.9.0-1_all.deb

The post install script sets up the guixbuild users however you may still
want to authorize Guix's build server hydra.gnu.org

You can do that with

.. code:: bash

    sudo guix archive --authorize /usr/share/guix/hydra.gnu.org.pub

Using this package with guix git clones
---------------------------------------

Once you have the guix package installed you may want to
modify or add new guix packages.

The following recipe will clones the guix repository, and configures the
checkout. ``dh_auto_configure`` is a convientent shortcut to set several
configure arguments especially --localstatedir=/var

Setting localstatedir is needed to find the guix-daemon socked and package
database.

Making the ~/.config/guix/latest symlink point to your checkout
allows guix to find your changes to official packaging repositories.

.. code:: bash

    cd <your favorite project directory>
    git clone git://git.savannah.gnu.org/guix.git
    cd guix
    ./bootstrap
    dh_auto_configure
    make	  
    mkdir ~/.config/guix
    cd ~/.config/guix
    ln -s <your favorite project directory>/guix latest

You can also use GUIX_PACKAGE_PATH to point to other directories
where you have your own packages defined. 
(However pointing GUIX_PACKAGE_PATH to the guix checkout causes trouble)

