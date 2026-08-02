# 👤 Kullanıcı ve Grup Yönetimi

Linux'ta kullanıcı ve grup işlemleri sistem yöneticisinin en sık yaptığı işlemlerden biridir.

---

# ➕ Kullanıcı Ekleme

Yeni bir kullanıcı oluşturur.

```bash
useradd Ahmet
```

Kullanıcı oluşturulduktan sonra şifre belirlemek için:

```bash
passwd Ahmet
```

> 💡 Ubuntu gibi dağıtımlarda `adduser Ahmet` komutu daha kullanıcı dostudur ve gerekli bilgileri adım adım ister.

---

# ❌ Kullanıcı Silme

Kullanıcıyı sistemden siler.

```bash
userdel Ahmet
```

Kullanıcının ev dizinini de silmek için:

```bash
userdel -r Ahmet
```

---

# ➕ Grup Oluşturma

Yeni bir grup oluşturur.

```bash
groupadd Security
```

---

# ❌ Grup Silme

Grubu sistemden siler.

```bash
groupdel Security
```

---

# 👥 Kullanıcıyı Gruba Ekleme

Bir kullanıcıyı mevcut bir gruba ekler.

```bash
usermod -aG Security Ahmet
```

### Parametreler

| Parametre | Açıklama |
|-----------|----------|
| `-a` | Mevcut grupları koruyarak ekleme yapar. |
| `-G` | Eklenecek ek grubu belirtir. |

> ⚠️ `-a` kullanılmazsa kullanıcı diğer gruplardan çıkarılabilir.

---

# 🚪 Kullanıcıyı Gruptan Çıkarma

Bir kullanıcıyı belirtilen gruptan çıkarır.

```bash
gpasswd -d Ahmet Security
```

---

# 📄 Kullanıcıları Görüntüleme

Sistemdeki kullanıcı hesaplarını listeler.

```bash
cat /etc/passwd
```

Daha okunabilir şekilde yalnızca kullanıcı adlarını görmek için:

```bash
cut -d: -f1 /etc/passwd
```

---

# 📄 Şifre Bilgileri

Kullanıcıların parola bilgileri (hash) burada tutulur.

```bash
cat /etc/shadow
```

> 🔒 Sadece root kullanıcısı görüntüleyebilir.

---

# 📄 Grupları Görüntüleme

Sistemde bulunan grupları listeler.

```bash
cat /etc/group
```

---

# 🔑 Sudo Yetkileri

Sudo kullanabilen kullanıcılar ve gruplar **sudoers** dosyasında tanımlanır.

Bu dosya doğrudan düzenlenmemelidir.

Düzenlemek için:

```bash
visudo
```

Örnek:

```text
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

---

## Satırların Anlamı

```text
root ALL=(ALL:ALL) ALL
```

Root kullanıcısı tüm komutları tüm kullanıcılar adına çalıştırabilir.

```text
%sudo ALL=(ALL:ALL) ALL
```

**sudo** grubuna dahil olan kullanıcılar da tüm komutları çalıştırabilir.

---

# 👤 Bir Kullanıcıya Sudo Yetkisi Vermek

Kullanıcıyı **sudo** grubuna eklemek yeterlidir.

```bash
usermod -aG sudo Ahmet
```

veya

```bash
adduser Ahmet sudo
```

---

# 📚 Faydalı Komutlar

Mevcut kullanıcı:

```bash
whoami
```

Oturum açmış kullanıcılar:

```bash
who
```

Kullanıcının UID ve grup bilgileri:

```bash
id Ahmet
```

Kullanıcının hangi gruplarda olduğunu görmek:

```bash
groups Ahmet
```

---

# 📝 Özet

| Komut | Görevi |
|--------|---------|
| `useradd` | Kullanıcı oluşturur |
| `userdel` | Kullanıcı siler |
| `passwd` | Kullanıcı şifresi oluşturur/değiştirir |
| `groupadd` | Grup oluşturur |
| `groupdel` | Grup siler |
| `usermod -aG` | Kullanıcıyı gruba ekler |
| `gpasswd -d` | Kullanıcıyı gruptan çıkarır |
| `cat /etc/passwd` | Kullanıcıları listeler |
| `cat /etc/group` | Grupları listeler |
| `cat /etc/shadow` | Şifre hash bilgilerini gösterir |
| `visudo` | Sudo yetkilerini düzenler |
