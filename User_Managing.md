
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



