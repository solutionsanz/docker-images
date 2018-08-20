# About this Docker Image

Note: This Docker Image extends the base capabilities of the offical Oracle OracleInstantClient Docker image by including a reference to the ADW credential wallet file and the TNS_ADMIN environment variable required for connectivity to the Oracle Autonomous Data Warehouse.

This Docker image contains the Oracle Instant Client 'Basic', 'SDK' and 'SQL*Plus' packages.  It can be extended to run OCI, OCCI, and JDBC applications.  It can also be extended to build and run scripting language drivers that use OCI such as Python's cx_Oracle, Node.js's node-oracledb, PHP's OCI8, and Ruby's ruby-oci8.  

The SQL*Plus command-line query tool is also included, allowing quick ad-hoc SQL and PL/SQL execution.

## About Oracle Instant Client

[Oracle Instant Client](http://www.oracle.com/technetwork/database/features/instant-client/) is a repackaging of Oracle Database libraries, tools and header files usable to create and run applications that connect to a remote (or local) Oracle Database.

## Refer https://redthunder.blog/2018/08/19/connect-dockerised-instant-client-to-autonomous-data-warehouse/
## If you are not using the solutionsanz/vagrant-boxes/DockerEngine approach then you will need to make a directory for the credential wallet

## Required files

Download the Oracle Instant Client RPMs from OTN into the same directory as this Dockerfile:

http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html

The following three RPMs are required:

- `oracle-instantclient<version>-basic-<version>-1.x86_64.rpm`
- `oracle-instantclient<version>-devel-<version>-1.x86_64.rpm`
- `oracle-instantclient<version>-sqlplus-<version>-1.x86_64.rpm`

## Download the ADW Credential Wallet from Oracle Cloud ADW Instance into the same directory as this Dockerfile

The name of the wallet is the same as the ADW instance name eg Instance Name DB201807201207 has a wallet named 
- `wallet_DB201807201207.zip`

Create a sub-directory under the directory holding the Dockerfile and download / unzip the wallet into ./adw_wallet directory

## Building

With the Oracle Instant Client RPMs downloaded into the same directory as the Dockerfile and with the ADW Credential Wallet (zip) downloaded and unzipped into a sub directory (./adw_wallet) as the `Dockerfile` and run:

```
docker build -t oracle/instantclient-adw:<version> .
```

For example, to build an 18.3 Instance Client Docker Image, run:

```
docker build -t oracle/instantclient-adw:18.3.0 .
```


## Usage

You can run a container interactively to execute ad-hoc SQL and PL/SQL statements in SQL*Plus:

```
docker run -ti --rm oracle/instantclient-adw:18.3.0 sqlplus username@servicename

or

docker run -it --rm oracle/instantclient-adw:18.3.0 bash

```

## Adding Oracle Database Drivers

To extend the image with optional Oracle Database drivers, follow your desired driver installation steps.  
The Instant Client libraries are in `/usr/lib/oracle/<version>/client64/lib` and 
the Instant Client headers are in `/usr/include/oracle/<version>/client64/`.

The Instant Client libraries are in the default library search path.
