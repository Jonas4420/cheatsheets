# dig

```shell
# Get everything
dig example.com

# Get MX records only
dig example.com MX

# Use a specific DNS server
dig @1.1.1.1 example.com
```

# lsof

```shell
# List open IP connections (hostname and port are not converted)
lsof -Pn -i
```

# nc

## TCP

```shell
# Connect
nc <HOSTNAME> <PORT>

# Listen (Linux)
nc -l -p <PORT>
# Listen (BSD)
nc -l <PORT>
```

## UDP

```shell
nc -u <HOSTNAME> <PORT>

# Listen (Linux)
nc -l -u -p <PORT>
# Listen (BSD)
nc -l -u <PORT>
```

## Check open port

```shell
# Scan port only, close the connection if established
nc -z <HOST> <PORT>
```

# nmap

```shell
# Scan specific ports, no ping
nmap -Pn -p 80,443,555 <HOST>
# Scan all ports, no ping, no DNS resolution
nmap -Pn -n -p 0-65535 <HOST>

# Scan all ports, be aggressive, probe for OS/app versions
nmap -A -T4 -p 1-65535 -v <HOST>

# Host discovery on a CIDR range
nmap -sP 10.0.0.0/24
```

# whois

```shell
whois example.com
```
