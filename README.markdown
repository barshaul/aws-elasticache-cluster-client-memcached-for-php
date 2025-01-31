# Amazon ElastiCache Cluster Client

Amazon ElastiCache Cluster Client is used to connect to ElastiCache for Memcached clusters. This client supports auto-discovery capabilities which allow you to easily manage your Memcached cluster configuration. This extension uses Amazon ElastiCache fork of libmemcached library to provide API for communicating with ElastiCache servers. Our changes are based on open-source memcached extension v.2.1.0 from https://github.com/php-memcached-dev/php-memcached. This code branch is compatible with PHP 7.x. Other PHP versions (including PHP 5.x) are not supported.

This client library is released under the [Apache 2.0 License](http://aws.amazon.com/apache-2-0/).

## To install from pre-compiled client artifact on 64-bit Linux, please follow the steps below:

### Ubuntu 22.04 AMI 

a) Launch a new instance from the AMI

b) Run the following commands

> sudo apt-get update

> sudo apt-get install gcc g++ make

c) Install PHP 7.x. Installation instructions for PHP 7.4:

> sudo apt -y install software-properties-common

> sudo add-apt-repository ppa:ondrej/php

> sudo apt-get update

> sudo apt -y install php7.4

d) Download and unzip Amazon ElastiCache Cluster Client from AWS ElastiCache Management Console.

e) With root permission, copy the extracted artifact file amazon-elasticache-cluster-client.so into the php extension directory /usr/lib/php/20190902. In case this extension dir does not exist, you can find it by running:

> php -i | grep extension_dir 

f) Insert the line "extension=amazon-elasticache-cluster-client.so" into file /etc/php/7.4/cli/php.ini (change "7.4" to the PHP version you have)

g) If you downloaded ElastiCache Cluster Client with PHP 7.4, install OpenSSL 1.1.x.

> wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz

> tar xvf openssl-1.1.1c.tar.gz

> cd openssl-1.1.1c

> ./config -Wl,--enable-new-dtags,-rpath,'$(LIBRPATH)'

> make

> sudo make install

> sudo ln -s /usr/local/lib/libssl.so.1.1 /usr/lib/x86_64-linux-gnu/libssl.so.1.1


### Amazon Linux 2 AMI 

a) Launch a new instance from the AMI

b) Run the following command

> sudo yum install gcc-c++ zlib-devel

c) Install PHP 7.x
Use amazon-linux-extras to install php7.4

> which amazon-linux-extras

If not installed, use following command to install

> sudo yum install -y amazon-linux-extras

confirm that PHP 7.x topic is available in our Amazon Linux 2 machine

> sudo  amazon-linux-extras | grep php

As we can see all PHP 7 topics, in this example we’ll enable php7.4 topic.

> sudo amazon-linux-extras enable php7.4

Now install PHP packages from the repository.

> sudo yum clean metadata

> sudo yum install php php-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap


d) Download and unzip Amazon ElastiCache Cluster Client from AWS ElastiCache Management Console

e) With root permission, copy the extracted artifact file amazon-elasticache-cluster-client.so into /usr/lib64/php/modules/

f) Insert the line "extension=amazon-elasticache-cluster-client.so" into file /etc/php.ini

g) If you downloaded ElastiCache Cluster Client with PHP 7.4, install OpenSSL 1.1.x. 
Installation instructions for OpenSSL 1.1.1:

> sudo yum -y update

> sudo yum install -y make gcc perl-core pcre-devel wget zlib-devel

> wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz

> tar xvf openssl-1.1.1c.tar.gz

> cd openssl-1.1.1c

> ./config -Wl,--enable-new-dtags,-rpath,'$(LIBRPATH)'

> make

> sudo make install

> sudo ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/libssl.so.1.1

### SUSE Linux 15 AMI 

a) Launch a new instance from the AMI

b) Run the following command

> sudo zypper refresh

> sudo zypper update -y

> sudo zypper install gcc

c) Install PHP 7.x

