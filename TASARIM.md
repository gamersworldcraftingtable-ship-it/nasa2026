# TWENTY · Tasarım ve Animasyon Belgesi

**Kaynak tasarım:** `tasarim/01-saglik-ozeti.webp` (Ekran 1: Sağlık Özeti, GPT ile üretildi)
**Belge sürümü:** v0.1 (31.10.2026)
**Bağlı belge:** `PLAN.md`

Bu belge tasarımı koda dönüştürmenin kurallarıdır: renkler, yazı tipleri, Türkçe metinler, animasyonlar ve bileşen yapısı. Kod yazan herkes önce buna bakar.

---

## 1. Temel kararlar

| Karar | Durum |
|---|---|
| Görsel yön | **"Uçuş El Kitabı"**: kemik rengi kâğıt zemin, mürekkep siyahı, tek vurgu rengi turuncu |
| Arayüz dili | **Türkçe** (varsayılan) |
| İngilizce | **Öneri:** Tüm metinler baştan tek bir sözlük dosyasında tutulsun, sağ üstte TR/EN anahtarı olsun. Global jüri Türkçe okumayabilir; sözlük baştan kurulursa maliyeti çok düşük |
| Proje adı | **TWENTY** (çalışma adı, tasarımda kullanıldı; ekip onayı bekliyor) |
| Hedef ekran | 16:10 dizüstü/tablet (1440×900 ana referans), 1280 genişliğe kadar bozulmadan |
| Animasyon | Her animasyon bir **bilgi** taşır: süs için hareket yok |

---

## 2. Ekran anatomisi (Sağlık Özeti)

```
┌──────────────┬──────────────────┬─────────────────────┬──────────────┬──────────────────────┐
│ TWENTY/      │ MARS YOLUNDA     │ MARS TRANSFERİ·GÜN  │ MÜRETTEBAT   │ DÜNYA BAĞLANTI       │
│ (logo)       │ slogan           │ ●──●──◉─·─·─·─● iz  │ A. DEMİR ▾   │ GECİKMESİ 14:32 (koyu)│
├────────┬─────┴──────────────────┴─────────────────────┴──────────────┴──────────────────────┤
│ MENÜ   │ MÜRETTEBAT SAĞLIĞI / SAĞLIK ÖZETİ  01                                               │
│ ▌Sağlık│ ┌─────────────┐   ┌───────────── VÜCUT HARİTASI ─────────────┐  ┌─────────────┐     │
│  Triyaj│ │ NABIZ       │   │ ÖN | ARKA                                 │  │ VÜCUT ISISI │     │
│  YZ Dr │ │ TANSİYON    │   │ çizgi insan figürü + 7 bölge + etiketler │  │ KAN ŞEKERİ  │     │
│  Dünya │ │ OKSİJEN     │   │                                           │  │ UYKU        │     │
│  Görev │ └─────────────┘   └───────────────────────────────────────────┘  │ RUH HALİ    │     │
│  Ayar  ├──────────────┬───────────────┬──────────────┬──────────────────────────────────────┤
│ [Mars] │ Mars manzara │ BUGÜNÜN ODAĞI │ alıntı       │ HIZLI İŞLEMLER (4 buton)             │
└────────┴──────────────┴───────────────┴──────────────┴──────────────────────────────────────┘
```

---

## 3. Tasarım tokenları

Renkler görselden piksel örneklenerek çıkarıldı; ufak ayarlar uygulamada yapılabilir.

### 3.1 Renkler

| Token | HEX | Kullanım |
|---|---|---|
| `paper` | `#F1EEE3` | Sayfa zemini |
| `paper-raised` | `#F6F0E4` | Kartlar |
| `rule` | `#E4DED2` | İnce çizgiler, kart kenarı, ızgara |
| `ink` | `#0A0A0A` | Başlıklar, büyük rakamlar |
| `ink-soft` | `#3A3833` | Gövde metni |
| `ink-mute` | `#7A766C` | Etiketler, "6 SAAT ÖNCE / ŞİMDİ", aralık bilgisi |
| `signal` | `#F2551A` | Vurgu: logo eğik çizgisi, aktif menü, gecikme sayacı, kalp |
| `signal-wash` | `#F6E4D4` | Aktif menü zemini, kalp ikonunun kutusu |
| `night` | `#0A0B0E` | Gecikme paneli, gece modu zemini |
| `ok` | `#9CD2A6` | NORMAL rozeti zemini |
| `ok-ink` | `#2F7A47` | NORMAL grafik çizgisi, rozet noktası |
| `warn` | `#FCCA70` | KONTROL rozeti zemini |
| `warn-ink` | `#E8622A` | KONTROL grafik çizgisi |
| `alert` | `#E2483D` | MÜDAHALE (tasarımda henüz yok, eklenecek) |
| `slate` | `#788C90` | Uyku çubukları, ruh hali çizgisi |
| `organ-neuro` | `#BCCFDA` | Beyin, akciğer (mavi-gri) |
| `organ-gut` | `#BDD6B7` | Sindirim (yeşil) |
| `organ-heart` | `#F8B490` | Kalp (şeftali, durumuna göre turuncuya döner) |
| `organ-muscle` | `#E9BFAD` | Kaslar |

