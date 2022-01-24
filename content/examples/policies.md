---
title: "Policies"
date: 2022-01-18T18:58:22Z
---

Before we can create a container, we need to define the policy. A policy defines which storage nodes on NeoFS you are happy to store your data on.


-- todo, how do policies get defined?


Basic example

{{< tabs >}}
{{% tab name="Go" %}}
```
const placementPolicy = `REP 2 IN X
CBF 2
SELECT 2 FROM * AS X
`
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
