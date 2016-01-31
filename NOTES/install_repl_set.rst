Replica Set on host1, host2 & host3
===================================

OS: CentOS release 6.7 (Final)

Add repo and install
--------------------

On all hosts:
:: 

  sudo vim /etc/yum.repos.d/mongodb.repo

Edit file:
::

  [mongodb]
  name=MongoDB Repository
  baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64/
  gpgcheck=0
  enabled=1

Install:
::

  yum install mongo-10gen mongo-10gen-server

On master (host1)
====================

Flush firewall rules
::

  sudo iptables -F

Start master
::

  sudo mkdir -p /data/masterdb/
  sudo mongod --master --dbpath /data/masterdb/ --bind_ip <host1 ip>

On slaves (host2 & host3)
----------------------------

Start slaves
::

  sudo vim /etc/hosts                         # Add host1 ip address
  sudo mkdir -p /data/slavedb/
  mongod --slave --source host1:27017 --dbpath /data/slavedb/

Hello World Example
===================

On host1:
::

  mongo --host <host1 bind ip address>

Mongo terminal:
::

  MongoDB shell version: 2.6.11
  connecting to: <ip>:<port>/test
  > use test
  switched to db test
  > db.first_collection.insert({'hello':'world'})
  WriteResult({ "nInserted" : 1 })

On host2 & host3
::

  MongoDB shell version: 2.6.11
  connecting to: test
  > use test
  switched to db test
  > db.first_collection.find()
  { "_id" : ObjectId("56ae226236e73ef652031b22"), "hello" : "world" }

