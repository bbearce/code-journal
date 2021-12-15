# Collections Counter

**Your task is to compute how much money Raghu earned.**

Raghu is a shoe shop owner. His shop has X number of shoes.He has a list containing the size of each shoe he has in his shop. There are N number of customers who are willing to pay x_i amount of money only if they get the shoe of their desired size.

Your task is to compute how much money Raghu earned.

**Input Format** 

The first line contains X, the number of shoes. The second line contains the space separated list of all the shoe sizes in the shop. The third line contains N, the number of customers. The next N lines contain the space separated values of the shoe_size desired by the customer and x_i, the price of the shoe.

Enter your code here. Read input from STDIN. Print output to STDOUT

**Sample Input:**
```
10 --> Number of Shoes
2 3 4 5 6 8 7 6 5 18 --> all shoe sizes
6 -- > Number of Customers
6 55
6 45
6 55
4 40
18 60
10 50
```

**S**ample Output:
```200```

**Explanation**

Customer 1: Purchased size 6 shoe for $55.  
Customer 2: Purchased size 6 shoe for $45.  
Customer 3: Size 6 no longer available, so no purchase.  
Customer 4: Purchased size 4 shoe for $40.  
Customer 5: Purchased size 18 shoe for $60.  
Customer 6: Size 10 not available, so no purchase.  

Total money earned = $200

```python
from collections import Counter

if __name__ == "__main__":
    num_of_shoes = int(input())
    all_shoe_sizes = Counter(map(int, input().split()))
    num_customers = int(input())
    total_earned = 0
    for n in range(num_customers):
        size, price = map(int, input().split())
        if all_shoe_sizes[size] != 0:
            total_earned = total_earned + price
            all_shoe_sizes[size] -= 1
        
    print(total_earned)
```