# Perplexed
- Link làm bài : https://play.picoctf.org/practice/challenge/458
- Dùng ida để decompile và ta sẽ có đoạn mã:
- Đoạn main không có j nhiều nên hãy coi hàm check:
```
_int64 __fastcall check(const char *a1)
{
  __int64 v2; // rbx
  __int64 v3; // [rsp+10h] [rbp-50h]
  __int64 v4[3]; // [rsp+18h] [rbp-48h]
  int v5; // [rsp+34h] [rbp-2Ch]
  int v6; // [rsp+38h] [rbp-28h]
  int v7; // [rsp+3Ch] [rbp-24h]
  int j; // [rsp+40h] [rbp-20h]
  unsigned int i; // [rsp+44h] [rbp-1Ch]
  int v10; // [rsp+48h] [rbp-18h]
  int v11; // [rsp+4Ch] [rbp-14h]

  if ( strlen(a1) != 27 )
    return 1LL;
  v3 = 0x617B2375F81EA7E1LL;
  v4[0] = 0xD269DF5B5AFC9DB9LL;
  *(__int64 *)((char *)v4 + 7) = 0xF467EDF4ED1BFED2LL;
  v11 = 0;
  v10 = 0;
  v7 = 0;
  for ( i = 0; i <= 22; ++i )
  {
    for ( j = 0; j <= 7; ++j )
    {
      if ( !v10 )
        v10 = 1;
      v6 = 1 << (7 - j);
      v5 = 1 << (7 - v10);
      if ( (v6 & *((char *)&v4[-1] + (int)i)) > 0 != (v5 & a1[v11]) > 0 )
        return 1LL;
      if ( ++v10 == 8 )
      {
        v10 = 0;
        ++v11;
      }
      v2 = v11;
      if ( v2 == strlen(a1) )
        return 0LL;
    }
  }
  return 0LL;
}
```
- Và từ đoạn main t cần hàm trả về 0 để giải mã
- Đầu tiên là đoạn 
```
if ( strlen(a1) != 27 )
    return 1LL;
``` 
- Nên ta cần nhập đúng 27 byte
- Vì hàm ở main là gets(lấy cả byte b'\x00') nên nhập đúng 26 kí tự
- Tiếp theo là thuật toán ( năng lực có hạn giải thích ko hiểu lm):
- Hàm chạy i (0 - 22) và j (0 - 7) và biến v5 sẽ là byte có 1 bit 1 tại vị trí 7 - j, v6 là tại 7 - v10
- Hàm if sẽ kiểm tra 7 bit của bộ nhớ và 7 bit nhỏ của a1[v11] có giống nhau ko
- Vậy tóm lại hàm sẽ check bộ nhớ từ v4[-1] 7 bit một và 7 bit nhỏ của tứng byte trong a1 có trùng nhau không.
- Vậy v4[-1] có giá trị gì?
- v4[-1] = v3 - hiểu là biến v3 dc khởi tạo ngay trước v4[0] nên v4[-1] sẽ chỉ định giá trị của v3
- Và do bộ nhớ được lưu dạng stack nên thứ tự địa chỉ sẽ là : &v3 -> v4 -> v4 + 1 .....
- Vậy nên ta cần đảo ngược từng đoạn 7 bit từ v4[-1] và chuyển về các byte đọc được!!
- Code python cơ bản của bài :
```
#!/usr/bin/python3

from pwn import *

tg = 'F467EDF4ED1BFED269DF5B5AFC9DB9617B2375F81EA7E1'

arr = []

for i in tg:
	arr.append(i)

print(len(arr))

arr1 = []

for i in range(23):
	kaka = arr[i*2] + arr[i*2+1]
	arr1.append(kaka)

arr1 = arr1[::-1]

arr2 = []
for i in arr1:
	ttg = int(i,16)
	ll = bin(ttg)[2:].zfill(8)
	arr2.append(ll)

binm = ''.join(i for i in arr2)

while len(binm)%7!=0:
	binm += '0'

password = ''

for i in range(27):
	chunk = binm[i*7:(i+1)*7]
	char_val = int(chunk,2)
	password += chr(char_val)

print(password)
```
- Chạy hàm và sẽ in ra Flag : picoCTF{0n3_bi7_4t_a_7im3}