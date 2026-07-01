# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/Dockerfile.api

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# Dockerfile.api

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

[![phonixcode](https://avatars.githubusercontent.com/u/48732133?v=4&size=40)](https://github.com/phonixcode) [phonixcode](https://github.com/qShipyard/narratorlog/commits?author=phonixcode)

[Implement team configuration management with routing and integration …](https://github.com/qShipyard/narratorlog/commit/a8c7b8af12ec9a9189fc20487ad03c3930cd4b75)

Open commit detailsfailure

Jun 27, 2026

[a8c7b8a](https://github.com/qShipyard/narratorlog/commit/a8c7b8af12ec9a9189fc20487ad03c3930cd4b75) · Jun 27, 2026

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/Dockerfile.api)

Open commit details

History

45 lines (29 loc) · 1.16 KB

## FilesExpand file tree

 main

/

# Dockerfile.api

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

45 lines (29 loc) · 1.16 KB

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/Dockerfile.api)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

FROM golang:1.25-alpine AS go-builder WORKDIR /app RUN apk add --no-cache git COPY apps/api/go.mod apps/api/go.sum ./ RUN go mod download COPY apps/api/ . RUN CGO\_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o narratorlog-api ./cmd/api RUN CGO\_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o narratorlog-worker ./cmd/worker FROM node:20-alpine AS plugin-builder RUN corepack enable WORKDIR /app COPY package.json pnpm-workspace.yaml pnpm-lock.yaml ./ COPY packages/sdk/ ./packages/sdk/ COPY plugins/ ./plugins/ RUN pnpm install --frozen-lockfile RUN pnpm --filter '@narratorlog/sdk' --filter './plugins/\*\*' run build FROM alpine:3.19 RUN apk add --no-cache ca-certificates nodejs WORKDIR /app COPY --from=go-builder /app/narratorlog-api . COPY --from=go-builder /app/narratorlog-worker . # Built TypeScript plugins, the SDK, and the resolved workspace node\_modules # (plugins are invoked at runtime via \`node plugins/<type>/<name>/dist/index.js\`). COPY --from=plugin-builder /app/node\_modules ./node\_modules COPY --from=plugin-builder /app/packages ./packages COPY --from=plugin-builder /app/plugins ./plugins EXPOSE 8080 ENTRYPOINT \["./narratorlog-api"\] CMD \[\]

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

FROM golang:1.25-alpine AS go-builder

WORKDIR /app

RUN apk add --no-cache git

COPY apps/api/go.mod apps/api/go.sum ./

RUN go mod download

COPY apps/api/ .

RUN CGO\_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o narratorlog-api ./cmd/api

RUN CGO\_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o narratorlog-worker ./cmd/worker

FROM node:20-alpine AS plugin-builder

RUN corepack enable

WORKDIR /app

COPY package.json pnpm-workspace.yaml pnpm-lock.yaml ./

COPY packages/sdk/ ./packages/sdk/

COPY plugins/ ./plugins/

RUN pnpm install --frozen-lockfile

RUN pnpm --filter '@narratorlog/sdk' --filter './plugins/\*\*' run build

FROM alpine:3.19

RUN apk add --no-cache ca-certificates nodejs

WORKDIR /app

COPY --from=go-builder /app/narratorlog-api .

COPY --from=go-builder /app/narratorlog-worker .

\# Built TypeScript plugins, the SDK, and the resolved workspace node\_modules

\# (plugins are invoked at runtime via \`node plugins/<type>/<name>/dist/index.js\`).

COPY --from=plugin-builder /app/node\_modules ./node\_modules

COPY --from=plugin-builder /app/packages ./packages

COPY --from=plugin-builder /app/plugins ./plugins

EXPOSE 8080

ENTRYPOINT \["./narratorlog-api"\]

CMD \[\]