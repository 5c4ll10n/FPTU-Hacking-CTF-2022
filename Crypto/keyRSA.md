
# keyRSA

* Challenge gồm 1 đường dẫn `nc 103.245.249.76 49160` và 1 file source.

``` python
from Crypto.Util.number import bytes_to_long, getPrime

flag = b'<REDACTED>'

p, q = getPrime(512), getPrime(512)
n = p * q
e = 65537

msg = bytes_to_long(flag)

assert n > msg
ct = pow(msg,e,n)

phi = (p-1) * (q-1)
d = pow(e,-1,phi)

yn = input(b'Do you want to try your own key? [ y/n ] : ) ')
if yn == 'y':
    print('\nSure, here are your keys \n')
    print(f'\ne = {e}')
    print(f'\nn = {n}')
    print(f'\nx = {p % (n // 2)}')

    user_d = int(input("\nEnter your key : \n"))
    if user_d != d:
        if pow(ct,user_d,n) == pow(ct,d,n):
            print(flag)
        else:
            print('Better luck next time !!')
            exit(0)
    else:
        print("Sorry you can't use my key, or maybe our keys were similar this time, try again !!")
        exit(0)
else:
    print('See you next time')
    exit(0)
```
* Ta thấy được rằng con số ta nhập vào chính là biến user_d và việc ta cần làm là nhập số sao cho `pow(ct,user_d,n) == pow(ct,d,n)` và `user_d != d`
* Tra từ khóa RSA trên wiki ([link](https://en.wikipedia.org/wiki/RSA_(cryptosystem))) ta có được công thức`(m^e)^d mod n = m mod n`. Bài hiện tại ta coi m = ct^d, suy ra đồng dư của nó chính là (m^d)^e. Ta tính toán được `user_d = d*d*e` chính là số cần tìm. Giờ chúng ta đi tính d:
* `x = {p % (n // 2)}` mà n = p*q suy ra x = p vì x/2 > p. Từ đó tính được q = n/x => phi = (p-1)(q-1) => `d = pow(e,-1,phi)`
* Code một đoạn python đơn giản. Giờ ta chỉ cần kết nối đến sever rồi cop giá trị n và x vào code rồi cop nhanh output.
``` python
e = 65537
n = 1
x = 1
p = x
q = n//p
phi = (p-1)*(q-1)
d = pow(e,-1,phi)
print(d*d*e)
```

### Flag: FPTUHacking{Y0u_kn0w_t0_CR34t3_y0ur_0wn_k3y!!!}

  
