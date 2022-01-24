---
title: "Tokens"
date: 2022-01-24T11:23:17Z
---


# Tokens

Tokens are requried to act on any objects within a container. Sessions last for limited time and are restricted by permissions. Session tokens use the private key to sign them while bearer tokens are issued to wallets by a container owner.

## Session Tokens

Session tokens are tokens that are generated from your private key and are required to access or interact with content inside a container (depending on permissions this can include READ capability). These tokens should only be used by container owners and not distributed to third parties

### Creating a token

You will need

1. to decide how long the token should last (shorter is better)
2. a context (generally you can use `context.Background()`) but this depends on your usecase
3. A client (see [client](/client))
4. Your private key, available from your wallet/account

```go
sessionResponse, err := cli.CreateSession(ctx, expiration)
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
