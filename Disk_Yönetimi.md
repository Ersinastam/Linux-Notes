# Linux Disk Yönetimi

Linux'ta disk yönetimi, sisteme bağlı diskleri **bölmek, dosya sistemi oluşturmak, kullanıma açmak ve yönetmek** anlamına gelir.

Temel mantık:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount
 ↓
Kullanım
```

---

## 1. Disk Nedir?

Disk, verilerin fiziksel olarak tutulduğu depolama alanıdır.

Linux'ta diskler genellikle şu şekilde görünür:

```text
/dev/sda
/dev/sdb
/dev/sdc
```

Örneğin:

```text
/dev/sda
```

80 GB'lık bir disk olabilir.

Diskleri görmek için:

```bash
lsblk
```

kullanılır.

Örnek:

```text
NAME   SIZE TYPE MOUNTPOINTS
sda   80.1G disk
└─sda1 80.1G part /
sdb    20G  disk
```

Burada:

* `sda` → disk
* `sda1` → partition
* `sdb` → başka bir disk

---

# 2. Partition Nedir?

**Partition (bölüm)**, bir diskin parçalara ayrılmış halidir.

Örneğin 100 GB diskimiz olsun:

```text
/dev/sdb
┌────────────────────────────────────┐
│              100 GB                │
└────────────────────────────────────┘
```

Bu diski iki parçaya bölebiliriz:

```text
/dev/sdb
┌──────────────────┬─────────────────┐
│     sdb1         │      sdb2       │
│      50 GB       │       50 GB     │
└──────────────────┴─────────────────┘
```

Artık:

```text
/dev/sdb1
/dev/sdb2
```

diye iki ayrı partition vardır.

### Partition oluşturmak

Örneğin `sdb` diski üzerinde çalışmak için:

```bash
sudo fdisk /dev/sdb
```

`fdisk` içerisinde:

```text
n → yeni partition oluştur
p → partition tablosunu göster
d → partition sil
w → değişiklikleri diske yaz
q → çık
```

Örneğin yeni partition oluşturmak:

```text
n
```

Sonra varsayılan seçenekleri kabul etmek için `Enter` kullanılabilir.

İşlemi diske kaydetmek için:

```text
w
```

kullanılır.

> ⚠️ `fdisk` kullanırken doğru diski seçmek çok önemlidir. Yanlış diskte işlem yapmak veri kaybına neden olabilir.

---

# 3. Filesystem Nedir?

Partition oluşturduktan sonra elimizde sadece bir **depolama alanı** vardır.

Örneğin:

```text
/dev/sdb1
```

Bu alanı dosya ve klasörleri saklayabilecek şekilde kullanmak için üzerine bir **filesystem (dosya sistemi)** oluşturabiliriz.

Linux'ta yaygın filesystem türleri:

```text
ext4
XFS
Btrfs
```

Örneğin `ext4` oluşturmak:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Burada:

```text
mkfs → make filesystem
ext4 → oluşturulacak filesystem türü
/dev/sdb1 → filesystem'in oluşturulacağı partition
```

Artık:

```text
/dev/sdb
   ↓
/dev/sdb1
   ↓
