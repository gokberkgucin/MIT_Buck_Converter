# Master Plan

Bu dosya, yeni buck converter tasariminin baslangicta kabul edilen ana yol haritasidir.

## Durum

- `Faz 1` tamamlandi: `yeni.odt -> yeni.md` aktarimi kapandi.
- Aktif asama `Faz 2`: defter notlarinin dijitale alinmasi.

## Temel karar

Ilerleme sirasi su sekilde olacak:
1. `yeni.odt` icerigini bolum bolum `yeni.md` dosyasina tasimak
2. Defter notlari ve hesaplamalari foto ile dijitale aktarmak
3. Defter icerigini secerek ve temizleyerek `yeni.md` yapisina dahil etmek
4. Tumu aktarildiktan sonra belgeyi butuncul sekilde sadeleştirmek, duzeltmek ve kaynaklari toparlamak
5. En son asamada tasarimi MATLAB, LTspice ve gerekiyorsa OrCAD/PSpice ile dogrulamak
6. Dogrulama sonucuna gore hesaplari ve tasarim kararlarini revize etmek

Bu sira bilerek secildi. Amaç, ham notlar tam aktarilmadan erken optimizasyona gitmemek ve teorik/simülasyon revizyonunu en sona birakmaktir.

## Neden iyi bir plan?

Bu yaklasim su riskleri azaltir:
- belge daginikliginin teknik hata ile karismasi
- ayni hesabin birden fazla yerde farkli versiyonla kalmasi
- simülasyona erken gecip eksik varsayimlarla vakit kaybetme
- defterde kalan degerli notlarin unutulmasi

## Fazlar

### Faz 1 - ODT'den Markdown'a kontrollu aktarim

Amac:
- `yeni.odt` icindeki mevcut dijital icerigi kaybetmeden GitHub uyumlu ana belgeye tasimak

Kurallar:
- Kopyala-yapistir yerine temizleyerek aktar
- Kırık sekil ve denklem referanslarini aynen tasima
- Emin olunmayan ifadeleri `TODO`, `CHECK`, `SOURCE` gibi notlarla isaretle
- `yeni.md` icine sadece okunabilir ve takip edilebilir icerik girsin

Bitis kriteri:
- `yeni.odt` icindeki teknik icerigin ana omurgasi `yeni.md` icinde bulunuyor olacak

### Faz 2 - Defter notlarinin dijitale alinmasi

Amac:
- Fiziksel defterde kalan hesap, cizim, grafik ve kararlarin kaybolmadan dijitale alinmasi

Kurallar:
- Fotolari parca parca cek
- Her foto icin tarih veya konu bazli isim kullan
- Ham icerigi once arsivle, sonra metne cevir
- Emin olunmayan hesap veya okumalari `uncertain` diye isaretle

Bitis kriteri:
- Defterin ilgili kisimlari dijital arsive alinmis olacak

### Faz 3 - Defter iceriginin secici entegrasyonu

Amac:
- Defter notlari icinden tasarim icin gerekli kisimlari `yeni.md`'ye yerlestirmek

Kurallar:
- Her ham not `yeni.md`'ye girmek zorunda degil
- Ayni konuya ait tekrarli hesaplar tek temiz versiyonda birlestirilsin
- Gerekirse ham notlar `tracking/` altinda ozetlenerek tutulabilir

Bitis kriteri:
- Tasarimla ilgili esas notlar, kararlar ve hesaplar tek ana belgede izlenebilir olacak

### Faz 4 - Butuncul belge revizyonu

Amac:
- Taslagi muhendislik dokumanina donusturmek

Yapilacaklar:
- Fazlaliklari at
- Tekrarlari birlestir
- Yanlis veya zayif ifadeleri duzelt
- Birimleri, degisken adlarini ve sembolleri tutarli hale getir
- Kaynakca ve atiflari temizle
- Gorselleri sec ve duzenle

Bitis kriteri:
- `yeni.md` daha temiz, daha tutarli ve kaynaklari izlenebilir bir belge haline gelecek

### Faz 5 - Tasarimin analitik ve simülasyon dogrulamasi

Amac:
- Belgedeki hesaplarin ve kararlarin sadece anlatim degil, teknik olarak da saglam oldugunu gostermek

Araclar:
- MATLAB
- LTspice
- Gerekiyorsa OrCAD/PSpice

Kontroller:
- steady-state
- startup
- load transient
- line transient
- ripple
- loop / kompanzasyon
- EMI ve giris filtresi etkisi
- gerekli ise termal ve kayip analizi

Bitis kriteri:
- Ana tasarim kararlarinin simulasyon veya hesap ile desteklenmis olmasi

### Faz 6 - Revizyon dongusu

Amac:
- Dogrulama sonuclarina gore tasarimi olgunlastirmak

Kurallar:
- Sorun cikarsa once varsayimi bul
- Sonra ilgili hesabi revize et
- Ardindan belgeyi ve simulasyonu birlikte guncelle
- Nihai sonuc belgede ve hesapta ayni gercegi gostermeli

## Calisma prensipleri

- `yeni.md` ana belge olacak
- `foto/` arsiv olarak kalabilir
- `images/` sadece belgede kullanilan secilmis gorseller icin kullanilacak
- `references/used_sources.md` sadece gercekten kullanilan veya kullanilacak kaynaklari izleyecek
- `tracking/open_questions.md` teknik belirsizlikleri tutacak
- Erken asamada kusursuz anlatim degil, izlenebilir dogruluk onceliklidir

## Ilk uygulama karari

Bu planin ilk fiili adimi olarak once `Giris` ve genel tasarim yaklasimi `yeni.md` dosyasina aktarilacak, sonra diger bolumlere sirayla gecilecektir.

