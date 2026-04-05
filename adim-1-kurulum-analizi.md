# Adım 1: Kurulum ve install.sh Analizi (Reverse Engineering)

**Görev:** Kurulum dosyalarının incelenmesi, tedarik zinciri (supply chain) analizi ve yetki/bağımlılık kontrollerinin yapılması.

Uptime Kuma projesinin kurulum yönergeleri ve betikleri incelendiğinde, modern ve güvenli kurulum standartlarının benimsendiği görülmektedir. Güvenlik dünyasında kritik bir zafiyet kapısı olan, dışarıdan kaynağı tam doğrulanamayan shell scriptlerinin kök yetkilerle çalıştırılması (`curl -sL https://... | sudo bash`) anti-paterni bu projede kullanılmamaktadır.

### Detaylı Analiz ve Bulgular

* **Paket Bütünlüğü ve Tedarik Zinciri Güvenliği:** Projenin bağımlılıkları Node.js ekosisteminin standart aracı olan `package.json` ile yönetilmektedir. Asıl güvenlik katmanı ise `package-lock.json` dosyasıdır. Bu dosya, projeye dahil edilen yüzlerce alt kütüphanenin tam sürüm numaralarını ve kriptografik hash değerlerini (SHA-512) barındırır. Eğer saldırganlar npm kütüphanesine sızıp zararlı bir kod enjekte ederse (Dependency Confusion veya Typosquatting saldırıları), `npm ci` komutu ile kurulum yapılırken hash değerleri uyuşmayacak ve sistem kurulumu otomatik olarak durdurarak projeyi koruyacaktır.

* **En Az Yetki Prensibi (Principle of Least Privilege):** Kurulum scriptleri ve npm süreçleri incelendiğinde, sistem geneline etki edecek `root` veya `sudo` yetkisine ihtiyaç duyulmamaktadır. Kurulum işlemleri tamamen süreci başlatan kullanıcının kendi alanında (userspace) gerçekleşir. Bu durum, olası bir zararlı bağımlılığın sistemi tamamen ele geçirmesini (Privilege Escalation) zorlaştırır.

* **Dış Bağlantı Analizi:** Kurulum esnasında resmi Docker Hub ve NPM Registry dışında bilinmeyen 3. parti sunuculara herhangi bir dış bağlantı (outbound request) yapılmadığı tespit edilmiştir.