ext4
```

olmuştur.

---

# 4. Filesystem Ne İşe Yarar?

Filesystem, disk üzerindeki dosyaların nasıl tutulacağını ve yönetileceğini belirler.

Örneğin:

```text
foto.jpg
notlar.txt
video.mp4
```

gibi dosyaların:

* nerede olduğu
* ne kadar yer kapladığı
* hangi kullanıcıya ait olduğu
* hangi izinlere sahip olduğu
* hangi alanların boş veya dolu olduğu

gibi bilgilerin yönetilmesini sağlar.

### ext4 içerisinde önemli kavramlar

```text
Superblock
Inode
Block
```

### Block

Verilerin tutulduğu küçük depolama alanlarıdır.

### Inode

Dosyanın kendisi değildir.

Dosya hakkında bilgiler ve dosyanın verilerinin nerede bulunduğuna dair bilgiler tutulur.

### Superblock

Filesystem hakkında temel bilgileri tutar.

---

# 5. Filesystem Türünü Görmek

Bir partition üzerinde hangi filesystem'in olduğunu görmek için:

```bash
lsblk -f
```

kullanılır.

Örneğin:

```text
NAME   FSTYPE  MOUNTPOINTS
sda
└─sda1 ext4    /
sdb
└─sdb1 ext4
```

Burada:

```text
sdb1 → ext4
```

görüyorsak `sdb1` üzerinde `ext4` filesystem vardır.

Alternatif olarak:

```bash
sudo blkid /dev/sdb1
```

kullanılabilir.

Örneğin:

```text
/dev/sdb1: UUID="..." TYPE="ext4"
```

---

# 6. Mount Nedir?

Filesystem oluşturmak tek başına yeterli değildir.

Filesystem'in Linux'un dosya ağacında **erişilebilir bir noktaya bağlanması** gerekir.

Bu işleme:

**mount (bağlama)** denir.

Örneğin:

```text
/dev/sdb1
   ↓
ext4
```

filesystem'imiz var.

Önce bir **mount point (bağlama noktası)** oluşturabiliriz:

```bash
sudo mkdir /data
```

Burada `/data`, mevcut Linux filesystem'imiz içinde oluşturulan normal bir klasördür.

Daha sonra:

```bash
sudo mount /dev/sdb1 /data
```

diyoruz.

Artık:

```text
/dev/sdb1
   ↓
  ext4
   ↓
 /data
```

olmuştur.

Yani `/data` üzerinden `sdb1` üzerindeki filesystem'e erişebiliriz.

---

# 7. Mount Sonrası Ne Olur?

Mount işleminden sonra:

```bash
cd /data
```

diyebiliriz.

Örneğin:

```bash
sudo touch /data/test.txt
```

dediğimizde `test.txt` dosyasının verileri **`/dev/sdb1` üzerindeki filesystem'de** tutulur.

Kontrol etmek için:

```bash
ls /data
```

kullanabiliriz.

---

# 8. Mount Edilmezse Ne Olur?

Filesystem vardır:

```text
/dev/sdb1
   ↓
ext4
```

Ama Linux dosya ağacına bağlanmamıştır.

Bu yüzden `/data` üzerinden ona erişemeyiz.

Mount ettikten sonra:

```text
/dev/sdb1
   ↓
ext4
   ↓
/data
```

olur.

Artık:

```bash
cd /data
```

ile filesystem'e erişebiliriz.

---

# 9. Mount Edilmiş Dosya Sistemlerini Görmek

Şu komut kullanılabilir:

```bash
lsblk -f
```

Örneğin:

```text
NAME   FSTYPE MOUNTPOINTS
sda
└─sda1 ext4   /
sdb
└─sdb1 ext4   /data
```

Burada:

```text
sda1 → /
sdb1 → /data
```

şeklinde görebiliriz.

---

# 10. Umount Nedir?

Mount işleminin tersidir.

Bir filesystem'i bağlı olduğu yerden çıkarmak için:

```bash
sudo umount /data
```

kullanılır.

Artık:

```text
/dev/sdb1
   ↓
ext4