**Durum sistemi** (renk asla tek başına kullanılmaz, her zaman şekil + etiketle):

| Durum | Rozet | Şekil | Etiket |
|---|---|---|---|
| Normal | `ok` zemin | ● dolu daire | **NORMAL** |
| Dikkat | `warn` zemin | ▲ üçgen | **KONTROL ET** |
| Uyarı | `alert` zemin | ■ kare / çift kenar | **HEMEN MÜDAHALE** |

### 3.2 Yazı tipleri (hepsinin `latin-ext` alt kümesi var; ı İ ş Ş ğ Ğ ç ö ü ilk kurulumda ekranda test edilecek)

| Rol | Font | Ağırlık | Örnek |
|---|---|---|---|
| Ekran başlığı, büyük rakamlar | **Barlow Condensed** | 800 | SAĞLIK ÖZETİ, 98, 118/76 |
| Kart başlıkları, menü, etiketler | **IBM Plex Mono** | 500-600 | NABIZ, TANSİYON, 6 SAAT ÖNCE |
| Gövde, açıklamalar | **IBM Plex Sans** | 400-500 | Senin aralığın, protokol metinleri |
| Alıntı | **IBM Plex Serif** italik | 400 | "Daha sağlıklı bir sen..." |

Hepsi Google Fonts, ücretsiz, `latin-ext` alt kümesiyle yüklenecek.

### 3.3 Ölçüler

- Izgara: 8 px taban. Kart iç boşluğu 24 px, kartlar arası 16 px.
- Köşe: kartlar 4 px (neredeyse keskin, el kitabı hissi), rozetler 2 px.
- Çizgi kalınlığı: kart kenarı 1 px `rule`, vücut çizimi 1,25 px, ikonlar 1,5 px.
- Büyük rakam: 64-72 px. Ekran başlığı: 72 px. Kart başlığı: 16 px mono, harf aralığı +0,04em.

---

## 4. Türkçe metinler

### 4.1 Türkçe tipografi kuralları (önemli)

1. `<html lang="tr">` zorunlu. Yoksa CSS `uppercase` "i" harfini "I" yapar: "TRİYAJ" yerine "TRIYAJ" görünür.
2. **Ondalık ayırıcı virgül:** 36,6 °C, 7,2 saat. Sayılar `Intl.NumberFormat('tr-TR')` ile biçimlenir. Elle nokta yazılmaz.
3. Türkçe kelimeler İngilizceden ortalama %15-25 uzun. Kart başlıkları taşmaya göre test edilir ("OKSİJEN SATÜRASYONU").
4. Birimler: `atım/dk`, `mmHg`, `% SpO₂`, `°C`, `mg/dL`, `saat`.
5. Metinlerde uzun tire kullanılmaz.

### 4.2 Metin sözlüğü (tasarımdaki İngilizce → arayüzde Türkçe)

