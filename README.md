This is a minimal snowflake experiment for shadow for debugging purposes only. This will not produce scientifically accurate results.

## Prerequisites

We'll need to compile and prepare Snowflake for shadow experimentation. Also make sure that tgen and shadow are installed in `~/.local/bin` as per instructions in the [shadow user manual](https://shadow.github.io/docs/guide/getting_started_tgen.html).

### Preparing Snowflake binaries

The configuration for this experiment assumes the following Snowflake binaries are installed in `~/.local/bin`:
- snowflake-server
- snowflake-broker
- snowflake-proxy
- snowflake-client

To compile and install the above binaries: clone the repo, check out the shadow branch, and run the following commands.

Note: the snowflake server requires the provided [patch](0001-Snowflake-shadow-patch.patch) to avoid error in the absense of support in Shadow for the `IP_BIND_ADDRESS_NO_PORT` socket option.

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

## Running shadow

The go runtime requires a special flag to run without blocking:

```
shadow --model-unblocked-syscall-latency=true snowflake-minimal.yaml > shadow.log
```
