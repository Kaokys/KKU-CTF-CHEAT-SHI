# สรุปไว้อ่านครับไอ่🙉ำ

# คำสั่ง Terminal พื้นฐาน (Common Terminal Commands)

## คำสั่งหลักและการนำทาง (Key Commands & Navigation)

ก่อนที่เราจะดูคำสั่งทั่วไป ขอแนะนำคำสั่งแป้นพิมพ์ที่มีประโยชน์มาก:
(Before we look at common commands, here are very helpful keyboard shortcuts:)

- `Up Arrow` (ลูกศรขึ้น): แสดงคำสั่งล่าสุดที่ใช้ (Shows your last command)
- `Down Arrow` (ลูกศรลง): แสดงคำสั่งถัดไป (Shows your next command)
- `Tab`: ช่วยเติมคำสั่งอัตโนมัติ (Auto-completes your command)
- `Ctrl + L`: ล้างหน้าจอ (Clears the screen)
- `Ctrl + C`: ยกเลิกคำสั่ง (Cancels a command)
- `Ctrl + R`: ค้นหาคำสั่ง (Searches for a command)
- `Ctrl + D`: ออกจาก terminal (Exits the terminal)

## คำสั่งคู่มือ (Manual Command)

ใน Linux และ Mac คำสั่ง `man` ใช้แสดง**คู่มือ**ของคำสั่งใดๆ ที่สามารถรันใน terminal ได้:
(On Linux and Mac, the `man` command shows the **manual** of any command:)

```bash
man ls
```

สำหรับ Windows ที่ใช้ Git Bash ไม่มีคำสั่ง `man` แต่สามารถใช้ `--help` แทนได้:
(For Windows using Git Bash, use `--help` instead:)

```bash
ls --help
```

ใช้ลูกศรหรือ page up/down เพื่อเลื่อน กด `q` เพื่อออก
(Use arrow keys or page up/down to navigate, press `q` to exit)

## คำสั่ง `whoami`

แสดงชื่อผู้ใช้ปัจจุบันที่ล็อกอินอยู่
(Shows the current logged-in user)

```bash
whoami
```

## คำสั่ง `date`

แสดงวันที่และเวลาปัจจุบัน
(Shows current date and time)

```bash
date
```

## การนำทางระบบไฟล์ (File System Navigation)