| Yer | Tasarımda | Arayüzde |
|---|---|---|
| Slogan 1 | ON THE WAY TO MARS | MARS YOLUNDA |
| Slogan 2 | THE DOCTOR ON EARTH IS 20 MINUTES AWAY | DÜNYA'DAKİ DOKTOR 20 DAKİKA UZAKTA |
| Görev | MARS TRANSIT · DAY 94 | MARS TRANSFERİ · GÜN 94 |
| Görev izi | EARTH / CRUISE / MARS | DÜNYA / SEYİR / MARS |
| Mürettebat | CREW MEMBER | MÜRETTEBAT |
| Gecikme | EARTH LINK DELAY · MM:SS | DÜNYA BAĞLANTI GECİKMESİ · DK:SN |
| Menü | HEALTH OVERVIEW | SAĞLIK ÖZETİ |
| Menü | TRIAGE | TRİYAJ |
| Menü | AI DOCTOR | YZ DOKTOR |
| Menü | EARTH WINDOW | DÜNYA PENCERESİ |
| Menü | MISSION TOOLS | GÖREV MODU |
| Menü | SETTINGS | AYARLAR |
| Yan görsel | A HEALTHY CREW BRINGS US FURTHER | SAĞLIKLI MÜRETTEBAT BİZİ DAHA İLERİ TAŞIR |
| Üst başlık | CREW HEALTH | MÜRETTEBAT SAĞLIĞI |
| Başlık | HEALTH OVERVIEW | SAĞLIK ÖZETİ |
| Alt başlık | REAL-TIME MONITORING · EARLY INSIGHTS · A STRONGER TOMORROW | ANLIK İZLEME · ERKEN UYARI · GÜÇLÜ YARINLAR |
| Kart | HEART RATE | NABIZ |
| Kart | BLOOD PRESSURE | TANSİYON |
| Kart | OXYGEN SATURATION | OKSİJEN SATÜRASYONU |
| Kart | BODY TEMPERATURE | VÜCUT ISISI |
| Kart | BLOOD SUGAR | KAN ŞEKERİ |
| Kart | SLEEP (LAST 24H) | UYKU (SON 24 SAAT) |
| Kart | MOOD | RUH HALİ |
| Grafik ekseni | 6 HOURS / NOW | 6 SAAT ÖNCE / ŞİMDİ |
| Aralık | Your range: 60 - 90 bpm | Senin aralığın: 60-90 atım/dk |
| Aralık | Your range (fasting) | Senin aralığın (açlık) |
| Hedef | Target: 7 - 9 hours | Hedef: 7-9 saat |
| Ruh hali | How are you feeling right now? | Şu an kendini nasıl hissediyorsun? |
| Destek | What feels hardest right now? | Şu an seni en çok ne zorluyor? |
| Destek | Fear / loneliness / uncertainty / fatigue / something else | Korku / yalnızlık / belirsizlik / yorgunluk / başka |
| Destek | Take a moment. You don't have to solve everything at once. | Bir an dur. Her şeyi aynı anda çözmen gerekmiyor. |
| Destek | Try a 60-second breathing exercise | 60 saniyelik nefes egzersizini dene |
| Destek | Try a grounding exercise | Şimdiye odaklanma egzersizini dene |
| Destek | Skip / Stop exercise | Atla / Egzersizi durdur |
| Destek | How do you feel now? | Şimdi nasıl hissediyorsun? |
| Destek | What happened? / What are you experiencing? | Ne oldu? / Hangi belirtileri yaşıyorsun? |
| Destek | System-assessed importance: 5 / 10 | Sistem önem seviyesi: 5 / 10 |
| Destek | Assessment based on reported symptoms; not a diagnosis. | Bildirilen belirtilere göre sınıflandırmadır; tanı değildir. |
| Destek | A specialist check-in is recommended. | Bir uzmanla görüşülmesi önerilir. |
| Destek | Why this level? | Bu seviye neden verildi? |
| Destek | Review alert for mission health team | Görev sağlık ekibine gidecek bildirimi incele |
| Destek | Expected reply delay: about 12 minutes | Tahmini yanıt gecikmesi: yaklaşık 12 dakika |
| Destek | Send / Edit / Cancel | Gönder / Düzenle / Vazgeç |
| Kabus | Possible sleep arousal detected | Olası gece uyanması algılandı |
| Kabus | You are awake. Are you safe right now? | Uyandın. Şu an güvende misin? |
| Kabus | Check physical symptoms | Fiziksel belirtilerini kontrol et |
| Kabus | Return to sleep / Stay awake and contact crew | Uykuya dön / Uyanık kal ve mürettebatla iletişime geç |
| Kabus | This is a simulated event, not a diagnosis. | Bu simüle edilmiş bir olaydır; tanı değildir. |
| Destek | Talk to a crewmate / Contact Earth / Open Earth Window | Mürettebattan biriyle konuş / Dünya ekibine ulaş / Dünya Penceresi'ni aç |
| Rozet | CLEAR / CHECK | NORMAL / KONTROL ET |
| Vücut | FRONT VIEW / BACK VIEW | ÖN / ARKA |
| Vücut | BRAIN · Focus · Sleep · Mood | BEYİN · Odak · Uyku · Ruh hali |
| Vücut | EYES / VISION · Visual health · Radiation exposure | GÖZLER · Görme sağlığı · Radyasyon maruziyeti |
| Vücut | LUNGS · Breathing · Oxygen | AKCİĞERLER · Solunum · Oksijen |
| Vücut | HEART · Circulation · Heart rhythm | KALP · Dolaşım · Kalp ritmi |
| Vücut | DIGESTION · Nutrition · Hydration | SİNDİRİM · Beslenme · Sıvı dengesi |
| Vücut | MUSCLES / BONES · Strength · Microgravity effects | KAS / KEMİK · Güç · Mikro yerçekimi etkisi |
| Vücut | SKIN · Temperature · Hydration | DERİ · Isı · Nem |
| Banner | SMALL STEPS, HEALTHY CREW, BIGGER HORIZONS | KÜÇÜK ADIMLAR · SAĞLIKLI MÜRETTEBAT · BÜYÜK UFUKLAR |
| Odak | TODAY'S FOCUS | BUGÜNÜN ODAĞI |
| Odak | Stay hydrated / Complete exercise session / Log your meals | Su içmeyi unutma / Egzersiz seansını tamamla / Öğünlerini kaydet |
| Alıntı | "A healthier you helps us all get closer to Mars." | "Sen iyiysen, hepimiz Mars'a bir adım daha yakınız." |
| Hızlı işlem | QUICK ACTIONS | HIZLI İŞLEMLER |
| Hızlı işlem | Run Symptom Check | Belirti Kontrolü |
| Hızlı işlem | Talk to AI Doctor | YZ Doktorla Konuş |
| Hızlı işlem | Breathing Exercise | Nefes Egzersizi |
| Hızlı işlem | Open Earth Window | Dünya Penceresi |

