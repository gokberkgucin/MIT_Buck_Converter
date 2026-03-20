# Defter Foto Cekim Kilavuzu

Bu dosya, fiziksel defter notlarini daha sonra `yeni.md` icine aktarabilmek icin cekilecek fotograflarin nasil hazirlanacagini ozetler.

## Amac

Hedef, fotograflari tek tek tekrar duzeltmek zorunda kalmadan okunabilir bir ham arsiv olusturmaktir.

## Onerilen dosya adlandirma

Onerilen format:
- `NB_YYYYMMDD_konu_p01.jpg`
- `NB_YYYYMMDD_konu_p02.jpg`

Ornekler:
- `NB_20260320_bootstrap_p01.jpg`
- `NB_20260320_input_caps_p02.jpg`
- `NB_20260320_emi_notes_p03.jpg`

## Cekim kurallari

- Sayfa tamamen kadraja girmeli.
- Perspektif egriyse mumkunse yeniden cekilmeli.
- Golge, parlama ve bulaniklik en aza indirilmeli.
- Ayni sayfa icin gerekirse bir yakin, bir tam sayfa cekimi alinabilir.
- Kirmizi, mavi veya kursun kalem farki kayboluyorsa daha iyi isikla tekrar cekilmeli.
- Cok uzun sayfalar iki parcaya bolunebilir ama isimde sira korunmali.

## Batch mantigi

Onerilen batch boyutu:
- `5-10` foto
- tek konu agirlikli

Ornek batch'ler:
- sadece bootstrap notlari
- sadece giris kapasiteleri
- sadece kompanzasyon
- sadece EMI ve yerlesim

## Kaydetme akisi

1. Ham foto dosyalarini `foto/defter_raw/` altina koy.
2. Gerekirse duzeltilmis kopyalari `foto/defter_reviewed/` altina ayir.
3. Yeni batch icin `tracking/notebook_batches.md` kaydini ac.
4. Gerekirse `tracking/notebook_batch_template.md` kopyalanarak batch notu olustur.

## Ne yapmiyoruz

- Bu asamada her fotografi otomatik olarak `yeni.md` icine almiyoruz.
- Tekrar eden veya okunmayan notlari zorla temiz bilgiye cevirmiyoruz.
- `/foto` arsivindeki eski karisik gorselleri ilk kaynak gibi kullanmiyoruz.
