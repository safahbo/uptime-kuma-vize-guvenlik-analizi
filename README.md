# Vize Projesi: Açık Kaynak Güvenlik ve Sistem Mimarisi Analizi

**İncelenen Proje:** Uptime Kuma (Kategori 5 - Veri İşleme, Lab, ve Home-Server)  
**Kapsam:** Güvenli Web Yazılımı Geliştirme Vize Projesi  

## Hazırlayan
**Safa Hacıbayramoğlu** Bilişim Güvenliği Teknolojisi  
İstinye Üniversitesi  

## Proje Özeti
Bu repoda, modern bir açık kaynak izleme (monitoring) aracı olan Uptime Kuma'nın sistem mimarisi ve güvenlik duruşu tersine mühendislik (reverse engineering) bakış açısıyla incelenmiştir. Uygulamanın tedarik zinciri (supply chain) güvenliği, CI/CD iş akışları, Docker izolasyon seviyeleri ve potansiyel kod zafiyetleri (Threat Modeling) analiz edilerek detaylı bir denetim (audit) raporu oluşturulmuştur.

## Analiz Raporları
Detaylı analiz adımlarına aşağıdaki bağlantılardan ulaşabilirsiniz:
1. [Adım 1: Kurulum ve install.sh Analizi](adim-1-kurulum-analizi.md)
2. [Adım 2: İzolasyon ve İz Bırakmadan Temizlik](adim-2-izolasyon-temizlik.md)
3. [Adım 3: İş Akışları ve CI/CD Pipeline Analizi](adim-3-cicd-analizi.md)
4. [Adım 4: Docker Mimarisi ve Konteyner Güvenliği](adim-4-docker-mimarisi.md)
5. [Adım 5: Kaynak Kod, Akış Analizi ve Tehdit Modelleme](adim-5-tehdit-modelleme.md)

---

## ⚠️ Yasal Uyarı (Disclaimer)
Bu depo, tamamen akademik ve eğitim amaçlı oluşturulmuştur. İçerikte bahsedilen güvenlik analizleri ve tehdit modelleme senaryoları, siber güvenlik farkındalığını artırmak ve defansif mimariler kurgulamak amacıyla hazırlanmıştır. Burada yer alan bilgilerin kötü amaçlı kullanımı yasa dışıdır ve sorumluluk tamamen kullanıcıya aittir.

## 📄 Lisans
Bu proje [MIT Lisansı](LICENSE) altında lisanslanmıştır.
