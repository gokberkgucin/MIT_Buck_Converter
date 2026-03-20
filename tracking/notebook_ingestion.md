# Notebook Ingestion

Bu dosya, fiziksel defter notlarinin dijitale alinmasi ve `yeni.md` icine secici olarak entegrasyonu icin kullanilir.

Durum:
- `yeni.odt -> yeni.md` aktarimi tamamlandi.
- Aktif faz artik defter notlarinin dijitale alinmasi ve secici entegrasyonudur.
- Ancak gorsel kullanimi acisindan ilk secim kaynagi `/foto` degil, `yeni.odt` icindeki gomulu gorseller olacaktir.
- Bu gorseller `images/odt_embedded/` altina cikartilip `yeni.md` icinde kullanima alinmistir.

## Temel akýþ

1. Ham foto `foto/defter_raw/` altina eklenir.
2. Gerekirse duzeltilmis kopya `foto/defter_reviewed/` altina alinir.
3. Batch kaydi `tracking/notebook_batches.md` icine eklenir.
4. Her batch icin okunan icerik once ham transkripsiyon mantigiyla ozetlenir.
5. Kullanilmaya deger teknik kisimlar `yeni.md` icindeki ilgili bolume tasinir.
6. Belirsiz kisimlar `tracking/open_questions.md` icine ya da batch notuna dusulur.

Ek belgeler:
- Cekim kurallari icin `tracking/defter_foto_cekim_kilavuzu.md`
- Batch kaydi acmak icin `tracking/notebook_batch_template.md`

## Batch boyutu

Onerilen calisma sekli:
- Bir seferde `5-10` foto
- Tek konu agirlikli batch tercih et
- Ornek: sadece bootstrap, sadece input capacitor, sadece EMI gibi

## Transkripsiyon kurallari

- Okunani oldugu gibi not et; erken duzeltme yapma.
- Emin olunmayan ifade ve sayilari `uncertain` diye isaretle.
- Birim eksikse tahmin edip gizlice duzeltme; eksigi acik yaz.
- Ayni hesap birden fazla sayfada tekrar ediyorsa once tekrar olarak isaretle, sonra tek temiz versiyona indir.
- Foto uzerindeki her bilgi `yeni.md` icine girmek zorunda degil.

## Her foto icin tutulacak minimum bilgi

- kaynak dosya adi
- konu tahmini
- okunabilen ana formuller / sayilar
- belirsiz kisimlar
- `yeni.md` icindeki olasi hedef bolum
- durum: `used`, `hold`, `reject`

## Entegrasyon kurali

Bir defter notu `yeni.md` icine ancak su durumda alinmali:
- tasarim kararini etkiliyorsa
- hesap veya secim gerekcesi tasiyorsa
- tekrar degilse
- okunusundan yeterince eminsen

Aksi halde ham kayitta kalabilir.

## Tamamlama olcutu

Defter fazi ancak su durumda tamamlanmis sayilacak:
- ilgili defter fotograflari batch'lere ayrildiysa
- kullanilan teknik notlar `yeni.md` icine tasindiysa
- kalan belirsizlikler `open_questions.md` veya batch kayitlarinda acikca yazildiysa



