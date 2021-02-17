---
id: watchers
title: Watchers
---

We support to use distributed messaging systems like [etcd](https://github.com/coreos/etcd) to keep consistence between multiple Casbin enforcer instances. So our users can concurrently use multiple Casbin enforcers to handle large number of permission checking requests.

Similar to policy storage adapters, we don't put watcher code in the main library. Any support for a new messaging system should be implemented as a watcher. A complete list of Casbin watchers is provided as below. Any 3rd-party contribution on a new watcher is welcomed, please inform us and I will put it in this list:)

<!--DOCUSAURUS_CODE_TABS-->

<!--Go-->
| Watcher                                                                                                            | Type             | Author                                         | Description                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------ | ---------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [Etcd Watcher](https://github.com/casbin/etcd-watcher)                                                             | KV store         | Casbin                                         | Watcher for [etcd](https://github.com/coreos/etcd)                                                                               |
| [NATS Watcher](https://github.com/Soluto/casbin-nats-watcher)                                                      | Messaging system | [Soluto](https://github.com/Soluto)            | Watcher for [NATS](https://nats.io/)                                                                                             |
| [ZooKeeper Watcher](https://github.com/grepsr/casbin-zk-watcher)                                                   | KV store         | [Grepsr](https://github.com/grepsr)            | Watcher for [Apache ZooKeeper](https://zookeeper.apache.org/)                                                                    |
| [Redis Watcher](https://github.com/billcobbler/casbin-redis-watcher)                                               | KV store         | [@billcobbler](https://github.com/billcobbler) | Watcher for [Redis](http://redis.io/)                                                                                            |
| [NATS, RabbitMQ, GCP Pub/Sub, AWS SNS & SQS, Kafka, InMemory](https://github.com/rusenask/casbin-go-cloud-watcher) | Messaging System | [rusenask](https://github.com/rusenask/)       | Watcher based on [Go Cloud Dev Kit](https://gocloud.dev/) that works with leading cloud providers and self-hosted infrastructure |

<!--Java-->
| Watcher                                                    | Type     | Author                                     | Description                                        |
| ---------------------------------------------------------- | -------- | ------------------------------------------ | -------------------------------------------------- |
| [Etcd Adapter](https://github.com/mapleafgo/jcasbin-extra) | KV store | [@mapleafgo](https://github.com/mapleafgo) | Watcher for [etcd](https://github.com/coreos/etcd) |

<!--Node.js-->
| Watcher                                                           | Type             | Author                                         | Description                                                              |
| ----------------------------------------------------------------- | ---------------- | ---------------------------------------------- | ------------------------------------------------------------------------ |
| [Etcd Watcher](https://github.com/node-casbin/etcd-watcher)       | KV store         | Casbin                                         | Watcher for [etcd](https://github.com/coreos/etcd)                       |
| [Redis Watcher](https://github.com/node-casbin/redis-watcher)     | KV store         | Casbin                                         | Watcher for [Redis](http://redis.io/)                                    |
| [Pub/Sub Watcher](https://github.com/node-casbin/pubsub-watcher)  | Messaging system | Casbin                                         | Watcher for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/docs) |
| [Postgres Watcher](https://github.com/mcollina/casbin-pg-watcher) | Database         | [Matteo Collina](https://github.com/mcollina/) | Watcher for [PostgreSQL](https://www.postgresql.org/)                    |

<!--Python-->
| Watcher                                                                     | Type     | Author                                          | Description                                           |
| --------------------------------------------------------------------------- | -------- | ----------------------------------------------- | ----------------------------------------------------- |
| [Redis Watcher](https://github.com/ScienceLogic/flask-casbin-redis-watcher) | KV store | [ScienceLogic](https://github.com/ScienceLogic) | Watcher for [Redis](http://redis.io/)                 |
| [PostgreSQL Watcher](https://github.com/pycasbin/postgresql-watcher)        | Database | Casbin                                          | Watcher for [PostgreSQL](https://www.postgresql.org/) |

<!--.NET-->
| Watcher                                                           | Type     | Author                           | Description                           |
| ----------------------------------------------------------------- | -------- | -------------------------------- | ------------------------------------- |
| [Redis Watcher](https://github.com/Sbou/Casbin.NET-Redis-Watcher) | KV store | [@Sbou](https://github.com/Sbou) | Watcher for [Redis](http://redis.io/) |

<!--END_DOCUSAURUS_CODE_TABS-->

## WatcherEx

In order to support incremental synchronization between multiple instances, we provide the `WatcherEx` interface. We hope it can notify other instances when the policy changes, but there is currently no implementation of `WatcherEx`. We recommend that you use dispatcher to achieve this. 
