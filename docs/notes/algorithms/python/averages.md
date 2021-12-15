# Averages

**Find the average of a student given a list of students**

You have a record of N students. Each record contains the student's name, and their percent marks in Maths, Physics and Chemistry. The marks can be floating values. The user enters some integer N followed by the names and marks for N students. You are required to save the record in a dictionary data type. The user then enters a student's name. Output the average percentage marks obtained by that student, correct to two decimal places.

Input Format:

The first line contains the integer N, the number of students. The next N lines contains the name and marks obtained by that student separated by a space. The final line contains the name of a particular student previously listed.

Constraints:

* 2<=N<=10  
* 0<=Marks<=100  
Output Format:
Print one line: The average of the marks obtained by the particular student correct to 2 decimal places.

Sample Input

```
3
Krishna 67 68 69
Arjun 70 98 63
Malika 52 56 60
Malika
```

Sample Output

```56.00```

Implementation:

```python
if __name__ == '__main__':
    # By accepting input as an int we can loop to grab all other inputs
    n = int(input())
    # Initialize marks dict
    student_marks = {}
    for _ in range(n):
        # Nice trick to grab first value as name and the rest in a list called line
        name, *line = input().split()
        scores = list(map(float, line))
        # Fill the student marks dict
        student_marks[name] = scores
    # Whose score do we want?
    query_name = input()
    selected_avg = round(sum(student_marks[query_name])/3,2)
    print("{0:.2f}".format(selected_avg))

```
