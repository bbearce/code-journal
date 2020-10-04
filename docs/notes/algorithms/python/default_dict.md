# Default Dict

**Print the indices of each occurrence of m in group A.**

In this challenge, you will be given integers, n and m. There are m words, which might repeat, in word group A. There are m words belonging to word group B. For each m words, check whether the word has appeared in group A or not. Print the indices of each occurrence of m in group A.If it does not appear, print -1.

Sample Input:
```
5 2
a
a
b
a
b
a
b
```

Sample Output:
```
1 2 4
3 5
```

Code: 
```python
from collections import defaultdict

if __name__ == '__main__':
    n, m = map(int,input().split())
    A = []; A = [input() for i in range(n)]
    B = []; B = [input() for i in range(m)]
    B_dict = defaultdict(list)
    for wordB in B:
        for A_indice in range(len(A)):
            if wordB == A[A_indice]:
                B_dict[wordB].append(A_indice+1)
        if B_dict[wordB] == []:
                B_dict[wordB].append(-1)

    for word in B_dict:
        print(" ".join([str(i) for i in B_dict[word]]))
```