---
sidebar_position: 500
---

# Limitations

Subsquid is helpful in most tasks involving historical blockchain data, especially on the networks where Archives are available, but of course there are exceptions. Here are some such use cases:

<details><summary>Retrieving native token balances on chains without traces or state diffs, such as MATIC on Polygon.</summary>
It is possible to retrieve all transactions with value from Archives, but since native tokens can be transfered within internal contract calls that info will not suffice for balance reconstruction. The only way to retrieve the balances is with direct RPC calls, so if sync performance is your main criterion and balances retrieval is your main bottleneck, then your squid will perform on par with other RPC-based indexers.
</details>
