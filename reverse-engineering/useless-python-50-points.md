# Useless Python - 50 points

Boredom took over, so I wrote this python file! I didn't want anyone to see it though because it doesn't actually run, so I used the coolest base-16 encoding to keep it secret. [python](https://github.com/EasyCTF/easyctf-2017-problems/blob/master/useless-python/useless.py)

### Solution
###### Writeup by Valar Dragon

The first step is clear from the problem statement, unhexlify the file useless.py

Unhexxing it gives:

``` python
exec(chr(101)+chr(120)+chr(101)+chr(99)+chr(40)+chr(99)+chr(104)+chr(114)+chr(40) ...)

```

This challenge was probably inspired by everyone (us included) solving Fzz Buzz 2 in a way they didn't intend, by using exec and converting all the banned characters to `exec('chr(ascii code)')`

So just create a for loop that keeps replacing ``'+chr(x)'`` with the actual character
Quickly writing this in `useless_solver.py`, gives the flag

``` python
exec(chr(101)xec(chr(101)xec(chr(102)lag = 'easyctf{python_3x3c_exec_3xec_ex3c}'
priint flag)))
```
There are some errors, due to the left edge case, but its good enough to get the flag without errors!
Quick 50 points.

### External Writeups

* [https://github.com/HackThisCode/CTF-Writeups/tree/master/2017/EasyCTF/Useless%20Python/README.md](https://github.com/HackThisCode/CTF-Writeups/tree/master/2017/EasyCTF/Useless%20Python/README.md)
* [https://www.bamsoftware.com/computers/easyctf-2017/#useless](https://www.bamsoftware.com/computers/easyctf-2017/#useless)
