# AMS Electronics — مکمل شاپ PWA اور ایڈمن پینل

یہ پروجیکٹ دو الگ صفحات فراہم کرتا ہے:
- `index.html` — خریداروں کی شاپ، سرچ، کیٹیگری، کارٹ، چیک آؤٹ اور WhatsApp آرڈر
- `admin/index.html` — Supabase Auth کے ذریعے محفوظ ایڈمن لاگ اِن، پروڈکٹ CRUD، فوٹو اپ لوڈ، اسٹاک اور آرڈر اسٹیٹس

## ضروری بات
کوڈ اور ڈیزائن تیار ہیں، مگر آپ کے نام سے لائیو ویب سائٹ خود بخود شائع نہیں ہو سکتی جب تک آپ کی ہوسٹنگ اور Supabase پروجیکٹ نہ بنے۔ نیچے والے مراحل ایک مرتبہ کرنے ہیں۔ **کبھی بھی `service_role` key ویب سائٹ یا `config.js` میں نہ ڈالیں۔** صرف `anon/public` key استعمال کریں۔

## 1) Supabase بیک اینڈ بنائیں
1. `https://supabase.com` پر اکاؤنٹ بنا کر نیا Project بنائیں۔ Database password محفوظ رکھیں۔
2. Project Settings → API سے Project URL اور `anon`/publishable key لیں۔
3. اس ZIP کے `config.js` میں `SUPABASE_URL` اور `SUPABASE_ANON_KEY` بدلیں۔ `WHATSAPP_NUMBER` میں اپنے دکان کے WhatsApp نمبر کو پاکستان کے کوڈ سمیت لکھیں، مثال: `923001234567` (پلس کا نشان نہ دیں)۔
4. Supabase → SQL Editor → New query کھولیں، `supabase_setup.sql` کی پوری فائل پیسٹ کریں اور Run کریں۔
5. Supabase → Authentication → Users → Add user میں اپنا ایڈمن ای میل/مضبوط پاس ورڈ بنائیں۔ اس صارف کا UUID کاپی کریں۔
6. SQL Editor میں صرف یہ کمانڈ چلائیں، UUID اپنی اصل ID سے بدلیں:
   `insert into public.admin_users(user_id) values ('YOUR-ADMIN-USER-UUID');`
7. ایڈمن صفحہ کھولیں: `https://YOUR-DOMAIN/admin/` اور اسی ای میل/پاس ورڈ سے لاگ اِن کریں۔ ایڈمن کا اختیار صرف `admin_users` ٹیبل میں موجود UUID کو ملے گا۔

## 2) مفت ہوسٹنگ (GitHub Pages یا Netlify)
1. GitHub پر نیا public repository بنائیں، مثلاً `ams-electronics-shop`۔
2. اس ZIP کے اندر موجود تمام فائلیں اور `assets`/`admin` فولڈر repository کی root میں اپ لوڈ کریں۔ `index.html` root پر ہونا چاہیے۔
3. GitHub repository → Settings → Pages → Deploy from branch → `main` اور `/ (root)` منتخب کریں۔ یا Netlify پر پورا فولڈر deploy کریں۔
4. HTTPS live ہونے کے بعد شاپ اور `/admin/` کھولیں۔ PWA install کے لیے Chrome کے مینیو سے Install app/Add to Home screen منتخب کریں۔

## 3) ٹیسٹ چیک لسٹ
- شاپ پر ایک پروڈکٹ ایڈمن سے شامل کریں اور دیکھیں کہ شاپ پر نظر آتی ہے۔
- پروڈکٹ تصویر JPEG/PNG/WebP/AVIF اور زیادہ سے زیادہ 5 MB رکھیں۔
- ٹیسٹ آرڈر دیں، پھر Admin → Orders میں اس کی تصدیق اور status تبدیل کریں۔
- WhatsApp نمبر درست کر کے ایک چھوٹا ٹیسٹ آرڈر کریں۔
- اصل استعمال سے پہلے ایک کم قیمت ٹیسٹ آرڈر، اسٹاک، فون اور ایڈریس فیلڈ چیک کریں۔

## موجودہ حدود
- یہ آن لائن کیٹلاگ اور آرڈر مینجمنٹ ہے، ادائیگی گیٹ وے شامل نہیں؛ ادائیگی/ڈیلیوری کی تصدیق دکان خود کرے گی۔
- آرڈر مستقل Supabase میں محفوظ ہوتا ہے؛ WhatsApp والا آرڈر صارف کے WhatsApp میں message کھولتا ہے اور صارف خود Send کرتا ہے۔
- `config.js` میں anon/public key براؤزر میں نظر آنا معمول ہے؛ حفاظت Supabase RLS پالیسیوں اور server-side function سے ہے۔ `service_role` key کبھی شامل نہ کریں۔
- اس ورژن میں کسٹمر اکاؤنٹ/لاگ اِن نہیں؛ آرڈر نام، فون اور ایڈریس کے ذریعے ہوتا ہے۔
- ایپ کو حقیقی اسٹور کے لیے استعمال کرنے سے پہلے پاکستان میں ٹیکس/واپسی/پرائیویسی/ڈیلیوری پالیسی اور پروڈکٹ کی درست قیمتیں شامل کریں۔
