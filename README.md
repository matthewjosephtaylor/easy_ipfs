# Easy IPFS

Shell script wrappers around IPFS docker container to make life a bit easier.

Usage:

## Downloading

```
git clone https://github.com/matthewjosephtaylor/easy_ipfs.git
```

## Starting

```
./ipfs-start <some-name>
```

## Stopping

```
./ipfs-stop
```

## Switching servers

```
./ipfs-switch <name>
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

## Details

- All Configuration and data located in `storage/{server-name}/data`
- The `staging` directory exists to copy files into the container when adding files to IPFS
