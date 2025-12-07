# 📊 xCodeWraith Market Teknik Analiz Agent AI

piyasayı okumak için saatlerini grafiklere gömmeyi bırak, artık bir yapay zeka sana sekreterlik yapacak.

geliştirdiğim bu ai ajanı telegram botuna dönüşüyor. sanki bir arkadaşına mesaj atarmış gibi yazıyorsun **"BTCUSD analiz et"** diyorsun ve arkana yaslanıyorsun. sistem arka planda tam bir wall street analist gibi çalışmaya başlıyor.

- **gpt-4o** isteğini anlıyor, ne istediğini biliyor
- **chart-img api** tradingview'dan profesyonel grafik çekiyor (mum grafik, rsi, macd)
- **gpt-4o vision** grafiği bir insan gibi okuyor (trend, destek, direnç, momentum)
- **sohbet hafızası** var, önceki konuşmaları hatırlıyor

ben kahvemi bitirene kadar telegram'da detaylı bir teknik analiz raporu hazır oluyor.

---

## 🎯 Ne Yapıyor?

```
Telegram Mesajı → AI Ajanı → Grafik Çek → Analiz → Yanıt
                    ↑
            GPT-4o + Hafıza
```

| Özellik | Açıklama |
|---------|----------|
| 📈 **Trend Analizi** | Yükseliş/düşüş/yatay |
| 🎯 **Destek/Direnç** | Kritik seviyeler |
| 📊 **RSI & MACD** | Momentum göstergeleri |
| ⏰ **Görünüm** | Kısa vadeli beklenti |
| 🇹🇷 **Türkçe** | Tüm analizler Türkçe |

---

## 💬 Örnek Kullanım

```
👤: BTCUSD analiz et

🤖: 📊 BINANCE:BTCUSD Teknik Analiz

   🔼 Trend: Yükseliş trendi devam ediyor
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
| 📈 **Hisse** | AAPL, GOOGL, TSLA, NVDA, AMZN, META |
| 💱 **Forex** | EURUSD, GBPUSD, USDJPY |

> Sistem otomatik olarak BINANCE: veya NASDAQ: prefix'ini ekler.

---

## 🛠️ Kurulum

### 1️⃣ Gerekli API'ler

| Servis | Nereden Alınır | Maliyet |
|--------|----------------|---------|
| **Telegram Bot** | [@BotFather](https://t.me/BotFather) | Ücretsiz |
| **OpenAI API** | [platform.openai.com](https://platform.openai.com) | ~$0.01-0.05/analiz |
| **Chart-img** | [chart-img.com](https://chart-img.com) | 100 istek/ay ücretsiz |

### 2️⃣ n8n Kurulumu

```bash
# n8n'i Docker ile çalıştır
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

### 3️⃣ Workflow Import

1. n8n arayüzünü aç (`http://localhost:5678`)
2. **Settings → Import from File**
3. `xCodeWraith Market Teknik Analiz Agent AI.json` dosyasını seç
4. Credential'ları ayarla:
   - `Telegram Bot API` → BotFather'dan aldığın token
   - `OpenAI API` → platform.openai.com'dan aldığın key
   - `Chart-img API` → chart-img.com'dan aldığın bearer token

### 4️⃣ Test Et

Telegram'dan botuna yaz:
```
BTCUSD analiz et
```

---

## 📁 Dosya Yapısı

```
crypto/
├── README.md                                    # Bu dosya
└── xCodeWraith Market Teknik Analiz Agent AI.json  # n8n workflow
```

---

## 🔧 Teknik Detaylar

### Node'lar

| Node | Görevi |
|------|--------|
| `Telegram Tetikleyici` | Mesaj dinler |
| `Yapay Zeka Ajanı` | LangChain agent, karar verir |
| `OpenAI GPT-4o` | Dil modeli |
| `Sohbet Hafızası` | Son 10 mesajı hatırlar |
| `Grafik Çek` | Tool olarak çağrılır |
| `TradingView Grafik API` | Chart-img'den grafik çeker |
| `Base64 Dönüştür` | Görsel hazırlar |
| `Grafik Analizi` | GPT-4o Vision ile analiz |
| `Grafik Gönder` | Telegram'a gönderir |

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

## 🚀 Geliştirme Fikirleri

- [ ] Zamanlanmış otomatik analizler (sabah/akşam)
- [ ] Birden fazla coin için toplu analiz
- [ ] Fiyat alarmları entegrasyonu
- [ ] Notion/Google Sheets loglama
- [ ] Farklı timeframe desteği (15m, 4h, 1d)

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

#n8n #ai #trading #crypto #bitcoin #ethereum #teknikanaliz #gpt4o #telegram #tradingview
