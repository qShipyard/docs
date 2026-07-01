# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/Dockerfile.web

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# Dockerfile.web

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/Dockerfile.web)

History

23 lines (14 loc) · 363 Bytes

## FilesExpand file tree

 main

/

# Dockerfile.web

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

23 lines (14 loc) · 363 Bytes

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/Dockerfile.web)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

FROM node:20-alpine AS builder WORKDIR /app COPY apps/web/package\*.json ./ RUN npm ci COPY apps/web/ . RUN npm run build FROM node:20-alpine WORKDIR /app ENV NODE\_ENV=production COPY --from=builder /app/.next/standalone ./ COPY --from=builder /app/.next/static ./.next/static COPY --from=builder /app/public ./public EXPOSE 3000 CMD \["node", "server.js"\]

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

FROM node:20-alpine AS builder

WORKDIR /app

COPY apps/web/package\*.json ./

RUN npm ci

COPY apps/web/ .

RUN npm run build

FROM node:20-alpine

WORKDIR /app

ENV NODE\_ENV=production

COPY --from=builder /app/.next/standalone ./

COPY --from=builder /app/.next/static ./.next/static

COPY --from=builder /app/public ./public

EXPOSE 3000

CMD \["node", "server.js"\]