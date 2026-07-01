# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/migrate.sh

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# migrate.sh

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/migrate.sh)

History

11 lines (8 loc) · 152 Bytes

 main

/

# migrate.sh

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

11 lines (8 loc) · 152 Bytes

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/migrate.sh)

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

#!/bin/sh

set -e

echo "Running database migrations..."

migrate \\

\-path /migrations \\

\-database "$DATABASE\_URL" \\

up

echo "Migrations complete."