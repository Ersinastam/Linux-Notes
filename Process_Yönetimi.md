# Linux Process Yönetimi

Linux'ta çalışan her program bir **process (süreç)** olarak çalışır.

Örneğin bir terminal açtığımızda, bir uygulama çalıştırdığımızda veya bir komut verdiğimizde Linux bu program için bir process oluşturur.

Temel mantık:

```text
Program
   ↓
Çalıştırılır
   ↓
Process oluşur
   ↓
CPU ve RAM kullanır
```

---

## 1. Process Nedir?

Process, çalışan bir programın işletim sistemi tarafından yönetilen halidir.

Örneğin:

```bash
firefox
```

komutunu çalıştırdığımızda Linux Firefox için bir process oluşturur.

Her process'in kendine ait bazı bilgileri vardır:

```text
PID
Kullanıcı
CPU kullanımı
RAM kullanımı
Durumu
```

---

## 2. PID Nedir?

**PID (Process ID)**, her process'e Linux tarafından verilen benzersiz kimlik numarasıdır.

Örneğin:

```text
PID
│
├── 1
├── 250
├── 731
└── 1024
```

Bir process'i takip etmek veya sonlandırmak istediğimizde PID'sini kullanabiliriz.

Process'leri görmek için:

```bash
ps
```

Daha detaylı görmek için:

```bash
ps aux
```

Örnek:

```text
USER   PID   %CPU  %MEM  COMMAND
root     1    0.0   0.1  systemd
kali   731    1.2   2.5  firefox
kali   850    0.0   0.5  bash
```

Burada:

```text
PID 731
```

Firefox process'inin kimlik numarasıdır.

---

## 3. Process'leri Görmek

### ps

```bash
ps
```

Mevcut terminal ile ilişkili process'leri gösterir.

Daha detaylı bilgi:

```bash
ps aux
```

Burada:

```text
a → Tüm kullanıcıların process'lerini gösterir
u → Kullanıcı bilgilerini gösterir
x → Terminale bağlı olmayan process'leri de gösterir
```

---

## 4. Process Ağacını Görmek

Process'lerin birbirleriyle olan ilişkisini görmek için:

```bash
pstree
```

kullanılır.

Örneğin:

```text
systemd
 ├── sshd
 │    └── bash
 │         └── vim
 └── NetworkManager
```

Burada bazı process'lerin başka process'ler tarafından oluşturulduğunu görebiliriz.

---

## 5. Parent ve Child Process

Bir process başka bir process oluşturabilir.

Bu durumda:

```text
Parent Process
      ↓
Child Process
```

ilişkisi oluşur.

Örneğin:

```text
systemd
   ↓
sshd
   ↓
bash
   ↓
vim
```

Burada:

```text
systemd → Parent
sshd    → Child
bash    → Child
vim     → Child
```

gibi düşünebiliriz.

Process'lerin PID ve PPID bilgilerini görmek için:

```bash
ps -ef
```

kullanılabilir.

Burada:

```text
PID  → Process ID
PPID → Parent Process ID
```

anlamına gelir.

---

## 6. Gerçek Zamanlı Process Takibi

Çalışan process'leri gerçek zamanlı olarak görmek için:

```bash
top
```

kullanılır.

Örneğin:

```text
PID   USER   CPU%   MEM%   COMMAND
731   kali    20.3    5.1  firefox
850   kali     1.2    0.5  bash
```

Buradan:

- CPU kullanımı
- RAM kullanımı
- PID
- Process adı

gibi bilgileri görebiliriz.

---

## 7. htop

`htop`, `top` komutuna alternatif daha kullanıcı dostu bir process izleme aracıdır.

```bash
htop
```

Kali'de kurulu değilse:

```bash
sudo apt install htop
```

ile kurulabilir.

---

## 8. Process Durumları

Process'ler farklı durumlarda olabilir.

Temel durumlar:

```text
R → Running
S → Sleeping
D → Uninterruptible Sleep
T → Stopped
Z → Zombie
```

### Running

Process çalışıyor veya çalışmaya hazırdır.

### Sleeping

Process şu anda bir olay veya kaynak bekliyordur.

### Stopped

Process durdurulmuştur.

### Zombie

Process çalışmasını tamamlamıştır fakat parent process henüz process'in son durumunu toplamamıştır.

---

## 9. Process Sonlandırmak

Bir process'i sonlandırmak için:

```bash
kill PID
```

