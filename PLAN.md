# NextGenLAB · NASA Space Apps Challenge 2026

**Challenge:** Create Health Monitoring Software for Astronauts on Space Missions (Intermediate · Human Exploration, Software, Space Exploration)
**Ekip:** Babil, Utku, Tuna · Mentorlar: Furkan Hoca, Okan Hoca
**Hackathon:** 14-15 Kasım 2026 (bugün 31 Ekim, **2 hafta var**)
**Belge sürümü:** v0.2 (31.10.2026), ekip toplantısında güncellenecek
**Tasarım:** Ekran 1 (Sağlık Özeti) hazır → `TASARIM.md` ve `tasarim/01-saglik-ozeti.webp`. Arayüz Türkçe ve animasyonlu olacak.

---

## 1. Tek cümlede fikrimiz

> **Mars yolunda Dünya'daki doktor 20 dakika uzaktayken, astronotun yanındaki uçuş doktoru.**

Diğer takımların çoğu "nabız/tansiyon ölçen, renkli grafik gösteren bir panel" yapacak. Biz bir **karar verme arkadaşı** yapıyoruz:

1. **Ayırıcı tanı yapan triyaj:** "Panik atak mı, gerçek bir rahatsızlık mı?" sorusunu tek değere değil, **desene, zaman seyrine ve ortama** bakarak cevaplar. Önce tehlikeliyi dışlar.
2. **Gecikmeyi bilen sistem:** Dünya ile iletişim gecikmesini (Ay ~1,3 sn, Mars 4-24 dk tek yön) hesaba katar: "Cevap 18 dk sonra gelecek, şu an yapman gereken şu."
3. **Mürettebat düzeyinde düşünür:** Üç kişide aynı anda baş ağrısı varsa sorun kişide değil, **kabin havasında** (CO2) olabilir.
4. **İnsanı da görür:** Psikolojik durum, uyku, yorgunluk ve Dünya özlemi aynı sistemin parçası.
5. **Jüri ürünü yaşar:** "Görev Modu" ile jüri astronotun gözünden senaryoyu oynar; sistem olmasa ne olurdu, sistemle ne oldu, yan yana görür.

---

## 2. Challenge ne istiyor, biz nasıl karşılıyoruz

| Challenge metni | Bizim karşılığımız |
|---|---|
| Astronotun kendi sağlığını değerlendirdiği görsel araç | Animasyonlu, tıklanabilir vücut haritası + renk kodlu göstergeler |
| Tedavi ve iyileştirme rehberliği | Protokol kartları (adım adım), AI doktor asistanı |
| Her türlü girdi (klavye, fare, kamera) | Manuel ölçüm girişi, simüle sensör akışı, kamera ile reaksiyon/göz testi (opsiyonel) |
| HRP'nin 5 tehlikesi (radyasyon, izolasyon, uzaklık, yerçekimi, kapalı ortam) | Her uyarı hangi tehlikeyle ilişkili olduğunu etiketler; "uzaklık" tehlikesini gecikme sayacıyla somut gösteririz |
| "Ne zaman mürettebat / yer doktoru devreye girmeli?" | 3 seviyeli eskalasyon: kendin yap / mürettebata haber ver / yer ekibine acil paket |
| "Kritik durumu önlemek için ne yapılabilir?" | Trend uyarıları (kötüleşmeden önce), günlük rutin görevleri |
| Gamification, zaman içinde grafikler, renk kodlama | Sera/Dünya Penceresi ödül sistemi, trend grafikleri, yeşil/sarı/kırmızı |
| "Astronotların her şeyi bildiğini varsayma" | Her ölçüm için "nasıl ölçülür, neden önemli, uzayda neden değişir" kartı |
| Uzay aracı, istasyon, gezgin, üs gibi ortamlar | Senaryolar farklı ortamlarda geçer (Mars transit, Ay üssü, istasyon) |

---

## 3. Ekip fikirlerinin değerlendirmesi (düzeltmelerle)

### 3.1 Panik atak mı, gerçek rahatsızlık mı? ✅ Çok güçlü fikir, ama tıbbi temel düzeltilmeli

**Düzeltme:** "Panik atakta büyük ve küçük tansiyon birlikte yükselir" kısmen doğru ama **ayırt edici değil**. Korku, ağrı, efor, düşük kan şekeri, kalp ritim bozukluğu da aynı şeyi yapar. "Panik atak nabzı" diye tıbbi bir terim yok; panikte görülen nabız normal bir **sinüs taşikardisidir** (kalbin normal ritmiyle hızlanması).

