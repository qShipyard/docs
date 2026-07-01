# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/Dockerfile.reader

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# Dockerfile.reader

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/Dockerfile.reader)

History

16 lines (10 loc) · 300 Bytes

 main

/

# Dockerfile.reader

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

16 lines (10 loc) · 300 Bytes

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/Dockerfile.reader)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

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

FROM rust:1.78-alpine AS builder

RUN apk add --no-cache musl-dev

WORKDIR /app

COPY packages/reader/ .

RUN cargo build --release

FROM alpine:3.19

WORKDIR /app

COPY --from=builder /app/target/release/narratorlog-reader .

ENV READER\_SOCKET=/tmp/narratorlog-reader.sock

CMD \["./narratorlog-reader"\]