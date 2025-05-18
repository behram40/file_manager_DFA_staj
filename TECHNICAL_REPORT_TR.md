# Teknik Rapor: Dosya Yönetim Web Uygulaması

## 1. Sistem Genel Bakış

### 1.1 Amaç
Dosya Yönetim Web Uygulaması, kullanıcılara güçlü kimlik doğrulama ve yetkilendirme mekanizmaları ile dosyalarını (PDF, PNG, JPG) yönetme imkanı sağlayan güvenli, tam yığın bir web uygulamasıdır.

### 1.2 Temel Özellikler
- Kullanıcı kaydı ve kimlik doğrulama
- Güvenli dosya yükleme ve yönetimi
- Çoklu dosya türü desteği (PDF, PNG, JPG)
- Kullanıcıya özel dosya depolama
- Görüntüler için gerçek zamanlı önizleme
- Duyarlı web arayüzü

## 2. Teknik Mimari

### 2.1 Teknoloji Yığını
- **Backend Framework**: Flask (Python)
- **Veritabanı**: SQLite ve SQLAlchemy ORM
- **Frontend**: HTML, Bootstrap 5, JavaScript
- **Kimlik Doğrulama**: JWT (JSON Web Tokens) + Flask-Login
- **Güvenlik**: Werkzeug (şifre hashleme)

### 2.2 Sistem Bileşenleri
```
file_manager/
├── app.py              # Ana Flask uygulaması
├── database.py         # Veritabanı modelleri ve kurulumu
├── requirements.txt    # Python bağımlılıkları
├── uploads/           # Yüklenen dosyaların dizini
├── static/            # Statik dosyalar (CSS, JS)
└── templates/         # HTML şablonları
    ├── base.html      # Temel şablon
    ├── login.html     # Giriş sayfası
    ├── register.html  # Kayıt sayfası
    └── dashboard.html # Dosya yönetim paneli
```

## 3. Veri Modeli

### 3.1 Veritabanı Şeması

