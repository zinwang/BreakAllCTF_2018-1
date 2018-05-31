<br />

# BreakallCTF 2018-1

<br />

# Misc

雖然是misc，可是除了兩題linux，其他都是ppc
<br />


兩題linux
----------------------------
<br />

兩題都是很簡單的linux<br />
一題是用 ```ls -al ``` 找到隱藏的檔案<br />
而另一題則是解壓tar.gz的檔案<br />
都是做到爛的題目<br />

PPC
----------------------------
而PPC只做了四題，有點可憐，雖然有很多事之前上課寫過的，可是還是花了一些時間<br />
就把他當複習吧!

<br />
1.<br />
有一題是解100題數英文字母:<br />
給一段字串，數某字母出現的次數，一百題<br />


```
#!/usr/bin/python3

from pwn import *

r=remote("140.110.112.29",5128)
r.recvuntil("Can you help us?\n")

for i in range(100):
  r.recvline()
  r.rcvuntil("How many ")
  b=r.recvuntil(" ").strip().decode('ascii')
  r.recvline("in ")
  a=r.recvline().strip().decode('ascii')
  print(a,b)
  cnt=0
  
  for j in range(len(a)):
    if a[j]==b:
      cnt+=1
  r.sendline(str(cnt))
  
 r.interactive()
 
 
 
```




<br />

2.<br />
還有這題，對於這題的印象最深，之前上課有做過，<br />
因為最簡單所以印象最深~<br />
<br />
這題是要把華氏轉攝氏，但用分數表達，一樣100題:<br />

```
#!/usr/bin/python3

from pwn import *

r=remote("140.110.112.29",5127)

r.recvuntil("-110/9\n")

for i in range(100):
  f=int(r.recvline())
  c=str((f-32)*5)
  c+="/9"
  r.sendline(c)
  
r.interactive()


```


<br />

3.<br />
這題也是上課講過的，利用lambda函數，<br />
他給四條公式:<br />
f0(x) = 3x^2 + x +3
f1(x) = 5x^2 + 8
f2(x) = 4*x^3 + 6x +6




















