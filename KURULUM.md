# Furkan Gökkaya - Matematik Öğretmeni Sitesi

## 1) Yerel Kurulum

```powershell
cd "C:\Users\canpolatgkky\Desktop\furkan-gokkaya-site"
npm install
npm run dev        # Geliştirme (canlı önizleme)
npm run build      # Üretim (dist/ klasörüne minified çıktı)
```

`npm run build` sonrası `dist/` klasöründeki dosyaları repo köküne kopyala:

```powershell
Copy-Item -Path dist\index.html, dist\favicon.svg -Destination . -Force
Copy-Item -Path dist\assets\* -Destination assets\ -Force
```

> Not: `src/`, `vite.config.js`, `package.json`, `dist/` `.gitignore` ile gizli. GitHub'da sadece `index.html` + `assets/` görünür (minified + mangled, okunamaz).

---

## 2) Telegram Bağlantısını Değiştir

`index.html` ve `src/main.js` içinde `t.me/furkangokkaya` yazılı yerleri kendi Telegram kullanıcı adınla değiştir:

```
https://t.me/SENIN_KULLANICI_ADIN
@SENIN_KULLANICI_ADIN
```

---

## 3) Yeni GitHub Hesabı ile Bağlama

### A. GitHub'da yeni hesap aç
1. https://github.com/signup adresinden yeni hesabı oluştur (örn: `furkangokkaya`).
2. E-postayı doğrula.

### B. Yeni repo oluştur (GitHub Pages için)
GitHub Pages'in **ücretsiz ve özel domain'siz** kullanıcı sitesi olması için repo adı **tam olarak** şu formatta olmalı:

```
KULLANICI_ADI.github.io
```

Örnek: hesap adı `furkangokkaya` ise repo adı: `furkangokkaya.github.io`

Repoyu **Public** olarak oluştur (ücretsiz GitHub Pages public repo gerektirir; ama source kodlar `.gitignore` ile gizli olduğundan sadece minified çıktı görünür).

### C. Windows'ta birden fazla GitHub hesabı yönetmek

Sen zaten `canpolatgokkaya` hesabıyla giriş yapmışsın. İkinci bir hesabı (Furkan) eklemenin 3 yolu:

#### Yol 1 (EN KOLAY) — Personal Access Token (PAT)
1. Furkan'ın GitHub hesabıyla giriş yap → **Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token (classic)**
2. Yetki: `repo` seç. Token oluştur ve **kopyala** (sadece 1 kez gösterilir!).
3. Bu klasörde aşağıdaki komutu çalıştır. `git push` istediğinde:
   - Username: `furkangokkaya`
   - Password: **az önce aldığın token'ı yapıştır**

#### Yol 2 — GitHub CLI ile (çok pratik)
```powershell
winget install --id GitHub.cli      # GitHub CLI kurulumu (bir kerelik)
gh auth login                       # Furkan hesabıyla giriş yap
gh auth switch                      # Hesaplar arası geçiş yap
```

#### Yol 3 — SSH key (ileri seviye, hesap karışmasın diye)
`~/.ssh/config` dosyasında ayrı host tanımla (`github-furkan` gibi) ve `git remote` URL'sini `git@github-furkan:furkangokkaya/...` yap.

---

### D. Bu klasörü yeni repo'ya bağla

PowerShell'de bu klasörde:

```powershell
cd "C:\Users\canpolatgkky\Desktop\furkan-gokkaya-site"

# Build alınmış olsun (assets/ doluyken)
npm install
npm run build
Copy-Item -Path dist\index.html -Destination . -Force
if (-not (Test-Path assets)) { mkdir assets }
Copy-Item -Path dist\assets\* -Destination assets\ -Force

# Git başlat
git init
git branch -M main
git add .
git commit -m "İlk yayın: Furkan Gökkaya matematik sitesi"

# Yeni hesabın repo URL'sini bağla
git remote add origin https://github.com/furkangokkaya/furkangokkaya.github.io.git
git push -u origin main
```

> Push sırasında kullanıcı adı/şifre sorulursa **Yol 1**'deki token'ı şifre yerine kullan.

### E. GitHub Pages'i Aktif Et
1. GitHub'da repo sayfası → **Settings → Pages**
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` / `(root)` → Save
4. 1–2 dakika sonra `https://furkangokkaya.github.io` adresinde yayında.

---

## 4) Site Özellikleri

- **Mouse animasyonu**: Antigravity tarzı 3D parçacık alanı, imleç yaklaşınca parçacıklar itiliyor.
- **Tıklama efekti**: 3 kademeli yerçekimi halkası + yörüngeli parçacıklar + 3D shockwave.
- **Telegram popup**: Tuşlara basınca veya 12 saniye sonra otomatik açılır (oturum başına bir kez).
- **Responsive**: Mobilde optimize, dokunmatik cihazda cursor efektleri devre dışı.
- **SEO + Open Graph** meta etiketleri hazır.
- **Kod gizli**: Sadece minified + mangled build push edilir; kaynak okunamaz.

---

## 5) Hızlı İçerik Güncelleme — `bilgilerim.json` (BUILD YOK!)

**En önemli dosya: `bilgilerim.json`** — Kök dizinde, GitHub web arayüzünden doğrudan düzenleyebilirsin. Build almaya gerek **YOK**, sadece kaydet ve push et.

İçinde olanlar:
- `ad_soyad` · `unvan` · `telegram_kullanici_adi`
- `mevcut_calisma_yeri` · `mezuniyet_okulu` · `mezuniyet_bolumu`
- `ozlu_soz` · `ozlu_soz_yazar`
- `istatistikler` (deneyim yılı, ders saati, öğrenci sayısı, memnuniyet yüzdesi)
- `hero_baslik` · `footer_slogan`

GitHub'da düzenleme:
1. `https://github.com/furkangokkaya/furkangokkaya.github.io` reposuna gir.
2. `bilgilerim.json` dosyasına tıkla → kalem (✏️) ikonu ile aç.
3. Değiştir, en altta "Commit changes" → kaydet.
4. 1 dakika içinde site güncellenir. Tarayıcı önbelleğini temizle (Ctrl+F5).

---

## 6) Tasarım / Yapısal Güncelleme (kod değişikliği)

**ÖNEMLİ**: Kaynak HTML artık `src/index.html` (kök `index.html` build çıktısıdır, manuel değiştirme!).

- **Metinler / yeni bölümler**: `src/index.html` düzenle.
- **Telegram username**: `src/index.html` içinde değiştir.
- **Renkler**: `src/style.css` üstündeki `:root` değişkenlerini değiştir.
- **JS davranışları**: `src/main.js`.

Her güncelleme sonrası tekrarla:
```powershell
npm run build
Copy-Item -Path dist\index.html -Destination . -Force
Remove-Item assets\* -Force
Copy-Item -Path dist\assets\* -Destination assets\ -Force
git add -A
git commit -m "Güncelleme"
git push
```
