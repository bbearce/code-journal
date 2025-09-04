# Find

```bash
find [path] [options] [expression]
```

1. Find file by name
```bash
cat test.txt
# Output
# a,1,2,3
# b,2,4,6
# c,3,6,9
cat Test2.txt
# Output
# d,4,8,12
# e,5,10,15
# f,6,12,18

find . -name "test.txt"
# Output
# ./test.txt

find . -name "Test2.txt"
# Output
# ./Test2.txt
```

Case-insensitive:
```bash
find . -name "test2.txt"
# Output

find . -iname "test2.txt"
# Output
# ./Test2.txt

find . -iname "*.txt"
# Output
# ./right.txt
# ./test.txt
# ./left.txt
# ./Test2.txt
# ./ACTI-QUANT/app_scoring/input/res/submission.txt

find . -name "*.txt"
# Output
# ./right.txt
# ./test.txt
# ./left.txt
# ./Test2.txt
# ./ACTI-QUANT/app_scoring/input/res/submission.txt
```

2. Find file by type

Regular files:
```bash
# Recursive so be careful
find . -type f
```


Directories:
```bash
# Recursive so be careful
find . -type d
```

Symbolic links:
```bash
# Recursive so be careful
find . -type l
```

3. Find by size

```bash
find . -size +100M    # Larger than 100 MB
find . -size -10k     # Smaller than 10 KB
```

4. Find by modification time

```bash
find . -mtime -1   # Modified in the last 24 hours
find . -mtime +7   # Modified more than 7 days ago
find . -mmin -30   # Modified in last 30 minutes
```

5. Find and execute a command

> {} = matched file, \; ends the command.
```bash
find . -name "test.txt" -exec cat {} \;
# Output
# a,1,2,3
# b,2,4,6
# c,3,6,9

find . -name "Test2.txt" -exec cat {} \;
# Output
# d,4,8,12
# e,5,10,15
# f,6,12,18
```

!!!!!!!!!!
You can run multiple commands using + for efficiency:
```bash
find . -name "*.txt" -exec ls {}
find . -name "*.txt" -exec ls -l {} +
find . -name "*.txt" -exec ls -lh {} +
```
!!!!!!!!!!

6. Find and print details

```bash
find . -name "*.txt" -printf "%p %k KB\n"
# Output
# ./right.txt 4 KB
# ./test.txt 4 KB
# ./left.txt 4 KB
# ./Test2.txt 4 KB
# ./ACTI-QUANT/app_scoring/input/res/submission.txt 4 KB
```

7. Combine conditions
```bash
find . -type f -name "*.txt" -size +1M
# Output

find . -type f -name "*.txt" -size +1k
# Output
# ./right.txt
# ./left.txt

find . -type f -name "*.txt" -size +10k
# Output

```
> Finds .txt files larger than 1 MB.

8. Exclude directories

TL;DR
```bash
find . -path "./node_modules" -prune -o -type f -name "*.js" -print
```
> Finds .js files but ignores node_modules.

...but like what and we are pruning (`-prune`) and there is an `-o` now...
```bash
find . -type f -name "*.txt"
# Output
# ./right.txt
# ./test.txt
# ./left.txt
# ./Test2.txt
# ./ACTI-QUANT/app_scoring/input/res/submission.txt

find . -path "./ACTI-QUANT" -type f -name "*.txt"
# Output

```
...no output?

> So -path is looking for files/folders with ./ACTI-QUANT exactly in the path but what you don't see is an automatic "AND" with the other flags to the right. Here path has to match exactly! So there are no directories that match ./ACTI-QUANT "AND" are files that have a "*.txt" pattern.

Let's explore:
```bash
find . -path "./ACTI-QUANT/*.txt" 
# Output
# ./ACTI-QUANT/app_scoring/input/res/submission.txt


find . -path "./ACTI-QUANT/*.txt" -type f -name "*.txt"
# Output
# ./ACTI-QUANT/app_scoring/input/res/submission.txt
```

`-o` - adds an "OR" with the other commands:
```bash
find . -path "./ACTI-QUANT" -o -type f -name "*.txt"
# Output
# ./right.txt
# ./test.txt
# ./left.txt
# ./Test2.txt
# ./ACTI-QUANT
# ./ACTI-QUANT/app_scoring/input/res/submission.txt
```
> So directory ./ACTI-QUANT "OR" files that end in ".txt"

Remember without `-type` directories are found too.
```bash
find . -path "./ACTI-QUANT"
# Output
# ./ACTI-QUANT
```

`-prune` - says, do not descent into this directory.

> We don't have to descent to match `-path` directories
```bash
find . -path "./ACTI-QUANT" -prune 
# Output
# ./ACTI-QUANT

find . -path "./ACTI-QUANT" -o -type f -name "*.txt"
# ./right.txt -> matched with ` -o -type f -name "*.txt"`
# ./test.txt -> matched with ` -o -type f -name "*.txt"`
# ./left.txt -> matched with ` -o -type f -name "*.txt"`
# ./Test2.txt -> matched with ` -o -type f -name "*.txt"`
# ./ACTI-QUANT -> matched with `find . -path "./ACTI-QUANT"`
# ./ACTI-QUANT/app_scoring/input/res/submission.txt -> matched with ` -o -type f -name "*.txt"`

find . -path "./ACTI-QUANT" -prune -o -type f -name "*.txt"
# ./right.txt -> matched with ` -o -type f -name "*.txt"`
# ./test.txt -> matched with ` -o -type f -name "*.txt"`
# ./left.txt -> matched with ` -o -type f -name "*.txt"`
# ./Test2.txt -> matched with ` -o -type f -name "*.txt"`
# ./ACTI-QUANT -> matched with `find . -path "./ACTI-QUANT" -prune`

# Same as:
find . -path "./ACTI-QUANT" -a -prune -o -type f -name "*.txt"
# ./right.txt -> matched with ` -o -type f -name "*.txt"`
# ./test.txt -> matched with ` -o -type f -name "*.txt"`
# ./left.txt -> matched with ` -o -type f -name "*.txt"`
# ./Test2.txt -> matched with ` -o -type f -name "*.txt"`
# ./ACTI-QUANT -> matched with `find . -path "./ACTI-QUANT" -a -prune`
```

> So carefully looking at the last example, ./ACTI-QUANT is pruned for deeper consideration but the directory is found. Find executes left to right so since the ./ACTI-QUANT is found and an implicit "AND" is operated between `-path` and `-prune`, those both together evaluate to "TRUE". Since that part is true, the `-o` stuff that follows is not considered when the directory is ./ACTI-QUANT, but if the files are not in that directory (`-path` not satisfied), then `find . -path "./ACTI-QUANT" -prune` is not "TRUE" and we can execute the stuff after `-o`.

Another way to visualize:
```bash
( -path "./ACTI-QUANT" -a -prune ) -o ( -type f -a -name '*.txt' -a -print )

```

> If you don’t give any action (-print, -exec, -delete, etc.), the default action is -print.

9. Find files owned by a user

```bash
find . -user bearceb
```