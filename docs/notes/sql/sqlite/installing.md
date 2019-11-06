# Installing 

> Courtest of [linuxfromscratch](http://www.linuxfromscratch.org/blfs/view/svn/server/sqlite.html)

## Installation of SQLite

If you downloaded the optional documentation, issue the following command to install the documentation into the source tree:

```bash
unzip -q ../sqlite-doc-3300100.zip
```

Install SQLite by running the following commands:

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

Now, as the root user:

```bash
make install
```

If you downloaded the optional documentation, issue the following commands as the root user to install it:

```bash
install -v -m755 -d /usr/share/doc/sqlite-3.30.1 &&
cp -v -R sqlite-doc-3300100/* /usr/share/doc/sqlite-3.30.1
```