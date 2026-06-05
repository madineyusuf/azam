# Hediye
# NoteApp - Güvenlik Açığı Analizi ve Düzeltme
**Hazırlayan: Madina Yusupova - 24360859922**

| No | Güvenlik Açığı | Hangi dosyalar? | Açıklama | Neler değişmeli? |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Zayıf Şifre Hashleme**  | `install.php`, `login.php`, `register.php` | Şifreler sadece ilk 5 karakteri alınarak tuzsuz SHA-256 ile güvensiz şekilde özetleniyordu | Budama işlemi kaldırılıp PHP'nin güvenli `password_hash(BCRYPT)` ve `password_verify()` fonksiyonları entegre edilmeli |
| **2** | **Çerez Tabanlı Kimlik Doğrulama**  | `index.php`, `addnote.php`, `editnote.php`, `login.php` | Kullanıcı oturumu sadece tarayıcıdaki güvensiz bir çerez değerine (kullanıcı ID'sine) dayandırılıyordu | Çerez tabanlı doğrulama tamamen kaldırılarak sunucu taraflı güvenli PHP `$_SESSION` mekanizmasına geçilmeli|
| **3** | **CSRF Korumasız Oturum Kapatma** | `logout.php`, `index.php` | Oturum kapatma işlemi, hiçbir doğrulama parametresi içermeyen düz bir GET isteğiyle çalışıyordu| Oturum kapatma süreci güvenli session imhası ve oturum sabitlemeyi önleyen `session_regenerate_id(true)` ile yeniden yapılmalı|
| **4** | **Sahiplik Kontrolsüz Erişim**  | `editnote.php`, `index.php` | URL'den gelen `noteid` parametresi, notun istek atan kullanıcıya ait olup olmadığı kontrol edilmeden işleniyordu | Veritabanı sorgularına `user_id` şartı eklenerek (`WHERE id = ? AND user_id = ?`) sadece not sahibinin erişim sağlaması garanti edilmeli |
| **5** | **Hassas Verilerin URL'e Sızması**  | `login.php` | Giriş formunda GET metodu kullanıldığı için kullanıcı paroları URL adres çubuğunda açıkça sızıyordu| Giriş formunun metodolojisi `POST` olarak değiştirilerek hassas verilerin HTTP istek gövdesinde güvenli taşınması sağlanmalı|


