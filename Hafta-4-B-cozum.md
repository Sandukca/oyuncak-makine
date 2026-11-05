# Hafta 4, B Grubu: Parabolün Değer Tablosu (Çözüm)

Soru metni `Hafta-4-B.md` dosyasındadır. A grubunun sorusu (Doğrunun değer tablosu) bu soruyla aynı becerileri ölçer, ama B grubunun çözümü A grubuna uymaz. Karşılaştırma için en alttaki tabloya bakınız.

## Önce cümlelerle yazalım

Programı yazmadan önce ne yapacağımızı Türkçe cümlelerle yazıyoruz: programın her satırı için bir cümle. Cümleler aşağıdaki akış şemasını kutu kutu izler.

**Hangi bilgileri hatırlamamız gerekiyor?**

- Girdi hücreleri (değişmez): `a`, `b`, `c`, `baslangic`, `bitis`, `adim`
- Çalışma hücresi: `x` (o anki x değeri)

**Akış şeması**

```mermaid
flowchart TD
    G["x = baslangic"] --> T{"(bitis − x) · adim negatif mi?"}
    T -- "evet" --> D["Dur"]
    T -- "hayır" --> X["x yaz"]
    X --> Y["y = (a·x + b)·x + c hesapla, yaz"]
    Y --> I["x = x + adim"]
    I -- "başa dön" --> T
```

**Satır satır cümleler**

| Satır | Şemadaki kutu | Cümle | Komut |
| --- | --- | --- | --- |
| 1 | x = baslangic | Başlangıç değerini akümülatöre getir. | `basla  load baslangic` |
| 2 | | Bu değeri x hücresine kopyala. x başlangıçtan başlasın. | `store x` |
| 3 | (bitis − x) · adim negatif mi? | Döngünün başı: bitiş değerini getir. | `dongu  load bitis` |
| 4 | | Bitişten x'i çıkar. Akümülatörde bitişe kalan yol var. | `sub x` |
| 5 | | Kalan yolu adımla çarp. (Bu satır yalnızca genel çözümde vardır. Adım pozitif çözümünde yoktur.) | `mul adim` |
| 6 | | Sonuç negatifse bitiş geçilmiştir, son satıra git. | `ifneg son` |
| 7 | x yaz | x'i akümülatöre getir. | `load x` |
| 8 | | x'i ekrana yaz. | `print` |
| 9 | y = (a·x + b)·x + c hesapla, yaz | a'yı akümülatöre getir. | `load a` |
| 10 | | x ile çarp. Akümülatörde a·x var. | `mul x` |
| 11 | | Üzerine b'yi ekle. Akümülatörde a·x + b var. | `add b` |
| 12 | | Yine x ile çarp. Akümülatörde a·x² + b·x var. | `mul x` |
| 13 | | Üzerine c'yi ekle. Akümülatörde artık y var. | `add c` |
| 14 | | y'yi ekrana yaz. | `print` |
| 15 | x = x + adim | x'i yeniden akümülatöre getir. | `load x` |
| 16 | | Üzerine adımı ekle. | `add adim` |
| 17 | | Yeni değeri x hücresine sakla. | `store x` |
| 18 | başa dön | Döngünün başına, 3. satıra dön. | `goto dongu` |
| 19 | Dur | Programı durdur. | `son  stop` |

**5. satırdaki tek çarpma neden işe yarar?** Girdi garantisi sayesinde bitiş geçilmediği sürece `bitis − x` ile `adim` aynı işaretlidir ve çarpımları 0 ya da pozitiftir. Bitiş geçildiği anda işaretler ters olur, çarpım negatife düşer. Bitişe tam denk gelinirse çarpım 0 olur ve o x de yazılır.

**9–13. satırlarda y nasıl kurulur?** Makinede üs alma yoktur. `a·x² + b·x + c`, `(a·x + b)·x + c` diye yazılırsa yalnızca çarpma ve toplamayla hesaplanır (Horner yöntemi). Ayrı bir `x²` hücresine gerek kalmaz. A grubundan farklı olarak burada 9. satırda `load a` gerekir: akümülatörde x vardır, ama hesap a ile başlar.

## Adım pozitif olduğunda çözüm

Önce daha kolay bir basamak: adımın yalnızca pozitif olduğunu varsayan çözüm. x hep büyüyerek bitişe yaklaştığı için "bitiş geçildi mi?" sorusu tektir: bitişe kalan yol, yani `bitis − x`, negatif mi?

