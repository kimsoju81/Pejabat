# Pejabat 2.0 (`pejabat.org`)

**Pejabat** ialah suite produktiviti digital dan Progressive Web App (PWA) luar talian sepenuhnya untuk pengguna Malaysia — merangkumi penyuntingan dokumen, hamparan data, pembentangan slaid, lakaran vektor/imej, studio suntingan video, arkib tempatan dan kalender tugasan dalam satu ruang kerja pantas.

- **Domain Rasmi:** [https://pejabat.org](https://pejabat.org)
- **Sokongan Pelayar:** Chrome / Edge / Safari / Firefox / ChromeOS 103+ / Android TWA & Median.co WebView

---

## Modul Utama

| Modul | Fungsi Utama | Format Eksport / Sokongan |
| --- | --- | --- |
| **Laci** | Arkib fail tempatan, pengurusan draf auto-simpan dan carian pantas | Storan tempatan / JSON |
| **Surat** | Penyunting dokumen rasmi, surat-menyurat, nota pantas dan templat | `.docx`, `.pdf`, `.txt`, `.html` |
| **Lembar** | Hamparan jadual, formula asas dan analisis data | `.xlsx`, `.csv` |
| **Bentang** | Pembina slaid pembentangan dengan jenis MIME rasmi | `.pptx`, `.ppt`, `.ppsx`, `.pdf` |
| **Lukis** | Kanvas lakaran, suntingan imej dan anotasi | `.png`, `.jpg`, `.webp`, `.svg` |
| **Layar** | Studio suntingan video garis masa berbilang trek (video & audio `Aud01`/`Aud02`), kawalan **Stabil** (4 hentian penstabilan) dan **Kabur** (kabur gerakan & campuran) | `.mp4` (`video/mp4` + audio), `.webm` |
| **Kalender** | Jadual harian, tugasan dan peringatan notifikasi | Notifikasi setempat & Push |

---

## Keupayaan PWA & PWABuilder (`public/manifest.json` & `public/sw.js`)

Manifest baharu (`public/manifest.json`) dan Service Worker (`public/sw.js`, cache `v5`) menyokong kesemua piawaian PWA moden:

- **`screenshots`** — Tangkap layar desktop (`wide`, `1280x720`) dan mudah alih (`narrow`, `720x1280`) untuk paparan pemasangan kaya (Rich Install UI).
- **`scope_extensions`** — Membenarkan navigasi dalam skop bagi `pejabat.org`, `www.pejabat.org`, `app.pejabat.org`, `auth.pejabat.org`, dan `ms.pejabat.org` bersama pengesahan `public/.well-known/web-app-origin-association`.
- **`related_applications`** — Mengisytiharkan identiti aplikasi web rasmi `org.pejabat.pwa` (`prefer_related_applications: false`).
- **`iarc_rating_id`** — Kod pensijilan umur IARC rasmi untuk kedai aplikasi.
- **`file_handlers`** — Buka terus fail `.docx`, `.doc`, `.rtf`, `.txt`, `.pdf`, `.xlsx`, `.xls`, `.csv`, `.ods`, `.pptx`, `.ppt`, `.ppsx`, `.odp`, `.png`, `.jpg`, `.webp`, `.svg`, `.mp4`, `.webm`, `.mov`, dan `.layar` terus ke modul berkaitan.
- **`launch_handler`** — Mod `["focus-existing", "auto"]` untuk memfokuskan tetingkap Pejabat sedia ada tanpa membuka salinan berganda.
- **`protocol_handlers`** — Sokongan protokol tersuai `web+pejabat:` dan `mailto:`.
- **`share_target`** — Terima teks, pautan dan fail media melalui `POST /share-target` (`multipart/form-data`) daripada aplikasi lain pada OS.
- **`widgets`** — Widget Windows Adaptive Card (`pejabat-quick-widget`) dengan templat & data JSON dinamik di `public/widgets/`.
- **`edge_side_panel`** — Sokongan bar sisi Microsoft Edge (`preferred_width: 480`).
- **`display_override` (`tabbed`) & `tab_strip`** — Sokongan mod bertab desktop bersama `home_tab` dan `new_tab_button`.
- **`note_taking`** — Pintasan nota baharu terus ke `./index.html?module=surat&action=new-note`.
- **Service Worker (`public/sw.js`)**:
  - **Luar Talian (App Shell)** dengan strategi *Network-First* (`cache: 'no-cache'`) untuk `manifest.json`, `index.html`, `web-app-origin-association`, dan fail widget supaya kemas kini domain `pejabat.org` dipaparkan serta-merta.
  - **Background Sync API** (`sync` — `pejabat-sync-laci`) untuk menyegerakkan data tertangguh apabila internet pulih.
  - **Periodic Background Sync API** (`periodicsync` — `pejabat-periodic-sync`) untuk menyegarkan aset teras dan widget secara berkala.
  - **Web Push Notifications API** (`push`, `pushsubscriptionchange`, `notificationclick`).

---

## Pembangunan Tempatan

Prasyarat: **Node.js 20+** dan **npm**.

```bash
# 1. Pasang pakej
npm install

# 2. Jalankan pelayan pembangunan (port 3000)
npm run dev

# 3. Semak TypeScript
npm run lint

# 4. Bina produksi ke folder dist/
npm run build

# 5. Pratonton binaan produksi
npm run preview
```

---

## Penerbitan (Deployment)

### GitHub Pages
- Aliran kerja automatik tersedia di `.github/workflows/deploy-pages.yml` (dicetuskan apabila menolak ke cabang `main`).
- Fail `public/CNAME` mengandungi `pejabat.org` supaya pautan domain tersuai tidak hilang setiap kali penerbitan baharu dijalankan.

### Netlify
- Dikonfigurasikan melalui `netlify.toml` dan `public/_redirects`:
  - `Cache-Control: no-cache, no-store, must-revalidate` untuk `/`, `/index.html`, `/sw.js`, `/manifest.json`, `/.well-known/*`, dan `/widgets/*`.
  - Pengalihan kanonik `301` daripada `https://www.pejabat.org/*` ke `https://pejabat.org/:splat`.
  - Sandaran SPA `/* -> /index.html 200`.

---
## Kemas Kini DNS & Penyegaran Segera Domain `pejabat.org`

Untuk memastikan perubahan baharu pada **`pejabat.org`** (manifest, ikon, Service Worker dan tetapan PWA) segar semula serta-merta di pelayar dan pengesah PWABuilder:

### 1. Tetapkan Rekod DNS dengan TTL Rendah (`300` saat / 5 minit)
Kemas kini tetapan DNS di panel pendaftar domain anda (contoh: Cloudflare, Namecheap, Porkbun, atau GoDaddy):

#### Pilihan A — Hosting di GitHub Pages

#### Pilihan B — Hosting di Netlify


*(Pilihan subdomain `scope_extensions`: halakan `app`, `auth`, dan `ms` sebagai `CNAME` ke hos yang sama jika digunakan.)*

### 2. Kosongkan Cache DNS & CDN (Instant Refresh)
1. **Cloudflare / CDN:** Klik **Caching → Configuration → Purge Everything** selepas penerbitan.
2. **Pengosongan Resolver Awam (pilihan untuk kesan serta-merta):**
   - Google Public DNS: [https://dns.google/cache](https://dns.google/cache) → masukkan `pejabat.org` (jenis `A`, `AAAA`, `CNAME`) dan tekan **Flush Cache**.
   - Cloudflare 1.1.1.1: [https://one.one.one.one/purge-cache/](https://one.one.one.one/purge-cache/) → masukkan `pejabat.org` dan tekan **Purge Cache**.
3. **Pengesahan Aplikasi:**
   - Buka `https://pejabat.org/manifest.json` untuk mengesahkan medan baharu dimuatkan tanpa cache lama.
   - Service Worker `v5` dan pendaftaran `updateViaCache: 'none'` di `src/main.tsx` akan memaksa pelanggan sedia ada memadam cache `v4` secara automatik dan memuat semula tetapan baharu.
