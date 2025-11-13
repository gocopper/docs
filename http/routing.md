# Routing

#### Routers

Routers are simple Go structs that implement the `chttp.Router` interface. Routers can be used to define and group together HTTP routes.

```go
type Router interface {
    Routes() []Route
}
```

{% hint style="info" %}
By default, Copper provides `pkg/app/router.go`. Create more routers as your project grows to organize and group routes.
{% endhint %}

Add a router to a package using the Copper CLI -

```
copper scaffold:router <package>
```

#### Create Routes

In a package with an existing Router, you can add a route to it using the Copper CLI -

```bash
copper scaffold:route \
    -handler=HandleRocketLaunch \
    -path=/rockets/{id}/launch \
    -method=Post \
    <package>
```

#### Sample Router

{% code title="pkg/rockets/router.go" %}
```go
package rockets

import (..)

type NewRouterParams struct {
	RW     *chttp.JSONReaderWriter
	Logger clogger.Logger
}

func NewRouter(p NewRouterParams) *Router {
	return &Router{
		rw:     p.RW,
		logger: p.Logger,
	}
}

// Router struct implements the `chttp.Router` interface. It holds all the
// dependencies needed for its handlers.
type Router struct {
	rw     *chttp.JSONReaderWriter
	logger clogger.Logger
}

// Routes returns a list of chttp.Route managed by this Router.
func (ro *Router) Routes() []chttp.Route {
	return []chttp.Route{
		{
			Path:    "/rockets/{id}/launch",
			Methods: []string{http.MethodPost},
			Handler: ro.HandleRocketLaunch,
		},
	}
}

// HandleRocketLaunch is invoked when a POST request is made to the 
// "/rockets/{id}/launch" endpoint
func (ro *Router) HandleRocketLaunch(w http.ResponseWriter, r *http.Request) {
	// Handle request here..
}
```
{% endcode %}
