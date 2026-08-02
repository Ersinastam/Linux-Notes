# 📦 Arşivleme (tar)

Linux'ta dosya ve klasörleri arşivlemek için en yaygın kullanılan araç **tar** programıdır.

> 💡 **Tar tek başına dosyaları sıkıştırmaz.** Sadece birden fazla dosya veya klasörü tek bir arşiv (`.tar`) dosyasında toplar. Sıkıştırma işlemi için `gzip`, `bzip2` veya `xz` gibi araçlar kullanılır.

---

# 📁 Sadece Arşiv Oluşturma

Bir veya birden fazla dosyayı tek bir `.tar` dosyasında toplar.

```bash
tar -cf yedekler.tar dosya1 dosya2 klasor1
```

Örnek:

```bash
tar -cf belgeler.tar belge1.txt belge2.txt resimler/
```

---

# 📦 Arşiv + Gzip ile Sıkıştırma

Arşiv oluşturur ve aynı anda **gzip** ile sıkıştırır.

```bash
tar -zcf arsiv.tar.gz klasor_adi
```

Örnek:

```bash
tar -zcf proje.tar.gz proje/
```

Oluşan dosya:

```text
proje.tar.gz
```

---

# 📦 Arşiv + Bzip2 ile Sıkıştırma

Daha yüksek sıkıştırma oranı sağlar ancak gzip'e göre daha yavaştır.

```bash
tar -jcf arsiv.tar.bz2 klasor_adi
```

---

# 📦 Arşiv + XZ ile Sıkıştırma

En yüksek sıkıştırma oranlarından birini sunar.

```bash
tar -Jcf arsiv.tar.xz klasor_adi
```

---

# 📂 Arşiv İçeriğini Listeleme

Arşivi çıkarmadan içeriğini görüntüler.

```bash
tar -tf arsiv.tar
```

Sıkıştırılmış arşiv için:

```bash
tar -ztf arsiv.tar.gz
```

---

# 📤 Arşiv Çıkarma

### Normal `.tar` dosyası

```bash
tar -xf arsiv.tar
```

### `.tar.gz` dosyası

```bash
tar -zxvf arsiv.tar.gz
```

### `.tar.bz2` dosyası

```bash
tar -jxvf arsiv.tar.bz2
```

### `.tar.xz` dosyası

```bash
tar -Jxvf arsiv.tar.xz
```

---

# 🔤 Tar Parametreleri

| Parametre | Anlamı |
|-----------|---------|
| `c` | **Create** → Yeni arşiv oluşturur. |
| `x` | **Extract** → Arşivi çıkarır. |
| `t` | **List** → Arşiv içeriğini listeler. |
| `f` | **File** → Arşiv dosyasının adını belirtir. |
| `v` | **Verbose** → İşlem sırasında yapılanları ekrana yazar. |
| `z` | **gzip** → Gzip ile sıkıştırır veya açar. |
| `j` | **bzip2** → Bzip2 ile sıkıştırır veya açar. |
| `J` | **xz** → XZ ile sıkıştırır veya açar. |

---

# 📝 En Sık Kullanılan Komutlar

| Komut | Açıklama |
|--------|----------|
| `tar -cf arsiv.tar dosya` | Arşiv oluşturur. |
| `tar -zcf arsiv.tar.gz dosya` | Gzip ile sıkıştırılmış arşiv oluşturur. |
| `tar -jcf arsiv.tar.bz2 dosya` | Bzip2 ile sıkıştırılmış arşiv oluşturur. |
| `tar -Jcf arsiv.tar.xz dosya` | XZ ile sıkıştırılmış arşiv oluşturur. |
| `tar -tf arsiv.tar` | Arşiv içeriğini listeler. |
| `tar -xf arsiv.tar` | Arşivi çıkarır. |
| `tar -zxvf arsiv.tar.gz` | Gzip arşivini çıkarır. |
| `tar -jxvf arsiv.tar.bz2` | Bzip2 arşivini çıkarır. |
| `tar -Jxvf arsiv.tar.xz` | XZ arşivini çıkarır. |
