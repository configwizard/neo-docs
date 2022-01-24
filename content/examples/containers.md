---
title: "Containers"
date: 2022-01-18T17:42:08Z
---

Containers manage the permissions/access of a group of objects that are being stored. Before being able to store an object, you need to create a container.

## Creating a container

Before being able to create a container, you will need to 

1. create a policy (`placementPolicy`)
2. have access to a wallet (`key`)
3. Decide on a set of permissions, (`permissions`)
4. Have created a client (`cli`)

### Owner ID

You will need to get the owner ID from the wallet private key. The owner ID is not the same as the wallet ID or public key. A straight forward way to do this is

```go
//see key retrieval generation for how to get a key
w, err := owner.NEO3WalletFromPublicKey(&key.PublicKey)
if err != nil {
    return fmt.Errorf("invalid private key")
}
ownerID := owner.NewIDFromNeo3Wallet(w)
```

Now we can get on with creating a container

```go
containerPolicy, err := policy.Parse(placementPolicy)
if err != nil {
  return fmt.Errorf("can't parse placement policy: %w", err)
}

cnr := container.New(
  // container policy defines the way objects will be
  // placed among storage nodes from the network map
  container.WithPolicy(containerPolicy),
  // container owner can set BasicACL and remove container
  container.WithOwnerID(ownerID),
  // read more about basic ACL in specification:
  // https://github.com/nspcc-dev/neofs-spec/blob/master/01-arch/07-acl.md
  container.WithCustomBasicACL(permissions),
  // Attributes are key:value string pairs they are always optional
  container.WithAttribute(
    container.AttributeTimestamp,
    strconv.FormatInt(time.Now().Unix(), 10),
  ),

```

Finally we can `put` the container on NeoFS. We will receive a response that contains the container's ID.

```go
response, err := cli.PutContainer(ctx, cnr)
if err != nil {
	return fmt.Errorf("can't put container: %w", err) 
}
fmt.Println(response.ID())
```
## Listing Containers

You can list all the containers owned by a wallet. This will return an array of container IDs

```go
response, err := cli.ListContainers(ctx, wallet.OwnerIDFromPrivateKey(key))
if err != nil {
  return fmt.Errorf("can't list container: %w", err)
}

walletList := response.IDList()
	
```

## Retrieve a Container

You can retrieve a container once you have the ID
```go
response, err := cli.GetContainer(ctx, containerID)
if err != nil {
  return fmt.Errorf("can't get container %s: %w", containerID, err)
}

contianeer := response.Container()
```

From a container, you can find out storage policies, owners and any other meta information about the container itself

## Deleting Containers

Once you have created a container, you will receive the ID of the container as part of the response (see above). Using this ID you can now delete the container with ease 
```go
response, err := cli.DeleteContainer(ctx, containerID)
if err != nil {
  return nil, fmt.Errorf("can't get container %s: %w", containerID, err)
}
fmt.Printf("deletion response %+v\r\n", response)
```

## Questions about containers

* if you delete a container what happens to all the objects within a container
* using multisig wallets, could two or more people share ownership of a container
* status - what information do I receive from a status?