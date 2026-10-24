# کارخانه داده ۳۱۱ نیویورک (مرحله ۱)

پکیج پایتون `nyc311_factory`: دریافت دسته‌ای و قابل‌ازسرگیری شکایت‌های ۳۱۱ → اعتبارسنجی با قرارداد و قرنطینه → پاک‌سازی → اتصال به جمعیت (سرشماری آمریکا) → دیتاست پرونده‌ای، جدول عریض و `run_manifest.json`.

## اجرا از پوشه خالی (یک دستور)
```bash
./run_all.sh          # Linux/macOS      |      run_all.bat   # Windows
```
این دستور venv می‌سازد، وابستگی‌های قفل‌شده را نصب می‌کند، تست‌ها را اجرا می‌کند و کل کارخانه را می‌چرخاند.
خروجی‌ها در `data/curated/` (دیتاست، جدول عریض، قرنطینه، گزارش کیفیت، `run_manifest.json`).

## دستورها
```bash
pip install -r requirements.txt && pip install -e .
nyc311 run                 # کل کارخانه
nyc311 fetch               # فقط دریافت (قابل ادامه)    | nyc311 curate  # فقط پاک‌سازی/خروجی
nyc311 --mode moving run   # فقط داده «از آخرین اجرا تا امروز»
nyc311 --max-batches 2 run # دریافت را بعد از ۲ دسته متوقف کن؛ اجرای دوباره ادامه می‌دهد
```
تنظیمات (بازه، اندازه دسته، مسیرها، تعداد تلاش مجدد، آستانه قرنطینه، دامنه آژانس) در `config/default.json` است. توکن‌ها فقط با متغیر محیطی (هرگز داخل Git): `NYC311_APP_TOKEN` (اختیاری، سهمیه Socrata) و `CENSUS_API_KEY` (کلید Census).

## کدهای خروج
`0` موفق · `2` تنظیمات · `3` شبکه · `4` تغییر ساختار API · `5` اعتبارسنجی · `6` عبور قرنطینه از آستانه · `7` داده ناکافی · `8` دریافت ناقص (ابتدا fetch کامل شود).

## تست‌ها (بدون اینترنت)
```bash
python -m unittest discover -s tests -t .
```

## نوتبوک
```bash
jupyter nbconvert --to notebook --execute --inplace solution.ipynb
```
خروجی‌های ذخیره‌شده را باید با اجرا روی داده واقعی تولید کنید. اجرای آفلاین روی داده **مصنوعی** (فقط برای دمو):
```bash
python scripts/make_demo_data.py && NYC311_CONFIG=config/demo.json nyc311 --config config/demo.json run
```

## ساختار
`src/nyc311_factory/` کد · `tests/` تست‌ها · `config/` تنظیمات · `docs/data_contract.md` قرارداد و سیاست‌ها · `data/raw` (خارج از Git) · `data/curated` خروجی‌ها.
