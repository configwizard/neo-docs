---
title: "Helpers"
date: 2022-01-18T17:42:08Z
---

There are some functions that don't quite fit into another topic. They will be referenced as `helper.X` in other tutorials

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

```
{{% /tab %}}
{{% tab name="C#" %}}
```c#

```
{{% /tab %}}
{{< /tabs >}}
