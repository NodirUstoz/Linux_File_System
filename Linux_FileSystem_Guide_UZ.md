# 🐧 Linux Fayl Tizimi va CTF Asoslari

<div align="center">

### Talabalar uchun o'quv qo'llanma 

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![CTF](https://img.shields.io/badge/CTF-FF6F00?style=for-the-badge&logo=hackthebox&logoColor=white)

</div>

---

| | |
|---|---|
| **👨‍🏫 Muallif** | Nodir Ustoz |
| **📱 Telegram** | [@Nodir_Odilov](https://t.me/Nodir_Odilov) |
| **📊 Daraja** | Boshlang'ich - O'rta |
| **🎯 Maqsad** | Linux fayl tizimini chuqur o'rganish va CTF musobaqalariga tayyorgarlik |
| **⏱ O'qish vaqti** | ~45 daqiqa |

---

> 💡 **Maslahat:** Bu qo'llanmani o'qib chiqqandan so'ng, [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) platformasida amaliy mashq qiling!

---

## 📚 Mundarija

1. [Kirish va asosiy tushunchalar](#1-kirish-va-asosiy-tushunchalar)
2. [Linux fayl tizimi arxitekturasi](#2-linux-fayl-tizimi-arxitekturasi)
3. [Navigatsiya va asosiy buyruqlar](#3-navigatsiya-va-asosiy-buyruqlar)
4. [Fayllarni qidirish usullari](#4-fayllarni-qidirish-usullari)
5. [Fayl mazmunini o'qish](#5-fayl-mazmunini-oqish)
6. [Huquqlar va ruxsatlar tizimi](#6-huquqlar-va-ruxsatlar-tizimi)
7. [Muhim kataloglar va ularning vazifalari](#7-muhim-kataloglar-va-ularning-vazifalari)
8. [Arxivlar bilan ishlash](#8-arxivlar-bilan-ishlash)
9. [Disk va mount operatsiyalari](#9-disk-va-mount-operatsiyalari)
10. [Simvolik havolalar (Symlinks)](#10-simvolik-havolalar-symlinks)
11. [Amaliy mashqlar](#11-amaliy-mashqlar)
12. [CTF strategiyalari](#12-ctf-strategiyalari)

---

## 1. Kirish va asosiy tushunchalar

### 1.1 Linux nima?

**Linux** - bu ochiq kodli operatsion tizim yadrosi (kernel) bo'lib, 1991-yilda Linus Torvalds tomonidan yaratilgan. Bugungi kunda Linux serverlar, superkompyuterlar, mobil qurilmalar (Android) va ko'plab boshqa sohalarda keng qo'llaniladi.

> **Qiziqarli fakt:** Dunyodagi 500 ta eng kuchli superkompyuterning 100% i Linux asosida ishlaydi.

### 1.2 CTF (Capture The Flag) nima?

**CTF** - bu kiberxavfsizlik bo'yicha musobaqalar bo'lib, ishtirokchilar turli xil topshiriqlarni yechib "flag" (bayroq) deb ataladigan maxfiy satrlarni topishadi.

**Flag formatlari:**
```
FLAG{bu_oddiy_flag}
CTF{capture_the_flag}
HTB{hack_the_box_flag}
flag{kichik_harflar}
```

### 1.3 Nima uchun Linux CLI (Command Line Interface)?

| Grafik interfeys (GUI) | Buyruq qatori (CLI) |
|------------------------|---------------------|
| Vizual, oson boshlash | Tez va samarali |
| Resurslarni ko'p sarflaydi | Minimal resurs |
| Avtomatlashtirish qiyin | Skriptlar bilan avtomatlashtirish oson |
| Server boshqaruvida cheklangan | Server boshqaruvida asosiy vosita |

### 1.4 Terminal va shell

**Terminal** - bu matn asosidagi interfeys oynasi.

**Shell** - bu buyruqlarni qayta ishlaydigan dastur. Mashhur shell'lar:
- **Bash** (Bourne Again Shell) - eng keng tarqalgan
- **Zsh** - zamonaviy imkoniyatlar bilan
- **Fish** - foydalanuvchiga qulay
- **sh** - asl Bourne shell

---

## 2. Linux fayl tizimi arxitekturasi

### 2.1 "Hamma narsa fayl" falsafasi

Linuxda hamma narsa fayl sifatida ifodalanadi:
- Oddiy fayllar (matn, binary)
- Kataloglar (directories)
- Qurilmalar (devices) - `/dev/sda`, `/dev/null`
- Processlar - `/proc/` ichida
- Tarmoq soketlari

### 2.2 Fayl tizimi ierarxiyasi (FHS)

```
/                      # Root - barcha fayllarning boshlanishi
├── bin/               # Asosiy buyruqlar (ls, cp, mv)
├── boot/              # Yuklash fayllari
├── dev/               # Qurilma fayllari
├── etc/               # Konfiguratsiya fayllari
├── home/              # Foydalanuvchilar kataloglari
│   ├── student/
│   └── admin/
├── lib/               # Kutubxonalar
├── media/             # Olinadigan qurilmalar
├── mnt/               # Vaqtinchalik mount nuqtalari
├── opt/               # Qo'shimcha dasturlar
├── proc/              # Process ma'lumotlari (virtual)
├── root/              # Root foydalanuvchi uy katalogi
├── run/               # Runtime ma'lumotlar
├── sbin/              # Tizim buyruqlari
├── srv/               # Xizmat ma'lumotlari
├── sys/               # Yadro ma'lumotlari (virtual)
├── tmp/               # Vaqtinchalik fayllar
├── usr/               # Foydalanuvchi dasturlari
│   ├── bin/
│   ├── lib/
│   ├── local/
│   └── share/
└── var/               # O'zgaruvchan ma'lumotlar
    ├── log/
    ├── www/
    └── backups/
```

### 2.3 tree buyrug'i - strukturani vizualizatsiya qilish

`tree` buyrug'i katalog strukturasini daraxt ko'rinishida ko'rsatadi:

```bash
tree                           # Joriy katalogni ko'rsat
tree -a                        # Yashirin fayllar ham
tree -L 2                      # Faqat 2 daraja chuqurlik
tree -d                        # Faqat kataloglar
tree -f                        # To'liq yo'llarni ko'rsat
tree -h                        # Fayl hajmlari (human-readable)
tree -p                        # Ruxsatlarni ko'rsat
tree --dirsfirst               # Kataloglarni birinchi
tree -I "node_modules|.git"    # Patternlarni chiqarib tashlash
```

**Foydalanish misollari:**
```bash
# Loyiha strukturasi hajmlar bilan
tree -L 3 -h --dirsfirst

# Faqat Python fayllar
tree -P "*.py"

# Muayyan kataloglarni chiqarib tashlash
tree -I "__pycache__|*.pyc"

# Faylga saqlash
tree > structure.txt

# Ranglar bilan (agar qo'llab-quvvatlansa)
tree -C
```

**Natija misoli:**
```
.
├── src/
│   ├── main.py
│   ├── utils/
│   │   ├── helper.py
│   │   └── config.py
│   └── tests/
│       └── test_main.py
├── docs/
│   └── README.md
├── requirements.txt
└── setup.py

4 directories, 6 files
```

**tree o'rnatish:**
```bash
# Debian/Ubuntu
sudo apt install tree

# CentOS/RHEL
sudo yum install tree

# macOS
brew install tree

# Windows (Git Bash yoki WSL bilan)
# Git Bash'da odatda oldindan o'rnatilgan
```

### 2.4 Fayl turlari

Linux'da 7 xil fayl turi mavjud:

| Belgi | Turi | Tavsif |
|-------|------|--------|
| `-` | Oddiy fayl | Matn, binary, rasm va h.k. |
| `d` | Katalog | Boshqa fayllarni o'z ichiga oladi |
| `l` | Simvolik havola | Boshqa faylga ko'rsatkich |
| `c` | Belgilik qurilma | Terminallar, klaviatura |
| `b` | Blokli qurilma | Disklar, USB |
| `p` | Named pipe (FIFO) | Processlar aloqasi uchun |
| `s` | Socket | Tarmoq aloqasi |

**Misollar:**
```bash
ls -la
# Natija:
# drwxr-xr-x  5 user group  4096 Jan 10 10:00 katalog/
# -rw-r--r--  1 user group  1234 Jan 10 09:30 fayl.txt
# lrwxrwxrwx  1 user group    15 Jan 10 08:00 havola -> /path/to/target
```

### 2.4 Inode tushunchasi

**Inode** - bu faylning metama'lumotlarini saqlaydigan ma'lumotlar strukturasi.

Inode o'z ichiga oladi:
- Fayl turi va ruxsatlar
- Egasi (owner) va guruh (group)
- Fayl hajmi
- Vaqt tamg'alari (timestamps)
- Ma'lumotlar bloklariga ko'rsatkichlar

```bash
# Inode raqamlarini ko'rish
ls -li

# Ma'lum inode haqida ma'lumot
stat fayl.txt
```

> **Muhim:** Fayl nomi inode'da emas, katalog yozuvida saqlanadi. Shuning uchun bir faylga bir nechta nom (hard link) bo'lishi mumkin.

---

## 3. Navigatsiya va asosiy buyruqlar

### 3.1 pwd - print working directory

Joriy katalogni ko'rsatadi:

```bash
pwd
# Natija: /home/student
```

**Qanday ishlaydi?** Shell har doim joriy katalog yo'lini `PWD` environment variable'da saqlaydi.

### 3.2 cd - change directory

Kataloglar o'rtasida harakatlanish:

```bash
cd /home           # Absolyut yo'l bilan o'tish
cd katalog         # Joriy katalogdagi papkaga o'tish
cd ..              # Bir daraja yuqoriga (parent directory)
cd ../..           # Ikki daraja yuqoriga
cd ~               # Uy katalogiga (home directory)
cd -               # Oldingi katalogga qaytish
cd                 # Uy katalogiga (cd ~ bilan bir xil)
```

**Yo'l turlari:**
- **Absolyut yo'l:** `/` dan boshlanadi, to'liq adres: `/home/student/documents`
- **Nisbiy yo'l:** Joriy katalogga nisbatan: `./documents` yoki `../parent`

### 3.3 ls - list directory contents

Katalog tarkibini ko'rsatish:

```bash
ls                 # Oddiy ro'yxat
ls -l              # Batafsil (long format)
ls -a              # Barcha fayllar (yashirin ham)
ls -la             # Batafsil + barcha (ENG KO'P ISHLATILADIGAN)
ls -lh             # Human-readable hajmlar (KB, MB, GB)
ls -lt             # Vaqt bo'yicha tartiblangan
ls -lS             # Hajm bo'yicha tartiblangan
ls -lR             # Rekursiv (ichki kataloglar ham)
ls -li             # Inode raqamlari bilan
```

**ls -la natijasini o'qish:**
```
drwxr-xr-x  2 student group  4096 Jan 10 10:00 documents
│├─┼─┼─┼─│  │ │       │      │    │            │
││ │ │ │ │  │ │       │      │    │            └── Fayl nomi
││ │ │ │ │  │ │       │      │    └── O'zgartirish sanasi
││ │ │ │ │  │ │       │      └── Fayl hajmi (bayt)
││ │ │ │ │  │ │       └── Guruh nomi
││ │ │ │ │  │ └── Egasi (owner)
││ │ │ │ │  └── Hard link soni
││ │ │ │ └── Boshqalar uchun ruxsatlar (others)
││ │ │ └── Guruh uchun ruxsatlar
││ │ └── Egasi uchun ruxsatlar
││ └── Fayl turi
│└── SUID/SGID/Sticky bit
└── Fayl turi (d=directory, -=file, l=link)
```

### 3.4 whoami va id

Foydalanuvchi haqida ma'lumot:

```bash
whoami             # Joriy foydalanuvchi nomi
# Natija: student

id                 # To'liq identifikatsiya
# Natija: uid=1000(student) gid=1000(student) groups=1000(student),27(sudo)
```

**Terminlar:**
- **UID** (User ID) - foydalanuvchi raqami
- **GID** (Group ID) - asosiy guruh raqami
- **Groups** - foydalanuvchi a'zo bo'lgan barcha guruhlar

### 3.5 uname - system information

```bash
uname -a           # Barcha tizim ma'lumotlari
uname -r           # Kernel versiyasi
uname -m           # Arxitektura (x86_64)
uname -n           # Hostname
```

---

## 4. Fayllarni qidirish usullari

### 4.1 find - kuchli qidiruv vositasi

`find` - bu Linux'dagi eng kuchli qidiruv buyrug'i.

**Asosiy sintaksis:**
```bash
find [qayerda] [shartlar] [harakat]
```

#### Nom bo'yicha qidirish:

```bash
find / -name "flag.txt"                    # Aniq nom
find / -name "*.txt"                       # Kengaytma bo'yicha
find / -name "flag*"                       # flag bilan boshlanadi
find / -name "*secret*"                    # O'rtasida secret bor
find / -iname "FLAG.txt"                   # Case-insensitive
```

#### Tur bo'yicha qidirish:

```bash
find / -type f                             # Faqat fayllar (file)
find / -type d                             # Faqat kataloglar (directory)
find / -type l                             # Faqat simvolik havolalar (link)
```

#### Hajm bo'yicha qidirish:

```bash
find / -size +1M                           # 1 MB dan katta
find / -size -100c                         # 100 baytdan kichik (c=bytes)
find / -size +1k -size -10k                # 1KB va 10KB orasida
```

**Hajm birliklarari:**
- `c` - bayt
- `k` - kilobayt
- `M` - megabayt
- `G` - gigabayt

#### Vaqt bo'yicha qidirish:

```bash
find / -mtime -1                           # Oxirgi 24 soatda o'zgargan
find / -mtime +7                           # 7 kundan oldin o'zgargan
find / -mmin -30                           # Oxirgi 30 daqiqada o'zgargan
find / -atime -1                           # Oxirgi 24 soatda ochilgan
find / -newer reference.txt                # Reference fayldan keyin
```

**Vaqt parametrlari:**
- `mtime` - modification time (o'zgartirish)
- `atime` - access time (ochish)
- `ctime` - change time (metadata o'zgarishi)

#### Ruxsatlar bo'yicha qidirish:

```bash
find / -perm 644                           # Aniq 644 ruxsat
find / -perm -4000                         # SUID bit o'rnatilgan
find / -perm -2000                         # SGID bit o'rnatilgan
find / -readable                           # O'qish mumkin bo'lganlar
find / -writable                           # Yozish mumkin bo'lganlar
find / -executable                         # Bajarish mumkin bo'lganlar
```

#### Egasi bo'yicha qidirish:

```bash
find / -user root                          # Root egasi
find / -group admin                        # Admin guruhiga tegishli
find / -uid 1000                           # UID 1000 ga tegishli
find / -nouser                             # Egasi yo'q fayllar
```

#### find bilan harakat bajarish:

```bash
find / -name "*.txt" -exec cat {} \;       # Har bir faylni o'qi
find / -name "*.log" -exec rm {} \;        # Har bir faylni o'chir
find / -name "*.sh" -exec chmod +x {} \;   # Ruxsat o'zgartir
find / -name "flag*" -exec ls -la {} \;    # Batafsil ko'rsat
```

> **Eslatma:** `{}` - topilgan fayl o'rnida, `\;` - buyruq oxiri

#### Xatolarni yashirish:

```bash
find / -name "flag*" 2>/dev/null           # Xato xabarlarni yashir
```

`2>/dev/null` - bu STDERR (xato oqimi) ni `/dev/null` ga yo'naltiradi, ya'ni xato xabarlar ko'rinmaydi.

### 4.2 locate - tez qidiruv

`locate` ma'lumotlar bazasidan qidiradi, shuning uchun `find`dan tezroq:

```bash
locate flag                                # flag so'zini qidir
locate -i flag                             # Case-insensitive
locate "*.txt"                             # Pattern bilan
```

**Kamchiligi:** Ma'lumotlar bazasi (`/var/lib/mlocate/mlocate.db`) eskirgan bo'lishi mumkin.

```bash
sudo updatedb                              # Bazani yangilash
```

### 4.3 which, whereis, type

Buyruq joylashuvini topish:

```bash
which python                               # Executable yo'li
# Natija: /usr/bin/python

whereis python                             # Binary, source, man sahifalari
# Natija: python: /usr/bin/python /usr/lib/python3.10 /usr/share/man/man1/python.1.gz

type ls                                    # Buyruq turi
# Natija: ls is aliased to 'ls --color=auto'
```

### 4.4 Globbing (wildcard patterns)

Shell'ning pattern matching imkoniyati:

| Pattern | Ma'nosi | Misol |
|---------|---------|-------|
| `*` | Har qanday belgilar | `*.txt` - barcha txt fayllar |
| `?` | Bitta belgi | `file?.txt` - file1.txt, fileA.txt |
| `[abc]` | a, b yoki c | `file[123].txt` |
| `[a-z]` | a dan z gacha | `file[a-z].txt` |
| `[!abc]` | a, b, c dan boshqa | `file[!0-9].txt` |
| `{a,b}` | a yoki b | `file.{txt,md}` |

**Misollar:**
```bash
ls *.txt                                   # Barcha txt fayllar
ls file?.log                               # file1.log, file2.log
ls /home/*/flag*                           # Barcha userlarda flag
cat /home/*/.bash*                         # Barcha bash konfiglari
```

---

## 5. Fayl mazmunini o'qish

### 5.1 cat - Concatenate

Fayl mazmunini to'liq chiqarish:

```bash
cat file.txt                               # Faylni o'qi
cat -n file.txt                            # Qator raqamlari bilan
cat -A file.txt                            # Maxsus belgilarni ko'rsat
cat file1.txt file2.txt                    # Ikki faylni birlashtir
cat file1.txt file2.txt > combined.txt     # Birlashtirib saqlash
```

### 5.2 less va more - Sahifalash

Katta fayllarni o'qish uchun:

```bash
less file.txt                              # Zamonaviy sahifalagich
more file.txt                              # Oddiy sahifalagich
```

**less navigatsiyasi:**
- `Space` yoki `f` - keyingi sahifa
- `b` - oldingi sahifa
- `g` - fayl boshiga
- `G` - fayl oxiriga
- `/pattern` - qidirish
- `n` - keyingi natija
- `q` - chiqish

### 5.3 head va tail

Fayl boshi va oxirini o'qish:

```bash
head file.txt                              # Birinchi 10 qator
head -n 5 file.txt                         # Birinchi 5 qator
head -c 100 file.txt                       # Birinchi 100 bayt

tail file.txt                              # Oxirgi 10 qator
tail -n 20 file.txt                        # Oxirgi 20 qator
tail -f logfile.log                        # Real-time kuzatish
```

### 5.4 strings - binary dan matn

Binary fayllardan o'qilishi mumkin bo'lgan matnni chiqarish:

```bash
strings binary_file                        # Barcha matnlar
strings -n 10 binary_file                  # 10+ belgili satrlar
strings binary_file | grep flag            # Flag qidirish
strings binary_file | grep -E "[A-Z]{3,}\{.*\}"   # Flag formati
```

> **CTF'da juda muhim!** Ko'p flaglar binary fayllar ichida yashirilgan bo'ladi.

### 5.5 file - fayl turini aniqlash

```bash
file mystery                               # Fayl turini aniqla
file *                                     # Barcha fayllarni tekshir
file -i file.txt                           # MIME type
```

**Natija misollari:**
```
mystery: ELF 64-bit LSB executable
archive: gzip compressed data
image: PNG image data
script: Bourne-Again shell script
```

### 5.6 xxd va hexdump - hex ko'rinish

Faylni hex formatida ko'rish:

```bash
xxd file.bin                               # Hex dump
xxd -l 100 file.bin                        # Faqat 100 bayt
xxd -r hex.txt > binary                    # Hex dan binary ga

hexdump -C file.bin                        # Canonical format
```

---

## 6. Yordam olish - --help, man, info

> 🎯 **CTF uchun muhim:** Har qanday buyruqni to'liq tushunish uchun avval uning yordamini o'qing. Ko'p CTF challenjelarda yechim noma'lum flag yoki opsiyadir!

### 6.1 --help flagi

Deyarli barcha Linux buyruqlari `--help` yoki `-h` flagini qo'llab-quvvatlaydi:

```bash
# Asosiy sintaksis
buyruq --help
buyruq -h

# Misollar
ls --help                                  # ls haqida qisqa yordam
grep --help                                # grep barcha opsiyalari
find --help                                # find sintaksisi
chmod --help                               # chmod opsiyalari
cat --help                                 # cat haqida ma'lumot
```

**--help natijasi tuzilishi:**

```
Usage: ls [OPTION]... [FILE]...        ← Sintaksis
List information about the FILEs       ← Tavsif

Mandatory arguments to long options... ← Izoh

  -a, --all             do not ignore entries starting with .
  │    │                └── Tavsif
  │    └── Uzun format (--all)
  └── Qisqa format (-a)

  -l                    use a long listing format
  -h, --human-readable  with -l, print sizes like 1K 234M 2G
  -R, --recursive       list subdirectories recursively
  -S                    sort by file size, largest first
  -t                    sort by time, newest first
  -r, --reverse         reverse order while sorting

Exit status:           ← Exit kodlari
 0  if OK,
 1  if minor problems,
 2  if serious trouble.
```

### 6.2 man (manual) sahifalari

`man` - eng to'liq va batafsil hujjatlarni ko'rsatadi:

```bash
# Asosiy sintaksis
man buyruq

# Misollar
man ls                                     # ls to'liq hujjati
man grep                                   # grep manual
man find                                   # find batafsil
man chmod                                  # chmod haqida
man bash                                   # Bash haqida (juda uzun!)
```

**man ichida navigatsiya:**

| Tugma | Vazifa |
|-------|--------|
| `Space` / `PgDn` | Keyingi sahifa |
| `b` / `PgUp` | Oldingi sahifa |
| `Enter` / `↓` | Bir qator pastga |
| `k` / `↑` | Bir qator yuqoriga |
| `/pattern` | Pattern qidirish (oldinga) |
| `?pattern` | Pattern qidirish (orqaga) |
| `n` | Keyingi natija |
| `N` | Oldingi natija |
| `g` | Boshiga o'tish |
| `G` | Oxiriga o'tish |
| `q` | Chiqish |
| `h` | man ichida yordam |

**man bo'limlari (sections):**

```bash
# man sahifalari 9 ta bo'limga bo'lingan
man 1 passwd                               # passwd buyrug'i (user commands)
man 5 passwd                               # /etc/passwd fayl formati (file formats)

# Bo'limlar:
# 1 - User commands (foydalanuvchi buyruqlari)
# 2 - System calls (tizim chaqiruvlari)
# 3 - Library functions (kutubxona funksiyalari)
# 4 - Special files (maxsus fayllar, /dev)
# 5 - File formats (fayl formatlari, config)
# 6 - Games (o'yinlar)
# 7 - Miscellaneous (turli xil)
# 8 - System admin commands (admin buyruqlari)
# 9 - Kernel routines (kernel)

# Barcha bo'limlarda qidirish
man -a passwd                              # Barcha passwd manlari
man -k password                            # password bilan bog'liq manlar
man -f passwd                              # passwd haqida qisqa (whatis)
```

**man sahifasi tuzilishi:**

```
NAME         - Buyruq nomi va qisqa tavsif
SYNOPSIS     - Sintaksis (qanday ishlatiladi)
DESCRIPTION  - Batafsil tavsif
OPTIONS      - Barcha opsiyalar ro'yxati
EXAMPLES     - Ishlatish misollari
FILES        - Bog'liq fayllar
SEE ALSO     - Bog'liq buyruqlar
BUGS         - Ma'lum xatolar
AUTHOR       - Muallif
```

### 6.3 info sahifalari

Ba'zi buyruqlar uchun `info` yanada batafsil hujjat beradi:

```bash
info coreutils                             # GNU core utilities haqida
info grep                                  # grep batafsil
info find                                  # find batafsil
info bash                                  # Bash to'liq hujjati
```

**info navigatsiya:**

| Tugma | Vazifa |
|-------|--------|
| `n` | Keyingi node |
| `p` | Oldingi node |
| `u` | Yuqori darajaga |
| `Enter` | Linkni ochish |
| `l` | Oxirgi nodega qaytish |
| `Space` | Keyingi sahifa |
| `Del` | Oldingi sahifa |
| `/` | Qidirish |
| `q` | Chiqish |

### 6.4 Qo'shimcha yordam usullari

```bash
# whatis - qisqa tavsif
whatis ls                                  # ls(1) - list directory contents
whatis grep                                # grep(1) - print lines matching a pattern

# apropos - kalit so'z bo'yicha qidirish
apropos "copy file"                        # Fayl nusxalash buyruqlari
apropos permission                         # Ruxsatlar bilan bog'liq
apropos search                             # Qidirish buyruqlari

# type - buyruq turi
type ls                                    # ls is aliased to 'ls --color=auto'
type cd                                    # cd is a shell builtin
type grep                                  # grep is /usr/bin/grep

# which - buyruq joylashuvi
which python                               # /usr/bin/python
which grep                                 # /usr/bin/grep

# whereis - buyruq, man va source joylashuvi
whereis ls                                 # ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz

# command -v - buyruq mavjudligini tekshirish
command -v git && echo "Git o'rnatilgan"
```

### 6.5 Yordam olish strategiyalari

**1. Yangi buyruq bilan tanishish tartibi:**

```bash
# 1-qadam: Qisqa yordam
command --help | head -30

# 2-qadam: whatis bilan tavsif
whatis command

# 3-qadam: Manual o'qish
man command

# 4-qadam: Misollar qidirish
man command | grep -A5 "EXAMPLE"
```

**2. CTF da foydali:**

```bash
# Buyruqdagi barcha flaglarni ko'rish
grep --help | grep -E "^\s+-"

# Faqat qisqa opsiyalar
ls --help 2>&1 | grep -oE "\s-[a-zA-Z]" | sort -u

# man dan faqat OPTIONS
man grep | sed -n '/^OPTIONS/,/^[A-Z]/p'

# Muayyan flag haqida
man find | grep -A3 "\-perm"
```

**3. Offline yordam:**

```bash
# man sahifalarini faylga saqlash
man ls > ls_manual.txt
man grep | col -b > grep_manual.txt       # Formatlashsiz

# Barcha manlarni qidirish
man -w ls                                  # /usr/share/man/man1/ls.1.gz
zcat /usr/share/man/man1/ls.1.gz          # man faylini o'qish
```

> 💡 **Maslahat:** CTF da notanish buyruqni ko'rsangiz, darhol `--help` va `man` bilan tanishing. Ko'pincha yechim maxsus flagda yashiringan bo'ladi!

---

## 7. Matn muharrirlari - nano va vim

> 🎯 **CTF uchun muhim:** Ko'p serverlarda faqat terminal muharrirlari mavjud. `nano` va `vim` ni bilish CTF va real serverlar bilan ishlashda zarur!

### 11.1 nano - oddiy muharrir

`nano` - boshlang'ich foydalanuvchilar uchun eng qulay terminal muharriri.

#### 11.1.1 nano ni ishga tushirish

```bash
nano                                       # Bo'sh fayl ochish
nano file.txt                              # Mavjud/yangi fayl ochish
nano +10 file.txt                          # 10-qatordan ochish
nano +10,5 file.txt                        # 10-qator, 5-ustundan
nano -l file.txt                           # Qator raqamlari bilan
nano -m file.txt                           # Mouse qo'llab-quvvatlash
nano -i file.txt                           # Auto-indent
nano -E file.txt                           # Tab -> spaces
nano -c file.txt                           # Cursor pozitsiyasini ko'rsatish
nano -w file.txt                           # Uzun qatorlarni wrap qilmaslik
```

#### 11.1.2 nano asosiy buyruqlari

**nano da `^` belgisi = Ctrl, `M-` = Alt**

| Kombinatsiya | Vazifa |
|--------------|--------|
| `^G` (Ctrl+G) | Yordam ko'rsatish |
| `^X` (Ctrl+X) | Chiqish (saqlashni so'raydi) |
| `^O` (Ctrl+O) | Saqlash (WriteOut) |
| `^R` (Ctrl+R) | Fayl qo'shish (Read file) |
| `^W` (Ctrl+W) | Qidirish (Where is) |
| `^\` (Ctrl+\) | Qidirish va almashtirish |
| `^K` (Ctrl+K) | Qatorni kesish (Cut) |
| `^U` (Ctrl+U) | Qo'yish (Paste/Uncut) |
| `^J` (Ctrl+J) | Paragrafni tekislash |
| `^C` (Ctrl+C) | Cursor pozitsiyasi |
| `^_` (Ctrl+_) | Qator/ustun raqamiga o'tish |
| `^T` (Ctrl+T) | Spell checker / Execute |

**Navigatsiya:**

| Kombinatsiya | Vazifa |
|--------------|--------|
| `^A` | Qator boshiga |
| `^E` | Qator oxiriga |
| `^Y` | Oldingi sahifa (Page Up) |
| `^V` | Keyingi sahifa (Page Down) |
| `M-\` (Alt+\) | Fayl boshiga |
| `M-/` (Alt+/) | Fayl oxiriga |
| `^Space` | Bir so'z oldinga |
| `M-Space` | Bir so'z orqaga |
| `^↑` | Oldingi blokga |
| `^↓` | Keyingi blokga |

**Tahrirlash:**

| Kombinatsiya | Vazifa |
|--------------|--------|
| `^K` | Qatorni kesish |
| `^U` | Kesishni qo'yish |
| `M-6` (Alt+6) | Qatorni nusxalash |
| `^6` (Ctrl+6) | Belgilashni boshlash (Mark) |
| `M-A` (Alt+A) | Belgilashni boshlash |
| `^D` | Bir belgi o'chirish (Delete) |
| `^H` | Orqaga o'chirish (Backspace) |
| `M-Del` | So'zni o'chirish |
| `M-T` | Qatordan oxirigacha kesish |
| `M-U` | Bekor qilish (Undo) |
| `M-E` | Qayta qilish (Redo) |

#### 11.1.3 nano qidirish va almashtirish

```bash
# Qidirish (Ctrl+W)
^W → pattern kiriting → Enter

# Qidirish opsiyalari (Ctrl+W ichida):
M-C    Case sensitive yoqish/o'chirish
M-R    Regex yoqish
M-B    Orqaga qidirish
^R     Faylda qidirish
^W     Keyingi natija

# Almashtirish (Ctrl+\)
^\ → qidiriluvchi → Enter → almashtiriluvchi → Enter
Y      Bittasini almashtirish
A      Hammasini almashtirish
N      O'tkazib yuborish
^C     Bekor qilish
```

#### 11.1.4 nano konfiguratsiya

```bash
# ~/.nanorc yoki /etc/nanorc
set linenumbers                            # Qator raqamlari
set autoindent                             # Auto-indent
set tabsize 4                              # Tab o'lchami
set tabstospaces                           # Tab -> spaces
set mouse                                  # Mouse qo'llab-quvvatlash
set smooth                                 # Smooth scroll
set constantshow                           # Cursor pozitsiyasi
set backup                                 # Backup yaratish
set nohelp                                 # Pastdagi yordamni yashirish

# Sintaksis highlighting
include "/usr/share/nano/*.nanorc"
include "/usr/share/nano/python.nanorc"
include "/usr/share/nano/sh.nanorc"
```

---

### 11.2 vim - kuchli muharrir

`vim` (Vi IMproved) - professional muharrir, o'rganish qiyin, lekin juda kuchli.

#### 11.2.1 vim rejimlari (modes)

**vim 4 ta asosiy rejimda ishlaydi:**

```
┌─────────────────────────────────────────────────────────────┐
│                      NORMAL MODE                             │
│                    (default rejim)                           │
│                                                              │
│    ┌──────────┐    ┌──────────┐    ┌──────────┐             │
│    │    i     │    │    v     │    │    :     │             │
│    │    I     │    │    V     │    │          │             │
│    │    a     │    │   ^V     │    │          │             │
│    │    A     │    │          │    │          │             │
│    │    o     │    │          │    │          │             │
│    │    O     │    │          │    │          │             │
│    ▼          │    ▼          │    ▼          │             │
│ ┌──────────┐  │ ┌──────────┐  │ ┌──────────┐  │             │
│ │  INSERT  │  │ │  VISUAL  │  │ │ COMMAND  │  │             │
│ │   MODE   │  │ │   MODE   │  │ │   MODE   │  │             │
│ └──────────┘  │ └──────────┘  │ └──────────┘  │             │
│    │          │    │          │    │          │             │
│    │ Esc      │    │ Esc      │    │ Enter    │             │
│    └──────────┼────┴──────────┼────┴──────────┘             │
│               │               │                              │
│               └───────────────┘                              │
│                    ▲                                         │
│                    │                                         │
│              Return to Normal                                │
└─────────────────────────────────────────────────────────────┘
```

| Rejim | Kirish | Chiqish | Vazifa |
|-------|--------|---------|--------|
| **Normal** | Esc | - | Navigatsiya, buyruqlar |
| **Insert** | i, I, a, A, o, O | Esc | Matn yozish |
| **Visual** | v, V, Ctrl+v | Esc | Matn belgilash |
| **Command** | : | Enter/Esc | Ex buyruqlari |

#### 11.2.2 vim ni ishga tushirish

```bash
vim                                        # Bo'sh fayl
vim file.txt                               # Fayl ochish
vim +10 file.txt                           # 10-qatorga o'tish
vim +/pattern file.txt                     # Pattern joylashuviga
vim -O file1 file2                         # Vertikal split
vim -o file1 file2                         # Horizontal split
vim -p file1 file2                         # Tablar bilan
vim -R file.txt                            # Faqat o'qish (readonly)
vim -d file1 file2                         # Diff mode
vim -c "set number" file.txt               # Buyruq bilan
vim +$ file.txt                            # Fayl oxiriga
```

#### 11.2.3 Normal mode buyruqlari

**Navigatsiya:**

| Buyruq | Vazifa |
|--------|--------|
| `h` `j` `k` `l` | ← ↓ ↑ → (yoki arrow keys) |
| `w` | Keyingi so'z boshiga |
| `W` | Keyingi SO'Z boshiga (bo'shliqdan keyin) |
| `b` | Oldingi so'z boshiga |
| `B` | Oldingi SO'Z boshiga |
| `e` | So'z oxiriga |
| `E` | SO'Z oxiriga |
| `0` | Qator boshiga |
| `^` | Birinchi belgiga (bo'sh emasga) |
| `$` | Qator oxiriga |
| `gg` | Fayl boshiga |
| `G` | Fayl oxiriga |
| `10G` | 10-qatorga |
| `:10` | 10-qatorga |
| `{` | Paragraf yuqorisiga |
| `}` | Paragraf pastiga |
| `%` | Juft qavsga o'tish |
| `H` | Ekran yuqorisiga (High) |
| `M` | Ekran o'rtasiga (Middle) |
| `L` | Ekran pastiga (Low) |
| `Ctrl+f` | Sahifa pastga (Forward) |
| `Ctrl+b` | Sahifa yuqoriga (Back) |
| `Ctrl+d` | Yarim sahifa pastga |
| `Ctrl+u` | Yarim sahifa yuqoriga |
| `zz` | Joriy qatorni markazga |
| `zt` | Joriy qatorni tepaga |
| `zb` | Joriy qatorni pastga |

**Tahrirlash (Normal modeda):**

| Buyruq | Vazifa |
|--------|--------|
| `x` | Bir belgi o'chirish |
| `X` | Oldingi belgi o'chirish |
| `dd` | Qatorni o'chirish |
| `dw` | So'zni o'chirish |
| `d$` yoki `D` | Qator oxirigacha o'chirish |
| `d0` | Qator boshigacha o'chirish |
| `dG` | Fayl oxirigacha o'chirish |
| `dgg` | Fayl boshigacha o'chirish |
| `yy` | Qatorni nusxalash (yank) |
| `yw` | So'zni nusxalash |
| `y$` | Qator oxirigacha nusxalash |
| `p` | Cursordan keyin qo'yish |
| `P` | Cursordan oldin qo'yish |
| `r` | Bir belgini almashtirish |
| `R` | Replace mode (overwrite) |
| `cc` | Qatorni o'chirib insert mode |
| `cw` | So'zni o'chirib insert mode |
| `c$` yoki `C` | Qator oxirigacha o'chirib insert |
| `~` | Case almashtirish (a↔A) |
| `u` | Bekor qilish (Undo) |
| `Ctrl+r` | Qayta qilish (Redo) |
| `.` | Oxirgi buyruqni takrorlash |
| `J` | Qatorlarni birlashtirish |
| `>>` | Indent (o'ngga surish) |
| `<<` | Dedent (chapga surish) |

**Insert modega kirish:**

| Buyruq | Vazifa |
|--------|--------|
| `i` | Cursordan oldin insert |
| `I` | Qator boshida insert |
| `a` | Cursordan keyin insert (append) |
| `A` | Qator oxirida insert |
| `o` | Pastda yangi qator ochib insert |
| `O` | Tepada yangi qator ochib insert |
| `s` | Belgini o'chirib insert |
| `S` | Qatorni o'chirib insert |

#### 11.2.4 Visual mode

```bash
v                                          # Character-wise visual
V                                          # Line-wise visual
Ctrl+v                                     # Block visual (ustunlar)

# Visual modeda:
d                                          # Belgilanganni o'chirish
y                                          # Belgilanganni nusxalash
c                                          # Belgilanganni o'chirib insert
>                                          # Indent
<                                          # Dedent
~                                          # Case almashtirish
u                                          # Kichik harfga
U                                          # Katta harfga
```

#### 11.2.5 Command mode (:)

**Saqlash va chiqish:**

```bash
:w                                         # Saqlash
:w filename                                # Boshqa nom bilan saqlash
:w!                                        # Majburiy saqlash
:q                                         # Chiqish
:q!                                        # Saqlamasdan chiqish
:wq                                        # Saqlash va chiqish
:wq!                                       # Majburiy saqlash va chiqish
:x                                         # :wq kabi
ZZ                                         # :wq kabi (Normal modeda)
ZQ                                         # :q! kabi (Normal modeda)
:e filename                                # Boshqa fayl ochish
:e!                                        # Faylni qayta yuklash
```

**Qidirish va almashtirish:**

```bash
/pattern                                   # Oldinga qidirish
?pattern                                   # Orqaga qidirish
n                                          # Keyingi natija
N                                          # Oldingi natija
*                                          # Cursor ostidagi so'zni oldinga qidirish
#                                          # Cursor ostidagi so'zni orqaga qidirish

# Almashtirish
:s/old/new/                                # Joriy qatorda birinchisini
:s/old/new/g                               # Joriy qatorda hammasini
:%s/old/new/g                              # Butun faylda hammasini
:%s/old/new/gc                             # Tasdiqlash bilan
:5,10s/old/new/g                           # 5-10 qatorlarda
:'<,'>s/old/new/g                          # Visual selection da

# Flaglar:
# g - global (barcha occurrences)
# c - confirm (tasdiqlash)
# i - case insensitive
# I - case sensitive
```

**Fayllar bilan ishlash:**

```bash
:split                                     # Horizontal split
:vsplit                                    # Vertical split
:sp filename                               # Split bilan fayl
:vsp filename                              # Vertical split bilan fayl
Ctrl+w w                                   # Oynalar orasida o'tish
Ctrl+w h/j/k/l                             # Oyna yo'nalishi bo'yicha
Ctrl+w c                                   # Oynani yopish
Ctrl+w o                                   # Boshqa oynalarni yopish
:tabnew filename                           # Yangi tab
gt                                         # Keyingi tab
gT                                         # Oldingi tab
:tabc                                      # Tabni yopish
```

**Sozlamalar:**

```bash
:set number                                # Qator raqamlari
:set nonumber                              # Raqamlarni o'chirish
:set nu!                                   # Toggle
:set relativenumber                        # Nisbiy raqamlar
:set hlsearch                              # Qidiruv highlighting
:set nohlsearch                            # Highlighting o'chirish
:noh                                       # Joriy highlighting o'chirish
:set ignorecase                            # Case insensitive
:set smartcase                             # Smart case
:set autoindent                            # Auto indent
:set tabstop=4                             # Tab o'lchami
:set expandtab                             # Tab -> spaces
:set syntax=python                         # Sintaksis
:syntax on                                 # Sintaksis highlighting
:set paste                                 # Paste mode (format saqlash)
:set nopaste                               # Paste mode o'chirish
```

#### 11.2.6 vim kuchli kombinatsiyalar

**Operator + Motion:**

```bash
d + motion                                 # Delete
y + motion                                 # Yank (copy)
c + motion                                 # Change (delete + insert)
> + motion                                 # Indent
< + motion                                 # Dedent
gU + motion                                # Uppercase
gu + motion                                # Lowercase

# Misollar:
d2w                                        # 2 ta so'z o'chirish
y3j                                        # 3 ta qator nusxalash
c$                                         # Qator oxirigacha o'zgartirish
>3j                                        # 3 ta qator indent
gUw                                        # So'zni uppercase
```

**Text objects:**

```bash
# i = inner (ichki), a = around (atrofidagi)
diw                                        # So'z ichini o'chirish
daw                                        # So'z + bo'shliqni o'chirish
di"                                        # "..." ichini o'chirish
da"                                        # "..." ni o'chirish
di(                                        # (...) ichini o'chirish
da(                                        # (...) ni o'chirish
di{                                        # {...} ichini o'chirish
di[                                        # [...] ichini o'chirish
dit                                        # HTML tag ichini o'chirish
dat                                        # HTML tagni o'chirish
dip                                        # Paragraf ichini o'chirish
dap                                        # Paragrafni o'chirish

# y, c bilan ham ishlaydi:
yiw                                        # So'zni nusxalash
ci"                                        # "..." ichini o'zgartirib insert
ca{                                        # {...} ni o'zgartirib insert
```

**Makrolar:**

```bash
qa                                         # 'a' registriga yozishni boshlash
# ... buyruqlar ...
q                                          # Yozishni tugatish
@a                                         # 'a' makrosini ishga tushirish
@@                                         # Oxirgi makroni takrorlash
10@a                                       # 'a' ni 10 marta ishlatish
```

#### 11.2.7 vim konfiguratsiya (~/.vimrc)

```vim
" Asosiy sozlamalar
set number                                 " Qator raqamlari
set relativenumber                         " Nisbiy raqamlar
set cursorline                             " Joriy qator highlighting
set showmatch                              " Qavs juftini ko'rsatish
set hlsearch                               " Qidiruv highlighting
set incsearch                              " Incremental search
set ignorecase                             " Case insensitive
set smartcase                              " Smart case

" Indentation
set tabstop=4                              " Tab = 4 space
set shiftwidth=4                           " Indent = 4 space
set expandtab                              " Tab -> spaces
set autoindent                             " Auto indent
set smartindent                            " Smart indent

" UI
syntax on                                  " Sintaksis highlighting
set background=dark                        " Dark tema
set wildmenu                               " Buyruq autocomplete
set laststatus=2                           " Status bar
set ruler                                  " Cursor pozitsiyasi
set scrolloff=5                            " Scroll offset

" Mapping (tugma o'zgartirish)
let mapleader = ","                        " Leader tugma
nnoremap <leader>w :w<CR>                  " ,w = saqlash
nnoremap <leader>q :q<CR>                  " ,q = chiqish
nnoremap <C-s> :w<CR>                      " Ctrl+s = saqlash
inoremap jj <Esc>                          " jj = Escape
```

#### 11.2.8 vim vs nano qiyoslash

| Xususiyat | nano | vim |
|-----------|------|-----|
| O'rganish | ⭐ Oson | ⭐⭐⭐⭐⭐ Qiyin |
| Kuch | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| Tezlik | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Mavjudlik | Deyarli hamma joyda | Hamma joyda |
| Mouse | Qo'llab-quvvatlaydi | Cheklangan |
| Makrolar | Yo'q | Ha |
| Plugins | Yo'q | Ko'p |
| Katta fayllar | Sekin | Tez |

> 💡 **Maslahat:** Boshlang'ich uchun `nano`, professional uchun `vim`. CTF da ikkalasini ham bilish kerak!

---

## 8. Huquqlar va ruxsatlar tizimi

### 14.1 Ruxsatlar matritsasi

Linux'da har bir faylda uchta toifa uchun uchta ruxsat turi mavjud:

```
         Owner    Group    Others
         -----    -----    ------
Read     r (4)    r (4)    r (4)
Write    w (2)    w (2)    w (2)
Execute  x (1)    x (1)    x (1)
```

### 14.2 Ruxsatlarni o'qish

```bash
ls -la file.txt
# -rw-r--r-- 1 student group 1234 Jan 10 10:00 file.txt
#  │││ │││ │││
#  │││ │││ └┴┴── Others: r-- (faqat o'qish)
#  │││ └┴┴────── Group: r-- (faqat o'qish)
#  └┴┴────────── Owner: rw- (o'qish va yozish)
```

### 10.3 Ikkilik (binary) sanoq tizimi - chmod ning asosi

> 💡 **Nima uchun bu muhim?** Linux ruxsatlari ikkilik sanoq tizimiga asoslangan. Buni tushunmasdan chmod kodlarini yodlab olish qiyin va mantiqsiz bo'ladi.

#### 10.3.1 Ikkilik sanoq tizimi nima?

Biz kundalik hayotda **o'nlik (decimal)** sanoq tizimidan foydalanamiz - u 10 ta raqamdan iborat: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`.

**Ikkilik (binary)** sanoq tizimi esa faqat **2 ta raqamdan** iborat: `0` va `1`.

Kompyuterlar ikkilik tizimda ishlaydi, chunki elektr signali faqat ikki holatda bo'ladi:
- ⚡ **Tok bor** = `1` (ON, true, ha)
- 💤 **Tok yo'q** = `0` (OFF, false, yo'q)

#### 10.3.2 O'nlikdan ikkilikka o'tkazish

**Pozitsiya qiymatlari (o'ngdan chapga):**

```
Pozitsiya:    7      6      5      4      3      2      1      0
            ─────  ─────  ─────  ─────  ─────  ─────  ─────  ─────
2 ning      2⁷     2⁶     2⁵     2⁴     2³     2²     2¹     2⁰
darajasi:   128    64     32     16     8      4      2      1
```

**0 dan 10 gacha sonlarning ikkilik ko'rinishi:**

| O'nlik | Ikkilik | Hisob-kitob | Tushuntirish |
|--------|---------|-------------|--------------|
| **0** | `0000` | 0 | Hech narsa yo'q |
| **1** | `0001` | 2⁰ = 1 | Faqat 1-pozitsiya |
| **2** | `0010` | 2¹ = 2 | Faqat 2-pozitsiya |
| **3** | `0011` | 2¹ + 2⁰ = 2+1 | 2 va 1 pozitsiyalari |
| **4** | `0100` | 2² = 4 | Faqat 4-pozitsiya |
| **5** | `0101` | 2² + 2⁰ = 4+1 | 4 va 1 pozitsiyalari |
| **6** | `0110` | 2² + 2¹ = 4+2 | 4 va 2 pozitsiyalari |
| **7** | `0111` | 2² + 2¹ + 2⁰ = 4+2+1 | 4, 2 va 1 pozitsiyalari |
| **8** | `1000` | 2³ = 8 | Faqat 8-pozitsiya |
| **9** | `1001` | 2³ + 2⁰ = 8+1 | 8 va 1 pozitsiyalari |
| **10** | `1010` | 2³ + 2¹ = 8+2 | 8 va 2 pozitsiyalari |

> 🎯 **Eslab qoling:** Har bir pozitsiya ikki marta oshadi: 1 → 2 → 4 → 8 → 16 → 32...

#### 10.3.3 Ikkilikdan o'nlikka o'tkazish

**Algoritm:** Har bir `1` turgan pozitsiyaning qiymatini qo'shing.

**Misol:** `1011` ni o'nlikka o'tkazish

```
Pozitsiya:     3      2      1      0
              ───    ───    ───    ───
Qiymat:        8      4      2      1
              ───    ───    ───    ───
Raqam:         1      0      1      1
              ───    ───    ───    ───
Hisob:        8×1  + 4×0  + 2×1  + 1×1  = 8 + 0 + 2 + 1 = 11
```

**Javob:** `1011₂` = `11₁₀`

#### 10.3.4 Endi chmod siri - 4, 2, 1 qayerdan keldi?

Linux ruxsatlari **3 bitli** ikkilik tizimga asoslangan:

```
Bit pozitsiyasi:    2      1      0
                   ───    ───    ───
Ikkilik qiymati:   2²     2¹     2⁰
                   ───    ───    ───
O'nlik qiymati:     4      2      1
                   ───    ───    ───
Ruxsat turi:        r      w      x
                  (read) (write) (execute)
```

**Bu degani:**
- `r` (read) = bit 2 = **4**
- `w` (write) = bit 1 = **2**
- `x` (execute) = bit 0 = **1**

#### 10.3.5 Ruxsatlarni ikkilik bilan o'qish

`ls -l` natijasini o'qish usuli:

```
Ruxsat belgisi bor joyga 1, yo'q joyga 0 qo'ying:

-rwxrwxrwx  →  Ikkilikka aylantiramiz
 │││ │││ │││
 111 111 111  →  Har bir guruhni o'nlikka o'tkazamiz
  7   7   7   →  chmod 777
```

**Batafsil misollar:**

| Ruxsat | U (Owner) | G (Group) | O (Others) | chmod |
|--------|-----------|-----------|------------|-------|
| `rwxrwxrwx` | 111 = 7 | 111 = 7 | 111 = 7 | **777** |
| `rwxr-xr-x` | 111 = 7 | 101 = 5 | 101 = 5 | **755** |
| `rw-r--r--` | 110 = 6 | 100 = 4 | 100 = 4 | **644** |
| `rw-------` | 110 = 6 | 000 = 0 | 000 = 0 | **600** |
| `rwx------` | 111 = 7 | 000 = 0 | 000 = 0 | **700** |
| `r--------` | 100 = 4 | 000 = 0 | 000 = 0 | **400** |

#### 10.3.6 To'liq chmod kodlari jadvali

| Ikkilik | O'nlik | Ruxsat | Tavsif |
|---------|--------|--------|--------|
| `000` | 0 | `---` | Hech qanday ruxsat yo'q |
| `001` | 1 | `--x` | Faqat bajarish |
| `010` | 2 | `-w-` | Faqat yozish |
| `011` | 3 | `-wx` | Yozish va bajarish |
| `100` | 4 | `r--` | Faqat o'qish |
| `101` | 5 | `r-x` | O'qish va bajarish |
| `110` | 6 | `rw-` | O'qish va yozish |
| `111` | 7 | `rwx` | Barcha ruxsatlar |

#### 10.3.7 Amaliy misollar

```bash
# Misol 1: chmod 755 script.sh
# Owner: rwx (111 = 7), Group: r-x (101 = 5), Others: r-x (101 = 5)
# Natija: -rwxr-xr-x

# Misol 2: chmod 644 document.txt
# Owner: rw- (110 = 6), Group: r-- (100 = 4), Others: r-- (100 = 4)
# Natija: -rw-r--r--

# Misol 3: chmod 700 private_dir
# Owner: rwx (111 = 7), Group: --- (000 = 0), Others: --- (000 = 0)
# Natija: drwx------

# Misol 4: chmod 600 secret.key
# Owner: rw- (110 = 6), Group: --- (000 = 0), Others: --- (000 = 0)
# Natija: -rw-------
```

> 🔐 **Xavfsizlik maslahatii:** `777` dan hech qachon foydalanmang! Bu hammaga to'liq ruxsat beradi va xavfli!

---

### 10.4 Raqamli (octal) ruxsatlar - xulosa

Har bir ruxsat ikkilik pozitsiyaga mos keladi:
- `r` = 4 (ikkilikda: `100`)
- `w` = 2 (ikkilikda: `010`)
- `x` = 1 (ikkilikda: `001`)

Ularni qo'shib hisoblanadi:

| Ruxsat | Hisob | Qiymat |
|--------|-------|--------|
| `rwx` | 4+2+1 | 7 |
| `rw-` | 4+2+0 | 6 |
| `r-x` | 4+0+1 | 5 |
| `r--` | 4+0+0 | 4 |
| `-wx` | 0+2+1 | 3 |
| `-w-` | 0+2+0 | 2 |
| `--x` | 0+0+1 | 1 |
| `---` | 0+0+0 | 0 |

**Keng tarqalgan kombinatsiyalar:**

| Raqam | Ruxsat | Ishlatilishi |
|-------|--------|--------------|
| 777 | rwxrwxrwx | Hammaga hamma narsa (xavfli!) |
| 755 | rwxr-xr-x | Dasturlar, skriptlar |
| 700 | rwx------ | Shaxsiy katalog |
| 644 | rw-r--r-- | Oddiy fayllar |
| 600 | rw------- | Maxfiy fayllar |
| 400 | r-------- | Faqat o'qish |

### 10.5 chmod - ruxsatlarni o'zgartirish

**Raqamli usul:**
```bash
chmod 755 script.sh                        # rwxr-xr-x
chmod 644 file.txt                         # rw-r--r--
chmod 600 secret.key                       # rw-------
```

**Simvolik usul:**
```bash
chmod u+x script.sh                        # Owner uchun execute qo'sh
chmod g-w file.txt                         # Group dan write ol
chmod o=r file.txt                         # Others faqat read
chmod a+r file.txt                         # All (hamma) ga read qo'sh
chmod u=rwx,g=rx,o=r file                  # To'liq belgilash
```

**Simvollar:**
- `u` - user (owner)
- `g` - group
- `o` - others
- `a` - all

### 10.6 SUID, SGID, sticky bit

Bu maxsus ruxsatlar:

#### SUID (set user ID) - 4xxx

SUID o'rnatilgan fayl egasining huquqlari bilan ishga tushadi:

```bash
ls -la /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
#    ^
#    SUID bit (s harfi x o'rnida)
```

`passwd` buyrug'i oddiy foydalanuvchi tomonidan ishga tushirilganda ham root huquqlari bilan ishlaydi.

**SUID fayllarni topish:**
```bash
find / -perm -4000 2>/dev/null
find / -perm -u=s 2>/dev/null
```

#### SGID (set group ID) - 2xxx

Guruh huquqlari bilan ishlash:

```bash
find / -perm -2000 2>/dev/null
```

#### Sticky bit - 1xxx

Katalogda faqat fayl egasi o'z faylini o'chira oladi:

```bash
ls -la /tmp
# drwxrwxrwt 10 root root ... /tmp
#          ^
#          Sticky bit (t harfi)
```

### 10.7 chown va chgrp

Egasi va guruhni o'zgartirish:

```bash
chown user file.txt                        # Egasini o'zgartir
chown user:group file.txt                  # Egasi va guruhni
chown -R user:group directory/             # Rekursiv
chgrp group file.txt                       # Faqat guruhni
```

---

## 9. Muhim kataloglar va ularning vazifalari

### 11.1 /etc - konfiguratsiya fayllari

Tizim konfiguratsiyasi shu yerda:

```bash
/etc/passwd                                # Foydalanuvchilar ro'yxati
/etc/shadow                                # Parol hashlari (root kerak)
/etc/group                                 # Guruhlar
/etc/sudoers                               # sudo ruxsatlari
/etc/hosts                                 # IP-hostname mappinglari
/etc/hostname                              # Kompyuter nomi
/etc/crontab                               # Rejalashtirilgan vazifalar
/etc/ssh/                                  # SSH konfiguratsiyasi
/etc/apache2/ yoki /etc/nginx/             # Web server konfig
```

**/etc/passwd formati:**
```
username:x:UID:GID:comment:home:shell
student:x:1000:1000:Student User:/home/student:/bin/bash
```

### 11.2 /home - foydalanuvchi kataloglari

```bash
/home/username/                            # Foydalanuvchi uy katalogi
/home/username/.bashrc                     # Bash konfiguratsiyasi
/home/username/.bash_history               # Buyruqlar tarixi
/home/username/.ssh/                       # SSH kalitlari
/home/username/.ssh/id_rsa                 # Shaxsiy kalit
/home/username/.ssh/authorized_keys        # Ruxsat berilgan kalitlar
```

> **CTF maslahat:** `.bash_history` faylida juda muhim ma'lumotlar bo'lishi mumkin!

### 11.3 /var - o'zgaruvchan ma'lumotlar

```bash
/var/log/                                  # Log fayllar
/var/log/syslog                            # Tizim loglari
/var/log/auth.log                          # Autentifikatsiya loglari
/var/www/html/                             # Web server fayllari
/var/backups/                              # Zaxira nusxalar
/var/mail/                                 # Email
/var/spool/cron/                           # Cron vazifalar
```

### 11.4 /tmp - vaqtinchalik fayllar

Barcha foydalanuvchilar yozishi mumkin bo'lgan katalog:

```bash
ls -la /tmp/
ls -la /tmp/.*                             # Yashirin fayllar
```

> **Xavfsizlik:** `/tmp` da sezgir ma'lumotlarni saqlash xavfli!

### 11.5 /proc - virtual fayl tizimi

Process ma'lumotlari bu yerda:

```bash
/proc/[PID]/                               # Har bir process uchun katalog
/proc/[PID]/cmdline                        # Process buyrug'i
/proc/[PID]/environ                        # Environment variables
/proc/[PID]/fd/                            # File descriptors
/proc/version                              # Kernel versiyasi
/proc/cpuinfo                              # CPU ma'lumotlari
/proc/meminfo                              # Xotira ma'lumotlari
```

**CTF'da juda muhim:**
```bash
strings /proc/*/environ 2>/dev/null | grep -i flag
strings /proc/*/environ 2>/dev/null | grep -i password
```

### 11.6 /opt - qo'shimcha dasturlar

Ko'pincha uchinchi tomon dasturlari bu yerda:

```bash
ls -la /opt/
find /opt -name "*.conf" 2>/dev/null
find /opt -readable -type f 2>/dev/null
```

### 7.7 /root - superuser uy katalogi

Root foydalanuvchining shaxsiy katalogi:

```bash
ls -la /root/                              # (root kerak)
cat /root/flag.txt                         # Klassik flag joylashuvi
```

---

## 10. Arxivlar bilan ishlash

### 14.1 tar - tape archive

**Arxiv yaratish:**
```bash
tar -cvf archive.tar files/                # Oddiy arxiv
tar -czvf archive.tar.gz files/            # Gzip bilan siqish
tar -cjvf archive.tar.bz2 files/           # Bzip2 bilan siqish
tar -cJvf archive.tar.xz files/            # XZ bilan siqish
```

**Arxiv ochish:**
```bash
tar -xvf archive.tar                       # Oddiy ochish
tar -xzvf archive.tar.gz                   # Gzip arxivni ochish
tar -xjvf archive.tar.bz2                  # Bzip2 arxivni ochish
tar -xJvf archive.tar.xz                   # XZ arxivni ochish
tar -xvf archive.tar -C /destination/      # Ma'lum joyga ochish
```

**Arxiv tarkibini ko'rish:**
```bash
tar -tvf archive.tar                       # Ro'yxatni ko'r
tar -tzvf archive.tar.gz                   # Siqilgan arxiv
```

**Flaglar:**
- `c` - create (yaratish)
- `x` - extract (ochish)
- `t` - list (ro'yxat)
- `v` - verbose (batafsil)
- `f` - file (fayl)
- `z` - gzip
- `j` - bzip2
- `J` - xz

### 14.2 zip / unzip

```bash
zip archive.zip file1 file2                # Zip yaratish
zip -r archive.zip directory/              # Rekursiv
zip -e archive.zip file                    # Parol bilan

unzip archive.zip                          # Ochish
unzip -l archive.zip                       # Ro'yxat ko'rish
unzip -d /destination archive.zip          # Ma'lum joyga
unzip -P password archive.zip              # Parol bilan
```

### 10.3 gzip / gunzip

```bash
gzip file.txt                              # file.txt.gz yaratadi
gunzip file.txt.gz                         # Ochadi
gzip -d file.txt.gz                        # gunzip bilan bir xil
zcat file.txt.gz                           # Ochmasdan o'qish
zgrep "pattern" file.gz                    # Ichida qidirish
```

### 10.4 bzip2 / bunzip2

```bash
bzip2 file.txt                             # file.txt.bz2 yaratadi
bunzip2 file.txt.bz2                       # Ochadi
bzcat file.txt.bz2                         # Ochmasdan o'qish
```

### 10.5 xz / unxz

```bash
xz file.txt                                # file.txt.xz yaratadi
unxz file.txt.xz                           # Ochadi
xzcat file.txt.xz                          # Ochmasdan o'qish
```

### 10.6 Arxiv kengaytmalarini tanish

| Kengaytma | Buyruq |
|-----------|--------|
| `.tar` | `tar -xvf` |
| `.tar.gz`, `.tgz` | `tar -xzvf` |
| `.tar.bz2`, `.tbz2` | `tar -xjvf` |
| `.tar.xz` | `tar -xJvf` |
| `.zip` | `unzip` |
| `.gz` | `gunzip` |
| `.bz2` | `bunzip2` |
| `.xz` | `unxz` |
| `.7z` | `7z x` |
| `.rar` | `unrar x` |

---

## 11. Disk va mount operatsiyalari

### 11.1 df - disk free

```bash
df                                         # Disk bo'sh joy
df -h                                      # Human-readable
df -T                                      # Filesystem type
df -a                                      # Barcha filesystems
```

**Natija:**
```
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda1      ext4   50G   25G   23G  52% /
/dev/sdb1      ext4  100G   30G   65G  32% /home
```

### 11.2 du - disk usage

```bash
du file.txt                                # Fayl hajmi
du -h file.txt                             # Human-readable
du -sh directory/                          # Katalog umumiy hajmi
du -sh *                                   # Joriy katalogdagi har biri
du -ah . | sort -rh | head -20             # Eng katta 20 ta
```

### 11.3 lsblk - block devices

```bash
lsblk                                      # Blok qurilmalar
lsblk -f                                   # Filesystem info
lsblk -a                                   # Hammasi
```

**Natija:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   49G  0 part /
└─sda2   8:2    0    1G  0 part [SWAP]
sdb      8:16   0  100G  0 disk
└─sdb1   8:17   0  100G  0 part /home
```

### 11.4 mount

```bash
mount                                      # Barcha mountlar
mount | grep -v "^none"                    # none dan boshqalar
mount /dev/sdb1 /mnt/usb                   # USB mount qilish
umount /mnt/usb                            # Unmount
```

**Muhim fayllar:**
```bash
cat /etc/fstab                             # Avtomatik mountlar
cat /etc/mtab                              # Joriy mountlar
findmnt                                    # Mount tree
```

---

## 12. Simvolik havolalar (Symlinks)

### 14.1 Hard link vs symbolic link

| Xususiyat | Hard Link | Symbolic Link |
|-----------|-----------|---------------|
| Inode | Bir xil inode | Yangi inode |
| Fayl o'chirilsa | Ishlaydi | Buziladi |
| Katalogga | Mumkin emas | Mumkin |
| Boshqa filesystem | Mumkin emas | Mumkin |

### 14.2 Symlink yaratish

```bash
ln -s /path/to/target linkname             # Symlink yaratish
ln -s /etc/passwd ~/passwd_link            # Misol
```

### 10.3 Symlinklarni ko'rish

```bash
ls -la                                     # l bilan boshlanadi
# lrwxrwxrwx 1 user group 11 Jan 10 10:00 link -> /etc/passwd

readlink linkname                          # Target ni ko'rsat
readlink -f linkname                       # To'liq yo'l
```

### 10.4 Symlinklarni topish

```bash
find / -type l 2>/dev/null                 # Barcha symlinks
find / -type l -ls 2>/dev/null             # Batafsil
find / -xtype l 2>/dev/null                # Buzilgan symlinks
```

---

## 13. Amaliy mashqlar

### Mashq 1: Asosiy navigatsiya

**Vazifa:** Quyidagi operatsiyalarni bajaring:

1. Joriy katalogingizni aniqlang
2. Uy katalogingizga o'ting
3. `/etc` katalogiga o'ting va u yerda `passwd` faylini toping
4. Oldingi katalogga qayting

**Yechim:**
```bash
pwd
cd ~
cd /etc && ls -la | grep passwd
cd -
```

### Mashq 2: Fayl qidirish

**Vazifa:** Quyidagi fayllarni toping:

1. Tizimdagi barcha `.txt` fayllar
2. `/home` katalogidagi yashirin fayllar
3. Hajmi 1MB dan katta fayllar `/var` da

**Yechim:**
```bash
find / -name "*.txt" 2>/dev/null
find /home -name ".*" -type f 2>/dev/null
find /var -size +1M -type f 2>/dev/null
```

### Mashq 3: Ruxsatlarni tahlil qilish

**Vazifa:** Quyidagi ruxsatlarni tushuntiring:

1. `-rw-r--r--`
2. `-rwxr-xr-x`
3. `drwxr-x---`
4. `-rwsr-xr-x`

**Javob:**
1. 644 - Owner: o'qish+yozish, Group/Others: faqat o'qish
2. 755 - Owner: hamma, Group/Others: o'qish+bajarish
3. 750 - Katalog, Owner: hamma, Group: o'qish+bajarish, Others: ruxsat yo'q
4. 4755 - SUID o'rnatilgan, bajarilganda owner huquqi bilan ishlaydi

### Mashq 4: grep bilan Qidirish

**Vazifa:** `/etc` katalogida:

1. "root" so'zini o'z ichiga olgan fayllarni toping
2. "password" yoki "passwd" mavjud qatorlarni toping

**Yechim:**
```bash
grep -rl "root" /etc 2>/dev/null
grep -rE "(password|passwd)" /etc 2>/dev/null
```

### Mashq 5: Arxiv bilan ishlash

**Vazifa:**

1. `test` nomli katalog yarating
2. Ichiga 3 ta fayl qo'shing
3. Gzip bilan tar arxiv yarating
4. Arxiv tarkibini ko'ring
5. Arxivni boshqa joyga oching

**Yechim:**
```bash
mkdir test
touch test/file{1,2,3}.txt
tar -czvf test.tar.gz test/
tar -tzvf test.tar.gz
tar -xzvf test.tar.gz -C /tmp/
```

---

## 14. CTF strategiyalari

### 14.1 Birinchi qadamlar (60 soniya)

Har bir CTF mashqda birinchi qiladigan ishlar:

```bash
# 1. Kim va qayerdaman?
id && pwd && whoami

# 2. Atrofda nima bor?
ls -la

# 3. Tezkor flag qidirish
find / -name "*flag*" 2>/dev/null
cat /home/*/flag* /root/flag* 2>/dev/null
```

### 14.2 Chuqurroq tekshirish (2-5 daqiqa)

```bash
# Bash tarixini tekshir
cat /home/*/.bash_history 2>/dev/null

# SUID fayllarni tekshir
find / -perm -4000 2>/dev/null

# Backuplarni tekshir
ls -la /var/backups/

# Environment variableslarni tekshir
cat /proc/*/environ 2>/dev/null | tr '\0' '\n' | grep -iE "(flag|pass|secret)"

# World-writable fayllar
find / -writable -type f 2>/dev/null | head -20
```

### 14.3 Flag format regex

Ko'p CTF musobaqalarida standart flag formatlari:

```bash
# Turli flag formatlarni qidirish
grep -rE "FLAG\{[^}]+\}" / 2>/dev/null
grep -rE "CTF\{[^}]+\}" / 2>/dev/null
grep -rE "HTB\{[^}]+\}" / 2>/dev/null
grep -rE "flag\{[^}]+\}" / 2>/dev/null
grep -rE "picoCTF\{[^}]+\}" / 2>/dev/null

# Umumiy pattern
grep -roE "[A-Z]{2,10}\{[a-zA-Z0-9_-]+\}" / 2>/dev/null
```

### 14.4 Binary fayllarni tekshirish

```bash
# strings bilan flag qidirish
strings suspicious_file | grep -iE "flag|password|secret"

# Executable fayllardan
find / -type f -executable 2>/dev/null | xargs strings 2>/dev/null | grep FLAG

# Fayl turini aniqlash
file mystery_file
```

### 14.5 Yashirin ma'lumotlarni topish

**Tekshirish kerak bo'lgan joylar:**

1. **Yashirin fayllar:** `.hidden`, `.secret`, `.flag`
2. **Backup fayllar:** `.bak`, `.old`, `~`, `.swp`
3. **Konfiguratsiya fayllari:** `/etc/*.conf`
4. **Log fayllar:** `/var/log/*`
5. **Temporary fayllar:** `/tmp/.*`
6. **Environment:** `/proc/*/environ`
7. **Cron:** `/etc/cron*`, `/var/spool/cron/`

### 14.6 Eng muhim one-liners

```bash
# Ultimate flag hunter
grep -rE "(FLAG|CTF|HTB|flag)\{[^}]+\}" / 2>/dev/null

# SUID fayllar bilan privelege escalation
find / -perm -4000 -type f -exec ls -la {} \; 2>/dev/null

# Sizga tegishli bo'lmagan, lekin o'qishingiz mumkin bo'lgan fayllar
find / ! -user $(whoami) -readable -type f 2>/dev/null | head -20

# Oxirgi 30 daqiqada o'zgargan fayllar
find / -mmin -30 -type f 2>/dev/null

# Password qidirish
grep -rn "password" /home /etc /opt /var 2>/dev/null | head -20
```

### 14.7 Xatolardan qochish

**Beginnerlar qiladigan xatolar:**

1. `2>/dev/null` ishlatmaslik - output juda ko'p xato xabarlari bilan to'ladi
2. `-la` o'rniga faqat `ls` ishlatish - yashirin fayllar ko'rinmaydi
3. `strings` buyrug'ini unutish - binary fayllardagi flaglar topilmaydi
4. Faqat `/home` ni tekshirish - `/opt`, `/var`, `/tmp` ham muhim
5. `.bash_history` ni e'tiborsiz qoldirish

---

## Xulosa

Bu qo'llanmada Linux fayl tizimining asosiy tushunchalari, buyruqlari va CTF musobaqalariga tayyorgarlik strategiyalari yoritildi. Eng muhim maslahat - **amaliyot**. Har kuni virtual mashinada mashq qiling, turli CTF platformalarida (HackTheBox, TryHackMe, picoCTF) o'z ko'nikmalaringizni sinab ko'ring.

**Eslab qoling:**
- Har doim `ls -la` ishlating
- `2>/dev/null` outputni tozalaydi
- `strings` binary fayllar uchun zarur
- `.bash_history` da qimmatli ma'lumotlar bo'lishi mumkin
- SUID fayllar privilege escalation uchun muhim

Omad tilaymiz!

---

**Qo'shimcha Manbalar:**

- [Linux Journey](https://linuxjourney.com/)
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)
- [TryHackMe](https://tryhackme.com/)
- [HackTheBox](https://www.hackthebox.com/)
- [picoCTF](https://picoctf.org/)

---

*Ushbu qo'llanma Nodir Ustoz tomonidan tayyorlangan. Savol va takliflar uchun ustozning o'ziga murojaat qiling.*