```
basla   load baslangic   # x, başlangıç değerinin kopyasıyla başlar
        store x
dongu   load bitis       # bitişe kalan yol: bitis - x
        sub x
        ifneg son        # negatifse x bitişi geçti, dur
        load x           # x'i yaz
        print
        load a           # y = (a*x + b)*x + c
        mul x
        add b
        mul x
        add c
        print            # y'yi yaz
        load x           # x = x + adim
        add adim
        store x
        goto dongu
son     stop
a         1
b         -2
c         -3
baslangic -2
bitis     4
adim      1
x         0
```

18 komut. Örnek 1'deki gibi (bitiş 4, adım 1):

| x | bitis − x | Sonuç |
| --- | --- | --- |
| 3 | 1 | devam |
| 4 | 0 | devam, tam denk geldi, yazılır |
| 5 | −1 | **dur** |

Çıktı: `-2, 5, -1, 0, 0, -3, 1, -4, 2, -3, 3, 0, 4, 5`

Bu çözüm negatif adımla çalışmaz. Örnek 2'de (adım −2) `bitis − x` ilk turda negatif çıkar ve program hiçbir şey yazmadan durur. Negatif adımı da kapsamak için bir sonraki bölümdeki genel çözüme `mul adim` satırı eklenir.

## Genel çözüm (adım pozitif ya da negatif)

"Genel" sözcüğü, bu çözümün adımın **işareti ne olursa olsun** çalıştığını anlatır. Pozitif adım çözümü yalnızca x'in büyüyerek bitişe gittiği durumu bilir. Genel çözüm, x'in küçülerek bitişe indiği durumu (adım negatif, başlangıç bitişten büyük) da aynı kodla kapsar. İki çözümün döngüsü ve y hesabı aynıdır. Fark yalnızca "bitiş geçildi mi?" testindedir.

| Adım | Bitişe giderken x | Bitiş geçilmeden `bitis − x` | Bitiş geçilince `bitis − x` | Pozitif adım testi | Genel test |
| --- | --- | --- | --- | --- | --- |
| Pozitif | büyür | 0 ya da pozitif | negatif | doğru durur | doğru durur |
| Negatif | küçülür | 0 ya da negatif | pozitif | ilk turda yanlış durur | doğru durur |

Negatif adımda `bitis − x`'in işareti ters döndüğü için pozitif adım testi yetmez. Genel çözüm bu farkı tek satırla, `sub x`'ten sonra `mul adim` ile giderir. Kalan yolu adımla çarpınca, bitiş geçilmediği sürece iki işaret aynı olur ve çarpım 0 ya da pozitif çıkar. Bitiş geçildiği anda işaretler ters olur ve çarpım negatife düşer. Böylece tek `ifneg` iki yönde de doğru yerde durur. Bu ayrıntı yukarıda, "5. satırdaki tek çarpma neden işe yarar?" paragrafında da anlatıldı.

Bunun için girdi garantisi gerekir: adım 0 olmaz, adım pozitifse başlangıç bitişten büyük olmaz, adım negatifse başlangıç bitişten küçük olmaz.

```
basla   load baslangic   # x, başlangıç değerinin kopyasıyla başlar
        store x          # (baslangic hücresi değişmez)
dongu   load bitis       # (bitis - x) * adim hesapla
        sub x
        mul adim
        ifneg son        # negatifse bitiş geçildi
        load x           # x'i yaz
        print
        load a           # y = (a*x + b)*x + c
        mul x            # a*x
        add b            # a*x + b
        mul x            # a*x*x + b*x
        add c            # a*x*x + b*x + c
        print            # y'yi yaz
        load x           # x = x + adim
        add adim
        store x
        goto dongu
son     stop
# Hafta 4 B: Parabolün değer tablosu, y = a*x*x + b*x + c
# Girdiler: denemek için yalnızca bu altı değeri değiştiriniz.
# (Örnek 1: y = x*x - 2x - 3, x: -2'den 4'e, adım 1)
a         1
b         -2
c         -3
baslangic -2
bitis     4
adim      1
# Çalışma hücresi: program çalışırken değişir.
x         0
```

19 komut. 6 girdi hücresi ve 1 çalışma hücresi.

### Doğru kabul edilen farklı çözümler

