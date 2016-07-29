---
date: 2016-03-09T00:11:02+01:00
title: Control panel
weight: 390
---

[This is a plugin](https://github.com/iris-contrib/plugin/tree/master/iriscontrol) which is working but not finished yet.

Which gives  access to your iris server's information via a web interface.
> You need internet connection the first time you will run this plugin, because the assets don't exists to this repository but [here](https://github.com/iris-contrib/iris-control-assets). The plugin will install these for you at the first run.

-----

How to use
```go
iriscontrol.New(port int, authenticatedUsers map[string]string) iris.IPlugin
```

Example

```go
package main

import (
    "github.com/kataras/iris"
    "github.com/iris-contrib/plugin/iriscontrol"
)

func main() {

    iris.Plugins.Add(iriscontrol.New(9090, map[string]string{
        "irisusername1": "irispassword1",
        "irisusername2": "irispassowrd2",
    }))
    //or
    // ....
    // iriscontrol.New(iriscontrol.Config{...})

    iris.Get("/", func(ctx *iris.Context) {
    })

    iris.Post("/something", func(ctx *iris.Context) {
    })

    iris.Listen(":8080")
}

```
