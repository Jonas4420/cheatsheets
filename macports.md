# MacPorts

## Update MacPorts and the ports tree

```shell
sudo port selfupdate
```

## Search ports

```shell
# Search for ports
port search <QUERY>

# Search for ports (name only)
port search --name <QUERY>
# Search for ports (exact match)
port search --name --exact <QUERY>
# Search for ports (matching regex)
port search --name --regex <REGEX>
```

## Install, update and uninstall

```shell
# Install a port
port install <PORT>

# List outdated ports
port outdated
# Upgrade all outdated ports
port upgrade outdated
# Upgrade a specific port
port upgrade <PORT>

# Uninstall a port
port uninstall <PORT>
```

## Port administration

```shell
# Set a port as explicitly installed
port setrequested <PORT>

# Show available selection groups
port select --summary
# Select the port to be primary selection for the group
port select --set <GROUP> <PORT>
```

## List installed ports 

```shell
# List all installed ports (even inactive ones)
port installed
# List installed versions of a given port
port installed <PORT>

# List explicitly installed ports
port echo requested
```

## Port informations

```shell
# Show port infos
port info <PORT>

# List files installed by a port
port contents <PORT>

# Show active version of port
port list <PORT>

# Show port dependencies
port deps <PORT>
# Show port dependency tree
port rdeps <PORT>
# Show reverse dependencies
port echo dependentof:<PORT>
```

## System cleanup

```shell
# Clean temporary files of installed ports
port clean --all installed

# Uninstall unrequested ports with no dependaents
port uninstall leaves
# Uninstall unrequested ports that no requested ports depend on
port uninstall rleaves

# List inactive ports
port echo inactive
# Uninstall inactive ports
port uninstall inactive
```
