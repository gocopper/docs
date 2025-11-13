# Create Project

Create your first Copper project

## Quick Start

```bash
copper create github.com/yourname/myapp
cd myapp
copper run -watch
```

Creates a full-stack app with React and PostgreSQL at [http://localhost:5901](http://localhost:5901)

## Options

```bash
copper create -frontend=<option> -storage=<option> github.com/yourname/app
```

### -frontend

| Option                    | Description           |
| ------------------------- | --------------------- |
| `inertia:react` (default) | React with Inertia.js |
| `none`                    | API-only              |

### -storage

| Option               |
| -------------------- |
| `postgres` (default) |
| `sqlite3`            |
| `mysql`              |
| `none`               |
