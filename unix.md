# Unix

- [Archives](#archives)
- [dd](#dd)
- [diff](#diff)
- [Kerberos](#kerberos)
- [ln](#ln)
- [Luks](#luks)
- [rsync](#rsync)
- [sed](#sed)
- [ssh](#ssh)
- [Steganography tools](#steganography-tools)
- [systemctl](#systemctl)
- [Users and groups](#users-and-groups)
- [VirtualBox](#virtualbox)

# Archives

```shell
# List content of tarball (compression method automatically detected)
tar tf <ARCHIVE>
# Same, but verbose
tar tvf <ARCHIVE>

# List content of zip archive
unzip -l <ARCHIVE>
# Same, but verbose
unzip -lv <ARCHIVE>
```

# dd

```shell
dd if=<IN> of=<OUT> bs=4M && sync
```

# diff

```shell
# Summary of differences between two directories
diff -qr <DIR_1> <DIR_2>
```

# Kerberos

## List supported ciphers for keytab

```shell
$ klist -e
Ticket cache: FILE:<TICKET_CACHE>
Default principal: <PRINCIPAL>

Valid starting       Expires              Service principal
01/01/1970 00:00:00  01/01/1970 03:00:00  krbtgt/<DOMAIN>
        renew until 01/01/1970 10:00:00, Etype (skey, tkt): <CIPHER>
```

## Update keytab file

```shell
# Enter keytab utility
$ ktutil

ktutil:  read_kt keytab
ktutil:  list
slot KVNO Principal
---- ---- ---------------------------------------------------------------------
   1    1                 <PRINCIPAL>
ktutil:  delete_entry 1
ktutil:  add_entry -password -p <PRINCIPAL> -k 1 -e <CIPHER>
Password for <PRINCIPAL>:
ktutil:  write_kt keytab.new
ktutil:  quit

# Update keytab file
mv .keytab .keytab.old
mv .keytab.new .keytab
```

# ln

```shell
# Create symlink on a regular file
ln -s <FILE> <SYMLINK>

# Create symlink on a directory (/!\ omit trailing slashes)
ln -s <DIR> <SYMLINK>
```

# Luks

## Dump

```shell
# Dump Luks metadata
sudo cryptsetup luksDump <DEVICE>

# Test passphrase
cryptsetup open --verbose --test-phassphrase <DEVICE>

# Change passphrase
sudo cryptsetup luksChangeKey <DEVICE>
```

## Change passphrase

# rsync

## Trailing slash caveat

```shell
# Copy source directory into destination (cp -r <SRC> <DST>)
rsync -r <SRC> <DST>
# Copy source directory content into destination (cp -r <SRC>/. <DST>)
rsync -r <SRC>/ <DST>
```

## Automatic backup

```shell
# Copy source directory into destination, keep files only in destination
rsync -aPh <SRC> <DST>

# Copy source directory into destination, delete files only in destination
rsync -aPh --delete <SRC> <DST>
```

## Remote backup

```shell
rsync -aPh <SRC> <USER>@<REMOTE>:<DST>
```

# sed

## Filters

```shell
# Print only lines that match the regex
sed -n '/<REGEX>/p'
# Print only lines that don't match the regex
sed -n '/<REGEX>/!p'

# Print lines 1-4
sed -n '1,4 p'
# Print lines 1-4, 6-7
sed -n -e '1,4 p' -e '6,7 p'

# Execute multiple commands
sed -e <COMMAND_1> -e <COMMAND_2>
```

## Search and replace

```shell
# Replace only first occurences of FROM to TO
sed 's/<FROM>/<TO>/'
# Replace all occurences of FROM to TO
sed 's/<FROM>/<TO>/g'

# Ignore case of input
sed 's/<FROM>/<TO>/I'

# Group and references
sed -E 's/(<GROUP_1>)<REGEX>(<GROUP_2>)/\1\2/'

# Replace FROM to TO, only on the line 2
sed '2 /s/<FROM>/<TO>/'
# Replace FROM to TO, only if the line match REGEX
sed '/<REGEX>/s/<FROM>/<TO>/'
```

## File manipulation

```shell
# Insert TEXT before the first line
sed '^i END'
# Insert TEXT before line 2
sed '2i <TEXT>'
# Insert TEXT before a line, if it matches REGEX
sed '/<REGEX>/i <TEXT>'

# Insert TEXT after the last line
sed '$a END'
# Insert TEXT after line 2
sed '2a <TEXT>'
# Append TEXT from line 2, after every 3rd line
sed '2~3a <TEXT>'

# Delete line 2
sed '2d' file.txt
# Delete last line
sed '$d' file.txt
# Delete empty lines
sed '/^$/d' file.txt

# Interleave a new line between lines
sed G
# Delete empty lines, and interleave a new line between lines
sed '/^$/d;G'

# Print line number before each line
sed '='
# Group a line with next one for matching
sed 'N;s/\n/\t/'
# Print line number before each line (left aligned)
sed '=' | sed 'N;s/\n/\t/'
```

# ssh

## Suspend session

```shell
# Needs to be immediatly after new line
~^Z
```

## Connection to server through a proxy

When forwarding is allowed:
```
Host <DESTINATION>
	Hostname "<HOSTNAME>"
	User <USER>
	ProxyJump <USER>@<PROXY>
```

When forwarding is not allowed:
```
Host <DESTINATION>
	Hostname "<HOSTNAME>"
	User <USER>
	ProxyCommand ssh <PROXY> nc -q0 %h %p 2> /dev/null
```

## Misc commands

```shell
# List supported key types
ssh -Q key

# Probe a server supported algorithms
nmap --script ssh2-enum-algos -sV -Pn -p 22 <HOSTNAME>

# Get info about an existing key (public or private)
ssh-keygen -l -f <KEYFILE>

# Remove an entry from known_hosts
ssh-keygen -R <HOSTNAME>
```

# Steganography tools

- `outguess`
- `steghide`
- `jphide` / `jpseek`: Linux and Windows version methods differ
- `stegdetect`: staganalysis tool

# systemctl

```shell
# Get service status
systemctl status <SERVICE>


# Start a service
systemctl start '<SERVICE>'
# Stop a service
systemctl stop '<SERVICE>'
# Restart a service
systemctl restart '<SERVICE>'
# Enable service at boot
systemctl enable '<SERVICE>'

# List all services
systemctl list-units --all
# List all unit files
systemctl list-unit-files
# Display dependencies
systemctl list-dependencies <SERVICE>

# Display a unit file
systemctl cat <SERVICE>
# Reload unit files after editing them
systemctl daemon-reload

# Mark a service as unstartable
systemctl mask <SERVICE>
# Unmask a service and return it to its previous state
systemctl unmask <SERVICE>
```

# Users and groups

```shell
# Add a new user, and create home directory
adduser -m <USER>

# Add another group to user
usermod -aG <GROUP> <USER>

# List current user groups
groups
# List another user groups
groups <USER>

# List users on the system
compgen -u
# List groups on the system
compgen -g
```

# VirtualBox

```shell
# List VMs
VBoxManage list vms
# List running VMs
VBoxManage list runningvms

# Start a VM in headless mode
VBoxManage startvm <VMNAME> --type headless
```