### 4.3 Psikolojik destek ekranı (korku ve kaygı)

- Ton sakin, saygılı ve yargısızdır. “Sakin ol”, “tehlike yok” veya “bu sadece kaygı” gibi küçümseyen/kanıtsız ifadeler kullanılmaz.
- İlk adım kısa bir kabul cümlesi ve tek soru olur; uzun açıklama, puanlama ya da zorunlu günlük kaydı yoktur.
- Kullanıcı iki egzersizden birini seçebilir, atlayabilir veya istediği an durdurabilir. Nefes egzersizinde nefesi tutma zorunluluğu ve hızlı/derin nefes talimatı yoktur; rahatsızlık olursa normal nefese dönmesi söylenir.
- Egzersiz bittiğinde duygu durumu tekrar sorulur ve insan desteği seçenekleri görünür kalır. Aile mesajı hiçbir zaman egzersiz tamamlama ya da başarı koşuluna bağlanmaz.
- Güvenlik işareti veya fiziksel belirti bildirildiğinde rahatlama egzersizi tek öneri olarak gösterilmez; ilgili tıbbi/güvenlik protokolüne ve ekip desteğine geçiş sağlanır.
- Psikolojik destek alanı özel ve düşük uyaranlı görünür: sade zemin, geniş satır aralığı, tek bir ana eylem. Duygu durumu renk ile tek başına kodlanmaz.

### 4.4 Önem seviyesi ve yer ekibine bildirim

- Astronot durum türünü (psikolojik/fiziksel), olayını ve belirtilerini girer. **1–10 seviyesini arayüzde sistem verir**; astronot puan seçmez.
- Seviye yanında “Sistem önem seviyesi: 5/10”, sınıflandırma gerekçesi, dayanak belirtiler ve önerilen eylem görünür. “Tanı değildir” açıklaması yakınında yer alır.
- Seviye 5 örneği: “Astronot [ad] psikolojik durum bildirimi yaptı: yoğun kaygı ve uyuyamama. Sistem önem seviyesini 5/10 olarak sınıflandırdı. Gerekçe: belirtiler sürüyor; acil güvenlik işareti bildirilmedi. Bir uzmanla iletişim kurulması önerilir.”
- Yer ekibine gidecek mesajda durum türü, sistem seviyesi, gerekçe, zaman ve gerekli belirtiler gösterilir; alıcı “Görev sağlık ekibi / yer ekibi” olarak adlandırılır. Tahmini Dünya yanıt gecikmesi belirtilir.
- Acil fiziksel kaza veya güvenlik işareti genel puandan bağımsız biçimde ilgili acil görev protokolüne geçer. Mars gecikmesinde mürettebat/araç içi destek Dünya yanıtından önce görünür.
- Seviye renkleri tek başına anlam taşımaz; metin, ikon ve öncelik etiketi de gösterilir. Yanıp sönme ve sesli/titreşimli alarm varsayılan değildir; acil araç uyarıları görev protokolüne bağlıdır.
- Seviyelerin hangi eylemi tetiklediği operasyonel eşik değildir; klinik ve uçuş operasyonu uzmanları doğrulamadan gerçek görevde kullanılmaz.

### 4.5 Kabus sonrası uyandırma desteği

