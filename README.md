# ipregion

Checks your IP geolocation against 40+ services in parallel — GeoIP databases, streaming platforms, and CDN endpoints. Shows a consensus country code across all GeoIP results.

## Quick start

```bash
wget -qO- https://raw.githubusercontent.com/softhoz/ipregion/main/ipregion.sh | bash
```

Or run locally:

```bash
curl -O https://raw.githubusercontent.com/softhoz/ipregion/main/ipregion.sh
chmod +x ipregion.sh
./ipregion.sh
```

## Requirements

- `bash` 4+
- `curl`
- `jq`

Missing packages are detected automatically and you will be prompted to install them. Supported package managers: `apt`, `pacman`, `dnf`, `yum`, `apk`, `zypper`, `brew`.

## Usage

```
./ipregion.sh [OPTIONS]

Options:
  -h, --help           Show help and exit
  -g, --group GROUP    Run one group: primary | custom | cdn | all (default: all)
  -4, --ipv4           Test IPv4 only
  -6, --ipv6           Test IPv6 only
  -s, --service NAME   Query a single service (e.g. -s NETFLIX)
  -p, --proxy ADDR     SOCKS5 proxy (host:port)
  -i, --interface IF   Bind to a specific network interface
  -t, --timeout SEC    Per-request timeout in seconds (default: 5)
  -j, --json           Output results as JSON
  -v, --verbose        Enable verbose logging
  -d, --debug          Save full trace and upload to 0x0.st
      --no-color       Disable color output (also respects $NO_COLOR)
```

## Examples

```bash
./ipregion.sh                      # all services, dual-stack
./ipregion.sh -4                   # IPv4 only
./ipregion.sh -g primary           # GeoIP services only
./ipregion.sh -g custom            # streaming/popular services only
./ipregion.sh -g cdn               # CDN endpoints only
./ipregion.sh -s NETFLIX           # single service
./ipregion.sh -p 127.0.0.1:1080    # through a SOCKS5 proxy
./ipregion.sh -j | jq .results    # JSON output
./ipregion.sh --no-color           # plain text (pipe-friendly)
```

## Configuration

**Config file** — set persistent defaults in `~/.config/ipregion.conf`:

```bash
CURL_TIMEOUT=10
GROUPS_TO_SHOW=primary
NO_COLOR_MODE=true
```

**API key overrides** — place a `.env` file in the current directory or at `~/.config/ipregion.env` to override embedded tokens:

```bash
NETFLIX_API_KEY=your_key
SPOTIFY_API_KEY=your_key
```

Tokens for Netflix, Twitch, and ChatGPT are refreshed automatically if they fail.

## Service groups

| Group | What it checks |
|---|---|
| `primary` | 17 GeoIP databases (MaxMind, ipinfo.io, Cloudflare, RIPE, …) |
| `custom` | 20 streaming/popular services (Netflix, Spotify, YouTube, Reddit, …) |
| `cdn` | Cloudflare CDN, YouTube CDN, Netflix CDN — reports nearest PoP |
