# Linux Servis Yönetimi

Linux'ta bazı programların sistem açıldığında otomatik olarak başlamasını ve arka planda çalışmasını isteriz.

Örneğin:

- SSH
- Web Server
- DNS Server
- Network servisleri

gibi programlar servis olarak çalışabilir.

Linux'ta servis yönetimi için günümüzde çoğunlukla **systemd** kullanılır.

Temel araç:

```bash
systemctl
```

---

## 1. Service Nedir?

**Service (servis)**, genellikle arka planda çalışan ve belirli bir işi gerçekleştiren programdır.

Örneğin SSH servisi, başka bilgisayarların SSH üzerinden sisteme bağlanmasını sağlar.

```text
SSH Client
    ↓
SSH Server
    ↓
sshd service
```

Servisler genellikle kullanıcı doğrudan başlatmadan arka planda çalışır.

---

## 2. systemd Nedir?

**systemd**, Linux sisteminin açılışını ve sistem servislerinin yönetimini sağlayan sistem yöneticisidir.

Linux açıldığında temel olarak:

```text
Bilgisayar açılır
      ↓
Kernel
      ↓
systemd
      ↓
Servisler başlatılır
```

systemd ile servisleri yönetmek için:

```bash
systemctl
```

komutu kullanılır.

---

## 3. Servisleri Listelemek

Sistemdeki servisleri görmek için:

```bash
systemctl --type=service
```

kullanılabilir.

Servisleri listelemek için:

```bash
systemctl list-units --type=service
```

kullanılabilir.

---

## 4. Servisin Durumunu Kontrol Etmek

Bir servisin durumunu görmek için:

```bash
systemctl status ssh
```

kullanılır.

Örneğin:

```text
Active: active (running)
```

görürsek SSH servisi çalışıyor demektir.

Temel durumlar:

```text
active   → Servis çalışıyor
inactive → Servis çalışmıyor
failed   → Servis başlatılırken hata oluşmuş
```

---

## 5. Servis Başlatmak

Bir servisi başlatmak için:

```bash
sudo systemctl start ssh
```

kullanılır.

Bu komut:

> SSH servisini şimdi başlat.

anlamına gelir.

Kontrol etmek için:

```bash
systemctl status ssh
```

---

## 6. Servis Durdurmak

Bir servisi durdurmak için:

```bash
sudo systemctl stop ssh
```

kullanılır.

Bu komut servisi durdurur.

Kontrol:

```bash
systemctl status ssh
```

---

## 7. Servisi Yeniden Başlatmak

Bir servisi yeniden başlatmak için:

```bash
sudo systemctl restart ssh
```

kullanılır.

Bu işlem:

```text
Servisi durdur
      ↓
Servisi tekrar başlat
```

şeklinde düşünülebilir.

Özellikle servis yapılandırmasında değişiklik yaptıktan sonra kullanılabilir.

---

## 8. Reload Nedir?

Bazı servislerde yapılandırma değişikliğini servisi tamamen kapatıp açmadan yeniden okutabiliriz.

```bash
sudo systemctl reload ssh
```

Burada servis çalışmaya devam ederken yapılandırma yeniden yüklenebilir.

Ancak her servis `reload` işlemini desteklemeyebilir.

---

## 9. Start ve Enable Farkı

Burada önemli bir fark vardır.

### start

```bash
sudo systemctl start ssh
```

Servisi **şimdi** başlatır.

### enable

```bash
sudo systemctl enable ssh
```

Servisin **sistem açıldığında otomatik olarak başlamasını** sağlar.

Yani:

```text
start
 ↓
Şimdi başlat
```

```text
enable
 ↓
Sistem açıldığında otomatik başlat
```

---

## 10. Enable ve Start Birlikte

Bir servisi hem şimdi başlatmak hem de sistem açıldığında otomatik başlamasını sağlamak için:

```bash
sudo systemctl enable --now ssh
```

kullanılabilir.

---

## 11. Disable

Bir servisin sistem açıldığında otomatik başlamasını engellemek için:

```bash
sudo systemctl disable ssh
```

kullanılır.

Bu komut servisi o anda durdurmaz.

Sadece:

> Sistem açıldığında otomatik başlama.

ayarını kaldırır.

Servisi hem durdurmak hem de açılışta başlamasını engellemek için:

```bash
sudo systemctl disable --now ssh
```

kullanılabilir.

---

## 12. Servisin Çalışıp Çalışmadığını Kontrol Etmek

```bash
systemctl is-active ssh
```

Örneğin:

```text
active
```

çıktısı servisin çalıştığını gösterir.

---

## 13. Servisin Açılışta Başlayıp Başlamadığını Kontrol Etmek

```bash
systemctl is-enabled ssh
```

Örneğin:

```text
enabled
```

çıktısı servisin sistem açılışında otomatik olarak başlayacak şekilde ayarlandığını gösterir.

```text
disabled
```

çıktısı otomatik başlamasının kapalı olduğunu gösterir.

Burada:

```text
is-active
    ↓
Şu anda çalışıyor mu?

is-enabled
    ↓
Sistem açıldığında başlayacak mı?
```

şeklinde düşünebiliriz.

---

## 14. Service Dosyaları

systemd tarafından yönetilen servisler genellikle `.service` dosyalarıyla tanımlanır.

Örneğin:

```text
ssh.service
```

Bir servisin dosyasını görmek için:

