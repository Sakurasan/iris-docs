---
date: 2016-03-09T00:11:02+01:00
title: Basic Authentication
weight: 270
---

This is a [middleware](https://github.com/iris-contrib/middleware/tree/master/basicuath).

HTTP Basic authentication (BA) implementation is the simplest technique for enforcing access controls to web resources because it doesn't require cookies, session identifiers, or login pages; rather, HTTP Basic authentication uses standard fields in the HTTP header, obviating the need for handshakes. Read [more](https://en.wikipedia.org/wiki/Basic_access_authentication).



### Simple example

```go

package main

import (
	"github.com/iris-contrib/middleware/basicauth"
	"github.com/kataras/iris"
)

func main() {
	authentication := basicauth.Default(map[string]string{"myusername": "mypassword", "mySecondusername": "mySecondpassword"})

	// to global iris.Use(authentication)
	// to party: iris.Party("/secret", authentication) { ... }

	// to routes
	iris.Get("/secret", authentication, func(ctx *iris.Context) {
		username := ctx.GetString("user") // this can be changed, you will see at the middleware_basic_auth_2 folder
		ctx.Write("Hello authenticated user: %s ", username)
	})

	iris.Get("/secret/profile", authentication, func(ctx *iris.Context) {
		username := ctx.GetString("user")
		ctx.Write("Hello authenticated user: %s from localhost:8080/secret/profile ", username)
	})

	iris.Get("/othersecret", authentication, func(ctx *iris.Context) {
		username := ctx.GetString("user")
		ctx.Write("Hello authenticated user: %s from localhost:8080/othersecret ", username)
	})

	iris.Listen(":8080")
}


```

### Configurable example

```go
package main

import (
	"time"

	"github.com/iris-contrib/middleware/basicauth"
	"github.com/kataras/iris"
)

func main() {
	authConfig := basicauth.Config{
		Users:      map[string]string{"myusername": "mypassword", "mySecondusername": "mySecondpassword"},
		Realm:      "Authorization Required", // if you don't set it it's "Authorization Required"
		ContextKey: "mycustomkey",            // if you don't set it it's "user"
		Expires:    time.Duration(30) * time.Minute,
	}

	authentication := basicauth.New(authConfig)

	// to global iris.Use(authentication)
	// to routes
	/*
		iris.Get("/mysecret", authentication, func(ctx *iris.Context) {
			username := ctx.GetString("mycustomkey") //  the Contextkey from the authConfig
			ctx.Write("Hello authenticated user: %s ", username)
		})
	*/

	// to party

	needAuth := iris.Party("/secret", authentication)
	{
		needAuth.Get("/", func(ctx *iris.Context) {
			username := ctx.GetString("mycustomkey") //  the Contextkey from the authConfig
			ctx.Write("Hello authenticated user: %s from localhost:8080/secret ", username)
		})

		needAuth.Get("/profile", func(ctx *iris.Context) {
			username := ctx.GetString("mycustomkey") //  the Contextkey from the authConfig
			ctx.Write("Hello authenticated user: %s from localhost:8080/secret/profile ", username)
		})

		needAuth.Get("/settings", func(ctx *iris.Context) {
			username := ctx.GetString("mycustomkey") //  the Contextkey from the authConfig
			ctx.Write("Hello authenticated user: %s from localhost:8080/secret/settings ", username)
		})
	}

	iris.Listen(":8080")
}

```