---
title: "Client"
date: 2022-01-18T21:13:48Z
---

## Client

A lot of interactions happen through a `client`. The client wraps around the raw API requests to NeoFS.
Once you have a client requests to NeoFS can be made so you will need to make this early on from the wallet/private key
Creating a client can be done like so

```go
cli, err := client.New(
  // provide private key associated with request owner
  client.WithDefaultPrivateKey(privateKey),
  // find endpoints in https://testcdn.fs.neo.org/doc/integrations/endpoints/
  client.WithURIAddress(TESTNET, nil),
  // check client errors in go compatible way
  client.WithNeoFSErrorParsing(),
)
if err != nil {
	return fmt.Errorf("can't create client: %w", err)
}
```

* Private key can be retrieved from a wallet - its type is `*ecdsa.PrivateKey`
* The network is a string, for now you can use 

```go
	TESTNET string = "grpcs://st01.testnet.fs.neo.org:8082"
	MAINNET = "grpcs://st01.testnet.fs.neo.org:8082"
```