- Uyku sinyalleri tek başına “kabus” diye etiketlenmez; **“olası gece uyanması”** olarak gösterilir. REM olasılığı, nabız/HRV, solunum ve hareket değişimi yalnızca birleşik bir ipucudur.
- Demo akışında “Olayı simüle et” düğmesi kullanılır. Gerçek donanım entegrasyonu yoktur; uyandırma bildirimi ve yanıt adımları simüle edilir.
- Önceden belirlenmiş tercih olarak düşük ses, yumuşak ışık veya hafif titreşim seçenekleri gösterilir. Tasarımda ani flaş, yüksek ses ve alarm efekti kullanılmaz; kullanıcının durdurma/uyanık kalma seçeneği vardır.
- Uyanış ekranı tek tek kısa adımlarla ilerler: yönelim → güvenlik/ekip kontrolü → fiziksel belirti kontrolü → sakinleşme veya ekibi çağırma. Acil fiziksel belirti varsa rahatlama egzersizi araya girmez.
- Tekrarlayan kabuslar için nazik uzman görüşmesi önerisi sunulur; kabus kaydı isteğe bağlıdır. İçerik terapi/tedavi yerine geçmez.
- Bilgi metni; kabusların çoğunlukla REM ile ilişkili olduğunu ama yalnız REM'de olmadığını, REM atonisinin solunum ve göz kaslarını kapsamadığını ve otonom/kortizol yanıtının kişiden kişiye değiştiğini açıklar. Herkesin tansiyonu yükselir veya sıçrayarak uyanır gibi kesin genellemeler kullanılmaz.
- Kaynak notu: [AASM kabus bozukluğu konum belgesi](https://pmc.ncbi.nlm.nih.gov/articles/PMC5991964/), [otonom ölçüm çalışması](https://pubmed.ncbi.nlm.nih.gov/30927477/), [REM atonisi derlemesi](https://pmc.ncbi.nlm.nih.gov/articles/PMC11891935/).

---

## 5. Animasyon dili

### 5.1 İlkeler

1. **Cihaz gibi hareket et, oyun gibi değil.** Hareketler kısa, kararlı, mekanik. Zıplama, sekme, parlama yok.
2. **Her animasyon veriye bağlı.** Kalp, gerçek nabız hızında atar. Akciğer, gerçek solunum hızında genişler. Sayaç gerçek gecikmeyi sayar.
3. **Dikkat sadece değişime.** Normal durumda ekran sakin. Bir şey değişince yalnızca o şey hareket eder.
4. **Bağlantıyı göster.** Vücut bölgesi ile ilgili kart her zaman birlikte tepki verir.

### 5.2 Süre ve hızlanma eğrileri

| Token | Değer | Kullanım |
|---|---|---|
| `t-tick` | 120 ms | Rakam değişimi, rozet damgası |
| `t-fast` | 200 ms | Hover, buton |
| `t-base` | 400 ms | Kart girişi, panel açılışı |
| `t-draw` | 1200 ms | Vücut çizimi, grafik çizgisi |
| `ease-out` | `cubic-bezier(0.2, 0.8, 0.2, 1)` | Girişler |
| `ease-in-out` | `cubic-bezier(0.65, 0, 0.35, 1)` | Durum geçişleri |
| `ease-step` | `steps(6)` | Mekanik sayaç hissi |

---

## 6. Açılış koreografisi (ilk yükleme, toplam ~2,4 sn)

| Zaman | Ne olur |
|---|---|
| 0 ms | Kâğıt zemin ve ince ızgara çizgileri belirir (opaklık 0→1) |
| 100 ms | Üst bar yukarıdan iner; **TWENTY** yazılır, turuncu **/** soldan kayarak gelir |
| 200 ms | Menü öğeleri sırayla soldan girer (40 ms arayla); aktif öğenin turuncu çizgisi yukarıdan aşağı uzar |
| 300 ms | Görev izinde nokta DÜNYA'dan başlayıp mevcut konumuna (SEYİR) kayar, geçtiği noktalar dolar |
| 300 ms | Gecikme sayacı rakamları mekanik sayaç gibi dönerek 14:32'ye oturur |
| 400 ms | **SAĞLIK ÖZETİ** başlığı alttan maske ile açılır; "01" sonra belirir |
| 500 ms | Vücut figürünün dış hatları **çizilerek** belirir (stroke çizim, 1,2 sn) |
| 900 ms | Organlar sırayla dolar: beyin → akciğer → kalp → sindirim → kaslar (80 ms arayla) |
| 1100 ms | Etiket çizgileri organdan etikete doğru uzar, etiket metni daktilo gibi yazılır |
| 600 ms | Sol kartlar soldan, sağ kartlar sağdan, 60 ms arayla girer |
| 800 ms | Büyük rakamlar 0'dan değerine sayar (600 ms) |
| 900 ms | Grafik çizgileri soldan sağa çizilir |
| 1400 ms | Rozetler "damga" gibi basılır (ölçek 1,15→1, hafif 2° dönüş düzelir) |
| 1600 ms | Alt şerit aşağıdan gelir; Bugünün Odağı maddeleri sırayla belirir |
| 2000 ms | KONTROL ET durumundaki öğeler (şu an: nabız ve kalp) dikkat çekmeye başlar |

---

## 7. Canlı (sürekli) animasyonlar

| Öğe | Animasyon | Veriye bağlantı |
|---|---|---|
| **Kalp organı** | Atım: ölçek 1→1,06→1, hafif turuncu hale | Periyot = 60 / nabız. 98 atım/dk = 612 ms |
| **Kalp ikonu** (kart) | Kalp organıyla **aynı ritimde** atar | Aynı |
| **Akciğerler** | Yavaş genişleme/daralma | Periyot = 60 / solunum hızı. 16/dk = 3,75 sn |
| **Grafikler** | Her 2 saniyede yeni nokta sağdan girer, çizgi sola kayar | Simülasyon akışı |
| **Büyük rakamlar** | Değer değişince sadece değişen hane yukarı kayarak değişir | Simülasyon akışı |
| **Gecikme sayacı** | Saniyeler gerçek zamanlı akar | Bkz. 7.1 |
| **Görev izi** | Mevcut konum noktası yavaşça nabız gibi büyüyüp küçülür | Görev günü |
| **Beyin** | Çok hafif, yavaş ton değişimi (6 sn) | Sadece ruh hali/uyku düşükse belirginleşir |
| **Destek ekranı** | Yumuşak opaklık geçişi (300 ms), sabit içerik | Duygusal destek sırasında nabız/panik efekti, titreşim ve geri sayım yok |

### 7.1 Gecikme sayacı nasıl çalışmalı (bilimsel doğruluk)

- Gösterilen değer **tek yön ışık süresi** = araç ile Dünya arasındaki uzaklık / ışık hızı (`app/src/sim/orbit.ts`, basit Hohmann transfer modeli).
- Değer görev boyunca **çok yavaş artar** (dakikalar içinde neredeyse sabit). Bu yüzden saniyeler "akmaz"; canlılık hissini görev izindeki Dünya'dan araca giden sinyal noktası ve "BAĞLANTI AKTİF" ışığı verir.
- Yer ekibine vaka gönderildiğinde sayacın altında ikinci bir **geri sayım** açılır: "CEVAP BEKLENİYOR · 23:46" (gidiş + dönüş). *(Sonraki adım)*
- ✅ Kontrol edildi: Tasarımdaki "GÜN 94 / 14:32" fiziksel olarak tutarsızdı (94. günde araç Dünya'ya ~0,2 AU uzakta, gecikme ~2 dk). Uygulamada **GÜN 240 → 11:53** kullanılıyor (`app/src/config.ts`). Hohmann transferi ~259 gün sürer ve varışta gecikme ~13 dk'ya çıkar. "20 dakika" sloganı Mars yüzeyindeki en kötü duruma (~22 dk) dayanır.

---

## 8. Etkileşim animasyonları

| Etkileşim | Ne olur |
|---|---|
| **Organ üzerine gelme** | Organ dolgusu koyulaşır, etiket çizgisi kalınlaşır, **ilgili kart(lar) da vurgulanır** (kenar `ink`, hafif yükselme). Kalp → Nabız + Tansiyon; Akciğer → Oksijen; Beyin → Uyku + Ruh hali; Deri → Vücut ısısı; Sindirim → Kan şekeri |
| **Kart üzerine gelme** | Tersi: ilgili organ vurgulanır |
| **Organa tıklama** | Sağdan detay paneli kayar: "Bu organ ne yapar", "Uzayda neden etkilenir", "Nasıl ölçülür", son ölçümler, öneriler |
| **ÖN / ARKA** | Figür yatayda daralıp genişleyerek döner (3D dönüş yok, düz el kitabı hissi); arka görünüm: omurga, kaslar, kemikler |
| **Rozet durum değişimi** | Eski rozet silinir, yenisi damga gibi basılır; organ rengi 400 ms içinde yeni duruma geçer |
| **Menü geçişi** | Turuncu aktif çizgi yeni öğeye kayar; içerik sayfa çevirir gibi yatayda kayar |
| **Hızlı işlem butonu** | Basıldığında ikon kısa bir mekanik "tık" hareketi yapar |
| **Sakinleşme akışı** | Egzersiz adımları yavaşça değişir; ilerleme göstergesi sade ve durdurulabilir | Kullanıcı temposu belirler; nefes ritmini zorla eşleştiren animasyon yok |
| **Bugünün Odağı maddesi** | Tıklanınca kutu çizilerek işaretlenir, madde üstü çizilir, sanal seraya +1 |

---

## 9. Alarm ve gece modu geçişi (Görev Modu S1 için)

Senaryo "Gece 03:12" başladığında:
1. Ekran 800 ms içinde **gece moduna** geçer: zemin `night`, metinler sıcak kırık beyaz, turuncu kısılmış.
2. Nabız rakamı hızla yükselir (98 → 182), kalp atım animasyonu hızlanır.
3. Kalp organı ve nabız kartı **MÜDAHALE** durumuna geçer: rozet damgası, kart kenarı kalınlaşır.
4. Ekranın geri kalanı %40 soluklaşır; tek bir büyük eylem butonu belirir: **"BELİRTİ KONTROLÜNÜ BAŞLAT"**.
5. Ses: kısa, yumuşak tek bir uyarı tonu (alarm sireni değil).

---

## 9.1 Görev Modu: birinci şahıs EVA ve olay karşılaştırması

- Ana görünüm birinci şahıs kask kamerasıdır; dış modül, el tutamakları, güneş paneli ve Dünya perspektifle çizilir. W/S ileri-geri, A/D yana, ↑/↓ fiziksel dikey hareket; fare/dokunmatik sürükleme bakış yönünü değiştirir.
- HUD; EVA durumu, oksijen yüzdesi, bağ uzunluğu ve görev adımını gösterir. Bunlar demoda sentetik değerlerdir.
- Astronot anten desteğinin tanımlı fiziksel çarpışma sınırına girince hareket durur ve aynı anda iki animasyon başlar: **Sistemimizle** olay algılama/bildirim, **Sistemsiz** olası gecikmiş fark etme akışı. İki kolonlu sonuçta belirti kontrolü, açıklanabilir demo önem seviyesi ve yer ekibi paketi gösterilir.
- Sistemsiz tarafta kesin zarar/sonuç iddia edilmez; “değerlendirme gecikebilir” gibi olasılık dili kullanılır. Tıbbi öneri ve eşikler uzman doğrulaması gerektirir.
- Karşılaştırmadan olay bildirimi akışına geçilebilir; baş çarpması fiziksel durum olarak ön doldurulur ve astronot belirtileri ayrıca doğrular.

## 10. Erişilebilirlik

- `prefers-reduced-motion` açıksa: çizim ve sayma animasyonları kapanır, kalp atımı yerine sabit vurgu, sadece opaklık geçişleri kalır.
- Durumlar renk + şekil + etiketle verilir (renk körlüğü).
- Metin kontrastı en az 4.5:1 (`ink-mute` küçük metinlerde kontrol edilecek).
- Tüm organlar ve kartlar klavyeyle gezilebilir ve tıklanabilir (Tab + Enter).

---

## 11. Teknik uygulama

### 11.1 Araçlar

| İş | Araç | Neden |
|---|---|---|
| Uygulama | React + Vite + TypeScript | Hızlı, öğrencilerin öğrenmesi kolay |
| Stil | Tailwind CSS 4 (tokenlar `app/src/index.css` içindeki `@theme` bloğunda) | Tasarım tokenları tek yerde |
| Fontlar | `@fontsource` paketleri (internetsiz çalışır) | Demo sırasında bağlantı gerekmez |
| UI animasyonları | Motion (Framer Motion) | Giriş, hover, panel, sayı geçişleri |
| Vücut ve grafikler | Elle yazılmış SVG | Her organ ayrı katman; çizim animasyonu tam kontrol |
| Durum | Zustand | Simülasyon verisi tek yerden akar |
| Metinler | `src/i18n/tr.ts` (+ `en.ts`) | Türkçe varsayılan, İngilizce hazır |

### 11.2 Bileşen ağacı

```
App
├─ TopBar            (Logo, Slogan, MissionTrack, CrewSelect, DelayCounter)
├─ SideNav           (NavItem × 6, MarsWindow)
└─ HealthOverview
   ├─ ScreenTitle
   ├─ VitalCard × 7  (Icon, Title, StatusBadge, BigNumber, Sparkline, RangeNote)
   ├─ BodyMap        (BodyOutline, Organ × 7, Callout × 7, ViewToggle)
   ├─ OrganDrawer
   └─ BottomStrip    (MarsBanner, TodayFocus, Quote, QuickActions)
```

`VitalCard` tek bileşen; 7 kart veriden üretilir. Uyku (çubuk) ve ruh hali (nokta-çizgi) sadece grafik tipini değiştirir.

### 11.3 Vücut figürü stratejisi (en kritik görsel iş)

Tasarımdaki figür ayrıntılı bir resim; animasyon için **katmanlı SVG** lazım. Yol:
1. GPT'den aynı stilde **ayrı katmanlar** istenir: sadece dış hat, sadece beyin, sadece akciğerler, sadece kalp... düz beyaz zeminde, aynı boyut ve konumda.
2. Her katman SVG'ye çevrilir (vektörleştirme), sadeleştirilir.
3. Kodda her organ `<path id="organ-heart">` olarak ayrı tutulur.
4. Yedek plan: Dış hat ve organlar elle sadeleştirilmiş SVG olarak çizilir (daha az detay ama tam kontrol).

### 11.4 GPT'den ayrıca istenecek görseller

- [ ] Vücut figürü katmanları (yukarıdaki gibi), ön ve arka
- [ ] Gecikme paneli için Mars görseli (yalnız, yazısız)
- [ ] Yan menü için pencereden Mars görseli (yalnız, yazısız)
- [ ] Alt şerit için Mars yüzeyi manzarası (yalnız, yazısız)
- [ ] Ekran 2-6 (Triyaj, YZ Doktor, Dünya Penceresi, Görev Modu sonucu, Gece modu)

> Not: Görseller yapay zekâyla üretildi. Space Apps yapay zekâ kullanımının beyan edilmesini isteyebilir; proje sayfasında belirtilecek (PLAN.md bölüm 12'de doğrulanacak).

### 11.5 Klasör yapısı

```
nextgenlab-spaceapps-2026/
├─ PLAN.md
├─ TASARIM.md
├─ tasarim/            # GPT çıktıları (referans)
└─ app/
   ├─ public/assets/   # Mars görselleri, SVG'ler
   └─ src/
      ├─ components/   # TopBar, SideNav, VitalCard, BodyMap...
      ├─ screens/      # HealthOverview, Triage, AiDoctor, EarthWindow, MissionMode
      ├─ sim/          # Simüle veri akışı, senaryo JSON'ları
      ├─ triage/       # Kural motoru
      ├─ i18n/         # tr.ts, en.ts
      └─ theme/        # tokenlar, animasyon süreleri
```

---

## 12. Tasarım üzerine notlar ve düzeltmeler

| Konu | Not |
|---|---|
| Nabız 98, rozet KONTROL ET | ✅ Doğru: genel aralıkta normal ama astronotun **kişisel aralığının** (60-90) üstünde. "Kişisel normal" fikrini çok iyi gösteriyor, sunumda vurgulanmalı |
| "Senin aralığın" ifadeleri | ✅ Kişiselleştirme hissi güçlü |
| Uyarı (kırmızı) durumu | ❌ Tasarımda yok. `alert` rengi ve MÜDAHALE rozeti eklenecek (Ekran 6 gece modunda görülecek) |
| Solunum hızı | ➕ Kart yok. Panik/hiperventilasyon ayrımı için önemli; triyaj ekranında veya kalp detay panelinde gösterilecek |
| MISSION TOOLS | 🔁 "GÖREV MODU" olarak değiştirildi (PLAN.md'deki senaryo modu) |
| Gün 94 / 14:32 | ⚠️ Bkz. 7.1 |
| NASA logosu | ✅ Tasarımda yok, öyle kalmalı |

---

## 13. Uygulama sırası (ilk somut adımlar)

1. [x] `app/` iskeleti: Vite + React + TS + Tailwind + Motion, tokenlar, fontlar, `lang="tr"`, i18n sözlüğü (TR + EN)
2. [x] Yerleşim: TopBar, SideNav, kartlar, alt şerit (1600×1000 sahne, pencereye ölçeklenir)
3. [x] `VitalCard` + `Sparkline` + sayı animasyonları, 2 saniyede bir güncellenen simüle veri
4. [x] `BodyMap` (sade SVG), organ ↔ kart bağlantısı, kalp nabız ritminde, akciğer solunum ritminde, organ detay paneli, ÖN/ARKA
5. [x] Açılış koreografisi
6. [x] Gecikme sayacı + görev izi (yörünge modeli)
7. [ ] Gece modu + alarm geçişi (S1'in başlangıcı)
8. [ ] GPT'den katmanlı vücut figürü gelince `bodyShapes.ts` güncellenecek
9. [ ] Mars görselleri şimdilik vektör çizim (`MarsArt.tsx`); istenirse yazısız GPT görselleriyle değişecek

> ⚠️ Kod yazmaya başlamadan önce PLAN.md bölüm 12'deki "hackathon öncesi kod yazma izni" kuralı doğrulanmalı.
