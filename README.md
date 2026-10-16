# pocket-play-announcements

Pocket Play için herkese açık JSON duyuru yayını.

Depo: https://github.com/barisgungor23-beep/pocket-play-announcements

Ham JSON: https://raw.githubusercontent.com/barisgungor23-beep/pocket-play-announcements/main/announcements.json

- `announcements.json`: uygulamanın okuyacağı şema v1 yayını. İki örnek de
  **enabled: false** durumundadır; kendiliğinden duyuru göstermez.
- `schema.json`: uygulama deposundaki `docs/announcements/schema.json` ile aynı şema.
  Şema değişirse yayın kopyasını da birlikte güncelleyin.

## Yayınlama

1. Bu depoda yalnız duyuru yayın dosyalarını tutun. Herkese açık dosyalara
   kişisel veri, erişim anahtarı veya gizli bilgi koymayın.
2. `announcements.json` içindeki TR/EN metinleri, benzersiz kimlikleri, UTC tarihleri
   ve uygulama sürümü aralıklarını düzenleyin. Örnek tarihleri yayından önce yenileyin.
3. JSON'u `schema.json` ile doğrulayın. Ayrıca uygulama, tarih/sürüm sırasını,
   kimlik tekrarlarını, HTTPS bağlantıları ve 256 KiB / 100 kayıt sınırlarını denetler.
4. Uygulama, yukarıdaki ham HTTPS JSON adresini kullanır. Adres değişirse önce
   yeni yayını doğrulayın; `AnnouncementConfiguration.verifiedFeedURL` değişikliği
   yeni uygulama derlemesi ister. Aynı adresteki içerik değişiklikleri derleme istemez.
5. Test derlemesinde TR/EN, tarih/sürüm ve gösterim davranışını doğrulayın; ardından
   yalnız yayımlamak istediğiniz kaydı `enabled: true` yapın.

## Davranış

Uygulama her açılış ve ön plana dönüşte kontrol eder. Son başarılı kontrolden bir
saat geçmediyse önbelleği kullanır; aynı anda tek istek çalışır. Başarısız denemeler
aynı uygulama sürecinde bir saat bekler. Periyodik arka plan indirmesi yoktur.
Çevrimdışıyken son geçerli veri, duyurunun kendi bitiş tarihine kadar kullanılabilir.

Kaldırmak için kaydı pasifleştirin veya silin; tümünü kaldırmak için
`{"schemaVersion": 1, "announcements": []}` yayınlayın. Değişiklik sonraki başarılı
kontrolde ulaşır; bu sistem anlık push bildirimi değildir.

Politikalar: `once`, `oncePerVersion`, `daily`, `always`. Pop-up ayrıca süreç başına
bir otomatik gösterimle sınırlıdır. Aynı kimlik metnini değiştirmek gösterim geçmişini
sıfırlamaz. `actionTitle` / `actionURL` isteğe bağlıdır; geçerli TR/EN başlık ve HTTPS
adres birlikte yoksa eylem gösterilmez. Başlangıç dahil, bitiş hariçtir.
