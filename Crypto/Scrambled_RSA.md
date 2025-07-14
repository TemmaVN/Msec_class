# Scrambled: RSA
- Người giải: Temma
- Thể loại: Mật mã
- Link bài: https://play.picoctf.org/practice/challenge/107?category=2&difficulty=3&page=2 
## Đề bài
Hmmm I wonder if you have learned your lesson... Let's see if you understand RSA and how the encryption works. Connect with nc mercury.picoctf.net 4484.
## Giải pháp
### Vấn đề
- Khi connect netcat trên terminal, ta sẽ thấy một vấn đề của flag.
```
(base) temma@DESKTOP-IE5BPNN:~$ nc mercury.picoctf.net 4484
flag: 700016894042300412229876257320589018693294585715443741186831598659557346931774986842576606844762318659015231121570318336155402933434137612933634952370633749917180633596427035234161046041774404869707814605393092168639758402499611171125341790065703614225132511000500888349754202963385804162566670446039724252958268380515262112701529768067763066889986513254946902692607696239673921932383538322135208205336749914515490947300206704114034743802694317389557961552740952523039447166877721500971625950491703670776154905217499345285414787092830454374494211627198755840459569968418763009090846258931946877016023684651848295507666849931384677309271861884770574320633537953592882690843204463508178528994509414917929967736225563618840481935454326888420212298267736086780450040954715762571707582277192694658659993551913296576068493227037639080925239197636163656659018954123172748201846125493391251963538832715750252463507887542756915630787341328895768903062733055851933847113359806045200656110981719697409965892006315050367329942320690095982080086570564496538331866642756432325002970658393021591277686344293286937042967823975511787938255274553042276157952110219916095609146466670071109975615542529394633716287984674020244237610055065748474181107558083004017675421738776220235011913698415321650618668625973100710328996575184884654666907571849390058822611584626820141341849571004873858218792454343203110174751400678796264022215223514125542110466150621383331529232605249820491095681595878876811336922624996292000908896503469395343015468473488798915903114304790894493522306937203.....
n: 96235663752460237882289371579535007146484584956338695724865174470234607032159940399685285898541453132626319417676912103640219368462511289265704235158717589174361569741890042100739175386748744252783456512612251738639877156250179835881670403642670779091786217237479677243057173618410160315269010984488723273577
e: 65537
I will encrypt whatever you give me:
```
- Nó siêu dài luôn nên chưa nghĩ đến việc giải mã, việc ép kiểu sang int đã khó rồi!
### Hướng đi sai!
- Ban đầu tôi lựa chọn cách chia flag thành từng phần xong gộp lại
- Vì tức quá và sai hướng nên đã delete nó đi!!
- Từ đó tôi tìm lại gợi ý và tìm lại hướng đi đúng
### Hiểu vấn đề 
- Vậy tôi bắt đầu phân tích lại gợi ý và phần mã hóa cho khi kết nối.
- Khi thử gõ 'a' thì ta sẽ nhận được 1 đoạn mã ngắn hơn!
```
I will encrypt whatever you give me: a
Here you go: 62982580984674476105580264762409334848835139633843695157641720861688160017170584340436177290910833345830630237838130444404507214992263886445662068311511184606910986783836269820433356716079607629676545532578523902776627745345617471977157873413764298267416359949416291224000236267548528432964559397136698739800
```
- Và khi gõ 'ab' thì có mã dài gấp đôi
```
I will encrypt whatever you give me: ab
Here you go: 11968581555756495074252678024999017753123189937831382815933916044877069784111775211203005405307844807479808494511811572149461815653671735401946336913124406906394563478033640109529374489467336867206122709239167462326516567357125388003375986544274719401736256574530705099956309996953665115795193795621153236232462982580984674476105580264762409334848835139633843695157641720861688160017170584340436177290910833345830630237838130444404507214992263886445662068311511184606910986783836269820433356716079607629676545532578523902776627745345617471977157873413764298267416359949416291224000236267548528432964559397136698739800
```
- Và hay nhất là trong nội dung có phần mã của 'a' trong đó nên tôi thắc mắc là liệu có 'b' trong đó không?
- Nhưng không..
```
I will encrypt whatever you give me: b
Here you go: 16470487553021009955825537340011822200726666953050387306558591594334989344314949753515672631931259543972556955236606498499092127261235092052616466373026550254028504472899796429157504135059682716924404584407652525755827153482502242978953479288488583871730033381658688354459562305590217734192328035720822394373
```
- Bản thân mắc thêm sai lầm khi cố điều chỉnh và tìm quy luật nhưng lại sai.
### Kết thúc
- Sau một hồi nghĩ và tra mạng tìm phương án giải quyết thì chợt nhận ra
- Nếu phần 'a' ở bên trái 'ab' thì chắc chắn có 'a' thì ta chỉ cần lần lượt thử các kí tự và thêm vào dần.
- Bằng cách cho một mảng gồm những phần đã biết và giải mã ( gọi là know_segments ) và đoạn mã cho đoạn mã đã giải thêm vào bởi kí tự mới
- Nhờ vậy ta chỉ cần lọc đoạn cũ, kiểm tra đoạn mới được thêm vào có trong flag không và tìm.
- Cũng khó hiểu nên bạn có thể tìm những write-up nữa và xem script của họ để dễ hiểu hơn.
- Nếu không có thể xem tạm đoạn sau!
```
#!/usr/bin/env python3

from pwn import *
from Crypto.Util.number import *
import string

lower =  string.ascii_lowercase
upper =  string.ascii_uppercase

server = 'mercury.picoctf.net'
port = 4484 

if server == '' or port == 0:
	print('Please sign a real server')
	exit(1)
else:p = remote(server,port)

info = lambda msg: log.info(msg)
s = lambda data, proc=None: proc.send(data) if proc else p.send(data)
sa = lambda msg, data, proc = None : proc.sendafter(msg,data) if proc else p.sendafter(msg,data)
sl = lambda data,proc = None: proc.sendline(data) if proc else p.sendline(data)
sla = lambda msg, data, proc = None: proc.sendlineafter(msg,data) if proc else p.sendlineafter(msg,data)
sn = lambda num, proc = None: proc.send(str(num).encode()) if proc else p.send(str(num).encode())
sna = lambda msg,num, proc = None: proc.sendafter(msg, str(num).encode()) if proc else p.sendafter(msg, str(num).encode())
sln = lambda num, proc = None: proc.sendline(str(num).encode()) if proc else p.sendline(str(num).encode())
slna = lambda msg, num, proc = None: proc.sendlineafter(msg,str(num).encode()) if proc else p.sendlineafter(msg, str(num).encode)
def ru(msg):
	return p.recvuntil(msg)
def rnl():
	return int(p.recvline().decode())
def rnu(msg):
	return int(p.recvuntil(msg))

n_text = b'n:'
e_text = b'e:'
c_text = b'flag: '

if n_text == '' or e_text == '' or c_text == '':
	print('Please sign the text')
	exit(1)

ru(c_text)
flag = p.recvline().decode().strip()
ru(n_text)
n = rnl()
ru(e_text)
e = rnl()

info(f'n = {n}')
info(f'e = {e}')
info(f'enc_flag = {flag}')

def replace_segments(result, segments):
	for segment in segments:
		result = result.replace(segment,'')
	return result

know_segments = []
decrypt_flag = ''
tap = 'picoCTF{'

for i in tap:
	decrypt_flag += i
	current_test = decrypt_flag
	sla(b'I will encrypt whatever you give me: ',decrypt_flag)
	current_str_test = p.recvuntil(b'\n').decode().strip()
	current_str_test = current_str_test.replace('Here you go: ','')
	current_char = replace_segments(current_str_test,know_segments)
	know_segments.append(current_char)
	info(f'Old char rep {decrypt_flag} + [{i}]')

while '}' not in decrypt_flag:
	for c in string.printable:
		current_test = decrypt_flag + c
		sla(b'I will encrypt whatever you give me: ',current_test)
		current_str_test = p.recvuntil(b'\n').decode().strip()
		current_str_test = current_str_test.replace('Here you go: ','')
		current_char_rep = replace_segments(current_str_test, know_segments)
		if (current_char_rep in flag):
			decrypt_flag += c
			info(f'Found new char {decrypt_flag} + [{c}]')
			know_segments.append(current_char_rep)
			break

info(f'Complete flag is {decrypt_flag}')
``` 
- Lưu ý đoạn script: đoạn 'picoCTF{' sẵn ở bên trái script nên đã chủ động thêm vào trước bởi đoạn code chạy cực lâu ( Thuật toán vét cạn ).
    - Flag thu được là: picoCTF{bad_1d3a5_5700361}