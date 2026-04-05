# 🐳 Adım 4: Docker Mimarisi ve Konteyner Güvenliği

> **Hedef:** `Dockerfile` ve `docker-compose.yml` dosyalarının güvenlik, izolasyon, yetkilendirme ve sistem mimarisi açısından analizi.

Uygulamanın konteynerizasyon mimarisi incelendiğinde hafif, ancak güvenlik açısından geliştirilmeye açık noktalar barındıran bir yapı tespit edilmiştir.

### 🏗️ İmaj ve Katman Analizi
`Dockerfile` incelendiğinde, temel (base) imaj olarak **Alpine Linux** dağıtımının kullanıldığı görülmektedir.
* **Güvenlik Avantajı:** Alpine, sistemde varsayılan olarak `bash`, `curl`, `wget` gibi yaygın araçları barındırmaz. Bu "minimalist" yapı, saldırganların içeri sızması durumunda hareket alanını kısıtlar ve uygulamanın Saldırı Yüzeyini (Attack Surface) ciddi oranda küçültür.

### 🚨 Kritik Zafiyet Bulgusu: Root Kullanıcısı Eksikliği
Uygulamanın `Dockerfile` dosyasında `USER` direktifi (örneğin `USER node`) tanımlanmamıştır. Bu eksiklik, Docker'ın doğası gereği uygulamanın konteyner içerisinde en yetkili kullanıcı olan **"root"** yetkileriyle çalışmasına sebep olur.

**Tehdit Etkisi:**
Eğer uygulamada bir RCE (Uzaktan Kod Çalıştırma) zafiyeti bulunursa, saldırgan konteyner içerisine doğrudan root yetkileriyle düşer. Bu durum, "Container Escape" (Konteynerden ana makineye kaçış) saldırıları için çok büyük bir basamaktır.

**Güvenli Mimari Önerisi (Remediation):**
`Dockerfile` içerisine son katmanda kısıtlı bir kullanıcı eklenmelidir:
```dockerfile
# Örnek Güvenli Kullanım
RUN addgroup -S kuma && adduser -S kuma -G kuma
USER kuma
