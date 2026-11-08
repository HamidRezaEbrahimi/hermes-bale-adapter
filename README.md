# Bale (بله) Platform Plugin for Hermes Agent

آداپتور و پلاگین رسمی پیام‌رسان ایرانی **«بله» (Bale)** برای فریم‌ورک **Hermes Agent**.

این پلاگین به Hermes Agent اجازه می‌دهد بدون نیاز به VPN یا پراکسی در داخل ایران، به عنوان ربات بله فعالیت کرده و امکان گفتگوی دوطرفه، اجرای دستورات، نوتیفیکیشن‌ها و تسک‌های زمان‌بندی‌شده را فراهم کند.

---

## ویژگی‌ها

- ✅ **اتصال مستقیم**: استفاده از Bot API بله بر بستر `https://tapi.bale.ai/bot` (بدون فیلتر)
- ✅ **همزمانی با تلگرام**: فعالیت کاملاً موازی در کنار پلتفرم تلگرام بدون تداخل
- ✅ **پشتیبانی کامل از پیام‌ها**: ارسال و دریافت متن، استریم، مالتی‌مدیا و فال‌بک خودکار MarkdownV2 به متن ساده
- ✅ **پشتیبانی از Allowlist**: امکان تعیین شناسه‌های مجاز (`BALE_ALLOWED_USERS`) برای جلوگیری از دسترسی‌های غیرمجاز
- ✅ **ارسال مستقل (Standalone)**: سازگاری کامل با ابزار `send_message` و تسک‌های پس‌زمینه/کران‌جاب‌ها (`cron`)
- ✅ **پشتیبانی اختیاری از پراکسی**: پشتیبانی از `BALE_PROXY` در صورت نیاز به روتینگ اختصاصی

---

## نحوه نصب و فعال‌سازی

### ۱. کپی پوشه پلاگین
محتوای این ریپازیتوری را در مسیر پلاگین‌های هرمس قرار دهید:
```bash
# داخل ریشه hermes-agent:
plugins/platforms/bale/
```

### ۲. تنظیم متغیرهای محیطی (.env)
متغیرهای زیر را در فایل `.env` پروژه قرار دهید:
```dotenv
# توکن ربات دریافتی از بازوی BotFather در بله:
BALE_BOT_TOKEN="your_bale_bot_token_here"

# شناسه چت یا کاربر پیش‌فرض جهت دریافت پیام‌ها و آلرت‌ها:
BALE_HOME_CHANNEL="your_chat_id"

# شناسه‌های کاربری مجاز (جدا شده با کاما) جهت امنیت:
BALE_ALLOWED_USERS="your_chat_id"
```

### ۳. فعال‌سازی در کانفیگ Hermes
```bash
hermes plugins enable platforms/bale
hermes config set platforms.bale.enabled true
```

---

## فایل‌های تغییریافته در هسته Hermes (Core Patch)

جهت سازگاری کامل لودر محیطی و فرستندهٔ مستقل Hermes با پلتفرم‌های سفارشی، فایل پچ `hermes-core-bale.patch` درون این مخزن قرار داده شده است که شامل موارد زیر است:
1. `gateway/config_env.py`: پل ارتباطی انتقال توکن `BALE_BOT_TOKEN` از seed به `platform_config.token`.
2. `tools/send_message_senders.py`: افزودن تشخیص توکن بله و اتصال به `tapi.bale.ai` همراه با خواندن `BALE_PROXY`.
3. `tools/send_message_tool.py`: مسیریابی ارسال پلتفرم `bale` به فرستنده مستقل.

برای اعمال پچ روی هرمس:
```bash
git apply hermes-core-bale.patch
```

---

## لایسنس
MIT License
