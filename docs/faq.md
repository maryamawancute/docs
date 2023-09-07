---
sidebar_position: 130
---

# FAQ

### What is the Premium plan?

The access to Archives is free. Aquarium offers a free and a premium plans. The Premium plan is currently available by invite via the [form](https://docs.google.com/forms/d/e/1FAIpQLSchqvWxRhlw7yfBlfiudizLJI9hEfeCEuaSlk3wOcwB1HQf6g/viewform?usp=sf_link). When released to the public, Premium will be based on the fixed subscription fee + pay-as-you-go model. Details will be announced in the official Subsquid channels.

### Do you have a roadmap ?

Yes, see the issue in [the official repo](https://github.com/subsquid/squid-sdk/issues/70)

### How does Subsquid handle unfinalized blocks?

Archives index only finalized blocks. Handling unfinalized blocks and potential block reorganisations is supported by the Squid SDK since the ArrowSquid release, see [Indexing unfinalized blocks](/basics/unfinalized-blocks).

### What is the latency for the data served by the squid? 

Since the ArrowSquid release, the Squid SDK has the option to ingest unfinalized blocks directly from an RPC endpoint, making the indexing real-time. Archive-only squids without an RPC datasource typically have a latency of a few minutes to a few hours.

### How do I enable GraphQL subscriptions for local runs?

Add `--subscription` flag to the `serve` command defined in `commands.json`. See [Subscriptions](/graphql-api/subscriptions) for details.

### How do squids keep track of their sync progress?

Depends on the data sink used. Squid processors that use [`TypeormDatabase`](/store/postgres) keep their state in a [schema](https://www.postgresql.org/docs/current/sql-createschema.html), not in a table. By default the schema is called `squid_processor` (name must be overridden in [multiprocessor squids](/basics/multichain)). You can view it with
```sql
select * from squid_processor.status;
```
and manually drop it with
```sql
drop schema squid_processor cascade;
```
to reset the processor status.

Squids that store their data in [file-based datasets](/store/file-store) store their status in `status.txt` by default. This can be overridden by defining custom [database hooks](/store/file-store/overview/#filesystem-syncs-and-dataset-partitioning).

### How to add a `preBlockHook` and `postBlockHook` executing before or after each block?

A batch processor receives a list of items grouped into blocks. In order to add custom logic to be executed before/after a block, simply use the iterator over `ctx.blocks`:

```ts
processor.run(new TypeormDatabase(), async (ctx) => {
  for (let c of ctx.blocks) {
    // pre-block hook logic here
    for (let log of c.logs) {
    }
    for (let txn of c.transactions) {
    }
    // ...processing of any other data items...
    // post-block logic here
  }
})
```

### How do I know which events and extrinsics I need on Substrate?

This part depends on the runtime business logic of the chain. The primary and the most reliable source of information is thus the Rust sources for the pallets used by the chain.

For a quick lookup of the documentation and the data format, it is often useful to check `Runtime` section of Subscan (e.g. [Statemine](https://statemine.subscan.io/runtime)). One can see the deployed pallets and drill down to events and extrinsics from there. One can also choose the spec version on the drop down.


### Where do I get a type bundle for my chain?

Most chains publish their type bundles as an npm package (for example: [Edgeware](https://www.npmjs.com/package/@edgeware/node-types)). One of the best places to check for the latest version is the [polkadot-js/app](https://github.com/polkadot-js/apps/tree/master/packages/apps-config/src/api/spec) and [polkadot-js/api](https://github.com/polkadot-js/api/tree/master/packages/types-known/src/spec) repositories. It's worth noting, however, that a types bundle is only needed for pre-Metadata v14 blocks, so for recently deployed chains it may be not needed.

:::info
**Note:** the type bundle format for typegen is slightly different from `OverrideBundleDefinition` of `polkadot.js`. The structure is as follows, all the fields are optional.
:::

```javascript
{
  types: {}, // top-level type definitions, as `.types` option of `ApiPromise`
  typesAlias: {}, // top-level type alieases, as `.typesAlias` option of `ApiPromise`
  versions: [ // spec version specific overrides, same as `OverrideBundleDefinition.types` of `polkadot.js`
    {
       minmax: [0, 1010] // spec range
       types: {}, // type overrides for the spec range
       typesAlias: {}, // type alias overrides for the spec range
    }
  ]
}
```
