# 📊 Crypto Pivot Scanner — Fibonacci Daily

Scanner cryptocurrency realtime dengan indikator **Pivot Fibonacci Standard** timeframe Daily.

---

## ✨ Fitur

- ✅ **Realtime** via Binance WebSocket Stream (harga live setiap tick)
- ✅ **30 pair** USDT teratas secara default
- ✅ **Pivot Fibonacci Daily** — PP, R1/R2/R3, S1/S2/S3 berdasarkan data H/L/C kemarin
- ✅ **Sinyal otomatis** — Strong Beli, Beli, Netral, Jual, Strong Jual
- ✅ Filter, search, sort, dan filter sinyal
- ✅ Detail panel dengan bar posisi Fibonacci
- ✅ Flash animasi saat harga bergerak
- ✅ Auto-reconnect WebSocket
- ✅ **Tidak butuh backend / server** — murni frontend

---

## 🚀 Cara Deploy

### Opsi 1 — Buka Langsung di Browser (Paling Mudah)

```bash
# Cukup buka file index.html di browser
open index.html         # macOS
start index.html        # Windows
xdg-open index.html     # Linux
```

> ⚠️ Catatan: Beberapa browser memblokir WebSocket dari `file://`. Gunakan opsi 2 jika tidak bisa.

---

### Opsi 2 — Local Server (Direkomendasikan untuk Development)

**Menggunakan Python:**
```bash
cd crypto-pivot-scanner
python3 -m http.server 8080
# Buka: http://localhost:8080
```

**Menggunakan Node.js (npx):**
```bash
cd crypto-pivot-scanner
npx serve .
# Buka: http://localhost:3000
```

**Menggunakan VS Code:**
- Install ekstensi **Live Server**
- Klik kanan `index.html` → Open with Live Server

---

### Opsi 3 — Deploy ke Netlify (Gratis, Online Permanen)

1. Buat akun di [netlify.com](https://netlify.com)
2. Drag & drop folder `crypto-pivot-scanner` ke dashboard Netlify
3. Selesai! Dapat URL publik seperti `https://your-scanner.netlify.app`

```bash
# Atau via Netlify CLI
npm install -g netlify-cli
netlify deploy --dir=crypto-pivot-scanner --prod
```

---

### Opsi 4 — Deploy ke GitHub Pages (Gratis)

```bash
# 1. Buat repo baru di GitHub
# 2. Upload folder isi crypto-pivot-scanner ke repo

git init
git add .
git commit -m "Crypto Pivot Scanner"
git remote add origin https://github.com/USERNAME/crypto-pivot-scanner.git
git push -u origin main

# 3. Di GitHub: Settings → Pages → Source: main branch
# URL: https://USERNAME.github.io/crypto-pivot-scanner
```

---

### Opsi 5 — Deploy ke Vercel (Gratis, Cepat)

```bash
npm install -g vercel
cd crypto-pivot-scanner
vercel --prod
```

---

### Opsi 6 — Deploy ke VPS / Nginx

```nginx
server {
    listen 80;
    server_name scanner.domain.com;
    root /var/www/crypto-pivot-scanner;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

```bash
# Upload file
scp -r crypto-pivot-scanner/ user@server:/var/www/

# Reload Nginx
sudo nginx -t && sudo systemctl reload nginx
```

---

## 📐 Formula Pivot Fibonacci

```
PP  = (High + Low + Close) / 3       ← Pivot Point (kemarin)

R1  = PP + 0.382 × (High - Low)
R2  = PP + 0.618 × (High - Low)
R3  = PP + 1.000 × (High - Low)

S1  = PP − 0.382 × (High - Low)
S2  = PP − 0.618 × (High - Low)
S3  = PP − 1.000 × (High - Low)
```

## 🎯 Logika Sinyal

| Kondisi              | Sinyal       |
|----------------------|--------------|
| Harga ≤ S2           | Strong Beli  |
| S2 < Harga ≤ S1      | Beli         |
| S1 < Harga < R1      | Netral       |
| R1 ≤ Harga < R2      | Jual         |
| Harga ≥ R2           | Strong Jual  |

---

## 🔧 Kustomisasi

**Menambah simbol baru** — edit array `SYMBOLS` di `index.html`:
```javascript
const SYMBOLS = [
  'BTCUSDT','ETHUSDT', // tambahkan pair Binance di sini
  'SOLUSDT','XRPUSDT',
  // ...
];
```

**Mengubah logika sinyal** — edit fungsi `getSignal()`:
```javascript
function getSignal(price, p) {
  if (price <= p.s1) return 'strong_buy';   // lebih agresif
  if (price >= p.r1) return 'strong_sell';
  return 'neutral';
}
```

**Mengganti timeframe pivot** — ganti `interval=1d` di `fetchDailyKlines()`:
```javascript
// Weekly pivot:
const url = `https://api.binance.com/api/v3/klines?symbol=${sym}&interval=1w&limit=2`;
// 4H pivot:
const url = `https://api.binance.com/api/v3/klines?symbol=${sym}&interval=4h&limit=2`;
```

---

## 📡 Data Source

- **Harga Realtime**: Binance WebSocket `wss://stream.binance.com:9443/stream?streams=...@miniTicker`
- **OHLCV Daily**: Binance REST API `GET /api/v3/klines?interval=1d`
- Tidak memerlukan API key (endpoint publik)

---

## 📁 Struktur File

```
crypto-pivot-scanner/
└── index.html      ← Satu file, siap pakai
```

---

*Scanner ini menggunakan Binance public API. Tidak ada data yang dikirim ke server pihak ketiga.*
