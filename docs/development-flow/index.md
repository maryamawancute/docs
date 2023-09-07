---
sidebar_position: 7
title: Make a squid
description: Development flow for building squids
---

# Make a squid

There are two main ways of making squids: auto-generating them and developing them from scratch.

Auto-generated squids can retrieve and decode events and transactions of smart contracts without transforming them. If that is what you need, visit [this section](/dead).

Developing a squid from scratch typically involves the following steps:

1. Determining which on-chain data is required and how it should be transformed.
2. Retrieving an appropriate template.
3. *Running typegen for constants and decoders.*
4. Configuring the processor to retrieve the required data.
5. Configuring an appropriate data sink.
6. Writing the batch handler: the function that decodes and transforms on-chain data, then saves the result.

Steps 3-5 prepare the tools for writing the batch handler:
*Show how steps 3-5 prepare us for 6 and make it very easy.*

After developing a squid you can [deploy it](/dead) and [query it](/dead) in your frontend application.
