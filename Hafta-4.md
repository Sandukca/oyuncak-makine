# Oyuncak Makine Ödevi: Hafta 4

Programını [Oyuncak Makine](https://toy.emrecan.dev/) simülatöründe yaz ve çalıştır. Programını sadece verilen örneklerle değil, kendi seçtiğin girdilerle de dene.

**Hatırlatma**

- Komut satırları en az bir boşlukla başlar. Sadece etiketler (`basla`, `bitir` gibi) satırın en başından yazılır.
- Programını `stop` komutuyla bitirmeyi unutma.
- Bellek hücrelerini programın sonunda tanımla, örneğin `toplam  0`.

---

## Soru: Adım adım sayma

**Görev:** Kullanıcıdan sırasıyla üç sayı oku:

1. Başlangıç değeri
2. Bitiş değeri
3. Adım değeri (pozitif ya da negatif olabilir)

Başlangıç değerinden başlayarak her seferinde adım değerini ekle ve elde ettiğin her değeri ekrana yaz. Bitiş değerini geçince dur.

**Örnek**

| Başlangıç | Bitiş | Adım | Ekrana yazılanlar |
| --- | --- | --- | --- |
| `1` | `10` | `3` | `1`, `4`, `7`, `10` |
| `2` | `9` | `2` | `2`, `4`, `6`, `8` |
| `10` | `1` | `-3` | `10`, `7`, `4`, `1` |
| `0` | `-7` | `-2` | `0`, `-2`, `-4`, `-6` |
| `5` | `5` | `1` | `5` |

**Açıklamalar**

- Adım değeri 0 olmayacaktır.
- Adım pozitifse başlangıç bitişten büyük olmayacak, adım negatifse başlangıç bitişten küçük olmayacaktır.
- Bitiş değerine tam denk gelinirse o değer de yazılır (1. ve 3. örnek). Denk gelinmezse bitişi geçen değer yazılmaz (2. ve 4. örnek).

**İpucu**

Adım pozitifken "bitişi geçtim mi?" sorusu ile adım negatifken sorulan soru aynı değildir. Önce adımın işaretine bak.
