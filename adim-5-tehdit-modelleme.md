# 🎯 Adım 5: Kaynak Kod, Akış Analizi ve Tehdit Modelleme

> **Hedef:** Uygulamanın çalışma mantığına göre olası siber tehdit senaryolarının çıkarılması ve defansif güvenlik mimarisinin (Mitigation) kurgulanması.

Uptime Kuma'nın temel işlevi, dış sistemlere sürekli olarak HTTP/TCP/Ping istekleri atmaktır. Sistemlerin izlenmesi için kurulan bu esnek yapı, güvenceye alınmadığı takdirde tehlikeli bir siber silaha dönüşebilir.

### 🕵️‍♂️ Tehdit Senaryoları (Threat Models)

#### 1. Kimlik Doğrulama Zafiyeti (CWE-307)
* **Vektör:** Kontrol paneline erişim tek bir giriş ekranı (entrypoint) üzerinden yapılmaktadır.
* **Risk:** Eğer uygulamada yeterli *Rate Limiting* (istek hız sınırlandırması) mekanizması yoksa, saldırganlar otomatize botlarla Brute-Force (Kaba Kuvvet) saldırısı yaparak yönetici parolasını ele geçirebilir.

#### 2. SSRF (Server-Side Request Forgery) - (CWE-918)
Bu uygulamanın doğası gereği maruz kalabileceği en tehlikeli zafiyettir.
* **Vektör:** Bir saldırgan sisteme giriş yaptığında veya bir zafiyet bulduğunda, izlenecek sistem (hedef) olarak `google.com` yerine iç ağ IP adreslerini yazar.
* **Risk Senaryosu:** Uygulamanın barındığı iç ağdaki dışarıya kapalı veritabanına `http://127.0.0.1:3306` veya bulut sunucu (AWS/Azure) meta verilerine `http://169.254.169.254` adresleri üzerinden istek atılır.
* **Sonuç:** Saldırgan, Uptime Kuma sunucusunu bir "Proxy" (Aracı) olarak kullanır ve dışarıdan erişilemeyen gizli verileri doğrudan kendi paneline yansıtır.

#### 🛡️ Mimari Çözüm ve Savunma (Remediation)
SSRF saldırılarını engellemek için, kullanıcının girdiği URL'lerin kaynak kod tarafında çözümlenmesi (DNS Resolution) ve bir **Blacklist (Kara Liste)** uygulanması zorunludur.

```javascript
// Güvenli Mimari Konsepti (Pseudo-Code)
function checkUrl(userUrl) {
    const ipAddress = resolveDNS(userUrl);
    const blacklist = ['127.0.0.0/8', '192.168.0.0/16', '10.0.0.0/8', '169.254.169.254'];
    
    if (isInBlacklist(ipAddress, blacklist)) {
        throw new Error("İç ağa veya güvenilmeyen IP bloklarına istek atılamaz!");
    }
    return sendRequest(ipAddress);
}
