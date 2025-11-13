# Queries

#### Queries

By convention, all database queries are wrapped in a Queries layer that can be generated using the Copper CLI -

```bash
copper scaffold:queries rockets
```

{% code title="pkg/rockets/queries.go" %}
```go
func NewQueries(querier csql.Querier) *Queries {
        return &Queries{
                querier: querier,
        }
}

type Queries struct {
        querier csql.Querier
}

// Add methods on the Queries struct that query the database
```
{% endcode %}

#### Querier

The auto-generated `Queries` struct has a dependency on `csql.Querier`. Querier allows you to run queries safely on your database and scan results into Go structs and slices with very little boilerplate.

The example below queries the `rockets` table and scans the results into the `rockets` slice -

```go
func (q *Queries) ListRockets(ctx context.Context) ([]Rocket, error) {
	const query = "SELECT * FROM rockets ORDER BY launch_date DESC"

	var (
		rockets []Rocket
		err = q.querier.Select(ctx, &rockets, query)
	)

	return rockets, err
}
```

Similarly, the example below scans the result into a single Go struct -

```go
func (q *Queries) GetRocketByID(ctx context.Context, id string) (*Rocket, error) {
	const query = "SELECT * FROM rockets where id=?"

	var (
		rocket Rocket
		err = q.querier.Get(ctx, &rocket, query, id)
	)

	return &rocket, err
}
```

Finally, if you want to execute SQL statements (such as `DELETE`), use the `Querier.Exec` method  -

```go
func (q *Queries) DeleteRocketByID(ctx context.Context, id string) error {
	const query = "DELETE FROM rockets where id=?"

	_, err := q.querier.Exec(ctx, query, id)
	return err
}
```

#### Transaction Middleware

The `csql.TxMiddleware` can be used to wrap every HTTP request in a database transaction. The transaction is automatically committed if the response code from the handler is success-like (i.e. 100-399) or rolled back for any other status code or even panics.

The middleware is added as a global middleware by default in `pkg/app/handler.go`.
