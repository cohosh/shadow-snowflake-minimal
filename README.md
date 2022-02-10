This is a minimal snowflake experiment for shadow for debugging purposes only. This will not produce scientifically accurate results.

## Prerequisites

We'll need to compile and prepare Snowflake for shadow experimentation. Also make sure that tgen and shadow are installed in `~/.local/bin` as per instructions in the [shadow user manual](https://shadow.github.io/docs/guide/getting_started_tgen.html).

### Preparing Snowflake binaries

The configuration for this experiment assumes the following Snowflake binaries are installed in `~/.local/bin`:
- snowflake-server
- snowflake-broker
- snowflake-proxy
- snowflake-client

Snowflake requires a patch to work correctly in a closed environment. A working [branch with this patch](https://gitlab.torproject.org/cohosh/snowflake/-/tree/shadow) is a available, the last commit of which may be rebased ontop of a more recent version of Snowflake. To compile and install the above binaries: clone the repo, check out the shadow branch, and run the following commands.

```
cd server
go build
cp server ~/.local/bin/snowflake-server

cd ../broker
go build
cp broker ~/.local/bin/snowflake-broker

cd ../proxy
go build
cp proxy ~/.local/bin/snowflake-proxy

cd ../client
go build
cp client ~/.local/bin/snowflake-client
```

### Preparing STUN binary

We also need a binary for the STUN server. The simplest one I have found that currently works in Shadow is a Go project called [stund](https://github.com/gortc/stund). To install in the right place, simply run

`GOBIN=~/.local/bin go install github.com/gortc/stund@latest`
