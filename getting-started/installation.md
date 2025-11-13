# Installation

#### Install Copper CLI

```bash
go install github.com/gocopper/cli/cmd/copper@latest
```

Ensure your copper installation works correctly

```bash
copper -h
```

#### Troubleshoot

If `copper -h` didn't work, it probably means that `$GOPATH/bin` is not in your `$PATH`. Add the following to `~/.zshrc` or `~/.bashrc` -

```
export PATH=$HOME/go/bin:$PATH
```

Then, restart your terminal session and try `copper -h` again.
