# Oyuncak Makine Uygulama Sınavı: A Grubu

**Süre:** 60 dakika

Programınızı [Oyuncak Makine](https://toy.emrecan.dev/) simülatöründe yazınız ve çalıştırınız. Programınızı sadece verilen örneklerle değil, kendi seçtiğiniz değerlerle de deneyiniz.

**Hatırlatma**

- Komut satırları en az bir boşlukla başlar. Sadece etiketler (`basla`, `bitir` gibi) satırın en başından yazılır. Satır başında sekme (Tab) değil, boşluk kullanınız.
- Programınızı `stop` komutuyla bitirmeyi unutmayınız.
- Bellek hücrelerini programın sonunda tanımlayınız, örneğin `toplam  0`.
- Açıklama yazmak isterseniz komutun sonuna, aynı satıra yazınız (`load x   # x'i getir`). `#` ile başlayan ayrı bir satır kodun arasında kalırsa simülatör o satırda takılır. Ayrı açıklama satırları yalnızca `stop`'tan sonra kullanılabilir.

---

## Soru: Doğrunun değer tablosu

**Durum:** Bir doğrunun denklemi `y = a·x + b` şeklindedir. Doğruyu çizmek için önce bir değer tablosu hazırlanır: x belli bir aralıkta adım adım değiştirilir ve her x için y hesaplanır.

Ödevdeki gibi x bir **başlangıç** değerinden başlar, her seferinde **adım** kadar değişir ve **bitiş** değeri geçilince durur. Adım pozitif ya da negatif olabilir.

![Her x için önce x, sonra y = a·x + b yazılır](hafta-4-gorseller/hafta4-a-dogru.png)

**Girdiler bellekte:** Bu soruda sayılar klavyeden okunmaz (`get` kullanılmaz). Programınızın sonunda beş **girdi hücresi** tam bu adlarla bulunmalıdır. Hücreler, sayfanın sonundaki **Kod taslağı**'nda hazır yazılıdır.

**Görev:** Her x için önce **x'i**, sonra **y'yi** ekrana yazınız. Bitiş geçilince program dursun.

**Değişken adları:** Program çalışırken değişen o anki x değeri için ayrı bir hücre tanımlayınız ve ona ne tuttuğunu anlatan bir ad veriniz. Girdi hücrelerinin değerlerini program içinde değiştirmeyiniz. Örneğin x, `baslangic`'ın bir kopyasıyla başlamalı, `baslangic` hücresi bozulmamalıdır.

**Örnek**

| a | b | baslangic | bitis | adim | Ekrana yazılanlar |
| --- | --- | --- | --- | --- | --- |
| `2` | `-3` | `-1` | `4` | `1` | `-1, -5, 0, -3, 1, -1, 2, 1, 3, 3, 4, 5` |
| `-1` | `4` | `6` | `-3` | `-2` | `6, -2, 4, 0, 2, 2, 0, 4, -2, 6` |

1. örnekte (x, y) çiftleri `(-1, -5)`, `(0, -3)`, `(1, -1)`, `(2, 1)`, `(3, 3)`, `(4, 5)`.

**Açıklamalar**

- Değerler doğru verilir. Adım pozitifse başlangıç bitişten büyük olmaz. Adım negatifse başlangıç bitişten küçük olmaz. Adım 0 olmaz.
- Bitiş değerine tam denk gelinirse o x de yazılır (1. örnek). Denk gelinmezse bitişi geçen x yazılmaz (2. örnekte −3 atlanır, −4 yazılmaz).
- Hücre adını yanlış yazarsanız (örneğin `baslangıc`) simülatör "there is no statement labeled …" uyarısı verir ve ekrana `NaN` yazar. Bu uyarıyı görürseniz önce adları kontrol ediniz.

## Kod taslağı

Aşağıdaki taslağı kopyalayıp simülatöre yapıştırınız. Girdi hücrelerinde 1. örneğin değerleri hazır yazılıdır.

```
BU SATIRI SİL, KOMUTLARINI YAZ
a         2
b         -3
baslangic -1
bitis     4
adim      1
```

**Önemli:** İlk satırı (`BU SATIRI SİL, KOMUTLARINI YAZ`) silip yerine komutlarınızı yazınız. Satırı silmeyi unutursanız simülatör `invalid opcode 'satiri' at 1` hatası verir ve durur.

Programınızı başka değerlerle denemek için yalnızca girdi hücrelerindeki sayıları değiştirip yeniden **Çalıştır**'a basınız.

## Kendiniz deneyiniz

Girdi hücrelerine aşağıdaki değerleri yazıp programınızı çalıştırınız. Her deneme 5 nokta (x, y) yazdırır. Ekranda sırasıyla her noktanın x'i ve y'si görünmelidir.

| # | Doğru | a | b | baslangic | bitis | adim | Yazılması gereken noktalar (x, y) | Neyi dener |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | y = 3x − 2 | `3` | `-2` | `0` | `4` | `1` | (0, −2) (1, 1) (2, 4) (3, 7) (4, 10) | Artan adım, bitişe tam denk gelme |
| 2 | y = x + 1 | `1` | `1` | `5` | `-4` | `-2` | (5, 6) (3, 4) (1, 2) (−1, 0) (−3, −2) | Negatif adım, bitiş −4 atlanır |
| 3 | y = −2x + 6 | `-2` | `6` | `-1` | `7` | `2` | (−1, 8) (1, 4) (3, 0) (5, −4) (7, −8) | Azalan doğru, adım 2 |
| 4 | y = 4 | `0` | `4` | `10` | `-2` | `-3` | (10, 4) (7, 4) (4, 4) (1, 4) (−2, 4) | a = 0: yatay doğru, adım −3 |
| 5 | y = −x | `-1` | `0` | `-6` | `6` | `3` | (−6, 6) (−3, 3) (0, 0) (3, −3) (6, −6) | b = 0: doğru başlangıç noktasından geçer |

Örneğin 1. denemede ekranda sırasıyla `0, -2, 1, 1, 2, 4, 3, 7, 4, 10` görünmelidir.
