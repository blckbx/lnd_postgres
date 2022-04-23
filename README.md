# ⚡ Guide: How to setup LND with PostgreSQL


## 📄 Prelude and Objective ##
A short guide on how to setup and configure a *new* lightning node using LND implementation with newly released PostgreSQL support as the default database backend. A short write-up was released by Lightning Labs: [postgres.md](https://github.com/lightningnetwork/lnd/blob/master/docs/postgres.md)

## 📜 Table of Content ##

- [Preconditions](#preconditions)
- [Setting things up](#setting-things-up)


## 🔎 Preconditions:

This guide is based on the following setup and components. It may be different to Umbrel / Raspiblitz / MyNode / etc. frameworks as it was established on a bolt/bare metal setup running on Ubuntu. 

- bare metal / bolt setup
- lnd-0.14.3-beta
- PostgreSQL 14.2 (latest version in ubuntu's repository)
- new node without funds and channels (⚠)

It's important to mention that this setup was built as a new lightning node without existing funds or channels. Backend migrations are not supported yet by LND (as of v0.14.3).

## 🛠 Setting things up:






_______________________________________________________________

Written by [osito](https://github.com/blckbx). If this guide was of help and you want to share some ♥ and appreciation, please feel free to send a small tip to the ⚡ address: 0x382f9cf667447bb8@ln.tips

[Greetings to the ⚡ Plebs!](https://t.me/plebnet)
