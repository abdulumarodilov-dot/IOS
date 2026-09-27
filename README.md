# Getcontact — Flutter versiyasi

Kivy ilovasining Flutter'ga ko'chirilgan versiyasi. Bitta koddan **Android** va **iOS** uchun ilova chiqadi.

---

## Nima o'zgardi

| Kivy (eski) | Flutter (yangi) |
|---|---|
| `buildozer` + `python-for-android` | `flutter build` |
| `sqlite3` | `sqflite` |
| `threading.Thread` | `async/await` (UI qotmaydi) |
| Parol `.txt` faylda | `SharedPreferences` |
| Canvas'da qo'lda chizilgan ikonkalar | Material ikonkalar |
| Faqat Android | Android **+ iOS** |

Barcha dizayn ranglari va o'lchamlari aynan ko'chirildi — ilova xuddi shunday ko'rinadi.

---

## Ishga tushirish

### 1. Flutter o'rnatish
https://docs.flutter.dev/get-started/install

### 2. Loyihani tayyorlash

```bash
cd getcontact

# Platforma papkalarini yaratish (android/ va ios/)
flutter create --platforms=android,ios .

# Sozlamalarni qo'llash
bash tool/patch_android.sh
bash tool/patch_ios.sh      # faqat macOS'da kerak

# Paketlar
flutter pub get

# Ikonka va yuklash ekrani
dart run flutter_launcher_icons
dart run flutter_native_splash:create
```

### 3. Yig'ish

```bash
flutter build apk --release        # Android APK
flutter build ios --release        # iOS (faqat Mac'da)
```

---

## GitHub Actions

Ikkita workflow tayyor:

| Fayl | Nima qiladi | Qachon ishga tushadi |
|---|---|---|
| `.github/workflows/android.yml` | APK yig'adi | Har `push` da |
| `.github/workflows/ios.yml` | iOS `.app` yig'adi | Faqat qo'lda (**Actions → iOS → Run workflow**) |

iOS build ~20-30 daqiqa davom etadi va **imzosiz** `.app` chiqaradi — u faqat Mac simulyatorida ishlaydi.

---

## Bazani joylash

### Android
`royxat.db` faylni quyidagi papkalardan biriga qo'ying:

- `/sdcard/qidiruv/royxat.db` ← tavsiya etiladi
- `/sdcard/royxat.db`
- `/sdcard/Download/royxat.db`

Ilova birinchi ochilganda **"Barcha fayllar"** ruxsatini so'raydi — ruxsat bering.

Ruxsat bermoqchi bo'lmasangiz: **Sozlamalar → Bazani import qilish** orqali ham yuklash mumkin.

### iOS
iOS'da ilova tashqi papkalarni ko'ra olmaydi. Ikki yo'l bor:

1. **Ilova ichidan:** Sozlamalar → **Bazani import qilish** → `royxat.db` ni tanlang
2. **Files ilovasi orqali:** `royxat.db` ni `Files → On My iPhone → Getcontact` papkasiga ko'chiring

---

## iOS'ni haqiqiy iPhone'ga o'rnatish

Buning uchun **Apple Developer** akkaunti kerak (**$99/yil**):

1. https://developer.apple.com da ro'yxatdan o'ting
2. Xcode'da loyihani oching: `open ios/Runner.xcworkspace`
3. **Signing & Capabilities** → jamoangizni tanlang
4. `flutter build ipa` → `.ipa` fayl tayyor
5. **TestFlight** yoki **App Store Connect** orqali tarqating

> Akkauntsiz ham Xcode orqali o'z iPhone'ingizga o'rnatish mumkin, lekin ilova **7 kundan keyin ishlamay qoladi**.

---

## Loyiha tuzilishi

```
lib/
├── main.dart                    # kirish nuqtasi
├── theme.dart                   # ranglar + gradient fon
├── models/
│   └── field_meta.dart          # ustun nomlari va ikonkalari
├── services/
│   ├── db_service.dart          # SQLite: qidirish, import, yo'l topish
│   └── auth_service.dart        # SHA-256 parol
├── screens/
│   ├── lock_screen.dart         # parol ekrani
│   ├── search_screen.dart       # asosiy qidiruv
│   └── settings_screen.dart     # sozlamalar + baza import
└── widgets/
    ├── common.dart              # tugmalar, inputlar, panellar
    └── result_card.dart         # natija kartochkasi + batafsil oyna
```

---

## Sozlash

**Ranglar:** `lib/theme.dart` → `AppColors`

**Qidiruv ustunlari:** `lib/models/field_meta.dart` → `searchColumns`

**Jadval nomi / limit:** `lib/services/db_service.dart` → `kTable`, `kMaxResults`
