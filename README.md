# Mahalle Platformu — İlk çalışan sürüm (MVP)

Tek bir responsive PWA: iPhone, Android, Windows ve macOS tarayıcılarına uyarlanmış Türkçe arayüz.
Masaüstünde lacivert yan menü, mobilde sabit alt menü.

## Mevcut işlevler

- Ana sayfa, mahalle değiştirici ve takip edilen mahalleler
- Türkiye il → ilçe → mahalle/köy seçimi (aşağıdaki veri kaynağına internetle erişirse)
- Düğün, nişan/söz, mevlit, sünnet, asker düğünü, diğer etkinlikler
- Aynı mahalle / tarihte etkinlik çakışma uyarısı ve aylık takvim
- Cenaze duyuruları: izin beyanı, kullanıcı bildirimi etiketi (doğrulanmış olarak sunulmaz)
- Duyurular, satış ilanları, fotoğraf yükleme, sorun bildirme, yardımlaşma
- Paylaşım detayları, düzenleme / silme (kendi içeriği), paylaşma bağlantısı ve şikâyet
- Supabase bağlanınca e-posta/şifreli üyelik ve ortak içerikler; takipçilere uygulama içi bildirim
- Statik, kurulabilir PWA iskeleti (HTTPS üzerinden servis edilmesi gerekir)

## 1 — Yerel demo açma

Bir terminalde bu klasörün içindeyken:

```bash
python -m http.server 8000
```

Tarayıcıdan `http://localhost:8000` adresine girin. `index.html`'e çift tıklamak yerine HTTP sunucusu kullanın.
Supabase bilgisi olmadan **DEMO MODU** açılır. “Demo olarak devam et” ile örnek kayıtlar eklenebilir;
veriler sadece aynı tarayıcının `localStorage` alanında tutulur. Gerçek hesap açılmaz ve farklı cihazlar birbirini görmez.

## 2 — Gerçek ortak veritabanını açma (Supabase)

1. https://supabase.com adresinde yeni bir proje oluşturun.
2. **SQL Editor** > **New query** ekranında `supabase/01_schema.sql` dosyasının tamamını çalıştırın.
3. **Authentication > Providers** bölümünde E-mail girişini etkinleştirin; e-posta onayı ve gönderici ayarlarını yapılandırın.
4. **Project Settings > API** bölümündeki **Project URL** ile **anon / publishable** anahtarını `config.js` içine girin.
5. Supabase **Authentication > URL Configuration** altında yayın adresini Site URL ve Redirect URL listesine ekleyin.
6. Dosyaları GitHub Pages gibi HTTPS destekli bir statik hostinge yayınlayın.
7. İki farklı hesapla giriş yaparak etkinlik, ilan, fotoğraf ve bildirim işleyişini test edin.

**ASLA** Supabase `service_role` anahtarını, veritabanı parolasını veya VAPID özel anahtarını `config.js` içine yazmayın.

## 3 — GitHub Pages

Yeni bir GitHub deposu açın. Bu klasördeki tüm dosya ve klasörleri deponun köküne ekleyin.
**Settings > Pages > Build and deployment > Deploy from a branch**, `main` ve `/ (root)` seçin.
İlk dağıtım sonrası linki iPhone Safari'de ana ekrana ekleyebilir, Android Chrome'da yükleyebilirsiniz.

## 4 — Yerleşim verisi

Tam Türkiye il/ilçe/mahalle-köy listesi, tarayıcıdan şu açık kaynak JSON dosyası üzerinden yüklenir:
https://github.com/ferhat-mousavi/turkiye-il-ilce-mahalle-koy
MIT lisanslıdır. Kaynak: 2025 © Ferhat Mousavi; lisans kopyası `THIRD_PARTY_NOTICES.txt` dosyasındadır.
Resmî ve güncel ADNKS/MAKS kaydı olduğuna dair garanti verilmez; listeyi kamu kullanımında doğrulayın.
İnternet/veri kaynağı yoksa uygulama sınırlı örnek ilçe-mahalle listesine düşer.
**Bu MVP tüm Türkiye yerleşimlerini paketin içinde çevrimdışı barındırmıyor.**

## Önemli: Canlı halka açmadan önce

- Şu an KVKK ve üyelik sözleşmesi **yalnızca taslak/yer tutucudur**. Gerçek veri sorumlusu, irtibat, amaç, hukuki sebep, saklama süresi, haklar ve rıza süreçleri için profesyonel inceleme gerekir. Yer tutucuyu kullanarak halka açmayın.
- Muhtar doğrulaması ve muhtar yönetimi **yok** (isteğe uygun).
- Uygulama içi bildirimler vardır; **gerçek kapalı uygulama push bildirimi yoktur**. Bunun için sunucu, VAPID, izin, abonelik ve oran sınırlama gerekir.
- Cenaze paylaşımları bağımsız olarak doğrulanmaz; yanlış ölüm duyurusu zarar verebilir. Bu yüzden **cenaze için otomatik toplu bildirim devre dışıdır**. Canlıda doğrulama/itiraz sistemi ve moderasyon gerekli.
- Şikâyet raporu veritabanına eklenir; henüz merkezi moderasyon arayüzü ve otomatik kaldırma sistemi yoktur.
- Üretim öncesinde oran sınırları, anti-spam/CAPTCHA, mahremiyet, silme talepleri, fotoğraf moderasyonu, depolama ve yedekleme planı, otomatik testler ve erişilebilirlik testi gerekir.
- Görsellerin bulunduğu Storage kovası **public** yapıdadır; şahsi/özel fotoğraflar yüklemeyin.
- Sıralama ilk 400 kayıtla sınırlıdır; Türkiye ölçeğinde mahalle bazlı API sorgusu, sayfalama ve indeksler ekleyin.
- Görselde gösterilen örnek içerikler **gerçek etkinlik, ölüm veya ilan değildir**.

## Teknik yapı

Vanilla JS + CSS + HTML. Derleme gerekmez. Supabase JS CDN'den yüklenir; offline demo için gerekli değildir.
Dosyalar: `index.html`, `styles.css`, `app.js`, `config.js`, `manifest.webmanifest`, `sw.js`, `assets/`, `supabase/`.

**Sürüm:** 0.1 MVP / 7 Kasım 2026
