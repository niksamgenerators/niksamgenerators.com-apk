# اپلیکیشن اندروید «نیک سام ژنراتور» (WebView)

این پروژه یک اپ اندرویدی کامل (Kotlin) است که سایت `https://niksamgenerators.com/` رو داخل خودش لود می‌کنه، دقیقاً همون نسخه‌ای که در مرورگر موبایل می‌بینی. این پروژه از روی اپ «ایران کمپ» ساخته شده و همه‌ی امکاناتش عیناً حفظ شده.

## امکاناتی که از قبل توش پیاده‌سازی شده

- لود کامل سایت با جاوااسکریپت، DOM Storage و کوکی فعال (لازمه برای لاگین و پنل کاربری)
- پشتیبانی از کوکی‌های Third-party برای درگاه‌های پرداخت ایرانی (زرین‌پال، آیدی‌پی، زیبال، نکست‌پی، شاپرک) — لیست دامنه‌های مجاز رو در `MainActivity.kt` می‌تونی ویرایش کنی
- دکمه Back اندروید که داخل تاریخچه سایت برمی‌گرده
- Pull-to-refresh (کشیدن به پایین برای رفرش)
- نوار پیشرفت لود صفحه + Splash Screen استاندارد
- صفحه‌ی «اتصال اینترنت نیست» با دکمه تلاش مجدد
- آپلود فایل از فرم‌های سایت (مثلاً آپلود مدرک یا عکس)
- دکمه‌های اشتراک‌گذاری و علاقه‌مندی‌ها، و نوار جستجوی داخلی در صفحه‌ی اصلی سایت
- باز شدن لینک‌های خارج از سایت (مثل شبکه‌های اجتماعی) در مرورگر جدا، نه داخل اپ
- باز کردن صفحاتی که با `target="_blank"` یا `window.open` باز می‌شن (معمولاً صفحه پرداخت)
- Deep Link برای بازگشت از درگاه پرداخت (`niksamgenerators://`) و App Link برای باز شدن مستقیم لینک‌های `niksamgenerators.com`
- امنیت SSL: خطای گواهی نادیده گرفته نمی‌شه (برای پرداخت امن)

## قبل از ساخت اپ (خیلی مهم — این‌ها رو حتماً بررسی/تنظیم کن)

1. **آیکون:** آیکون فعلی همون آیکون جایگزین قبلی است (`app/src/main/res/mipmap-*` و `drawable/ic_launcher.xml`). حتماً قبل از انتشار در Android Studio از منوی `New > Image Asset` آیکون واقعی برند «نیک سام ژنراتور» رو جایگزین کن.
2. **رنگ برند:** رنگ‌های `brand_primary` و `brand_primary_dark` در `app/src/main/res/values/colors.xml` فقط یه پیش‌فرض موقتی هستن (چون به رنگ رسمی برند سایت دسترسی نداشتم). این‌ها رو با رنگ اصلی سایت `niksamgenerators.com` جایگزین کن.
3. **درگاه پرداخت:** فعلاً همون درگاه‌های ایرانی رایج (زرین‌پال، آیدی‌پی، زیبال، نکست‌پی، شاپرک) به‌عنوان مجاز اضافه شده‌ن. اگه سایت درگاه دیگه‌ای داره یا اصلاً پرداخت آنلاین نداره، این دو فایل رو اصلاح کن:
   - `app/src/main/res/xml/network_security_config.xml`
   - `app/src/main/java/com/niksamgenerators/app/MainActivity.kt` (لیست `allowedDomains`)
4. **لینک «علاقه‌مندی‌ها»:** در `MainActivity.kt` مقدار `FAVORITE_URL` رو موقتاً `https://niksamgenerators.com/wishlist/` گذاشتم (مسیر رایج ووکامرس/وردپرس). اگه سایت مسیر دیگه‌ای برای این صفحه داره (یا اصلاً همچین صفحه‌ای نداره)، این خط رو اصلاح کن یا دکمه‌ی مربوطه رو در `activity_main.xml` حذف کن.
5. **گوگل لاگین/ورود با شبکه‌های اجتماعی:** اگه سایت از لاگین گوگل یا مشابه استفاده می‌کنه، معمولاً همون WebView کافیه، ولی گوگل گاهی ورود از WebView غیر از Chrome Custom Tabs رو محدود می‌کنه. اگه با این مشکل مواجه شدی، بگو تا نسخه‌ی Chrome Custom Tabs رو هم اضافه کنم.

## روش پیشنهادی: ساخت APK در فضای ابری گیت‌هاب (بدون نیاز به VPN یا Android Studio)

توی این پروژه دو ورک‌فلوی گیت‌هاب اکشن گذاشته شده که خودشون روی سرورهای گیت‌هاب (بدون فیلترینگ) پروژه رو build می‌کنن:

- `.github/workflows/generate-keystore.yml` → یک‌بار اجرا می‌شه و فایل امضای (Keystore) اپ رو می‌سازه
- `.github/workflows/main.yml` → با هر Push به شاخه‌ی `main` (یا اجرای دستی)، نسخه‌ی نهایی و امضاشده‌ی APK رو می‌سازه