Panik atak tıpta bir **dışlama tanısıdır**: önce tehlikeli nedenler elenir. Bizim yazılımımızın farkı tam olarak bu sırayı otomatik ve açıklanabilir şekilde uygulamak olmalı.

**Ayırt edici ipuçları (triyaj motorunun kuralları):**

| Bulgu | Panik atakta | Başka bir şeyi düşündürür |
|---|---|---|
| Nabız başlangıcı | Kademeli hızlanır, genelde 100-140 | **Bir anda başlayıp bir anda biten**, 150+ sabit nabız → SVT (ritim bozukluğu) |
| Ritim | Düzenli | **Düzensiz** → atriyal fibrilasyon |
| Oksijen (SpO2) | Normal, hatta yüksek (hızlı nefes) | **Düşük** → panik değil: hipoksi, akciğer sorunu, kabin havası |
| Kan şekeri | Normal | **< 70 mg/dL** → hipoglisemi panik taklidi yapar (terleme, titreme, çarpıntı) |
| Kabin CO2 | Normal | **Yüksek** → hiperkapni: baş ağrısı, nefes darlığı, huzursuzluk. ISS'te gerçek ve bilinen bir sorun |
| Mürettebat | Sadece bir kişi | **Birden fazla kişide aynı anda** → çevresel neden |
| EKG | Sinüs taşikardisi | ST değişikliği → kalp kası beslenme sorunu (iskemi) |
| Zaman seyri | ~10 dk'da zirve, 20-30 dk'da geçer | Sürüyor veya kötüleşiyor → eskalasyon |
| Eşlik eden | El/dudak karıncalanması, hızlı nefes, ölüm korkusu | Bayılma, göğüste baskı, tek taraflı güçsüzlük → acil |

**Sistem akışı:**
1. Kırmızı bayrak var mı? (SpO2 düşük, düzensiz ritim, 150+ sabit nabız, bilinç değişikliği, göğüs ağrısı + EKG değişikliği) → Varsa acil protokol + yer ekibine paket.
2. Taklitçileri ele: kan şekeri ölç, kabin CO2'ye bak, mürettebata sor.
3. Hepsi temizse: "Panik atak olasılığı yüksek" + **neden böyle düşündüğünü gösterir** (açıklanabilirlik jüri için çok değerli) + rehberli nefes egzersizi.
4. 10. ve 20. dakikada tekrar ölçüm ister. Geçmiyorsa eskale eder.

> Sistem **tanı koymaz**, "olasılık + gerekçe + yapılacaklar" verir. Sunumda bunu açıkça söylemek "Validity" puanını artırır.

### 3.2 Kafa darbesi → "uyuma" telkini ⚠️ Fikir doğru, tavsiye güncel değil

"Kafa darbesinden sonra uyutma" eski bir halk bilgisi. Güncel yaklaşım:
- **Hemen kırmızı bayrak kontrolü:** bilinç kaybı, tekrarlayan kusma, giderek artan baş ağrısı, göz bebeklerinde eşitsizlik, konfüzyon, nöbet, tek taraflı güçsüzlük → acil.
- Kırmızı bayrak yoksa **uyuyabilir**, ama ilk gece bir mürettebat arkadaşı **2-4 saatte bir uyandırıp kontrol eder**. Asistan bu kontrolleri alarm olarak kurar, mürettebat arkadaşına kontrol listesini verir.
- Basit nörolojik testler ekranda yapılır: reaksiyon zamanı testi, kısa hafıza soruları, kamera ile göz takibi (opsiyonel).
- Olay otomatik olarak yer ekibine raporlanır.

Uzay bağlamı: mikro yerçekiminde kapak/ambar çarpmaları gerçekten yaşanan bir yaralanma türü. Senaryoya çok uygun.

### 3.3 AI doktor asistanı ✅ Güçlü, iki katmanlı kurulmalı

