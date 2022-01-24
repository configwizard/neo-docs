---
title: "Helpers"
date: 2022-01-18T17:42:08Z
---

There are some functions that don't quite fit into another topic. They will be referenced as `helper.X` in other tutorials

## Convert String to Uin160

Convert a string format Neo (3) address into a `uint160`. This is the format required for making transfers

{{< tabs >}}
{{% tab name="Go" %}}
```go
// StringToUint160 attempts to decode the given NEO address string
// into an Uint160.
const NEO3Prefix byte = 0x35
func StringToUint160(s string) (u util.Uint160, err error) {
    b, err := base58.CheckDecode(s)
    if err != nil {
        return u, err
    }
    if b[0] != NEO3Prefix {
        return u, errors.New("wrong address prefix")
    }
    return util.Uint160DecodeBytesBE(b[1:21])
}
```
{{% /tab %}}
{{% tab name="Python" %}}
```python
print("please help by opening an MR and filling in these code snippets!")
```
{{% /tab %}}
{{% tab name="C#" %}}
```c#
Console.WriteLine("please help by opening an MR and filling in these code snippets!");
```
{{% /tab %}}
{{< /tabs >}}

## Get credentials from path

This returns the private key, which anything can be derived from, with regards to a [wallet](/neo-docs/examples/wallets)
{{< tabs >}}
{{% tab name="Go" %}}
```go
// GetCredentialsFromPath retrieves the private key from a wallet file 
func GetCredentialsFromPath(path, address, password string) (*ecdsa.PrivateKey, error) {
w, err := wallet.NewWalletFromFile(path)
if err != nil {
    return nil, fmt.Errorf("can't read the wallet: %walletPath", err)
}

var (
    err  error
)
addr := w.GetChangeAddress() //default address

acc := w.GetAccount(addr)
if acc == nil {
    return nil, fmt.Errorf("invalid wallet address %s: %w", addrStr, err)
}

if err := acc.Decrypt(password, keys.NEP2ScryptParams()); err != nil {
    return nil, errors.New("[decrypt] invalid password - " + err.Error())
}

return &acc.PrivateKey().PrivateKey, nil

}
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

