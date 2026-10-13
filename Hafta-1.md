[Oyuncak Makine](https://toy.emrecan.dev/)

## 1. Üs alma — $a^b$

Klavyeden önce taban, sonra üs alınır. Çarpma işlemi döngü içerisinde tekrarlanır. Üs negatif olmayan bir tam sayı olmalıdır.

```

basla   get
        store taban
        get
        store us
        load us
        ifneg hata
        load 1
        store sonuc

dongu   load us
        ifzero bitir
        load sonuc
        mul taban
        store sonuc
        load us
        sub 1
        store us
        goto dongu

bitir   load sonuc
        print
        stop

hata    load -1
        print
        stop

taban   0
us      0
sonuc   1

```

Örnek: Taban `2`, üs `5` girilirse sonuç `32` olur. `0` üssü için sonuç `1` üretilir. Negatif üslerde hata göstergesi olarak `-1` yazdırılır.

## 2. Faktöriyel — $n!$

Klavyeden negatif olmayan bir tam sayı alınır ve $n \times (n-1) \times \cdots \times 1$ hesaplanır.

```

basla   get
        store n
        load n
        ifneg hata
        load 1
        store sonuc

dongu   load n
        ifzero bitir
        load sonuc
        mul n
        store sonuc
        load n
        sub 1
        store n
        goto dongu

bitir   load sonuc
        print
        stop

hata    load -1
        print
        stop

n       0
sonuc   1

```

Örnek: `5` girilirse `120` yazdırılır. `0! = 1` durumu da desteklenir.

## 3. Fibonacci — İlk N terimi yazdırma

Klavyeden kaç terim istendiği alınır. Fibonacci dizisi `0, 1, 1, 2, 3, 5, 8, ...` biçiminde başlar.

```

basla   get
        store n
        load n
        ifneg hata
        load 0
        store a
        load 1
        store b

dongu   load n
        ifzero bitir
        load a
        print
        load a
        add b
        store yeni
        load b
        store a
        load yeni
        store b
        load n
        sub 1
        store n
        goto dongu

bitir   stop

hata    load -1
        print
        stop

n       0
a       0
b       1
yeni    0

```

Örnek: `8` girildiğinde aşağıdaki değerler sırayla yazdırılır:

`0, 1, 1, 2, 3, 5, 8, 13`

## Algoritmaların karşılaştırması

| Algoritma  | Kullanılan temel komutlar | Döngü sayısı    |
| ---------- | ------------------------- | --------------- |
| Üs alma    | `mul`, `sub`, `ifzero`    | Üs değeri kadar |
| Faktöriyel | `mul`, `sub`, `ifzero`    | N kadar         |
| Fibonacci  | `add`, `sub`, `ifzero`    | N kadar         |


Fibonacci örneği daha öğretici; çünkü `a`, `b` ve `yeni` değişkenleriyle ara sonuçların nasıl saklandığını gösteriyor.
