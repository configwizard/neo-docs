---
title: "Wallets"
date: 2022-01-18T18:57:09Z
---

# Wallets

Almost everything you may want to do with NeoFS will require access to a wallet. Here are a few handy ways to get a wallet

#### Retrieve from a NEP-6 file (json format)

```go
w, err := wallet.NewWalletFromFile(path)
if err != nil {
  return fmt.Errorf("can't read the wallet: %w", err)
}
```

## Create a new wallet

This wallet has no password but is the simplest form of wallet that you can generate

```go
acc, err := wallet.NewAccount() //generates a new private key
if err != nil {
    return &wallet.Wallet{}, err
}
w, err := wallet.NewWallet(path)
if err != nil {
	return fmt.Errorf("can't create the wallet: %w", err)
}
w.AddAccount(acc)
```

#### A slighty more secure wallet... with a password 

```go
w, err := wallet.NewWallet(path)
w.CreateAccount(name, password)
if err != nil {
    return fmt.Errorf("can't create the wallet: %w", err)
}
```

### Retrieving the private key from a wallet

Sometimes for specific actions you will need the private key of the wallet. If you have the password you can extract the private key

Notice this `decrypts` the wallet. At this point the wallet is considered "unlocked". Sometimes you may find you are performing an action on a walelt and the error is it is locked.
The result of decrypting is unlocking the wallet.

```go
addr := w.GetChangeAddress() //get the default wallet
acc := w.GetAccount(addr)
if acc == nil {
  return nil, fmt.Errorf("invalid wallet address %s: %w", addrStr, err)
}

if err := acc.Decrypt(password, keys.NEP2ScryptParams()); err != nil {
    return nil, errors.New("[decrypt] invalid password - " + err.Error())
}
privateKey := &acc.PrivateKey().PrivateKey
```


## Retrieving wallet balances

Often you will want to know the balances on a wallet. There is a potential minor confusion at this stage