- **İki dallı bitiş testi (ödevdeki gibi):** önce adımın işaretine bakılır, sonra adım pozitifse `bitis − x`, negatifse `x − bitis` negatif mi diye sorulur. 5 komut daha uzundur ve doğru kabul edilir. (A grubu çözümündeki örnek parçanın aynısı.)
- **Formülün doğrudan hesabı:** `x·x·a` bir hücreye, `b·x` başka bir hücreye yazılıp toplanır, sonra `c` eklenir. Daha uzundur ve doğru kabul edilir.
- Simülatörün belgelenmemiş `square` komutu (akümülatörün karesini alır) ile `load x`, `square`, `mul a` … biçiminde bir hesap. Sayfada anlatılmadığı için öğrenciden beklenmez, kullanan olursa doğru kabul edilir.
- Çalışma hücrelerine farklı ama anlamlı adlar verilmesi (`simdikix`, `deger` gibi).

### Çalışan ama istenene uymayan çözümler

- **`baslangic` hücresini x olarak ilerletmek.** Çıktı doğru çıkar, ama hücrenin adı artık içindekini anlatmaz ve girdi bozulur. Soruda açıkça yasaklanmıştır.
- **Girdileri `get` ile okumak.** Döngü ve hesap doğru olabilir, ama soru girdilerin bellekte durmasını ister.

## Çözüm nasıl çalışır: şekildeki iki örnek

![Parabolün değer tablosu, iki örnek](hafta-4-gorseller/hafta4-b-parabol.png)

Şekilde kesikli çizgi parabolün tamamıdır. Her nokta programın yazdığı bir (x, y) çiftidir, dairedeki numara yazılma sırasını gösterir.

### Örnek 1: y = x² − 2x − 3, x: −2'den 4'e, adım +1

Girdi hücreleri: `a = 1`, `b = -2`, `c = -3`, `baslangic = -2`, `bitis = 4`, `adim = 1`

| Tur | x | (bitis − x) · adim | Devam? | a·x + b | (a·x + b)·x | y = … + c |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | −2 | (4 + 2) · 1 = 6 | evet | −4 | 8 | 5 |
| 2 | −1 | 5 | evet | −3 | 3 | 0 |
| 3 | 0 | 4 | evet | −2 | 0 | −3 |
| 4 | 1 | 3 | evet | −1 | −1 | −4 |
| 5 | 2 | 2 | evet | 0 | 0 | −3 |
| 6 | 3 | 1 | evet | 1 | 3 | 0 |
| 7 | 4 | 0 | evet (tam denk) | 2 | 8 | 5 |
| — | 5 | −1 | **dur** | | | |

Çıktı: `-2, 5, -1, 0, 0, -3, 1, -4, 2, -3, 3, 0, 4, 5`

y'nin 0 olduğu x = −1 ve x = 3, denklemin kökleridir: x² − 2x − 3 = (x + 1)(x − 3). Parabol bu iki kök arasında x ekseninin altındadır (turlar 3, 4, 5). En alt nokta x = 1'de y = −4'tür.

### Örnek 2: y = −x² + 4x, x: 6'dan −1'e, adım −2

Girdi hücreleri: `a = -1`, `b = 4`, `c = 0`, `baslangic = 6`, `bitis = -1`, `adim = -2`

| Tur | x | (bitis − x) · adim | Devam? | a·x + b | (a·x + b)·x | y = … + c |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 6 | (−1 − 6) · (−2) = 14 | evet | −2 | −12 | −12 |
| 2 | 4 | (−5) · (−2) = 10 | evet | 0 | 0 | 0 |
| 3 | 2 | 6 | evet | 2 | 4 | 4 |
| 4 | 0 | 2 | evet | 4 | 0 | 0 |
| — | −2 | (−1 + 2) · (−2) = −2 | **dur** | | | |

Çıktı: `6, -12, 4, 0, 2, 4, 0, 0`

Bu örnek üç tuzağı birlikte yoklar:

- **Negatif adım:** x büyükten küçüğe gider.
- **Bitiş atlanır:** x, −1'e hiç denk gelmez, 0'dan sonra −2'ye geçer. −2 yazılmamalıdır.
- **a negatif:** parabol aşağı doğru açılır. Kökler x = 0 ve x = 4'tür (−x² + 4x = −x(x − 4)), tepe noktası x = 2'de y = 4'tür.

## Test değerleri (gerçek simülatör motoruyla denendi)

Öğrenci dosyasındaki "Kendiniz deneyiniz" tablosundaki 5 deneme de aynı şekilde denendi.

