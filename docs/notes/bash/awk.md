# Awk

[WIKI](https://en.wikipedia.org/wiki/AWK) AWK  is a scripting language designed for text processing and typically used as a data extraction and reporting tool. Like `sed` and `grep`, it is a filter, and it is a standard feature of most Unix-like operating systems.

It works **line by line**, splitting input into fields and applying actions.

1. Basic Usage\Structure:
```bash
awk 'pattern { action }' file
```

* pattern: condition to match (like if).
* action: what to do if the pattern matches (default = print line).
* file: input file (or you can pipe data in).

> If no pattern → action applies to every line.

> If no action → default action is { print $0 } (print entire line).


2. Fields

AWK splits each line by whitespace (default).

* $0 → whole line
* $1 → first field
* $2 → second field
* ..can go indefinitely with integers
* NF → number of fields
* NR → record (line) number

```bash
echo "Alice 30 Doctor" | awk '{print $1, $2}'
# Output: Alice 30
echo "Alice 30 Doctor" | awk '{print $1, $3}'
# Output: Alice Doctor
```

test.txt:
```bash
cat test.txt

asdfasdf
fjfjfjfjfj
aaaaa
```

```bash
awk '{print $1}' test.txt
# Output:
# asdfasdf
# fjfjfjfjfj
# aaaaa

awk '{print $2}' test.txt
# Output:
# 
# 
# 
```
> Note each line only has 1 field.

Also using cat on a file and piping to awk is the same as above.
```bash
cat test.txt | awk '{print $1}'
# Output:
# asdfasdf
# fjfjfjfjfj
# aaaaa

cat test.txt | awk '{print $2}'
# Output:
# 
# 
# 
```

1. Useful Examples
```bash
awk '{print NR, $0}' test.txt
# Output
# 1 asdfasdf
# 2 fjfjfjfjfj
# 3 aaaaa
awk '{print NR, $1}' test.txt
# Output
# 1 asdfasdf
# 2 fjfjfjfjfj
# 3 aaaaa
awk '{print NF, $0}' test.txt
# Output
# 1 asdfasdf
# 1 fjfjfjfjfj
# 1 aaaaa
```

4. Built in Variables
```bash
* NR → line number
* NF → number of fields
* FS → input field separator (default: space)
* OFS → output field separator (default: space)
```


```bash
cat test.txt
# Output
# a
# b
# c
awk 'BEGIN {OFS="|"} {print $1}' test.txt
# Output
# a
# b
# c
awk 'BEGIN {OFS="|"} {print $1,$2}' test.txt
# Output
# a|
# b|
# c|
awk 'BEGIN {OFS="|"} {print $1,$2,$3}' test.txt
# Output
# a||
# b||
# c||
cat test.txt
# Output
# a,1,2,3
# b,2,4,6
# c,3,6,9
awk 'BEGIN {FS=","; OFS="|"} {print $1,$2,$3}' test.txt
# Output
# a|1|2
# b|2|4
# c|3|6
awk 'BEGIN {FS=""; OFS="|"} {print $1,$2,$3,$4,$5,$6,$7}' test.txt
# Output
# a|,|1|,|2|,|3
# b|,|2|,|4|,|6
# c|,|3|,|6|,|9
awk 'BEGIN {FS=" "; OFS="|"} {print $1,$2,$3}' test.txt
# Output
# a,1,2,3||
# b,2,4,6||
# c,3,6,9||
```

5. BEGIN and END Blocks

* BEGIN runs before reading lines.
* END runs after all lines processed.

```bash
cat test.txt
# Output
# a,1,2,3
# b,2,4,6
# c,3,6,9
awk 'BEGIN {count=0} {count++} END {print "Total:", count}' test.txt
# Output
# Total: 3
```

1. Arithmetic
You can do math on fields:
```bash
awk '{sum = $2 + $3; print $1, sum}' test.txt
# Output -> Notice without BEGIN and FS, even commas are part of $1 - field 1
# a,1,2,3 0
# b,2,4,6 0
# c,3,6,9 0

awk 'BEGIN {FS=","} {sum = $2 + $3; print $1, sum}' test.txt
# Output
# a 3
# b 6
# c 9

awk 'BEGIN {FS=","; OFS="|"} {sum = $2 + $3; print $1, sum}' test.txt
# Output
# a|3
# b|6
# c|9
```