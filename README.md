# Sonoff-GK-200MP2-B-C
Sonoff GK-200MP2 reset çalışmıyorsa muhtemel sorun Winbond W25Q64 flash chipin kilitli kalması
Sonoff GK-200MP2-B/C Kamera SD Kart ile Kalıcı Çözüm
Sorun
Sonoff GK-200MP2-B/C kameralarda kullanılan Winbond W25Q64 flash chip'i zamanla fiziksel olarak bozulabiliyor. Bu durumda:

Reset butonu çalışıyor (GPIO algılıyor) ama sıfırlama tamamlanamıyor
eWeLink uygulamasından cihaz silinse bile yeniden bağlanamıyor
JFFS2 dosya sisteminde CRC hataları oluşuyor (Node CRC ffffffff != calculated CRC)
Cihaz her açılışta bozuk chip'ten eski/geçersiz kimlik bilgilerini yüklüyor

Çözüm
Bozuk flash chip'i tamamen devre dışı bırakıp SD kartı kalıcı depolama olarak kullanmak. Cihaz her açılışta SD karttan doğru ayarları yüklüyor, bozuk chip'e hiç dokunmuyor.
Nasıl Çalışır

Boot sırasında SD karttaki boot.sh devreye giriyor
Bozuk JFFS2 bölümü umount ediliyor
Yerine RAM tabanlı tmpfs mount ediliyor
SD karttan kaydedilmiş ayarlar RAM'e yükleniyor
Cihaz normal şekilde açılıyor

Kurulum
Gereksinimler

FAT32 formatında SD kart (herhangi bir boyut)
eWeLink uygulaması

Adım 1: boot.sh dosyasını SD karta koy

Adım 2: İlk eşleşme

boot.sh dosyasını SD karta koy
SD kartı kameraya tak
Kamerayı aç — cihaz otomatik olarak eşleşme moduna girecek (AP modu + SmartLink + QR kod hepsi aktif)
eWeLink uygulamasından cihazı ekle
Eşleşme tamamlandıktan sonra en az 3 dakika bekle — arka planda yedekleme işlemi devam ediyor
kamera_yedek/YEDEK_BITTI.txt dosyası oluşunca yedekleme tamamlanmış demektir
Artık kamerayı normal şekilde kullanabilirsin

Adım 3: Sonraki açılışlar
SD kart takılı olduğu sürece cihaz otomatik olarak SD karttan yükleniyor, başka bir şey yapmanı gerekmez.
Önemli Uyarılar
⚠️ SD kart her zaman takılı kalmalı — SD kart olmadan cihaz bozuk chip'ten açılır ve çalışmaz
⚠️ SD kartı kaybetme — SD kartın yedeğini başka bir yerde sakla (boot.sh + kamera_yedek/ klasörü)
⚠️ Uygulama üzerinden SD kart biçimlendirme — eWeLink uygulamasından SD kartı biçimlendirirsen, işlem bittikten sonra 3-4 dakika beklemelisin. Arka planda çalışan daemon biçimlendirmeyi algılayıp boot.sh ve tüm yedekleri otomatik olarak SD karta geri yazacak. Bu süre dolmadan kamerayı kapatma.
Donanım Bilgisi

SoC: GOKE GK7102S
Sensör: GC2053
WiFi: RTL8188FU / RTL8192EU
Flash: Winbond W25Q64JV (8MB, SOIC-8)
Firmware: fw_version=5520.2053.0402build20220712
