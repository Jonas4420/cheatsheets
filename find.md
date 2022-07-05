# Find

## Search by filename

```shell
find . -name '<REGEX>'
```

## Search by file type

```shell
# Regular file
find . -type f
# Directory
find . -type d
# Symbolic link
find . -type l
```

## User / Group

```shell
# USER and GROUP can be name or numeric ID
find . -user <USER>
find . -group <GROUP>
```

## Size

```shell
# Exactly n units
find . -size n

# Greater than n units
find . -size +n
# Less than n units
find . -size -n
# MIN < size < MAX
find . -size +MIN -size -MAX

# Files less than 1X (but because of round up, so empty files only)
find . -size -1X
# Files less than 1024 * 1024 bytes (no round up)
find . -size -1048576c

# Empty files
find . -empty
```

Units (values are rounded up to unit, so it can be tricky)
- `c`: (bytes, *default*)
- `k`: (kilo)
- `M`: (mega)
- `G`: (giga)
- ...

## Permissions

```shell
# Exact match
find . -perm MODE

# All bits of MODE are set
find . -perm /MODE
# Any bits of MODE are set
find . -perm -MODE

# Executable
find . -executable
```

`MODE` can be octal (`640`, `0640`, ...) or symbolic (`u=rw`, `u=rw,g=r`, ...)

## Execute something on matches

```shell
# Execute command on each file individually
find . -exec <COMMAND> \;
# Positional argument with {}
find . -exec <COMMAND> '{}' \;

# Group all matching files, and run command on the group
find . -exec <COMMAND> +
```

## Execute something on matches with xargs

```shell
# Run on group of matched files
find . -print0 | xargs -0 <COMMAND>
# Positional argument with {}
find . -print0 | xargs -0 -n 1 -I {} <COMMAND>

# Group matches by N, and run the command on each groups
find . -print0 | xargs -0 -n N <COMMAND>
```

## Delete files

```shell
find . -name '<REGEX>' -delete
```