| a | b | c | baslangic | bitis | adim | Beklenen çıktı | Neyi yoklar |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `1` | `-2` | `-3` | `-2` | `4` | `1` | `-2, 5, -1, 0, 0, -3, 1, -4, 2, -3, 3, 0, 4, 5` | Örnek 1, kökler, bitişe tam denk gelme |
| `-1` | `4` | `0` | `6` | `-1` | `-2` | `6, -12, 4, 0, 2, 4, 0, 0` | Örnek 2, negatif adım, bitiş atlanır |
| `1` | `0` | `0` | `0` | `0` | `1` | `0, 0` | Başlangıç = bitiş, tek nokta |
| `2` | `0` | `-8` | `-3` | `3` | `1` | `-3, 10, -2, 0, -1, -6, 0, -8, 1, -6, 2, 0, 3, 10` | b = 0, simetrik parabol |
| `0` | `0` | `-1` | `4` | `-4` | `-4` | `4, -1, 0, -1, -4, -1` | a = 0, b = 0: sabit y = −1 |
| `1` | `0` | `1` | `-2` | `2` | `2` | `-2, 5, 0, 1, 2, 5` | Parabol hep eksenin üstünde |

## Sık yapılan hatalar

| Hata | Örnekte çıkan sonuç |
| --- | --- |
| `#` ile başlayan açıklama satırını kodun arasına yazmak | Program o satıra gelince takılır, "infinite loop?" uyarısı. O satırdan önce yazılanlar ekranda kalır |
| x'i `baslangic`'tan kopyalamamak (x 0'dan başlar) | Örnek 1: `0, -3, 1, -4, 2, -3, 3, 0, 4, 5`. Örnek 2: `0, 0` |
| Adım pozitif çözümünü negatif adımla çalıştırmak (`mul adim` yok) | Örnek 1 doğru çıkar. Örnek 2'de ekrana hiçbir şey yazılmaz |
| Bitiş testinde 0'ı da "geçildi" saymak (`ifneg son` + `ifzero son`) | Bitişe tam denk gelen son nokta yazılmaz. Örnek 1: `…, 3, 0` ile biter, `4, 5` eksik. Örnek 2 doğru çıkar |
| `b·x` yerine `b` eklemek (`x·x·a + b + c`) | Örnek 1: `-2, -1, -1, -4, 0, -5, 1, -4, 2, -1, 3, 4, 4, 11`. Örnek 2: `6, -32, 4, -12, 2, 0, 0, 4` |
| Horner'da `load a` unutmak (akümülatördeki x ile devam etmek: x³ + b·x + c hesaplanır) | Örnek 1: `-2, -7, -1, -2, 0, -3, 1, -4, 2, 1, 3, 18, 4, 53`. Örnek 2: `6, 240, 4, 80, 2, 16, 0, 0` |
| Girdi hücresinin adını farklı yazmak: `baslangic` ya da `adim` | "there is no statement labeled …" uyarısı, ekran `NaN` ile dolar, program durmaz ("infinite loop?") |
| Girdi hücresinin adını farklı yazmak: `bitis` | Uyarı gelir, ama `NaN` görünmez. Noktalar doğru yazılır, bitiş geçilince de durmaz, "infinite loop?" |
| Girdi hücresinin adını farklı yazmak: `a`, `b` ya da `c` | Uyarı gelir, x'ler doğru, her y `NaN`. Program biter |
| x'e adımı eklememek ya da `store x` unutmak | Hep ilk nokta yazılır (Örnek 1: `-2, 5, -2, 5, …`), "infinite loop?" uyarısı |

## A ve B grubunun karşılaştırması

| Beceri | A: Doğru | B: Parabol |
| --- | --- | --- |
| Döngü | Ödevdeki başlangıç / bitiş / adım | Aynı |
| Girdi hücreleri | 5 | 6 |
| y hesabı | `a·x + b`, akümülatördeki x ile doğrudan (`mul a`, `add b`) | `(a·x + b)·x + c`, `load a` ile yeniden başlanır |
| Tuzak | Negatif adımda bitiş testi, `print`'ten sonra akümülatörde x'in kaldığını görmek | Aynı bitiş testi, x²'yi kurmak, `b·x` yerine `b` eklemek |
| Çözüm uzunluğu | 16 komut, 6 hücre | 19 komut, 7 hücre |

İki soru aynı döngüyü ister. Fark yalnızca y'nin hesabındadır: B'de 3 komut daha var ve formül daha kolay yanlış kurulur. Asıl iş iki grupta da döngü olduğu için iki soru yaklaşık aynı zorluktadır.
