# Create Project

Use the `copper create` command to create a new project -

```
$ copper create github.com/nasa/starship
$ cd starship
$ copper run -watch
```

Open [_http://localhost:5901_](http://localhost:5901) in your browser.



You can configure the project template using `-frontend` and `-storage` arguments.

#### -frontend

The Copper CLI provides templates for the following frontend frameworks & libraries -

* [Go Templates](go-templates.md) (default)
* [Tailwind](tailwind.md)
* [React](react.md)
* [React + Tailwind](react-+-tailwind.md)
* [REST API (no frontend)](rest-api.md)

#### -storage

Configure your storage stack using the `-storage` argument. By default, it's set to `sqlite3`. You have options to set it to `postgres`, `mysql`, or skip storage entirely with `none`.

