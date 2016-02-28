# Overview

This is a forward-port of the OpenSSH LPK support patch for OpenSSH 6.6p1 and OpenSSH 7.2p1.

It adds support for storing OpenSSH public keys in LDAP. It also supports grouping of machines in the LDAP data to limit users to specific machines.

The latest homepage for the LPK project is:
http://code.google.com/p/openssh-lpk/

The rediff patch file for openssh-6.5p1 is:
http://www.findthatzip-file.com/search-173607525-fZIP/winrar-winzip-download-openssh-lpk-6.5p1-0.3.14.patch.gz.htm
N.B.) The above site is now unavailable.

# How to use (for 6.6p1)

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

~~~ bash
# LPK flags
confflags += --with-ldap --with-libs=-lldap --with-cppflags=-DWITH_LDAP_PUBKEY
~~~

Now you can build a package!

~~~ bash
sudo dpkg-buildpackage -us -uc
~~~

## for others
You can simply apply this patch before building an openssh package.

# About patch for 7.2p1

The patch for OpenSSH 7.2p1 (the latest version as of Feb 28, 2016) is tentatively released. A authentication part seems to be largely changed from a verion 6.x, thus I have no confidence you will successfully use OpenSSH that you apply this patch. I really appreciate your careful test.

## how to use
~~~ bash
cd WORKING_DIR
git clone git://anongit.mindrot.org/openssh.git
git clone https://github.com/takkeybook/openssh-lpk.git
cd openssh
patch -p1 < ../openssh-lpk/openssh-lpk-7.2p1-0.3.14.patch
autoreconf
./configure --with-ldap --with-libs=-lldap --with-cppflags=-DWITH_LDAP_PUBKEY --disable-strip --with-mantype=doc --with-4in6 --with-privsep-path=/var/run/sshd --with-tcp-wrappers --with-pam --with-libedit --with-kerberos5=/usr --with-ssl-engine --with-selinux  --with-xauth=/usr/bin/xauth --without-xauth 
make
make install
~~~
N.B.: The bunch of options specified for the configure script is just examples. You can add or remove one or more as you want.
