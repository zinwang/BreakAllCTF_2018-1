<br />

# BreakallCTF 2018-1

<br />

# Misc

雖然是misc，可是除了兩題linux，其他都是ppc
<br />


Linux
----------------------------
<br />

兩題都是很簡單的linux<br />
一題是用 ```ls -al ``` 找到隱藏的檔案<br />
而另一題則是到tmp資料夾```tar -zxvf```解壓tar.gz的檔案<br />
都是做到爛的題目<br />

PPC
----------------------------
而PPC只做了四題，有點可憐，雖然有很多是之前上課寫過的，可是還是花了一些時間<br />
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

<br />

PPC後記
----------------------------
因為之前上了ppc的課程，學了pwntools<br />
在某次閒閒沒事做時在android termux 終端裝了 pwntools<br />
又因為我的kali的某個pwntools的需求套件版本太新了，和pwntools不相容裝不起來<br />
到比賽開始要裝pwntools時才發現，如果要排除問題就會浪費時間，所以後來解ppc都是用手機做的~<br />
其實也蠻方便的，走到哪都可以解題。<br />
看到一些之前上課講過的題目，可是忘記怎麼做了<br />
而且好難debug，有時找了好久才發現丟字串時多了空格沒有strip掉<br />
不過這次比賽真的讓我重新複習之前學的東西~~感覺又更清楚了!


<br />

# Web

web實在沒時間研究，只解了3題<br />
有點記憶模糊，不太記得題目解題過程了

1.<br />
有一題是直接f12開發者工具看原始碼，就會發現base64加密的答案了

<br />
2.<br />
有一題是簡單的LFI，題目網址後面加/flag答案就出來了<br />

可以參考類似的題目: [MyFirstSecurity-Web](https://github.com/zinwang/CTF_2018/blob/master/Web101) Web第五題<br />

<br />
3.<br />
然後有一題是302重導，利用curl [網址]，好像不是馬上出現flag，後來過程有點記憶模糊了，<br />
不過curl 後大概後面就能順利找到flag了!
<br />



# 心得:
因為除了要解題以外，還要準備學校考試和唸書，所以能拿來解題的時間不多，所以只解了一些4題ppc<br />
兩題linux和幾題web,只能利用下課時間稍微想一下,web因為不太有時間研究,所以沒有太投入,不過經<br />
過這次的搶旗,讓我拾回了ppc的記憶,對於ppc和pwntools的運用更加熟悉了,雖然有些題目明明很有機<br />
會寫出來,可是一直找不到bug,最後就殘念了。這次比賽排名一直在30左右,結束時排名32,我想如果ppc<br />
多做幾題,排名應該更前面吧。

<br /><br />










