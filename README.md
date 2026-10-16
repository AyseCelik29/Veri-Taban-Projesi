# Otel Rezervasyon ve Konaklama Yönetim Sistemi

Proje, bir otelin oda, oda tipi, misafir, rezervasyon, konaklama, ödeme, ek hizmet ve personel süreçlerini yöneten kapsamlı bir ilişkisel veritabanı ve web uygulamasıdır.

##  Proje Ekibi
* **Proje Başkanı  :** 455118 Ayşe Çelik
* **Proje Üyesi    :** 455096 Elif Sudem Kurt Kurt
* **Ders Yürütücüsü:** Arş. Gör. Dr. Hakan Aydın
* **Ders:          :** Veritabanı ve Yönetimi

---

##  Projenin Amacı ve Kapsamı
Projenin amacı; İlişkisel veritabanı tasarımı, 3NF normalizasyonu, çok tablolu SQL sorguları, tarih aralığı kontrolü, **TRIGGER**, **STORED PROCEDURE**, **VIEW**, **TRANSACTION** ve **indeksleme** gibi veritabanı kavramlarını uygulamalı olarak öğrenmek; bu yapıyı sağlam bir **Backend REST API** ve kullanıcı dostu bir **Frontend** arayüzü ile desteklemektir.

---

##  Kullanılan Teknolojiler
* **Veritabanı:** PostgreSQL 
* **Backend:** Spring Boot (Java)  
* **Frontend:** HTML, CSS, JavaScript 
* **Güvenlik:** JWT (JSON Web Token) tabanlı rol bazlı yetkilendirme (Misafir, Resepsiyon, Yönetici)

---

##  Veritabanı Mimarisi ve Tasarımı

### 1. Tablolar 
Projede veri bütünlüğünü sağlamak amacıyla en az 3. Normal Form (3NF) seviyesinde tasarlanmış şu tablolar yer almaktadır:
1. `OdaTipi`
2. `Oda`
3. `SezonFiyati`
4. `Misafir`
5. `Kullanici`
6. `Personel`
7. `Rezervasyon`
8. `Konaklama`
9. `EkHizmet`
10. `KonaklamaHizmeti`
11. `Odeme`
12. `TemizlikKaydi`
13. `IslemLog`

### 2. Kritik İş Kuralları ve Veritabanı Nesneleri
* **3 TRIGGER:**
  1. *Çakışan Tarih Kontrolü:* Aynı odaya aynı tarihlerde iki kez rezervasyon yapılmasını veritabanı seviyesinde engeller.
  2. *Oda Durum Güncellemesi:* Check-in yapıldığında odanın durumu "Dolu", check-out yapıldığında ise "Temizlikte" olarak otomatik güncellenir.
  3. *Loglama:* Rezervasyon ve ödeme değişiklikleri otomatik olarak `IslemLog` tablosuna işlenir.

* **3 STORED PROCEDURE:**
  1. Verilen tarih aralığına ve oda tipine göre boş odaları listeleyen prosedür.
  2. Konaklama toplam fiyatını (sezon fiyatları + ek hizmetler dahil) hesaplayan prosedür.
  3. Aylık doluluk oranı ve gelir raporunu çıkaran prosedür.

* **2 VIEW:**
  1. Bugün giriş ve çıkış yapacak misafirleri listeleyen görünüm.
  2. Odanın güncel durumu ve son temizlik tarihini gösteren görünüm.

* **TRANSACTION Yönetimi:**
  * Check-out işlemi tek bir `TRANSACTION` içinde olur: Konaklama kapatılır, ödeme kaydedilir ve oda durumu güncellenir. Adımlardan herhangi birinde hata olması durumunda `ROLLBACK` ile veritabanı güvenliği sağlanır.

* **İndeksleme:**
  * Rezervasyon tarihleri, oda numarası ve misafir e-postası üzerine performans artırıcı indeksler tanımlanmıştır.

---

##  Roller ve Yetkiler (JWT)
* **Misafir:** Kayıt olma/giriş, boş oda arama, rezervasyon yapma/iptal etme ve geçmiş konaklamaları görüntüleme.
* **Resepsiyon:** Günlük giriş/çıkış listesi takibi, check-in/check-out işlemleri, ek hizmet ve ödeme alma.
* **Yönetici:** Oda, oda tipi, sezon fiyatı, ek hizmet ve personel yönetim (CRUD) işlemleri ile aylık raporlara erişim.

---

##  Dosya Düzeni ve Kurulum
'''text
├── database/          # SQL betikleri (Tablolar, Triggerlar, Prosedürler, Örnek Veriler)
├── backend/           # API kaynak kodları (Katmanlı mimari)
├── frontend/          # Arayüz dosyaları
├── docs/              # ER Diyagramı ve test senaryoları raporu
└── README.md