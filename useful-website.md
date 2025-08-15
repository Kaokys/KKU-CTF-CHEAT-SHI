# 🛠 CTF Tools & Websites Cheat Sheet (พร้อมคำอธิบาย)

## 🖼 Image Forensics
- **Forensically** - https://29a.ch/photo-forensics/  
  วิเคราะห์การตัดต่อ/แก้ไขภาพแบบออนไลน์
- **Forensic Magnifier** - https://29a.ch/photo-forensics/#forensic-magnifier  
  ขยายภาพระดับพิกเซล ดูรายละเอียดเล็กๆ
- **Clone Detection** - https://29a.ch/photo-forensics/#clone  
  ตรวจหาพื้นที่ที่ถูกคัดลอกซ้ำในภาพ
- **Error Level Analysis (ELA)** - https://29a.ch/photo-forensics/#ela  
  ดูการบีบอัดต่างกันเพื่อหาการแก้ไข
- **Noise Analysis** - https://29a.ch/photo-forensics/#noise  
  ตรวจสัญญาณรบกวนเพื่อดูว่ามีการรีทัชหรือไม่
- **Metadata Viewer** - https://29a.ch/photo-forensics/#metadata  
  อ่านข้อมูล EXIF, GPS ของภาพ

---

## 💾 Digital Forensics & File Analysis
- **The Sleuth Kit** - https://www.sleuthkit.org/sleuthkit/man/  
  วิเคราะห์ไฟล์, ดิสก์อิมเมจ, ระบบไฟล์
- **Autopsy** - https://www.autopsy.com/  
  GUI ของ The Sleuth Kit ใช้งานง่าย
- **ExifTool** - https://exiftool.org/  
  อ่าน/แก้ metadata ของไฟล์ทุกชนิด
- **Binwalk** - https://github.com/ReFirmLabs/binwalk  
  แยกและวิเคราะห์ firmware หรือไฟล์ binary
- **Foremost** - http://foremost.sourceforge.net/  
  กู้ไฟล์จากดิสก์อิมเมจตามนามสกุล
- **FTK Imager** - https://accessdata.com/product-download/ftk-imager  
  ทำสำเนา (image) ดิสก์แบบ forensics
- **Volatility** - https://www.volatilityfoundation.org/  
  วิเคราะห์ dump หน่วยความจำ (RAM)
- **Magnet AXIOM** - https://www.magnetforensics.com/products/axiom/  
  วิเคราะห์ข้อมูลจากดิสก์ มือถือ และ cloud

---

## 🔐 Cryptography & Encoding
- **CyberChef** - https://gchq.github.io/CyberChef/  
  ครอบจักรวาล encode/decode/crypto analysis
- **CrackStation** - https://crackstation.net/  
  แตก hash ด้วย rainbow table
- **dCode** - https://www.dcode.fr/  
  เครื่องมือแกะรหัสหลากหลายแบบ
- **Cipher Tools** - https://www.dcode.fr/caesar-cipher  
  Caesar cipher และ cipher แบบคลาสสิก
- **Hash Analyzer** - https://www.tunnelsup.com/hash-analyzer/  
  ตรวจประเภท hash จากค่า
- **MD5Hashing** - https://md5hashing.net/  
  แตก MD5, SHA1, SHA256
- **Factordb** - http://factordb.com/  
  ฐานข้อมูลตัวประกอบ (ใช้แกะ RSA)

---

## 🌐 Web Exploitation
- **Burp Suite** - https://portswigger.net/burp  
  Proxy และ pentest web app
- **OWASP ZAP** - https://www.zaproxy.org/  
  Web scanner ฟรี
- **SQLmap** - http://sqlmap.org/  
  SQL Injection อัตโนมัติ
- **Nikto** - https://cirt.net/Nikto2  
  สแกนช่องโหว่เว็บเซิร์ฟเวอร์
- **Gobuster** - https://github.com/OJ/gobuster  
  Brute force directory/ไฟล์
- **Ffuf** - https://github.com/ffuf/ffuf  
  Directory fuzzing เร็วและยืดหยุ่น
- **XSStrike** - https://github.com/s0md3v/XSStrike  
  ตรวจและ exploit XSS

---

## 🔍 Reverse Engineering
- **Ghidra** - https://ghidra-sre.org/  
  Reverse engineering จาก NSA
- **IDA Free** - https://hex-rays.com/ida-free/  
  Disassembler/Debugger
- **Radare2** - https://rada.re/n/  
  Framework reverse engineering
- **Online Disassembler** - https://onlinedisassembler.com/  
  ถอดรหัส binary ออนไลน์
- **Decompiler Explorer** - https://dogbolt.org/  
  เทียบ output จากหลาย decompiler
- **Binary Ninja (Demo)** - https://binary.ninja/  
  RE tool ใช้ง่าย
- **Cutter** - https://cutter.re/  
  GUI ของ Radare2
- **Frida** - https://frida.re/  
  Hook และ instrument app runtime

---

## 💣 Binary Exploitation & PWN
- **pwntools** - https://github.com/Gallopsled/pwntools  
  Python framework ทำ exploit
