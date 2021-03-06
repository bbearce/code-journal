# Screen

> Courtesy of [linuxize.com](https://linuxize.com/post/how-to-use-linux-screen/) and this [video](https://www.youtube.com/watch?v=I4xVn6Io5Nw) by **Linux Leech**.

## Basics: 

```C-a``` means ```Control+a``` and seems to be the basis of most commands.  

```C-a ?``` Means to press ```control+a``` and then the ```?``` for help about other command for screen. 
   

Start a new screen with the word ```screen```. 


Name a screen session with ```screen –S secondscreen``` 

### Rename Screen:

To list running screen sessions, use ```screen -ls``` 

```$screen -ls ```
 
```
There is a screen on: 
        12129.testsession       (Detached) 
1 Socket in /var/run/screen/S-root. 
```

Yeah, here we go!! Renaming the screen session name ```testsession``` to something else. Here is the command to rename the existing session. 

> Note: ```sessionname``` as used below is a command so it is always necessary 
 
```
$ screen -S 12129.testsession -X sessionname newname 
```

```C-a d``` is to detach. 

Once you detach you can see all screens with ```screen –ls```

Now connect to a screen... 

```
bbearce@bbearce-XPS-15-9560:~$ screen -ls 

There are screens on: 

    530.new_screen    (05/28/2019 03:27:17 PM)    (Detached) 

    370.pts-4.bbearce-XPS-15-9560    (05/28/2019 03:24:18 PM)    (Detached) 

2 Sockets in /var/run/screen/S-bbearce. 
```

Connect to either with the name of the screen or the PID (prefacing numbers {530, 370}) 

 

### To get rid of a screen: 

```
bbearce@bbearce-XPS-15-9560:~$ screen -X -S 370 quit 
```
The ```–X``` is for sending a command to a screen and ```–S``` is to identify the name of the screen to send the command. The command is ```quit```. 

Now use ```screen –ls```: 

```
bbearce@bbearce-XPS-15-9560:~$ screen -ls 

There is a screen on: 

    530.new_screen    (05/28/2019 03:27:17 PM)    (Detached) 

1 Socket in /var/run/screen/S-bbearce. 
```

The other way to kill a screen is from within it.

> Keep in mind this is technically for windows and not screens, but will kill a screen if there is only 1 window

```C-a k```

This will prompt you for whether or not you are sure. ```(y/n)```

Now ```screen –ls```: 

```
bbearce@bbearce-XPS-15-9560:~$ screen -ls 

No Sockets found in /var/run/screen/S-bbearce. 
```

## Windows: 

Once in a screen use ```C-a c``` to create a new window. 

```C-a n```  is for *next*  
```C-a p```  is for *previous*  
```C-a w```  is for *listing windows*  
```C-a "``` is for *showing a menu of windows*  

Don't forget ```C-a k``` will kill a window and eventually the screen if there is only one window. 
 

If you make 4 screens and echo 0-3 in them, we can jump to each with these commands: 

```C-a 0``` will jump to the first window with ```echo 0``` in in it  
```C-a 1``` will jump to the first window with ```echo 1``` in in it  
```C-a 2``` will jump to the first window with ```echo 2``` in in it  
```C-a 3``` will jump to the first window with ```echo 3``` in in it  
 

```C-a "``` will show them all and notice they are all named bash. We can rename them to be more useful. 

Ex: 

```
Num Name 

 

  0 bash 
  1 bash 
  2 bash 
  3 bash 

```

If you press ```C-a A``` we can rename our windows. Notice what happens during ```C-a "``` now after renaming: 


Ex: 
```
Num Name 

 

  0 bash 
  1 bash 
  2 window-2 
  3 bash 

```

### Panes: 
 
```C-a |``` will split the window *vertically*  
```C-a S``` will split the window *horizontally*  
```C-a tab``` to *change panes*  
```C-a X``` to *exit panes*  
```C-a x``` to *lock the terminal\screen* - **you will need a password to get back in.**  
```C-a t``` to to get the *time and load* on the system  
 

> Tab over to a new pane that is empty and open a window with the general window commands. 

```C-a X``` will close a pane as well as performing ```C-a :``` which will bring up a prompt starting with ```:```. At the prompt type ```remove``` and press enter. This removes the pane as well. 

## Run Commands with Screen: 

Use VI to make counter.py file as such: 
```
$ vi counter.py 
```
...write the code below 

 
```python
import time  

for i in range(5): 

    print(time.ctime(time.time())) 

    time.sleep(1) 

```

Now we can run this program in a screen but will kill the screen when complete 

```
$ screen -d -m counter.py 
```

You can see the screen momentarily before it quits by running ```screen –r```. Also we can run this in a screen and not have it automatically quit by connecting first.  


## Problems

[There is no screen to be resumed matching <screen-name\>](https://unix.stackexchange.com/questions/187001/there-are-screens-in-the-list-but-no-screen-to-be-resumed)


```
azureuser@cbibop3:~$ screen -r codalab
There is a screen on:
    8967.codalab    (10/18/2019 06:56:52 PM)    (Attached)
There is no screen to be resumed matching codalab.
```

As ```screen -r``` says, there is one screen, but it is attached. To resume it on your current terminal, you have to detach it from the other one first: ```screen -d -r 8967```, see manpage ```-d```.

Edit: use ```-d``` instead of ```-x```.

Edit2: @alex78191: When using ```-x```, screen attaches to the currently running session, resulting in a "multi-display mode": you see the session on both terminals simultaneously, i.e., when entering a command on one terminal, it also appears on the second. However, detaching from a multi-display mode just detaches the current terminal. You hence get the message that it is still attached (on the other terminal).

## Resize Region

[gnu](http://www.gnu.org/software/screen/manual/screen.html#Resize)

> Find section "resize" under "9.5 - Regions"

The amount of lines to add or remove can be expressed a couple of different ways. By specifying a number ```n``` by itself will resize the region by that absolute amount. You can specify a relative amount by prefixing a plus ‘+’ or minus ‘-’ to the amount, such as adding +n lines or removing -n lines. Resizing can also be expressed as an absolute or relative percentage by postfixing a percent sign ‘%’. Using zero ‘0’ is a synonym for min and using an underscore ‘\_’ is a synonym for max.

Some examples are:
```bash
resize +N       increase current region by N
resize -N       decrease current region by N
resize N        set current region to N
resize 20%      set current region to 20% of original size
resize +20%     increase current region by 20%
resize -b =     make all windows equally
resize max      maximize current region
resize min      minimize current region
```

Without any arguments, screen will prompt for how you would like to resize the current region.
