# Create Project

Create your first Copper project

### Quick Start

```bash
copper create github.com/yourname/myapp
cd myapp
copper run -watch
```

Creates a full-stack app with React and PostgreSQL at [http://localhost:5901](http://localhost:5901)

### Options

Customize your project template with `-frontend` and `-storage` flags:

```bash
copper create -frontend=<option> -storage=<option> github.com/yourname/app
```

#### -frontend

- `inertia:react` (default) - React with Inertia.js
- `none` - API-only, no frontend

#### -storage

- `postgres` (default) - PostgreSQL database
- `sqlite3` - SQLite database
- `mysql` - MySQL database
- `none` - No database
