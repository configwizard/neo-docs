---
title: "Objects"
date: 2022-01-24T11:23:17Z
---

Objects represent items stored within a container. These are subject to the permissions of the container being the most relaxed possible permissions that can be applied to an object. It is possilbe using Session/Bearer Tokens to restrict permissions further on objects within a container however

Please note actions on objects are restricted by the permissions on the container AND the permissions of the token used to access the functions. 

## Uploading Objects 

Before uploading an object, you will need

1. A session Token
2. A container ID to upload the object to, with the correct permisisions
3. An object to upload (`filepath`)
4. Have created a client (`cli`)

### Attributes

Attributes are key value pairs (string:string) that are attached to the metadata of objects. You can specify anything as an attribute, however there are a couple of reserved ones
```go
var attributes []*object2.Attribute

timeStampAttr := new(object.Attribute)
timeStampAttr.SetKey(object.AttributeTimestamp) // AttributeTimestamp key is like a 'created at' attribute
timeStampAttr.SetValue(strconv.FormatInt(time.Now().Unix(), 10))

fileNameAttr := new(object.Attribute)
fileNameAttr.SetKey(object.AttributeFileName) // AttributeFileName key is the filename to be associated with the object. This will become useful later
fileNameAttr.SetValue(path.Base(filepath))
attributes = append(attributes, []*object2.Attribute{timeStampAttr, fileNameAttr}...)

```

See [tokens](/examples/tokens) for how to create a session token

## Upload 

```go
f, err := os.Open(filepath)
defer f.Close()
if err != nil {
    return "", err
}

reader := bufio.NewReader(f)
var ioReader io.Reader
ioReader = reader

ownerID, err := wallet.OwnerIDFromPrivateKey(key)
if err != nil {
    return "", err
}
cntId := new(cid.ID)
cntId.Parse(containerID)
id, err := object.UploadObject(ctx, cli, cntId, ownerID, attributes, sessionToken, &ioReader)
if err != nil {
    return fmt.Println("error attempting to upload", err)
}
return id.String(), err //id is the object ID that you will want to reference
```

Depending on your container's permissions you should now be able to view the file you uploaded at:

https://http.testnet.fs.neo.org/CONTAINER_ID/OBJECT_ID

however, if you set the 

```go
fileNameAttr := new(object.Attribute)
fileNameAttr.SetKey(object.AttributeFileName) // AttributeFileName key is the filename to be associated with the object. This will become useful later
fileNameAttr.SetValue(path.Base(filepath))
```
attribute, you can also refer to the object by its filename, i.e

https://http.testnet.fs.neo.org/CONTAINER_ID/upload.png

Note `path.Base(filepath)` will give you back just the filename part of a filepath:

```go
filepath := /users/bond/home/upload.png
filename := path.Base(filepath) //filename = upload.png
```

## Listing the content of a container

Once you have uploaded objects to a container, you will want to list them out. Listing is a special case of searching within a container.
To search for specific objects, you add filters to the search. By setting the only filter as a root filter, it will list everything within the container

```go
var searchParams = new (client.SearchObjectParams)
var filters = object.SearchFilters{}
filters.AddRootFilter()
searchParams.WithContainerID(containerID)
searchParams.WithSearchFilters(filters)
res, err := cli.SearchObjects(ctx, searchParams, client.WithSession(sessionToken))
if err != nil {
    return fmt.Errorf(err)
}
fmt.Printf("list objects %+v\n", res.IDList())
```

## Retrieve an Object

Once you have the ID of an object, you can download it.

You will need 
1. an io.Writer to write the data to a file such as a file writer
2. an object address, which is made up of a container ID and an object ID

To generate an object address from the string forms of a containerID and an objectID:

```go
contID := cid.New()
contID.Parse(containerID)
objID := obj.NewID()
objID.Parse(objectID)
objAddress := obj.NewAddress()
objAddress.SetObjectID(objID)
objAddress.SetContainerID(contID)
```

so now you can retrieve the object

```go
var getParams = new(client.GetObjectParams)
getParams.WithAddress(objectAddress)
getParams.WithPayloadWriter(*writer)
o, err := cli.GetObject(ctx, getParams, client.WithSession(sessionToken))
if err != nil {
    return fmt.Errorf(err)
}
// payload is in bytes []bytes 
payload := o.Object().Payload()
}
```

## Retrieving an objects HEAD/metadata

Sometimes you want information about an object, without actually downloading the entire object, for instance the size of an object

From a container, you can find out storage policies, owners and any other meta information about the container itself. This is very similar to retrieving the object

```go
var headParams = new(client.ObjectHeaderParams)
headParams.WithAddress(objectAddress)
headObject, err := cli.HeadObject(ctx, headParams, client.WithSession(sessionToken))
if err != nil {
  return &client.ObjectHeadRes{}, err
}
size := headObject.Object().PayloadSize()
```

## Deleting Objects

You may wish to delete an object for a container

```go
	var deleteParams = new (client.DeleteObjectParams)
deleteParams.WithAddress(objectAddress)
_, err := cli.DeleteObject(ctx, deleteParams, client.WithSession(sessionToken))
if err != nil {
    return fmt.Errorf(err)
}
```

## Questions about objects

* Can you update the attributes of an existing object
* Can you determine the original uploader of an object
