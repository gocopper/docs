# Middleware

A middleware wraps an HTTP request to add additional logic before or after the request has been handled. A middleware can be any struct that implements the `chttp.Middleware` interface.

```go
type Middleware interface {
    Handle(next http.Handler) http.Handler
}
```

If you have a simple function, you can use the `chttp.HandleMiddleware` function to create a middleware out of it.

#### Request Logger Middleware

This is an example middleware that logs each incoming HTTP request.

```go
func NewRequestLoggerMiddleware(logger clogger.Logger) *RequestLoggerMiddleware {
    return &RequestLoggerMiddleware{logger: logger}
}

type RequestLoggerMiddleware struct {
    logger clogger.Logger
}

func (mw *RequestLoggerMiddleware) Handle(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
	mw.logger.WithTags(map[string]any{
		"url": r.URL.Path,
		"method": r.Method
	}).Info("Handling HTTP Request..")
	
	next.ServeHTTP(w, r)
    }
}
```

#### Route Middlewares

The `chttp.Route` struct accepts a list of middlewares that will be executed, in order, for each request that matches the route.

```go
func (ro *Router) Routes() []chttp.Route {
    return []chttp.Route{
        {
            Path:        "/rockets/{id}/launch",
            Methods:     []string{http.MethodPost},
            Middlewares: []chttp.Middleware{ro.requestLoggerMW},
            Handler:     ro.HandleRocketLaunch,
        },
    }
}
```

#### Global Middlewares

In `pkg/app/handler.go`, you can pass a list of middlewares to the `chttp.NewHandler` function. These middlewares will be executed on all requests handled by this handler.

{% code title="pkg/app/handler.go" %}
```go
func NewHTTPHandler(p NewHTTPHandlerParams) http.Handler {
    return chttp.NewHandler(chttp.NewHandlerParams{
        GlobalMiddlewares: []chttp.Middleware{
            p.RequestLoggerMW,
        },
        ...
    })
}
```
{% endcode %}
