# Installing 

> Courtest of [linuxfromscratch](http://www.linuxfromscratch.org/blfs/view/svn/server/sqlite.html)

## Introduction to SQLite

The SQLite package is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine.

This package is known to build and work properly using an LFS-9.0 platform.

### Package Information
* Download (HTTP): [https://sqlite.org/2019/sqlite-autoconf-3300100.tar.gz](https://sqlite.org/2019/sqlite-autoconf-3300100.tar.gz)
* Download MD5 sum: 51252dc6bc9094ba11ab151ba650ff3c  
* Download size: 2.7 MB  
* Estimated disk space required: 73 MB  
* Estimated build time: 0.4 SBU (Using parallelism=4)  

### Additional Downloads

#### Optional Documentation

* Download (HTTP): [https://sqlite.org/2019/sqlite-doc-3300100.zip](https://sqlite.org/2019/sqlite-doc-3300100.zip)
* Download MD5 sum: 0a631f0f293167c82be0c10831642469  
* Download size: 9.1 MB  

## Installation of SQLite

If you downloaded the optional documentation, issue the following command to install the documentation into the source tree:

```bash
unzip -q ../sqlite-doc-3300100.zip
```

Install SQLite by running the following commands:

> before that unzip with ```tar zxvf sqlite-autoconf-3300100.tar.gz``` and ```cd sqlite-autoconf-3300100```

```bash
./configure --prefix=/usr     \
            --disable-static  \
            --enable-fts5     \
            CFLAGS="-g -O2                    \
            -DSQLITE_ENABLE_FTS3=1            \
            -DSQLITE_ENABLE_FTS4=1            \
            -DSQLITE_ENABLE_COLUMN_METADATA=1 \
            -DSQLITE_ENABLE_UNLOCK_NOTIFY=1   \
            -DSQLITE_ENABLE_DBSTAT_VTAB=1     \
            -DSQLITE_SECURE_DELETE=1          \
            -DSQLITE_ENABLE_FTS3_TOKENIZER=1" &&
make
```

This package does not come with a test suite.

Now, as the root user run ```sudo make install```:

```bash
bbearce@bbearce-XPS-15-9560:~/Downloads/sqlite-autoconf-3300100$ sudo make install
make[1]: Entering directory '/home/bbearce/Downloads/sqlite-autoconf-3300100'
 /bin/mkdir -p '/usr/lib'
 /bin/bash ./libtool   --mode=install /usr/bin/install -c   libsqlite3.la '/usr/lib'
libtool: install: /usr/bin/install -c .libs/libsqlite3.so.0.8.6 /usr/lib/libsqlite3.so.0.8.6
libtool: install: (cd /usr/lib && { ln -s -f libsqlite3.so.0.8.6 libsqlite3.so.0 || { rm -f libsqlite3.so.0 && ln -s libsqlite3.so.0.8.6 libsqlite3.so.0; }; })
libtool: install: (cd /usr/lib && { ln -s -f libsqlite3.so.0.8.6 libsqlite3.so || { rm -f libsqlite3.so && ln -s libsqlite3.so.0.8.6 libsqlite3.so; }; })
libtool: install: /usr/bin/install -c .libs/libsqlite3.lai /usr/lib/libsqlite3.la
libtool: install: /usr/bin/install -c .libs/libsqlite3.a /usr/lib/libsqlite3.a
libtool: install: chmod 644 /usr/lib/libsqlite3.a
libtool: install: ranlib /usr/lib/libsqlite3.a
libtool: warning: remember to run 'libtool --finish /usr/local/lib'
 /bin/mkdir -p '/usr/bin'
  /bin/bash ./libtool   --mode=install /usr/bin/install -c sqlite3 '/usr/bin'
libtool: install: /usr/bin/install -c sqlite3 /usr/bin/sqlite3
 /bin/mkdir -p '/usr/include'
 /usr/bin/install -c -m 644 sqlite3.h sqlite3ext.h '/usr/include'
 /bin/mkdir -p '/usr/share/man/man1'
 /usr/bin/install -c -m 644 sqlite3.1 '/usr/share/man/man1'
 /bin/mkdir -p '/usr/lib/pkgconfig'
 /usr/bin/install -c -m 644 sqlite3.pc '/usr/lib/pkgconfig'
make[1]: Leaving directory '/home/bbearce/Downloads/sqlite-autoconf-3300100'

```

> Not sure what ```warning: remember to run 'libtool --finish /usr/local/lib'``` is for but I ran it and it didn't break anything and output this message:

```bash
bbearce@bbearce-XPS-15-9560:~/Downloads/sqlite-autoconf-3300100$ libtool --finish /usr/local/lib
libtool: finish: PATH="/home/bbearce/gems/bin:/home/bbearce/bin:/home/bbearce/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/sbin" ldconfig -n /usr/local/lib
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/local/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

```

...a little background from [gnu](https://www.gnu.org/software/libtool/):

*GNU libtool is a generic library support script. Libtool hides the complexity of using shared libraries behind a consistent, portable interface.*

If you downloaded the optional documentation, issue the following commands as the root user to install it:

```bash
install -v -m755 -d /usr/share/doc/sqlite-3.30.1 &&
cp -v -R sqlite-doc-3300100/* /usr/share/doc/sqlite-3.30.1
```