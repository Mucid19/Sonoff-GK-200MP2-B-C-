# Sonoff-GK-200MP2-B-C

[![GitHub Repo](https://img.shields.io/badge/GitHub-Mucid19%2FSonoff--GK--200MP2--B--C--181717?logo=github)](https://github.com/Mucid19/Sonoff-GK-200MP2-B-C-)
[![Bitcoin](https://img.shields.io/badge/Donate-Bitcoin-orange?logo=bitcoin)](https://bitcoin.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Sonoff GK-200MP2 reset çalışmıyorsa muhtemel sorun Winbond W25Q64 flash chip'in kilitli kalması / bozulmasıdır.**  
> Sonoff GK-200MP2-B/C Kamera SD Kart ile Kalıcı Çözüm

---

## 📌 Sorun

Sonoff GK-200MP2-B/C kameralarda kullanılan Winbond W25Q64 flash chip'i zamanla fiziksel olarak bozulabiliyor. Bu durumda:

* **Reset butonu çalışıyor** (GPIO algılıyor) ama sıfırlama tamamlanamıyor.
* **eWeLink uygulamasından** cihaz silinse bile yeniden bağlanamıyor.
* **JFFS2 dosya sisteminde CRC hataları** oluşuyor (`Node CRC ffffffff != calculated CRC`).
* Cihaz her açılışta bozuk chip'ten eski/geçersiz kimlik bilgilerini yüklüyor.

---

## 💡 Çözüm

Bozuk flash chip'i tamamen devre dışı bırakıp SD kartı kalıcı depolama olarak kullanmak. Cihaz her açılışta SD karttan doğru ayarları yüklüyor, bozuk chip'e hiç dokunmuyor.

### ⚙️ Nasıl Çalışır?

1. Boot sırasında SD karttaki `boot.sh` devreye giriyor.
2. Bozuk JFFS2 bölümü `umount` ediliyor.
3. Yerine RAM tabanlı `tmpfs` mount ediliyor.
4. SD karttan kaydedilmiş ayarlar RAM'e yükleniyor.
5. Cihaz normal şekilde açılıyor.

---

## 🛠️ Kurulum

### Gereksinimler
* FAT32 formatında SD kart (herhangi bir boyut)
* eWeLink uygulaması

---

### Adım 1: Dosyayı SD Karta Koyma
* `boot.sh` dosyasını SD kartın kök dizinine kopyalayın.

---

### Adım 2: İlk Eşleşme
1. `boot.sh` dosyasını SD karta koyun.
2. SD kartı kameraya takın.
3. Kamerayı açın — cihaz otomatik olarak eşleşme moduna girecek (AP modu + SmartLink + QR kod hepsi aktif).
4. eWeLink uygulamasından cihazı ekleyin.
5. Eşleşme tamamlandıktan sonra en az **3 dakika bekleyin** — arka planda yedekleme işlemi devam ediyor.
6. `kamera_yedek/YEDEK_BITTI.txt` dosyası oluşunca yedekleme tamamlanmış demektir.
7. Artık kamerayı normal şekilde kullanabilirsiniz.

---

### Adım 3: Sonraki Açılışlar
* SD kart takılı olduğu sürece cihaz otomatik olarak SD karttan yüklenir, başka bir şey yapmanız gerekmez.

---

## ⚠️ Önemli Uyarılar

* ⚠️ **SD kart her zaman takılı kalmalı:** SD kart olmadan cihaz bozuk chip'ten açılır ve çalışmaz.
* ⚠️ **SD kartı kaybetmeyin:** SD kartın yedeğini başka bir yerde saklayın (`boot.sh` + `kamera_yedek/` klasörü).
* ⚠️ **Uygulama üzerinden SD kart biçimlendirme:** eWeLink uygulamasından SD kartı biçimlendirirseniz, işlem bittikten sonra **3-4 dakika beklemelisiniz**. Arka planda çalışan daemon biçimlendirmeyi algılayıp `boot.sh` ve tüm yedekleri otomatik olarak SD karta geri yazacaktır. Bu süre dolmadan kamerayı kapatmayın.

---

## 🔬 Donanım Bilgisi

| Bileşen | Model / Bilgi |
| :--- | :--- |
| **SoC** | GOKE GK7102S |
| **Sensör** | GC2053 |
| **WiFi** | RTL8188FU / RTL8192EU |
| **Flash** | Winbond W25Q64JV (8MB, SOIC-8) |
| **Firmware** | `fw_version=5520.2053.0402build20220712` |

---

## ☕ Destek ve Bağış (Donations)

Bu donanımsal onarım çözümü işinize yaradıysa ve çalışmalarımı desteklemek isterseniz Bitcoin (BTC) ile bağış yapabilirsiniz:

### 🪙 Bitcoin (BTC) Bağış Adresi:
```text
bc1qxf5cfrxasshlkt79x0q805l9t3feer868en68nhlxmwetlr6sv4qdfda5s
```

*Katkılarınız ve desteğiniz için çok teşekkür ederim!*
