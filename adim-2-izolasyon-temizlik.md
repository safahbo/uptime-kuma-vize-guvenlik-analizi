# 🧹 Adım 2: İzolasyon ve İz Bırakmadan Temizlik (Forensics & Cleanup)

> **Hedef:** Kurulum sonrası dosya sisteminde ve ağ katmanında kalan izlerin tespiti ile eksiksiz bir "Forensics Cleanup" stratejisinin oluşturulması.

Uptime Kuma'nın bir Sanal Makine (VM) üzerinde çalıştırılması sonrası, işletim sistemi üzerinde bırakılan adli bilişim (forensics) kalıntıları tespit edilmiş ve izolasyon prosedürleri belirlenmiştir.

### 🔬 Sistemde Bırakılan İzler (Artifacts)

1. **Ağ İzleri (Network Sockets):** Uygulama başlatıldığında varsayılan olarak `TCP 3001` portunda dinleme (LISTEN) moduna geçer.
2. **Kalıcı Veri ve Loglar:** Uygulama dış bir veritabanı sunucusu yerine gömülü SQLite kullanır. Tüm kritik şifreler, kullanıcı verileri ve sistem logları proje dizinindeki `data/kuma.db` dosyasında tutulur.
3. **Bağımlılık Kalıntıları (Cache):** Node.js ortamında indirilen paketler `node_modules` klasöründe ve `~/.npm` önbellek dizininde yer kaplar.

### 🗑️ İz Bırakmadan Temizlik Stratejisi (Remediation)

Uygulamanın çalıştırıldığı VM üzerinden tamamen izole edilmesi ve silinmesi için aşağıdaki adımlar uygulanmalıdır:

#### Docker Ortamında Temizlik
Eğer sistem Docker üzerinden ayağa kaldırıldıysa:
```bash
# 1. Konteyneri durdur ve sil
docker stop uptime-kuma && docker rm uptime-kuma

# 2. Kalıcı verilerin tutulduğu Volume'u tamamen yok et
docker volume rm uptime-kuma

# 3. İmajı sistemden temizle
docker rmi louislam/uptime-kuma:latest
