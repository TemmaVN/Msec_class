# rsa-pop-quiz
- Cảnh báo bài hơi dài nha!!
- Người giải: Temma
- Thể loại: mật mã
- Link đề: https://play.picoctf.org/practice/challenge/61?category=2&difficulty=3&page=3
## Đề bài
Class, take your seats! It's PRIME-time for a quiz... nc jupiter.challenges.picoctf.org 1981
## Giải quyết
- Connect và giải quyết từng phần thôi !!
- Vì đã 'lỡ' viết script và dài nên bài này sẽ cắt từng đoạn theo script và giải thích
### Vấn đề 1
```
┌──(pwnenv)─(temma㉿Temma)-[~/Documents/picoCTF/Crypto/rsa_pop_quiz]
└─$ ./solve.py
[+] Opening connection to jupiter.challenges.picoctf.org on port 1981: Done
[*] Switching to interactive mode
Good morning class! It's me Ms. Adleman-Shamir-Rivest
Today we will be taking a pop quiz, so I hope you studied. Cramming just will not do!
You will need to tell me if each example is possible, given your extensive crypto knowledge.
Inputs and outputs are in decimal. No hex here!
#### NEW PROBLEM ####
q : 60413
p : 76753
##### PRODUCE THE FOLLOWING ####
n
IS THIS POSSIBLE and FEASIBLE? (Y/N):$ Y
#### TIME TO SHOW ME WHAT YOU GOT! ###
n: $  
```
- Sau khi connect ta sẽ nhận được p,q. Việc cần làm là tính n = p*q và gửi lại.
### Vấn đề 2
```
 Outstanding move!!!


#### NEW PROBLEM ####
p : 54269
n : 5051846941
##### PRODUCE THE FOLLOWING ####
q
IS THIS POSSIBLE and FEASIBLE? (Y/N):$ Y
#### TIME TO SHOW ME WHAT YOU GOT! ###
q: $  
```
- Lần này cho n và p, ta chỉ cần gửi q = n//p là được
### Vấn đề 3
```
 Outstanding move!!!


#### NEW PROBLEM ####
e : 3
n : 12738162802910546503821920886905393316386362759567480839428456525224226445173031635306683726182522494910808518920409019414034814409330094245825749680913204566832337704700165993198897029795786969124232138869784626202501366135975223827287812326250577148625360887698930625504334325804587329905617936581116392784684334664204309771430814449606147221349888320403451637882447709796221706470239625292297988766493746209684880843111138170600039888112404411310974758532603998608057008811836384597579147244737606088756299939654265086899096359070667266167754944587948695842171915048619846282873769413489072243477764350071787327913
##### PRODUCE THE FOLLOWING ####
q
p
IS THIS POSSIBLE and FEASIBLE? (Y/N):$  
```
- Lần này đề cho n và e, bắt tìm p và q. Ta có thể thử trên factorydb.com hoặc một số trang phân tích khác nhưng vẫn đều không được. Pick 'N' thôi :))
### Vấn đề 4
```
Outstanding move!!!


#### NEW PROBLEM ####
q : 66347
p : 12611
##### PRODUCE THE FOLLOWING ####
totient(n)
IS THIS POSSIBLE and FEASIBLE? (Y/N):$  
```
- Đề cho p và q, ở đây totient là hàm phi của n = p*q nên chỉ cần cần gửi phi = (p-1)*(q-1) là ra!
### Vấn đề 5
```
#### NEW PROBLEM ####
plaintext : 6357294171489311547190987615544575133581967886499484091352661406414044440475205342882841236357665973431462491355089413710392273380203038793241564304774271529108729717
e : 3
n : 29129463609326322559521123136222078780585451208149138547799121083622333250646678767769126248182207478527881025116332742616201890576280859777513414460842754045651093593251726785499360828237897586278068419875517543013545369871704159718105354690802726645710699029936754265654381929650494383622583174075805797766685192325859982797796060391271817578087472948205626257717479858369754502615173773514087437504532994142632207906501079835037052797306690891600559321673928943158514646572885986881016569647357891598545880304236145548059520898133142087545369179876065657214225826997676844000054327141666320553082128424707948750331
##### PRODUCE THE FOLLOWING ####
ciphertext
IS THIS POSSIBLE and FEASIBLE? (Y/N):$ 
```
- Tiếp đến cho plaintext, e và n.
- Yêu cầu tính ciphertext ->> đơn giản là gửi ciphertext = pow(plaintext, e,n)
### Vấn đề 6
```
Outstanding move!!!


#### NEW PROBLEM ####
ciphertext : 107524013451079348539944510756143604203925717262185033799328445011792760545528944993719783392542163428637172323512252624567111110666168664743115203791510985709942366609626436995887781674651272233566303814979677507101168587739375699009734588985482369702634499544891509228440194615376339573685285125730286623323
e : 3
n : 27566996291508213932419371385141522859343226560050921196294761870500846140132385080994630946107675330189606021165260590147068785820203600882092467797813519434652632126061353583124063944373336654246386074125394368479677295167494332556053947231141336142392086767742035970752738056297057898704112912616565299451359791548536846025854378347423520104947907334451056339439706623069503088916316369813499705073573777577169392401411708920615574908593784282546154486446779246790294398198854547069593987224578333683144886242572837465834139561122101527973799583927411936200068176539747586449939559180772690007261562703222558103359
##### PRODUCE THE FOLLOWING ####
plaintext
IS THIS POSSIBLE and FEASIBLE? (Y/N):$ Y
#### TIME TO SHOW ME WHAT YOU GOT! ###
plaintext: $  
```
- Đề cho ciphertext, e (bé nha), và n
- Yêu cầu tìm plaintext. Dùng thư viện gmpy2 và kiểm tra ( đến khoảng 1000) cho cbrt(c + i*n).
- Nếu ra True và cho plaintext thì gửi Y và plaintext
- Nếu không gửi lại False
### Vấn đề 7
```
Outstanding move!!!


#### NEW PROBLEM ####
q : 92092076805892533739724722602668675840671093008520241548191914215399824020372076186460768206814914423802230398410980218741906960527104568970225804374404612617736579286959865287226538692911376507934256844456333236362669879347073756238894784951597211105734179388300051579994253565459304743059533646753003894559
p : 97846775312392801037224396977012615848433199640105786119757047098757998273009741128821931277074555731813289423891389911801250326299324018557072727051765547115514791337578758859803890173153277252326496062476389498019821358465433398338364421624871010292162533041884897182597065662521825095949253625730631876637
e : 65537
##### PRODUCE THE FOLLOWING ####
d
IS THIS POSSIBLE and FEASIBLE? (Y/N):$  
```
- Cho p,q và e. Yêu cầu tìm d.
- Ta có thể tạo hàm nghịch đảo module (nếu được) hoặc dùng pycryptodome và dùng hàm inverse để giải bài.
### Vấn đề 8
```
 Outstanding move!!!


#### NEW PROBLEM ####
p : 153143042272527868798412612417204434156935146874282990942386694020462861918068684561281763577034706600608387699148071015194725533394126069826857182428660427818277378724977554365910231524827258160904493774748749088477328204812171935987088715261127321911849092207070653272176072509933245978935455542420691737433
ciphertext : 18031488536864379496089550017272599246134435121343229164236671388038630752847645738968455413067773166115234039247540029174331743781203512108626594601293283737392240326020888417252388602914051828980913478927759934805755030493894728974208520271926698905550119698686762813722190657005740866343113838228101687566611695952746931293926696289378849403873881699852860519784750763227733530168282209363348322874740823803639617797763626570478847423136936562441423318948695084910283653593619962163665200322516949205854709192890808315604698217238383629613355109164122397545332736734824591444665706810731112586202816816647839648399
e : 65537
n : 23952937352643527451379227516428377705004894508566304313177880191662177061878993798938496818120987817049538365206671401938265663712351239785237507341311858383628932183083145614696585411921662992078376103990806989257289472590902167457302888198293135333083734504191910953238278860923153746261500759411620299864395158783509535039259714359526738924736952759753503357614939203434092075676169$                                                                                                          179112452620687731670534906069845965633455748606649062394293289967059348143206600765820021392608270528856238306849191113241355842396325210132358046616312901337987464473799040762271876389031455051640937681745409057246190498795697239
##### PRODUCE THE FOLLOWING ####
plaintext
IS THIS POSSIBLE and FEASIBLE? (Y/N):$ Y
#### TIME TO SHOW ME WHAT YOU GOT! ###
plaintext: $  
```
- Đề cho p, ciphertext, n và e. Yêu cầu tìm plaintext.
- Tìm q = n // p, phi = (p-1)*(q-1), d = inverse(e,phi) xong gửi plaintext = pow(ciphertext,d,n) là xong!
### Cuối cùng
```
 Outstanding move!!!


If you convert the last plaintext to a hex number, then ascii, you'll find what you need! ;
```
- Lạ nhỉ server không gửi flag, chuyển thử thành dạng hex và string của plaintext trên thử.
```
[*] hex_plain = 0x7069636f4354467b7741385f74683474245f696c6c336147616c2e2e6f64653031653462627d
[*] str_plain = picoCTF{wA8_th4t$_ill3aGal..ode01e4bb}
```
- Vậy là dạng string của plaintext là flag cần tìm!!
### Script tham khảo
```
#!/usr/bin/env python3

from Crypto.Util.number import *
from pwn import *
import gmpy2

server = 'jupiter.challenges.picoctf.org'
port = 1981

ser = remote(server,port)

e = 0
n = 0
p = 0
q = 0

### Pharse 1 ###


ser.recvuntil(b'q :')
q = int(ser.recvline().decode())
ser.recvuntil(b'p :')
p = int(ser.recvline().decode())
ser.sendlineafter(b'(Y/N):',b'Y')
ser.sendlineafter(b'n:',str(p*q).encode())

log.info(f'q = {q}')
log.info(f'p = {p}')



## Pharse 2 ###

ser.recvuntil(b'p :')
p = int(ser.recvline().decode())
ser.recvuntil(b'n :')
n = int(ser.recvline().decode())
ser.sendlineafter(b'(Y/N):',b'Y')
ser.sendlineafter(b'q:',str(n//p).encode())
log.info(f'p = {p}')
log.info(f'n = {n}')

## Pharse 3 ###

ser.sendlineafter(b'(Y/N):',b'N')

### Pharse 4 ###

ser.recvuntil(b'q :')
q = int(ser.recvline().decode())
ser.recvuntil(b'p :')
p = int(ser.recvline().decode())
ser.sendlineafter(b'(Y/N):',b'Y')
ser.sendlineafter(b'(n)',str((p-1)*(q-1)).encode())
log.info(f'q = {q}')
log.info(f'p = {p}')

### Pharse 5 ###


ser.recvuntil(b'plaintext : ')
plain = int(ser.recvline().decode())
ser.recvuntil(b'e : ')
e = int(ser.recvline().decode())
ser.recvuntil(b'n :')
n = int(ser.recvline().decode())
ser.sendlineafter(b'(Y/N):', b'Y')
ser.sendlineafter(b'ciphertext:',str(pow(plain,e,n)).encode())

### Pharse 6 ###


ser.recvuntil(b'ciphertext : ')
c = int(ser.recvline().decode())
ser.recvuntil(b'e : ')
e = int(ser.recvline().decode())
ser.recvuntil(b'n :')
n = int(ser.recvline().decode())

plain = 0
IsTrue = False
for i in range(1000):
	plain, IsTrue = gmpy2.iroot(c + i*n,3)
	if IsTrue: break
if IsTrue: 
	ser.sendlineafter(b'(Y/N):',b'Y')
	ser.sendlineafter(b'plaintext:',str(plain).encode())
else: ser.sendlineafter(b'(Y/N):',b'N')

### Pharse 7 ###

ser.recvuntil(b'q :')
q = int(ser.recvline().decode())
ser.recvuntil(b'p :')
p = int(ser.recvline().decode())
ser.recvuntil(b'e :')
e = int(ser.recvline().decode())

phi = (p-1)*(q-1)

ser.sendlineafter(b'(Y/N):',b'Y')
ser.sendlineafter(b'd:',str(inverse(e,phi)).encode())

### Pharse 8 ###

ser.recvuntil(b'p :')
p = int(ser.recvline().decode())
ser.recvuntil(b'ciphertext : ')
c = int(ser.recvline().decode())
ser.recvuntil(b'e : ')
e = int(ser.recvline().decode())
ser.recvuntil(b'n :')
n = int(ser.recvline().decode())

q = n//p
phi = (p-1)*(q-1)
d = inverse(e,phi)
plain = pow(c,d,n)

ser.sendlineafter(b'(Y/N):',b'Y')
ser.sendlineafter(b'plaintext:',str(plain).encode())

hex_plain = hex(plain)
str_plain = long_to_bytes(plain).decode()

log.info(f'hex_plain = {hex_plain}')
log.info(f'str_plain = {str_plain}')

ser.interactive()
```
- Flag là : picoCTF{wA8_th4t$_ill3aGal..ode01e4bb}