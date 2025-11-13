# Tailwind

If you'd like to style your HTML pages with [Tailwind](https://tailwindcss.com/), use the `go:tailwind` frontend stack -

```
$ copper create -frontend=go:tailwind github.com/nasa/starship
```

Run the server -

```bash
$ cd starship
$ copper run -watch
```

In a separate terminal window, run the Tailwind server -

```
$ cd starship/web
$ npm run dev
```

Open [_http://localhost:5901_](http://localhost:5901)
