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
給一段字串，數某字母出現的次數，一百題<br /><br />


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
他給五條公式:<br />
f0(x) = 3x^2 + x +3<br />
f1(x) = 5x^2 + 8<br />
f2(x) = 4x^3 + 6x +6<br />
f3(x) = 7x^3 + 5x^2<br />
f4(x) = x^2 + 4x + 3<br />
<br />
然後問一百道題目，看是要用哪一條公式並帶入x值:<br />

```
#!/usr/bin/python3

from pwn import *

r=remote("140.110.112.29",5124)

r.recvuntil("f(x) = 28\n")

f0 =lambda x: 3*(x**2) + x + 3
f1 =lambda x: 5*(x**2) + 8
f2 =lambda x: 4*(x**3) + 6*x + 6
f3 =lambda x: 7*(x**3) + 5*(x**2)
f4 =lambda x: x**2 + 4*x + 3

for i in range(100):
  r.recvline()
  r.recvuntil("function : ")
  a=r.recvline().strip().decode('ascii')
  r.recvuntil("x = ")
  b=r.recvline.strip().decode('ascii')
  c=str(eval("f" + a + "(" + b + ")"))
  print(a,b,c)
  r.sendline(c.strip())
  
r.interactive()

```

<br />
4.<br />
至於這題則是一百道凱薩位移:<br />
<br />

```
#!/usr/bin/python3

from pwn import *

r=remote("140.110.112.29",5123)
r.recvline()

def add(m,n):
  e=''
  for i in range(len(m)):
    if ord(m[i])>=65 and ord(m[i])<=90:
      e+=chr(65 + (ord(m[i]) - 65 + n)%26)
    else:
      e+=m[i]
  return e
  
def minus(m,n):
  e=''
  for i in range(len(m)):
    if ord(m[i])>=65 and ord(m[i])<=90:
      e+=chr(90 - (90 - ord(m[i]) + n)%26)
    else:
      e+=m[i]
  return e
  
  
 for i in range(100):
  r.recvline()
  r.recvuntil("word by ")
  a=r.recvuntil(" ").strip().decode('ascii')
  r.recvuntil(": ")
  b=r.recvline().decode('ascii')
  
  d=''
  
  if a[0]=="+":
    c=int(a.strip("+"))
    d=add(b,c)
  elif a[0]=="-":
    c=int(a.strip("+"))
    d=minus(b,c)
  
  print(a,b,d)
  r.sendline(str(d).strip())
 
 r.interactive()
  

```


















