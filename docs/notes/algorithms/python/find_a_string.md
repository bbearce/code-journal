# Find a string

**Find all occurrences of a substring in a string**

In this challenge, the user enters a string and a substring. You have to print the number of times that the substring occurs in the given string. String traversal will take place from left to right, not from right to left.

> NOTE: String letters are case-sensitive.

**Input Format:**  
The first line of input contains the original string. The next line contains the substring.

Constraints:  
* 1 <= len(string) <= 200  
* Each character in the string is an ascii character.  

**Output Format:**  
Output the integer number indicating the total number of occurrences of the substring in the original string.

Sample Input:
```bash
ABCDCDC
CDC
```

Sample Output:  
```2```

Concept

```python
def count_substring(string, sub_string):
    index = 0
    counts = 0
    while index < len(string):
        index = string.find(sub_string, index)
        if index == -1:
            break
        counts += 1
        index += 1 
    return counts


if __name__ == '__main__':
    string = input().strip()
    sub_string = input().strip()
    
    count = count_substring(string, sub_string)
    print(count)
```
