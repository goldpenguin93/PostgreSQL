![postgresql-logo](https://user-images.githubusercontent.com/31435126/49341196-79422600-f68d-11e8-87d8-bbe9f80d270a.png)




<h2>It is PostgreSQL Tutorial.</h2>

https://www.tutorialspoint.com/postgresql/index.htm <ENG.VER>

# PostgreSQL


PostgreSQL, often simply Postgres, is an object-relational database management system (ORDBMS) with an emphasis on extensibility and standards compliance. It can handle workloads ranging from small single-machine applications to large Internet-facing applications (or for data warehousing) with many concurrent users; on macOS Server, PostgreSQL is the default database; and it is also available for Microsoft Windows and Linux (supplied in most distributions).

PostgreSQL is ACID-compliant and transactional. PostgreSQL has updatable views and materialized views, triggers, foreign keys; supports functions and stored procedures, and other expandability.

PostgreSQL is developed by the PostgreSQL Global Development Group, a diverse group of many companies and individual contributors. It is free and open-source, released under the terms of the PostgreSQL License, a permissive software license.

<h2>Development</h2>
PostgreSQL does not have a bug tracker (while it "has a bug-submission form that feeds into the pgsql-bugs mailing list"), making it quite difficult to know the status of bugs.

<h2>Multiversion concurrency control (MVCC)</h2>
PostgreSQL manages concurrency through a system known as multiversion concurrency control (MVCC), which gives each transaction a "snapshot" of the database, allowing changes to be made without being visible to other transactions until the changes are committed. This largely eliminates the need for read locks, and ensures the database maintains the ACID (atomicity, consistency, isolation, durability) principles in an efficient manner. PostgreSQL offers three levels of transaction isolation: Read Committed, Repeatable Read and Serializable. Because PostgreSQL is immune to dirty reads, requesting a Read Uncommitted transaction isolation level provides read committed instead. PostgreSQL supports full serializability via the serializable snapshot isolation (SSI) technique.

<h2>Storage and replication</h2>
  
<h2>Replication</h2>
PostgreSQL includes built-in binary replication based on shipping the changes (write-ahead logs) to replica nodes asynchronously, with the ability to run read-only queries against these replicated nodes. This allows splitting read traffic among multiple nodes efficiently. Earlier replication software that allowed similar read scaling normally relied on adding replication triggers to the master, introducing additional load onto it.

PostgreSQL also includes built-in synchronous replication that ensures that, for each write transaction, the master waits until at least one replica node has written the data to its transaction log. Unlike other database systems, the durability of a transaction (whether it is asynchronous or synchronous) can be specified per-database, per-user, per-session or even per-transaction. This can be useful for work loads that do not require such guarantees, and may not be wanted for all data as it will have some negative effect on performance due to the requirement of the confirmation of the transaction reaching the synchronous standby.

There can be a mixture of synchronous and asynchronous standby servers. A list of synchronous standby servers can be specified in the configuration which determines which servers are candidates for synchronous replication. The first in the list which is currently connected and actively streaming is the one that will be used as the current synchronous server. When this fails, it falls to the next in line.

Synchronous multi-master replication is currently not included in the PostgreSQL core. Postgres-XC which is based on PostgreSQL provides scalable synchronous multi-master replication, available in version 1.2.1 (April 2015 version) is licensed under the same license as PostgreSQL. A similar project is called Postgres-XL. Postgres-R is yet another older fork. Bi-directional replication (BDR) is an asynchronous multi-master replication system for PostgreSQL.

The community has also written some tools to make managing replication clusters easier, such as repmgr.

There are also several asynchronous trigger-based replication packages for PostgreSQL. These remain useful even after introduction of the expanded core capabilities, for situations where binary replication of an entire database cluster is not the appropriate approach:

Slony-I
Londiste, part of SkyTools (developed by Skype)
Bucardo multi-master replication (developed by Backcountry.com)
SymmetricDS multi-master, multi-tier replication
<h2>Indexes</h2>
PostgreSQL includes built-in support for regular B-tree and hash indexes, and four index access methods: generalized search trees (GiST), generalized inverted indexes (GIN), Space-Partitioned GiST (SP-GiST) and Block Range Indexes (BRIN). Hash indexes are implemented, but discouraged because they cannot be recovered after a crash or power loss, although this will no longer be the case from version 10. In addition, user-defined index methods can be created, although this is quite an involved process. Indexes in PostgreSQL also support the following features:

Expression indexes can be created with an index of the result of an expression or function, instead of simply the value of a column.
Partial indexes, which only index part of a table, can be created by adding a WHERE clause to the end of the CREATE INDEX statement. This allows a smaller index to be created.
The planner is capable of using multiple indexes together to satisfy complex queries, using temporary in-memory bitmap index operations (useful in data warehousing applications for joining a large fact table to smaller dimension tables such as those arranged in a star schema).
k-nearest neighbors (k-NN) indexing (also referred to KNN-GiST) provides efficient searching of "closest values" to that specified, useful to finding similar words, or close objects or locations with geospatial data. This is achieved without exhaustive matching of values.
Index-only scans often allow the system to fetch data from indexes without ever having to access the main table.
PostgreSQL 9.5 introduced Block Range Indexes (BRIN).
<h2>Schemas</h2>
In PostgreSQL, a schema holds all objects (with the exception of roles and tablespaces). Schemas effectively act like namespaces, allowing objects of the same name to co-exist in the same database. By default, newly created databases have a schema called "public", but any additional schemas can be added, and the public schema isn't mandatory.

A search_path setting determines the order in which PostgreSQL checks schemas for unqualified objects (those without a prefixed schema). By default, it is set to $user, public ($user refers to the currently connected database user). This default can be set on a database or role level, but as it is a session parameter, it can be freely changed (even multiple times) during a client session, affecting that session only.

Non-existent schemas listed in search_path are silently skipped during objects lookup.

New objects are created in whichever valid schema (one that presently exists) appears first in the search_path.Schema is an outline of database.

<h2>Data types</h2>
A wide variety of native data types are supported, including:

Boolean
Arbitrary precision numerics
Character (text, varchar, char)
Binary
Date/time (timestamp/time with/without timezone, date, interval)
Money
Enum
Bit strings
Text search type
Composite
HStore, an extension enabled key-value store within PostgreSQL
Arrays (variable length and can be of any data type, including text and composite types) up to 1 GB in total storage size
Geometric primitives
IPv4 and IPv6 addresses
CIDR blocks and MAC addresses
XML supporting XPath queries
UUID
JSON, and a faster binary JSONB (since version 9.4; not the same as BSON
In addition, users can create their own data types which can usually be made fully indexable via PostgreSQL's indexing infrastructures – GiST, GIN, SP-GiST. Examples of these include the geographic information system (GIS) data types from the PostGIS project for PostgreSQL.

There is also a data type called a "domain", which is the same as any other data type but with optional constraints defined by the creator of that domain. This means any data entered into a column using the domain will have to conform to whichever constraints were defined as part of the domain.

A data type that represents a range of data can be used which are called range types. These can be discrete ranges (e.g. all integer values 1 to 10) or continuous ranges (e.g. any point in time between 10:00 am and 11:00 am). The built-in range types available include ranges of integers, big integers, decimal numbers, time stamps (with and without time zone) and dates.

Custom range types can be created to make new types of ranges available, such as IP address ranges using the inet type as a base, or float ranges using the float data type as a base. Range types support inclusive and exclusive range boundaries using the [/] and (/) characters respectively. (e.g., [4,9) represents all integers starting from and including 4 up to but not including 9.) Range types are also compatible with existing operators used to check for overlap, containment, right of etc.

<h2>User-defined objects</h2>
New types of almost all objects inside the database can be created, including:

Casts
Conversions
Data types
Domains
Functions, including aggregate functions and window functions
Indexes including custom indexes for custom types
Operators (existing ones can be overloaded)
Procedural languages
<h2>Inheritance</h2>
Tables can be set to inherit their characteristics from a "parent" table. Data in child tables will appear to exist in the parent tables, unless data is selected from the parent table using the ONLY keyword, i.e. SELECT * FROM ONLY parent_table;. Adding a column in the parent table will cause that column to appear in the child table.

Inheritance can be used to implement table partitioning, using either triggers or rules to direct inserts to the parent table into the proper child tables.

As of 2010, this feature is not fully supported yet – in particular, table constraints are not currently inheritable. All check constraints and not-null constraints on a parent table are automatically inherited by its children. Other types of constraints (unique, primary key, and foreign key constraints) are not inherited.

Inheritance provides a way to map the features of generalization hierarchies depicted in entity relationship diagrams (ERDs) directly into the PostgreSQL database.

<h2>Other storage features</h2>
Referential integrity constraints including foreign key constraints, column constraints, and row checks
Binary and textual large-object storage
Tablespaces
Per-column collation
Online backup
Point-in-time recovery, implemented using write-ahead logging
In-place upgrades with pg_upgrade for less downtime (supports upgrades from 8.3.x and later)
<h2>Control and connectivity</h2>
<h2>Foreign data wrappers</h2>
PostgreSQL can link to other systems to retrieve data via foreign data wrappers (FDWs). These can take the form of any data source, such as a file system, another RDBMS, or a web service. This means that regular database queries can use these data sources like regular tables, and even join multiple data-sources together.

<h2>Interfaces</h2>
PostgreSQL has several interfaces available and is also widely supported among programming language libraries. Built-in interfaces include libpq (PostgreSQL's official C application interface) and ECPG (an embedded C system). External interfaces include:

libpqxx: C++ interface
Pgfe: C++ interface
PostgresDAC: PostgresDAC (for Embarcadero RadStudio/Delphi/CBuilder XE-XE3)
DBD::Pg: Perl DBI driver
JDBC: JDBC interface
Lua: Lua interface
Npgsql: .NET data provider
ST-Links SpatialKit: Link Tool to ArcGIS
PostgreSQL.jl: Julia interface
node-postgres: Node.js interface
pgoledb: OLEDB interface
psqlODBC: ODBC interface
psycopg2: Python interface (also used by HTSQL)
pgtclng: Tcl interface
pyODBC: Python library
php5-pgsql: PHP driver based on libpq
postmodern: A Common Lisp interface
pq: A pure Go PostgreSQL driver for the Go database/sql package. The driver passes the compatibility test suite.
RPostgreSQL: R interface
dpq: D interface to libpq
epgsql: Erlang interface
<h2>Procedural languages</h2>
Procedural languages allow developers to extend the database with custom subroutines (functions), often called stored procedures. These functions can be used to build triggers (functions invoked upon modification of certain data) and custom aggregate functions. Procedural languages can also be invoked without defining a function, using the DO command at SQL level.

Languages are divided into two groups: "Safe" languages are sandboxed and can be safely used by any user. Procedures written in "unsafe" languages can only be created by superusers, because they allow bypassing the database's security restrictions, but can also access sources external to the database. Some languages like Perl provide both safe and unsafe versions.

PostgreSQL has built-in support for three procedural languages:

Plain SQL (safe). Simpler SQL functions can get expanded inline into the calling (SQL) query, which saves function call overhead and allows the query optimizer to "see inside" the function.
PL/pgSQL (safe), which resembles Oracle's PL/SQL procedural language and SQL/PSM.
C (unsafe), which allows loading custom shared libraries into the database. Functions written in C offer the best performance, but bugs in code can crash and potentially corrupt the database. Most built-in functions are written in C.
In addition, PostgreSQL allows procedural languages to be loaded into the database through extensions. Three language extensions are included with PostgreSQL to support Perl, Python and Tcl. There are external projects to add support for many other languages,including Java, JavaScript (PL/V8), R, Ruby, and others.

<h2>Triggers</h2>
Triggers are events triggered by the action of SQL DML statements. For example, an INSERT statement might activate a trigger that checks if the values of the statement are valid. Most triggers are only activated by either INSERT or UPDATE statements.

Triggers are fully supported and can be attached to tables. Triggers can be per-column and conditional, in that UPDATE triggers can target specific columns of a table, and triggers can be told to execute under a set of conditions as specified in the trigger's WHERE clause. Triggers can be attached to views by using the INSTEAD OF condition. Multiple triggers are fired in alphabetical order. In addition to calling functions written in the native PL/pgSQL, triggers can also invoke functions written in other languages like PL/Python or PL/Perl.

<h2>Asynchronous notifications</h2>
PostgreSQL provides an asynchronous messaging system that is accessed through the NOTIFY, LISTEN and UNLISTEN commands. A session can issue a NOTIFY command, along with the user-specified channel and an optional payload, to mark a particular event occurring. Other sessions are able to detect these events by issuing a LISTEN command, which can listen to a particular channel. This functionality can be used for a wide variety of purposes, such as letting other sessions know when a table has updated or for separate applications to detect when a particular action has been performed. Such a system prevents the need for continuous polling by applications to see if anything has yet changed, and reducing unnecessary overhead. Notifications are fully transactional, in that messages are not sent until the transaction they were sent from is committed. This eliminates the problem of messages being sent for an action being performed which is then rolled back.

Many of the connectors for PostgreSQL provide support for this notification system (including libpq, JDBC, Npgsql, psycopg and node.js) so it can be used by external applications.

<h2>Rules</h2>
Rules allow the "query tree" of an incoming query to be rewritten. Rules, or more properly, "Query Re-Write Rules", are attached to a table/class and "Re-Write" the incoming DML (select, insert, update, and/or delete) into one or more queries that either replace the original DML statement or execute in addition to it. Query Re-Write occurs after DML statement parsing, but before query planning.

<h2>Other querying features</h2>
Transactions
Full-text search
Views
Materialized views
Updateable views
Recursive views
Inner, outer (full, left and right), and cross joins
Sub-selects
<h2>Correlated sub-queries</h2>
Regular expressions
Common table expressions and writable common table expressions
Encrypted connections via TLS (current versions do not use vulnerable SSL, even with that configuration option)
Domains
Savepoints
Two-phase commit
TOAST (The Oversized-Attribute Storage Technique) is used to transparently store large table attributes (such as big MIME attachments or XML messages) in a separate area, with automatic compression.
Embedded SQL is implemented using preprocessor. SQL code is first written embedded into C code. Then code is run through ECPG preprocessor, which replaces SQL with calls to code library. Then code can be compiled using a C compiler. Embedding works also with C++ but it does not recognize all C++ constructs.
<h2>Concurrency model</h2>
PostgreSQL server is process-based (not threaded), and uses one operating system process per database session. Multiple sessions are automatically spread across all available CPUs by the operating system. Starting with PostgreSQL 9.6, many types of queries can also be parallelized across multiple background worker processes, taking advantage of multiple CPUs or cores. Client applications can use threads and create multiple database connections from each thread.

<h2>Security</h2>
PostgreSQL manages its internal security on a per-role basis. A role is generally regarded to be a user (a role that can log in), or a group (a role of which other roles are members). Permissions can be granted or revoked on any object down to the column level, and can also allow/prevent the creation of new objects at the database, schema or table levels.

PostgreSQL's SECURITY LABEL feature (extension to SQL standards), allows for additional security; with a bundled loadable module that supports label-based mandatory access control (MAC) based on SELinux security policy.

PostgreSQL natively supports a broad number of external authentication mechanisms, including:

password (either MD5 or plain-text)
GSSAPI
SSPI
Kerberos
ident (maps O/S user-name as provided by an ident server to database user-name)
peer (maps local user name to database user name)
LDAP
Active Directory
RADIUS
certificate
PAM
The GSSAPI, SSPI, Kerberos, peer, ident and certificate methods can also use a specified "map" file that lists which users matched by that authentication system are allowed to connect as a specific database user.

These methods are specified in the cluster's host-based authentication configuration file (pg_hba.conf), which determines what connections are allowed. This allows control over which user can connect to which database, where they can connect from (IP address/IP address range/domain socket), which authentication system will be enforced, and whether the connection must use TLS.

<h2>Standards compliance</h2>
PostgreSQL claims high, but not complete, conformance with the SQL standard. One exception is the handling of unquoted identifiers like table or column names. In PostgreSQL they are folded – internal – to lower case characters whereas the standard says that unquoted identifiers should be folded to upper case. Thus, Foo should be equivalent to FOO not foo according to the standard.

<h2>Benchmarks and performance</h2>
Many informal performance studies of PostgreSQL have been done. Performance improvements aimed at improving scalability started heavily with version 8.1. Simple benchmarks between version 8.0 and version 8.4 showed that the latter was more than 10 times faster on read-only workloads and at least 7.5 times faster on both read and write workloads.

The first industry-standard and peer-validated benchmark was completed in June 2007, using the Sun Java System Application Server (proprietary version of GlassFish) 9.0 Platform Edition, UltraSPARC T1-based Sun Fire server and PostgreSQL 8.2. This result of 778.14 SPECjAppServer2004 JOPS@Standard compares favourably with the 874 JOPS@Standard with Oracle 10 on an Itanium-based HP-UX system.

In August 2007, Sun submitted an improved benchmark score of 813.73 SPECjAppServer2004 JOPS@Standard. With the system under test at a reduced price, the price/performance improved from $84.98/JOPS to $70.57/JOPS.

The default configuration of PostgreSQL uses only a small amount of dedicated memory for performance-critical purposes such as caching database blocks and sorting. This limitation is primarily because older operating systems required kernel changes to allow allocating large blocks of shared memory. PostgreSQL.org provides advice on basic recommended performance practice in a wiki.

In April 2012, Robert Haas of EnterpriseDB demonstrated PostgreSQL 9.2's linear CPU scalability using a server with 64 cores.

Matloob Khushi performed benchmarking between Postgresql 9.0 and MySQL 5.6.15 for their ability to process genomic data. In his performance analysis he found that PostgreSQL extracts overlapping genomic regions eight times faster than MySQL using two datasets of 80,000 each forming random human DNA regions. Insertion and data uploads in PostgreSQL were also better, although general searching capability of both databases was almost equivalent.

<h2>Platforms</h2>
PostgreSQL is available for the following operating systems: Linux (all recent distributions), Windows (Windows 2000 SP4 and later; compilable by e.g. Visual Studio, now with up to most recent 2017 version), FreeBSD, OpenBSD,[59] NetBSD, OS X (macOS),[11] AIX, HP-UX, Solaris, and UnixWare; and not officially tested: DragonFly BSD, BSD/OS, IRIX, OpenIndiana,[60] OpenSolaris, OpenServer, and Tru64 Unix. Most other Unix-like systems could also work; most modern do support.

PostgreSQL works on any of the following instruction set architectures: x86 and x86-64 on Windows and other operating systems; these are supported on other than Windows: IA-64 Itanium (external support for HP-UX), PowerPC, PowerPC 64, S/390, S/390x, SPARC, SPARC 64, ARMv8-A (64-bit)and older ARM (32-bit, including older such as ARMv6 in Raspberry Pi[62]), MIPS, MIPSel, and PA-RISC. It is also known to work on Alpha (dropped in 9.5), M68k, M32R, NS32k, and VAX. In addition to these, it is possible to build PostgreSQL for an unsupported CPU by disabling spinlocks.

<h2>Database administration</h2>
See also: Comparison of database tools
Open source front-ends and tools for administering PostgreSQL include:

<h2>psql</h2>
The primary front-end for PostgreSQL is the psql command-line program, which can be used to enter SQL queries directly, or execute them from a file. In addition, psql provides a number of meta-commands and various shell-like features to facilitate writing scripts and automating a wide variety of tasks; for example tab completion of object names and SQL syntax.
pgAdmin
The pgAdmin package is a free and open-source graphical user interface administration tool for PostgreSQL, which is supported on many computer platforms.The program is available in more than a dozen languages. The first prototype, named pgManager, was written for PostgreSQL 6.3.2 from 1998, and rewritten and released as pgAdmin under the GNU General Public License (GPL) in later months. The second incarnation (named pgAdmin II) was a complete rewrite, first released on January 16, 2002. The third version, pgAdmin III, was originally released under the Artistic License and then released under the same license as PostgreSQL. Unlike prior versions that were written in Visual Basic, pgAdmin III is written in C++, using the wxWidgets framework allowing it to run on most common operating systems. The query tool includes a scripting language called pgScript for supporting admin and development tasks. In December 2014, Dave Page, the pgAdmin project founder and primary developer, announced that with the shift towards web-based models work has started on pgAdmin 4 with the aim of facilitating Cloud deployments.In 2016, pgAdmin 4 was released. pgAdmin 4 backend was written in Python, using Flask and Qt framework.
<h2>phpPgAdmin</h2>
phpPgAdmin is a web-based administration tool for PostgreSQL written in PHP and based on the popular phpMyAdmin interface originally written for MySQL administration.
<h2>PostgreSQL Studio</h2>
PostgreSQL Studio allows users to perform essential PostgreSQL database development tasks from a web-based console. PostgreSQL Studio allows users to work with cloud databases without the need to open firewalls.
<h2>TeamPostgreSQL</h2>
AJAX/JavaScript-driven web interface for PostgreSQL. Allows browsing, maintaining and creating data and database objects via a web browser. The interface offers tabbed SQL editor with auto-completion, row-editing widgets, click-through foreign key navigation between rows and tables, "favorites" management for commonly used scripts, among other features. Supports SSH for both the web interface and the database connections. Installers are available for Windows, Mac and Linux, as well as a simple cross-platform archive that runs from a script
<h2>LibreOffice/OpenOffice.org Base</h2>
LibreOffice/OpenOffice.org Base can be used as a front-end for PostgreSQL.
<h2>pgBadger</h2>
The pgBadger PostgreSQL log analyzer generates detailed reports from a PostgreSQL log file.
<h2>pgDevOps</h2>
pgDevOps is a suite of web tools to install & manage multiple PostgreSQL versions, extensions, and community components, develop SQL queries, monitor running databases and find performance problems
A number of companies offer proprietary tools for PostgreSQL. They often consist of a universal core that is adapted for various specific database products. These tools mostly share the administration features with the open source tools but offer improvements in data modeling, importing, exporting or reporting.


<h2>Reference site<h2>
https://www.wikipedia.org/
