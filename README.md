# TizyCI GitHub Action

Lightweight CI runner metrics collection with self-contained HTML report for GitHub Actions.

## Usage

Add TizyCI to any GitHub Actions workflow — just one line:

```yaml
- uses: saravananpolyglot/tizyci-github@v1
  with:
    token: ${{ secrets.TIZYCI_AGENT_TOKEN }}
```

That's it. TizyCI starts collecting metrics at the beginning of the job and
automatically stops in the post step, generating an HTML report artifact.

## Full Example

```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: saravananpolyglot/tizyci-github@v1
        with:
          token: ${{ secrets.TIZYCI_AGENT_TOKEN }}

      - uses: actions/checkout@v4

      - name: Build
        run: make build

      - name: Test
        run: make test
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | TizyCI agent token for runner registration | **Yes** | — |
| `api-url` | TizyCI API base URL | No | `https://api.tizycloud.com` |
| `gateway-url` | TizyCI WebSocket gateway URL | No | `wss://wss.tizycloud.com:8443` |
| `theme` | Report theme: `light` or `dark` | No | `dark` |
| `output` | Output report file path | No | `tizyci-metrics-report.html` |
| `upload-artifact` | Upload report as Actions artifact | No | `true` |
| `artifact-name` | Name for the uploaded artifact | No | `tizyci-metrics-report` |
| `github-token` | GitHub token for step detection | No | `${{ github.token }}` |
| `hold-on-failure` | Hold execution on failure for debugging | No | `false` |
| `hold-timeout` | Max hold time on failure (e.g. `10m`, `30m`, `1h`) | No | `30m` |

## What It Collects

- **CPU**: User, system, iowait load
- **Memory**: Active, available, swap usage
- **Disk**: I/O throughput, IOPS, latency, usage
- **Network**: RX/TX throughput, connection states
- **Processes**: Top processes, lifecycle tracking, zombie detection
- **Docker**: Container CPU/memory/network, image pull times
- **System health**: OOM kills, file descriptors, thread count
- **Steps**: Per-step resource analysis with cost estimation

## How It Works

1. The action starts a lightweight Go daemon in the background
2. The daemon collects system metrics at configurable intervals
3. On job completion (post step), it stops collecting and generates an HTML report
4. The report is uploaded as a GitHub Actions artifact

No source code from the metrics agent is included in this repository — only
pre-built binaries and the compiled Action wrapper.

## License

MIT
