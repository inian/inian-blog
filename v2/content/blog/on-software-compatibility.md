---
title: On Software Compatibility
date: 2023-10-01
external: false
description: I discuss the various levels of compatibility - from building into existing ecosystems, implementing a fork or a compatibility layer to doing everything from scratch.
---

The product decisions you take early on in a company dictate how easy it is to adopt it by new users. If the product you are building on top of existing ecosystem, it becomes far easier to adopt than bootstrapping an entirely new one.

I discuss the various levels of compatibility - from building into existing ecosystems, implementing a fork or a compatibility layer to doing everything from scratch.

And you will find most of my examples are from the database space, since I work at [a database company](https://supabase.com), but should be relevant for other developer tools as well.

## Building into existing ecosystems

The recent [ElectricSQL launch](https://electric-sql.com/blog/2023/09/20/introducing-electricsql-v0.6) got me thinking about the importance of maintaining compatibility with existing tools and workflows. They have managed to remove a dependency on a new [database they built for CRDTs](https://github.com/electric-sql/vaxine) to just using Postgres. This massively simplifies adopting ElectricSQL.

There are tons of great places to [host Postgres](https://database.new/), it has an established [active community](https://www.postgresql.org/community/), lots of companies [operate it at scale](https://www.notion.so/blog/sharding-postgres-at-notion) and [documented it’s gotchas](https://ottertune.com/blog/the-part-of-postgresql-we-hate-the-most) etc. There are existing [podcasts](https://postgres.fm/) and [newsletters](https://postgresweekly.com/) covering Postgres that will be happy to share about ElectricSQL since they are adding to the Postgres ecosystem. They can build integrations with all the companies offering Postgres hosting. There are [client libraries](https://wiki.postgresql.org/wiki/Client_Libraries) built for almost every single language. Getting these working for a new database would have been infinitely harder.

We take this route with Supabase since we innovate on the workflows on top of Postgres. It is not a fork, it is not just wire compatible - [it is just Postgres](https://www.google.com/search?q=%22just+postgres%22+supabase). This makes conversations with users much easier since they know they can always fall back to using it as just another Postgres database. Migrating to and away from Supabase is simple.

And when we want to extend Postgres, it is easier to build a Postgres extension and remain within the Postgres ecosystem. Timescale has also gone down this route, instead of building a brand new database for timeseries data presumably for similar reasons.

But it may not always be possible to build on top of existing tools, if thats exactly the layer you want to disrupt.

## Building a Fork

The advantage with building a fork that it can be made compatible with the existing ecosystem and all the advantages mentioned in the previous section applies. The disadvantage with forks is that maintaining compatibility with upstream will always be a challenge, especially if upstream is also releasing updates regularly. For example, Postgres releases a new major version every year and if you are building a Postgres fork, you need to ensure that you maintain compatibility with each release. This may not be easy to do.

The other issue with forks is that even though the high level behaviour might be the same, things might look different when it comes to performance characteristics, behaviour under load, etc.

[OrioleDB](https://github.com/orioledb/orioledb) is a fork of Postgres with a different storage engine and is a magnitude of times faster than vanilla Postgres. The goal of OrioleDB is to become a Postgres extension running on native Postgres without being a Postgres fork which would take it to the [earlier category](#Building-into-existing-ecosystems).

[Yugabyte](https://www.yugabyte.com/) is another example of a Postgres fork which has replaced the storage engine to be distributed.

[Libsql](https://github.com/tursodatabase/libsql) is an open contribution fork to SQLite. They [decided to fork](https://github.com/tursodatabase/libsql#why-a-fork) SQLite since it doesn’t accept any external contributions and they couldn’t get their improvements in. It still maintains compatibility to SQLite to it is easier to migrate workloads to use libsql.

## Building a compatibility layer

**Compatibility with an existing ecosystem**

There are new Javascript runtimes taking on Node.js, like Deno and Bun. Even though both of them are built on completely different technologies, they offer a Node and npm compatibility layer. Bun launched with a npm compatibility layer from day 1 and Deno added npm compatibility after strong feedback from the community.

### Compatibility with a protocol standard

**Postgres - the database standard**

On the database side of things, being Postgres compatible is pretty much the standard for any new database. There are a huge number of databases which aren’t really Postgres under the hood but are Postgres wire compatible like [Google AlloyDB](https://cloud.google.com/alloydb) and [AWS Aurora Postgres](https://aws.amazon.com/rds/aurora/features/). [CockroachDB](https://www.cockroachlabs.com/) is compatible with a subset of Postgres feature set.

**S3 - the storage standard**

For storage, S3 compatibility has become the standard. Pretty much all storage providers like Backblaze, R2 and even GCP Cloud Storage implement the S3 protocol.

[Supabase Storage](https://supabase.com/storage) uses the S3 protocol internally to integrate with multiple providers to store objects, as long as a storage provider is S3 compatible, you can use Supabase Storage to manage objects with that provider. But we had our own API for users to upload objects to Supabase.

We realised we need to support the S3 protocol externally as well, so that users can interact with Supabase Storage via the same tools they are familiar with.

Building this compatibility layer might seem like a step backwards, but is important to bring over existing users. Arguably [Supabase Storage API](https://supabase.com/docs/guides/storage/uploads#standard-upload) _is_ simpler than the S3 API, but it is not the standard.

**Docker - the packaging standard**

Pretty much any hosting provider supports Dockerfiles as the standard way of deploying arbitrary applications. What [Fly.io](https://fly.io) did is interesting here - they support Dockerfiles as the packaging format, even though they don’t run Docker under the hood! They parse the Dockerfile and extract it to a filesystem which is attached to a Firecracker microVM. By building this translation layer, Fly reduced the effort required to try them out or migrate workloads to them.

**Git - the code workflow standard**

[Graphite](https://graphite.dev/) builds on top of the Github workflow. You can use Graphite, even if the rest of your team is on Github. If they had tried to build something that required everyone in the team to buy in to their workflow, converting users would be much harder.

## Starting from scratch

Sometimes your product or workflow is so different, that there is no reasonable way of integrating with the status quo of doing things. In this case, it might make sense to start everything from scratch. Some projects which have done this successfully here are [Clickhouse](https://clickhouse.com/), [Node.js](https://nodejs.org/en) and [Postgrest](https://postgrest.org/en/stable/).

This may be an option for open source projects without a specific timeline, but may not be an option if you are running a (venture backed) company. For example, Clickhouse was founded in 2009 in Yandex and it took years to bootstrap a new community around it and finally a commercial company was built around it in 2021.

## Why does this matter

If your product is compatible with the rest of the landscape, it becomes a growth driver. It is easier for users to try out your product. Developers hate vendor lock in and knowing that they can switch away to a different provider if something doesn’t work out alleviates a major concern when evaluating a product.

At Supabase, some of the decisions, this kind of thinking has influenced

- Adding npm support to [Edge functions](https://supabase.com/edge-functions)
- Implementing S3 compatiblity to [Storage](https://supabase.com/storage)
- Experimental support for Open Telemetry in [Logflare](https://logflare.app/)
- Prototying a workflow engine based on the [AWS States Langauge](https://states-language.net/).

Are there exceptions where a product has managed to grow fast even without integrating with an existing ecosystem? How did they do it?
