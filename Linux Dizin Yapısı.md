# 📂 Linux Dizin Yapısı

Linux'ta her şey **kök dizin ( / )** altında bulunur. Tüm dosya ve dizinler bu yapının içerisinde yer alır.

---

# 📌 /

## Kök (Root) Dizini

Linux dosya sisteminin en üst seviyesidir.

- Sadece `/` karakteri ile gösterilir.
- Sistemdeki tüm dosya ve dizinler bunun altında bulunur.
- Windows'taki `C:\` sürücüsüne benzetilebilir ancak Linux'ta tek bir kök dizin vardır.

---

# 📌 /bin

## Temel Kullanıcı Komutları

**bin**, **Binaries** kelimesinin kısaltmasıdır.

Sistemin çalışması için gerekli temel komutlar burada bulunur.

Hem **root** hem de normal kullanıcılar bu komutları çalıştırabilir.

### Örnek Komutlar

```bash
ls
cp
mv
cat
pwd
mkdir
rm
echo
```

---

# 📌 /sbin

## Sistem Yönetim Komutları

**sbin**, **System Binaries** anlamına gelir.

Buradaki komutlar sistem yönetimi içindir ve çoğu root yetkisi gerektirir.

### Örnek Komutlar

```bash
iptables
fdisk
reboot
shutdown
mkfs
ifconfig
```

---

# 📌 /etc

## Yapılandırma Dosyaları

Sistemin ve uygulamaların yapılandırma dosyaları burada bulunur.

### Örnekler

```text
/etc/passwd
```

Kullanıcı hesap bilgileri

```text
/etc/shadow
```

Şifrelerin hash değerleri

```text
/etc/ssh/ssh_config
```

SSH istemci yapılandırması

```text
/etc/ssh/sshd_config
```

SSH sunucu yapılandırması

```text
/etc/dhcp/dhclient-exit-hooks.d/
```

DHCP istemcisi için çalışan scriptler

> **Not:** DHCP'nin ana yapılandırma dosyası dağıtıma göre farklı olabilir.

---

# 📌 /root

## Root Kullanıcısının Ev Dizini

Normal kullanıcıların ev dizini `/home` altında bulunurken;

Root kullanıcısının ev dizini doğrudan

```text
/root
```

dizinidir.

Diğer kullanıcılardan güvenlik amacıyla ayrılmıştır.

---

# 📌 /home

## Kullanıcı Ev Dizinleri

Sistemde oluşturulan kullanıcıların kişisel dosyaları burada tutulur.

Örnek:

```text
/home/ersin
/home/ahmet
```

Belgeler, Masaüstü, İndirilenler gibi klasörler burada bulunur.

---

# 📌 /var

## Değişken Dosyalar

**Variable** kelimesinden gelir.

Sistem çalıştıkça sürekli değişen dosyalar burada tutulur.

### Önemli Alt Dizinler

```text
/var/log
```

Sistem log dosyaları

```text
/var/mail
```

E-postalar

```text
/var/www
```

Web sunucusu dosyaları (Apache, Nginx vb.)

---

# 📌 /tmp

## Geçici Dosyalar

Programların geçici olarak oluşturduğu dosyalar burada tutulur.

- Genellikle sistem yeniden başladığında temizlenir.
- Önemli dosyalar burada saklanmamalıdır.

---

# 📌 /usr

## Kullanıcı Programları

Kurulu uygulamaların büyük bir kısmı burada bulunur.

Örneğin:

- Wireshark
- Nmap
- SQLMap
- Firefox
- Vim

Önemli alt dizinler:

```text
/usr/bin
```

Kullanıcı komutları

```text
/usr/lib
```

Program kütüphaneleri

```text
/usr/share
```

Dokümantasyon ve ortak dosyalar

---

# 📌 /lib - /lib32 - /lib64

## Paylaşımlı Kütüphaneler

Programların çalışması için gerekli olan ortak kütüphane dosyaları burada bulunur.

Linux'taki `.dll` benzeri dosyalardır.

Genellikle

```text
.so
```

uzantısına sahiptirler.

Örneğin;

- PHP
- PostgreSQL
- apt
- Hashcat

gibi uygulamalar bu kütüphaneleri kullanır.

---

# 📌 /dev

## Aygıt Dosyaları

Linux'ta **her şey bir dosyadır** mantığı burada görülür.

Donanımlar dosya olarak temsil edilir.

Örnekler:

```text
/dev/sda
```

Disk

```text
/dev/null
```

Boş aygıt

```text
/dev/random
```

Rastgele sayı üreticisi

```text
/dev/tty
```

Terminal

---

# 📌 /proc

## İşlem Bilgileri

Diskte fiziksel olarak bulunmayan sanal dosya sistemidir.

Kernel tarafından oluşturulur.

İçerisinde;

- çalışan işlemler
- işlemci bilgileri
- RAM kullanımı
- sistem durumu

gibi bilgiler bulunur.

---

# 📌 /sys

## Kernel ve Donanım Bilgileri

Yine sanal dosya sistemidir.

Donanım sürücüleri ve kernel ayarları ile etkileşim kurmayı sağlar.

---

# 📌 /mnt

## Manuel Bağlama Noktası

Sistem yöneticilerinin geçici olarak disk veya dosya sistemlerini bağlamak için kullandığı dizindir.

Örnek:

```bash
mount /dev/sdb1 /mnt
```

---

# 📌 /media

## Otomatik Bağlama Noktası

USB bellek, CD/DVD gibi harici aygıtlar otomatik olarak burada bağlanır.

Örnek:

```text
/media/ersin/USB
```

---

# 📌 /opt

## Üçüncü Parti Yazılımlar

APT dışındaki yöntemlerle kurulan büyük uygulamalar burada tutulur.

Örneğin;

- Google Chrome
- Discord
- VMware
- IntelliJ IDEA

---

# 📌 /boot

## Açılış Dosyaları

Sistemin açılması için gerekli dosyalar burada bulunur.

İçerisinde;

- Linux Kernel (`vmlinuz`)
- GRUB Bootloader
- initramfs

dosyaları yer alır.

> ⚠️ Bu dizindeki kritik dosyaların silinmesi sistemin açılmamasına neden olabilir.

---

# 📚 Özet Tablosu

| Dizin | Açıklama |
|--------|----------|
| `/` | Kök dizin |
| `/bin` | Temel kullanıcı komutları |
| `/sbin` | Sistem yönetim komutları |
| `/etc` | Yapılandırma dosyaları |
| `/root` | Root kullanıcısının ev dizini |
| `/home` | Kullanıcı ev dizinleri |
| `/var` | Loglar ve değişken dosyalar |
| `/tmp` | Geçici dosyalar |
| `/usr` | Kullanıcı uygulamaları |
| `/lib` | Paylaşımlı kütüphaneler |
| `/dev` | Donanım dosyaları |
| `/proc` | Çalışan işlemler |
| `/sys` | Kernel ve donanım bilgileri |
| `/mnt` | Manuel mount noktası |
| `/media` | Otomatik mount noktası |
| `/opt` | Üçüncü parti yazılımlar |
| `/boot` | Açılış dosyaları |
