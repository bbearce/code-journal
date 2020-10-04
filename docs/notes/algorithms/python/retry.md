# retry()

**Retry a GET request a maximum of 5 times.**

Let's assume you want to get some response from a get command. For all intensive purposes we will assume this is all happening inside a function called ```get_response(choice)```

Next youy want to make a function ```try_again(response='pass')``` that will keep trying to execute ```get_response(choice)``` if we get a '401' response, which means that the client could not be authenticated. The catch is that we want the maximum retries to be 5 before accepting the failed authentication. Furthermore each time the retry function is called the function should sleep for n^2 seconds. It shouldn't wait anytime to execute the first ```get_response(choice)```, which is the one initially passed to ```try_again()```. Finally if we get '404' then return a '404 Not Found Error'.

We start by defining get_response()

```
def get_response(choice=3):
    choices = {1:'404',2:'pass',3:'401'}
    return choices[choice]
```

Now we need try_again(). I have to give credit to [Zagaran](https://zagaran.com/) which you can find a link to on the Sources tab for this algorithm. During an interview I attempted a recursive solution, but they said a for loop would of done. So for extra practice I implemented both.

```python
def try_again_recursive(response, try_count=1):
    """
    This function will take a response object
    and do the following:
    
    - If we get 'pass', return 'we connected!'
    - If we get '404', return '404 Not Found Error'
    - If we get '401', try again, however each time
      you try again wait the square of the try 
      seconds before trying again. Try a maximum of 5 
      times before returning 'client could not be authenticated.'

    Example:
        try1: wait 1 seconds to retry
        try2: wait 4 seconds to retry
        try3: wait 9 seconds to retry
        try4: wait 16 seconds to retry
        try5: wait 0 seconds to retry! Since the max tries is 5,
              there is no need to wait the additional 25 seconds
              before exiting.
    """

    if response == '404':
        return '404 Not Found Error'
    elif response == '401':
        if try_count == 5:
            print('for n = {0}, waiting {1} seconds.'.format(try_count, 0))
            print('client could not be authenticated.')
        else:
            print('for n = {0}, waiting {1} seconds.'.format(try_count, try_count**2))
            time.sleep(try_count**2)
            try_again_recursive(get_response(), try_count=try_count+1)
    else:
        return 'we connected!'

    return response
```

Check out the output below

```python
# Tests
# >>>try_again_recursive(get_response())
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# for n = 3, waiting 9 seconds.
# for n = 4, waiting 16 seconds.
# for n = 5, waiting 0 seconds.
# client could not be authenticated.
# '401'
```

One thing to note, I cheated a little. For the sake of practicing the multiple calls, I made sure the get_response() function always returned a '401'. We will address this a little later. For now let's implement the for loop version.

```python
# For Loop Solution:
def try_again_for_loop(response):
    """
    This function will take a response object
    and do the following:
    
    - If we get 'pass', return 'we connected!'
    - If we get '404', return '404 Not Found Error'
    - If we get '404', try again, however each time
      you try again wait the square of the try 
      seconds before trying again. Try a maximum of 5 
      times before returning 'client could not be authenticated.'

    Example:
        try1: wait 1 seconds to retry
        try2: wait 4 seconds to retry
        try3: wait 9 seconds to retry
        try4: wait 16 seconds to retry
        try5: wait 0 seconds to retry! Since the max tries is 5,
              there is no need to wait the additional 25 seconds
              before exiting.
    """
    for i in range(1,6): # [1,2,3,4,5]

        if response == '404':
            return '404 Not Found Error'
        elif response == '401':
            if i == 5:
                print('for n = {0}, waiting {1} seconds.'.format(i, 0))
                print('client could not be authenticated.')
            else:
                # pdb.set_trace()
                print('for n = {0}, waiting {1} seconds.'.format(i, i**2))
                time.sleep(i**2)
                response = get_response()
        else:
            return 'we connected!'

    return response
```

Output:

```python
# >>> try_again_for_loop(get_response())
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# for n = 3, waiting 9 seconds.
# for n = 4, waiting 16 seconds.
# for n = 5, waiting 0 seconds.
# client could not be authenticated.
# '401'
```

Now we need to simulate reality a little better to give the other logic a chance to be executed. Let's make it so get_response() could return any of the other values ('pass','404').

Go into the recursive version of ```try_again()``` and find this line ```try_again_recursive(get_response(), try_count=try_count+1)```, and change it to ```try_again_recursive(get_response(random.randint(1,3)), try_count=try_count+1)```. You will need to ```import random``` at the top of your script. Then also add to your call of ```try_again_recursive()```

Results:

```python
# Simulation
# >>> try_again_recursive(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# '401'
# >>> try_again_recursive(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# '401'
# >>> try_again_recursive(get_response(random.randint(1,3)))
# 'we connected!'
# >>> try_again_recursive(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# '401'
# >>> try_again_recursive(get_response(random.randint(1,3)))
# '404 Not Found Error'
# >>> try_again_recursive(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# for n = 3, waiting 9 seconds.
# for n = 4, waiting 16 seconds.
# for n = 5, waiting 0 seconds.
# client could not be authenticated.
# '401'
```

Again, the for loop version changes the same thing, ie. ```response = get_response(``` to this ```response = get_response(random.rendint(1,3))```.

```python
# >>> try_again_for_loop(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# '404 Not Found Error'
# >>> try_again_for_loop(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# for n = 3, waiting 9 seconds.
# 'we connected!'
# >>> try_again_for_loop(get_response(random.randint(1,3)))
# for n = 1, waiting 1 seconds.
# for n = 2, waiting 4 seconds.
# for n = 3, waiting 9 seconds.
# 'we connected!'
# >>> try_again_for_loop(get_response(random.randint(1,3)))
# '404 Not Found Error'
```