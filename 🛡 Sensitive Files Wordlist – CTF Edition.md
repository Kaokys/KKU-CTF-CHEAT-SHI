# 🛡 Sensitive Files Wordlist – CTF Edition

ใช้สำหรับสแกนหาไฟล์สำคัญบนเว็บเซิร์ฟเวอร์ที่อาจเปิดให้เข้าถึงได้ (Sensitive Files Disclosure / Information Disclosure)  
เหมาะสำหรับ CTF, Bug Bounty, และ Web Pentest

---

## 🔹 Web Server Config & Access Control
| Path | คำอธิบาย |
|------|-----------|
| `.htaccess` | ไฟล์ตั้งค่า Apache ต่อไดเรกทอรี เช่น rewrite, deny access |
| `.htpasswd` | ไฟล์เก็บ user/pass (hash) ของ Apache Basic Auth |
| `web.config` | ไฟล์ตั้งค่าเว็บ IIS |
| `nginx.conf` | ไฟล์ตั้งค่า Nginx |

---

## 🔹 Search Engine Hints
| Path | คำอธิบาย |
|------|-----------|
| `robots.txt` | ไฟล์บอก search engine ว่าห้ามหรืออนุญาต path ไหน |
| `sitemap.xml` | ไฟล์รวม URL ทั้งหมดของเว็บ |

---

## 🔹 Backup & Database Dumps
| Path | คำอธิบาย |
|------|-----------|
| `config.php.bak` | backup ของ config.php |
| `config.bak` | backup ของไฟล์ config |
| `db.sql` / `dump.sql` / `database.sql` | ไฟล์ dump ฐานข้อมูล |
| `backup.zip` / `backup.tar.gz` / `site-backup.zip` | backup ของเว็บทั้งเว็บ |

---

## 🔹 Logs & Debugging Files
| Path | คำอธิบาย |
|------|-----------|
| `error.log` | ไฟล์ log ข้อผิดพลาด |
| `access.log` | ไฟล์ log การเข้าถึง |
| `debug.log` | ไฟล์ log สำหรับ debug |
| `phpinfo.php` | ไฟล์แสดง config ของ PHP (เผยข้อมูลระบบ) |

---

## 🔹 Source Code & Credentials
| Path | คำอธิบาย |
|------|-----------|
| `config.php` | ไฟล์ config หลัก (มักมี DB credentials) |
| `wp-config.php` | WordPress config (DB, keys) |
| `.env` | Environment variables (API keys, passwords) |

---

## 🔹 Version Control Leftovers
| Path | คำอธิบาย |
|------|-----------|
| `.git/` | Git repo ของเว็บ |
| `.svn/` | Subversion repo |
| `.hg/` | Mercurial repo |
| `.DS_Store` | ไฟล์ index ของ macOS |

---

## 🔹 Temp & Old Files
| Path | คำอธิบาย |
|------|-----------|
| `index.php~` | ไฟล์ชั่วคราวของ editor |
| `index.php.save` | ไฟล์ save เก่า |
| `old.php` | ไฟล์เวอร์ชันเก่า |
| `test.php` | ไฟล์ทดสอบ |

---

## 💡 วิธีใช้กับ dirsearch
```bash
dirsearch -u http://target.com -w sensitive.md
