# Configuration

Copper provides the `cconfig` package that helps read hierarchical TOML files into simple Go structs. The TOML config files live in the `./config` directory with a few out-of-the-box files such as `base.toml`, `dev.toml`, and `prod.toml`.

You are encouraged to extend and customize the structure of the `./config` directory as needed for your project.

#### Extensible TOML Files

The configurations are defined in simple TOML files that support a special `extends` key:

{% code title="config/dev.toml" %}
```toml
extends = ["base.toml"]

[starship]
destination = "HD1"
use_wormholes = false
```
{% endcode %}

In the example above, the `dev.toml` file defines a top-level `extends` key which is set to `base.toml`. As a result, any project that is run with the `dev.toml` config file will inherit all configuration values from `base.toml`.

{% hint style="warning" %}
Do not use top-level configuration values. Remember to group all values under a section so they can be loaded per package without needing a "global" app config.
{% endhint %}

#### Read Configuration

You can read a config section using `cconfig.Loader#Load`. The values are loaded into a destination Go struct. The config loader accounts for the hierarchy of all TOML files that use `extends`.

{% code title="pkg/starship/config.go" %}
```go
package starship

import (..)

func LoadConfig(configs cconfig.Loader) (Config, error) {
	var config Config

	err := configs.Load("starship", &config)
	if err != nil {
		return Config{}, cerrors.New(err, "failed to load starship config", nil)
	}

	return config, nil
}

type Config struct {
	Destination  string `toml:"destination"`
	UseWormholes bool   `toml:"use_wormholes"`
}
```
{% endcode %}
