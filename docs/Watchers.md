---
id: watchers
title: Watchers
---

We support to use distributed messaging systems like [etcd](https://github.com/coreos/etcd) to keep consistence between multiple Casbin enforcer instances. So our users can concurrently use multiple Casbin enforcers to handle large number of permission checking requests.

Similar to policy storage adapters, we don't put watcher code in the main library. Any support for a new messaging system should be implemented as a watcher. A complete list of Casbin watchers is provided as below. Any 3rd-party contribution on a new watcher is welcomed, please inform us and I will put it in this list:)

<!--DOCUSAURUS_CODE_TABS-->

<!--Go-->
Watcher | Type | Author | Description
----|------|----|----
[Etcd Watcher](https://github.com/casbin/etcd-watcher) | KV store | Casbin | Watcher for [etcd](https://github.com/coreos/etcd)
[Redis Watcher](https://github.com/casbin/redis-watcher) | KV store | Casbin | Watcher for [Redis](http://redis.io/)
[Redis Watcher](https://github.com/billcobbler/casbin-redis-watcher) | KV store | [@billcobbler](https://github.com/billcobbler) | Watcher for [Redis](http://redis.io/)
[TiKV Watcher](https://github.com/casbin/tikv-watcher) | KV store | Casbin | Watcher for [TiKV](https://github.com/tikv/tikv)
[Kafka Watcher](https://github.com/wgarunap/casbin-kafka-watcher) | Messaging system | [@wgarunap](https://github.com/wgarunap) | Watcher for [Apache Kafka](https://kafka.apache.org/)
[NATS Watcher](https://github.com/Soluto/casbin-nats-watcher) | Messaging system | [Soluto](https://github.com/Soluto) | Watcher for [NATS](https://nats.io/)
[ZooKeeper Watcher](https://github.com/grepsr/casbin-zk-watcher) | Messaging system | [Grepsr](https://github.com/grepsr) | Watcher for [Apache ZooKeeper](https://zookeeper.apache.org/)
[NATS, RabbitMQ, GCP Pub/Sub, AWS SNS & SQS, Kafka, InMemory](https://github.com/rusenask/casbin-go-cloud-watcher) | Messaging System | [@rusenask](https://github.com/rusenask/) | Watcher based on [Go Cloud Dev Kit](https://gocloud.dev/) that works with leading cloud providers and self-hosted infrastructure |
[RocketMQ Watcher](https://github.com/fmyxyz/casbin-rocketmq-watcher) | Messaging system | [@fmyxyz](https://github.com/fmyxyz) | Watcher for [Apache RocketMQ](https://rocketmq.apache.org/)

<!--Java-->
Watcher | Type | Author | Description
----|------|----|----
[Etcd Adapter](https://github.com/mapleafgo/jcasbin-extra) | KV store | [@mapleafgo](https://github.com/mapleafgo) | Watcher for [etcd](https://github.com/coreos/etcd)
[Redis Watcher](https://github.com/jcasbin/redis-watcher) | KV store | Casbin | Watcher for [Redis](http://redis.io/)
[Kafka Watcher](https://github.com/jcasbin/kafka-watcher) | Messaging system | Casbin | Watcher for [Apache Kafka](https://kafka.apache.org/)

<!--Node.js-->
Watcher | Type | Author | Description
----|------|----|----
[Etcd Watcher](https://github.com/node-casbin/etcd-watcher) | KV store | Casbin | Watcher for [etcd](https://github.com/coreos/etcd)
[Redis Watcher](https://github.com/node-casbin/redis-watcher) | KV store | Casbin | Watcher for [Redis](http://redis.io/)
[Pub/Sub Watcher](https://github.com/node-casbin/pubsub-watcher) | Messaging system | Casbin | Watcher for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/docs)
[Postgres Watcher](https://github.com/mcollina/casbin-pg-watcher) | Database | [Matteo Collina](https://github.com/mcollina/) | Watcher for [PostgreSQL](https://www.postgresql.org/)

<!--Python-->
Watcher | Type | Author | Description
----|------|----|----
[Redis Watcher](https://github.com/ScienceLogic/flask-casbin-redis-watcher) | KV store | [ScienceLogic](https://github.com/ScienceLogic) | Watcher for [Redis](http://redis.io/)
[PostgreSQL Watcher](https://github.com/pycasbin/postgresql-watcher) | Database | Casbin| Watcher for [PostgreSQL](https://www.postgresql.org/)

<!--.NET-->
Watcher | Type | Author | Description
----|------|----|----
[Redis Watcher](https://github.com/Sbou/Casbin.NET-Redis-Watcher) | KV store | [@Sbou](https://github.com/Sbou) | Watcher for [Redis](http://redis.io/)

<!--Ruby-->
Watcher | Type | Author | Description
----|------|----|----
[Redis Watcher](https://github.com/CasbinRuby/casbin-ruby-redis-watcher) | KV store | [CasbinRuby](https://github.com/CasbinRuby) | Watcher for [Redis](http://redis.io/)
[RabbitMQ Watcher](https://github.com/CasbinRuby/casbin-ruby-rabbitmq-watcher) | Messaging system | [CasbinRuby](https://github.com/CasbinRuby) | Watcher for [RabbitMQ](https://www.rabbitmq.com/)

<!--PHP-->
Watcher | Type | Author | Description
----|------|----|----
[Redis Watcher](https://github.com/Tinywan/casbin) | KV store | [Tinywan](https://github.com/Tinywan) | Watcher for [Redis](http://redis.io/)

<!--END_DOCUSAURUS_CODE_TABS-->

## WatcherEx

In order to support incremental synchronization between multiple instances, we provide the `WatcherEx` interface. We hope it can notify other instances when the policy changes, but there is currently no implementation of `WatcherEx`. We recommend that you use dispatcher to achieve this. 

Compared with `Watcher` interface, with implementing `WatcherEx` what kind of update action can be distinguished, etc `AddPolicy`, `RemovePolicy`, etc. 

WatcherEx Apis:
| api | description |
| ---- | ---- |
| SetUpdateCallback(func(string)) error | SetUpdateCallback sets the callback function that the watcher will call, when the policy in DB has been changed by other instances. A classic callback is Enforcer.LoadPolicy(). |
| Update() error | Update calls the update callback of other instances to synchronize their policy. It is usually called after changing the policy in DB, like Enforcer.SavePolicy(), Enforcer.AddPolicy(), Enforcer.RemovePolicy(), etc. |
| Close() | Close stops and releases the watcher, the callback function will not be called any more. |
| UpdateForAddPolicy(params ...string) error | UpdateForAddPolicy calls the update callback of other instances to synchronize their policy. It is called after Enforcer.AddPolicy() |
| UpdateForRemovePolicy(params ...string) error | UPdateForRemovePolicy calls the update callback of other instances to synchronize their policy. It is called after Enforcer.RemovePolicy() |
| UpdateForRemoveFilteredPolicy(fieldIndex int, fieldValues ...string) error | UpdateForRemoveFilteredPolicy calls the update callback of other instances to synchronize their policy. It is called after Enforcer.RemoveFilteredNamedGroupingPolicy() |
| UpdateForSavePolicy(model model.Model) error | UpdateForSavePolicy calls the update callback of other instances to synchronize their policy. It is called after Enforcer.RemoveFilteredNamedGroupingPolicy() |
