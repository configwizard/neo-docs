---
title: "Tokens"
date: 2022-01-24T11:23:17Z
---

Tokens are requried to act on any [objects](/neo-docs/examples/objects) within a [container](/neo-docs/examples/containers). Sessions last for limited time and are restricted by permissions. [Session tokens](/neo-docs/examples/tokens) use the private key to sign them while [bearer tokens(/examples/tokens) are issued to wallets by a container owner.

## Session Tokens

Session tokens are tokens that are generated from your private key and are required to access or interact with content inside a container (depending on permissions this can include READ capability). These tokens should only be used by container owners and not distributed to third parties

### Creating a token

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
  return &session.Token{}, err
}
st := session.NewToken()
id, err := wallet.OwnerIDFromPrivateKey(key)
if err != nil {
  return &session.Token{}, err
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
