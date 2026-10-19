# الرحمن پاکستان پراپرٹی ہب — موبائل سے تیار کرنے کی فائلیں

یہ پیکج موبائل کے لیے responsive web app / PWA اور Firebase integration کا سورس کوڈ ہے۔ **یہ ابھی compiled Android APK/AAB نہیں ہے** اور Firebase کی کنفیگریشن مکمل کیے بغیر حقیقی آن لائن اکاؤنٹس یا پوسٹس محفوظ نہیں ہوں گی۔

## موجودہ خصوصیات
- اردو RTL موبائل فرسٹ ڈیزائن
- پاکستان کے متعدد بڑے شہروں کی فہرست، شہر کے حساب سے اشتہارات
- پراپرٹی پوسٹ فارم: قسم، مقصد، شہر، ضلع، علاقہ، سائز، قیمت، تفصیل، رابطہ نمبر، تصویر کا URL
- Firebase Authentication کے لیے signup/login
- Firestore میں پوسٹ کو `pending` حیثیت سے محفوظ کرنے کا کوڈ
- منظور شدہ اشتہارات ہی عوامی طور پر دکھانے کے لیے Firestore rules
- ایڈمن پینل کا ابتدائی کوڈ، منظور/مسترد کارروائی
- PWA manifest اور app icon

## اصل multi-user ایپ فعال کرنے کے لیے (ایک بار سیٹ اپ ضروری)
1. `https://console.firebase.google.com/` کھولیں، Google اکاؤنٹ سے لاگ اِن کرکے نیا project بنائیں۔
2. Project settings → Your apps → Web app میں ویب ایپ بنائیں اور Firebase config حاصل کریں۔
3. `firebase-config.js` میں `PASTE_API_KEY_HERE`, `YOUR_PROJECT_ID`, sender ID اور app ID اپنی اصل values سے تبدیل کریں۔ `adminEmails` میں اپنے ایڈمن اکاؤنٹ کی ای میل درج کریں۔
4. Firebase Authentication → Sign-in method میں **Email/Password** فعال کریں۔
5. Firestore Database بنائیں، پھر `firestore.rules` میں `YOUR_ADMIN_EMAIL@example.com` کو اپنے ایڈمن کی اصل ای میل سے بدل کر قواعد publish کریں۔
6. فائلیں HTTPS hosting پر deploy کریں (مثلاً Firebase Hosting یا کسی static host پر)۔ `index.html` کو صرف موبائل پر کھولنے سے دوسرے لوگوں کے ساتھ مشترکہ ڈیٹا بیس فعال نہیں ہوتا۔
7. کسی دوسرے اکاؤنٹ سے پوسٹ بنا کر جانچیں؛ پوسٹ `pending` رہنی چاہیے۔ ایڈمن اکاؤنٹ سے منظور کریں، پھر عوامی شہر کی فہرست میں نظر آنی چاہیے۔

## احتیاط
- Firebase config میں ویب app کی public configuration آ سکتی ہے، مگر **service account private key یا کوئی admin secret کبھی بھی frontend میں نہ رکھیں**۔
- ایڈمن ای میل کو JS میں لکھ دینا اکیلا سیکیورٹی نہیں ہے؛ اصل اختیار Firestore Rules میں بھی محدود ہے۔ اوپر دیے گئے rules میں ایڈمن ای میل کو لازماً بدلیں۔
- عوامی فون نمبر صارف کی واضح رضامندی کے بعد ہی دکھائیں۔ پوسٹوں کی قانونی حیثیت، ملکیت کے کاغذات اور دھوکا دہی کی شکایات کی جانچ کے لیے الگ moderation process بنائیں۔
- اس پیکج میں فی الحال حقیقی comments، تصویر upload/storage، نقشے پر pin، SMS OTP، push notifications اور مکمل ضلع/تحصیل directory شامل نہیں؛ ان کے لیے مزید development درکار ہے۔
- Google Play Store کے لیے Android App Bundle (AAB)، Play Console اکاؤنٹ، signing، privacy policy اور Google Play کی موجودہ پالیسیوں کی تعمیل درکار ہوگی۔ یہ zip بذاتِ خود Play Store پر اپ لوڈ کرنے والی AAB فائل نہیں ہے۔

## ٹیسٹ
Static files کو HTTPS hosting پر deploy کریں۔ چونکہ `app.js` ES modules اور Firebase SDK استعمال کرتا ہے، عام `file://` کے ذریعے کھولنے کے بجائے hosting استعمال کریں۔
