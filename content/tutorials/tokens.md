---
title: "Tokens"
date: 2022-01-24T11:23:17Z
---

Tokens are requried to act on any [objects](/neo-docs/examples/objects) within a [container](/neo-docs/examples/containers). Sessions last for limited time and are restricted by permissions. [Session tokens](/neo-docs/examples/tokens) use the private key to sign them while [bearer tokens(/examples/tokens) are issued to wallets by a container owner.

## Session Tokens

Session tokens are tokens that are generated from your private key and are required to access or interact with content inside a container. These tokens should only be used by container owners and not distributed to third parties

### Creating a session token

You will need

1. to decide how long the token should last (e.g `const DEFAULT_EXPIRATION = 140000`)
2. a context (generally you can use `context.Background()`) but this depends on your usecase
3. A [NeoFS client](/neo-docs/examples/clients)
4. have access to a private key. This is retrieved from a json file using the [helper function](/neo-docs/examples/helpers/#get-credentials-from-path) `helper.GetCredentialsFromPath` (`key`)

{{< tabs >}}
{{% tab name="Go" %}}
```go
sessionResponse, err := cli.CreateSession(ctx, DEFAULT_EXPIRATION)
if err != nil {
    return fmt.Errorf("can't create session: %w", err)
}
st := session.NewToken()
id, err := wallet.OwnerIDFromPrivateKey(key)
if err != nil {
    return fmt.Errorf("can't get owner ID: %w", err)
}
st.SetOwnerID(id)
st.SetID(sessionResponse.ID())
st.SetSessionKey(sessionResponse.SessionKey())
//st is your new session token

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

Session tokens are all that is required for the owner, or the account with the private key that created the container, to access the container. The owner is the most privileged account with regards to a container so a session key should never be shared.

## Bearer Tokens

Bearer tokens allow the account that owns the container, to issue limited time access tokens to other accounts. Bearer tokens require rules to be applied to grant access and to deny access to anyone else. Read more about [EACL Tables](/neo-docs/examples/acl-permissions) to get a better understanding of what they are

Once an account has a bearer token it can pass the token along with any request to the container as it does a Session Token.
