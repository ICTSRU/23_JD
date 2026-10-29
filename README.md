# نموذج الوصف الوظيفي — Job Description (JD) · v1.3

صفحة HTML (RTL، عربي/إنجليزي) لإعداد الوصف الوظيفي بنفس بنية النموذج المعتمد (HCD)، مع **حفظ في Google Sheets**، **فتح السجلات وتعديلها**، و**تصدير PDF مطابق للنموذج**. الدخول مقيّد بحسابات الجامعة `@sr.edu.sa`.

## الملفات
| الملف | الوصف |
|---|---|
| `apps-script/Code.gs` | الخلفية: الحفظ/التحميل، التحقق من الحساب، سجل الدخول `AccessLog`، الترجمة، ودالة `setup()` |
| `apps-script/Index.html` | صفحة النموذج نفسها (تُعرض من داخل Apps Script) |
| `apps-script/Denied.html` | صفحة «غير مصرّح» |
| `apps-script/Logos.html` | شعارات SRU/ICTD/ITOC مضمّنة (Kit v1.4) |
| `apps-script/JdLogo.html` | شعار النموذج الأصلي مضمّن لاستخدامه في ملف PDF |
| `index.html` + `assets/` | صفحة الدخول (ITOC) على GitHub Pages — تفتح النموذج |
| `records/index.html` | صفحة دخول تفتح مباشرة «جميع السجلات» (`?page=records`) |
| `CHANGELOG.md` | سجل الإصدارات |

## ملف Google Sheet
**سجل الوصف الوظيفي - ICTD** — https://docs.google.com/spreadsheets/d/1pwwOhWTyI6uQQyEZk_L6qLSvP4x_wWxcumD4O9Cn0vY/edit
التبويبات: `JD_Records` (سجل لكل وصف) · `JD_History` (نسخة عن كل حفظ) · `AllowedUsers` · `AccessLog`.
معرّف الشيت مثبّت مسبقًا في `Code.gs` (`SHEET_ID`).

## خطوات الإعداد (مرة واحدة)
1. افتح الشيت ← **الإضافات (Extensions) ← Apps Script**.
2. احذف الملفات الافتراضية وأنشئ/الصق: `Code.gs`، `Index.html`، `Denied.html`، `Logos.html`، `JdLogo.html` (من مجلد `apps-script/`) ثم احفظ.
3. شغّل الدالة **`setup()`** مرة واحدة (تنشئ التبويبات وقائمة `AllowedUsers`) ووافق على الصلاحيات (الشيت + ترجمة اللغة + هوية المستخدم).
4. **Deploy ← New deployment ← Web app**:
   - Execute as: **Me** · Who has access: **Anyone within Sulaiman Al Rajhi University**
   - انسخ رابط `/exec`.
5. رابط `/exec` مثبّت مسبقًا في `index.html` (`CONFIG.WEB_APP_URL`)؛ غيّره فقط إذا أنشأت نشرًا جديدًا.
6. ارفع محتوى هذا المجلد إلى مستودع GitHub (مثلاً `ICTSRU/jd`) ← **Settings ← Pages ← Branch main / root**.
7. اختبر: افتح رابط صفحة GitHub ← سجّل بحساب `@sr.edu.sa` (يجب أن يفتح النموذج) ثم بحساب خارجي (يجب أن تظهر «غير مصرّح»)، وتأكد من ظهور الدخول في `AccessLog`.

> عند تعديل الكود لاحقًا: **Deploy ← Manage deployments ← Edit ← New version** (لا تنشئ New deployment حتى يبقى رابط `/exec` ثابتًا).

## صفحة «جميع السجلات» (v1.2)
تبويب **جميع السجلات** داخل الصفحة نفسها (أو الرابط `…/exec?page=records` أو `records/index.html` على GitHub): بطاقات إحصاء، بحث، فلتر الحالة والنواقص، فرز، فتح السجل في النموذج، **PDF** لأي سجل، **طباعة / PDF لجميع السجلات** المعروضة (ملف واحد)، وتصدير **CSV (Excel)** و**JSON** للنتائج المعروضة. شارة «ناقص» تعدّ الحقول (عربي/إنجليزي) الفارغة.

## التحكم في الوصول (Kit v1.4)
`ACCESS_MODE = 'LIST'`: الدخول لمن في تبويب `AllowedUsers` (عمود Active ✔). الافتراضي مديرا النظام فقط؛ أضف المستخدمين هناك (أو غيّر إلى `'DOMAIN'` لكل حسابات الجامعة). تُسجَّل الدخولات في `AccessLog` (تُنقل نسخة v1.0 القديمة تلقائيًا إلى `AccessLog_old_…`).

## الاستيراد الدفعي (v1.1)
زر **استيراد دفعة** ← اختر ملف `jd_batch_import.json` ← تُحفظ كل الأوصاف بحالة «مسودة». يمكن إنشاء الملف من ملفات Word (يتولى Claude التحويل والترجمة) ثم مراجعة السجلات من «فتح سجل محفوظ».

## الاستخدام
1. املأ الحقول بالعربية والإنجليزية (يمكن استخدام أزرار «ترجمة» لمسودة آلية ثم مراجعتها).
2. **حفظ في Google Sheets** (أو Ctrl+S). الحفظ بنفس الرقم المرجعي يحدّث نفس السجل ويرفع رقم المراجعة.
3. **فتح سجل محفوظ** للبحث وتعديل أي وصف سابق. **نسخة جديدة** لبدء وصف جديد من سجل قائم.
4. **طباعة / حفظ PDF** ← اختر «حفظ كـ PDF» وفعّل «الرسوميات في الخلفية (Background graphics)» إن لم تكن مفعّلة. يُستحسن Chrome/Edge.

## إعدادات في `Code.gs`
- `ACCESS_MODE`: `'LIST'` (الافتراضي، يشترط الإدراج في `AllowedUsers`) أو `'DOMAIN'` (أي حساب `@sr.edu.sa`).
- `SHEET_ID`, `TZ` (Asia/Riyadh).

## ملاحظات
- شعارات SRU/ICTD/ITOC والخلفية التقنية تُحمَّل من الحزمة المشتركة `ictsru.github.io/brand`.
- اقتراحات حقل «الإدارة» تُجلب (إن أمكن) من `sru-departments.json` المركزي، والكتابة الحرة مسموحة دائمًا.
- الصفحة تعمل أيضًا محليًا (وضع معاينة يحفظ في المتصفح فقط) عند فتحها خارج Apps Script بعد استبدال علامات القالب.