คำสั่งสำหรับนำทางระบบไฟล์มีความสำคัญมาก คุณจะใช้บ่อยมาก:
(File navigation commands are very important - you'll use them constantly:)

| คำสั่ง (Command)                    | คำอธิบาย (Description)                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| pwd                                 | แสดงเส้นทางของไดเรกทอรีปัจจุบัน (Lists path to working directory)                     |
| ls                                  | แสดงเนื้อหาในไดเรกทอรี (List directory contents)                                      |
| ls -a                               | แสดงรวมไฟล์ที่ซ่อนอยู่ (List including hidden files starting with dot)                |
| ls -l                               | แสดงข้อมูลเพิ่มเติมรวมสิทธิ์ (List with more info including permissions)              |
| ls -r                               | แสดงในลำดับย้อนกลับ (List in reverse order)                                            |
| cd                                  | เปลี่ยนไปยังไดเรกทอรีหลัก (Change to home directory)                                  |
| cd [dirname]                        | เปลี่ยนไปยังไดเรกทอรีที่ระบุ (Change to specific directory)                           |
| cd ~                                | เปลี่ยนไปยังไดเรกทอรีหลัก (Change to home directory)                                  |
| cd ..                               | เปลี่ยนไปยังไดเรกทอรีแม่ (Change to parent directory)                                 |
| cd -                                | เปลี่ยนไปยังไดเรกทอรีก่อนหน้า (Change to previous directory)                          |
| find [dirtosearch] -name [filename] | ค้นหาตำแหน่งของโปรแกรม (Find location of a program)                                   |

คุณสามารถรวม flags เข้าด้วยกันได้ เช่น `ls -la` สำหรับดูข้อมูลเพิ่มเติมและไฟล์ซ่อน
(You can combine flags together, like `ls -la` for detailed info and hidden files)

## การเปิดโฟลเดอร์หรือไฟล์ (Opening Folders or Files)

เปิดไฟล์หรือโฟลเดอร์ใน GUI จาก terminal:
(Open files or folders in GUI from terminal:)

- Mac: `open [dirname]`
- Windows: `start [dirname]`  
- Linux: `xdg-open [dirname]`

สามารถเปิดโฟลเดอร์ ไฟล์ และ URL ได้:
(Can open folders, files, and URLs:)

```bash
open https://traversymedia.com
```

## การแก้ไขไฟล์และไดเรกทอรี (Modifying Files & Directories)

| คำสั่ง (Command)                | คำอธิบาย (Description)                                                    |
| ------------------------------- | ------------------------------------------------------------------------- |
| mkdir [dirname]                 | สร้างไดเรกทอรี (Make directory)                                           |
| touch [filename]                | สร้างไฟล์ (Create file)                                                   |
| rm [filename]                   | ลบไฟล์ (Remove file)                                                      |
| rm -i [filename]                | ลบไฟล์แต่ถามก่อน (Remove file, but ask before)                            |
| rm -r [dirname]                 | ลบไดเรกทอรี (Remove directory)                                            |
| rm -rf [dirname]                | ลบไดเรกทอรีพร้อมเนื้อหา (Remove directory with contents)                  |
| rm ./\*                         | ลบทุกอย่างในโฟลเดอร์ปัจจุบัน (Remove everything in current folder)        |
| cp [filename] [dirname]         | คัดลอกไฟล์ (Copy file)                                                     |
| mv [filename] [dirname]         | ย้ายไฟล์ (Move file)                                                       |
| mv [dirname] [dirname]          | ย้ายไดเรกทอรี (Move directory)                                             |
| mv [filename] [filename]        | เปลี่ยนชื่อไฟล์หรือโฟลเดอร์ (Rename file or folder)                       |
| mv [filename] [filename] -v     | เปลี่ยนชื่อแบบแสดงรายละเอียด (Rename verbose - show source/destination)   |

สามารถใช้หลายคำสั่งพร้อมกันด้วย `&&`:
(You can run multiple commands at once with `&&`:)

```bash
cd test2 && mkdir test3
```

## สัญลักษณ์มุมฉาก > (Right angle bracket >)

บอกระบบให้ส่งผลลัพธ์ไปยังที่ที่ระบุ มักเป็นชื่อไฟล์:
(Tells system to output results to specified target, usually a filename:)

```bash
> [filename]
```

เสร็จแล้วกด `ctrl+D`
(When done, press `ctrl+D`)

## คำสั่ง `cat` (concatenate)

คำสั่งที่ใช้บ่อยมาก ช่วยสร้าง ดู รวมไฟล์ และเปลี่ยนทิศทางผลลัพธ์:
(Very common command for creating, viewing, concatenating files and redirecting output:)

แสดงเนื้อหาของไฟล์:
(Display file contents:)

```bash
cat [filename]
```

ดูเนื้อหาหลายไฟล์:
(View multiple files:)

```bash
cat [filename] [filename]
```

สร้างไฟล์ใหม่:
(Create new file:)

```bash
cat > [filename]
```

เพิ่มข้อมูลต่อท้ายไฟล์:
(Append to file:)

```bash
cat >> [filename]
```

แสดงพร้อมหมายเลขบรรทัด:
(Show with line numbers:)

```bash
cat -n [filename]
```

## คำสั่ง `less`

ใช้ดูเนื้อหาไฟล์และเลื่อนขึ้นลงได้:
(View file contents with scrolling capability:)

```bash
less [filename]
```

กด `q` เพื่อออก (Press `q` to exit)

## คำสั่ง `echo`

ใช้แสดงข้อความหรือสร้างและเขียนไฟล์:
(Display messages or create and write to files:)

```bash
echo "Hello World"
```

สร้างไฟล์:
(Create file:)

```bash
echo "Hello World" > [filename]
```

เพิ่มข้อมูลต่อท้าย:
(Append to file:)

```bash
echo "Hello World" >> [filename]
```

## คำสั่ง `nano`

โปรแกรมแก้ไขข้อความที่ใช้งานง่าย:
(Easy-to-use text editor:)

```bash
nano [filename]
```

กด `Ctrl + X` เพื่อออก แล้ว `Y` บันทึก หรือ `N` ไม่บันทึก
(Press `Ctrl + X` to exit, then `Y` to save or `N` to not save)

## คำสั่ง `head` และ `tail`

`head` แสดงส่วนต้นของไฟล์ (ค่าเริ่มต้น 10 บรรทัด):
(`head` shows first part of files - default 10 lines:)

```bash
head [filename]
head -n 5 [filename]  # แสดง 5 บรรทัดแรก (show first 5 lines)
```

`tail` แสดงส่วนท้ายของไฟล์:
(`tail` shows last part of files:)

```bash
tail [filename]
tail -n 5 [filename]  # แสดง 5 บรรทัดสุดท้าย (show last 5 lines)
```

## คำสั่ง `grep`

ใช้ค้นหาข้อความในไฟล์:
(Search for text patterns in files:)

```bash
grep [searchterm] [filename]
```

ค้นหาในหลายไฟล์:
(Search in multiple files:)

```bash
grep [searchterm] [filename] [filename]
```

## คำสั่ง `find`

ค้นหาตำแหน่งของไฟล์และไดเรกทอรีตามเงื่อนไข:
(Find location of files and directories based on conditions:)

สร้างไฟล์ 100 ไฟล์เพื่อทดลอง:
(Create 100 files for testing:)

```bash
touch file-{001..100}.txt
```

ค้นหาไฟล์เฉพาะ:
(Find specific file:)

```bash
find . -name "file-001.txt"
```

ค้นหาไฟล์ที่ตรงกับรูปแบบ:
(Find files matching pattern:)

```bash
find . -name "file-*"
```

ค้นหาไฟล์ว่าง:
(Find empty files:)

```bash
find . -empty
```

ลบไฟล์ที่ค้นพบ:
(Delete found files:)

```bash
find . -name "file-*" -delete
```

## การส่งต่อ (Piping)

เป็นวิธีเปลี่ยนทิศทางผลลัพธ์ไปยังปลายทางอื่น:
(Way of redirecting output to another destination:)

สร้างไฟล์ 10 ไฟล์:
(Create 10 files:)

```bash
touch file-{001..010}.txt
```

ส่งผลลัพธ์การค้นหาไปยังไฟล์ใหม่:
(Pipe search results to new file:)

```bash
find . -name "file-0*" > output.txt
cat output.txt  # ดูผลลัพธ์ (view results)
```

## การสร้าง Symlink

Symlink คือไฟล์พิเศษที่ชี้ไปยังไฟล์อื่น เหมือนทางลัด:
(Symlink is a special file that points to another file, like a shortcut:)

สร้าง symlink:
(Create symlink:)

```bash
ln -s [filename] [symlinkname]
```

ลบ symlink:
(Remove symlink:)

```bash
rm [symlinkname]
```

สำหรับ Windows (ไม่ใช่ Git Bash):
(For Windows without Git Bash:)

```bash
mklink [symlinkname] [filename]
```

## การบีบอัดไฟล์ (File Compression)

`tar` ใช้รวมไฟล์หลายไฟล์เป็นไฟล์เดียวที่เรียกว่า **tarball**:
(`tar` concatenates multiple files into one **tarball**:)

| คำสั่ง (Command)                    | คำอธิบาย (Description)                        |
| ----------------------------------- | --------------------------------------------- |
| tar czvf [dirname].tar.gz [dirname] | สร้าง tarball (Create tarball)               |
| tar tzvf [dirname]                  | ดูเนื้อหาใน tarball (See tarball contents)   |
| tar xzvf [dirname].tar.gz           | แยกไฟล์จาก tarball (Extract tarball)         |

ความหมายของ flags:
(Flag meanings:)
- -c: สร้าง archive (Creates archive)
- -x: แยกไฟล์ (Extract archive)
- -f: สร้างด้วยชื่อไฟล์ที่กำหนด (Creates with given filename)
- -t: แสดงไฟล์ใน archive (Lists files in archive)
- -v: แสดงข้อมูลละเอียด (Verbose information)
- -z: ใช้ gzip (Use gzip compression)

## คำสั่ง `history`

แสดงประวัติคำสั่งที่เคยใช้:
(Display command history:)

```bash
history
```

ใช้ `!` เพื่อรันคำสั่งจากประวัติ:
(Use `!` to run command from history:)

```bash
!100  # รันคำสั่งลำดับที่ 100 (run command #100)
```

---

## เคล็ดลับเพิ่มเติม (Additional Tips)

- ใช้ `Tab` บ่อยๆ เพื่อ auto-complete และประหยัดเวลา (Use `Tab` frequently for auto-complete)
- `Ctrl + C` เมื่อติดหรือต้องการยกเลิก (Use `Ctrl + C` when stuck or to cancel)
- `clear` หรือ `Ctrl + L` เพื่อล้างหน้าจอ (Clear screen)
- `history | grep [term]` เพื่อค้นหาคำสั่งในประวัติ (Search command history)

คำสั่งเหล่านี้เป็นพื้นฐานที่จำเป็นสำหรับการใช้งาน terminal ฝึกใช้บ่อยๆ จะทำให้คล่องแคล่วขึ้น!
(These are essential terminal commands. Practice frequently to become more fluent!)
