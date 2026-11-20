# MarketUz — Zamonaviy Marketplace (E-Commerce)

O'zbekistondagi **Uzum Market** uslubiga mos, zamonaviy va tezkor internet-do'kon platformasi.

---

## 🚀 Texnologiyalar

- **Asosiy freymvork:** Next.js 14 (App Router) + React 18 + TypeScript
- **Dizayn & Stillar:** Tailwind CSS (Uzum binafsha ranglar palitrasi `#7000FF`, yumshoq soyalar, mobil-moslashuvchanlik)
- **Ikonkalar:** Lucide React
- **Holat boshqaruvi (State Management):** React Context API + `localStorage` (Savat va Saralanganlar sahifa yangilansa ham to'liq saqlanadi)
- **Telegram Integratsiyasi:** Server Route (`app/api/order/route.ts`), Telegram Bot API orqali kanalga real vaqtda yangi buyurtmalar xabarnomasi
- **Mahsulotlar:** `data/products.json` (48 ta to'liq namunaviy mahsulot, 8 ta kategoriya, narxlar, oylik to'lov, sharhlar, xususiyatlar va lokal SVG tasvirlar)
- **Ko'p tillilik (i18n):** O'zbek (lotin) va Rus tillari (sarlavhada UZ/RU almashtirgich)

---

## 📂 Loyiha strukturasi

```text
├── data/
│   ├── categories.ts        # 8 ta asosiy toifa va ularning subkategoriyalari
│   └── products.json        # 48 ta to'liq mahsulotlar bazasi
├── public/
│   └── images/products/     # Mahsulotlarning sifatli lokal vektor SVG rasmlari (144 ta)
├── scripts/
│   └── generate-data.js     # Namunaviy ma'lumotlar va SVG tasvirlarni qayta yaratuvchi skript
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   └── order/
│   │   │       └── route.ts # Telegram Bot API server integratsiyasi va xavfsizlik
│   │   ├── catalog/         # Filtrlar, qidiruv va saralash sahifasi
│   │   ├── cart/            # Savat sahifasi va xulosa
│   │   ├── checkout/        # To'liq buyurtma rasmiylashtirish sahifasi
│   │   ├── favorites/       # Saralanganlar (Wishlist) sahifasi
│   │   ├── order-success/   # Buyurtma muvaffaqiyatli qabul qilinganligi sahifasi
│   │   ├── product/[id]/    # Mahsulot sahifasi, galereya va tezkor sotib olish
│   │   ├── profile/         # Foydalanuvchi profili sahifasi
│   │   ├── globals.css      # Tailwind va scrollbar stillari
│   │   ├── layout.tsx       # Asosiy layout (Header, Footer, BottomNav)
│   │   └── page.tsx         # Bosh sahifa (Bannerlar, Kategoriyalar, 4 ta to'plam)
│   ├── components/
│   │   ├── BannerSlider.tsx # Avto-aylanuvchi banner karuseli
│   │   ├── BottomNav.tsx    # Mobil pastki navigatsiya paneli
│   │   ├── CategoryPills.tsx# Kategoriyalar gorizontal qatori
│   │   ├── CheckoutModal.tsx# "Hoziroq sotib olish" tezkor modali
│   │   ├── Footer.tsx       # Sayt pastki qismi va to'lov tizimlari
│   │   ├── Header.tsx       # Sticky qidiruv, katalog megamenyu va boshqaruv paneli
│   │   ├── ProductCard.tsx  # Uzum uslubidagi mahsulot kartochkasi
│   │   └── Providers.tsx    # Barcha Context Providerlar
│   ├── context/
│   │   ├── CartContext.tsx  # Savat boshqaruvi va localStorage
│   │   ├── LanguageContext.tsx # Til boshqaruvi (UZ / RU)
│   │   └── WishlistContext.tsx # Saralanganlar va localStorage
│   ├── lib/
│   │   ├── translations.ts  # Tarjimalar lug'ati
│   │   └── utils.ts         # Narx (1 250 000 so'm), telefon maskasi, HTML escape
│   ├── types/
│   │   └── index.ts         # TypeScript interfeyslari
│   └── config.ts            # Sayt nomi va sozlamalari (SITE_NAME = "MarketUz")
├── .env.local               # Telegram bot maxfiy sozlamalari
├── .env.example             # Namunaviy muhit o'zgaruvchilari
├── package.json
├── tailwind.config.js
└── tsconfig.json
```

---

## ⚙️ Sozlamalar va O'rnatish

### 1. Repozitoriyani yuklab olish va paketlarni o'rnatish:
```bash
npm install
```

### 2. Muhit o'zgaruvchilarini sozlash (`.env.local`):
Loyihaning ildiz papkasida `.env.local` faylini yarating va quyidagi parametrlarni kiriting:
```env
TELEGRAM_BOT_TOKEN="9283550757:AAGgwXv3hUEbLKcZuTXOaCs_r-ridupOyKQ"
TELEGRAM_CHAT_ID="-1004317314614"
```

> **Eslatma:** Ushbu o'zgaruvchilar `NEXT_PUBLIC_` prefiksisiz kiritilgan. Shuning uchun ular faqat server tomonida (`app/api/order/route.ts`) ishlaydi va hech qachon brauzer kodiga yoki tarmoq so'rovlariga oshkor bo'lmaydi.

### 3. Loyihani ishlab chiqish rejimida ishga tushirish:
```bash
npm run dev
```
Brauzerda `http://localhost:3000` manzilini oching.

### 4. Ishlab chiqarish (Production) build qilish:
```bash
npm run build
npm run start
```

---

## 🛠️ Sayt nomini o'zgartirish

Sayt nomini osongina o'zgartirish uchun `src/config.ts` faylini oching:
```typescript
export const SITE_NAME = "MarketUz"; // O'zingiz xohlagan nomni yozing
```
Butun sayt bo'ylab (Header, Footer, Metadata, Sahifalar) nom avtomatik yangilanadi.

---

## 📲 Telegram Integratsiyasi Tafsilotlari

Buyurtma berilganda `app/api/order/route.ts` orqali Telegram Bot API'ga so'rov yuboriladi:
- **Serverda narxni qayta hisoblash:** Mijoz yuborgan narxga ishonilmaydi, har bir tovar serverdagi `data/products.json` dan tekshirilib, narx serverda xavfsiz hisoblanadi.
- **In-memory Rate Limiting:** Bitta IP manzildan daqiqasiga ko'pi bilan 3 ta buyurtma qabul qilinadi.
- **Honeypot:** Yashirin maydon orqali spam-botlar avtomatik filtrlanadi.
- **HTML Escape:** Mijoz tomonidan kiritilgan barcha matnlar (`<`, `>`, `&`) xavfsiz filtrlanadi.

**Kanalga yuboriladigan xabar ko'rinishi:**
```html
🛒 <b>YANGI BUYURTMA #1234</b>
🕒 20.11.2026 15:42

👤 <b>Mijoz:</b> Alisher Navoiy
📞 <b>Telefon:</b> +998 90 123 45 67
📍 <b>Manzil:</b> Toshkent shahri, Yunusobod tumani, Amir Temur ko'chasi 15-uy
💬 <b>Izoh:</b> Kuryer yetib kelgach qo'ng'iroq qilsin

📦 <b>Mahsulotlar:</b>
1. Smartfon Nova Pro Max 256GB Midnight Black (Qora (Midnight), 256GB) — 1 ta × 3 850 000 so'm = 3 850 000 so'm
2. Simsiz quloqchinlar AirPulse ANC v2 — 1 ta × 420 000 so'm = 420 000 so'm

🚚 <b>Yetkazish:</b> Uygacha kuryer orqali yetkazish
💳 <b>To'lov:</b> Naqd pul orqali
💰 <b>JAMI: 4 270 000 so'm</b>
```

---

## ☁️ Vercel'ga Deploy qilish yo'riqnomasi

1. GitHub'da yangi repozitoriy yarating va kodlarni unga yuklang:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: MarketUz e-commerce platform"
   git remote add origin https://github.com/<username>/<repo-name>.git
   git push -u origin main
   ```
2. [Vercel](https://vercel.com) saytiga kiring va o'z hisobingizga kiring.
3. **"Add New..."** -> **"Project"** tugmasini bosing va GitHub repozitoriyangizni tanlang.
4. **Environment Variables** bo'limida quyidagi ikkita o'zgaruvchini qo'shing:
   - `TELEGRAM_BOT_TOKEN`: `9283550757:AAGgwXv3hUEbLKcZuTXOaCs_r-ridupOyKQ`
   - `TELEGRAM_CHAT_ID`: `-1004317314614`
5. **Deploy** tugmasini bosing. 1-2 daqiqada saytingiz jonli efirga chiqadi!