- **ROP Gadget** - https://github.com/JonathanSalwan/ROPgadget  
  หา gadget สำหรับ ROP
- **One Gadget** - https://github.com/david942j/one_gadget  
  หา one-shot exploit libc
- **LibcDB** - https://libc.blukat.me/  
  ค้นหา libc version ตาม symbol
- **gef** - https://github.com/hugsy/gef  
  GDB plugin สำหรับ pwn
- **peda** - https://github.com/longld/peda  
  GDB enhancement
- **angr** - https://angr.io/  
  Binary analysis (symbolic execution)

---

## 🕵️ OSINT & Reconnaissance
- **Shodan** - https://www.shodan.io/  
  ค้นหาอุปกรณ์ IoT ที่เปิดบนเน็ต
- **Censys** - https://censys.io/  
  สแกนอินเทอร์เน็ตแบบเจาะลึก
- **Wayback Machine** - https://web.archive.org/  
  ดูเว็บย้อนหลัง
- **TinEye** - https://tineye.com/  
  Reverse image search
- **Have I Been Pwned** - https://haveibeenpwned.com/  
  เช็กข้อมูลรั่ว
- **Maltego (CE)** - https://www.maltego.com/  
  สร้างกราฟความสัมพันธ์ข้อมูล
- **Hunter.io** - https://hunter.io/  
  หาข้อมูลอีเมลของโดเมน
- **theHarvester** - https://github.com/laramies/theHarvester  
  เก็บข้อมูลจากแหล่งสาธารณะ

---

## 🗃 Steganography
- **StegOnline** - https://stegonline.georgeom.net/  
  Stego ออนไลน์
- **Steghide** - http://steghide.sourceforge.net/  
  ซ่อนข้อมูลในภาพ/เสียง
- **zsteg** - https://github.com/zed-0xff/zsteg  
  ตรวจ stego ใน PNG/BMP
- **StegSolve** - https://github.com/eugenekolo/sec-tools/tree/master/stego/stegsolve  
  วิเคราะห์ภาพด้วย filter

---

## 📡 Network Analysis
- **Wireshark** - https://www.wireshark.org/  
  Packet analyzer ครบเครื่อง
- **NetworkMiner** - https://www.netresec.com/?page=NetworkMiner  
  Network forensics
- **tcpdump** - https://www.tcpdump.org/  
  Packet capture CLI
- **Ettercap** - https://www.ettercap-project.org/  
  MITM และ sniffing
- **Bettercap** - https://www.bettercap.org/  
  Network attack framework
- **Nmap** - https://nmap.org/  
  Network scanner

---

## 📶 Wireless Hacking
- **Aircrack-ng** - https://www.aircrack-ng.org/  
  Crack WEP/WPA/WPA2
- **Kismet** - https://www.kismetwireless.net/  
  ตรวจจับเครือข่ายไร้สาย
- **WiFi Pumpkin** - https://github.com/P0cL4bs/WiFi-Pumpkin  
  Rogue AP / WiFi phishing

---

## 🔑 Password Cracking
- **John the Ripper** - https://www.openwall.com/john/  
  Multi-hash password cracker
- **Hashcat** - https://hashcat.net/hashcat/  
  GPU accelerated cracking
- **CeWL** - https://github.com/digininja/CeWL  
  สร้าง wordlist จากเว็บ

---

## 📱 Mobile Forensics & Pentesting
- **MobSF** - https://mobsf.github.io/Mobile-Security-Framework-MobSF/  
  วิเคราะห์แอปมือถือทั้ง static/dynamic
- **APKTool** - https://ibotpeaches.github.io/Apktool/  
  Decompile/Recompile APK
- **Objection** - https://github.com/sensepost/objection  
  Hook แอป Android/iOS runtime

---

## 🦠 Malware Analysis
- **REMnux** - https://remnux.org/  
  Linux distro สำหรับ malware analysis
- **Any.Run** - https://any.run/  
  Interactive malware sandbox
- **Hybrid Analysis** - https://www.hybrid-analysis.com/  
  Malware analysis online

---

## 🧰 Miscellaneous Tools
- **CyberChef Recipes** - https://github.com/mattnotmax/cyberchef-recipes  
  สูตรสำเร็จสำหรับ CyberChef
- **GTFOBins** - https://gtfobins.github.io/  
  Unix binary bypass security
- **PayloadsAllTheThings** - https://github.com/swisskyrepo/PayloadsAllTheThings  
  Payloads และเทคนิคโจมตี
- **HackTricks** - https://book.hacktricks.xyz/  
  รวมวิธี pentest และ tips
- **Regex101** - https://regex101.com/  
  ทดสอบ regex ออนไลน์

---

## 💻 Online IDEs & Compilers
- **Replit** - https://replit.com/  
  IDE ออนไลน์หลายภาษา
- **OnlineGDB** - https://www.onlinegdb.com/  
  Compile/Debug ออนไลน์
- **Compiler Explorer** - https://godbolt.org/  
  ดู output assembly ของ code
