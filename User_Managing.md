
Kullanıcı ekleme
```text
┌──(root㉿kali)-[~]
└─# useradd Ahmet  

```
Kullanıcı Silme
```
┌──(root㉿kali)-[~]
└─# userdel Ahmet
```
Grup ekleme
```
┌──(root㉿kali)-[~]
└─# groupadd Security 
```
Grup Silme
```
┌──(root㉿kali)-[~]
└─# groupdel Security 

```
Kullanıcıyı Gruba Dahil Etme
```
┌──(root㉿kali)-[~]
└─# sudo usermod -aG Security Ahmet 

```
Kullanıcıyı Gruptan Çıkarma
```
┌──(root㉿kali)-[~]
└─# passwd -d Ahmet Security 

```

cat /etc/shadow ==> Mevcut Kullanılar görtüntülenir. 
cat /etc/group  ==> Mevcut Gruplar görtüntülenir. 

Yetkili Kullanıcı Ve Sudo Komudunu Çalıştırabilen Kullanıcıların bulunduğu config dosyasına Aşağıdaki komut aracılığı ile erişilir.

```
┌──(root㉿kali)-[~]
└─# visudo 

# User privilege specification 
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

```




