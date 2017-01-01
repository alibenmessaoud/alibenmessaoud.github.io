---
layout: post
title: ACID Transactions
tags: acid database db
---

Programming is about managing data that point to the state of a system. Databases are the right place to store data. So they helped us to build applications pretty easily and to manage the state of systems. But with the nature of the applications, handling the storage is not that easy though, because anything can happen, from failures to concurrency issues. Transactions are the answer to these issues. In this article, we will see a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps: `ACID`.

`ACID` appeared first in a paper published in the 80s called [Principles of transaction-oriented database recovery, Theo Haerder, and Andreas Reuter](https://sites.fas.harvard.edu/~cs265/papers/haerder-1983.pdf):

> **A**tomic, **C**onsistency, **I**solation, **D**urability

## 1. Atomic

Atomic represents something that cannot be split; hence, all the instructions have to be successfully executed, or none of them will execute. For example, a transfer from Alice's account to Bob's account fails on updating Bob's account must do a rollback.

## 2. Consistency

 Consistency refers to the fact that a transaction will bring the database from one valid state to another. Hence a database is initially in a consistent state, and it should remain consistent after every transaction.

## 3. Isolation

Are you the only user? I don’t think so. So if the multiple transactions are running at the same time, they should not be affected by each other; i.e., the result should be the same as the result obtained if the transactions were running sequentially.

## 4. Durability

Durability makes sure that the data can be stored without fear of losing it. If a transaction has committed successfully, any written data should remain even in the case of software and hardware failure.

## Conclusion

The ACID properties make sure that databases retain their sustainability. These checks ensure the execution of a single entity of work, with consistency each time, isolated from one another, be stored in disks that are checked for durability over time.