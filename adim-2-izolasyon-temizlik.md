# Adım 2: İzolasyon ve İz Bırakmadan Temizlik (Forensics & Cleanup)

**Görev:** Kurulum sonrası sistemde, dosya sisteminde ve ağ katmanında kalan izlerin tespiti ile eksiksiz bir temizlik stratejisinin oluşturulması.

Bir uygulamanın sistemden tam olarak izole edilebilmesi için kurulum sonrası bıraktığı tüm "artifact"lerin (kalıntıların) tespit edilmesi gerekir. Uptime Kuma'nın bir Sanal Makine (VM) üzerinde çalıştırılması sonrası elde edilen adli bilişim (forensics) bulguları aşağıdadır.

### Tespitler

* **Ağ İzleri (Network Sockets):** Uygulama ayağa kalktığında varsayılan olarak TCP `3001` portunu dinlemeye (LISTEN) başlar. Ayrıca dış sistemleri kontrol etmek için ICMP (Ping) paketleri üretir.
* **Veri Kalıntıları:** Uptime Kuma, harici bir veritabanı (MySQL vb.) kurmak yerine gömülü SQLite mimarisi kullanır. Tüm kritik yapılandırmalar, şifrelenmiş parolalar ve loglar projenin ana dizinindeki `data/kuma.db` dosyasında saklanır.
* **Paket Önbellekleri:** Node.js tabanlı olduğu için sistemde büyük bir `node_modules` klasörü ve işletim sisteminin gizli dizinlerinde npm cache dosyaları oluşturulur.

### İz Bırakmadan Temizlik Stratejisi (Cleanup)

Eğer Uptime Kuma manuel (Node.js) olarak kurulduysa:
1. Uygulama işlemi sonlandırılır. Terminal üzerinden `ss -tulnp | grep 3001` veya `lsof -i :3001` komutları ile dinleyen hiçbir "zombi process" kalmadığı teyit edilir.
2. Tüm proje klasörü `rm -rf uptime-kuma` komutu ile silinir. (Bu işlem `kuma.db` veritabanını da yok edeceği için tüm veriler silinir).
3. `npm cache clean --force` komutu ile sistemde kalan paket kalıntıları temizlenir.

Eğer Docker ile kurulduysa:
1. `docker stop uptime-kuma` ve `docker rm uptime-kuma` ile konteyner kaldırılır.
2. `docker volume rm uptime-kuma` komutu ile uygulamanın veritabanını tuttuğu kalıcı veri alanı (volume) tamamen silinerek tam bir temizlik sağlanır.
