# TWENTY · NextGenLAB · NASA Space Apps 2026

Astronotlar için sağlık izleme ve karar destek yazılımı.

Psikolojik destek akışı, görev sırasında korku ve kaygıyı yargılamadan ele alır; astronotun bildirdiği durum türü ve belirtilere göre sistemin verdiği açıklanabilir 1–10 önem seviyesiyle yer ekibi bildirimi taslağı sunar. Tanı veya tedavi yerine geçmez.

- `PLAN.md`: proje fikri, özellikler, senaryolar, zaman çizelgesi
- `TASARIM.md`: renkler, fontlar, Türkçe metinler, animasyon kuralları
- `tasarim/`: GPT ile üretilen tasarım görselleri (referans)
- `app/`: uygulama (React + Vite + TypeScript)

Arayüzü, belirti sınıflandırmayı, kabus sonrası uyandırma akışını ve birinci şahıs 3D EVA görevini görmek için `TWENTY-demo.html` dosyasını bir tarayıcıda aç. Astronotu WASD ve ↑/↓ ile fiziksel anten desteğine doğru hareket ettir; çarpışınca sistemli/sistemsiz animasyonlar kendiliğinden başlar. Tek dosyalık demo kurulum gerektirmez; 3D sahne, sensör verileri ve bildirimler simülasyondur.

Vercel gibi statik barındırma servislerinde kök sayfa `index.html` dosyasından açılır; bu dosya demo arayüzünün kopyasıdır.

## Çalıştırma

Node.js 20 veya üzeri gerekir.

```bash
cd app
npm install
npm run dev
```

Tarayıcıda `http://localhost:5173` açılır.

## Nerede ne var

| Değiştirmek istediğin | Dosya |
|---|---|
| Ekrandaki Türkçe / İngilizce yazılar | `app/src/i18n/tr.ts`, `app/src/i18n/en.ts` |
| Renkler ve fontlar | `app/src/index.css` (`@theme` bloğu) |
| Animasyon süreleri, açılış sırası | `app/src/theme/motion.ts` |
| Ölçüm eşikleri, kişisel aralıklar | `app/src/sim/vitals.ts` |
| Görev günü, astronot adı | `app/src/config.ts` |
| Vücut figürü çizimleri | `app/src/components/bodyShapes.ts` |
