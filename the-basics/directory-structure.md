# Directory Structure

When you create a Copper project using the Copper CLI, you'll get a directory structure similar to this -

```
cmd/
├─ app/
│  ├─ main.go
│  ├─ wire.go
pkg/
├─ app/
│  ├─ handler.go
│  ├─ router.go
│  ├─ wire.go
web/
```

All binaries are defined under the `cmd/` directory. By default, the `app` binary is present, and you are welcome to create more binaries here based on your project needs. If your project uses storage, there will be a `migrate` binary that runs database migrations.

Your application code lives under the `pkg/` directory. It is mostly freeform but it is recommended to keep the directory structure as flat as possible. To create new packages, use the Copper CLI's `copper scaffold:pkg <package>` command.

Depending on which template you chose to generate your project with, you may get a `web/` directory. This is where all HTML, CSS, and JS files go. The only Go file here is that enables the embedding of static data into the app binary - so you get a single binary to run your backend and frontend together.
