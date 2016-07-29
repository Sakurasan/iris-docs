---
date: 2016-03-09T00:11:02+01:00
title: Configuration
weight: 100
---

Configuration is a relative package `github.com/kataras/iris/config`

> No need to download it separately, it's downloaded automatically when you install Iris.

### Why?

I took this decision after a lot of thought and I ensure you that this is the best and easiest
architecture:

* change the configs without needing to re-write all of their fields.

  ```go
    irisConfig := config.Iris{ DisablePathCorrection: true }
    api := iris.New(irisConfig)
  ```

* **easy to remember**: `iris` type takes `config.Iris`, sessions takes `config.Sessions`, `iris.Config.Render` is of type `config.Render`, `iris.Config.Render.Template` is the type `config.Template`, `Logger` takes `config.Logger` and so on...

* **easy to search & find out what features exists and what you can change**: just navigate to the config folder and open the type you want to learn about, for example `/websocket.go /iris.Websocket 's`configuration is inside `/config/websocket.go`

* Enables you to do this **without setting up a config yourself**: `iris.Config.Gzip = true` or `iris.Config.Charset = "UTF-8"`.

* **\(Advanced usage\) merge configs**:


```go
//...
import "github.com/kataras/iris/config"
//...
websocketFromDefault:= config.DefaultWebsocket()
//....
websocketManual:= config.Websocket{  Endpoint: "/ws"}

websocketConfig := websocketFromDefault.MergeSingle(websocketManual)

```

# [Click here to view all station's configs](https://github.com/kataras/iris/tree/master/config)

