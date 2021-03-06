---
title: Upgrade procedures
---

This document contains administration procedure for each version upgrade.
Only upgrade from version N to N+1 is documented and supported.
If you upgrade from an older version than previous one, you have to review all upgrade notes and apply each steps one by one for the possible database changes.
You also have to stop your old ejabberd server, and start the new one.

Until release note explicitely state you must restart the server for upgrade, you should be able to run soft upgrade using a cluster.
If you don't have cluster, upgrade from older release than previous one, or have explicite note soft upgrade does not work, then you have to fallback to standalone upgrade process.

# Standalone upgrade process

This is the simplest process, and require service restart.

- read the upgrade note
- apply the required changes in database from the upgrade note.
- stop old node
- archive content of mnesia database directory (database, /opt/ejabberd-XX.YY/database, /usr/local/var/lib/ejabberd, ...)
- install new version
- extract database archive in new path
- start new node

# Soft upgrade process

This process needs you to run in cluster, with at lease two nodes. In this case, we assume you run node A and B with version N, and will upgrade to version N+1.

- read the upgrade note, make sure it does not explicitely states "soft upgrade is not supported".
- apply the required changes in database from the upgrade note.
- make sure node A is running
- run `leave_cluster` command on node B
- stop old node B
- install new version on B's host
- start new node B
- run `join_cluster` command on node B, passing node A as parameter
- make sure both nodes are running and working as expected
- run `leave_cluster` command on node A
- stop old node A
- install new version on A's host
- start new node A
- run `join_cluster` command on node A, passing node B as parameter
