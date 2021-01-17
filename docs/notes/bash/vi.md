# VI


## Find and Replace

>Source [linfo](http://www.linfo.org/vi/search.html)


>TL;DR

Single Line scope (single replacement):
```bash
:s/search_for_text/replace_text/
```
Single Line scope (global replacement):
```bash
:s/search_for_text/replace_text/g
```
Whole file scope:
```bash
:s%/search_for_text/replace_text/<g or no g>
```
## Formal Notes:
vi also has powerful search and replace capabilities. To search the text of an open file for a specific string (combination of characters or words), in the command mode type a colon (:), "s," forward slash (/) and the search string itself. What you type will appear on the bottom line of the display screen. Finally, press ENTER, and the matching area of the text will be highlighted, if it exists. If the matching string is on an area of text that is not currently displayed on the screen, the text will scroll to show that area.

The formal syntax for searching is:

```bash
:s/string
```

For example, suppose you want to search some text for the string "cherry." Type the following and press ENTER:

```bash
:s/cherry
```

The first match for "cherry" in your text will then be highlighted. To see if there are additional occurrences of the same string in the text, type n, and the highlight will switch to the next match, if one exists.

The syntax for replacing one string with another string in the current line is

```bash
:s/pattern/replace/
```

Here "pattern" represents the old string and "replace" represents the new string. For example, to replace each occurrence of the word "lemon" in a line with "orange," type:

```bash
:s/lemon/orange/
```

The syntax for replacing every occurrence of a string in the entire text is similar. The only difference is the addition of a "%" in front of the "s":

```bash
:%s/pattern/replace/
```

Thus repeating the previous example for the entire text instead of just for a single line would be:

```bash
:%s/lemon/orange/
```




Next: Working With Multiple Files