### مرحله‌ی ۱ — ساخت Repository و آپلود پروژه

1. یه حساب رایگان در [github.com](https://github.com) بساز (اگه نداری).
2. یه Repository جدید بساز (دکمه‌ی سبز **New**)، مثلاً به اسم `NiksamGeneratorsApp`، **Public** یا **Private** فرقی نمی‌کنه، و **Create repository** رو بزن.
3. توی صفحه‌ی خالی Repository، روی لینک **"uploading an existing file"** کلیک کن.
4. کل محتویات این پوشه (همونی که الان داری) رو Drag & Drop کن و بعد از اتمام آپلود، دکمه‌ی **Commit changes** رو بزن.
   - پوشه‌های `.gradle`، `.idea` و `build` (اگه بعداً با Android Studio ساختی) لازم نیست آپلود بشن.

### مرحله‌ی ۲ — ساخت Keystore (فقط یک‌بار)

1. برو به تب **Actions**، روی ورک‌فلوی **"Generate Keystore (run once)"** کلیک کن و دکمه‌ی **Run workflow** رو بزن.
2. چند ثانیه صبر کن، وقتی تیک سبز خورد روی اجرا کلیک کن و از قسمت **Artifacts**، فایل `niksamgenerators-release-keystore` رو دانلود کن. داخلش فایل `niksamgenerators-release.jks` هست — این فایل رو جایی امن نگه دار (برای آپدیت‌های بعدی اپ لازمه و قابل بازیابی نیست).
3. این فایل jks رو به Base64 تبدیل کن تا بتونی به‌صورت Secret به گیت‌هاب بدی:
   - در ویندوز (PowerShell): `[Convert]::ToBase64String([IO.File]::ReadAllBytes("niksamgenerators-release.jks")) | Set-Content keystore_base64.txt`
   - در مک/لینوکس: `base64 -i niksamgenerators-release.jks -o keystore_base64.txt`

### مرحله‌ی ۳ — تنظیم Secrets در گیت‌هاب

برو به `Settings > Secrets and variables > Actions` توی Repository و این ۴ Secret رو بساز:

| نام Secret | مقدار |
|---|---|
| `KEYSTORE_BASE64` | محتوای فایل `keystore_base64.txt` |
| `KEYSTORE_PASSWORD` | `NiksamGen@2026Secure` (همونی که در `generate-keystore.yml` تنظیم شده — پیشنهاد می‌شه بعداً تغییرش بدی و در همون فایل هم آپدیت کنی) |
| `KEY_ALIAS` | `niksamgenerators` |
| `KEY_PASSWORD` | `NiksamGen@2026Secure` |

### مرحله‌ی ۴ — ساخت APK نهایی

1. برو به تب **Actions**، ورک‌فلوی **"Build APK"** خودش با هر Push به `main` اجرا می‌شه؛ یا از پهلو روی اون بزن و **Run workflow** رو انتخاب کن.
2. صبر کن (حدود ۳-۵ دقیقه) تا تیک سبز ✅ بخوره، بعد روی اجرا کلیک کن.
3. از قسمت **Artifacts** پایین صفحه، `NiksamGeneratorsApp-release-apk` رو دانلود کن. داخل zip، فایل `app-release.apk` هست — همون فایل نصبی نهایی و امضاشده.
4. این فایل رو به گوشی منتقل کن و نصبش کن (با فعال کردن «نصب از منابع ناشناس»).

## روش جایگزین: ساخت با Android Studio روی سیستم خودت

1. [Android Studio](https://developer.android.com/studio) رو نصب کن.
2. این پوشه رو با گزینه‌ی **Open** باز کن.
3. صبر کن Gradle Sync تموم بشه.
4. برای تست سریع: دکمه‌ی ▶ (Run) رو بزن.
5. برای نسخه‌ی نهایی: `Build > Generate Signed Bundle / APK` → APK → همون Keystore که در مرحله‌ی ۲ ساختی رو انتخاب کن (یا یکی جدید بساز) → Release → Finish.

## انتشار در گوگل‌پلی (اختیاری)

گوگل گاهی اپ‌های ساده‌ی WebView رو رد می‌کنه چون ارزش افزوده‌ی کمی نسبت به مرورگر دارن. برای افزایش شانس تأیید:

- حتماً آیکون و اسکرین‌شات واقعی بذار
- Privacy Policy برای سایتت آماده کن (چون لاگین/پرداخت داره، گوگل حتماً می‌خواد)
- در توضیحات اپ مشخص کن چه خدماتی ارائه می‌ده (نه فقط «نمایش سایت»)

## تغییر آدرس سایت در آینده

اگه دامنه سایت عوض شد، این خط رو در `app/src/main/java/com/niksamgenerators/app/MainActivity.kt` ویرایش کن:

```kotlin
private val BASE_URL = "https://niksamgenerators.com/"
```

و دامنه‌ی جدید رو به `allowedDomains` (همون فایل) و `app/src/main/res/xml/network_security_config.xml` هم اضافه کن.
