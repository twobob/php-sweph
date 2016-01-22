# A -very simple- build instruction #

## Introduction ##

This instruction describes how I build php-sweph on my Debian Linux. Building on other platforms should be similar.

My environment:
  * Debian Linux (lenny), i386
  * Apache/2.2.9 (Debian)
  * PHP Version 5.2.6-1+lenny8
  * Swiss Ephemeris 1.76

## Steps ##

  * Get the source code from google code svn
```
joel@joel-home:~$ svn checkout http://php-sweph.googlecode.com/svn/trunk/ php-sweph
A    php-sweph/php_sweph.h
A    php-sweph/config.m4
A    php-sweph/sweph.c
Checked out revision 7.
```
  * Put the Swiss Ephemeris header files (you just need sweodef.h and swephexp.h) to /usr/local/bin. This can be changed in config.m4.
  * Put the compiled static Swiss Ephemeris library (libswe.a) in /usr/local/lib
  * Under php-sweph directory, enter 'phpize'. Of course, you need PHP
```
joel@joel-home:~/php-sweph$ phpize
Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
```
  * enter './configure --enable-sweph'
```
joel@joel-home:~/php-sweph$ ./configure --enable-sweph
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking for a sed that does not truncate output... /bin/sed
checking for gcc... gcc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
...
checking whether to build shared libraries... yes
checking whether to build static libraries... no
configure: creating libtool
appending configuration tag "CXX" to libtool
appending configuration tag "F77" to libtool
configure: creating ./config.status
config.status: creating config.h
```
  * 'make'
  * Once it's done, you should be able to see sweph.so there.
```
joel@joel-home:~/php-sweph$ ls -l modules
total 744
-rw-r--r-- 1 joel joel    809 2010-04-18 14:06 sweph.la
-rwxr-xr-x 1 joel joel 752689 2010-04-18 14:06 sweph.so
```
  * 'make install'. On my machine, the php extensions are placed in '/usr/lib/php5/20060613+lfs/'
```
joel@joel-home:~/php-sweph$ ls /usr/lib/php5/20060613+lfs/
curl.so  mcrypt.so  mysql.so      pdo.so         sweph.so
gd.so    mysqli.so  pdo_mysql.so 
```
  * add this line to 'php.ini' file (generally in /etc/php5/apache2/php.ini)
```
extension=sweph.so
```
  * restart Apache
  * Check phpinfo() (In case you don't have it, make a .php file with only this line: `<?php phpinfo(); ?>`). If you are able to see 'sweph' module and a table similar to the following one, you're all set.

| sweph support	| enabled |
|:--------------|:--------|
| Swiss Ephemeris version	| 1.76.00 |
| default ephemeris file path | .:/users/ephe2/:/users/ephe/ |