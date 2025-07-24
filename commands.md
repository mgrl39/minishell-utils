```bash
test@cbr2s5:~$ << "$LOL"
> 
> ""
> "$LOL"
> " "
> ""
> 
>  
> $LOL
```

```bash
test@cbr2s5:~$ << a << b << c 
> a
> b
> c
test@cbr2s5:~$ << a << b << c 
a
b
c
test@cbr2s5:~$ << a << b << c 
> c
> b
> a
> a
> b
> c
test@cbr2s5:~$
```
```bash
test@cbr2s5:~$ <<\:
> :
test@cbr2s5:~$
```