X /data'ya bağlı değil
```

durumuna gelir.

> `umount` yazılır. `unmount` değil.

---

# 11. Filesystem Nasıl Silinir?

Filesystem'i temizlemek için `wipefs` kullanılabilir.

Önce kontrol:

```bash
sudo wipefs /dev/sdb1
```

Filesystem imzasını temizlemek:

```bash
sudo wipefs -a /dev/sdb1
```

Sonra:

```bash
lsblk -f
```

ile kontrol edilebilir.

Örneğin:

```text
sdb
└─sdb1
```

görülür ve `FSTYPE` kısmında `ext4` artık görünmez.

> ⚠️ `wipefs -a` veri erişimini bozabilir. Gerçek sistemlerde kullanmadan önce doğru disk ve partition kesinlikle kontrol edilmelidir.

---

# 12. Disk Kullanımını Kontrol Etmek

Filesystem'in ne kadar dolu olduğunu görmek için:

```bash
df -h
```

kullanılır.

Örneğin:

```text
Filesystem      Size  Used Avail Use%
/dev/sda1        80G   20G   60G  25%
/dev/sdb1        20G  500M   19G   3%
```

Burada:

```text
Size  → toplam alan
Used  → kullanılan alan
Avail → kullanılabilir alan
Use%  → kullanım yüzdesi
```

---

# 13. Diskleri Görmek

```bash
lsblk
```

Disklerin ve partition'ların yapısını gösterir.

Daha detaylı filesystem bilgisi:

```bash
lsblk -f
```

Disk partition tablosunu görmek:

```bash
sudo fdisk -l
```

Filesystem bilgisi:

```bash
sudo blkid
```

Disk kullanımını görmek:

```bash
df -h
```

---

# 14. Temel Disk Yönetimi Akışı

Yeni bir disk geldiğini düşünelim:

```text
/dev/sdb
```

### Adım 1 — Diski gör

```bash
lsblk
```

### Adım 2 — Partition oluştur

```bash
sudo fdisk /dev/sdb
```

Örneğin:

```text
/dev/sdb
└── /dev/sdb1
```

### Adım 3 — Filesystem oluştur

```bash
sudo mkfs.ext4 /dev/sdb1
```

Artık:

```text
/dev/sdb
└── /dev/sdb1
       └── ext4
```

### Adım 4 — Mount point oluştur

```bash
sudo mkdir /data
```

### Adım 5 — Mount et

```bash
sudo mount /dev/sdb1 /data
```

Artık:

```text
/dev/sdb
   ↓
/dev/sdb1
   ↓
ext4
   ↓
/data
```

### Adım 6 — Kontrol et

```bash
lsblk -f
```

### Adım 7 — Kullan

```bash
cd /data
sudo touch test.txt
```

---

# 15. Konunun Tam Özeti

En önemli zincir:

```text
                 DISK
                  │
                  ▼
              PARTITION
                  │
                  ▼
             FILESYSTEM
                ext4
                  │
                  ▼
                MOUNT
                  │
                  ▼
               /data
                  │
                  ▼
              DOSYALAR
```

### Kısaca:

**Disk**

> Fiziksel depolama alanı.

**Partition**

> Diskin bölünmüş kısmı.

**Filesystem**

> Partition üzerindeki dosyaların nasıl organize edileceğini belirleyen yapı.

**Mount**

> Filesystem'i Linux'un dosya ağacında erişilebilir bir yere bağlama işlemi.

---

## Örnek

Elimizde:

```text
/dev/sdb
```

adında 20 GB disk olsun.

Yaptığımız işlemler:

```bash
sudo fdisk /dev/sdb
```

↓

```text
/dev/sdb1
```

↓

```bash
sudo mkfs.ext4 /dev/sdb1
```

↓

```text
/dev/sdb1 → ext4
```

↓

```bash
sudo mkdir /data
```

↓

```bash
sudo mount /dev/sdb1 /data
```

↓

```text
/dev/sdb1 → ext4 → /data
```

Artık:

```bash
sudo touch /data/test.txt
```

dediğimizde dosya **yeni disk üzerindeki filesystem'e** yazılır.

---

# 🧠 Ezberlemek İçin

```text
Disk
↓
"Neyi kullanıyorum?"

Partition
↓
"Diskin hangi bölümünü kullanıyorum?"

Filesystem
↓
"Bu alandaki dosyaları nasıl yöneteceğim?"

Mount
↓
"Bu alana Linux'ta nereden erişeceğim?"
```

> **Disk yönetiminin temel mantığı budur.**
