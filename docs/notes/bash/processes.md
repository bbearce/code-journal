# Processes


## Kill a process (TL;DR)

```$ kill <pid>```

Ex: ```$ kill 1234```

## By Port
> Source [mr-khan.gitlab.io](https://mr-khan.gitlab.io/linux/2018/05/02/kill-specific-port-on-linux.html)

Sometimes I fed up with searching my program PID. As you know the port number so you can easily find the port PID and kill it. If you want to kill a process running on port number 8000 then first you need to find the PID and then kill it. Run the following command to find port number PID:

```bash
sudo lsof -t -i:8000
```

then kill:

```bash
sudo kill $(sudo lsof -t -i:8000)
```

> Courtesy of [booleanworld.com](booleanworld.com/kill-process-linux/#targetText=It%20is%20very%20easy%20to,that%20you%20want%20to%20kill.)

...more...below:

Use ```top``` to see running processes.

```
azureuser@miccai:~$ top

top - 19:14:36 up 28 min,  1 user,  load average: 0.00, 0.02, 0.05
Tasks: 115 total,   1 running, 114 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   3514568 total,   715812 used,  2798756 free,    32776 buffers
KiB Swap:        0 total,        0 used,        0 free.   336792 cached Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                                                     
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd                                                                                                                                    
     3 root      20   0       0      0      0 S   0.0  0.0   0:00.10 ksoftirqd/0                                                                                                                                 
     5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H                                                                                                                                
     6 root      20   0       0      0      0 S   0.0  0.0   0:00.19 kworker/u256:0                                                                                                                              
     7 root      20   0       0      0      0 S   0.0  0.0   0:00.90 rcu_sched                                                                                                                                   
     8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh                                                                                                                                      
     9 root      20   0       0      0      0 S   0.0  0.0   0:00.99 rcuos/0                                                                                                                                     
    10 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcuob/0                                                                                                                                     
    11 root      rt   0       0      0      0 S   0.0  0.0   0:00.07 migration/0                                                                                                                                 
    12 root      rt   0       0      0      0 S   0.0  0.0   0:00.02 watchdog/0                                                                                                                                  
    13 root      rt   0       0      0      0 S   0.0  0.0   0:00.02 watchdog/1                                                                                                                                  
    14 root      rt   0       0      0      0 S   0.0  0.0   0:00.03 migration/1                                                                                                                                 
    15 root      20   0       0      0      0 S   0.0  0.0   0:00.11 ksoftirqd/1                                                                                                                                 
    17 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/1:0H                                                                                                                                
    18 root      20   0       0      0      0 S   0.0  0.0   0:00.42 rcuos/1                                                                                                                                     
    19 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcuob/1                                                                                                                                     
    20 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 khelper                                                                                                                                     
    21 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kdevtmpfs                                                                                                                                   
    22 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 netns                                                                                                                                       
    23 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 perf                                                                                                                                        
    24 root      20   0       0      0      0 S   0.0  0.0   0:00.00 khungtaskd                                                                                                                                  
    25 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 writeback                                                                                                                                   
    26 root      25   5       0      0      0 S   0.0  0.0   0:00.00 ksmd                                                                                                                                        
    27 root      39  19       0      0      0 S   0.0  0.0   0:00.03 khugepaged                                                                                                                                  
    28 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 crypto                                                                                                                                      
    29 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kintegrityd                                                                                                                                 
    30 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset                                                                                                                                      
    31 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kblockd                                
    ...
    ...
    ...
```

The other option is ```$ ps aux```. This will give the command used to run the process (this can sometimes be more helpful).

```
azureuser@miccai:~$ ps aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.2  0.1  35080  5100 ?        Ss   18:46   0:04 /sbin/init
root          2  0.0  0.0      0     0 ?        S    18:46   0:00 [kthreadd]
root          3  0.0  0.0      0     0 ?        S    18:46   0:00 [ksoftirqd/0]
root          5  0.0  0.0      0     0 ?        S<   18:46   0:00 [kworker/0:0H]
root          6  0.0  0.0      0     0 ?        S    18:46   0:00 [kworker/u256:0]
root          7  0.0  0.0      0     0 ?        S    18:46   0:00 [rcu_sched]
root          8  0.0  0.0      0     0 ?        S    18:46   0:00 [rcu_bh]
root          9  0.0  0.0      0     0 ?        S    18:46   0:01 [rcuos/0]
root         10  0.0  0.0      0     0 ?        S    18:46   0:00 [rcuob/0]
root         11  0.0  0.0      0     0 ?        S    18:46   0:00 [migration/0]
root         12  0.0  0.0      0     0 ?        S    18:46   0:00 [watchdog/0]
root         13  0.0  0.0      0     0 ?        S    18:46   0:00 [watchdog/1]
root         14  0.0  0.0      0     0 ?        S    18:46   0:00 [migration/1]
...
...
...
```

The advantage of using ps is that you can easily filter this list with the grep command. For example, to find a process associated with the term "SCREEN", you can use:

```
azureuser@miccai:~$ ps aux | grep -i SCREEN
azureus+   1813  0.0  0.0  26104  2756 ?        Ss   19:02   0:00 SCREEN -S mysql
azureus+   2058  0.0  0.0   8212  2148 pts/0    S+   19:17   0:00 grep --color=auto -i SCREEN
```

Thus, even when there are no “vnstat” related processes running, we would get one entry showing the grep process:

```
azureuser@miccai:~$ ps aux | grep -i "vnstat"
azureus+   2070  0.0  0.0   8212  2212 pts/0    S+   19:18   0:00 grep --color=auto -i vnstat
```

Killing a process:

There are various commands you can use to kill a process — ```kill```, ```killall```, ```pkill``` and ```top```. We will begin from the simplest one: the ```killall``` command.

## killall

Killing processes with the ```killall``` command:

The killall command is one of the easiest ways to kill a process. If you know the exact name of a process, and you know that it’s not running as another user and it is not in the Z or D states, then you can use this command directly; there’s no need to manually locate the process as we described above.

By default,  For example, to kill a process named “firefox”, run:

```$ killall firefox```

To forcibly kill the process with ```SIGKILL```, run:

```$ killall -9 firefox```

You can also use ```-SIGKILL``` instead of ```-9```.

If you want to kill processes interactively, you can use -i like so:

```$ killall -i firefox```

If you want to kill a process running as a different user, you can use **sudo**:

```$ sudo killall firefox```

You can also kill a process that has been running for a certain period of time with the ```-o``` and ```-y``` flags. So, if you want to kill a process that has been running for more than 30 minutes, use:

```$ killall -o 30m <process-name>```

If you want to kill a process that has been running for less than 30 minutes, use:

```$ killall -y 30m <process-name>```

Similarly, use the following abbreviations for the respective units of time:

```
s   seconds
m   minutes
h   hours
d   days
w   weeks
M   months
y   years

```

## pkill

Killing processes with the ```pkill``` command
Sometimes, you only know part of a program’s name. Just like ```pgrep```, ```pkill``` allows you to kill processes based on partial matches. For example, if you want to kill all processes containing the name apache in the name, run:

```pkill apache```

If you want to use a ```SIGKILL``` instead of a ```SIGTERM```, use:

```pkill -9 apache```

Again, you can also use ```-SIGKILL``` instead of ```-9```.

## kill

Killing processes with the ```kill``` command:

Using the ```kill``` command is straightforward. Once you have found out the PID of the process that you want to ```kill```, you can terminate it using the ```kill``` command. For example, if you want to kill a process having a PID of 1234, then use the following command:

```kill 1234```

As we mentioned previously, the default is to use a ```SIGTERM```. To use a ```SIGKILL```, use ```-9``` or ```-SIGKILL``` as we have seen before:

```kill -9 1234```

## Using top

Killing processes with the ```top``` command:

It is very easy to kill processes using the ```top``` command. First, search for the process that you want to kill and note the PID. Then, press ```k``` while top is running (this is case sensitive). It will prompt you to enter the PID of the process that you want to kill.

