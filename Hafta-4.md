# Oyuncak Makine Ödevi: Hafta 4

Programınızı [Oyuncak Makine](https://toy.emrecan.dev/) simülatöründe yazınız ve çalıştırınız. Programınızı sadece verilen örneklerle değil, kendi seçtiğiniz girdilerle de deneyiniz.

**Hatırlatma**

- Komut satırları en az bir boşlukla başlar. Sadece etiketler (`basla`, `bitir` gibi) satırın en başından yazılır.
- Programınızı `stop` komutuyla bitirmeyi unutmayınız.
- Bellek hücrelerini programın sonunda tanımlayınız, örneğin `toplam  0`.

---

## Soru: Adım adım sayma

**Görev:** Kullanıcıdan sırasıyla üç sayı okuyunuz:

1. Başlangıç değeri
2. Bitiş değeri
3. Adım değeri (pozitif ya da negatif olabilir)

Başlangıç değerinden başlayarak her seferinde adım değerini ekleyiniz ve elde ettiğiniz her değeri ekrana yazınız. Bitiş değeri geçildiğinde program dursun.

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

Adım pozitifken "bitiş geçildi mi?" sorusu ile adım negatifken sorulan soru aynı değildir. Önce adımın işaretine bakınız.

---

## Kendiniz çözünüz

1. **Basketbol maçı:** Bir oyuncunun attığı 3'lük, 2'lik ve 1'lik (serbest atış) sayısını okuyup toplam kaç sayı attığını yazınız.
   `4, 5, 3` → `25`

2. **Çarpım tablosu:** Girilen sayının 1'den 10'a kadar katlarını sırayla ekrana yazınız.
   `7` → `7, 14, 21, 28, 35, 42, 49, 56, 63, 70`

3. **Telefon şarjı:** Şarjı her saat 15 puan azalan bir telefonun, girilen şarj yüzdesiyle kaç tam saat dayanacağını döngüyle bulunuz.
   `100` → `6`

4. **Zar atışı:** 0 girilene kadar atılan zar sonuçlarını okuyup kaç tanesinin çift geldiğini yazınız.
   `3, 6, 2, 5, 4, 0` → `3`

5. **Donan günler:** Önce gün sayısını, sonra o günlerin sıcaklıklarını okuyup sıcaklığın kaç gün sıfırın altında olduğunu yazınız.
   `5, -3, 4, -1, 0, 7` → `2`

6. **Yarışma birincisi:** 0 girilene kadar yarışmacıların puanlarını okuyup en yüksek puanı yazınız.
   `45, 78, 62, 0` → `78`

7. **Bakteri kolonisi:** Her saat ikiye katlanan bir bakteri kolonisinin, girilen başlangıç sayısından kaç saat sonra 1000'i geçeceğini yazınız.
   `1` → `10`  ·  `100` → `4`

8. **Video süresi:** Saniye cinsinden girilen bir video süresini saat, dakika ve saniye olarak yazınız.
   `3725` → `1`, `2`, `5`

9. **Rakamlar toplamı:** Girilen bir sayının rakamlarının toplamını yazınız.
   `2026` → `10`
