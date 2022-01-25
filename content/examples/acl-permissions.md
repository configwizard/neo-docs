---
title: "ACL Permissions"
date: 2022-01-18T21:13:48Z
---

Permissions can be applied to both containers and to individual tokens that are issued, such as Bearer tokens. This document will outline how they work and how to use them

### Basic ACL Container Permissions

Containers can have permissions attached to them allowing for many different privileges.

You can read further about this in the [neofs-spec](https://nspcc.ru/upload/neofs-spec-latest.pdf#13)
When creating a container, you are going to need to create a permission

{{< tabs >}}
{{% tab name="Go" %}}
```go
permissions := acl.EACLReadOnlyBasicRule
```
{{% /tab %}}
{{% tab name="Python" %}}
```python
print("please help by opening a Pull Request and filling in these code snippets!")
```
{{% /tab %}}
{{% tab name="C#" %}}
```c#
Console.WriteLine("please help by opening a Pull Request and filling in these code snippets!");
```
{{% /tab %}}
{{< /tabs >}}

{{% notice note %}}
You can read more about ACL permissions within the NeoFS documentation however a set of available to use permissions are available. Most languages with an SDK will have constants set to these values
{{% /notice %}}


| Name      | Value | Description |
| ----------- | ----------- |----------- |
| PublicBasicRule      | 0x1FBFBFFF       | PublicBasicRule is a basic ACL value for final public-read-write container for which extended ACL CANNOT be set.       |
| PrivateBasicRule      | 0x1C8C8CCC       | PrivateBasicRule is a basic ACL value for final private container for which extended ACL CANNOT be set.|
| ReadOnlyBasicRule | 0x1FBF8CFF | ReadOnlyBasicRule is a basic ACL value for final public-read container for which extended ACL CANNOT be set. |
| PublicAppendRule | 0x1FBF9FFF |  PublicAppendRule is a basic ACL value for final public-append container for which extended ACL CANNOT be set. |
| EACLPublicBasicRule | 0x0FBFBFFF |  EACLPublicBasicRule is a basic ACL value for non-final public-read-write container for which extended ACL CAN be set. |
| EACLPrivateBasicRule | 0x0C8C8CCC |  EACLPrivateBasicRule is a basic ACL value for non-final private container for which extended ACL CAN be set. |
| EACLReadOnlyBasicRule | 0x0FBF8CFF |  EACLReadOnlyBasicRule is a basic ACL value for non-final public-read container for which extended ACL CAN be set. |
| EACLPublicAppendRule | 0x0FBF9FFF |  EACLPublicAppendRule is a basic ACL value for non-final public-append container for which extended ACL CAN be set. |
