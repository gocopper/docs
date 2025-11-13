# Error Handling

Copper provides the `cerrors` package that helps add structure and context to your errors.

```go
err := s.rockets.ThrustEngine(ctx, Engine3)
if err != nil {
    return cerrors.New(err, "failed to thrust engine", map[string]any{
        "engine": Engine3,
    })
}
```

{% hint style="success" %}
Always wrap your errors with a helpful message and tags before returning them. This will produce much better logs.
{% endhint %}
