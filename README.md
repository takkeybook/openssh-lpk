# Overview

This is a forward-port of the OpenSSH LPK support patch for OpenSSH 6.6p1.

It adds support for storing OpenSSH public keys in LDAP. It also supports grouping of machines in the LDAP data to limit users to specific machines.

The latest homepage for the LPK project is:
http://code.google.com/p/openssh-lpk/

The rediff patch file for openssh-6.5p1 is:
http://www.findthatzip-file.com/search-173607525-fZIP/winrar-winzip-download-openssh-lpk-6.5p1-0.3.14.patch.gz.htm
N.B.) The above site is now unavailable.

# How to use

## for Debian users
You can add this patch into debian/patches directory and apply it with quilt command, as follows,

~~~ bash
sudo apt-get update
cd WORKING_DIR
apt-get source openssh-server
cd openssh-6.6p1
cp ../openssh-lpk-6.6p1-0.3.14.patch debian/patches
vi debian/patches/series
~~~

Add the entry, *openssh-lpk-6.6p1-0.3.14.patch* in the last line of *series* file before applying the patch file with quilt command.

~~~ bash
quilt push
vi debian/rules
~~~
The following lines should be added where confflags is set, for example, just before the comment line, **# Linker flags.**

># LPK flags
confflags += --with-libs=-lldap --with-cppflags=-DWITH_LDAP_PUBKEY

~~~ bash
sudo dpkg-buildpackage -us -uc
~~~

## for others
You can simply apply this patch before building an openssh package.
