# RSA_ORACLE

- Link đề bài : https://play.picoctf.org/practice/challenge/422
- Chạy thử lệnh ```nc titan.picoctf.net 50006``` (port mỗi người sẽ khác nên để ý).
- Cơ bản E sẽ sinh ra mã từ đoạn plaintext
- D giải mã 
- Nhưng khi đưa message vào thì: ```Enter text to decrypt: 1634668422544022562287275254811184478161245548888973650857381112077711852144181630709254123963471597994127621183174673720047559236204808750789430675058597
Lol, good try, can't decrypt that for you. Be creative and good luck```
- V nên ta sẽ thêm 2 vào
- Tức là m ** e (mod n) = c
- 2 ** e (mod n) = c1 (Dùng E để tạo)
- ==> (2*m)**e (mod n) = c*c1 = c2
- Sau đó decrypt với c2 - xong từ đó chia 2 và t được key
- Tiếp theo ở gợi ý 2 cho ta lệnh ```OpenSSL can be used to decrypt the message. e.g openssl enc -aes-256-cbc -d ...```
- Cụ thể với file enc của ta thì câu lệnh đủ là
```OpenSSL can be used to decrypt the message. e.g openssl enc -aes-256-cbc -d -in secret.enc -k {mật khẩu trên}```
- Vậy thay mật khẩu trên là oke!
- Code python tham khảo ( viết tay nên hơi xấu !!)
```
#!/usr/bin/python3

from Crypto.Util.number import *
import binascii
from pwn import *

p = remote('titan.picoctf.net',59448)

response = p.recvuntil('decrypt.')

payload = b'E' + b'\n'

p.send(payload)

response = p.recvuntil('keysize): ')

password = 1634668422544022562287275254811184478161245548888973650857381112077711852144181630709254123963471597994127621183174673720047559236204808750789430675058597

payload = b'\x02' + b'\n'

p.send(payload)

response = p.recvuntil('(m ^ e mod n) ')
response = p.recvline()
num  = int(response.decode())*password

print(num,'----------------------')

response = p.recvuntil('decrypt.')
payload = b'D' + b'\n'
p.send(payload)
response = p.recvuntil('decrypt:')

payload = str(num) + '\n'

p.send(payload)

response = p.recvuntil('(c ^ d mod n): ')

response = p.recvline()

tg = response.decode().strip()

tg1 = int(tg,16)

tg1 = tg1//2

print(long_to_bytes(tg1))
```
- Dùng code chạy lấy mk rồi áp vào thôi!
- Flag là : picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}