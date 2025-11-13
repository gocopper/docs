# Deploy with Fly.io

To deploy your Copper app on [fly.io](https://fly.io/), you'll first need to containerize the app.



Create `Dockerfile` in your project root:

```docker
# syntax = docker/dockerfile:1-experimental
FROM node:18-alpine AS web-build

WORKDIR /root
COPY web/ ./

RUN npm install
RUN npm run build

FROM golang:1.17-alpine AS go-build

RUN apk add build-base
RUN go install github.com/gocopper/cli/cmd/copper@v1.2.2

WORKDIR /root

COPY go.mod go.mod
COPY go.sum go.sum
RUN go mod download

COPY . .
COPY --from=web-build /root/build ./web/build/.

RUN --mount=type=cache,target=/root/.cache/go-build copper build

FROM alpine:3.16

WORKDIR /root

COPY config/ config/

COPY --from=go-build /root/build/migrate.out .
RUN ["./migrate.out", "--config", "config/prod.toml"]

COPY --from=go-build /root/build/app.out .
CMD ["./app.out", "--config", "config/prod.toml"]
```

Customize `Dockerfile` for your project like so:

* If your project doesn't have a frontend, you can safely remove lines 2-9 and line 22
* If your project doesn't use any storage, you can safely remove lines 32-33

Run `flyctl launch` to launch your app.



You can find more details on deploying your Docker app here:\
[https://fly.io/docs/getting-started/dockerfile/](https://fly.io/docs/getting-started/dockerfile/)