```bash
systemctl cat ssh
```

kullanılabilir.

---

## 15. Servis Loglarını Görmek

Linux'ta systemd tarafından tutulan logları görmek için:

```bash
journalctl
```

kullanılır.

Belirli bir servisin loglarını görmek için:

```bash
journalctl -u ssh
```

kullanılır.

Son 50 log kaydını görmek için:

```bash
journalctl -u ssh -n 50
```

kullanılabilir.

Logları canlı olarak takip etmek için:

```bash
journalctl -u ssh -f
```

kullanılır.

Buradaki:

```text
-f → Yeni gelen logları canlı olarak takip et
```

anlamına gelir.

---

## 16. Service ve Process Arasındaki Fark

Service ve process birbirine yakın kavramlardır fakat aynı şey değildir.

### Process

Çalışan programın işletim sistemi tarafından yönetilen örneğidir.

```text
Program
   ↓
Process
   ↓
PID
```

### Service

Genellikle arka planda çalışan ve sistem tarafından yönetilen bir program veya uygulamadır.

Örneğin:

```text
ssh.service
     ↓
systemd tarafından yönetilir
     ↓
sshd process'i
     ↓
PID
```

Basitçe:

> **Process = Çalışan program**

> **Service = Sistem tarafından yönetilen arka plan hizmeti**

---

## 17. Service ve Process İlişkisi

Örneğin SSH servisini düşünelim:

```text
ssh.service
     ↓
systemd
     ↓
sshd
     ↓
Process
     ↓
PID
```

Servisin durumunu:

```bash
systemctl status ssh
```

ile kontrol ederiz.

Process'i görmek için:

```bash
ps aux | grep ssh
```

kullanabiliriz.

Yani:

```text
systemctl
    ↓
Service'i yönetir

ps / top
    ↓
Process'i görüntüler

kill
    ↓
Process'e signal gönderir
```

---

## 18. Bir Servis Çalışmıyorsa Ne Yapılır?

Bir servis çalışmıyorsa önce durumuna bakılır:

```bash
systemctl status ssh
```

Eğer:

```text
failed
```

görülüyorsa loglarına bakabiliriz:

```bash
journalctl -u ssh
```

Daha fazla bilgi için:

```bash
journalctl -u ssh -n 50
```

kullanılabilir.

Daha sonra servisi tekrar başlatmayı deneyebiliriz:

```bash
sudo systemctl restart ssh
```

Tekrar kontrol:

```bash
systemctl status ssh
```

Temel sorun giderme mantığı:

```text
Servis çalışmıyor
       ↓
systemctl status
       ↓
Hata mesajını incele
       ↓
journalctl ile logları kontrol et
       ↓
Sorunu düzelt
       ↓
Servisi restart et
       ↓
Tekrar kontrol et
```

---

# 19. Temel Servis Komutları

```bash
systemctl status servis
```

Servisin durumunu gösterir.

```bash
sudo systemctl start servis
```

Servisi başlatır.

```bash
sudo systemctl stop servis
```

Servisi durdurur.

```bash
sudo systemctl restart servis
```

Servisi yeniden başlatır.

```bash
sudo systemctl reload servis
```

Servisin yapılandırmasını yeniden yükler.

```bash
sudo systemctl enable servis
```

Servisin sistem açılışında otomatik başlamasını sağlar.

```bash
sudo systemctl disable servis
```

Servisin sistem açılışında otomatik başlamasını engeller.

```bash
systemctl is-active servis
```

Servisin şu anda çalışıp çalışmadığını kontrol eder.

```bash
systemctl is-enabled servis
```

Servisin açılışta otomatik başlayıp başlamadığını kontrol eder.

```bash
journalctl -u servis
```

Servisin loglarını gösterir.

---

# 20. Uygulama

SSH servisinin durumunu kontrol edelim:

```bash
systemctl status ssh
```

Servisi durduralım:

```bash
sudo systemctl stop ssh
```

Tekrar kontrol edelim:

```bash
systemctl status ssh
```

Servisi tekrar başlatalım:

```bash
sudo systemctl start ssh
```

Tekrar kontrol:

```bash
systemctl status ssh
```

Servisin açılışta otomatik başlayıp başlamadığını kontrol edelim:

```bash
systemctl is-enabled ssh
```

Servisin loglarını görüntüleyelim:

```bash
journalctl -u ssh -n 20
```

---

# Özet

Linux'ta servis yönetiminin temel aracı:

```bash
systemctl
```

Temel yapı:

```text
Linux açılır
     ↓
systemd
     ↓
Servisler
     ↓
Process'ler
     ↓
CPU / RAM kullanımı
```

Önemli kavramlar:

```text
systemd   → Sistem ve servis yöneticisi

systemctl → systemd ile servisleri yönetmemizi sağlayan komut

service   → Arka planda çalışan ve sistem tarafından yönetilen hizmet

process   → Çalışan program örneği
```

En önemli komutlar:

```bash
systemctl status
systemctl start
systemctl stop
systemctl restart
systemctl reload
systemctl enable
systemctl disable
systemctl is-active
systemctl is-enabled
journalctl -u
```

En önemli ayrım:

```text
start
 ↓
Servisi şimdi başlatır

enable
 ↓
Sistem açıldığında otomatik başlatır
```

ve:

```text
Process
 ↓
Çalışan program

Service
 ↓
Sistem tarafından yönetilen arka plan hizmeti
```
