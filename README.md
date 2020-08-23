# Easy IPFS

Shell script wrappers around IPFS docker containers to make life a bit easier.

Allows one to easily manage IPFS inside a docker container

- Build different versions of IPFS
- One command setup for a public swarm or private swarm
- Easily manage IPFS configuration

## Downloading / Installing

```
git clone https://github.com/matthewjosephtaylor/easy_ipfs.git
cd easy_ipfs
```

## Configuring

```
vi easy_ipfs/config/constants
```

## Build local IPFS container from source (edit config/constants to use the official containers)

- Useful to get latest version on any architecture (like arm64)

```
./ipfs-build
```

## Setup public IPFS container

```
./ipfs-public-setup
./ipfs-start -ep
```

## Setup private IPFS container

```
./ipfs-create-swarm-secret      # Will output a random secret, copy this and use for other nodes in private swarm
./ipfs-private-setup <secret>
./ipfs-start -ep
```

## Start container

- Start with either the container ports exposed or unexposed (default unexposed)

```
./ipfs-start [--expose-ports]
```

## Stop container

```
./ipfs-stop
```

## Switch container
- Can have multiple containers running at once.
- NOTE: currently only one container can have ports exposed since they all share the same config

```
./ipfs-switch <container-name>
```

## Add file to IPFS

```
./ipfs-add <path-to-file>
```

## Cat IPFS ContentID (aka CID)

```
./ipfs-cat Qmdeadbeef.... > <some-filename>
```

## Run random IPFS command

```
./ipfs-cmd help
```

## Shell into IPFS Docker container

```
./ipfs-shell
```

## Many other commands

The list of commands will continue to grow over time. Suggest looking at contents of script to see what they do.

## Details

- All Configuration and data located in `storage/{server-name}/data`
- The `staging` directory exists to copy files into the container when adding files to IPFS
- Change Go / Javascript implementation in `config/constants`

## Blame

Matt Taylor https://mjt.dev
