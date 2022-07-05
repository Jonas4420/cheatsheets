# Bash

## History expansion

```bash
# Last command
!!
# Last command with sudo
sudo !!

# Command N
!N
# Command -N
!-N
# Most recent call to COMMAND
!<COMMAND>

# Replace first occurence of FROM to TO
!!:s/<FROM>/<TO>/
# Replace all occurences of FROM to TO
!!:gs/<FROM>/<TO>/
```

## Parameter expansion

```bash
# Number of parameters
$?
# All parameters
$@
# Parameter N
$N
# Last parameter
$_

# All parameters from previous command
!*
# First parameter from previous command
!$
# Last parameter from previous command
!$
# 
!$
```

## Files extensions

```bash
$ FILE=file.tar.gz

$ echo ${FILE%.*}
file.tar
$ echo ${FILE%%.*}
file

$ echo ${FILE#*.}
tar.gz
$ echo ${FILE##*.}
gz
```

## Filter process list by regex, except the one greping

```bash
$ ps | grep '[p]rocess'
```

## File manipulation

**/!\ Caution**: `<FILE>` needs to be substituted by the file name, including the matching brackets.

```bash
# Dump file into variable
$ var=$(< <FILE>)

# Truncate file
$ > <FILE>

# Read lines of file
while IFS= read -r line; do
	echo "$line"
done < <FILE>
```

## Variable manipulation

```bash
# Default if unset
${FOO:default}
# Default if unset or null
${FOO:-default}
# Set FOO to default if unset or null, and returns FOO
${FOO:=default}

# Substring (position, length)
${FOO:0:3}
# Substring from right
${FOO:(-3):3}

# Lower case first letter
${STR,}
# Lower case all
${STR,,}
# Upper case first letter
${STR^}
# Upper case all
${STR^^}

# Replace first match
${STR/FROM/TO}
# Replace all matches
${STR//FROM/TO}

# Replace prefix
${STR/#PREFIX/TO}
# Replace suffix
${STR/%SUFFIX/TO}
```

## Array

```bash
# Declare array
fruits=("Apple" "Banana" "Orange")
# Array Length
${#fruits[@]}

# Element N
${fruits[N]}
# Last element
${fruits[-1]}

# From position N to last
${fruits[@]:N}
# Slice (position, length)
${fruits[@]:N:M}
```

## Join list

```bash
join_array()
{
	local delim=$1
	local first=$2

	shift 2 && printf "%s" "$first" "${@/#/${delim}}"
}

$ join_array ',' a b c d
a,b,c,d
```