> sudo zypper addrepo http://download.opensuse.org/repositories/devel:/languages:/php/openSUSE_Leap_15.3/ php

> sudo zypper install php7 

d) Download and unzip Amazon ElastiCache Cluster Client from AWS ElastiCache Management Console

e) With root permission, copy the extracted artifact file amazon-elasticache-cluster-client.so into /usr/lib64/php7/extensions/

f) Insert the line "extension=amazon-elasticache-cluster-client.so" into file /etc/php7/cli/php.ini

## To compile the client from source, do the following set of steps:

### Prerequests libraries
- OpenSSL >= 1.1.0 (unless TLS support is disabled by ./configure --disable-tls).
- SASL (libsasl2, unless SASL support is disabled by ./configure --disable-sasl).
- 
### Compile the library

a) Launch a Linux-based EC2 instance and install PHP 7.x along with its required dependencies.

b) Checkout and compile the dependency package aws-elasticache-cluster-client-libmemcached via https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached

c) Run the following set of commands under the current directory:

> phpize

> mkdir BUILD

> cd BUILD

> ../configure --with-libmemcached-dir=&lt;path to libmemcached build directory&gt; --disable-memcached-sasl

If running ../configure fails to find *libssl* (OpenSSL library) it may be necessary to tweak the PKG_CONFIG_PATH environment variable:
> PKG_CONFIG_PATH=/path/to/ssl/lib/pkgconfig ../configure --with-libmemcached-dir=&lt;path to libmemcached build directory&gt; --disable-memcached-sasl

Alternately, if you are not using TLS, you can disable it by running:
> ../configure --with-libmemcached-dir=&lt;path to libmemcached build directory&gt; --disable-memcached-sasl --disable-memcached-tls


Note: If you want to enable igbinary support, checkout, compile, and install the upstream igbinary7 package https://github.com/igbinary/igbinary7, and append the option "--enable-memcached-igbinary" at the end of the "configure" command above.

> make

> make install

Note: you can statically link the libmemcached library into the PHP binary so it can be ported across various Linux platforms. To do that, run the following command before "make":

> sed -i "s#-lmemcached#<libmemcached build directory>\/lib\/libmemcached.a -lcrypt -lpthread -lm -lstdc++ -lsasl2#" Makefile

> sed -i "s#-lmemcachedutil#<libmemcached build directory>/lib/libmemcachedutil.a#" Makefile

# Resources
---------
 * [Github link] (https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php)
 * [AmazonElastiCache Auto Discovery](http://docs.amazonwebservices.com/AmazonElastiCache/latest/UserGuide/AutoDiscovery.html)
 * [php-memcached] (https://github.com/php-memcached-dev/php-memcached)
 * [How do I install a software package from the Extras Library on an EC2 instance running Amazon Linux 2?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-install-extras-library-software/)

Dependencies
------------

php-memcached 3.x:
* Supports PHP 7.0 - 7.4.
* Requires libmemcached 1.x or higher.
* Optionally supports igbinary 2.0 or higher.
* Optionally supports msgpack 2.0 or higher.

php-memcached 2.x:
* Supports PHP 5.2 - 5.6.
* Requires libmemcached 0.44 or higher.
* Optionally supports igbinary 1.0 or higher.
* Optionally supports msgpack 0.5 or higher.

[libmemcached](http://libmemcached.org/libMemcached.html) version 1.0.18 or
higher is recommended for best performance and compatibility with memcached
servers.

[igbinary](https://github.com/igbinary/igbinary) is a faster and more compact
binary serializer for PHP data structures. When installing php-memcached from
source code, the igbinary module must be installed first so that php-memcached
can access its C header files. Load both modules in your `php.ini` at runtime
to begin using igbinary.

[msgpack](https://msgpack.org) is a faster and more compact data structure
representation that is interoperable with msgpack implementations for other
languages. When installing php-memcached from source code, the msgpack module
must be installed first so that php-memcached can access its C header files.
Load both modules in your `php.ini` at runtime to begin using msgpack.
