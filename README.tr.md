# Ozillo YouTube Research

**Ozillo Studio verisiyle çalışan, Claude Code için YouTube niş ve kanal araştırma skill'leri.**

Claude'a "bu niş mantıklı mı?", "sırada ne çekeyim?" ya da "bu kanal nasıl bu kadar izleniyor?" diye sorun, cevaplar tahmine değil gerçek videolara dayansın. Her iddia şu bilgilerle gelir: kanal, başlık, izlenme, videonun kanalının normalini kaç kat geçtiği ve verinin kaydedildiği tarih.

> 🇬🇧 English documentation: [README.md](README.md)

---

## İçindekiler

- [Neler var](#neler-var)
- [Nasıl çalışır](#nasıl-çalışır)
- [Gereksinimler](#gereksinimler)
- [Kurulum](#kurulum)
- [Hızlı başlangıç](#hızlı-başlangıç)
- [Skill'ler ayrıntılı](#skiller-ayrıntılı)
  - [ozillo-niche-finder](#ozillo-niche-finder)
  - [ozillo-niche-check](#ozillo-niche-check)
  - [ozillo-niche-breakdown](#ozillo-niche-breakdown)
  - [ozillo-niche-bending](#ozillo-niche-bending)
  - [ozillo-video-ideas](#ozillo-video-ideas)
  - [ozillo-title-formulas](#ozillo-title-formulas)
  - [ozillo-thumbnail-concepts](#ozillo-thumbnail-concepts)
  - [ozillo-script-breakdown](#ozillo-script-breakdown)
  - [ozillo-comment-mining](#ozillo-comment-mining)
  - [ozillo-channel-breakdown](#ozillo-channel-breakdown)
  - [ozillo-channel-audit](#ozillo-channel-audit)
  - [ozillo-sponsorship-scout](#ozillo-sponsorship-scout)
  - [ozillo-competitor-watch](#ozillo-competitor-watch)
  - [ozillo-data](#ozillo-data)
- [Temel kavramlar](#temel-kavramlar)
- [MCP araçları](#mcp-araçları)
- [Limitler ve kotalar](#limitler-ve-kotalar)
- [Daha iyi sonuç için ipuçları](#daha-iyi-sonuç-için-ipuçları)
- [Sorun giderme](#sorun-giderme)
- [Sık sorulan sorular](#sık-sorulan-sorular)
- [Yol haritası](#yol-haritası)
- [Katkıda bulunma](#katkıda-bulunma)
- [Lisans](#lisans)

---

## Neler var

| Alan | Skill | Komut | Cevapladığı soru |
|---|---|---|---|
| Niş | **Niş bulucu** | `/ozillo-youtube-research:ozillo-niche-finder` | Henüz nişim yok. İlgi alanımın içindeki hangi nişler değer? Kanıtlı, sıralı bir kısa liste. |
| Niş | **Niş kontrolü** | `/ozillo-youtube-research:ozillo-niche-check` | Bu nişe girilir mi? Sonunda net bir karar: **gir**, **şu açıyla gir** ya da **girme**. |
| Niş | **Niş çözümlemesi** | `/ozillo-youtube-research:ozillo-niche-breakdown` | Bu niş nasıl işliyor? Oyuncular kim, ne sıçrıyor, hangi başlık formülleri tekrarlanıyor, izleyici ne istiyor, boşluklar nerede. |
| Niş | **Niş bükme** | `/ozillo-youtube-research:ozillo-niche-bending` | Nişim çok kalabalık. Komşu nişten hangi format, kitle ya da açı beni öne çıkarır? |
| İçerik | **Video fikirleri** | `/ozillo-youtube-research:ozillo-video-ideas` | Sırada ne çekeyim? Outlier'lardan, başlık formüllerinden ve yorumlardan çıkarılmış fikirler ve başlıklar. |
| İçerik | **Başlık formülleri** | `/ozillo-youtube-research:ozillo-title-formulas` | Başlığım iyi mi? İşe yaramış formüllere dayanan, istatistikli daha güçlü alternatifler. |
| İçerik | **Kapak görseli konseptleri** | `/ozillo-youtube-research:ozillo-thumbnail-concepts` | Kapak görselim nasıl olmalı? Outlier'ların görsel kalıplarından tasarımcıya hazır konseptler. |
| İçerik | **Senaryo çözümlemesi** | `/ozillo-youtube-research:ozillo-script-breakdown` | Kazanan videolar nasıl kurgulanıyor? Transcript'lerden giriş, yapı, doruk; yeniden kullanılabilir şablon ve istenirse kendi senaryonuzun incelemesi. |
| İçerik | **Yorum madenciliği** | `/ozillo-youtube-research:ozillo-comment-mining` | İzleyici ne istiyor? Sorular, istekler, dertler ve kendi kelimeleri; beğeni sayısına göre ağırlıklı. |
| Kanal | **Kanal çözümlemesi** | `/ozillo-youtube-research:ozillo-channel-breakdown` | Bu kanal bunu nasıl yapıyor? Oyun planı: hit'ler, başlık formülleri, video yapısı, izleyici, rakipler. |
| Kanal | **Kanal denetimi** | `/ozillo-youtube-research:ozillo-channel-audit` | Kanalım neden büyümüyor? Rakiplere ve nişe göre dürüst teşhis ve önceliklendirilmiş hızlı kazanımlar. İstenirse acımasız da olur. |
| Kanal | **Sponsorluk araştırması** | `/ozillo-youtube-research:ozillo-sponsorship-scout` | Sponsor bulabilir miyim? Çevrenizde sponsor ne kadar yaygın, hangi benzer kanallarda var, hangi markalar görünüyor ve nasıl teklif götürülür. |
| Kanal | **Rakip takibi** | `/ozillo-youtube-research:ozillo-competitor-watch` | Rakiplerim ne paylaştı? Yeni videoları, hangilerinin normalini aştığı ve nasıl karşılık verileceğinin özeti. |
| — | **Veri katmanı** | `ozillo-data` | Elle çalıştırılmaz: her skill'in önce yüklediği ortak kural kitabı. |

Bir skill'i komutuyla çağırabilir ya da isteğinizi düz bir cümleyle yazabilirsiniz. Claude isteğe uygun skill'i kendisi seçer.

## Nasıl çalışır

```
Siz ──► Claude Code ──► skill (iş akışı + kurallar) ──► Ozillo Studio MCP sunucusu ──► Ozillo verisi
                                                            studio.ozillo.com/mcp
```

- **Skill'ler**, Claude'a hangi adımları izleyeceğini, hangi araçları çağıracağını ve sonuçları nasıl sunacağını söyleyen talimat dosyalarıdır.
- **Ozillo Studio MCP sunucusu**, Claude'a Ozillo verisi üzerinde 31 araç verir:
  - anlama göre kanal arama, outlier videolar ve niş istatistikleri,
  - kanal profilleri, başlık kalıpları, yorumlar ve transcript'ler,
  - kendi koleksiyonlarınız ve rakipleriniz.
- **Ozillo verisi** milyonlarca YouTube kanalını ve videolarını kapsar. Her kanalın yapay zekâyla oluşturulmuş bir içerik profili vardır (niş, kategori, dil, üslup). Anlama göre aramayı mümkün kılan bu profillerdir.

Plugin hem skill'leri hem de MCP bağlantısını kurar.

## Gereksinimler

- [Claude Code](https://claude.com/claude-code) (komut satırı, masaüstü uygulaması ya da IDE eklentisi).
- **Aktif ücretli planı** olan bir [Ozillo Studio](https://studio.ozillo.com) hesabı. MCP sunucusu ücretli planı olmayan hesapları kabul etmez.

## Kurulum

### 1. Marketplace'i ekleyin ve plugin'i kurun

Claude Code'da:

```
/plugin marketplace add ozillostudio/ozillo-youtube-research
/plugin install ozillo-youtube-research@ozillo-youtube-research
```

### 2. Ozillo Studio'ya giriş yapın

1. `/mcp` komutunu çalıştırın ve `ozillo` sunucusunu seçin.
2. **Authenticate** seçeneğini seçin; tarayıcınızda Ozillo Studio giriş sayfası açılır.
3. Giriş yapıp erişimi onaylayın. Sayfa bağlantının kurulduğunu söyleyince Claude Code'a dönün.

Claude Code oturumu saklar ve kendisi yeniler. Süresi dolarsa bu adımları tekrarlayın.

### 3. Çalıştığını kontrol edin

Şunu sorun:

```
Ozillo hesap durumumu kontrol et.
```

Claude plan adınızı ve token bakiyenizi söylemeli. Ozillo araçlarının kullanılamadığını söylüyorsa [Sorun giderme](#sorun-giderme) bölümüne bakın.

### Güncelleme ve kaldırma

```
/plugin marketplace update ozillo-youtube-research
/plugin uninstall ozillo-youtube-research@ozillo-youtube-research
```

### Claude Code dışında kullanım

MCP sunucusu, OAuth destekli uzak MCP sunucularını kullanabilen her istemciyle çalışır. Adresi:

```
https://studio.ozillo.com/mcp
```

- claude.ai'de özel bağlayıcı olarak ekleyin (Ayarlar → Bağlayıcılar). Team ve Enterprise planlarında bağlayıcıları yalnızca organizasyon sahibi ekleyebilir; ekleyemiyorsanız yöneticinizden isteyin.
- `skills/` klasöründeki skill dosyaları claude.ai'ye özel skill olarak yüklenebilir.

## Hızlı başlangıç

Şu istekleri deneyin:

```
"Evde pratik yemek tarifleri" yeni bir kanal için iyi bir niş mi?
```

```
Bütçe dostu seyahat vlog'u için 10 video fikri ver. Küçük bir kanalım.
```

```
@kanaladi kanalını çözümle: neden bu kadar izleniyor?
```

```
/ozillo-youtube-research:ozillo-niche-check yeni başlayanlar için kişisel finans, İngilizce konuşan kitle
```

## Skill'ler ayrıntılı

### ozillo-niche-finder

**Amaç:** geniş bir ilgi alanından kısa ve sıralı bir niş listesine ulaşmak.

**Ne zaman kullanılır:** kanal açmak istiyorsunuz ama nişi bilmiyorsanız ya da birkaç yönü karşılaştırmak istiyorsanız.

**Örnek istekler**

- "Yazılım mühendisiyim. Hangi YouTube nişlerine girebilirim?"
- "Yemek etrafında yüz göstermeden yapılabilecek bir niş bul."
- "/ozillo-youtube-research:ozillo-niche-finder öğrenciler için kişisel gelişim"

**Ne yapar**

1. İlgi alanınızın içinde 8–12 aday niş üretir. Adaylar kitle, format ve açı bakımından farklıdır; kısıtlarınıza (yüzsüz, yalnızca Shorts, düşük bütçe) uyar.
2. Veride neredeyse hiç karşılığı olmayan adayları ve birbirinin tekrarı olanları eler.
3. En iyi 5–6 adayın niş istatistiklerini ölçer.
4. Görünür gerekçelerle sıralar: outlier oranı, 10 bini geçen yeni kanallar, nişin tepede ne kadar yığıldığı, aktiflik ve sponsorluk.
5. İlk 3 niş için kanıt videoları gösterir.

**Ne alırsınız:** bir kısa liste tablosu; ilk 3 nişin kanıt satırları ve giriş açısıyla ayrıntısı; elenen adaylar ve nedenleri; sonraki adım (favorinize `ozillo-niche-check`).

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 15–20 araç çağrısı.

---

### ozillo-niche-check

**Amaç:** bir niş için sayılara dayanan bir gir / girme kararı.

**Ne zaman kullanılır:** niş seçerken, kanal açarken ya da bir konunun doymuş olup olmadığını merak ettiğinizde.

**Örnek istekler**

- "Araba inceleme kanalı açmak hâlâ mantıklı mı?"
- "Minecraft survival kanalı tutar mı?"
- "/ozillo-youtube-research:ozillo-niche-check antika saat tamiri"

**Ne yapar**

1. Fikrinizi aramanın en iyi anladığı biçime, yani kısa bir niş tanımına çevirir.
2. Eşleşen en fazla 80 kanal için niş istatistiklerini çeker: kanal sayısı, abone dağılımı, yeni kanallar, aktiflik, videoların ne sıklıkla sıçradığı, sponsorluk ve mağaza oranları.
3. Aynı istatistikleri yalnızca 50 binin altındaki kanallar için çeker. Böylece küçük kanalların büyükler kadar sıçrayıp sıçramadığını görür.
4. Kanalının normalini en çok aşan videoları kanıt olarak bulur.
5. Kalabalığı, yeni kanalların başarısını, outlier oranını, aktifliği ve sponsorluğu birlikte tartıp karar verir.

**Ne alırsınız**

- **Karar**: gir / şu açıyla gir / girme, ve ana gerekçe.
- **Sayılar**: bir tablo ve verinin tarih aralığı.
- **Ne sıçrıyor**: 3–5 kanıt satırı ve arkalarındaki ortak kalıp.
- **Yeni gelen nereye sığar**: küçük kanal karşılaştırması ve örnek alınacak en büyük yeni kanallar.
- **Riskler** ve **sonraki adım**; çoğunlukla aynı niş için `/ozillo-youtube-research:ozillo-video-ideas` çalıştırmak.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 4 araç çağrısı.

---

### ozillo-niche-breakdown

**Amaç:** bir nişin nasıl işlediğinin tam haritası. Niş kontrolü girip girmemeye karar verir; bu skill nasıl kazanılacağını anlatır.

**Ne zaman kullanılır:** bir nişe girerken ya da zaten içindeyken onu daha iyi anlamak istediğinizde.

**Örnek istekler**

- "Kişisel finans nişini çözümle."
- "Evde egzersiz Shorts'larında şu an ne işe yarıyor?"

**Ne yapar**

1. Nişin şeklini çıkarır: abone bantları, yeni kanallar, aktiflik, outlier oranı ve sponsorluk.
2. Oyuncuları gruplar: yerleşik kanallar, yükselen yeni kanallar ve izlenmesi olağandışı yüksek küçük kanallar.
3. Outlier'ları konu, vaat ve formata göre kümeler; uzun video ve Shorts için ayrı ayrı.
4. Aralarında tekrarlanan başlık formüllerini bulur.
5. En güçlü videoların yorumlarından talebi okur.
6. Talep ile arzı karşılaştırıp boşlukları bulur.

**Ne alırsınız:** tek paragraflık özet, nişin şekli, kanıtlı kazanan kümeler, başlık formülleri, izleyici talebi ve kanıtlı 3–5 boşluk.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 15 araç çağrısı.

---

### ozillo-niche-bending

**Amaç:** komşu bir nişten kanıtlanmış bir şeyi ödünç alarak kalabalık bir nişin yanında daha taze bir konum bulmak.

**Ne zaman kullanılır:** bir niş doymuş görünüyorsa ya da içinde öne çıkmak istiyorsanız.

**Örnek istekler**

- "Araba inceleme çok kalabalık. Nasıl bükebilirim?"
- "Yemek kanalı için farklı bir açı ver."

**Ne yapar**

1. Ana nişi ölçer ve onu zorlaştıran şeyi adlandırır.
2. Ana nişin outlier'larının zaten kullandığı format ve açıları bulur.
3. Üç türde 6–10 bükme önerisi üretir:
   - başka bir nişin formatı (belgesel, meydan okuma, sıralama),
   - daha dar bir kitle (öğrenciler, ebeveynler, bir şehir),
   - başka bir açı (bütçe, tarih, hatalar).
4. Her bükmenin şu an var olup olmadığını kontrol eder ve umut verenleri ölçer.
5. Ödünç alınan formatın kendi nişinde gerçekten işe yaradığını kanıtlar.

**Ne alırsınız:** ana nişin neden zor olduğu, bir bükme tablosu ve en iyi 3 bükme (kanıt, örnek başlıklar ve risklerle). Henüz kimsenin yapmadığı bir bükme hipotez olarak işaretlenir.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 15–20 araç çağrısı.

---

### ozillo-video-ideas

**Amaç:** ilhama değil kanıta dayanan video fikirleri.

**Ne zaman kullanılır:** bir niş ya da kendi kanalınız için konu, başlık veya içerik planı gerektiğinde.

**Örnek istekler**

- "Sırada ne çekeyim? Kanalım @kanalim."
- "Evde egzersiz için 10 Shorts fikri."
- "10 binin altında aboneli bir kişisel finans kanalı için video fikirleri."

**Ne yapar**

1. Bir kanal verirseniz profilini okuyup nişini, dilini ve formatını öğrenir.
2. Nişteki 25'e kadar outlier videoyu toplar. Küçük kanallar için benzer büyüklükteki kanallarda da arar; böylece fikirler sizin erişiminize uyar.
3. Outlier'ları kelimelere göre değil, arkalarındaki fikre göre gruplar (konu, vaat, açı). Bir kalıp sayılması için en az iki farklı kanalda görülmesi gerekir; tek video daha zayıf, tekil bir sinyaldir.
4. En güçlü başlıkları Ozillo'nun başlık formülü veritabanında arar ve her formülün ne kadar yaygın işe yaradığına bakar.
5. En güçlü videoların öne çıkan yorumlarında tekrar eden soruları ve istekleri arar. Bunlar izleyicinin zaten istediği fikirlerdir.
6. Fikirleri kanalınıza uyarlayarak yazar. Başlığı kopyalamaz; yapıyı ve vaadi ödünç alır.

**Ne alırsınız**, her fikir için:

- bir çalışma başlığı ve bir alternatif,
- tek cümlelik açı,
- neden işe yarayacağı: 1–3 kanıt satırı ya da bir yorum teması,
- format ve ilk 10 saniyenin girişi.

Liste, görülen kalıplar ve fikirlerin ne kadar veriye dayandığına dair bir notla biter.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 5–10 araç çağrısı.

---

### ozillo-title-formulas

**Amaç:** nişinizde gerçekten işe yaramış formüllere dayanan başlıklar.

**Ne zaman kullanılır:** bir başlığa ihtiyacınız olduğunda, bir taslağı kontrol etmek istediğinizde ya da bir niş için başlık rehberi istediğinizde.

**Örnek istekler**

- "Bu başlık iyi mi: 'Bir ay asgari ücretle yaşamayı denedim'?"
- "İlk araba alma videom için başlık seçenekleri."

**Ne yapar**

1. Nişin outlier'larında tekrarlanan formülleri bulur.
2. Her formülün gücünü başlık kalıbı veritabanından çeker: video ve kanal sayısı, medyan izlenme, trend skoru.
3. İsterseniz nişin henüz kullanmadığı, yükselen formülleri de ekler.
4. Taslağınızı kontrol eder: hangi formüle uyduğunu, ayrıca vaat, merak, somutluk ve uzunluğu.
5. Her biri adı belli bir formüle dayanan 8–10 alternatif yazar.

**Ne alırsınız:** taslağınızın incelemesi, formülü ve kanıtıyla başlık seçenekleri (en iyi 3'ü işaretli) ve niş için bir formül özeti.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 10–15 araç çağrısı.

---

### ozillo-thumbnail-concepts

**Amaç:** nişte tıklanan şeylere dayanan kapak görseli yönleri.

**Ne zaman kullanılır:** kapak görseli fikrine ya da tasarımcı için bir brife ihtiyaç duyduğunuzda.

**Örnek istekler**

- "'1990 model Fiat'ı yeniledim' videom için kapak fikirleri."
- "Tasarımcım için en iyi seyahat vlog'ları tarzında bir kapak brifi yaz."

**Ne yapar**

1. Nişteki outlier'ları toplar.
2. Milyonlarca thumbnail'ı içeriklerine göre arar (Türkçe isteklerde bile sizin için kısa İngilizce görsel açıklamalar yazar). Kanallar arasında yüksek izlenmeyle tekrar eden bir görünüm, kanıtlanmış bir yöndür. Bir referans video üzerinde görsel benzerlik araması isteğe bağlıdır.
3. İstemci görsel görebiliyorsa kapak görsellerine bakar; göremiyorsa kümelerden çalışır ve linkleri size verir.
4. 3–5 görsel kalıp çıkarır ve her birinin başlıkla nasıl eşleştiğini not eder.
5. 4–6 konsept yazar: yerleşim, görsel üstü metin (en fazla 3–4 kelime), özne ve ifade, renkler ve eşleşen başlık.

**Ne alırsınız:** nişin kanıtlı görsel kalıpları, tasarımcıya hazır konseptler ve önce A/B testine alınacak 2 konsept.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 5–10 araç çağrısı.

---

### ozillo-script-breakdown

**Amaç:** kazanan videoların nasıl kurgulandığını transcript'lerinden öğrenmek.

**Ne zaman kullanılır:** bir videonun girişini ve yapısını anlamak, bir niş için senaryo şablonu almak ya da kendi senaryonuzu inceletmek istediğinizde.

**Örnek istekler**

- "Bu videonun senaryosunu çözümle: https://www.youtube.com/watch?v=..."
- "En iyi kişisel finans videoları nasıl açılıyor? Bana şablon ver."
- "Senaryomu incele." (metni yapıştırın)

**Ne yapar**

1. Farklı kanallardan 2–3 güçlü outlier'ı ya da verdiğiniz videoyu seçer.
2. Transcript'lerini çeker. 3'ten fazla kullanmadan önce kalan kotayı söyler.
3. Her bölümü zamanlar:
   - giriş (ilk 30 saniye),
   - kurulum,
   - bölümler,
   - izleyiciyi tutan teknikler (açık döngüler, kalıp kırıcılar),
   - doruk,
   - sponsor ve çağrı,
   - dakika başına kelime.
4. Videoları karşılaştırır ve ortak noktalarını şablona dönüştürür.
5. İsterseniz kendi senaryonuzu şablonla kıyaslar. Talep ederseniz Ozillo'nun ücretli izlenebilirlik ve politika riski analizini çalıştırır; önce token maliyetini gösterir.

**Ne alırsınız:** her video için bir zaman çizelgesi tablosu, hedef süreleriyle doldurulabilir bir senaryo şablonu, konunuz için bir giriş cümlesi bankası ve istenirse senaryo incelemesi.

**Maliyet:** 2–3 transcript (önceden çekilmişse ücretsiz). İsteğe bağlı analiz token harcar; yalnızca onayınızla çalışır.

---

### ozillo-comment-mining

**Amaç:** yorumlardan izleyici içgörüsü çıkarmak.

**Ne zaman kullanılır:** izleyicilerden video fikri almak, tepkileri anlamak ya da bir ürün veya teklif öncesinde kitle araştırması yapmak istediğinizde.

**Örnek istekler**

- "@kanaladi videolarının altında izleyiciler ne istiyor?"
- "Bu videonun yorumlarını analiz et."
- "Evde egzersiz nişi için kitle araştırması."

**Ne yapar**

1. Videoları seçer:
   - tek bir video,
   - bir kanalın en iyi ve en yeni videoları,
   - ya da bir nişin en güçlü outlier'ları.
2. Her video için en fazla 300 popüler yorumu okur; yeni videolarda ayrıca en yeni yorumlara bakar.
3. Yorumları soru, istek, dert, itiraz, somut övgü ve kişisel hikâye olarak etiketler; her birini beğeni sayısıyla ağırlıklandırır.
4. Başlık ve giriş için izleyicinin kullandığı kelimeleri toplar.

**Ne alırsınız:** sayılarıyla ve kısa örnekleriyle ana temalar, izleyiciden gelen video fikirleri, bir kitle profili (çıkarım olarak işaretli) ve kullanılacak kelimeler.

**Maliyet:** transcript ve token kullanmaz. Yaklaşık 5–8 araç çağrısı.

---

### ozillo-channel-breakdown

**Amaç:** bir kanalın oyun planını anlamak; ondan öğrenmek ya da onunla rekabet etmek için.

**Ne zaman kullanılır:** bir rakibi, örnek aldığınız bir kanalı ya da kendi kanalınızı incelerken.

**Örnek istekler**

- "@kanaladi kanalını analiz et."
- "Bu kanal nasıl büyüdü? https://www.youtube.com/@kanaladi"
- "Rakibimi çözümle ve hangi boşlukları bıraktığını söyle."

**Ne yapar**

1. Kanal profilini okur: abone, video sayısı, video başına ortalama izlenme, format, niş, skor ve RPM tahmini.
2. Tüm zamanların en iyi videolarını ve son 25 videosunu listeler. Her yeni videoyu kanalın ortalamasıyla kıyaslayıp şu an neyin işe yaradığını ve neyin artık yaramadığını görür.
3. En iyi başlıklarını başlık formülleriyle eşleştirir.
4. En güçlü 2 videosunun transcript'ini okuyup girişi, yapıyı, doruk noktasını ve çağrıyı çıkarır. Transcript'ler daha önce çekilmemişse bu, günlük transcript hakkınızdan 2 tane kullanır.
5. Öne çıkan yorumları özetler: izleyicilerin neyi övdüğü, ne istediği ve neden şikâyet ettiği.
6. Benzer kanalları bulur ve kanalı en yakın rakipleriyle karşılaştırır.

**Ne alırsınız:** oyun planının tek paragraflık özeti, ardından:

- profil,
- kendi normalini aşan videolar (kanıt satırlarıyla),
- başlık formülleri,
- en iyi videolarının içi,
- izleyicinin istedikleri,
- rakiplerle karşılaştırma,
- 3–5 somut çıkarım ve kanalın açık bıraktığı boşluklar.

**Maliyet:** en fazla 2 transcript (önceden çekilmişse ücretsiz), token kullanmaz. Yaklaşık 10 araç çağrısı.

---

### ozillo-channel-audit

**Amaç:** kanalınızın dürüst bir teşhisi ve yapılacaklar listesi. Acımasız yorum isterseniz sert ama adil olur: kişiyi değil işi eleştirir ve her zaman veriye dayanır.

**Ne zaman kullanılır:** büyüme durduğunda, izlenmeler düştüğünde ya da dışarıdan bir göz istediğinizde.

**Örnek istekler**

- "@kanalim kanalımı denetle."
- "Neden büyümüyorum? Acımasız ol."
- "Kanalımı yerden yere vur."

**Ne yapar**

1. Kanalınızı en yakın 4 rakibiyle karşılaştırır.
2. Son 25 videonuzun hit'lerini ve ıskalarını kendi ortalamanıza göre bulur. Konu kayması, zayıflayan başlıklar, değişen format karışımı ve yavaşlayan ritim arar.
3. Zayıf başlıklarınızda, hit'lerinizin kullandığı formüllerin eksik olup olmadığına bakar.
4. Hit oranınızı nişin outlier oranıyla kıyaslar.
5. Yeni yorumlarda şikâyet ve izleyici kaybı işaretleri arar.
6. İsterseniz en zayıf videonuzda ücretli senaryo riski analizini onayınızla çalıştırır.

**Ne alırsınız:** 3 cümlelik karar, bir karne (siz / rakipler / niş), neyin işe yaradığı, neyin yaramadığı ve muhtemel nedeni, öncelikli 5 hızlı kazanım ve riskler.

**Maliyet:** senaryo incelemesi istemedikçe transcript kullanmaz. Yaklaşık 10–15 araç çağrısı.

---

### ozillo-sponsorship-scout

**Amaç:** çevrenizde sponsorluk parasının zaten nereye aktığını bulmak.

**Ne zaman kullanılır:** marka işbirliği ya da AdSense dışında gelir istediğinizde.

**Örnek istekler**

- "Kanalım sponsor bulabilir mi? @kanalim"
- "Türkiye'de teknoloji inceleme kanallarına hangi markalar sponsor oluyor?"

**Ne yapar**

1. Nişinizin sponsorluk ve mağaza oranlarını ölçer.
2. 8–10 benzer kanalda sponsor ve mağaza olup olmadığına bakar. Sponsoru olan en küçük kanalı not eder; bu, sponsorların ne kadar erken geldiğini gösterir.
3. Sponsorlu 2–3 rakibin son popüler videolarının transcript'lerinde sponsor okumalarını arar: marka, kategori ve yerleşim. Bu tam bir liste değil, bir örneklemdir.
4. Sizi sponsorlu rakiplerle karşılaştırır.

**Ne alırsınız:** nişin sponsor iklimi, sponsorlu rakipler tablosu, görülen markalar, hazırlık durumunuz ve bir plan: teklif götürülecek marka kategorileri, teklif açısı ve entegrasyon fikirleri.

**Maliyet:** 2–3 transcript (önceden çekilmişse ücretsiz), token kullanmaz. Anlaşma fiyatı tahmini yapmaz.

---

### ozillo-competitor-watch

**Amaç:** rakiplerinizin yayımladıklarının düzenli bir özeti.

**Ne zaman kullanılır:** rakipleri haftalık ya da aylık kontrol etmek istediğinizde.

**Örnek istekler**

- "Bu hafta rakiplerim ne paylaştı?"
- "'Teknoloji' koleksiyonum için aylık rakip özeti."

**Ne yapar**

1. Rakip listenizi okur: Ozillo koleksiyonlarınızda "competitor" etiketli kanallar. Listeniz yoksa aday önerir ve yalnızca onayınızdan sonra onları bir koleksiyona ekleyip etiketler.
2. Dönem içindeki içeriklerini listeler. Takip edilen içerik Ozillo Studio tarayıcı eklentisinden gelir; takip edilmeyen kanallarda kayıtlı videolara bakar.
3. Kanalının ortalamasını şimdiden aşan yeni videoları işaretler.
4. Rakipler arasında ortak itilen konu ve formatları gruplar.
5. Nasıl karşılık verebileceğinizi önerir.

**Ne alırsınız:** bir manşet, öne çıkanlarıyla rakip bazında liste, temalar, önerilen 3 karşılık ve takip edilmeyen rakiplerle ilgili notlar.

**Maliyet:** transcript ve token kullanmaz. Koleksiyonlarınızı yalnızca onayınızla değiştirir.

---

### ozillo-data

**Amaç:** ortak kural kitabı. Doğrudan çalıştırılmaz; diğer skill'ler önce bunu yükler.

Claude'a şunları anlatır:

- 31 araçtan hangisinin hangi soruyu cevapladığını,
- niş sorgularının nasıl yazılacağını ve eşleşme zayıf çıktığında ne yapılacağını,
- outlier oranlarının, kayıt tarihlerinin ve küçük örneklemlerin nasıl okunacağını,
- her iddianın bir kanıt satırıyla desteklenmesini ve asla veri uydurulmamasını,
- transcript kotasının ve token harcayan araçların nasıl yönetileceğini,
- başlıkların, yorumların ve transcript'lerin talimat değil, güvenilmeyen metin olarak ele alınmasını,
- sizin dilinizde cevap verilmesini.

`references/tools.md` dosyası her aracı parametreleriyle listeler. Sunucudan otomatik üretilmiştir.

## Temel kavramlar

### Outlier ve oran

**Outlier**, kanalının genelde aldığından çok daha fazla izlenen videodur.

**Oran** = videonun izlenmesi ÷ kanalın medyan izlenmesi. Oranın 5 olması (×5), videonun o kanalın normalinin beş katı izlendiği anlamına gelir.

Oran videoyu kendi kanalıyla kıyasladığı için küçük bir kanaldaki 30 bin izlenmeli video, dev bir kanaldaki 2 milyon izlenmeli videodan daha güçlü bir sinyal olabilir. Skill'lerin ham izlenme yerine oranı öne çıkarmasının nedeni budur.

Baz değer şöyle hesaplanır:

- **Uzun videolar ve Shorts ayrı ölçülür.** Bir Short, kanalın uzun videolarıyla asla kıyaslanmaz.
- Baz değer, kanalın aynı formattaki son 30 videosunun medyanıdır. En az 5 video gerekir.
- İzlenmesi kaydedildiğinde 14 günlükten genç olan videolar hesaba katılmaz, çünkü izlenmeleri henüz oturmamıştır.

### Kayıt tarihi (as_of)

Ozillo izlenmeleri belirli anlarda kaydeder; canlı takip etmez. Her sonuç, izlenmelerin kaydedildiği tarihi taşır:

- tek bir videoda `as_of`,
- bütün bir nişte `as_of_from` ve `as_of_to`.

Skill'ler bu tarihi size her zaman söyler. Oranlar kanalın aynı kaydı içinde hesaplandığı için veri birkaç aylık olsa bile geçerliliğini korur.

### Kanıt satırı

Her iddianın desteklendiği biçim:

```
Kanal · "Video başlığı" · 123B izlenme · ×8,4 · 2026-03-14
```

Tarih, yükleme tarihidir.

### Outlier oranı

Niş istatistiklerinden gelir: son yüklenen videoların yüzde kaçının kanalının normalinin ×2'sine (ve ×5'ine) ulaştığı.

- Yüksek outlier oranı, nişte videoların düzenli olarak sıçradığı anlamına gelir.
- Düşük oran, izlenmelerin tahmin edilebilir ve aşılması zor olduğu anlamına gelir.

### Eşleşme kalitesi

Anlama göre arama `match_quality` döndürür.

- `good`: niş net biçimde bulundu.
- `weak`: sorgu, verinin tanıdığı bir nişi tarif etmedi. Claude sorguyu yeniden yazıp tekrar dener; zayıf kalırsa size söyler.

### Küçük örneklem

`low_sample`, niş istatistiklerinin yeterli ölçülebilir videosu olan 15'ten az kanala dayandığı anlamına gelir. Bu sayıları kaba tahmin olarak değerlendirin.

### RPM

RPM değerleri, video süresi ve kanal büyüklüğünden yapılan **tahminlerdir**. Reklamverenlerin bir konuya ne ödediğini yansıtmaz ve gerçek kazanç değildir. Skill'ler nişleri asla RPM'e göre sıralamaz. Bir nişin para kazanma potansiyeli için sponsorlu ya da mağazalı kanalların oranına bakar.

### Geçmiş veri yok

Ozillo abone sayılarının geçmişini değil, güncel değerini saklar. Bu yüzden skill'ler bir kanalın ya da nişin zaman içinde "büyüdüğünü" veya "düştüğünü" iddia etmez.

## MCP araçları

Ozillo Studio MCP sunucusu 31 araç sunar. Skill'ler bunları sizin yerinize seçer; isterseniz herhangi birini doğrudan da isteyebilirsiniz.

| Alan | Araçlar |
|---|---|
| Kanal bulma | `search_channels` (anlama ya da kelimeye göre), `find_similar_channels` |
| Niş araştırması | `search_outliers`, `get_niche_stats` |
| Kanal ayrıntıları | `get_channel`, `get_channel_top_videos`, `get_channel_recent_videos`, `compare_channels` |
| Videolar | `search_similar_videos`, `search_thumbnails`, `get_video_comments`, `get_video_transcript` |
| Başlık formülleri | `search_title_patterns`, `get_title_pattern`, `get_title_pattern_videos` |
| Koleksiyonlarınız | `list_collections`, `get_collection`, `create_collection`, `add_channel_to_collection`, `add_video_to_collection`, `remove_channel_from_collection`, `remove_video_from_collection` |
| Rakipleriniz | `list_competitors`, `get_competitor_overview`, `get_competitor_latest_videos`, `set_channel_competitor` |
| Senaryo analizi | `estimate_transcript_analysis`, `start_transcript_analysis`, `get_transcript_analysis`, `list_transcript_analyses` |
| Hesap | `get_account_status` |

Hesabınızda değişiklik yapan araçlar: koleksiyon oluşturan, ekleyen ya da çıkaran araçlar, `set_channel_competitor` ve `start_transcript_analysis`. Claude bunları yalnızca siz istediğinizde kullanır.

Tüm parametreler: [skills/ozillo-data/references/tools.md](skills/ozillo-data/references/tools.md).

## Limitler ve kotalar

| Limit | Değer |
|---|---|
| Transcript | Kullanıcı başına günde 10 **yeni** transcript. Herhangi birinin daha önce çektiği transcript'ler önbellekten gelir ve ücretsizdir. Her transcript yanıtı kalan hakkınızı ve sayacın ne zaman sıfırlanacağını gösterir (UTC gece yarısı, Türkiye saatiyle 03:00). |
| Senaryo analizi | Ozillo token'ı harcar: 5.000 karakter başına 1 token, başlık ve açıklama önerileri için +1. Claude maliyeti her zaman gösterir ve başlamadan önce onay ister. |
| İstek hızı | Kullanıcı başına dakikada 60, saatte 600, günde 3.000 araç çağrısı. |
| Çağrı başına sonuç | En fazla 25 öğe (yorumlarda 500). Devamı için sayfa kullanılır. |
| Kapsam | Niş araştırmasında yalnızca en az 1.000 abonesi olan kanallar yer alır. Ozillo'da henüz bulunmayan bir kanal analiz edilemez. |

## Daha iyi sonuç için ipuçları

- **Soruyu değil, nişi tarif edin.** "Bütçe dostu seyahat vlog'u", "ucuza nasıl tatil yapılır?" sorusundan daha iyi sonuç verir. Claude ifadenizi kendisi de düzeltir, ama net bir niş daha iyi sonuç verir.
- **Büyüklüğünüzü ve formatınızı söyleyin.** "Küçük bir Shorts kanalıyım" demek, fikirleri ve karşılaştırmaları size uygun hale getirir.
- **Kanallar için handle ya da link kullanın.** `@handle` ya da kanal adresi yanlış kanalın seçilmesini önler.
- **Sonraki adımı isteyin.** Niş kontrolünden sonra "şimdi bu niş için fikir ver" deyin; skill'ler birbirine bağlanacak şekilde tasarlandı.

## Sorun giderme

| Sorun | Ne yapmalı |
|---|---|
| "Ozillo araçları kullanılamıyor" | `/mcp` komutunu çalıştırın, `ozillo` sunucusunu seçin, Authenticate ile giriş yapın. |
| "Requires an active paid subscription" | MCP erişimi ücretli bir Ozillo Studio planı gerektirir. Planınızı studio.ozillo.com'da kontrol edin. |
| claude.ai'de bağlayıcı eklenemiyor | Team/Enterprise planlarında bağlayıcıları organizasyon sahibi ekler. Yöneticinizden isteyin ya da Claude Code kullanın. |
| `match_quality: weak` sürekli geliyor | Niş çok dar ya da alışılmadık biçimde ifade edilmiş. Birkaç sade kelimeyle tarif edin ya da İngilizce deneyin. Hiç eşleşmeyen bir niş, YouTube'da gerçekten çok küçük olabilir. |
| "Channel was not found in Ozillo Studio" | Ozillo o kanalı henüz dizine eklememiş. Tam @handle'ı deneyin ya da benzer bir kanalı analiz edin. |
| "Daily transcript limit reached" | Mesajda gösterilen sıfırlanma saatini bekleyin. Önbellekteki transcript'ler çalışmaya devam eder; skill'ler transcript olmadan da devam eder. |
| "Too many requests" | Dakikalık istek sınırına ulaştınız. Bir dakika bekleyip devam edin. |
| Sayılar eski görünüyor | Cevaptaki `as_of` tarihine bakın. İzlenmeler canlı sayaç değil, kaydedilmiş anlık görüntülerdir. |

## Sık sorulan sorular

**YouTube API'sini benim hesabımla mı kullanıyor?**
Hayır. Claude, Ozillo hesabınızla Ozillo Studio'ya bağlanır. Ozillo kendi kayıtlı verisini okur; YouTube hesabınız işin içine girmez.

**YouTube Studio analizlerimi görebilir mi?**
Hayır. Yalnızca herkese açık veriyi kullanır: kanallar, videolar, izlenmeler, yorumlar ve transcript'ler.

**İzlenmeler neden canlı değil?**
Ozillo milyonlarca videonun izlenmesini dönem dönem kaydeder. Oranlar videoları kanalın aynı kaydı içinde kıyasladığı için, kayıt birkaç aylık olsa bile sonuçlar geçerliliğini korur. Tarih her zaman gösterilir.

**Verilerim paylaşılıyor mu?**
İstekleriniz, web sitesini kullanır gibi, hesabınızla Ozillo Studio'ya gider. Plugin'in kendisi hiçbir şey saklamaz.

## Yol haritası

- **Trend raporu**: şu anda neyin yükseldiği. Taze izlenme verisi gerektiği için son yüklemelerin anlık yenilenmesiyle birlikte gelecek.
- Fikirleriniz: bir issue açın.

## Katkıda bulunma

Issue ve pull request'ler memnuniyetle karşılanır. Bir skill'i değiştirirken:

- talimatları İngilizce tutun (Claude kullanıcının dilinde cevap verir),
- yalnızca `skills/ozillo-data/references/tools.md` içinde bulunan araçları kullanın,
- hiçbir skill'in bir araçtan almadığı bir sayıyı söylemesine izin vermeyin,
- pull request açmadan önce `claude plugin validate .` çalıştırın.

## Lisans

[MIT](LICENSE)
