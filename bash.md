# Bash

## History expansion

```bash
!!					# Last command
sudo !!					# Last command with sudo

!N					# Command N from top of history
!-N					# Command N from bottom of history
!<COMMAND>				# Most recent call to COMMAND

!*					# All parameters of last command
!^					# First parameter of last command
!$					# Last parameter of last command

!!:s/<FROM>/<TO>/			# Replace first occurence of FROM to TO
!!:gs/<FROM>/<TO>/			# Replace all occurences of FROM to TO
```

## Special parameters

```bash
$0					# Name or path of the script (not reliable)
$1					# Positional parameter
$#					# Number of positional parameters

# If all the positional parameters are: "a" "b" "c d"
$*					# Split as a list of words: <a><b><c><d>
$@					# Split as a list of words: <a><b><c><d>
"$*"					# Join all the words using IFS character: <a b c d>
"$@"					# Expand as a list of the individual words: <a><b><c d>

$$					# PID of the current shell
$!					# PID of the last executed command
$?					# Exit code of last executed command
```

## Parameter expansion

```bash
${foo:VALUE}				# use default VALUE (if foo is unset)
${foo:-VALUE}				# use default VALUE (if foo is unset or null)
${foo:+VALUE}				# use alternate VALUE (if foo is not unset or not null)
${foo:=VALUE}				# assign default VALUE (if foo is unset or null)

${foo:offset:length}			# Substring
${#foo}					# String length

${foo}					# hello, World!
${foo^}					# Hello, World!
${foo^^}				# HELLO, WORLD!
${foo,}					# hello, World!
${foo,,}				# hello, world!

${foo/FROM/TO}				# Replace first match
${foo//FROM/TO}				# Replace all matches
${foo/#PREFIX/TO}			# Replace prefix
${foo/%SUFFIX/TO}			# Replace suffix

${file}					# file.tar.gz
${file#*.}				#      tar.gz
${file##*.}				#         .gz
${file%.*}				# file.tar
${file%%.*}				# file

${file}					# /usr/include/stdio.h
${file#*/}				#  usr/include/stdio.h
${file##*/}				#              stdio.h
${file%/*}				# /usr/include
${file%%/*}				#
```

## Array

```bash
fruits=("Apple" "Banana" "Orange")	# Declare array

${fruits[N]}				# Element N
${fruits[-1]}				# Last element

${fruits[@]}				# All elements
${fruits[@]:offset:length}		# Slice
${#fruits[@]}				# Array Length
```

## File manipulation

```bash
var=$(< ${file})			# Dump file into variable
> ${file}				# Truncate file

# Read lines of file
while IFS= read -r line; do
	echo "$line"
done < ${file}
```

## Filter process list by regex, except the one greping

```bash
$ ps | grep '[p]rocess'
```

## Join list

```bash
join_array()
{
	local delim=$1
	local first=$2

	shift 2 && printf "%s" "$first" "${@/#/${delim}}"
}

join_array ',' a b c d			# a,b,c,d
```
