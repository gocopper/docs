# Migrations

#### Running Migrations

When developing locally, use `copper run` to run database migrations before starting the app server. You can disable this behavior with `copper run -migrate=false`.

To run migrations in production, use the `migrate` binary built by `copper build`. This binary can be run with `prod.toml` configuration on your server.

#### New Migrations

Migrations are stored in the `migrations/` directory found at the root of the project. By default, it already has `0001_initial.sql` in it that looks similar to -

```sql
-- +migrate Up

-- +migrate Down

```

You can SQL statements that update your database schema after the `-- +migrate Up` line and SQL statements that rollback those changes after the `-- +migrate Down` line -

```sql
-- +migrate Up
CREATE TABLE rockets (
    id text PRIMARY KEY,
    name text
);

-- +migrate Down
DROP TABLE rockets;

```

Each migration file can only be applied once. Once a migration has been applied, create a new migration file `migrations/0002_launches.sql`. Continue creating a new migration file for each change to your database schema.
