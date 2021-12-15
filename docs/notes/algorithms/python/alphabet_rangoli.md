# Alphabet Rangoli

An alphabet diamond design in a square

You are given an integer, N. Your task is to print an alphabet rangoli of size N. (Rangoli is a form of Indian folk art based on creation of patterns.)

Different sizes of alphabet rangoli are shown below:
```
#size 3 -> 9

----c----
--c-b-c--
c-b-a-b-c
--c-b-c--
----c----

#size 5 -> 17

--------e--------
------e-d-e------
----e-d-c-d-e----
--e-d-c-b-c-d-e--
e-d-c-b-a-b-c-d-e
--e-d-c-b-c-d-e--
----e-d-c-d-e----
------e-d-e------
--------e--------

#size 10 -> 37

------------------j------------------
----------------j-i-j----------------
--------------j-i-h-i-j--------------
------------j-i-h-g-h-i-j------------
----------j-i-h-g-f-g-h-i-j----------
--------j-i-h-g-f-e-f-g-h-i-j--------
------j-i-h-g-f-e-d-e-f-g-h-i-j------
----j-i-h-g-f-e-d-c-d-e-f-g-h-i-j----
--j-i-h-g-f-e-d-c-b-c-d-e-f-g-h-i-j--
j-i-h-g-f-e-d-c-b-a-b-c-d-e-f-g-h-i-j
--j-i-h-g-f-e-d-c-b-c-d-e-f-g-h-i-j--
----j-i-h-g-f-e-d-c-d-e-f-g-h-i-j----
------j-i-h-g-f-e-d-e-f-g-h-i-j------
--------j-i-h-g-f-e-f-g-h-i-j--------
----------j-i-h-g-f-g-h-i-j----------
------------j-i-h-g-h-i-j------------
--------------j-i-h-i-j--------------
----------------j-i-j----------------
------------------j------------------
```

The center of the rangoli has the first alphabet letter a, and the boundary has the Nth alphabet letter (in alphabetical order).

Input Format Only one line of input containing N, the size of the rangoli. Constraints 0< N< 27 Output Format Print the alphabet rangoli in the format explained above. Sample Input 5 Sample Output

```
--------e--------
------e-d-e------
----e-d-c-d-e----
--e-d-c-b-c-d-e--
e-d-c-b-a-b-c-d-e
--e-d-c-b-c-d-e--
----e-d-c-d-e----
------e-d-e------
--------e--------
```

The code:
```python
import string
def print_rangoli(size):
    # your code goes here
    N = size
    width = 4*(N-1)+1
    letters=string.ascii_letters[0:26]
    # triangle up
    for i in range(N-1):
        s = '{}'.format(letters[N-i-1])
        for n in range(i):
            s = '{}-'.format(letters[N-i+n]) + s + '-{}'.format(letters[N-i+n])
        s = s.center(width, '-')
        print(s); del(s)

    # center
    s = '{}'.format(letters[0])
    for n in range(N-1):
        s = '{}-'.format(letters[n+1]) + s + '-{}'.format(letters[n+1])
    print(s); del(s)

    # triangle down
    for i in range(N-2,-1,-1):
        s = '{}'.format(letters[N-i-1])
        for n in range(i):
            s = '{}-'.format(letters[N-i+n]) + s + '-{}'.format(letters[N-i+n])
        s = s.center(width, '-')
        print(s); del(s)


if __name__ == '__main__':
    n = int(input())
    print_rangoli(n)
```
