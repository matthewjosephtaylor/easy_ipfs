# Easy IPFS

Shell script wrappers around IPFS docker container to make life a bit easier.

## Downloading / Installing

```
git clone https://github.com/matthewjosephtaylor/easy_ipfs.git
cd easy_ipfs
```

## Build local IPFS container from source (edit config/constants to use the official containers)

```
./ipfs-build
```

## Initialize

```
./ipfs-init
```

## Starting

```
./ipfs-start [--expose-ports]
```

## Stopping

```
./ipfs-stop
```

## Switching servers

```
./ipfs-switch <server-name>
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