- **Protokol motoru (sabit kurallar):** Tıbbi kararlar, eşik değerleri, adım adım talimatlar sabit ve doğrulanmış kurallardan gelir. AI ilaç dozu "uydurmaz".
- **Sohbet katmanı (LLM):** Astronotla doğal dilde konuşur, sakinleştirir, açıklar, protokolü insan diliyle anlatır, sesli konuşabilir.
- **Gecikme farkındalığı:** "Dr. Yılmaz'a paket gönderdim, cevap ~18 dk sonra gelir. Bu arada şunları yapalım."
- **Çevrimdışı mimari:** Mars'ta internet yok. Demo için bulut API kullanabiliriz ama mimari şemada "araç içi model" olarak gösteririz.

### 3.4 Mental ve psikolojik bölüm ✅ Az takımın derinlemesine yapacağı alan

Eklenmesi önerilenler:
- **Günlük 30 saniyelik ruh hali kontrolü** (emoji/slider + tek kelime).
- **Korku ve kaygı için isteğe bağlı sakinleşme akışı:** Astronotun duygusunu yargılamadan adlandırmasına, bulunduğu ana yönelmesine ve bir sonraki küçük adımı seçmesine yardımcı olur. Kullanıcı isterse 60-90 saniyelik yavaş nefes veya 5-4-3-2-1 duyusal farkındalık egzersizi sunulur; atlama ve durdurma her an mümkündür.
- **Korku tetikleyicisini anlama:** “Şu an seni en çok ne zorluyor?” seçenekleri (sağlık belirtisi, yalnızlık/Dünya özlemi, görev belirsizliği, uyku/yorgunluk, başka) ve isteğe bağlı kısa not. Serbest metin zorunlu değildir.
- **Kontrol hissi ve bağlantı:** Egzersiz sonrası “Şimdi nasıl hissediyorsun?” kontrolü, seçilebilir sonraki adım (dinlenme, güvendiği ekip arkadaşıyla konuşma, Dünya Penceresi, yer ekibine mesaj). Sakinleşme tıbbi belirti taramasının yerine geçmez.
- **Bilişsel yorgunluk testi:** 3 dakikalık reaksiyon zamanı testi (NASA'nın kullandığı PVT, Psychomotor Vigilance Test mantığıyla). Tarayıcıda kolayca yapılır, gerçekçi ve ölçülebilir.
- **Uyku takibi:** ISS'te astronotlar ortalama ~6 saat uyuyor, günde 16 gün doğumu görüyorlar. Uyku borcu göstergesi.
- **"Üçüncü çeyrek etkisi":** İzolasyon çalışmalarında görevin 3. çeyreğinde moralin düştüğü gözlemlenmiş. Sistem görev takvimine göre bu dönemi önceden işaretler.
- **Gizlilik önce:** Astronotlar görevden alınma korkusuyla belirtilerini az raporlayabiliyor. Bizim çözüm: psikolojik veriler önce astronotta kalır, neyin paylaşılacağını astronot seçer (kritik güvenlik eşikleri hariç). Bu, jüriye "gerçek sorunu anlamış takım" mesajı verir.

**Güvenli destek ilkeleri:** Sistem korkuyu “zayıflık” veya tanı olarak etiketlemez, “her şey yolunda” diye garanti vermez ve duyguyu bastırmaya çalışmaz. Önce empati kurar, sonra kısa ve uygulanabilir seçenekler sunar. Göğüs ağrısı, bayılma, nefes alamama, kendine zarar verme düşüncesi veya kişinin güvende olmadığını belirtmesi gibi acil işaretlerde egzersiz akışını bırakıp güvenlik protokolü ve mürettebat/yer ekibi desteğine yönlendirir. Bu özellik ruh sağlığı tedavisi ya da tanı aracı değildir.

### 3.4.2 Kabus sonrası nazik uyandırma ve destek

- Bileklik/uyku sensörlerinden nabız, nabız değişkenliği, solunum ve hareket örüntülerindeki değişim izlenebilir; REM evresi olasılığıyla birlikte belirgin otonom uyarılma **olası gece uyanması/kabus olayı** olarak işaretlenir. Bu sinyaller kabusu tek başına teşhis edemez; yanlış alarm ve kişisel bazal farklılıkları hesaba katılmalıdır.
- Olay sürerse, astronotun önceden seçtiği düşük sesli uyarı, yumuşak kabin ışığı veya giyilebilir hafif titreşimle kademeli uyandırma denenir. Tercih ve uyandırma eşiği kişi tarafından görev öncesinde belirlenir; demo fiziksel cihazı kontrol etmez.
- Uyanınca ekranda sakin yönelim: “Uyandın. Görev aracındasın. Şu an güvendesin mi?” Ardından konum/zaman bilgisi, normal nefese dönme seçeneği, kısa duyusal farkındalık egzersizi ve fiziksel belirti kontrolü sunulur. “Güvendesin” ifadesi yalnızca çevre durumu doğrulanabiliyorsa kullanılır; aksi halde ekip kontrolü önerilir.
- Göğüs ağrısı, ciddi nefes darlığı, bayılma, olağandışı ritim veya yaralanma varsa bunu kabus varsayma; fiziksel triyaj/acil görev protokolüne geçir. Her gece veriye bakarak kişiyi otomatik uyandırma; doğrulama, eşik ve tekrar politikası uzman incelemesi gerektirir.
- Sık tekrarlayan veya gündüz işlevini bozan kabuslar için sağlık/ruh sağlığı uzmanı görüşmesi önerilir. Kabus içeriğini kaydetmek isteğe bağlıdır. Rescripting/Imagery Rehearsal gibi tedaviler uygulama tarafından kendiliğinden başlatılmaz; klinik destekle değerlendirilir.
- Demo göstergeleri sentetik veridir; algılama ve uyanma adımları ekranda simüle edilir.

**Bilgi doğruluğu notu:** Kabuslar çoğunlukla REM uykusuyla ilişkilidir, ancak kabus benzeri uyanmalar yalnız REM'de görülmez. REM'de iskelet kaslarında normal atoni bulunur; solunum ve göz hareketi kasları bundan istisnadır. Kabus sırasında otonom uyarılma artabilir, fakat her kişide tansiyon/kalp hızı artışı veya kortizol/adrenalin “patlaması” olacağı söylenemez; bir küçük fizyolojik çalışmada kabus anında otonom göstergeler artarken kortizol için anlamlı fark bulunmadı. Göz hareketi kabusun içeriğini takip ettiğini kanıtlamaz; sıçrayarak uyanma da zorunlu tipik belirti değildir. [AASM kabus bozukluğu konum belgesi](https://pmc.ncbi.nlm.nih.gov/articles/PMC5991964/), [otonom ölçüm çalışması](https://pubmed.ncbi.nlm.nih.gov/30927477/), [REM atonisi derlemesi](https://pmc.ncbi.nlm.nih.gov/articles/PMC11891935/). AASM yetişkinlerde tekrarlayan kabuslar için Imagery Rehearsal Therapy'yi klinik tedavi seçeneği olarak destekler; demo bu tedaviyi uygulamaz.

### 3.4.1 Sistem tarafından verilen önem seviyesi (1–10)

Astronot **durum türünü** (psikolojik veya fiziksel) ve yaşadığı belirtiyi/kazayı bildirir. Sistemin açıklanabilir kural motoru, girilen belirtiler ve görev bağlamına göre 1–10 önem seviyesi verir; astronottan puan seçmesi istenmez. Her sonuçta puanın yanında hangi bildirimlerin/kuralların bu seviyeyi oluşturduğu ve önerilen sonraki adım gösterilir. Serbest metin varsa puanı tek başına belirlemez.

| Seviye | Demo için önerilen anlam | Önerilen sistem davranışı |
|---|---|---|
| 1–2 | Hafif; acil işaret bildirilmedi | Kısa öz bakım/gözlem önerisi ve yeniden kontrol zamanı |
| 3–4 | Takip gerektiren belirti veya kaygı | Mürettebat içi destek ve belirli sürede yeniden değerlendirme önerisi |
| 5 | Uzman değerlendirmesi yararlı | Yer ekibine “uzmanla iletişim önerilir” mesajı oluştur; tahmini iletişim gecikmesini göster |
| 6–7 | Hızlı değerlendirme gerekli olabilir | Araç içi sağlık sorumlusuna yönlendir; yer ekibine yüksek öncelikli paket hazırla |
| 8–9 | Ciddi veya hızla kötüleşen durum işaretleri | Acil görev protokolünü ve mürettebat desteğini öne çıkar; yer ekibine acil öncelikli bildirimi sıraya al |
| 10 | Kural setinde tanımlanmış kritik acil işaret | Puanı bekletmeden acil araç içi protokolü başlat ve yer ekibine en yüksek öncelikli bildirimi sıraya al |

Bu seviyeler **demo için taslak sınıflandırmadır; klinik ölçek veya doğrulanmış tıbbi triyaj değildir.** Psikolojik ve fiziksel olaylar ayrı kural kümeleriyle değerlendirilir; aynı puan farklı eylem/protokol önerebilir. Herhangi bir acil fiziksel veya güvenlik işareti, genel puandan bağımsız olarak doğrudan ilgili acil protokole gider. 1–10 seviyesi ve eşikleri uçuş hekimi, ruh sağlığı uzmanı ve görev operasyonları tarafından doğrulanmadan gerçek görevde kullanılmaz. Mars gecikmesinde araç içi destek, Dünya yanıtını beklemeden gösterilir.

**Örnek seviye 5 bildirimi:** “Astronot [ad] psikolojik durum bildirimi yaptı: yoğun kaygı ve uyuyamama. Sistem önem seviyesini **5/10** olarak sınıflandırdı. Gerekçe: belirtiler sürüyor; acil güvenlik işareti bildirilmedi. Bir uzmanla iletişim kurulması önerilir. Tahmini Dünya yanıt gecikmesi: [süre].” Sistem kaygıyı tanı gibi adlandırmaz; puanın hangi girilen bulgulardan üretildiğini açıklar.

**Gizlilik ve alıcı:** Bildirimde durum türü, puan, gerekçe, zaman ve gerekli belirtiler yer alır. Ürün, gerçek NASA entegrasyonu varmış gibi görünmez; prototipte alıcı “Görev sağlık ekibi / yer ekibi” olarak adlandırılır. Rutin ve acil bildirimlerin gönderim/iletim politikası görev sağlık konseptiyle tanımlanmalı, asgari gerekli bilgi paylaşılmalı ve erişim mahrem tutulmalıdır.

### 3.5 Dünya özlemi ✅ Duygusal ve akılda kalıcı, jüri sunumunun kalbi olabilir

**"Dünya Penceresi"** modülü:
- Ailenin göreve çıkmadan kaydettiği video/ses mesajları, belirli günlerde açılır (doğum günü, bayram, görevin 100. günü).
- Memleketin **şu anki** saati ve hava durumu: "İstanbul'da şu an yağmur yağıyor, saat 21:40."
- Doğa sesleri: yağmur, orman, deniz, kuş sesi, ezan/çan gibi kişiselleştirilebilir.
- **Sanal sera / toprak:** Sağlıklı rutinler (egzersiz, uyku, ölçüm) bir bitkiyi büyütür. Gerçek bilgi: ISS'teki Veggie bitki yetiştirme çalışmalarının astronotlara psikolojik faydası raporlandı.

> ⚠️ Tasarım kuralı: **Aile mesajları asla ödül olarak kilitlenmez.** Ödüller sera, ses paketleri, Dünya fotoğrafları gibi şeylerdir. Aile ile bağı oyunlaştırmak etik değil, jüri de bunu fark eder.

---

## 4. Ek önerilerimiz (bizi ayıracak özellikler)

| # | Özellik | Neden fark yaratır |
|---|---|---|
| 1 | **Kişisel normal (baseline):** Değerler sadece genel aralığa göre değil, astronotun kendi normalinden sapmaya göre de değerlendirilir | Sporcu astronotun nabzı 52 olabilir, bu normaldir. Genel panel bunu hata sanar |
| 2 | **Gecikme sayacı:** Ekranda "Dünya'ya mesaj: 14 dk 32 sn" | HRP'nin "Dünya'dan uzaklık" tehlikesini somut gösterir. NASA'nın Dünya'dan bağımsız tıbbi operasyonlar (EIMO) araştırma yönüyle örtüşür |
| 3 | **Mürettebat paneli:** Tüm ekibin durumu tek ekranda, ortak desen tespiti | Kimse bireysel değil grup düzeyinde düşünmeyecek |
| 4 | **Kabin ortamı entegrasyonu:** CO2, sıcaklık, radyasyon dozu sağlık verisiyle birlikte yorumlanır | "Hostile closed environment" tehlikesine doğrudan cevap |
| 5 | **Göz testi (SANS):** Ekranda Amsler ızgarası ve yakın görme testi | Uzun görevlerde görme değişikliği (SANS) NASA'nın güncel ve gerçek bir endişesi; çoğu takım bilmeyecek |
| 6 | **Uzayda vücut ısısı notu:** Uzun görevlerde vücut çekirdek ısısının ~1°C daha yüksek seyrettiği gözlendi | Ateş eşiğini uzaya göre yorumlamak "uzmanlık" izlenimi verir |
| 7 | **Yer ekibi paketi:** Tek tuşla, sıkıştırılmış, okunaklı vaka özeti | Gecikmeli iletişimde zaman kazandırır |
| 8 | **Dünya'ya faydası:** Antarktika istasyonları, gemiler, denizaltılar, kırsal bölgeler, afet bölgeleri | "Impact" kriteri için güçlü; NASA'nın astronot sağlık yazılımları zaten Dünya'da uzaktan hasta takibine dönüştü |

---

## 5. Oyun mu, simülasyon mu? → **Öneri: "Görev Modu" (oynanabilir senaryo, ürünün içinde)**

Ayrı bir oyun yapmıyoruz. Ürünün kendisi, simüle edilmiş sensör verisiyle, **astronotun gözünden oynanabilir senaryolar** halinde çalışıyor.

**Neden:**
- Jürinin zamanı çok kısa. Ayrı bir oyun, ürünü göstermek yerine oyunu anlatmaya zaman harcatır.
- Ayrı oyun "oyuncak" algısı yaratabilir. Görev Modu'nda jüri **gerçek ürünü** kullanıyor, bu da "Validity" puanına yarar.
- Oyunun iyi taraflarını yine de alıyoruz: zaman baskısı, karar anları, sonuç ekranı, hikâye.

**Jüriyi etkileyecek ana hamle: "Sistemsiz / Sistemli" karşılaştırması.** Her senaryonun sonunda ekran ikiye bölünür:
- Sol: Sistem yokken astronot "panik atak geçiriyorum" diye düşünüp uzanıyor, ritim bozukluğu 40 dakika fark edilmiyor.
- Sağ: Sistem 90 saniyede SVT şüphesini yakalıyor, manevrayı öğretiyor, yer ekibine paket gidiyor.

### Senaryo taslakları

| # | Ad | Ortam | Ne gösterir |
|---|---|---|---|
| S1 | **Gece 03:12** | Mars transit, gün 240 (gecikme ~12 dk, bkz. TASARIM.md 7.1) | Ani çarpıntı. Panik mi? Dallanma: (a) gerçek panik → nefes egzersizi, geçer; (b) SVT → protokol + yer ekibi; (c) kabin CO2 yüksek, 2 kişide baş ağrısı → çevre alarmı. **Ana demo senaryosu.** |
| S2 | **Ambar kapağı** | İstasyon | Kafa darbesi, kırmızı bayrak kontrolü, nöro testler, gece kontrol alarmları |
| S3 | **Gün 147** | Mars transit, 3. çeyrek | Uyku, reaksiyon testi ve ruh hali yavaş yavaş kötüleşiyor; sistem erken fark ediyor, Dünya Penceresi devreye giriyor |
| S4 | **Güneş fırtınası** (opsiyonel) | Ay üssü | Radyasyon uyarısı, sığınağa geçiş, doz takibi |

**Görev Modu'nun ana deneyimi:** Astronotun gözünden birinci şahıs 3D EVA/uzay yürüyüşü. Kullanıcı WASD ve ↑/↓ kontrolleriyle üç boyutlu alanda ilerleyip fiziksel anten desteğine çarpar. Çarpışma algılanınca aynı anda iki animasyon başlar: **sistemimizle** erken olay kaydı, belirti kontrolü, açıklanabilir önem seviyesi ve ekip/yer ekibine bildirim paketi; **sistemsiz** otomatik olay kaydı olmayışı, belirtinin geç paylaşılması ve ekip değerlendirmesinin gecikme olasılığı. Sistemsiz akış olası bir senaryo olarak anlatılır, kesin tıbbi sonuç gibi sunulmaz. Simülasyondaki 3D çizim ve veriler temsili/sentetiktir.

---

## 6. Ölçümler ve renk kodları (taslak, bir hekime kontrol ettirilmeli)

Uzayda gerçekten yapılabilen ölçümler: tansiyon aleti, parmak pulse oksimetre, EKG (ör. Kanada Uzay Ajansı'nın Bio-Monitor/Astroskin giysisi), glukometre, termometre, uyku/aktivite takibi, ultrason, ortam sensörleri (CO2, radyasyon).

| Gösterge | 🟢 Normal | 🟡 Dikkat | 🔴 Uyarı |
|---|---|---|---|
| Nabız (istirahat) | 60-100 | 50-59 veya 101-120 | <50 + belirti veya >120 istirahatte |
| Tansiyon (mmHg) | <120 / <80 | 120-139 / 80-89 | ≥140/90 (yüksek) · ≥180/120 veya sistolik <90 (acil) |
| SpO2 | ≥%95 | %92-94 | <%92 |
| Vücut ısısı | 36,1-37,2 °C | 37,3-37,9 °C | ≥38 °C (uzay notu: bazal ısı yüksek seyredebilir) |
| Kan şekeri (açlık) | 70-99 mg/dL | 100-125 | <70 (hipoglisemi) veya ≥126 açlık / ≥200 rastgele |
| Solunum hızı | 12-20 /dk | 21-24 | >24 veya <10 |
| Uyku | 7-9 saat | 5-7 | <5 veya 3 gün üst üste <6 |

Kişisel normal etkinse, astronotun kendi ortalamasından belirgin sapma da sarı/kırmızı tetikler.

---

## 7. Özellik önceliklendirme (2 haftaya sığacak şekilde)

**Mutlaka (demo çekirdeği)**
- [ ] Animasyonlu, tıklanabilir vücut haritası + renk kodları (tasarımımız buraya gelecek)
- [ ] Gösterge kartları + zaman içinde trend grafikleri
- [ ] Ayırıcı triyaj motoru (panik vs 5 taklitçi), gerekçesini gösteren
- [ ] AI doktor asistanı (sohbet + protokol kartları)
- [ ] Korku/kaygı anında empatik destek akışı (isteğe bağlı nefes veya duyusal farkındalık, egzersiz sonrası kontrol ve destek seçeneği)
- [ ] Belirti/olay ve durum türünden sistemin verdiği açıklanabilir 1–10 önem seviyesi; gerekçeli yer ekibi mesajı (uzman doğrulaması gerektirir)
- [ ] Gecikme sayacı + yer ekibi paketi
- [ ] Görev Modu: birinci şahıs 3D EVA/uzay yürüyüşü, baş çarpması olayı ve sistemli/sistemsiz iki paralel sonuç

**Olursa çok iyi**
- [ ] Reaksiyon zamanı (PVT) testi
- [ ] Günlük ruh hali kontrolü + uyku borcu
- [ ] Dünya özlemi, yalnızlık ve görev kaygısı için kısa check-in ve Dünya Penceresi'ne gönüllü geçiş
- [ ] Dünya Penceresi + sanal sera (gamification)
- [ ] Mürettebat paneli
- [ ] S3 senaryosu

**Zaman kalırsa**
- [ ] Amsler ızgarası göz testi (SANS)
- [ ] Kamera ile göz takibi / yüz analizi
- [ ] S4 radyasyon senaryosu
- [ ] Sesli asistan

**Bu sürümde yok:** Gerçek sensör donanımı (simüle veri yeterli ve senaryoya daha uygun).

---

## 8. Teknik yaklaşım (taslak)

- **Web uygulaması:** React + Vite + TypeScript. Jüri link ile açar, kurulum yok.
- **Görsel:** Tailwind CSS, SVG vücut haritası. Animasyon kütüphanesi tasarımımız gelince seçilecek (Framer Motion / GSAP / Rive / Lottie).
- **Grafikler:** Recharts veya D3.
- **Triyaj motoru:** TypeScript'te kural tabanlı, açıklanabilir. Her karar "hangi kural tetiklendi" bilgisini döndürür.
- **Senaryolar:** JSON zaman çizelgeleri (saniye saniye sensör verisi + olaylar). Yeni senaryo eklemek kod değil veri işi, öğrenciler kolayca ekler.
- **AI asistan:** LLM API (Claude), sabit protokol kartlarıyla sınırlandırılmış sistem talimatı. API olmadan da çalışan basit yedek mod.
- **Yayın:** GitHub + Vercel/GitHub Pages.

---

## 9. Ekip ve roller (öneri, ekip karar verecek)

| Alan | Kim | Görev |
|---|---|---|
| Tasarım ve animasyon | ? | Tasarımı uygulamaya aktarma, vücut haritası, geçişler |
| Tıbbi içerik ve triyaj | ? | Kurallar, eşikler, protokol kartları, kaynakça |
| Görev Modu ve hikâye | ? | Senaryo yazımı, JSON verisi, sistemsiz/sistemli ekranı |
| AI asistan | Mentor desteğiyle | Sistem talimatı, protokol entegrasyonu |
| Sunum ve video | Tüm ekip | 30 sn video / slaytlar, proje sayfası |

---

## 10. Zaman çizelgesi

| Tarih | Hedef |
|---|---|
| 31 Ekim - 3 Kasım | Bu planı ekip olarak gözden geçir, isim seç, rolleri dağıt, kuralları doğrula (bölüm 12) |
| 3-7 Kasım | Tasarım teslimi ve animasyon planı; senaryo metinleri; tıbbi kural tablosunun hekim kontrolü |
| 7-13 Kasım | Kurallar izin veriyorsa iskelet + triyaj motoru + S1; izin vermiyorsa tüm içerik, veri ve tasarım hazır |
| 14-15 Kasım | Hackathon: entegrasyon, S2, cila, video, proje sayfası |
| Sonrası | Jüri değerlendirmesi |

---

## 11. Jüri sunumu iskeleti

1. **Kanca (5 sn):** Mars yolunda, gece 03:12. Kalbin çarpıyor. Dünya'daki doktor 20 dakika uzakta. Panik mi, ölümcül mü?
2. **Sorun:** HRP'nin 5 tehlikesi + astronot kendi doktoru olmak zorunda.
3. **Ürün:** Görev Modu'ndan 2-3 net kare (triyaj gerekçesi, gecikme sayacı, vücut haritası).
4. **Fark:** Sistemsiz/sistemli bölünmüş ekran.
5. **İnsan tarafı:** Dünya Penceresi, aile mesajı.
6. **Dünya'ya faydası:** Antarktika, gemiler, kırsal bölgeler.

---

## 12. Doğrulanması gerekenler (resmi siteden kontrol)

Resmi site bu oturumda otomatik okunamadı. Ekip spaceappschallenge.org üzerinden şunları kontrol etmeli:
- [ ] Hackathon öncesi **kod yazmaya** izin var mı, yoksa sadece araştırma/planlama mı? (Plan buna göre ayarlanacak)
- [ ] Teslim şartları: proje sayfası, demo formatı (önceki yıllarda 30 sn video veya 7 slayt + proje linki idi), repo linki
- [ ] Global jüri kriterleri (önceki yıllarda: Impact, Creativity, Validity, Relevance, Presentation)
- [ ] Yapay zekâ araçlarının kullanımı ve beyan zorunluluğu
- [ ] NASA logosu/markası kullanım yasağı (tasarımda NASA logosu kullanmayalım)
- [ ] Reşit olmayan katılımcılar için kurallar, takım büyüklüğü
- [ ] Challenge'ın **Resources** sekmesindeki veri setleri ve NASA HRP kaynakları

---

## 13. Riskler

| Risk | Önlem |
|---|---|
| Tıbbi hata | Kuralları bir hekime kontrol ettir, kaynak göster, "tanı koymaz" uyarısı |
| AI'nın yanlış bilgi vermesi | Kararlar sabit kurallardan, AI sadece anlatır |
| Kapsamın büyümesi | Bölüm 7'deki "Mutlaka" listesi bitmeden diğerine geçme |
| Demo sırasında internet/API sorunu | Görev Modu çevrimdışı çalışsın, AI için yedek mod |
| Tasarımın geç gelmesi | Triyaj motoru ve senaryolar tasarımdan bağımsız ilerleyebilir |

---

## 14. Açık kararlar (ekip toplantısı)

- [ ] **Proje adı.** Tasarımda çalışma adı olarak *TWENTY* kullanıldı; ekip onayı bekleniyor
- [ ] Arayüz Türkçe; global jüri için TR/EN anahtarı eklensin mi? (öneri: evet, bkz. TASARIM.md bölüm 1)
- [ ] AI asistanın adı ve kişiliği (sakin, güven veren bir uçuş doktoru)
- [ ] Ana demo senaryosu S1 mi, başka mı?
- [ ] Senaryodaki astronot karakteri (ad, uyruk, memleket: Dünya Penceresi için)
- [x] Animasyon aracı: Motion (Framer Motion) + elle yazılmış SVG (TASARIM.md bölüm 11)
