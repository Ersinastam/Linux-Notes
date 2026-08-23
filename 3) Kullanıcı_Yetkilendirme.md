# 🔐 Kullanıcı Yetkilendirme (Permissions)

Linux'ta her dosya ve klasörün bir **sahibi (Owner)**, bir **grubu (Group)** ve **erişim izinleri (Permissions)** vardır.

İzinler sayesinde hangi kullanıcının dosyayı okuyabileceği, yazabileceği veya çalıştırabileceği belirlenir.

---

# 👤 Yetki Türleri

Her dosya için üç farklı kullanıcı grubu bulunur.

| Kısaltma | Açıklama |
|----------|----------|
| **u (User)** | Dosyanın sahibi |
| **g (Group)** | Dosyanın ait olduğu grup |
| **o (Others)** | Diğer kullanıcılar |

---

# 🔑 İzin Türleri

| Harf | Anlamı | Sayısal Değeri |
|------|---------|---------------:|
| `r` | Read (Okuma) | 4 |
| `w` | Write (Yazma) | 2 |
| `x` | Execute (Çalıştırma) | 1 |

---

# 📄 Dosya İzinlerini Görüntüleme

```bash
ls -l
```

Örnek çıktı:

```text
-rwxr-xr-- 1 root security 1250 Jul 31 script.sh
```

### Açıklaması

```text
-rwxr-xr--
```

| Bölüm | Anlamı |
|--------|--------|
| `-` | Normal dosya |
| `rwx` | Sahibi: Okuma, Yazma, Çalıştırma |
| `r-x` | Grup: Okuma, Çalıştırma |
| `r--` | Diğerleri: Sadece okuma |

---

# 🔢 Sayısal Yetkilendirme

Her izin bir sayı ile temsil edilir.

| İzin | Değer |
|------|------:|
| r | 4 |
| w | 2 |
| x | 1 |

Toplanarak yetki oluşturulur.

| Sayı | Yetki |
|------:|--------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 3 | -wx |
| 2 | -w- |
| 1 | --x |
| 0 | --- |

---

# ✏️ chmod Komutu

Dosya veya klasör izinlerini değiştirir.

## Sayısal Kullanım

```bash
chmod 755 script.sh
```

Anlamı

| Kullanıcı | Yetki |
|------------|--------|
| Owner | rwx |
| Group | r-x |
| Others | r-x |

---

```bash
chmod 644 belge.txt
```

Anlamı

| Kullanıcı | Yetki |
|------------|--------|
| Owner | rw- |
| Group | r-- |
| Others | r-- |

---

# 🔤 Harflerle Yetki Verme

Yetki eklemek

```bash
chmod u+x script.sh
```

Sahibine çalıştırma yetkisi verir.

---

Gruba yazma yetkisi vermek

```bash
chmod g+w dosya.txt
```

---

Diğer kullanıcılardan okuma yetkisini kaldırmak

```bash
chmod o-r dosya.txt
```

---

Herkese çalıştırma yetkisi vermek

```bash
chmod a+x script.sh
```

> `a` = all (user + group + others)

---

# 👤 Dosya Sahibini Değiştirme

```bash
chown Ahmet dosya.txt
```

Dosyanın sahibini değiştirir.

---

Sahip ve grubu aynı anda değiştirmek

```bash
chown Ahmet:Security dosya.txt
```

---

# 👥 Dosyanın Grubunu Değiştirme

```bash
chgrp Security dosya.txt
```

---

# 📁 Klasör ve Altındaki Dosyalara Yetki Vermek

Recursive (özyinelemeli) işlem yapmak için:

```bash
chmod -R 755 proje/
```

Sahibini değiştirmek:

```bash
chown -R Ahmet:Security proje/
```

---

# 🔐 Sudo Yetkisi

Bir kullanıcıya yönetici yetkisi vermek için kullanıcı **sudo** grubuna eklenebilir.

```bash
usermod -aG sudo Ahmet
```

Kullanıcının gruplarını görmek:

```bash
groups Ahmet
```

veya

```bash
id Ahmet
```

---

# 📌 Faydalı Komutlar

Dosya izinlerini görüntüle

```bash
ls -l
```

Bulunduğun kullanıcı

```bash
whoami
```

Kullanıcının UID ve grupları

```bash
id
```

Aktif kullanıcı

```bash
who
```

---

# 📝 En Sık Kullanılan Yetkiler

| Yetki | Açıklama |
|--------|----------|
| `777` | Herkes tam yetkili (**önerilmez**) |
| `755` | Owner tam yetkili, diğerleri okuyup çalıştırabilir |
| `775` | Owner ve grup tam yetkili |
| `750` | Sadece owner ve grup erişebilir |
| `700` | Sadece owner erişebilir |
| `644` | Dosyalar için en yaygın izin |
| `600` | Sadece owner okuyabilir ve yazabilir |

---

# ⚠️ Güvenlik Notları

- `777` yetkisi güvenlik açısından önerilmez.
- Gereksiz yere root kullanıcısıyla işlem yapılmamalıdır.
- En az yetki (**Principle of Least Privilege**) prensibi uygulanmalıdır.
- Sistem dosyalarının izinlerini rastgele değiştirmek sistemin çalışmasını bozabilir.
