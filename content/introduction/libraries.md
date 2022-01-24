---
title: "Libraries"
date: 2022-01-18T18:14:25Z
---

# Code Libraries

The Go SDK is the basis of all the functionality that is available to you as a developer with regards to NeoFS. Due to the underlying protocol of NeoFS being gRPC, this can be ported to other languages (coming soon here) however for the time being, to get started it is best to use the [Go SDK](github.com/nspcc-dev/neofs-sdk-go).

There are however certain 'functionalities' that you will require from the Neo-Go library (such as wallet management and cryptography packages), which you can [read more about here](github.com/nspcc-dev/neo-go).

## SDK

The SDK is responsible for providing functionality to interact with NeoFS itself via gRPC requests. You will need the SDK for such things as:

* creating, retrieving, deleting containers
* creating, updating, retrieving, deleting objects
* permissions

## Neo-Go library

General interaction with NeoFS doesn't require Neo-Go however invariably you will need to manage wallets or use cryptographic functions that are required while setting permissions throughout the NeoFS system.