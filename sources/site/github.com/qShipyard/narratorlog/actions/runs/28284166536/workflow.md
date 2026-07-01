# Source: https://github.com/qShipyard/narratorlog/actions/runs/28284166536/workflow

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

[CI](https://github.com/qShipyard/narratorlog/actions/workflows/ci.yml)

# Dev #50

[Sign in to view logs](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2FqShipyard%2Fnarratorlog%2Factions%2Fruns%2F28284166536%2Fworkflow)

- [Sign in to view logs](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2FqShipyard%2Fnarratorlog%2Factions%2Fruns%2F28284166536%2Fworkflow)

## Workflow file

# Dev

## 

Dev #50

### Uh oh!

There was an error while loading. [Please reload this page]().

 

#### Workflow file for this run

[.github/workflows/ci.yml](https://github.com/qShipyard/narratorlog/blob/6ab9b196d766dab528ad9f8bac33245bea55b18a/.github/workflows/ci.yml) at [6ab9b19](https://github.com/qShipyard/narratorlog/commit/6ab9b196d766dab528ad9f8bac33245bea55b18a)

| | name: CI |
| --- | --- |
| | |
| | on: |
| | push: |
| | branches: \[main, dev\] |
| | pull\_request: |
| | branches: \[main, dev\] |
| | |
| | jobs: |
| | # Detect which language trees changed so each job runs only when relevant. |
| | # Skipped jobs report success, so they never block an unrelated PR. |
| | changes: |
| | runs-on: ubuntu-latest |
| | outputs: |
| | go: ${{ steps.filter.outputs.go }} |
| | rust: ${{ steps.filter.outputs.rust }} |
| | typescript: ${{ steps.filter.outputs.typescript }} |
| | steps: |
| | \- uses: actions/checkout@v4 |
| | \- uses: dorny/paths-filter@v3 |
| | id: filter |
| | with: |
| | filters: | |
| | go: |
| | \- 'apps/api/\*\*' |
| | rust: |
| | \- 'packages/reader/\*\*' |
| | typescript: |
| | \- 'apps/web/\*\*' |
| | \- 'packages/sdk/\*\*' |
| | \- 'plugins/\*\*' |
| | |
| | go: |
| | name: Go |
| | needs: changes |
| | if: ${{ needs.changes.outputs.go == 'true' }} |
| | runs-on: ubuntu-latest |
| | steps: |
| | \- uses: actions/checkout@v4 |
| | \- uses: actions/setup-go@v5 |
| | with: |
| | go-version: '1.25' |
| | \- name: Test |
| | run: cd apps/api && go test ./... |
| | \- name: Vet |
| | run: cd apps/api && go vet ./... |
| | |
| | rust: |
| | name: Rust |
| | needs: changes |
| | if: ${{ needs.changes.outputs.rust == 'true' }} |
| | runs-on: ubuntu-latest |
| | steps: |
| | \- uses: actions/checkout@v4 |
| | \- uses: dtolnay/rust-toolchain@stable |
| | \- name: Test |
| | run: cd packages/reader && cargo test |
| | \- name: Clippy |
| | run: cd packages/reader && cargo clippy -- -D warnings |
| | |
| | typescript: |
| | name: TypeScript |
| | needs: changes |
| | if: ${{ needs.changes.outputs.typescript == 'true' }} |
| | runs-on: ubuntu-latest |
| | steps: |
| | \- uses: actions/checkout@v4 |
| | \- uses: pnpm/action-setup@v4 |
| | \- uses: actions/setup-node@v4 |
| | with: |
| | node-version: '20' |
| | cache: 'pnpm' |
| | \- run: pnpm install |
| | \- run: pnpm typecheck |
| | \- run: pnpm lint |