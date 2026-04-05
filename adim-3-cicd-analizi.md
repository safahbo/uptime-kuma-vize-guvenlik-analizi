# ⚙️ Adım 3: İş Akışları ve CI/CD Pipeline Analizi

> **Hedef:** Projenin `.github/workflows/` dizininde bulunan otomasyon süreçlerinin, CI/CD adımlarının ve tedarik zinciri (supply chain) zafiyetlerinin incelenmesi.

Uptime Kuma projesinin kaynak kodu incelendiğinde, yazılım geliştirme ve yayınlama (deployment) süreçlerinin **GitHub Actions** altyapısı kullanılarak tamamen otomatize edildiği tespit edilmiştir. Bir siber güvenlik uzmanı gözüyle bu süreçler hem büyük bir kolaylık hem de potansiyel bir saldırı vektörüdür.

### 🔄 Pipeline Mimarisi ve Otomasyon

* **Statik Kod Analizi ve Linting:** Geliştiriciler tarafından açılan her Pull Request (PR) işleminde, kodun standartlara uygunluğunu denetleyen kalite araçları tetiklenmektedir. Bu, zafiyetli bir kod parçasının ana projeye sızmasını engelleyen ilk savunma hattıdır.
* **Sürekli Dağıtım (Continuous Deployment):** Proje yeni bir sürüm (`Release`) aldığında, `docker.yml` gibi iş akışları devreye girer. Kaynak kodu derleyerek Docker Hub ve GitHub Container Registry (GHCR) platformlarına `latest` etiketiyle otomatik olarak yükler.

### ⚠️ Kritik Güvenlik Analizi: Webhook ve PR İstismarı

Açık kaynaklı projelerde dışarıdan gelen PR'lar doğrudan CI/CD sunucularında (Runner) çalıştırılır. Bu durum şu riskleri barındırır:

* **Zafiyet Riski:** Eğer iş akışı dosyalarında `permissions` modülü (izinler) "read-only" olarak kısıtlanmadıysa, saldırgan zararlı bir kod teklifiyle GitHub Actions Runner'ı üzerinde **Remote Code Execution (RCE)** elde edebilir.
* **Tehdit Senaryosu:** Kötü niyetli bir PR, CI ortamındaki `$GITHUB_TOKEN` değerlerini veya sisteme kayıtlı gizli Docker Hub API anahtarlarını (Secrets) çalmak için tasarlanmış bir komut satırı içerebilir.

### 🛡️ Güvenli Mimari Önerisi
Safa Hacıbayramoğlu olarak yaptığım analiz sonucunda, projenin CI/CD süreçlerinin güvenliğini artırmak için şu adımlar önerilmektedir:
1. **İzin Sıkılaştırma:** Tüm workflow dosyalarında `permissions: contents: read` gibi en az yetki prensibi uygulanmalıdır.
2. **Onay Mekanizması:** Dışarıdan gelen (fork edilmiş repolardan) PR'ların, bir yönetici onayı olmadan pipeline üzerinde çalışması engellenmelidir.
