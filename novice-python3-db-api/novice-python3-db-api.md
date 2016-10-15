The Novice's Guide to the Python 3 DB-API
=========================================

The source for this guide lives in GitHub [here](https://github.com/philvarner/novices-guides/blob/master/novice-python3-db-api/novice-python3-db-api.md).

Overview
--------

The standard API for interacting with relational databases in Python is defined in [PEP 249 Python Database API Specification v2.0](http://legacy.python.org/dev/peps/pep-0249).  

The most popular database libraries are:

* SQLite: [sqlite3](https://docs.python.org/3.4/library/sqlite3.html)
* Pyscopg: [psycopg2](http://initd.org/psycopg/)
* MySQL: [mysql](http://dev.mysql.com/doc/connector-python/en/)
* Oracle: [cx_Oracle](http://cx-oracle.sourceforge.net/)
* MS SQL Server: [pypyodbc](https://code.google.com/p/pypyodbc/), [pyodbc](https://code.google.com/p/pyodbc), [pymssql](http://pymssql.org)


Importing
---------

There are three common ways of importing the DB-API implementation library

    import databasepackage

or

    import databasepackage as db

or 

    from databasepackage import connect

In general, the only function used directly in the library is `connect`, as most other operations are performed on objects returned after calling this.

Connecting
----------

The two top-level objects when working with the DB-API are the *connection* and the *cursor*.  First you get a connection to a database:

     conn = db.connect()

There are several ways to specify the database connection parameters.  For most libraries, the default values for the connect method will connect to a default configured locally-installed database.  Some databases have their own options, like sqlite3 has the option for a non-persistent, in-memory database:

    conn = sqlite3.connect(':memory:')

Next you get a cursor, which will be used for executing transactional commands, SQL queries, and data manipulations. 

     cursor = conn.cursor()

The best way to use the connection and cursor are from within resource handlers.  Most database libraries support resource handling on the connection, but only a few support it on the cursor.  Using `with`, both the connection and cursor are closed after usage. 

    server_params = {'database': 'mydb',
                     'host': 'localhost', 'port': '5432',
                     'user': 'postgres', 'password': 'postgres'}
    with db.connect(**server_params) as conn:
        with conn.cursor() as curs:
          pass  # SQL commands go here

If only connection resource handling is supported, then the cursor must be wrapped in a try/finally block to ensure the cursor is closed:

    with sqlite3.connect(':memory:') as conn:
        curs = conn.cursor()
        try:
            pass  # SQL commands go here
        except Exception as e:
            print(e)
        finally:
            if curs:
                curs.close()

 If connection resource handling is not supported, both have close() methods which must be called as part of a finally block:

    conn = sqlite3.connect(':memory:')
    curs = conn.cursor()
    try:
        pass  # SQL commands go here  
    except Exception as e:
        print(e)
    finally:
        if conn:
            conn.close()
        if curs:
            curs.close()  

All libraries for databases that support transactions will automatically start a new one when the first statement on a new cursor or immediatly after a call to `commit()` on a cursor.  All cursors on the connection will execute within that transaction.  If using `with` for resource handling, the transaction will be committed at the end of the block.  If manually managing the resources, this transaction must be explicitly committed before closing the connection, or it will be automatically rolled back. Rollback and commit are done with the methods of the same name:

    conn.rollback()
    conn.commit() 

Autocommit can also be enabled by setting conn.autocommit = True in pyscopg2 after creating the connection but before the first execute.  

Exception handling can be done either with the generic `Exception` class or with classes specific to each library.


Query
-----

A cursor has only two methods, `execute` and `executemany`, which are used for all queries and DML:

    curs.execute("SELECT * FROM actors")

For queries which involve parameters, there are five styles of substitution built into the `execute` methods: 

* qmark 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (?, ?, ?)'
* numeric INSERT INTO actors(first_name, last_name, birth_date) VALUES (:1, :2, :3)'
* named 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (:first_name, :last_name, :birth_date)'
* format 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (%s, %s, %s)'
* pyformat 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (%(first_name)s, %(last_name)s, %(birth_date)s)'
 
It is highly encouraged to use one of these forms of substitution rather than doing direct string construction or replacement.  Using Python’s built-in formatting operators is not the correct way to do this.

Each DB-API is only required to support one of these, but most libraries support more than one.  

* sqlite3: qmark, numeric, and named
* pyscopg: format, pyformat
* PyMySQL: format
* cx_Oracle: named

If you want to tell at least one of the styles your DB-API library supports, each library has a global variable `paramstyle` that has the value, e.g., sqlite3.paramstyle 

Use placeholders in the statement, and then pass a tuple for positional parameters or a dictionary for named parameters.

qmark:

    curs.execute("SELECT * FROM actors where actor.first_name = ?", ("D'Angelo", ))

numeric:

    curs.execute("SELECT * FROM actors where actor.first_name = :1", ("D'Angelo", ))

named:

    curs.execute("SELECT * FROM actors where actor.first_name = :first_name",{'first_name': "D'Angelo"})

format:

    curs.execute("SELECT * FROM actors where actor.first_name = %s", ("D'Angelo", ))

pyformat:

    curs.execute("SELECT * FROM actors where actor.first_name = %(first_name)s",{'first_name': "D'Angelo"})


Calls to `excute` always returns None.  No results are actually pulled from the database until we make a call to fetch them. 

We use the fetch methods to get results of the query:

    curs.fetchall() # returns a list
    curs.fetchone() # returns an object
    curs.fetchmany(size=N) # returns a list

Different databases also provide proprietary extensions for functionality not specified in the DB-API.  For example, psycopg makes the `cursor` object iterable, so you can scalably loop over a potentionall large results set:

    curs.execute("SELECT * FROM actors where actor.first_name = %(first_name)s",{'first_name': "D'Angelo"})
    for row in curs:
        print(row)


Update, Insert, and Delete
--------------------------

Update

    curs.execute("INSERT INTO actors (last_name) VALUES (%s);", ("O'Connor", ))

The related executemany method can be used to provide multiple sets of parameters in one call:

    curs.executemany("INSERT INTO actors (last_name) VALUES (%s);", [("O'Connor", ), ("O'Neill", )]).


SQLite using sqlite3
----------------------

SQLite is supported with the [sqlite3](https://docs.python.org/3.4/library/sqlite3.html) module. 

Install

None required, as it comes packaged with Python 3. 

Import

    import sqlite3

Example

    import sqlite3 as db
    from sqlite3 import Error
    
    insert_qmark = 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (?, ?, ?)'
    insert_numeric = 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (:1, :2, :3)'
    insert_named = 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (:first_name, :last_name, :birth_date)'
    
    with db.connect(':memory:') as conn:
        curs = conn.cursor()
        try:
            curs.execute("CREATE TABLE actors (first_name varchar(100), last_name varchar(100), birth_date date)")
            curs.execute(insert_qmark, ('Michael', 'Palin', '1943-05-05'))
            curs.execute(insert_numeric, ('Graham', 'Chapman', '1941-01-08'))
            curs.execute(insert_named, {'first_name': 'John', 'last_name':'Cleese', 'birth_date':'1939-10-27'})
            curs.execute("SELECT * FROM actors WHERE first_name=?", ('John',))
            print(curs.fetchone())
        except Error as e:
            print("Error({0}): {1}".format(e.pgcode, e.pgerror))
        finally:
            if curs:
                curs.close()


PostgreSQL using psycopg2
-------------------------

PostgreSQL is supported with the [psycopg2](http://initd.org/psycopg/) module.

Install

> pip3 install psycopg2

Import

    import psycopg2

If running on Mac OS X, doing this import may give you an error about libssl not being of a high-enough version. To fix this, copy the libssl and libcrypto out of your PostgresSQL install dir to /usr/lib

> sudo cp /Library/PostgreSQL/9.2/lib/libssl.1.0.0.dylib /usr/lib
> sudo cp /Library/PostgreSQL/9.2/lib/libcrypto.1.0.0.dylib /usr/lib

And then update/create the symlinks in /usr/lib:

> sudo ln -fs /usr/lib/libssl.1.0.0.dylib /usr/lib/libssl.dylib
> sudo ln -fs /usr/lib/libcrypto.1.0.0.dylib /usr/lib/libcrypto.dylib     

The connect method supports either a set of parameters or a DSN string.  One way to specify the parameters is to create them within a dictionary and then destructure the dictionary with the double-astrisk operator.

Example using an empty database 'mydb':
    
    import psycopg2 as db
    from psycopg2 import Error

    print(db.paramstyle)

    insert_format = 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (%s, %s, %s)'
    insert_pyformat = 'INSERT INTO actors(first_name, last_name, birth_date) VALUES (%(first_name)s, %(last_name)s, %(birth_date)s)'

    server_params = {'database': 'mydb',
                     'host': 'localhost', 'port': '5432',
                     'user': 'postgres', 'password': 'postgres'}

    with db.connect(**server_params) as conn:
        with conn.cursor() as curs:
            try:
                curs.execute("CREATE TABLE IF NOT EXISTS actors (first_name varchar(100), last_name varchar(100), birth_date date)")
                curs.execute("TRUNCATE actors")
                curs.execute(insert_format, ('Eric', 'Idle', '1943-03-29'))
                curs.execute(insert_pyformat, {'first_name': ' Terry', 'last_name':'Jones', 'birth_date':'1942-02-01'})
                curs.execute("SELECT * FROM actors WHERE first_name=%s", ('Eric',))
                print(curs.fetchone())
            except Error as e:
                print("Error({0}): {1}".format(e.pgcode, e.pgerror))

MySQL 
--------

Several Python libraries for MySQL only support Python 2. These are the most commonly used libraries which support Python 3:

* [PyMySQL](https://github.com/PyMySQL/PyMySQL) : pure Python implementation, actively developed
* [mysql-connector-python](https://pypi.python.org/pypi/mysql-connector-python/1.0.9) : from Oracle

Oracle using cx_Oracle 
----------------------

* [cx_Oracle](http://cx-oracle.sourceforge.net/)

MS SQL Server
-------------

* [pypyodbc - A pure Python ODBC interface module based on ctypes](https://code.google.com/p/pypyodbc/)
* [pyodbc](https://code.google.com/p/pyodbc/) 
* [pymssql](http://pymssql.org/)
