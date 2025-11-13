# App Lifecycle

Copper provides an "App Container" that runs your application code. This container manages the app lifecycle i.e. running code during startup (ex. opening database connections) and shutdown (ex. closing database connections). It also listens to interrupt signals so it can allow for a graceful shutdown of the app.

The `copper.App` struct, returned by `copper.New()` contains a pre-configured [logger](logging.md), [configuration loader](configuration.md), and the app lifecycle manager - that you can use to add custom logic on app startup & shutdown.

### Cleanup Code

To run cleanup code when your app is shutting down, register the function using the app lifecycle -

```go
func NewServer(lc *clifecycle.Lifecycle) *Server {
  myServer := &Server{..}
  
  // Stop the server when the app is shutting down..
  lc.OnStop(func(ctx context.Context) error {
    return myServer.Stop(ctx)
  })
  
  return myServer
}
```
