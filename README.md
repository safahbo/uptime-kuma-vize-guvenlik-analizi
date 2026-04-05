# 🛡️ Uptime Kuma - Güvenlik ve Sistem Mimarisi Analizi (Security Audit)

![Bölüm](https://img.shields.io/badge/Bölüm-Bilişim_Güvenliği_Teknolojisi-00529B?style=for-the-badge)
![Üniversite](https://img.shields.io/badge/Üniversite-İstinye_Üniversitesi-D32F2F?style=for-the-badge)
![Ders](https://img.shields.io/badge/Ders-Güvenli_Web_Yazılımı-4CAF50?style=for-the-badge)

**Proje Kapsamı:** Güvenli Web Yazılımı Geliştirme Vize Projesi  
**İncelenen Araç Kategorisi:** Kategori 5 - Veri İşleme, Lab, ve Home-Server

---

## 👨‍💻 Hazırlayan
**Safa Hacıbayramoğlu** *İstinye Üniversitesi, Bilişim Güvenliği Teknolojisi Programı*

---

## 📋 Proje Özeti
Bu repoda, modern bir açık kaynak izleme (monitoring) aracı olan **Uptime Kuma**'nın sistem mimarisi ve güvenlik duruşu, tersine mühendislik (reverse engineering) ve defansif güvenlik bakış açısıyla incelenmiştir. 

Uygulamanın; tedarik zinciri (supply chain) güvenliği, CI/CD iş akışları, Docker izolasyon seviyeleri ve potansiyel kod zafiyetleri (Threat Modeling) analiz edilerek detaylı bir akademik denetim raporu oluşturulmuştur.

---

## 📂 Analiz Raporları (İçindekiler)
Detaylı inceleme adımlarına aşağıdaki bağlantılardan ulaşabilirsiniz:

- 🔍 **[Adım 1: Kurulum ve install.sh Analizi (Reverse Engineering)](adim-1-kurulum-analizi.md)**
- 🧹 **[Adım 2: İzolasyon ve İz Bırakmadan Temizlik (Forensics & Cleanup)](adim-2-izolasyon-temizlik.md)**
- ⚙️ **[Adım 3: İş Akışları ve CI/CD Pipeline Analizi](adim-3-cicd-analizi.md)**
- 🐳 **[Adım 4: Docker Mimarisi ve Konteyner Güvenliği](adim-4-docker-mimarisi.md)**
- 🎯 **[Adım 5: Kaynak Kod, Akış Analizi ve Tehdit Modelleme](adim-5-tehdit-modelleme.md)**

---

## ⚖️ Yasal Uyarı ve Telif Hakları (Disclaimer & Copyright)

**Telif Hakkı Bildirimi:**
Bu depoda incelenen orijinal yazılımın (**Uptime Kuma**) kaynak kodları, mimarisi ve tüm fikri mülkiyet hakları projenin asıl geliştiricisi olan **Louis Lam**'a ve projeye katkıda bulunanlara aittir. Bu GitHub deposu, uygulamanın kendisini (kaynak kodlarını) değil; sistemin mimarisi üzerine yapılmış bağımsız bir güvenlik denetimi raporunu barındırmaktadır.

**Yasal Uyarı:**
Bu çalışma, tamamen **akademik ve eğitim amaçlı** oluşturulmuştur. İçerikte bahsedilen güvenlik analizleri, zafiyet tespitleri ve tehdit modelleme senaryoları; siber güvenlik farkındalığını artırmak ve defansif mimariler kurgulamak amacıyla hazırlanmıştır. Burada yer alan bilgilerin gerçek ve izinsiz sistemler üzerinde kötü amaçlı kullanımı yasa dışıdır. Oluşabilecek olumsuz durumlardan raporu hazırlayan kişi sorumlu tutulamaz.
