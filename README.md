# هيكل مشروع "فتوى" الجاهز للرفع على GitHub

## اللي جاهز عندك دلوقتي (في الملفات اللي بعتهالك)
```
fatwa-app/
├── www/
│   └── index.html              ← الكود بتاع التطبيق (فيه __GEMINI_API_KEY__ placeholder)
├── resources/
│   └── icon.png                ← لوجو "يستفتونك" (1254×1254)
├── .github/
│   └── workflows/
│       └── build-apk.yml       ← بيبني الـ APK ويولّد الأيقونات أوتوماتيك
├── package.json
├── capacitor.config.json
└── .gitignore
```

## اللي لسه محتاجة تضيفيه بنفسك (من عندك، مش عندي)
فولدرات `Library` و`quran` اللي شفتهم في صورة الملفات عندك — دول
لازم يترفعوا **جوه `www/` بنفس الاسم بالظبط** (حساسية حالة الأحرف
مهمة)، لأن الكود بيعتمد عليهم كمسارات نسبية:

```
www/
├── index.html
├── library/          ← لازم يبقى اسمه بحروف صغيرة "library" مش "Library"
│   ├── library_index.json
│   ├── library_data_part1.js
│   ├── ... باقي أجزاء المكتبة
│   └── books/...
└── quran/
    ├── quran_data.js
    ├── quran_pdf_index.js
    ├── quran_page_map.js
    └── pages/...
```

**ملحوظة مهمة:** في صورتك، فولدر المكتبة اسمه `Library` (بحرف
كبير)، لكن الكود بيدوّر على `library/` (بحروف صغيرة). أندرويد
(زي Linux) بيفرّق بين الحروف الكبيرة والصغيرة في أسماء الملفات،
فلازم تسمّي الفولدر `library` بالظبط وإلا التطبيق مش هيلاقي بيانات
المكتبة.

فولدر `Assets` (اللي فيه ملف الصوت القديم بتاع Ambient Hum) —
**متحتاجيش ترفعيه خالص**، لأن الميزة دي اتشالت من الكود بالكامل.

## خطوات الرفع من Termux

```bash
pkg install nodejs git -y
git clone https://github.com/alisaeed9x/fatwa-app.git
cd fatwa-app
```

بعد كده انسخي كل الملفات اللي بعتهالك (`www/index.html`,
`resources/icon.png`, `.github/workflows/build-apk.yml`,
`package.json`, `capacitor.config.json`, `.gitignore`) في نفس
المكان بالظبط جوه فولدر `fatwa-app`، وانسخي فولدري `library`
و`quran` بتوعك جوه `www/`.

```bash
npm install
npm install @capacitor/android
npx cap add android
npx cap sync
```

**إضافة مفتاح Gemini كـ Secret (مرة واحدة بس):**
من صفحة الريبو: Settings → Secrets and variables → Actions →
New repository secret
- الاسم: `GEMINI_API_KEY`
- القيمة: مفتاح Gemini جديد (مش القديم اللي بقى محروق)

```bash
git add .
git commit -m "إعداد مشروع Capacitor كامل مع أيقونة التطبيق"
git push
```

بعد الـ push، الـ workflow هيشتغل أوتوماتيك، يولّد كل أحجام
الأيقونة من `resources/icon.png`، ويبني الـ APK، وتقدري تحمّليه
من تبويب Actions تحت Artifacts.
