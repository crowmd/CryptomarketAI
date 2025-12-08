# 📊 xCodeWraith Market Teknik Analiz Agent AI

piyasayı okumak için saatlerini grafiklere gömmeyi bırak, artık bir yapay zeka sana sekreterlik yapacak.

geliştirdiğim bu ai ajanı telegram botuna dönüşüyor. sanki bir arkadaşına mesaj atarmış gibi yazıyorsun **"BTCUSD analiz et"** diyorsun ve arkana yaslanıyorsun. sistem arka planda tam bir wall street analist gibi çalışmaya başlıyor.

- **gpt-4o vision** grafiği bir insan gibi okuyor
- **chart-img api** tradingview'dan profesyonel grafik çekiyor (mum grafik, rsi, macd)
- **sohbet hafızası** var, önceki konuşmaları hatırlıyor
- **langchain agent** ile akıllı karar verme

ben kahvemi bitirene kadar telegram'da detaylı bir teknik analiz raporu hazır oluyor.

---

## 🎯 Ne Yapıyor?

```
┌─────────────────────────────────────────────────────────────┐
│                    ANA İŞ AKIŞI                              │
│  Telegram → AI Ajanı → [grafik_cek tool çağırır] → Yanıt    │
│                 ↑                                            │
│          GPT-4o + Hafıza                                     │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  ALT İŞ AKIŞI (Tool)                         │
│  Sembol Ayrıştır → Chart-img API → GPT-4o Vision →          │
│  → Telegram'a Grafik + Analiz Gönder                         │
└─────────────────────────────────────────────────────────────┘
```

| Özellik | Açıklama |
|---------|----------|
| 📈 **Trend Analizi** | Yükseliş/düşüş/yatay |
| 🎯 **Destek/Direnç** | Kritik seviyeler |
| 📊 **RSI & MACD** | Momentum göstergeleri |
| ⏰ **Görünüm** | Kısa vadeli beklenti |
| 🇹🇷 **Türkçe** | Tüm analizler Türkçe |
| 💬 **Hafıza** | Son 10 mesajı hatırlar |

---

## 💬 Örnek Kullanım

```
👤: BTCUSD analiz et

🤖: [Grafik + Analiz]
   📊 BINANCE:BTCUSD Teknik Analiz

   📈 Trend: Yükseliş trendi devam ediyor
   🎯 Destek: $42,500 | Direnç: $45,000
   📊 RSI: 58 - Nötr bölgede
   📊 MACD: Pozitif sinyal
   ⏰ Görünüm: Kısa vadede pozitif

👤: Peki ETH?

🤖: [Önceki sohbeti hatırlayarak ETH analizi yapar...]
```

---

## ⚙️ Desteklenen Semboller

| Tür | Örnekler |
|-----|----------|
| 🪙 **Kripto** | BTCUSD, ETHUSD, SOLUSD, XRPUSD, ADAUSD, DOGEUSD |
| 📈 **ABD Hisse** | AAPL, GOOGL, TSLA, NVDA, AMZN, META |
| 💱 **Forex** | EURUSD, GBPUSD, USDJPY |

> Sistem otomatik olarak BINANCE: veya NASDAQ: prefix'ini ekler.

---

## 🛠️ Kurulum Rehberi

### Adım 1: Gerekli API Anahtarlarını Al

