# ⚡ Guide: How to setup LND with PostgreSQL


## 📄 Prelude and Objective ##
A short guide on how to setup and configure a **_new_** lightning node using LND implementation with recently released PostgreSQL support as database backend. A brief write-up was released by Lightning Labs here: [postgres.md](https://github.com/lightningnetwork/lnd/blob/master/docs/postgres.md)

## 📜 Table of Content ##

- [Preconditions](#-preconditions)
- [Install and setup PostgreSQL](#-install-and-setup-postgresql)
- [Configure LND](#-configure-lnd)
- [Upgrade PostgreSQL](#-upgrade-postgresql)
- [Addendum](#-addendum)


## 🔎 Preconditions:

This guide is based on the following setup and components. It may be different to Umbrel / Raspiblitz / MyNode / etc. frameworks as it was established on a bolt/bare metal setup running on Ubuntu. 

- setup: bare metal / bolt setup
- lightning implementation: lnd-0.14.3-beta
- database backend: PostgreSQL 14.2 (latest version in ubuntu's repository)
- **node state: new node without funds and channels (⚠)**

It's important to mention that this is initially built as a new lightning node without existing funds or channels. Migrations between different backends (bbolt, postgres, etcd) are not yet supported (as of v0.14.3).

## 🛠 Install and setup PostgreSQL

Installation process of LND is omitted in this guide as this is widely described on the internet. So I assume a valid installation here and continue to setup the backend:

```sh
# Install postgresql-14 

$ sudo apt install postgresql-14 postgresql-server-dev-14

# Configure postgresql to your needs (port, datadir, logging, etc.) or just leave it all to default.

$ sudo nano /etc/postgresql/14/main/postgresql.conf

# Setup postgres user

$ sudo -i -u postgres
$ psql

postgres=# \password

# Take note of your password!

# Quit PSQL
\q

```

Create user and database for LND:

```sh
# As postgres user

$ sudo -i -u postgres

# Create user 'lnd' and set a password
$ createuser --createdb --pwprompt lnd

# Enter password and take note of it!

# Create database 'lndb' 
$ createdb -O lnd lndb

# Exit user postgres
$ exit
```

## 🛠 Configure LND:

Configure your `lnd.conf`:

```sh
$ sudo nano /home/lnd/.lnd/lnd.conf
```

Edit and add your postgresql config and credentials, like so: postgresql://<user>:<pw>@<ip>:<port>/<db>

```ini
[db]
db.backend=postgres
db.postgres.dsn=postgresql://lnd:password@127.0.0.1:5432/lndb
db.postgres.timeout=0
```

If everything went fine, you can see this in the `lnd.log`:
```
[INF] LTND: Opening the main database, this might take a few minutes...
[INF] LTND: Using remote postgres database! Creating graph and channel state DB instances
```

## ♻ Upgrade PostgreSQL
  
Upgrading PostgreSQL should be easy using Postgres' own tool `pg_upgrade`. Install and setup next version in parallel. Upgrade like the following procedure (`--check` = dry-run) from v13 to v14:

```
/usr/lib/postgresql/14/bin/pg_upgrade \
  --old-datadir=/var/lib/postgresql/13/main \
  --new-datadir=/var/lib/postgresql/14/main \
  --old-bindir=/usr/lib/postgresql/13/bin \
  --new-bindir=/usr/lib/postgresql/14/bin \
  --old-options '-c config_file=/etc/postgresql/13/main/postgresql.conf' \
  --new-options '-c config_file=/etc/postgresql/14/main/postgresql.conf' \
  --check
```

## ✴ Addendum: 
  
- [Upgrading PostgreSQL 13 to 14](https://www.kostolansky.sk/posts/upgrading-to-postgresql-14/)
- [Advanced setup with replication (two instances)](https://github.com/gabridome/docs/blob/master/c-lightning_with_postgresql_reliability.md)
  
_______________________________________________________________

Written by [osito](https://github.com/blckbx). If this guide was of help and you want to share some ♥ and appreciation, please feel free to send a small tip to the ⚡ address: 0x382f9cf667447bb8@ln.tips

[Greetings to the ⚡ Plebs!](https://t.me/plebnet)