kullanılır.

Örneğin:

```bash
kill 731
```

Burada:

```text
731 → Sonlandırmak istediğimiz process'in PID'si
```

---

## 10. Signal Nedir?

`kill` komutu process'i doğrudan öldürmekten ziyade process'e bir **signal (sinyal)** gönderir.

Sık kullanılan signal'ler:

```text
SIGTERM → 15
SIGKILL → 9
SIGSTOP → 19
SIGCONT → 18
```

---

## 11. SIGTERM

```bash
kill -15 731
```

veya:

```bash
kill 731
```

Process'e düzgün şekilde kapanmasını isteyen bir signal gönderir.

Process'e:

> "Çalışmanı düzgün şekilde sonlandır."

mesajı gönderilmiş olur.

Genellikle ilk tercih `SIGTERM` olmalıdır.

---

## 12. SIGKILL

```bash
kill -9 731
```

Process'i zorla sonlandırır.

Process'in düzgün şekilde kapanmasını beklemez.

Bu nedenle:

```text
Önce → kill PID
Gerekirse → kill -9 PID
```

şeklinde düşünmek daha doğrudur.

---

## 13. Process'i Durdurmak ve Devam Ettirmek

Bir process'i durdurmak:

```bash
kill -STOP 731
```

Process'i tekrar devam ettirmek:

```bash
kill -CONT 731
```

Bu işlemler process'i öldürmez.

Sadece çalışmasını durdurur ve tekrar devam ettirir.

---

## 14. Process Aramak

Çok fazla process varsa belirli bir process'i aramak için:

```bash
ps aux | grep firefox
```

kullanabiliriz.

Buradaki:

```text
ps aux
   ↓
Tüm process'leri göster

|
   ↓
Çıktıyı grep'e gönder

grep firefox
   ↓
firefox geçen satırları göster
```

mantığı vardır.

---

## 15. Process'i İsmiyle Sonlandırmak

PID'sini bilmediğimiz bir process'i ismi üzerinden sonlandırmak için:

```bash
pkill firefox
```

kullanılabilir.

`pkill` process adına göre çalıştığı için dikkatli kullanılmalıdır.

---

## 16. Arka Planda Process Çalıştırmak

Bir komutun terminali meşgul etmeden arka planda çalışmasını istiyorsak komutun sonuna `&` ekleyebiliriz.

Örneğin:

```bash
sleep 300 &
```

Bu komut arka planda çalışır.

Çalışan job'ları görmek için:

```bash
jobs
```

kullanılır.

---

## 17. Process'i Arka Plana Göndermek

Terminalde çalışan bir programı:

```text
Ctrl + Z
```

ile durdurabiliriz.

Daha sonra:

```bash
bg
```

ile arka planda devam ettirebiliriz.

Tekrar ön plana almak için:

```bash
fg
```

kullanılır.

---

## 18. Process Yönetiminde Temel Komutlar

```bash
ps aux
```

Çalışan process'leri gösterir.

```bash
top
```

Process'leri gerçek zamanlı olarak gösterir.

```bash
htop
```

Process'leri daha kullanıcı dostu bir arayüzle gösterir.

```bash
pstree
```

Process'leri ağaç şeklinde gösterir.

```bash
kill PID
```

Process'e signal gönderir.

```bash
pkill process_adi
```

Process adına göre signal gönderir.

```bash
jobs
```

Terminal job'larını gösterir.

```bash
bg
```

Job'ı arka planda çalıştırır.

```bash
fg
```

Job'ı ön plana getirir.

---

## 19. Uygulama

Yeni bir process oluşturalım:

```bash
sleep 300 &
```

Process'i bulalım:

```bash
ps aux | grep sleep
```

PID'sini öğrendikten sonra:

```bash
kill PID
```

ile sonlandıralım.

Daha sonra tekrar kontrol edelim:

```bash
ps aux | grep sleep
```

---

# Özet

Linux'ta çalışan her program bir **process** olarak çalışır.

Temel yapı:

```text
Program
   ↓
Process
   ↓
PID
   ↓
CPU / RAM kullanımı
```

Önemli kavramlar:

```text
Process → Çalışan program
PID     → Process'in kimlik numarası
PPID    → Parent process'in kimlik numarası
Signal  → Process'e gönderilen kontrol mesajı
```

En önemli komutlar:

```bash
ps aux
top
htop
pstree
kill PID
pkill process_adi
jobs
bg
fg
```
