# Timsort

> Source: [geeksforgeeks](https://www.geeksforgeeks.org/timsort/)

TimSort is a sorting algorithm based on Insertion Sort and Merge Sort.

We divide the Array into blocks known as Run. We sort those runs using insertion sort one by one and then merge those runs using the combine function used in merge sort. If the size of the Array is less than run, then Array gets sorted just by using Insertion Sort. The size of the run may vary from 32 to 64 depending upon the size of the array. Note that the merge function performs well when size subarrays are powers of 2. The idea is based on the fact that insertion sort performs well for small arrays.



```python
# Python3 program to perform basic timSort
MIN_MERGE = 8

def calcMinRun(n):
    # print(f"n: {n}")
    # array_len = n
    """Returns the minimum length of a
    run from 23 - 64 so that
    the len(array)/minrun is less than or
    equal to a power of 2.
    e.g. 1=>1, ..., 63=>63, 64=>32, 65=>33,
    ..., 127=>64, 128=>32, ...
    """
    r = 0
    # print(f"In calcMinRun: r:{r}")
    while n >= MIN_MERGE:
        # print(f"r: {r} n:{n}")
        # pdb.set_trace()
        r |= n & 1 # r = r | (n & 1)
        n >>= 1
        # print(f"r: {r} n:{n}")
    # print(f"minRunn: {n + r} and len(array)/minrun = {array_len/(n+r)}")
    return n + r


# This function sorts array from left index to
# to right index which is of size atmost RUN
def insertionSort(arr, left, right):
    print(f"arr: {arr} left: {left} right: {right}")
    for i in range(left + 1, right + 1):
        j = i
        while j > left and arr[j] < arr[j - 1]:
            arr[j], arr[j - 1] = arr[j - 1], arr[j]
            j -= 1
            print(f"arr: {arr}")

# Merge function merges the sorted runs
def merge(arr, l, m, r):
    # original array is broken in two parts
    # left and right array
    print("In merge()")
    print(f"arr: {arr} l: {l} m: {m} r: {r} ")
    len1, len2 = m - l + 1, r - m
    print(f"len1: {len1} len2: {len2}")
    left, right = [], []
    print(f"left: {left} right: {right}")
    for i in range(0, len1):
        left.append(arr[l + i])
        print(f"left: {left} right: {right}")
    for i in range(0, len2):
        right.append(arr[m + 1 + i])
        print(f"left: {left} right: {right}")
    i, j, k = 0, 0, l
    # after comparing, we merge those two array
    # in larger sub array
    while i < len1 and j < len2:
        if left[i] <= right[j]:
            arr[k] = left[i]
            i += 1
            print(f"arr: {arr} left: {left} right: {right}")
        else:
            arr[k] = right[j]
            j += 1
            print(f"arr: {arr} left: {left} right: {right}")
        k += 1
    print("# Copy remaining elements of left, if any)")
    # Copy remaining elements of left, if any
    while i < len1:
        arr[k] = left[i]
        k += 1
        i += 1
        print(f"arr: {arr} left: {left} right: {right}")
    # Copy remaining element of right, if any
    while j < len2:
        arr[k] = right[j]
        k += 1
        j += 1
        print(f"arr: {arr} left: {left} right: {right}")

# Iterative Timsort function to sort the
# array[0...n-1] (similar to merge sort)
def timSort(arr):
    n = len(arr)
    print(f"n: {n}; calling calcMinRun(n)")
    minRun = calcMinRun(n)
    print(f"midRun: {minRun}")
    # Sort individual subarrays of size RUN
    for start in range(0, n, minRun):
        print(f"start: {start} of minRun: {minRun}")
        end = min(start + minRun - 1, n - 1)
        print(f"calling insertionSort({arr}, {start}, {end})")
        insertionSort(arr, start, end)
    # Start merging from size RUN (or 32). It will merge
    # to form size 64, then 128, 256 and so on ....
    size = minRun
    print(f"size: {size}")
    while size < n:
        # Pick starting point of left sub array. We
        # are going to merge arr[left..left+size-1]
        # and arr[left+size, left+2*size-1]
        # After every merge, we increase left by 2*size
        for left in range(0, n, 2 * size):
            # Find ending point of left sub array
            # mid+1 is starting point of right sub array
            mid = min(n - 1, left + size - 1)
            right = min((left + 2 * size - 1), (n - 1))
            # Merge sub array arr[left.....mid] &
            # arr[mid+1....right]
            print(f"mid: {mid} left: {left} right: {right}")
            if mid < right:
                print(f"calling merge({arr}, {left}, {mid}, {right})")
                merge(arr, left, mid, right)
        size = 2 * size

 
# Driver program to test above function
# if __name__ == "__main__":

arr = [-2, 7, 15, -14, 0, 15, 0,
        7, -7, -4, -13, 5, 8, -14, 12]

print("Given Array is")
print(arr)

# Function Call
timSort(arr)


print("After Sorting Array is")
print(arr)
# [-14, -14, -13, -7, -4, -2, 0, 0,
#         5, 7, 7, 8, 12, 15, 15]

```