#### Kullanıcı Tablosu
```sql
CREATE TABLE user (
    id INTEGER PRIMARY KEY,
    username VARCHAR(80) UNIQUE NOT NULL,
    password_hash VARCHAR(120) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### Dosya Tablosu
```sql
CREATE TABLE file (
    id INTEGER PRIMARY KEY,
    filename VARCHAR(255) NOT NULL,
    original_filename VARCHAR(255) NOT NULL,
    file_type VARCHAR(10) NOT NULL,
    upload_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES user (id)
);
```

### 3.2 Dosya Depolama
- Fiziksel dosyalar `uploads` dizininde saklanır
- Dosyalar benzersizlik için kullanıcı adı ile öneklenir
- Maksimum dosya boyutu: 16MB
- Desteklenen formatlar: PDF, PNG, JPG/JPEG

## 4. Güvenlik Uygulaması

### 4.1 Kimlik Doğrulama
- 1 saatlik token süresi olan JWT tabanlı kimlik doğrulama
- Oturum yönetimi için Flask-Login
- Werkzeug güvenlik fonksiyonları ile şifre hashleme
- Güvenli şifre gereksinimleri:
  - En az 10 karakter
  - En az bir büyük harf
  - En az bir küçük harf
  - En az bir özel karakter

### 4.2 Yetkilendirme
- Kullanıcı sahipliği doğrulaması ile dosya erişim kontrolü
- `@login_required` dekoratörü ile korumalı rotalar
- Yetkisiz erişim denemeleri için 403 Yasak yanıtları
- Kullanıcıya özel dosya izolasyonu

### 4.3 Dosya Güvenliği
- Güvenli dosya adı işleme
- Dosya türü doğrulama
- MIME türü doğrulama
- Kullanıcıya özel dosya depolama
- Doğrudan dosya erişimini önleme

## 5. API Uç Noktaları

### 5.1 Kimlik Doğrulama Uç Noktaları
- `POST /register`: Kullanıcı kaydı
- `POST /login`: Kullanıcı kimlik doğrulama
- `GET /logout`: Kullanıcı çıkışı
- `POST /check_username`: Kullanıcı adı kullanılabilirlik kontrolü

### 5.2 Dosya Yönetimi Uç Noktaları
- `GET /dashboard`: Kullanıcı dosya paneli
- `POST /upload`: Dosya yükleme
- `GET /download/<file_id>`: Dosya indirme
- `GET /preview/<file_id>`: Görüntü önizleme
- `POST /delete/<file_id>`: Dosya silme

## 6. Frontend Uygulaması

### 6.1 Kullanıcı Arayüzü
- Bootstrap 5 ile duyarlı tasarım
- Gerçek zamanlı form doğrulama
- Etkileşimli dosya yönetim paneli
- Modal tabanlı görüntü önizlemeleri
- Flash mesajları ile kullanıcı geri bildirimi

### 6.2 İstemci Tarafı Özellikler
- Gerçek zamanlı kullanıcı adı kullanılabilirlik kontrolü
- Şifre gücü doğrulama
- Şifre görünürlük değiştirme
- Dosya türü doğrulama
- Silme işlemleri için onay diyalogları

## 7. Performans Değerlendirmeleri

### 7.1 Veritabanı Optimizasyonu
- İndekslenmiş birincil anahtarlar
- Yabancı anahtar kısıtlamaları
- Verimli sorgu desenleri
- İlişkilerin tembel yüklenmesi

### 7.2 Dosya İşleme
- Dosya boyutu sınırları (16MB)
- Asenkron dosya işlemleri
- Verimli dosya türü kontrolü
- Güvenli dosya depolama yapısı

## 8. Hata Yönetimi

### 8.1 İstemci Tarafı Doğrulama
- Form giriş doğrulama
- Dosya türü doğrulama
- Şifre gereksinim kontrolü
- Kullanıcı adı kullanılabilirlik kontrolü

### 8.2 Sunucu Tarafı Doğrulama
- Veritabanı kısıtlama uygulaması
- Dosya türü doğrulama
- Kimlik doğrulama kontrolleri
- Yetkilendirme doğrulaması

### 8.3 Hata Yanıtları
- 400 Hatalı İstek: Geçersiz giriş
- 401 Yetkisiz: Kimlik doğrulama gerekli
- 403 Yasak: Yetkilendirme reddedildi
- 404 Bulunamadı: Kaynak bulunamadı
- 413 İstek Gövdesi Çok Büyük: Dosya boyutu aşıldı

## 9. Gelecek İyileştirmeler

### 9.1 Potansiyel Geliştirmeler
- Dosya şifreleme
- İki faktörlü kimlik doğrulama
- Dosya paylaşım özellikleri
- Dosya sürüm oluşturma
- Bulut depolama entegrasyonu
- API hız sınırlama
- Gelişmiş dosya önizleme özellikleri
- Toplu dosya işlemleri

### 9.2 Ölçeklenebilirlik Değerlendirmeleri
- PostgreSQL'e veritabanı geçişi
- Bulut depolamaya dosya depolama geçişi
- Önbellek uygulaması
- Yük dengeleme
- Statik dosyalar için CDN entegrasyonu

## 10. Sonuç

Dosya Yönetim Web Uygulaması, kullanıcı dosya yönetimi için güvenli ve verimli bir çözüm sunmaktadır. Uygulama, güvenlik en iyi uygulamalarını takip eder ve gelecekteki geliştirmeler için sağlam bir temel sağlar. Sistemin mimarisi, kullanıcı dostu bir arayüzü korurken veri bütünlüğünü, kullanıcı gizliliğini ve güvenli dosya işlemeyi sağlar.

---

*Not: Bu teknik rapor mevcut uygulamaya dayanmaktadır ve sistem geliştikçe güncellenebilir.* 