| Servis | Nereden Alınır | Maliyet |
|--------|----------------|---------|
| **Telegram Bot** | [@BotFather](https://t.me/BotFather) | Ücretsiz |
| **OpenAI API** | [platform.openai.com](https://platform.openai.com) | ~$0.01-0.05/analiz |
| **Chart-img** | [chart-img.com](https://chart-img.com) | 100 istek/ay ücretsiz |

#### 📱 Telegram Bot Oluşturma
1. Telegram'da @BotFather'a git
2. `/newbot` yaz
3. Bot adı ve kullanıcı adını belirle
4. **Token'ı kopyala** (örn: `123456789:ABCdefGHI...`)

#### 🤖 OpenAI API Key Alma
1. [platform.openai.com](https://platform.openai.com) adresine git
2. API Keys menüsünden yeni key oluştur
3. **GPT-4o erişimi** olduğundan emin ol

#### 📊 Chart-img API Key Alma
1. [chart-img.com](https://chart-img.com) adresine git
2. Google ile giriş yap
3. Dashboard'dan API key'i kopyala

---

### Adım 2: n8n Kurulumu

```bash
# Docker ile n8n çalıştır
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

> n8n arayüzü: `http://localhost:5678`

---

### Adım 3: Workflow Import

1. n8n arayüzünü aç
2. Sol menüden **Workflows** → **Import from File**
3. `xCodeWraith Market Teknik Analiz Agent AI.json` dosyasını seç
4. **Import** butonuna tıkla

---

### Adım 4: Credentials (Kimlik Bilgileri) Ayarlama

n8n'de **Settings → Credentials** menüsüne git ve şu 3 credential'ı oluştur:

#### 1️⃣ Telegram Bot API
- **Type:** Telegram API
- **Name:** `Telegram Bot API`
- **Access Token:** BotFather'dan aldığın token

#### 2️⃣ OpenAI API
- **Type:** OpenAI
- **Name:** `OpenAI API`
- **API Key:** platform.openai.com'dan aldığın key

#### 3️⃣ Chart-img API
- **Type:** Header Auth
- **Name:** `Chart-img API`
- **Name:** `Authorization`
- **Value:** `Bearer YOUR_CHARTIMG_API_KEY`

---

### Adım 5: ⚠️ Workflow ID Ayarlama (ÖNEMLİ!)

Bu workflow **self-referencing** yapı kullanıyor. AI Ajanı, grafik çekmesi gerektiğinde aynı workflow'daki alt akışı tetikliyor. Bunun için:

1. Workflow'u import ettikten sonra, **URL'deki workflow ID'yi not et**
   - Örnek: `http://localhost:5678/workflow/abc123` → ID: `abc123`

2. **Grafik Çek** node'una tıkla

3. **Workflow to Call** alanında:
   - "From list" seçeneğini seç
   - Listeden **bu workflow'u** (kendisini) seç
   - VEYA "By ID" seçip ID'yi yapıştır

4. **Kaydet**

---

### Adım 6: Node'lardaki Credential'ları Eşleştir

Her node'a gidip doğru credential'ı seç:

| Node | Credential |
|------|------------|
| `Telegram Tetikleyici` | Telegram Bot API |
| `Telegram Yanıt Gönder` | Telegram Bot API |
| `Grafik Gönder` | Telegram Bot API |
| `OpenAI GPT-4o` | OpenAI API |
| `Grafik Analizi` | OpenAI API |
| `TradingView Grafik API` | Chart-img API |

---

### Adım 7: Workflow'u Aktifleştir ve Test Et

1. Sağ üstten **Active** toggle'ını aç
2. Telegram'dan botuna yaz:
   ```
   BTCUSD analiz et
   ```
3. Grafik + analiz geldi mi? 🎉

---

## 🔧 Teknik Detaylar

### Node Listesi

| Node | Tip | Görevi |
|------|-----|--------|
| `Telegram Tetikleyici` | Trigger | Mesaj dinler |
| `Yapay Zeka Ajanı` | LangChain Agent | Karar verir, tool çağırır |
| `OpenAI GPT-4o` | LLM | Dil modeli |
| `Sohbet Hafızası` | Memory | Son 10 mesaj |
| `Grafik Çek` | Tool Workflow | Alt akışı tetikler |
| `Alt İş Akışı Tetikleyici` | Execute Workflow Trigger | Alt akış başlangıcı |
| `Sembol Ayrıştır` | Code | Sembol + prefix ekler |
| `TradingView Grafik API` | HTTP Request | Chart-img API |
| `Base64 Dönüştür` | Code | Görsel hazırlar |
| `Grafik Analizi` | OpenAI | GPT-4o Vision |
| `Grafik Gönder` | Telegram | Foto + analiz gönderir |

### Chart-img API Ayarları

```json
{
  "symbol": "BINANCE:BTCUSD",
  "interval": "1h",
  "width": 800,
  "height": 600,
  "style": "candle",
  "theme": "dark",
  "timezone": "Europe/Istanbul",
  "studies": [
    { "name": "RSI", "inputs": { "length": 14 } },
    { "name": "MACD" }
  ]
}
```

---

## 💰 Maliyet Tahmini

| Kullanım | Maliyet |
|----------|---------|
| 10 analiz/gün | ~$3-15/ay |
| 50 analiz/gün | ~$15-75/ay |
| 100 analiz/gün | ~$30-150/ay |

> Bir starbucks kahvesi parasıyla haftalarca kullanırsın.

---

## ❓ Sorun Giderme

### "Workflow not found" hatası
→ Adım 5'teki Workflow ID ayarını kontrol et

### "Credential not found" hatası
→ Tüm credential'ları oluşturdun mu? Node'lara eşleştirdin mi?

### Grafik gelmiyor
→ Chart-img API key'in doğru mu? Bearer prefix'i var mı?

### GPT yanıt vermiyor
→ OpenAI API key'in aktif mi? GPT-4o erişimin var mı?

---

## 🚀 Geliştirme Fikirleri

- [ ] Zamanlanmış otomatik analizler (sabah/akşam)
- [ ] Birden fazla coin için toplu analiz
- [ ] Fiyat alarmları entegrasyonu
- [ ] Farklı timeframe desteği (15m, 4h, 1d)
- [ ] BIST desteği ekleme

---

## 👨‍💻 Geliştirici

**xCodeWraith**

---

## 📜 Lisans

MIT License - Dilediğince kullan, isteyen alıp kendi sistemini kursun.

---

## ⚠️ Yasal Uyarı

> **Bu araç yalnızca eğitim ve kişisel kullanım amaçlıdır.**

🚫 Bu araç **FİNANSAL TAVSİYE** niteliği taşımaz.

🚫 Yatırım kararlarınızı bu analizlere dayandırmayın.

🚫 Her türlü kayıp kullanıcının sorumluluğundadır.

*Geliştirici, bu aracın kullanımından doğabilecek zararlardan sorumlu değildir.*

---

#n8n #ai #trading #crypto #bitcoin #ethereum #teknikanaliz #gpt4o #telegram #tradingview #langchain
