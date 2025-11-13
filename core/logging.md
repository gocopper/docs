# Logging

Copper provides the `clogger` package that allows structured logging in both dev and prod environments. It integrates deeply with `cerrors` to produce insightful logs with detailed error messages.

```go
err := s.starships.PropelRocket(ctx, "my-test-rocket")
if err != nil {
	s.logger.WithTags(map[string]any{
		"rocket": "my-test-rocket",
	}).Error("Failed to propel rocket", err)
}
```

The code snippet above may produce the following log statement:

```
Failed to propel rocket where rocket=my-test-rocket because
> failed to thrust engine where engine_number=3 because
> no more fuel where tank=1
```

Note that the errors are fully unwrapped, each providing a level of detail on what may have gone wrong. Since each error is tagged, we see helpful information like "engine\_number=3" and "tank=1".

{% hint style="info" %}
The logger can be configured to produce a multiline log statement for better readability or a JSON formatted single-line statement so that it can be easily parsed and indexed by log aggregators.
{% endhint %}
