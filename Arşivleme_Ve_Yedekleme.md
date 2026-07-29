### ARŞİVLEME

tar programı kullanılır

Dosya arşivleme
birden fazla dosyanın tek çatı(dosya)altında toplamaktır. sıkıştırma işlemi yapılmaz!

tar -cf yedekler.tar(yedeleklendikten sonra sorsayın adı) yedeklenecek dosyalar
Arşiv+sıkıştırmak için

tar -zcf arsiv.tar(yeni arşiv ve ziplenecek  dosyası) klasor_adi(arşive ve ziplenecek dosyaların adı)

Parametrelerin anlamları:
c = create → Yeni bir arşiv oluştur.
z = gzip → Arşivi gzip ile sıkıştır. → tar -zcf
f = file → Arşivin dosya adını belirt.
j = bizip2 → Arşivi bizip2 ile sıkıştır. → tar -jcf
---


