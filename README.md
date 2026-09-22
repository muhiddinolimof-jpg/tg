# YouTube → Telegram avtomatik bot

YouTube kanalingizga yangi video chiqqanda, avtomatik ravishda Telegram
kanalingizga **link + sarlavha** shaklida joylaydi (Telegram o'zi linkdan
rasm/thumbnaili ko'rsatadi).

Bulutda ishlaydi (GitHub Actions orqali, **bepul**), har soatda tekshirib turadi.

---

## 1-qadam: Telegram bot yaratish

1. Telegramda **@BotFather** ga yozing.
2. `/newbot` buyrug'ini yuboring, nom bering.
3. Sizga **token** beradi (masalan `123456:ABC-DEF...`) — saqlab qo'ying.
4. Botni o'z Telegram **kanalingizga admin** qilib qo'shing (post yuborish huquqi bilan).
5. Kanal username bo'lsa — `@mening_kanalim` shu ID bo'ladi.
   Kanal maxfiy (username yo'q) bo'lsa, kanal ID raqamini olish kerak
   (masalan `-1001234567890`) — buning uchun kanalga biror xabar yuboring va
   `https://api.telegram.org/bot<TOKEN>/getUpdates` orqali `chat.id` ni ko'ring.

## 2-qadam: YouTube kanal ID sini topish

1. YouTube kanalingizga o'ting → **"About"/"Kanal haqida"** bo'limi →
   **"Share channel" → "Copy channel ID"**.
2. ID `UC` bilan boshlanadi, masalan: `UCxxxxxxxxxxxxxxxxxxxxxx`.

## 3-qadam: GitHub repo tayyorlash

1. Ushbu papkadagi barcha fayllarni yangi (yoki mavjud) GitHub repo'ga yuklang.
2. Repo → **Settings → Secrets and variables → Actions → New repository secret**
   orqali quyidagi 3 ta maxfiy qiymatni qo'shing:
   - `TG_BOT_TOKEN` — BotFather bergan token
   - `TG_CHANNEL_ID` — kanal username yoki ID
   - `YT_CHANNEL_ID` — YouTube kanal ID

3. Tayyor! `.github/workflows/check.yml` fayli avtomatik ravishda **har soatda**
   ishga tushadi va yangi video bo'lsa, Telegramga joylaydi.

## Qo'lda test qilish

GitHub repo → **Actions** bo'limi → "YouTube -> Telegram tekshiruv" →
**Run workflow** tugmasini bosing — darhol ishga tushadi, natijani loglardan
ko'rasiz.

## Tekshirish chastotasini o'zgartirish

`.github/workflows/check.yml` faylidagi shu qatorni o'zgartiring:

```yaml
- cron: "0 * * * *"   # har soatda
```

Masalan har 15 daqiqada: `*/15 * * * *`
(Eslatma: GitHub Actions bepul rejada eng minimal interval odatda ~5 daqiqa,
lekin haqiqiy ishga tushish vaqti biroz kechikishi mumkin.)

## Muhim eslatmalar

- Bot faqat **eng so'nggi 1 ta videoni** kuzatadi va uni oxirgi joylangan
  video bilan solishtiradi — shuning uchun bir vaqtning o'zida 2+ video
  chiqsa, faqat eng oxirgisi topiladi (keyingi tekshiruvda o'tib ketgan
  video qolib ketishi mumkin, chunki API kaliti ishlatilmayapti — bu oddiy
  RSS usuli). Agar bu muammo bo'lsa, aytib bering — YouTube Data API bilan
  kengaytirilgan versiyasini yozib beraman (bir nechta video kuzatadigan).
- `last_video_id.txt` fayli repo'ga avtomatik commit qilinadi — shuning
  uchun holat saqlanib qoladi.
