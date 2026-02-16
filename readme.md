# Stardoll Automation & Management Suite

![Project Status](https://img.shields.io/badge/Status-Production-green)
![Tech Stack](https://img.shields.io/badge/Stack-Node.js%20%7C%20Puppeteer%20%7C%20Express-blue)
![Type](https://img.shields.io/badge/Type-Freelance%20%2F%20Closed%20Source-red)

> **Not:** Bu proje, bir müşteri için özel olarak geliştirilmiş ticari bir otomasyon yazılımıdır. Gizlilik anlaşması gereği kaynak kodları paylaşılmamaktadır. Bu dokümantasyon, projenin teknik mimarisini ve yeteneklerini sergilemek amacıyla hazırlanmıştır.

## 🎯 Proje Özeti

Bu proje, Stardoll platformu üzerinde kullanıcı etkileşimlerini otomatize eden, yüksek performanslı ve ölçeklenebilir bir **RPA (Robotic Process Automation)** çözümüdür. Sistem, oylama süreçlerini, hesap yönetimini ve veri madenciliğini (crawling) tek bir merkezi yönetim panelinden kontrol etmeyi sağlar.

Geleneksel tarayıcı otomasyonunun ötesine geçerek, **Hybrid Request/Browser** mimarisi ile kaynak tüketimini minimize ederken işlem hızını maksimize eder.

## 🛠️ Teknik Mimari ve Kullanılan Teknolojiler

Proje, modüler bir yapıda **Node.js** üzerinde geliştirilmiştir.

*   **Backend:** Node.js, Express.js (REST API & Web Panel Sunucusu)
*   **Otomasyon & Scraping:**
    *   **Puppeteer:** Karmaşık oturum açma işlemleri, token yakalama ve JavaScript tabanlı güvenlik kontrollerini (OneTrust, vb.) geçmek için.
    *   **Axios & Cheerio:** Yüksek hızlı HTTP istekleri ve HTML parsing işlemleri için (StarDesign modülü).
    *   **Fetch API (Browser Context):** Puppeteer içinde `evaluate` hook'ları kullanılarak tarayıcı bağlamında doğrudan API istekleri göndermek için.
*   **Performans & Ölçeklenebilirlik:**
    *   **Node.js Worker Threads:** Crawler modülünde binlerce veriyi paralel işlemek ve ana thread'i bloklamamak için çoklu iş parçacığı kullanımı.
    *   **Auto-Scaling Concurrency:** Başarı/Hata oranına göre anlık istek sayısını (concurrency) dinamik olarak artıran veya azaltan algoritma.
*   **Ağ & Güvenlik:**
    *   **Cloudflare Tunnels:** Lokal sunucuyu port açmadan güvenli bir şekilde dış dünyaya açmak ve uzaktan yönetmek için entegrasyon.
    *   **Session Management:** Özel çerez ve oturum yönetimi.
    *   **User-Agent Rotation:** Bot tespitini engellemek için dinamik parmak izi yönetimi.

## 🚀 Ana Modüller ve Özellikler

### 1. Yönetim Paneli (Web Dashboard)
Sistemin beyni olarak çalışan, Express.js tabanlı web arayüzü.
*   **Canlı İzleme:** Anlık başarı/hata oranları, işlem hızı (req/min) ve log akışı.
*   **Konfigürasyon:** Hedef kullanıcılar, limitler ve bot ayarlarının JSON tabanlı dinamik yönetimi.
*   **Uzaktan Erişim:** Cloudflare Tunnel entegrasyonu ile mobilden veya farklı ağlardan güvenli erişim.
*   **Rol Tabanlı Yetkilendirme:** Admin ve Standart Kullanıcı girişleri.

### 2. StarDesign Oylama Botu (High-Performance)
*   Tarayıcı açmadan, saf HTTP istekleri (Axios) ile çalışır.
*   **Keep-Alive Agent:** TCP bağlantılarını açık tutarak el sıkışma (handshake) maliyetini düşürür.
*   **Akıllı Hata Yönetimi:** 429 veya 5xx hatalarında otomatik olarak yavaşlar (Backoff Strategy), sistem stabil olduğunda tekrar hızlanır.

### 3. Catwalk Botu (Hybrid Architecture)
*   **Mobil & Masaüstü Simülasyonu:** Hem mobil API (`mcom`) hem de masaüstü web arayüzünü taklit edebilir.
*   **Token Injection:** Puppeteer ile giriş yapıp `SESSION_TOKEN` ve `REQUEST_HASH` değerlerini yakalar, ardından bu tokenları kullanarak tarayıcıyı yormadan arka planda API çağrıları yapar.

### 4. Account Crawler (Multi-Threaded)
*   Belirlenen ID aralıklarını tarayarak "geri alınabilir" (silinmiş e-postalara bağlı) eski hesapları tespit eder.
*   **Worker Threads:** CPU yoğunluklu işlemleri (Regex parsing, XML parsing) ana thread'den izole ederek arayüzün donmasını engeller.
*   **Cross-Check:** Bulunan hesapların e-postalarını Microsoft (Hotmail/Outlook) ve Stardoll API'lerinde çapraz sorgular.

## 📊 Performans İyileştirmeleri

Bu projede karşılaşılan en büyük zorluk, yüksek hacimli işlemlerde Node.js Event Loop'unun bloklanması ve hedef sunucunun hız limitlerine (Rate Limiting) takılmaktı.

*   **Çözüm 1 (Event Loop):** Crawler modülü `worker_threads` kullanılarak tamamen izole edildi. Bu sayede arayüz tepki süresi milisaniyeler seviyesinde kaldı.
*   **Çözüm 2 (Rate Limiting):** Statik bir bekleme süresi yerine, sunucu yanıtlarına göre dinamik değişen bir "Concurrency Control" mekanizması geliştirildi. Başarılı işlemlerde worker sayısı artarken, hata alındığında logaritmik olarak azalır.

## 📸 Ekran Görüntüleri


| Yönetim Paneli | Terminal Logları |
(eklenecek)

## 💡 Kazanımlar

Bu proje sayesinde:
*   Karmaşık asenkron yapıların (Promises, Async/Await) yönetimi,
*   Tersine mühendislik (Reverse Engineering) ile API endpoint'lerinin analizi,
*   Bot koruma sistemlerini (Fingerprinting, Cookies) analiz etme ve aşma,
*   Microservice benzeri modüler mimari tasarımı konularında derinlemesine tecrübe edinilmiştir.

---
*Developed by [@bulutemresakarya]*
