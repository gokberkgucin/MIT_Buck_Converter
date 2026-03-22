# LM5146-Q1 Tabanli Buck Converter Tasarimi



Bu dosya tez formati icin degil, GitHub uzerinden surdurulen yasayan tasarim dokumani icin tutulur.



Ana yol haritasi: `tracking/master_plan.md`



Kurallar:

- Yalnizca secilen ve gercekten kullanilan kaynaklari buraya tasiyin.

- Ham notlari buraya oldugu gibi yigmayin; dogrulanmis ve okunabilir hale getirin.

- Kullanilacak gorselleri mumkunse `images/` klasorune kopyalayip temiz isimlerle cagirin.



## 0. Calisma Plani



Bu belge icin kabul edilen ana sira soyledir:

1. `yeni.odt` icerigini bolum bolum `yeni.md` dosyasina aktarmak

2. Defter notlarini ve hesaplamalari foto ile dijitale almak

3. Defter icerigini secerek ve temizleyerek ana belgeye tasimak

4. Tum belgeyi butuncul sekilde gozden gecirip sadeleştirmek, duzeltmek ve kaynaklari toparlamak

5. En son asamada MATLAB, LTspice ve gerekiyorsa OrCAD/PSpice ile teknik dogrulama yapmak

6. Sorun bulunursa hesaplari ve tasarimi revize edip belgeyi guncellemek



Ayrintili surum: `tracking/master_plan.md`



Durum:

- `yeni.odt -> yeni.md` aktarimi tamamlandi.

- Siradaki aktif faz: defter notlarinin `foto/defter_raw/` uzerinden batch batch dijitale alinmasi.

- Takip dosyalari: `tracking/notebook_ingestion.md`, `tracking/notebook_batches.md`



## 1. Arka Plan



Ilk donemdeki tasarim, onceki tez calismasinda tamamlanan ve simulasyonla hedef spesifikasyonlari sagladigi gosterilen buck converter tasarimidir. Yeni calisma ise bu ilk tasarimin uzerine kurulan, daha ayrintili hesaplar, ek teknik gereksinimler ve daha bilincli komponent secimleri iceren ikinci tasarim iterasyonudur.



Bu ikinci iterasyonda sadece mevcut devreyi tekrar etmek yerine, tasarim kararlarini daha guclu kaynaklarla ve daha sistematik hesaplarla desteklemek hedeflenmektedir. Ozellikle kontrolcu tasarimi, kapasitör secimi, giris filtresi, EMI, MOSFET kayiplari ve bootstrap davranisi daha derin ele alinacaktir.



## 2. Durum Ozeti



### 2.1 Ilk tasarim



Ilk tasarim tamamlanmis bir referans noktasi olarak kabul edilir:

- simulasyonlari alinmistir

- temel spesifikasyonlari sagladigi gosterilmistir

- ilk donem raporunda anlatilmistir



Bu nedenle ilk tasarim, yeni calisma icin hem karsilastirma tabani hem de dogrulanmis baslangic noktasi olarak degerlendirilir.



### 2.2 Yeni tasarim



Yeni tasarim daha ileri bir noktayi hedeflemektedir:

- ek spesifikasyonlar tanimlanmistir

- ayrintili hesaplamalar yapilmistir

- notlarin bir kismi dijital dosyalarda, bir kismi fiziksel defterde kalmistir

- simulasyon dogrulamasi henuz tamamlanmamistir



Bu nedenle mevcut durum, "tasarim dusuncesi olgun, belge ve dogrulama yarim" seklinde ozetlenebilir.



### 2.3 Tasarim yaklasimindaki degisim



`yeni.odt` taslagina gore ilk donemde kontrolcu tasarimi daha once ogrenilen yaklasimla kurulmustu. Yeni iterasyonda ise Can Hoca'dan ogrenilen K-factor yonteminin uygulanmasi hedeflenmistir. Bu degisim, yalnizca kompanzasyon devresini degil, tum tasarim mantigini daha sistematik hale getirme niyetini gostermektedir.



Ayni zamanda bu iterasyonda su basliklara daha fazla agirlik verilmistir:

- LM5146-Q1 ve EVM referanslarinin ayrintili incelenmesi

- dusuk EMI tasarim yaklasimi

- giris ve cikis kapasitelerinin daha bilincli secimi

- pratik komponent ve yerlesim kararlarinin gerekcelendirilmesi



## 3. Tasarim Hedefleri



Bu bolumu tek bir dogruluk kaynagi olarak kullan.



| Parametre | Hedef / Sinir | Not |

| --- | --- | --- |

| Giris gerilimi | `24 V - 36 V` | ana calisma araligi |

| Giris gerilimi transient dayanim | `44 V`, `1 ms` | hayatta kalma siniri, bu aralikta regule calisma zorunlu degil |

| Cikis gerilimi, statik | `14 V +- 3%` | ana setpoint |

| Cikis gerilimi, transient | `14 V +- 20%` | minimum ve maksimum yuk adimlari sirasinda |

| Cikis gucu | `50 W - 125 W` | rezistif yuk |

| Cikis akimi, turetilmis | `3.571 A - 8.929 A` | $I_{out} = \dfrac{P_{out}}{V_{out}}$ |

| Yuk adimi | `3.571 A -> 9 A` | taslakta `5.429 A` load-step olarak not edilmis |

| Anahtarlama frekansi | `332 kHz` | mevcut tasarim secimi, orijinal zorunlu spec degil |

| Izin verilen `Vout` ripple | `100 mVpp` | herhangi bir `Rload` icin |

| Izin verilen `Vin` ripple | `0.24 Vpp` | giris gerilimi peak-to-peak |

| Izin verilen `Vin` transient | `0.36 V` | undershoot veya overshoot limiti |

| Izin verilen giris akimi ripple | `50 mApp` | ideal kaynak varsayimi |

| Minimum verim | `90%` | gerilim ve yuk boyunca |

| Sicaklik hedefi | `76 degC` board worst-case | taslak spec notu |

| EMI hedefi | `CISPR 25 Class 5` | `pi-stage` giris EMI filtresi ve elektrolitik parallel damping hedefi |

| Harici bias/VCC | kullanilmayacak | harici bias secenegi bulunuyor ama bu iterasyonda kullanilmiyor |



## 4. Kullanilan Ana Kaynaklar



Gercek kaynak listesi icin `references/used_sources.md` dosyasini guncelle.

PDF kaynaklari bu belge icinde tiklanabilir olacaksa, dosyalari `references/pdfs/` altinda tut.



Su an icin cekirdek adaylar:

- `G94_SPEC.txt`

- `G31_Robert_Erikson_fundamentals-of-power-electronics-3n_2020.txt`

- [G32_lm5146-q1.pdf](./references/pdfs/G32_lm5146-q1.pdf)

- [G35_Basic Calculation of a Buck Converter's Power Stage (Rev. B).pdf](./references/pdfs/G35_Basic%20Calculation%20of%20a%20Buck%20Converter's%20Power%20Stage%20(Rev.%20B).pdf)

- [G46_LM5146-Q1EVM User's Guide.pdf](./references/pdfs/G46_LM5146-Q1EVM%20User's%20Guide.pdf)

- [G101_The K Factor.pdf](./references/pdfs/G101_The%20K%20Factor.pdf)

- `G103_comp.txt`

- `G95_04_K_FACTOR_CAN_HOCA.txt`

- [G114_Simple Solution for Input Filter Stability Issue in DC_DC_slua929a.pdf](./references/pdfs/G114_Simple%20Solution%20for%20Input%20Filter%20Stability%20Issue%20in%20DC_DC_slua929a.pdf)

- [G112_Simple Success with Conducted EMI and Radiated EMI_snva755.pdf](./references/pdfs/G112_Simple%20Success%20with%20Conducted%20EMI%20and%20Radiated%20EMI_snva755.pdf)

- [G113_An Engineer’s Guide to Low EMI in DC_DC Regulators_slyy208.pdf](./references/pdfs/G113_An%20Engineer%E2%80%99s%20Guide%20to%20Low%20EMI%20in%20DC_DC%20Regulators_slyy208.pdf)



## 5. Guc Katinin Tasarimi



### 5.1 Topoloji ve temel varsayimlar



#### 5.1.1 LM5146-Q1 dahili LDO siniri


ODT'den aktarilan metin (`4.2. Giriş Gerilimi Vin`):

> Lm’nin içinde bir LDO (Low Dropout Regulator) barındırır. Bu LDO, 7.5V Vcc beslemesi sağlar, Mosfet kapı sürücülerini ve kontrol devrelerini besler.

![Giris gerilimi / dahili LDO civari](images/odt_embedded/fig_02_vin_ldo_vcc.png)

> VVCC-LDO VIN to VCC dropout voltage = 0.25V(typ) - 0.72V(max)
>
> Harici Vcc kullanılmayacaksa, LDO’nun düzgün çalışabilmesi için Giriş gerilimi Vin’in en az 7.5 V + 0.72V(max) = 8.22V olmalıdır.
>
> Yani Vin > 7.5V + dropout voltage olursa, Vcc gerilimi 7.5V sabitlenir. Vin = 9V iken de Vin = 50 V iken de Vcc = 7.5V olur. :*
>
> Eğer Vin < 7.5V + dropout voltage olursa
> LDO regulatorü, 7.5V’u çıkaramaz. Denklem geçerlidir.
> Örneğin giriş gerilimi Vin = 6V olsun.
> Vcc gerilimi en iyi ihtimalle 6V-0.25V= 5.75 V olur.
> Vcc gerilimi, Vin’i takip eder.
> Bu düşük Vcc gerilimi gate sürücüleri için yeterli olmayabilir, mosfetler tam açılmaz, Rdson artar (İleride göreceğiz), verim düşer.
>
> Özetle, Vin < 7,5 V çalıştığınızda:
>
> Regülatör Dropout’u
>
> LDO’nun giriş–çıkış arasındaki “dropout” gerilimi devreye girer. Örneğin, Vin = 6 V iken LDO çıkışı 5,8 V’a düşer.
>
> Artan İletim Kayıpları
>
> Düşük VGS → MOSFET kanalı tam açılmaz → RDS(on) ↑ → iletim kayıpları (I²·R) artar.
>
> Aynı zamanda aşırı akım koruması (OCP) voltaj ölçümü yanıltabilir.
>
> Artan Anahtarlama Kayıpları
>
> Düşük gate gerilimi → gate şarj/deşarjı yavaşlar → geçiş süreleri uzar → switching kayıpları ↑.
>
> MOSFET Seçim Kısıtı
>
> Standart tipler yerine “logic-level” (RDS(on) @ VGS = 4,5 V garantili) MOSFET kullanmak gerekir.
>
> Çözüm Önerisi
>
> Vin ≥ 7,5 V sağlamak veya 8 – 13 V’luk harici VCC rail’ı Şekil 8-2’deki gibi DVCC ile bağlayarak LDO’yu bay-pas etmek, hem verimi hem termal performansı iyileştirir.





#### 5.1.2 Ek notlar: dusuk `Vin` bolgesinin etkisi



Bu ODT parcasi, dahili LDO'nun tam regulasyon ve dropout bolgeleri arasindaki tasarim farkini netlestiriyor. GitHub uyumlu matematik bicimiyle bu sinirlar su sekilde yazilabilir:

```math
V_{in,\min} = 7.5\,\text{V} + 0.72\,\text{V} = 8.22\,\text{V}
```

```math
V_{CC} \approx V_{in} - V_{dropout}
```

Ornegin tipik dropout ile $V_{in} = 6\,\text{V}$ icin:

```math
V_{CC} \approx 6.00\,\text{V} - 0.25\,\text{V} = 5.75\,\text{V}
```

Bu bolgede teknik olarak su riskler one cikar:

- $R_{DS(on)} \approx 8.2\,\text{m}\Omega$
- gate sarj / desarj surelerinin uzamasi nedeniyle switching kayiplari
- koruma ve akim algilama marjlarinin degismesi
- gerekirse logic-level MOSFET veya harici `VCC` kullanim ihtiyaci

Bu nedenle tasarimda iki yol acik tutulmalidir:

- sistemi yalnizca $V_{in} \ge 8.22\,\text{V}$ bolgesinde optimize etmek
- veya `DVCC` uzerinden harici $8\,\text{V} - 13\,\text{V}$ $V_{CC}$ rayi ile LDO'yu baypas etmek



#### 5.1.3 Cikis gerilimi setpoint direncleri FB



ODT'den aktarilan metin (`4.1. Çıkış gerilimi setpoint dirençleri FB`):

> = 14V yapmak istiyoruz.  
> 0402 boyutlarında.  
> R14 Ohm R14 = 16.5 kΩ (RFB1)  
> R10 = 1.00 kΩ (RFB2)  
> Dirençleri % 0.1 tolerance’lı seçmek….



Ek not:

Bu parca, geri besleme bolucusunun `14 V` cikis hedefi icin secildigini ve fiziksel paket hedefinin `0402` oldugunu kaydediyor. GitHub uyumlu matematik bicimiyle temel setpoint iliskisi su sekilde yazilabilir:

```math
V_{out} = V_{FB}\left(1 + \frac{R_{FB1}}{R_{FB2}}\right)
```

Burada $V_{FB} = 0.8\,\text{V}$, $R_{FB1} = 16.5\,\text{k}\Omega$ ve $R_{FB2} = 1.00\,\text{k}\Omega$ alinirse:

```math
V_{out} = 0.8\left(1 + \frac{16.5}{1.0}\right) = 14.0\,\text{V}
```

Bu secim, hedef cikis gerilimi ile uyumludur. Ayrica `%0.1` toleransli direnç secimi, setpoint hatasini dusuk tutmak icin mantikli bir tercihtir.



### 5.2 Duty cycle, `tON(min)`, `tOFF(min)` ve sinir durumlari



#### 5.2.1 Birinci yaklasim ideal duty hesabi



Nominal ilk yaklasimda senkron buck donusturucu icin duty orani su sekilde alinabilir:



```math
D \approx \frac{V_{out}}{V_{in}}
```



Buna gore mevcut spesifikasyonlarla:

- $D_{\max,\text{ideal}} = \dfrac{14}{24} = 0.583$

- $D_{\min,\text{ideal}} = \dfrac{14}{36} = 0.389$



Bu degerler, nominal `24 V - 36 V` giris araliginda high-side anahtarin iletim orani icin ilk taslak sinirlari verir.



#### 5.2.2 `332 kHz` icin zaman alanina ceviri



Mevcut tasarim seciminde:



```math
f_{sw} = 332\,\text{kHz}
```



Buna gore periyot:



```math
T_{sw} = \frac{1}{f_{sw}} \approx 3.01\,\mu\text{s}
```



Ideal duty degerleri zaman alanina cevrilirse:

- $t_{ON,\max,\text{ideal}} = 0.583 \times 3.01\,\mu\text{s} \approx 1.76\,\mu\text{s}$

- $t_{ON,\min,\text{ideal}} = 0.389 \times 3.01\,\mu\text{s} \approx 1.17\,\mu\text{s}$

- $t_{OFF,\min,\text{ideal}} = (1 - 0.583) \times 3.01\,\mu\text{s} \approx 1.25\,\mu\text{s}$

- $t_{OFF,\max,\text{ideal}} = (1 - 0.389) \times 3.01\,\mu\text{s} \approx 1.84\,\mu\text{s}$



#### 5.2.3 `D_{max}` donanimsal sinirlari ve minimum on/off sureleri



ODT'den aktarilan metin (`6.1.4. Dmax,donanımsal..?`):

> LM5146-Q1’nun
> - 40-ns tON(min) for high VIN / VOUT ratio
> - 140-ns tOFF(min) for low dropout
> Main mosfet’ın iletimde olma süresi 40ns’den daha kısa olmaz.
> … Dikkat burası tam doğru olmayabilir. *** Bakarsın bir video vardı. Eklersin.
> Arada başka zamanlar da var. Ölü zaman gibi…
>
> ODT'deki denklem notasyonu bozuk aktarilmis; temiz matematik yazimi asagida verilmistir.





Ek not:

Bu parca, denetleyicinin ideal duty hesabindan ayri olarak donanimsal zaman sinirlarini hatirlatiyor. ODT notunda da isaret edildigi gibi bu hesap, olu zaman ve diger ayrintilar ihmal edilirse yalnizca ilk yaklasim olarak kullanilmalidir.

GitHub uyumlu matematik bicimiyle donanimsal ust duty siniri su sekilde yazilabilir:

```math
D_{\max,\text{donanimsal}} \approx \frac{T_{sw} - t_{OFF(\min)}}{T_{sw}}
```

```math
D_{\max,\text{donanimsal}} \approx \frac{\frac{1}{f_{sw}} - t_{OFF(\min)}}{\frac{1}{f_{sw}}}
```

$f_{sw} = 332\,\text{kHz}$ ve $t_{OFF(\min)} = 140\,\text{ns}$ icin:

```math
D_{\max,\text{donanimsal}} \approx
\frac{\frac{1}{332\,\text{kHz}} - 140\,\text{ns}}{\frac{1}{332\,\text{kHz}}}
\approx 0.953
```

Ayni yaklasimla en kucuk on-time sinirindan bir alt duty tahmini de elde edilebilir:

```math
D_{\min,\text{donanimsal}} \approx \frac{t_{ON(\min)}}{T_{sw}}
= t_{ON(\min)} f_{sw}
\approx 40\,\text{ns} \times 332\,\text{kHz}
\approx 0.013
```

Bu nedenle nominal $V_{in} = 24\,\text{V} - 36\,\text{V}$ giris gerilimi araliginda gereken duty degerleri, ilk yaklasimda bu donanimsal sinirlarin icinde gorunmektedir. Ancak olu zaman, propagation delay ve kontrolcuye ozgu diger zamanlar nihai yorumdan once ayrica dogrulanmalidir.



#### 5.2.4 Taslaktaki duty notlari ile fark



Bootstrap hesabinin icinde ayrica su duty notlari bulunuyor:

- $D_{\text{high,max}} = 0.648$

- $D_{\text{high,min}} = 0.432$

- buradan $D_{\text{low,min}} = 1 - 0.648 = 0.352$



Bu degerler ideal `Vout / Vin` yaklasimindan daha yuksek gorunmektedir. Bu fark su olasiliklardan birinden geliyor olabilir:

- kayiplar ve gercek duty ihtiyaci dahil edilmistir

- farkli bir worst-case senaryo alinmistir

- ara notlarda muhafazakar bir bootstrap varsayimi kullanilmistir

- notlarda tekrar kontrol gerektiren bir tutarsizlik vardir



Su an icin en dogru tavir, bu iki duty kumesini yan yana tutmak ve daha sonra datasheet, secilen komponentler ve gerekirse simulasyon ile tek bir nihai duty zarfina indirmektir.



Bu nedenle acik kontrol maddesi olarak su soru korunacaktir:

- Bootstrap hesabinda kullanilan `0.648 / 0.432` duty degerleri hangi varsayimdan turetildi?



### 5.3 Bobin secimi



#### 5.3.1 Cikis bobini



ODT'den aktarilan metin (`5. çıkış bobini`):

> BOŞ EKLENECEK



Ek not:

Bu alt bolum su anda placeholder olarak tutuluyor. Sonraki parcada bobin degeri, ripple gerekcesi, slew-rate iliskisi ve gerekiyorsa denklemler GitHub uyumlu matematik biciminde eklenebilir.



#### 5.3.2 Slew-rate mantigi



Taslakta `Slew Rate Hesabi` basligi altinda vurgulanan ana fikir dogrudur:

- regulator, yuk aniden arttiginda bobin akimini sonsuz hizla artiramaz

- ilk anda gereken ek akim cikis kapasitelerinden gelir

- bobin akiminin yukselme hizi birinci yaklasimda $\dfrac{dI_L}{dt} \approx \dfrac{V_L}{L}$ ile sinirlanir



Bu nedenle bobin secimi dogrudan su iki buyuklugu etkiler:

- cikis ripple akimi

- yuk adiminda gereken toparlanma suresi



Taslak notlarda ayrica gercek devrede MOSFET `RDS(on)`, bobin `DCR` ve yerlesim kaynakli kayiplarin ideal slew-rate hesabini azaltacagi da fark edilmis. Bu gozlem yerindedir ve nihai hesapta korunmalidir.



#### 5.3.3 Mevcut durum



`yeni.odt` taslaginda bobin seciminin sayisal son hali temiz bicimde yer almiyor. Buna ragmen proje klasorundeki BOM, somut bir cikis bobini adayinin daha once secildigini gosteriyor:

- `BOM/L1_Wurth_Elektronik_7443640680B_7443640680B.pdf`



Bu, bobin seciminin tamamen sifirdan acikta olmadigini; ancak sayisal gerekcesinin henuz `yeni.md` icine duzgun tasinmadigini gosteriyor.



#### 5.3.4 Bu bolume daha sonra eklenecekler



Bu bolum tamamlanirken su maddeler netlestirilecek:

- secilen `L` degeri

- secilen bobinin `DCR` degeri

- saturation akimi ve RMS akim uygunlugu

- ripple akimi hesabi

- yuk adimi altinda gereken minimum akim slew-rate ile uyumu

- secilen BOM parcasi ile hesaplarin birebir eslestirilmesi



### 5.4 Cikis kapasiteleri



#### 5.4.1 Cikis sigaçlari



ODT'den aktarilan metin (`6. çıkış sığaçları`):

> Çıkış sığaçlarının iki temel işlevi vardır.
> • Enerji depolamak: Yük geçişlerinde (load transients), ani akım taleplerine anında cevap verebilmek için, Yeteri kadar hızlı cevap veremezse artar.
> • Çıkış Impedance’sını düşürmek: Böylece hem hem de gerilimlerini düşürür.
>
> 6.1. Yük altındaki Çıkış Gerilimi Değişimleri ve Çözüm Yolları



Ek not:

Bu parca, cikis kapasitelerinin iki ana rolunu ayiriyor: transient sirasinda kisa sureli enerji tamponu olmak ve frekansa bagli cikis empedansini dusurmek. ODT kopyasinda bazi semboller eksik gorunse de teknik olarak korunmak istenen ana fikir nettir: kapasitör bankasi hem `load transient` sirasindaki gerilim sapmasini hem de ripple davranisini birlikte etkiler.



#### 5.4.2 Bu iterasyondaki tasarim karari



Taslakta mevcut yaklasim su sekilde olgunlasmis gorunuyor:

- cikista ana agirlik MLCC'ler uzerinden kurulacak

- mevcut iterasyonda cikisa bulk kapasitör eklemekten kacinilacak

- bunun temel gerekcesi, bulk kapasitörlerin cikis empedansini ve dolayisiyla kompanzasyon davranisini etkilemesidir



Bu karar, `bulk kesinlikle kullanilmaz` anlamina gelmiyor. Su anki anlami daha dar ve daha muhendislik odakli:

- eger MLCC tabanli cikis agi ripple ve transient kriterlerini sagliyorsa, cikisa ek bulk koymayip kontrol tasarimini daha sade tutmak tercih edilir

- eger daha sonra transient hedefleri sikilasirsa, cikis bulk kapasitörü yeniden degerlendirilir



#### 5.4.3 Overshoot / Undershoot gerilimi ve acik-cevrim cikis impedance


ODT'den aktarilan metin (`6.1.1. Overshoot/Undershoot Gerilimi`):

> Çıkış yük akımında büyüklüğünde bir değişim olduğunda*? Çıkış geriliminin değişim miktarı kadar.
> : Açık Çevrim Çıkış Impedance’nin crossover frequency’ındaki büyüklüğü.
>
> ( Hata: Başvuru kaynağı bulunamadı.4 )
>
> miktarını düşürmek için, crossover frequency’ındaki açık-çevrim çıkış impedance’sını düşürmemiz şarttır.
> Bunu iki yolla yapabiliriz:
> a. Zou Şekil Hata: Başvuru kaynağı bulunamadı.1’e bakarsak, C’nin artmasıyla,vssss Zout’un düşürerek.***
> b. Fc frekansını arttırarak, (tasarım felsefemiz gereği süreç ilerlerken değiştirmeyeceğiz**)



Ek not:

ODT kopyasinda formül sembollerinin bir kismi eksik gelmis gorunuyor; bu nedenle kaynak metni oldugu gibi korudum. Teknik anlam olarak burada anlatilan iliski buyuk olasilikla asagidaki sezgiye karsilik geliyor:

```math
\Delta V_{out} \approx \Delta I_{load}\,\bigl|Z_{out}(f_c)\bigr|
```

Burada:

- $\Delta I_{load}$, yuk akimindaki ani degisim
- $\Delta V_{out}$, cikis gerilimindeki overshoot veya undershoot buyuklugu
- $\lvert Z_{out}(f_c)\rvert$, crossover frekansi civarinda gorulen cikis empedansi buyuklugu

Bu nedenle bu alt bolumde korunacak iki temel tasarim fikri sunlardir:

- etkin cikis kapasitansini artirarak `Zout`'u dusurmek
- `f_c` degerini arttirarak dongunun yuk degisimine daha hizli cevap vermesini saglamak

Ancak mevcut tasarim felsefesinde `f_c` sonradan rastgele oynanacak bir parametre gibi ele alinmadigi icin, pratik agirlik daha cok kapasitör aginin sekillendirilmesine kaymaktadir.



![Acik-cevrim cikis impedance civari](images/odt_embedded/fig_01_acik_cevrim_cikis_impedance.jpg)





#### 5.4.4 Vripple ve `fsw` civarindaki `Zout`


ODT'den aktarilan metin (`6.1.2. Vripple Gerilimi`):

> Çıkış gerilimini düşürmek için, bobin akımının tepeden tepeye değerini düşürmek gerekir. Bunu da L’yi arttırarak yapabiliriz. Çıkış bobinin inductance’ı artarsa tepeden tepeye bobin akımının dalgalanması azalır. Eğer inductance çok artarsa, dalgalanmayı düşürürüz fakat, maliyet artar, fiziksel boyut artar, transient response bozulabilir.
>
> gerilimini düşürmenin daha etkili bir yolu, fsw frekansındaki çıkış impedance’sını düşürmektir. Şekil Hata: Başvuru kaynağı bulunamadı.1’a bakarsak; Zout, Daha yüksek capacitance ve daha düşük bir esr direncinden oluşursa; Capacitance eğrisi sola kayar, yatayda düz bir eğri olan esr direnci de azaldığı için daha aşağıda olur. fsw frekansında Zout‘u etkin oldüşürdüğünü görebiliriz.****?
>
```math
V_{ripple} = I_{L(P-P)} \cdot Z_{OUT}(f_{sw})
```
>
> ( Hata: Başvuru kaynağı bulunamadı.5 )



Ek not:

Bu parca, `Vripple` dusuncesini iki ayri tasarim koluna ayiriyor:

- bobin akimi dalgalanmasini azaltmak icin `L` degerini buyutmek
- anahtarlama frekansi civarinda `Zout`'u dusurmek

GitHub uyumlu matematik bicimiyle ilk sezgi su sekilde yazilabilir:

```math
\Delta I_L \propto \frac{1}{L}
```

Yani `L` arttikca bobin akiminin tepeden tepeye dalgalanmasi azalir. Ancak ODT'de dogru sezildigi gibi, bunun karsiliginda hacim, maliyet ve transient cevap hizi etkilenebilir.

Ikinci yol ise $f_{sw}$ civarindaki cikis empedansini dusurmektir. Bunun pratik anlami sunlardir:

- daha yuksek etkin kapasitans
- daha dusuk `ESR`
- uygun yerlesim ile daha dusuk parasitikler

Bu nedenle bu alt bolumde korunacak ana fikir sunudur: $V_{ripple}$ yalnizca bobin akim dalgalanmasi problemi degil, ayni zamanda $f_{sw}$ civarindaki $Z_{out}$'un sekillendirilmesi problemidir.



#### 5.4.5 Bulk kapasitör eklemenin getirdigi sifirlar


ODT'den aktarilan metin (`6.1.3. Kapasitörlerin Getirdiği Sıfırlar`):

> İki farklı kapasitör kullanırsak, MLCC’lere ek olarak bulk capacitor de kullanırsak, bulk capacitorlerin getirdiği düşük frekansta sıfırlar olacak. Yüksek frekanstaki MLCC’lere ek olarak. bulk capacitor kullanmasak mı, ŞUANDA BELLİDEĞİL
>
> G71 kaynağı, yüksek akım ve ani yük değişimi olan uygulamalarda çıkış capacitorlerinin tasarlanmasına yöntemler gösteriyor. Geleneksel yöntemlerin yetersiz olduğunu vurguluyor.
> Bizim tasarımımızda, G71’deki yöntemleri uygulamanın gerekli olup olmadığından emin değildim.



Ek not:

Bu parca, farkli kapasitör teknolojilerinin ayni cikis aginda kullanilmasinin yalnizca toplam kapasitansi degil, frekans cevabini da degistirdigini vurguluyor. Yani MLCC + bulk kombinasyonu, dusuk frekansta ek sifirlar ve yeni `ESR/ESL` etkileri getirerek `Zout` egrisinin seklini degistirebilir.

Bu nedenle burada korunacak karar mantigi sunudur:

- sadece daha fazla enerji depolamak icin bulk eklemek dogrudan dogruya dogru karar olmayabilir
- bulk ekleme karari ayni zamanda kontrol ve kompanzasyon kararidir
- G71 yonteminin gerekliligi, bu tasarimin yuk profili ve transient hedefleri ile birlikte yeniden degerlendirilmelidir



#### 5.4.6 Slew-rate ve cikis kapasitörlerinin enerji tamponu rolu


ODT'den aktarilan metin (`G71 transient yorumu`):

> G71’deki t1 anına kadar, inductor akımı yükü karşılayamadı, ancak t1 anından sonra karşılayabildi. delta_Vout un türevinin 0 (dVOUT/dt) olduğu yer aslında. Gerilim azalmasının durduğu ana karşılık geliyor bu t1 anı, aynı zamanda bu t1 anı, t_undershoot süresinin sonu anlamına da gelir.

![G71 cikis transient yontemi civari](images/odt_embedded/fig_03_g71_transient_yontemi.png)

Ani bir yuk akimi degisimi sirasinda `Vout`'un minimum noktasi ve `t1` ani.



ODT'den aktarilan metin (`Slew Rate Hesabı`):

![Slew-rate hesabi civari - 1](images/odt_embedded/fig_04_slew_rate_gecici_davranis.png)

> Bir voltage regulator’ü, ani yük değişimlerinde (Load transient) hemen cevap veremez. Çünkü çıkıi bobini, akımın oranını (Current Slew Rate) sınırlayan bir parametredir. Bobinlerin akımı ani olarak çok hızlı değişemez. **?
> Bir yükte ani değişiklik olduğunda, ilk anda gereken ekstra akımı, regulator sağlayamaz. Bu nedenle ihtiyaç duyulan fazladan akım, Çıkış sığaçlarından sağlanmak zorundadır.

![Slew-rate hesabi civari - 2](images/odt_embedded/fig_05_slew_rate_notu.jpg)

Ani bir yük akımı değişimi sırasında `Vout`…

> Feedback Control loop çıkış gerilimindeki değişimi algılar, ve mosfet duty cylcle’ını ayarlamaya başlar. Ancak duty cycle %100’e çıksa bile (Kullandığımız IC’nin donanımsal sınırından ötürü mümkün değil, Denklem Hata: Başvuru kaynağı bulunamadı.7’ten ) akımın artış hızı, Istep akımının artış hızından daha yavaştır.
>
> Istep akımı tramp süresinde artarken,
> Bobinin akımı t_undershoot süresinde artar. Düzelt a.q***
> Bobinin Tanım denkleminden yararlanarak:
>
> yerine gerilimini kullanma sebebim, Slew Rate’ı daha düşük yapar-eğimi azaltır, süresini uzatır. [1] ( Hata: Başvuru kaynağı bulunamadı.8 )
>
> Bobin akımının, akımı kadar artması bulduk.
>
> Ancak power-stage? kayıplarını (Mosfetlerin Rds(on) direnci, bobinin dcr direnci, layout direncini) göz önünde bulundurmadık. Bu kayıpları da dikkate alırsak:
>
> ( Hata: Başvuru kaynağı bulunamadı.9 )
>
> Gerçek devrede Çıkış akımının akımı kadar artış süresine diye isimlendirdik.
> süresini de gerçekçi bir değer olarak belirlemek için Evm’deki başarım eğrilerinden aldım. ??Fig.14’ten olarak belirlenmiş.
>
> olması, Bobinin akımının, yük artışını yeterince hızlı karşılayamadığını gösterir. Kalan yükleri Capacitorler karşılayacak,
>
> Evm’de sadece MLCC capacitor kullanıp, hiç bulk capacitor kullanmamalarını bu hesaptan sonra anladım.
> İlerleyen süreçte, çıkışta bulk capacitor kullanmayı gerektirecek şekilde design requirement’ı sıkılaştırabilirim.



Ek not:

ODT kopyasinda bu bolumde de bazi semboller ve denklemler eksik gorunuyor; bu nedenle kaynak metni aynen korudum. Teknik olarak burada korunmak istenen ana iliskiler su sekilde yazilabilir:

```math
- bobin akiminin yukselme hizi birinci yaklasimda $\dfrac{dI_L}{dt} \approx \dfrac{V_L}{L}$ ile sinirlanir
```

Yani bobin akiminin artis hizi, bobin uzerindeki anlik gerilim ve enduktans tarafindan sinirlanir. Bu nedenle feedback dongusu duty oranini arttirsa bile akim sonsuz hizla yukselemez.

Yukteki ani artis ile bobin akimi arasindaki fark ilk anda cikis kapasitörleri tarafindan karsilanir:

```math
I_C = I_{load} - I_L
```

Bu da cikis geriliminin gecici davranisini belirler:

```math
C_{out}\frac{dV_{out}}{dt} = -I_C
```

$t_1$ ani ve kullanicinin not ettigi $t_{undershoot}$ sonu, matematiksel olarak su kosulla iliskilendirilebilir:

```math
\frac{dV_{out}}{dt} = 0
```

Bu anda bobin akimi, en azindan o anlik durumda, yuk talebini karsilayabilecek seviyeye yaklasmistir. ODT'deki EVM yorumu da bu bakimdan anlamlidir: eger MLCC bankasi bu gecis suresinde yeterli enerjiyi saglayabiliyorsa, bulk kapasitör ihtiyaci her durumda zorunlu olmayabilir.




#### 5.4.7 Acik teknik dogrulamalar



Bu bolumde daha sonra eklenecekler:

- cikis kapasitörleri icin etkin kapasitans hesabi

- ESR ve ESL etkisi

- cikis sapmasi ile kapali-cevrim cikis empedansi arasindaki bag

- MLCC sayisi ve de-rating hesabi

- G71 benzeri hizli-yuk yaklasimlarinin bu tasarima ne kadar gerekli oldugu

- cikista bulk kapasitör gerekip gerekmedigine dair nihai karar



### 5.5 Giris kapasiteleri



#### 5.5.1 Giris sigaçlari



ODT'den aktarilan metin (`7. giriş sıgaçları`):

> Çıkış kapasitörlerinde, mevcut MLCC'lere ek olarak bulk kapasitör kullanmaktan kaçındım; çünkü bulk kapasitörler çıkış empedansını değiştirerek kontrolcü tasarımını doğrudan etkiler ve kompanzasyonun yeniden ele alınmasını gerektirebilir. Ancak yaptığım hesaplamalarda mevcut MLCC’lerin, tanımlı gerilim ripple ve transient kriterlerini karşılaması yeterli olduğundan, ilave bulk kapasiteye ihtiyaç duyulmadı. Öte yandan, giriş kapasitörleri kontrolcü tasarımını doğrudan etkilemediği için (giriş empedansı, kontrol döngüsünden bağımsızdır), EMI ve termal sınırlar gözetilerek bulk kapasitör kullanımına yer verdim. Bu sayede orijinal spesifikasyonlarda yer almayan zorlu koşullara (örneğin ani giriş transiyentleri) karşı sistem dayanımı artırıldı.***



Ek not:

Bu parca, cikis ve giris kapasiteleri arasindaki tasarim felsefesi farkini netlestiriyor. Cikista bulk kapasitör karari kontrol ve kompanzasyon tarafini etkilerken, giriste bulk kapasitör kullanimi daha cok `EMI`, termal dayanım ve transient dayanimi acisindan degerlendiriliyor.



#### 5.5.2 Yeni eklenen giris gereksinimleri



ODT'den aktarilan metin (`Eklenen yeni teknik şartlar`):

> Allowed input peak-to-peak ripple voltage: ΔVIN_PP ≤ 0.24V
> Allowed input transient undershoot or overshoot: ΔVIN_Tran ≤ 0.36V



Ek not:

Bu iki teknik sart, giris kapasiteleri icin artik yalnizca ortalama enerji depolama degil, ayni zamanda anlik gerilim kalite sinirlari da tanimlandigini gosteriyor. GitHub'da daha duzgun gorunmesi icin bu sinirlar su sekilde de okunabilir:

- $\Delta V_{IN,PP} \le 0.24\,\text{V}$
- $\Delta V_{IN,Tran} \le 0.36\,\text{V}$



#### 5.5.3 Secilen genel strateji



ODT'den aktarilan metin:

> Yüksek frekans ripple’ını bastırmak için MLCC kullanacağım, ancak yüksek dc bias gerilimi altında capacitance’ları düşüyor.
> Elektrolitik kapasitörler, düşük maliyetli ve büyük enerji depolama kapasitesine sahiptir ama yüksek ESR nedeniyle ripple bastırmakta zayıftır.
> Bu yüzden, MLCC+Elektrolitik capacitor’u beraber kullanacağım…****



Ek not:

Bu strateji, farkli kapasitör teknolojilerinin guclu oldugu frekans araliklarini birlikte kullanma mantigina dayanir:

- MLCC tarafı: yuksek frekans ripple ve dusuk `ESR/ESL`
- elektrolitik tarafı: daha buyuk enerji tamponu ve daha dusuk maliyetli bulk kapasite

Ancak MLCC seciminde `dc-bias` altindaki etkin kapasitans dususu mutlaka hesaba katilmalidir; katalog degeri tek basina yeterli degildir.



#### 5.5.4 MLCC `Cin` hesabi



ODT'den aktarilan metin (`7.1. MLCC Sığaç Hesabı / 7.1.1. MLCC Cin Hesabı?`):

> PKfsfsfsd
> Girişteki ripple gerilimini ΔVIN_PP ≤ 0.24V ‘un altında tutabilecek, capacitor capacitance’nin en düşük değerini buluyoruz. [2] [3]
> En yüksek a [3] ve [2] kaynaklarındaki örneklerde … D’nin … en kötü durum D = 0.5 iken ..
> Bu değer etkin capacitance değeridir.
> Dc Bias’a bak.
> Voltage rating’e bak.
> Tolerance’ı dikkate al yüzde kaçsa düş. Acaba kemet nasıl hesaplıyor?
>
> Voltage rating’e de dikkat. Emv’de 100V’luk voltage rating’i olan capacitor’ler kullanılmış. Geniş bir giriş gerilim aralığında çalışacak(5.5-100V?) şekilde tasarlandığı için olmalı…?



Ek not:

Bu alt bolumde korunacak esas mantik su:

- once gerekli minimum etkin `Cin` bulunur
- sonra `dc-bias`, tolerans ve gerilim sinifi etkileri eklenerek gercek secilecek parca degerine gidilir

ODT notunda gecen $D = 0.5$ varsayimi, ilk muhendislik yaklasimi olarak makul bir worst-case kontrol noktasi olabilir; ancak nihai hesapta gercek $V_{in} / V_{out}$ araligina gore tekrar kontrol edilmelidir. Bu nedenle burada asil hedef katalog `uF` degerini degil, etkin $C_{in}$ gereksinimini bulmaktir.



#### 5.5.5 RMS akimi ve termal bakis



ODT tarafinda dogru olarak vurgulanan ikinci konu, MLCC'lerin yalnizca kapasitans acisindan degil, RMS akim ve sicaklik artisi acisindan da boyutlandirilmasidir.

ODT'den aktarilan metin (`7.1.2. MLCC Sığaçlarının RMS Akımı?`):

> Girişteki ripple gerilimini karşılamanın yanı sıra?** seramik sığaçlar, ısıl dayanımı da olmalıdır.?** Azami giriş akımının rms’sini hesaplayacağız.
> Tam yükte..
> Full load: Sistemin maksimum çıkış gücüyle (örneğin 125 W) çalıştığı durumu ifade eder. Bu durumda: Çıkış akımı (Iₒ) en yüksek seviyededir (senin örneğinde 9A Dolayısıyla girişten çekilen toplam akım da en yüksek olur. Ripple akımı DC akım üzerine binen AC bileşen olduğundan, DC arttıkça RMS ripple bileşeni de artar.***

Kaba Hesap:

![MLCC RMS akimi kaba hesap](images/odt_embedded/fig_06_input_mlcc_rms_kaba_hesap.jpg)

> Mavi eğri IIn_rms /Iload oranını verir.
> D =0.5 iken: IIn_rms /Iload =yaklaşık 0.5’tır mavi eğri, dikey eksen seviyesi.
> IIn_rms = Iload*0.5
> IIn_rms = 9A*0.5
> IIn_rms = 4.5A rms
>
> ODT'deki denklem notasyonu bozuk aktarilmis; temiz matematik yazimi asagida verilmistir.



Ek not:

Bu alt bolumde ana teknik fikir, MLCC seciminin yalnizca kapasitansla degil RMS akim ve termal dayanım ile birlikte yapilmasi gerektigidir. ODT notundaki kaba $D = 0.5$ yaklasimi ilk kontrol icin uygundur ve mavi egri uzerinden $I_{in,RMS} \approx 0.5 I_{load}$ sonucu okunarak $4.5\,\text{A}_{RMS}$ mertebesi bulunabilir.

Kaba yaklasimin temiz yazimi:

```math
\frac{I_{in,RMS}}{I_{load}} \approx 0.5
\qquad (D = 0.5)
```

```math
I_{in,RMS}
= I_{load}\cdot 0.5
= 9\,\text{A}\cdot 0.5
= 4.5\,\text{A}_{RMS}
```

GitHub uyumlu matematik bicimiyle verilen daha ayrintili ifade su sekilde yazilabilir:

```math
I_{in,RMS,\max}
= I_{LOAD,\max}
\sqrt{
D(1-D)
+ \frac{1}{12}\cdot
\left(\frac{V_{OUT}}{L\,f_{sw}\,I_{\max}}\right)^2
\cdot (1-D)^2 \cdot D
}
```

ODT notasyonundaki sayisal yerlestirme korunursa:

```math
I_{in,RMS,\max}
= 9\,\text{A}\,
\sqrt{
0.5(1-0.5)
+ \frac{1}{12}\cdot
\left(\frac{14\,\text{V}}{6.8\,\mu\text{H}\cdot 332\,\text{kHz}\cdot 9\,\text{A}}\right)^2
\cdot (1-0.5)^2 \cdot 0.5
}
\approx 4.55\,\text{A}_{RMS}
```

Bu sonuc, giris MLCC bankasinin yalnizca ripple gerilimini degil, RMS akim tasima ve sicaklik artisi tarafini da karsilamasi gerektigini gosterir.

ODT'den aktarilan metin (`7.1.3. MLCC Sığaçlarının Sıcaklıkla RMS Akımıı?`):

> NOT: ELEMAN SEÇMEYE ÇALIŞMAAA
> 4.55 Arms akımı
> X7R, X5R’den daha iyi sıcaklığa daha dayanıklı.

![MLCC sicaklik / X7R-X5R civari](images/odt_embedded/fig_07_input_mlcc_temp_x7r_x5r.png)

![MLCC sicaklik civari - 1](images/odt_embedded/fig_08_input_mlcc_temp_grafik_01.png)

> Giriş kapasitorlerinin pozitif ucu, High-side mosfet’ın Drain ucuna, mmümkün (Kondansatörün negatif ucu, low-side MOSFET’in source ucuna, pozitif ucu ise high-side MOSFET’in drain ucuna mümkün olduğunca yakın yerleştirilmelidir.
> di/dt’yi düşürmek için. ****

![MLCC sicaklik civari - 2](images/odt_embedded/fig_09_input_mlcc_temp_grafik_02.png)

> 48V, 8A’de Main Mosfet’teki Marker1: 73.9 derece ölçülmüş. 24V, 8A’de Main Mosfet’teki Marker1: 64.8 derece ölçülmüş.
> Bizim sistemde Vinmax = 36V, Marker1 bölgesinden biraz dahasoğukbölgeolduüğundan en fazla 70derecedir çıkarımını yaptım.

![MLCC sicaklik civari - 3](images/odt_embedded/fig_10_input_mlcc_temp_grafik_03.png)

> Çıkış sıcağının bulunduğu yerdeki Kart sıcaklığı 70derece olarak tahmin ettik** Iin_rms_max = 4.55Arms bulmuştuk.
>
> Üzerinden geçen bu akımla ne kadar sıcaklık artışı oluyor. Bu dikeydeki temp.rise derecesi kartın derecesine eklenir. Diyelim ki Hiç akım yokken kartın sıcaklığı ne ise ona değen kapaciyorun de sıcaklığı o olur. Termodinamik birbirine değen…
> Kapasitor, Sıcaklık artışının ne kadara kabul ediyor. Buna bak.
>
> Paralel capacitor kullanarak Iin_rms_max = 4.55Arms’yi böleriz. TEMP.rise da az olur.



Ek not:

Bu parca, MLCC seciminde yalnizca elektriksel degerlerin degil sicaklik dayanimının da hesaba katilmasi gerektigini vurguluyor. ODT notundaki ana fikirler sunlardir:

- $X7R$, $X5R$'ye gore sicaklik davranisi acisindan daha guvenli bir tercihtir
- giris MLCC'leri high-side drain ile low-side source arasindaki sicak donguye olabildigince yakin yerlestirilmelidir
- $I_{in,RMS,\max} = 4.55\,\text{A}_{RMS}$ akimini birden fazla paralel kapasitore paylastirmak, her bir kapasitordeki `temp rise` degerini azaltir

`48 V / 8 A` ve `24 V / 8 A` EVM gozlemlerinden yapilan $\sim 70\,^{\circ}\text{C}$ kart sicakligi tahmini, yalnizca ilk muhendislik referansi olarak tutulmalidir; nihai secimde secilen MLCC datasheet'indeki izin verilen sicaklik artisi ve ortam / kart sicakligi birlikte kontrol edilmelidir.




#### 5.5.6 ESL ve kucuk degerli yardimci MLCC'ler


ODT'den aktarilan metin (`7.1.4. MLCC Sığaçlarının ESL değeri ve Küçük Capacitance’lı Mlcc eklemek`):

> Mlccler yüksek frekansta çalışırken ESL değerleri çok önemli olur.ç***
> Düşük esl düşük ripple’için gereklidir.
> Paralel bağlarsak esl’yi düşürürüz.

![MLCC ESL ve paralelleme civari - 1](images/odt_embedded/fig_11_input_esl_parallel_mlcc.png)

![MLCC ESL ve paralelleme civari - 2](images/odt_embedded/fig_12_input_esl_not_01.png)

![MLCC ESL ve paralelleme civari - 3](images/odt_embedded/fig_13_input_esl_not_02.png)

![MLCC ESL ve paralelleme civari - 4](images/odt_embedded/fig_14_input_esl_not_03.png)

> genel ilke
> ESL ne kadar düşükse, yüksek frekanslı spike’lar o kadar iyi bastırılır



Ek not:

Bu parca, yuksek frekansta MLCC seciminde `ESL`'nin belirleyici oldugunu net sekilde vurguluyor. Burada korunacak ana fikirler sunlardir:

- dusuk `ESL`, yuksek frekansli spike ve ringing bastirma acisindan kritiktir
- birden fazla MLCC'yi paralel baglamak etkin `ESL`'yi dusurmeye yardim eder
- buyuk degerli ana MLCC'lere ek olarak daha kucuk kapasitansli, fiziksel olarak kucuk ve dusuk `ESL`'li yardimci kapasitörler de kullanilabilir

Bu nedenle giris kapasitör agi tek tip ve tek degerli dusunulmemelidir; final karar yerlesim, akim dongusu ve parasitikler ile birlikte verilmelidir.

ODT'den aktarilan metin (`7.1.4.1 Çok Küçük Farad’lı, Küçük Boyutlu MLCC’ler eklemek`):

> Mosfetler çok hızlı açlıp kapandığında, giriş akımında ani sıçramalar oluşur. Giriş geriliminde ani kısa süreli dalgalanlanma Input spikes: Phase-node ringing:
> Ali Shirsavar’den de örnek ver.
> Küçük boyutlu (örneğin 0402, 0603) MLCC’ler genellikle daha düşük ESL değerine sahiptir.
> Bu nedenle, sisteme 1 µF, 0.1 µF gibi küçük seramik kondansatörler eklenerek, bu yüksek frekanslı istenmeyen etkiler azaltılabilir.

![Kucuk MLCC ekleme civari - 1](images/odt_embedded/fig_15_small_mlcc_high_freq_01.png)

![Kucuk MLCC ekleme civari - 2](images/odt_embedded/fig_16_small_mlcc_high_freq_02.png)



Ek not:

Bu parca, ana MLCC bankasina ek olarak cok kucuk degerli yardimci MLCC ekleme mantigini veriyor. Buradaki fikir, enerji depolamaktan cok, `input spikes` ve `phase-node ringing` gibi cok yuksek frekansli istenmeyen etkileri bastirmaktir.

Kucuk paketli `0402` veya `0603` MLCC'ler genellikle daha dusuk `ESL` sundugu icin, `1 µF` ve `0.1 µF` gibi degerler sicak akim dongusune yakin yerlestirilerek anahtarlama kenarlarindaki ani akim ihtiyacina daha hizli cevap verebilir.




#### 5.5.7 Bulk kapasitör secimi mantigi


ODT'den aktarilan metin (`7.2. Giriş Bulk Capacitor Seçimi`):

> MLCC’ler düşük esr’leri sayesinde çok iyi ripple akımı yaşıyabilir. Ancak Mlcclerin etkin capacitance’sı Dc Bias ve sıcaklık altında önemli ölçüde azalır.
> Bu da transient’lerde yeterli enerji depolayamamasına neden olur. Bu neden Bulk capacitorlerde kullanılır.

![Giris bulk capacitor secimi - 1](images/odt_embedded/fig_17_input_bulk_selection_01.png)

![Giris bulk capacitor secimi - 2](images/odt_embedded/fig_18_input_bulk_selection_02.png)

![Giris bulk capacitor secimi - 3](images/odt_embedded/fig_19_input_bulk_selection_03.png)

![Giris bulk capacitor secimi - 4](images/odt_embedded/fig_20_input_bulk_selection_note.jpg)



Ek not:

Bu parca, bulk kapasitörlerin neden hala gerekli olabildigini netlestiriyor. MLCC'ler yuksek frekans ripple bastirmada cok guclu olsalar da, `dc-bias` ve sicaklik altinda etkin kapasitans kaybi yasadiklari icin daha yavas transient enerji ihtiyacinda tek baslarina yetersiz kalabilirler.

Bu nedenle bulk kapasitörler burada yuksek frekansli sicak akim dongusunun ana parcasi gibi degil, enerji tamponu ve daha yavas dinamiklerin destekleyicisi olarak dusunulmelidir.




#### 5.5.8 Acik teknik dogrulamalar



Bu bolume daha sonra su hesaplar eklenecek:

- minimum etkin `Cin` hesabi

- duty'ye bagli gercek `Iin_rms` hesabi

- dc-bias ve toleransla duzeltilmis gercek kapasitans

- MLCC basina RMS akim dagilimi

- sicaklik artisi ve component derating kontrolu

- yuksek frekans spike bastirma icin ek kucuk MLCC stratejisi

- bulk kapasitör secimi ve damping gerekcesi

- EMI filtresi ile birlikte giris aginin son hali



### 5.6 MOSFET secimi ve kayiplar



#### 5.6.1 Mevcut aday parca



Proje klasorunde secilmis MOSFET adayi olarak su parca gorunuyor:

- `BOM/M2_Texas_Instruments_CSD18543Q3A_csd18543q3a.pdf`



Ek not dosyalarinda bu parcaya ait ilk parametre ozeti de bulunuyor:

- $R_{DS(on)} \approx 8.2\,\text{m}\Omega$

- $Q_{g,\text{total}} \approx 16\,\text{nC}$

- $C_{gd} \approx 54\,\text{pF}$

- $Q_{gd} \approx 1.2\,\text{nC}$ yaklasimi

- $V_{\text{drive}} \approx 7.5\,\text{V}$

- $R_{gate} = 2.2\,\Omega$ notu



Bu, MOSFET seciminin tamamen acikta olmadigini; fakat son gerekceli anlatimin henuz Markdown belgeye tasinmadigini gosteriyor.



#### 5.6.2 Secim mantigi



Bu tasarimda MOSFET secimi yalnizca dusuk `RDS(on)` aramak degildir. Mevcut notlarin isaret ettigi temel denge su sekildedir:

- dusuk `RDS(on)` ile iletim kayiplari azaltilmak istenir

- dusuk gate charge ile switching kayiplari ve surucu yuku sinirli tutulmak istenir

- `332 kHz` gibi orta-yuksek bir frekansta, yalnizca iletim kaybina bakmak yetersizdir

- termal limitler, paket yetenegi ve gercek PCB kosullari birlikte degerlendirilmelidir



Bu nedenle MOSFET secimi en az su basliklarla savunulmalidir:

- `VDS` gerilim marji

- `RDS(on)`

- `Qg`, ozellikle `Qgd`

- termal metrikler ve paket

- guvenli calisma alani ve akim/avalanche sinirlari



#### 5.6.3 Ilk kayip notlari



`Verimlilik.txt` icinde bu aday MOSFET ve secilen frekans civari icin su ilk tahminler yer aliyor:

- toplam MOSFET switching kaybi `~540 mW`

- toplam MOSFET conduction kaybi `~1.65 W`



Bu sayilar henuz son dogrulanmis sonuc olarak alinmamalidir; ancak taslak dusunceyi gosterir:

- bu tasarimda iletim kaybi, switching kaybindan daha baskin gorunmektedir

- buna ragmen `332 kHz` frekansinda switching kaybi ihmal edilecek kadar kucuk degildir



Bu da secilen frekansin ve secilen MOSFET'in birlikte degerlendirilmesi gerektigini gosterir.



#### 5.6.4 Thermal ve datasheet yorumu acisindan dikkatler



Yerel kaynaklar icinde MOSFET current rating yorumuna dair onemli bir not da bulunuyor: datasheet'teki akim sinirlari genellikle olculen degil, belirli termal varsayimlarla hesaplanan degerlerdir.



Bu nedenle son raporda su ilke korunmalidir:

- MOSFET'in datasheet continuous current degeri dogrudan yeterlilik kaniti olarak kullanilmayacak

- bunun yerine gercek kayip hesabi ve termal varsayim zinciri kurulacak

- `RthetaJA`, `RthetaJC`, kart sicakligi ve tahmini junction sicakligi birlikte yorumlanacak



#### 5.6.5 Bu bolume daha sonra eklenecekler



Bu bolum tamamlanirken su maddeler netlestirilecek:

- secilen MOSFET'in tam datasheet ozeti

- `VDS` marji ve maksimum gerilim gercekleri

- `RDS(on)`'un surme gerilimi ve sicaklikla degisimi

- iletim kaybi hesabi

- switching kaybi hesabi

- gate surucu kaybi ve dead-time etkileri

- gerekiyorsa common-source inductance ve parasitik etkiler

- tahmini junction sicakligi ve termal marj



### 5.7 Bootstrap agi



#### 5.7.1 ODT'den aktarilan metin (`8. mosfetler`)



> Eklenecek..



#### 5.7.2 Bootstrap direncini (`R_{boot}`) hesaplama ve secim mantigi



ODT'den aktarilan metin (`8.1. Bootstrap Direncini (Rboot) Hesaplama ve Seçim Mantığı`, denklemler okunur formatta yeniden yazildi):

1. "Hangi duty?"

PWM'de high-side MOSFET'in en uzun sure acik kaldigi durum, bootstrap kondansatorunun en kisa sarj penceresini verir. ODT notunda verilen degerlerle:

```math
D_{\text{high,max}} = 0.648
```

```math
D_{\text{low,min}} = 1 - D_{\text{high,max}} = 1 - 0.648 = 0.352
```

Bu nedenle bootstrap sarji icin kullanilan worst-case pencere, periyodun `%35.2`'lik low-side iletim araligidir.

2. Sarj penceresi ve zaman sabiti

ODT notunda bu adim, $R_{boot} = 2.2\,\Omega$ ve $C_{boot} = 0.1\,\mu\text{F}$ secimiyle tartisiliyor. Standart RC zaman sabiti tanimi ile:

```math
\tau = R_{boot} C_{boot}
= 2.2\,\Omega \times 0.1\,\mu\text{F}
= 0.22\,\mu\text{s}
```

PWM frekansi $332\,\text{kHz}$ icin:

```math
T_{sw} = \frac{1}{f_{sw}}
= \frac{1}{332\,\text{kHz}}
\approx 3.01\,\mu\text{s}
```

Worst-case bootstrap sarj penceresi:

```math
t_{\text{charge,min}} = D_{\text{low,min}} T_{sw}
= 0.352 \times 3.01\,\mu\text{s}
\approx 1.06\,\mu\text{s}
```

Dolayisiyla:

```math
\frac{t_{\text{charge,min}}}{\tau}
= \frac{1.06}{0.22}
\approx 4.82
```

ODT notunda gecen `0.625\,\mu\text{s}` degeri, standart RC zaman sabiti tanimi degildir. Standart tanimla bakildiginda, sarj penceresi yaklasik `4.8\tau` surmektedir. Bu, ideal bir RC sarj modelinde bootstrap kondansatorunun cok buyuk oranda dolabilecegini gosterir.

3. Bootstrap gerilimi

ODT notunda `V_{DD} = 8.4\,\text{V}` ve bootstrap diyot dusumu yaklasik `0.5\,\text{V}` alinmis. Buna gore hedef bootstrap gerilimi:

```math
V_{C_{boot}} \approx V_{DD} - V_{\text{BootDiode}}
= 8.4\,\text{V} - 0.5\,\text{V}
= 7.9\,\text{V}
```

Eger bootstrap kondansatoru worst-case pencerenin basinda tamamen bos kabul edilirse, ideal RC modeliyle bu pencere sonunda ulasilan gerilim:

```math
V_{C_{boot}}(t_{\text{charge,min}})
\approx 7.9\left(1 - e^{-t_{\text{charge,min}}/\tau}\right)
\approx 7.9\left(1 - e^{-4.82}\right)
\approx 7.84\,\text{V}
```

Bu, ODT notundaki "bootstrap voltaji yeterince hizli olusuyor" sonucunu destekler.

4. Kondansatörde depolanan enerji

Bootstrap kondansatorunde depolanan enerji:

```math
E_{\text{cap}}
= \frac{1}{2} C_{boot} V_{C_{boot}}^2
= \frac{1}{2}\cdot 0.1\,\mu\text{F}\cdot (7.9\,\text{V})^2
\approx 3.12\,\mu\text{J}
```

Bu, kondansatorun yaklasik `7.9 V` seviyesine sarj oldugunda depoladigi enerjiyi verir.

5. Direncte yakilan enerji hakkinda dikkat

ODT notunda dirençte yakilan enerji icin ayrica bir hesap niyeti bulunuyor. Ancak bu kisimda kullanilan ifade, mevcut haliyle nihai denklem olarak alinmamalidir. Tam `0 V -> 7.9 V` RC sarj adiminda ideal seri dirençte harcanan enerji, ayni buyukluk mertebesinde:

```math
E_{R,\text{full-step}}
\approx \frac{1}{2} C_{boot} V_{C_{boot}}^2
\approx 3.12\,\mu\text{J}
```

olur. Fakat bootstrap kondansatoru her cevrimde sifirdan sarj olmadigi icin, gercek cevrimsel kayip bundan daha dusuk olur. Bu nedenle direnç watt secimi yalnizca bu tek enerji hesabina baglanmamalidir.

6. Ilk tepe akimi

Bootstrap sarjinin ilk anindaki yaklasik tepe akimi:

```math
I_{\text{pk}}
= \frac{V_{DD} - V_{\text{BootDiode}}}{R_{boot}}
= \frac{8.4\,\text{V} - 0.5\,\text{V}}{2.2\,\Omega}
\approx 3.59\,\text{A}
```

Bu nedenle bootstrap diyotu ve PCB izleri, en azindan bu buyukluk mertebesindeki pik sarj akimini tasiyabilecek sekilde degerlendirilmelidir.



Ek not:

Bu bolumde korunacak ana muhendislik mantigi sunudur:

- worst-case bootstrap sarj penceresi, $D_{\text{low,min}}$ ile belirlenir
- $R_{boot}$ buyurse sarj yavaslar; cok kuculurse pik akim ve ringing artar
- $R_{boot} = 2.2\,\Omega$ ve $C_{boot} = 0.1\,\mu\text{F}$ secimi, ilk bakista sarj hizi acisindan makul gorunmektedir
- fakat direnç guc derecelendirmesi, diyot peak akimi ve bootstrap dugumu ringing davranisi nihai olarak simulasyon veya deneyle dogrulanmalidir

Bu nedenle bu bolumde ozellikle su maddeler sonra yeniden kontrol edilmelidir:

- $D_{\text{high,max}} = 0.648$
- `VDD` ve diyot dusumunun datasheet ile teyidi
- bootstrap diyotunun peak akim ve ters gerilim marji
- `R_{boot}` seciminin ringing ve sarj hizi acisindan uygunlugu
- gerekiyorsa LTspice / PSpice ile bootstrap dugumu dalga sekli



## 6. Kontrolcu ve Kompanzasyon



### 6.1 Modulator ve power stage modeli



#### 6.1.1 Genel kontrol yapisi



Taslakta kontrol dongusu su bloklar uzerinden dusunuluyor:

- modulator

- power stage, yani cikis filtresi ve yuk

- hata yukselteci / kompanzator

- geri besleme bolucusu



Bu cerceve, voltage-mode control dusunce yapisi ile uyumludur. Kompanzatorun gorevi, power stage'in kutup ve sifirlarini hesaba katarak istenen crossover frekansi ve faz marjini saglamaktir.



#### 6.1.2 Modulator blogunun rolu


ODT'den aktarilan metin (`9.2. Modulator Bloğu`):

> Frekansa bağımlı değildir.
>
> Duty Cycle'ı belirler. Compensator bloğunda gelen işareti artarsa işaretinin genişliği artar, duty cycle artar, Q1 ana mosfet daha uzun süre açık (`on`) iletimde kalır.

![Voltage Mode Modulator](images/odt_embedded/fig_22_voltage_mode_modulator.png)

Voltage Mode Modulator

> Modulator bloğunun transfer function'ı:

```math
H_{\text{mod}}(s)
= \frac{V_{in}}{V_{\text{ramp}}}
= k_{FF}
= 15\,\text{V/V}
```

> Kullandığımız LM5146-Q1'ın input voltage feedforward özelliği var. Bu özellik sayesinde modülatör kazancı sabit kalıyor. Örneğin giriş gerilimi $V_{in}$ artarsa, $V_{\text{ramp}}$ da artarak $k_{FF} = 15\,\text{V/V}$ değerinin sabit kalmasını sağlıyor.



Ek not:

Bu bolumde korunacak ana fikir, voltage-mode kontrol yapisinda modulator blogunun frekansa bagimli bir filtre gibi davranmamasidir; asil gorevi, kompanzator cikisini PWM gorev oranina cevirmektir. Bu nedenle:

- kompanzator cikisi artarsa high-side iletim suresi artar
- duty cycle artar
- guc katina aktarilan ortalama enerji artar

LM5146-Q1 icindeki input-voltage feedforward yapisi nedeniyle, rampa genligi giris gerilimiyle birlikte olceklenir. Bu da modulator kazancinin yaklasik sabit tutulmasini saglar ve kontrol tasarimini sadeleştirir. Bu projede ilk yaklasim olarak:

```math
V_{\text{ramp}} \approx \frac{V_{in}}{15}
```

ve dolayisiyla

```math
H_{\text{mod}}(s)
= \frac{V_{in}}{V_{\text{ramp}}}
\approx 15
```

alinabilir.



#### 6.1.3 Cikis filtresi ve yuk



ODT'den aktarilan metin (`Çıkış Filtresi ve Yük`):

![Cikis Filtresi ve Yuk](images/odt_embedded/fig_23_output_filter_and_load.png)

Çıkış Filtresi ve Yük

> Cikis filtresi ve yuk icin kullanilan transfer fonksiyonu:

```math
H_{\text{filter}}(s)
= \frac{V_{out}}{V_{in}}
= \frac{1 + C_{out} R_{esr} s}{a_0 + a_1 s + a_2 s^2}
```

Burada katsayilar:

```math
a_0 = 1 + \frac{R_{Damp}}{R_{load}}
```

```math
a_1 = \frac{L_F}{R_{load}} + R_{esr} C_{out} + R_{Damp} C_{out}
+ \frac{R_{Damp} R_{esr} C_{out}}{R_{load}}
```

```math
a_2 = \left(\frac{R_{load} + R_{esr}}{R_{load}}\right)L_F C_{out}
```

> Sayisal yerlestirme:

```math
a_0 = 1 + \frac{21.13\,\text{m}\Omega}{1.59\,\Omega} = 1.01329
```

```math
a_1 =
\frac{6.8\,\mu\text{H}}{1.59\,\Omega}
+ (0.26\,\text{m}\Omega)(70\,\mu\text{F})
+ (21.13\,\text{m}\Omega)(70\,\mu\text{F})
+ \frac{(21.13\,\text{m}\Omega)(0.26\,\text{m}\Omega)(70\,\mu\text{F})}{1.59\,\Omega}
= 5.774 \times 10^{-6}
```

```math
a_2 =
\left(\frac{1.59\,\Omega + 0.26\,\text{m}\Omega}{1.59\,\Omega}\right)
(6.8\,\mu\text{H})(70\,\mu\text{F})
= 4.761 \times 10^{-10}
```

```math
C_{out} R_{esr} = (70\,\mu\text{F})(0.26\,\text{m}\Omega) = 1.82 \times 10^{-8}
```

> Yaklasik katsayi bicimi:

```math
H_{\text{filter}}(s)
\approx
\frac{1 + 1.82 \times 10^{-8} s}
{1.01329 + 5.774 \times 10^{-6} s + 4.761 \times 10^{-10} s^2}
```

![Tasarimimizda kullandigimiz cikis capacitorleri](images/odt_embedded/fig_24_output_capacitor_bank.png)

Tasarımımızda kullandığımız çıkış capacitorleri

> Iki farkli turde cikis capacitor'u mevcut. $H_{\text{filter}}(s)$ transfer fonksiyonu hesabinda $0.1\,\mu\text{F}$ degerindeki `C28` capacitor'unu ihmal ettik.
>
> Sebebi: ihmal edilmis bicimi yeterince dogru; bu capacitor esas olarak daha yuksek frekans bolgesinde etkili.



Ek not:

Bu bolumde ana fikir, kontrol dongusunun plant kisminin yalnizca bobinden degil, cikis capacitor bank'i ve yuk ile birlikte olusan ikinci dereceden bir yapidan gelmesidir. Burada:

- $L_F$ enerji depolayan ana kutbu etkiler
- $C_{out}$ cikis dugumunun dinamiklerini belirler
- $R_{esr}$ ESR sifirini olusturur
- $R_{load}$ ve varsa sönumle ilgili direncler kutup yerlerini degistirir

$C_{28} = 0.1\,\mu\text{F}$ gibi cok kucuk bir MLCC'nin ilk transfer fonksiyonu hesabinda ihmal edilmesi, ancak bu elemanin etkisi crossover civarinda gercekten kucukse kabul edilebilir. Genelde bu tip cok kucuk kapasiteler ana enerji depolama elemani olmaktan cok, daha yuksek frekansli spike ve ringing bastirma tarafinda etkili olur. Bu nedenle kompanzator hesabinda ilk yaklasimda ihmal edilmesi makul olabilir; yine de son kararin bode veya AC sweep ile dogrulanmasi gerekir.




#### 6.1.4 Bu bolumde korunacak ayrim



Su an icin iki ayri katman oldugu acik yazilmalidir:

- genel yontem ve blok diyagram mantigi

- gercek tasarima ait sayisal transfer fonksiyonu



`G103_comp.txt` icindeki sayilar faydali bir ornek / calisma dosyasidir; ancak bunlarin tamami dogrudan bu proje icin son deger olarak alinmamalidir. Nihai transfer fonksiyonu, bu projede secilen gercek `L`, `C`, `ESR`, `DCR`, yuk ve modulator parametrelerinden yeniden kurulacaktir.



### 6.2 K-factor yaklasimi



#### 6.2.1 Neden bu yontem?


ODT'den aktarilan metin (`9. kontrolcü tasarımı`):

> Bitirme Projesi 1 dersinde kontrolcü tasarımını [1] kaynağındaki gerilim kontrollü Type 3 PID Compensator yöntemini izleyerek yapmıştım. Bu dönemde, [4] kaynağındaki K factor yöntemi izleyerek yapacağım.



Ek not:

Bu parca, yalnizca baska bir kompanzator topolojisine gecisi degil, ayni kontrol tasarim problemini daha sistematik bir yontemle yeniden kurma niyetini gosteriyor. K-factor yaklasimi burada ozellikle su nedenle degerli:

- hedef crossover frekansinda gereken faz katkisini dogrudan hesaplamaya zorlar
- kompanzator kazancini plant ve modulator ile birlikte ele alir
- Type-III eleman degerlerini daha izlenebilir bir akista turetmeyi kolaylastirir



#### 6.2.2 Taslaktaki akisin ozeti



Yerel notlar ve `G95_04_K_FACTOR_CAN_HOCA.txt` icindeki ornek akis su mantigi izliyor:

1. hedef crossover frekansi secilir

2. hedef faz marji secilir

3. plant ve modulatorun crossover frekansindaki fazi bulunur

4. kompanzatorun saglamasi gereken ek faz bulunur

5. kompanzatorun crossover frekansindaki gereken kazanci bulunur

6. buradan `K` faktoru hesaplanir

7. Type-III agin `R` ve `C` degerleri turetilir



Bu akis, tasarimi deneme-yanilma yerine daha kontrollu hale getirir.



#### 6.2.3 Scriptlerde gorulen ilk ipuclari



Yerel scriptte su genel iliskiler kullaniliyor:

- crossover frekansi kabaca $f_t = 0.1\,f_s$

- gerekli kompanzator kazanci, modulator ve plant buyukluklerinden turetiliyor

- $K = \tan^2\!\left(\dfrac{\phi_{comp} + 90^\circ}{4}\right)$ tipinde standart K-factor iliskisi kullaniliyor





Bu iliskiler, taslaktaki yontemin teorik omurgasini veriyor. Ancak burada yine ayni dikkat korunmali:

- scriptteki ornek `60 V -> 15 V`, `100 kHz` ve belirli `L/C/R` degerleri dogrudan bu projenin son degerleri degil

- bu nedenle sadece yontem aynen alinacak; sayisal sonuc bu proje icin yeniden hesaplanacak



#### 6.2.4 Bu projede K-factor'un gorevi



Bu tasarim ozelinde K-factor yontemi ile sonunda su ciktilar uretilmek isteniyor:

- secilen `332 kHz` anahtarlama frekansi icin uygun bir crossover frekansi

- hedef faz marji

- Type-III kompanzator komponentleri

- acik ve kapali cevrim bode yorumu



Bu bolum, ileride MATLAB ve LTspice ile yapilacak loop dogrulamasinin teorik baslangic noktasi olacak.



#### 6.2.5 Compensator topolojisi ve temel model



ODT'den aktarilan metin (`Compensator`):

![Type 3 Compensator Devresi](images/odt_embedded/fig_25_type3_compensator.png)

Type 3 Compensator Devresi

> Compensator transfer function:

```math
H_{\text{compensator}}(s)
= \frac{V_{\text{control}}}{V_{\text{sense}}}
= \frac{H_{\text{error-amp,ol}}(s)}
{1 + H_{\text{error-amp,ol}}(s)\,\beta(s)}
```

> Error amplifier open-loop modeli:

```math
H_{\text{error-amp,ol}}(s)
= \frac{A_{VOL}}
{1 + \dfrac{s}{\omega_{\text{opamp}}}}
```

```math
\omega_{\text{opamp}}
= 2\pi\,\frac{GBW}{A_{VOL}}
```

> Bu kompanzator hesabinda kullanilan cikis filtresi ve yuk transfer fonksiyonu:

```math
H_{\text{filter}}(s)
= \frac{V_{out}}{V_{in}}
= \frac{1 + C_{out} R_{esr} s}{a_0 + a_1 s + a_2 s^2}
```

Burada:

```math
a_0 = 1 + \frac{R_{Damp}}{R_{load}}
```

```math
a_1 = \frac{L_F}{R_{load}} + R_{esr} C_{out} + R_{Damp} C_{out}
+ \frac{R_{Damp} R_{esr} C_{out}}{R_{load}}
```

```math
a_2 = \left(\frac{R_{load} + R_{esr}}{R_{load}}\right)L_F C_{out}
```

> Sayisal yerlestirme:

```math
a_0 = 1 + \frac{21.13\,\text{m}\Omega}{1.59\,\Omega} = 1.01329
```

```math
a_1 =
\frac{6.8\,\mu\text{H}}{1.59\,\Omega}
+ (0.26\,\text{m}\Omega)(70\,\mu\text{F})
+ (21.13\,\text{m}\Omega)(70\,\mu\text{F})
+ \frac{(21.13\,\text{m}\Omega)(0.26\,\text{m}\Omega)(70\,\mu\text{F})}{1.59\,\Omega}
= 5.774 \times 10^{-6}
```

```math
a_2 =
\left(\frac{1.59\,\Omega + 0.26\,\text{m}\Omega}{1.59\,\Omega}\right)
(6.8\,\mu\text{H})(70\,\mu\text{F})
= 4.761 \times 10^{-10}
```

```math
C_{out} R_{esr} = (70\,\mu\text{F})(0.26\,\text{m}\Omega) = 1.82 \times 10^{-8}
```

```math
H_{\text{filter}}(s)
\approx
\frac{1 + 1.82 \times 10^{-8} s}
{1.01329 + 5.774 \times 10^{-6} s + 4.761 \times 10^{-10} s^2}
```

![LM5146-Q1'nin Buck Regulator Poles and Zeros](images/odt_embedded/fig_26_compensator_related.png)

LM5146-Q1'nin Buck Regulator Poles and Zeros



Ek not:

Bu bolumde artik sadece "Type-III kompanzator kullanilacak" demekle yetinmiyoruz; kompanzatorun analitik olarak hangi bloktan geldigi de acik hale geliyor. Burada temel mantik su:

- sonlu acik-cevrim kazanca sahip hata yukselteci, `beta(s)` geri besleme agi ile birlikte kapali-cevrim kompanzatoru olusturur
- `H_{\text{compensator}}(s)` kontrol dugumundeki gerilimi sekillendirir
- bu blok, modulator ve power-stage ile carpilarak acik-cevrim donguyu belirler

Bu nedenle kompanzator tasarimi yalnizca `R` ve `C` secimi degil; ayni zamanda su uc ifadenin birlikte dusunulmesidir:

```math
H_{\text{open-loop}}(s)
= H_{\text{mod}}(s)\,H_{\text{filter}}(s)\,H_{\text{compensator}}(s)
```

Buradaki Type-III topoloji mantiklidir; ancak bu, komponent degerlerinin otomatik olarak dogru oldugu anlamina gelmez. Nihai `R/C` secimi, gercek plant, hedef crossover frekansi ve faz marji ile birlikte sonraki adimda yapilacaktir.




### 6.3 Hedef frekans yerlestirmesi



#### 6.3.1 Ilk ilke



Yerel scriptlerde ve notlarda ilk yaklasim olarak crossover frekansinin anahtarlama frekansinin yaklasik onda biri civarinda secilmesi fikri kullaniliyor.



Bu tasarimda secilen:

- $f_{sw} = 332\,\text{kHz}$



olduguna gore ilk muhendislik tahmini su olur:



```math
f_{c,\text{ilk}} \approx \frac{f_{sw}}{10} \approx 33.2\,\text{kHz}
```



Bu sadece ilk yerlestirme tahminidir; son deger plant kutuplari, ESR sifiri, transient hedefleri ve faz marji ile birlikte belirlenecektir.



#### 6.3.2 Sonradan eklenecekler



Bu bolume daha sonra su maddeler eklenecek:

- gercek hedef `fc` secimi

- `fo`, ESR sifiri ve kompanzator sifir/kutup konumlari

- secilen frekans yerlestirmesinin transient ve EMI ile iliskisi



### 6.4 Beklenen faz ve kazanc marjlari



#### 6.4.1 Taslak hedef bandi



Bu bolumde henuz nihai sayisal bode sonucu yoktur. Ancak yerel K-factor notlari ve voltage-mode buck kompanzasyon mantigina dayanarak ilk taslak hedef su sekilde yazilabilir:

- faz marji pozitif ve rahat bir bolgede olmali

- ilk hedef olarak `50 deg - 60 deg` bandi makul gorunuyor

- yerel ornek akislarda `55 deg` civari hedef kullaniliyor

- kazanc marji da pozitif kalmali; nihai deger bode sonucu ile raporlanmali



Buradaki sayilar su an "tasarim niyeti" seviyesindedir. Gercek `L`, `C`, `ESR`, `DCR`, modulator kazanci ve yuk araligiyla olusan plant uzerinden tekrar kontrol edilmeden final kabul edilmeyecektir.



#### 6.4.2 Neden bu kadar onemli?



Faz marji ve kazanc marji yalnizca akademik bir bode ciktisi degil, bu tasarimin davranisini dogrudan etkileyen karar olcutleridir:

- faz marji dusuk kalirsa load transient sonrasi ringing ve asiri tasma artabilir

- faz marji gereksiz fazla secilirse sistem gereksiz yavaslayabilir

- giris filtresi, parasitikler ve gercek ESR/DCR degerleri bu marjlari kagittaki ilk tahminden uzaklastirabilir



Bu nedenle nihai tasarimda hedef, yalnizca teorik olarak stabil bir dongu degil; transient, EMI ve uygulama toleranslari altinda da guvenli kalan bir dongu olmaktir.



#### 6.4.3 Kararlilik kriterinin bu projedeki anlami


ODT'den aktarilan metin (`Kararlılık Kriteri`):

![Kararlilik kriteri](images/odt_embedded/fig_21_stability_criterion.png)

Buck DC/DC Regulator Control Block Diagram

> Ani bir yük akımı değişimi sırasında Vout…. Şekil 9’daki block diagramının kapalı çevrim transfer fonksiyonu şöyledir:

GitHub uyumlu matematik bicimiyle:

```math
H_{\text{system}}(s)
= \frac{V_{out}}{V_{ref}}
= \frac{\text{Ileri Yol}}{1 + \text{Açık Çevrim}}
```

```math
H_{\text{system}}(s)
= \frac{H_{\text{mod}}(s)\,H_{\text{filter}}(s)}
{1 + H_{\text{mod}}(s)\,H_{\text{filter}}(s)\,H_{\text{compensator}}(s)}
```

```math
H_{\text{open-loop}}(s)
= H_{\text{mod}}(s)\,H_{\text{filter}}(s)\,H_{\text{compensator}}(s)
```

Dolayisiyla kapali cevrim ifade su sekilde de yazilabilir:

```math
H_{\text{system}}(s)
= \frac{H_{\text{mod}}(s)\,H_{\text{filter}}(s)}
{1 + H_{\text{open-loop}}(s)}
```



Ek not:

Bu bolumde korunacak ana fikir, kararliligin yalnizca "cikis bir sekilde regule oldu" seviyesinde degil, acik-cevrim ve kapali-cevrim iliskisinin acikca kurulmasi ile degerlendirilmesidir. Bu nedenle:

- uygun crossover bolgesinde `0 dB` gecisi
- pozitif faz marji
- pozitif kazanc marji
- transient davranisi ile uyumlu kapali cevrim cevap

birlikte yorumlanmalidir. Giris filtresi, parazitikler ve coklu kutup/sifir yapilari varsa stabilite yorumu sadece kabaca `f_{sw}/10` kuralina birakilmamalidir.





## 7. EMI, Giris Filtresi ve Yerlesim



### 7.1 Hot-loop ve kritik akim yollari



Senkron buck yapisinda en kritik yuksek `di/dt` dongusu, giris kapasitörleri ile yarim kopru arasindaki sicak dongudur. Pratik olarak bu dongu su elemanlar uzerinden kapanir:

- giris MLCC / bulk kapasitörleri

- high-side MOSFET

- low-side MOSFET

- tekrar giris kapasitörlerinin donus yolu



Bu alan buyudukce parasitik enduktans artar, anahtarlama anindaki gerilim sivrilmeleri ve yayilan EMI agirlasir. Bu nedenle:

- giris seramik kapasitörleri yarim kopruye fiziksel olarak cok yakin konulmalidir

- high-side drain ile low-side source arasindaki akim yolu kisa ve kompakt tutulmalidir

- switch-node bakir alani gereksiz buyutulmamali, sadece zorunlu akim tasima ve termal ihtiyac kadar birakilmalidir



Ikinci onemli akim yolu, bobin ve cikis kapasitörleri etrafindaki cikis akis yoludur. Bu yol hot-loop kadar sert `dv/dt` uretmese de ripple, transient ve olcum kalitesi uzerinde belirgindir.



### 7.2 Differential-mode EMI



Differential-mode EMI'nin ana kaynagi, buck donusturucunun kaynaktan cektigi darbeli giris akimidir. Bu tasarimda ilgili spesifikasyon dogrudan zorlayicidir:

- `CISPR 25 Class 5` hedefi var

- `pi-stage` giris EMI filtresi ve elektrolitik parallel damping hedefi zaten specification notlarina girmis durumda



Bu nedenle differential-mode EMI yalnizca "sonradan filtre eklenir" mantigiyla ele alinmamalidir. Asagidaki basliklar guc kati tasariminin bir parcasi kabul edilmelidir:

- giris kapasitörlerinin ESL/ESR ve dc-bias altindaki gercek davranisi

- filtre once/sonra kapasitör dagilimi

- EMI bobini ve kapasitörlerle olusan rezonanslar

- damping ihtiyaci

- layout kaynakli ek parasitikler



Bu bolumde ileride sayisal olarak eklenecek ana maddeler:

- filtre topolojisi

- diferansiyel-mod zayiflatma hedefi

- filtre kesim / rezonans frekanslari

- damping elemanlarinin gerekcesi



### 7.3 Common-mode EMI



Common-mode EMI, yalnizca giris akim dalgalanmasindan degil, hizli `dv/dt` ureten switch-node'un parasitik kapasiteler uzerinden cevreye enerji baglamasindan da kaynaklanir. Bu nedenle ortak-mod acisindan en hassas bolgeler:

- switch-node bakiri

- yarim kopruye yakin sicak alanlar

- kabloya, saseye veya olcum duzenine kapasitif bag yapabilecek yuzeyler



Bu tasarimda common-mode davranisini iyilestirmek icin ilk yerlesim refleksleri sunlar olmalidir:

- switch-node bakirini gereksiz buyutmeme

- feedback, COMP ve benzeri kucuk isaret hatlarini switch-node altindan veya yakinindan gecirmeme

- kirli guc alani ile sakin analog alani fiziksel olarak ayirma

- gerekiyorsa shield / ekranlama seceneklerini en sonda degerlendirme



Common-mode EMI bu asamada henuz hesaplanmis degildir; ama layout kararlarini bastan yonlendiren bir sinir olarak yazilmalidir.



### 7.4 Giris filtresi kararliligi



Giris EMI filtresi eklendiginde mesele yalnizca bastirma elde etmek degil, donusturucunun bu filtre ile birlikte kararliligini korumaktir. Yerel notlarda ve acik sorular listesinde dogru olarak isaretlendigi gibi burada Middlebrook tarzi bir uyum kontrolu gerekecektir.



Bu bolumde korunacak ana fikir su olmali:

- EMI filtresinin cikis empedansi ile donusturucunun giris tarafi dinamik davranisi tehlikeli bir etkilesime girmemelidir

- filtre rezonansi kontrol dongusu ve guc kati ile problemli bir cift kutup / negatif empedans etkisi olusturmamalidir

- gerekli ise elektrolitik parallel damping veya ayri damping agi kullanilmalidir



Bu kisimda henuz final esitlik ve sayisal kontrol yoktur. Final turda su adimlar eklenecek:

1. secilen `L_f` ve `C_in,filter` ile rezonans frekansi hesaplanacak

2. damping yaklasimi secilecek

3. LTspice veya PSpice ile filtreli ve filtresiz giris davranisi karsilastirilacak

4. Middlebrook uyumu rapora acik bir kontrol maddesi olarak yazilacak



### 7.5 Yerlesim kurallari



Bu bolum, ileride eklenecek PCB layout incelemesinin kontrol listesi gibi kullanilmalidir. Bu turda korunacak ilk kurallar:

- giris MLCC'lerini high-side / low-side guc yoluna olabildigince yakin koy

- bootstrap yolu, gate-drive dongusu ve yarim kopru alanini kisa tut

- switch-node alanini minimumda tut; analog hatlari bu bolgeden uzaklastir

- feedback gerilimini gurultulu switch-node'dan degil, sakin cikis dugumunden Kelvin benzeri mantikla al

- analog toprak ile guc topragi iliskisini kontrolcu datasheet'indeki mantiga gore kur; kirli geri donus akislarini analog referans uzerinden gecirme

- giris EMI filtresinin "kirli taraf" ve "temiz taraf" yerlestirmesini fiziksel olarak ayir

- olcum ve dogrulama icin kritik dugumlere test noktasi birak



Sonraki turda bu genel kurallar, gercek PCB veya EVM layout gorselleri uzerinden maddelendirilerek daha somut hale getirilecektir.



## 8. LTspice Dogrulama Plani



Bu bolumde amac, simulasyonu yalnizca "dalga sekli cizdirme" araci gibi degil, hesap ve tasarim kararlarini sistematik olarak sinayan bir dogrulama hatti haline getirmektir.



### 8.1 Baslangic model seti



YENI klasorunde simdiden kullanilabilecek bazi aday dosyalar bulunuyor:

- `LTspice_InputFilterDesign/SyncBuck_average_CL.asc`

- `LTspice_InputFilterDesign/SyncBuck_average_CL_IF.asc`

- `LTspice_InputFilterDesign/SyncBuck_switching_CL.asc`

- `LTspice_InputFilterDesign/SyncBuck_switching_CL_IF.asc`

- `LTspice_InputFilterDesign/SyncBuck_switching_OL_IF.asc`

- `LM5146-Q1_PSPICE_TRANS/LM5146-Q1_TRANS.DSN`



Bu dosyalar dogrudan final tasarim modeli kabul edilmemelidir. Ancak giris filtresi etkisi, averaged / switching model farki ve uretici makromodelinin davranisi icin iyi birer baslangic noktasi olabilirler.



### 8.2 Simulasyon asamalari



Bu proje icin en temiz siralama su olabilir:

1. averaged model ile ilk kontrol ve giris filtresi etkisini anlamak

2. switching model ile ripple, duty sinirlari ve transient davranisini gormek

3. gerekiyorsa TI PSpice makromodeli ile denetleyiciye daha yakin bir dogrulama yapmak

4. en sonda parasitikler ve damping detaylarini eklemek



Bu sira, sorun oldugunda hangi katmanda bozulma basladigini ayirmayi kolaylastirir.



### 8.3 Steady-state kontrol listesi



Ilk tur steady-state dogrulamada en az su koseler bakilmalidir:

- $V_{in} = 24\,\text{V}$, minimum yuk

- $V_{in} = 24\,\text{V}$, maksimum yuk

- $V_{in} = 36\,\text{V}$, minimum yuk

- $V_{in} = 36\,\text{V}$, maksimum yuk



Bu koselerde kaydedilecek temel buyuklukler:

- `Vout` statik dogruluk

- cikis ripple

- giris ripple / giris akim darbeleri

- bobin akim ripple'i

- MOSFET gerilim ve akim stresleri

- tahmini verim veya kayip dagilimi



### 8.4 Dinamik testler



Bu tasarimin asil karakteri, yalnizca steady-state degil transientlerde ortaya cikacagi icin asagidaki testler zorunlu gorulmelidir:

- load step: `3.571 A -> 9 A`

- ters load step: `9 A -> 3.571 A`

- line step: `24 V <-> 36 V`

- startup / soft-start

- duty cycle ve `tON(min) / tOFF(min)` sinirlarina yakin koseler



Burada ozellikle izlenecek ciktilar:

- `14 V +- 20%` transient sinirinin asilip asilmayacagi

- settling suresi

- ringing ve olasi snubber ihtiyaci

- cikis bulk kapasitörsuz yaklasimin yeterli olup olmadigi



### 8.5 Giris filtresi ve EMI ile baglantili testler



Giris EMI filtresi eklenecegi icin filtreli ve filtresiz durumlar birlikte karsilastirilmalidir:

- filtre yokken temel davranis

- filtre varken line / load transient

- damping elemani varken ve yokken giris ringing'i

- averaged model ile filtre rezonansi yorumu

- gerekiyorsa LISN benzeri yapiyla ilk iletilen EMI fikir taramasi



Bu kisimda hedef, tam standart EMI olcumu yapmak degil; tasarim karari vermeye yetecek ilk elektronik dogrulamayi elde etmektir.



### 8.6 44 V transient notu



Specification notuna gore `44 V for up to 1 ms` olayi bir "survive" kosuludur; donusturucunun bu anda regule calismasi zorunlu degildir. Bu nedenle bu test ayri bir kategori gibi ele alinmalidir:

- asiri gerilim stresini gormek

- kritik dugumlerde abs max veya guvenlik payi asimi olup olmadigina bakmak

- gerekiyorsa clamp, TVS veya input filter revizyonunu dusunmek



### 8.7 Hesapla simulasyonun birlestirilmesi



Her simulasyon turu bir hesap maddesine baglanmalidir. En saglikli yontem su olur:

- her grafik icin hangi hesap veya spesifikasyon kontrol ediliyor acik yaz

- uyusmayan sonuc varsa once model varsayimini, sonra hesap varsayimini sorgula

- simulasyonu hesap yerine gecen bir "hakem" gibi degil, hesapla birlikte calisan ikinci goz gibi kullan



Kontrol listesi:

- [ ] steady-state

- [ ] load transient

- [ ] line transient

- [ ] startup

- [ ] min/max `Vin`

- [ ] min/max yuk

- [ ] kompanzasyon dogrulama

- [ ] ringing / snubber ihtiyaci

- [ ] giris filtresi etkisi



Simulasyon dosyalari notlari:

- Kullanilacak ana semalari burada ozetle.

- Gerekirse ilgili `asc` veya `DSN` dosyalarini bu bolumde bagla.



## 9. Secilmis Gorseller

Bu belgedeki ilk gorsel seti `yeni.odt` icinden cikartilan `26` gomulu gorselden olusur ve hepsi ilgili teknik bolumlere dagitilmistir.

Not:
- `/foto` dizinindeki arsiv gorselleri simdilik `yeni.md` icinde kullanilmiyor.
- ODT icinden cikartilan gorsellerin envanteri ve hedef bolumleri `tracking/odt_embedded_images.md` dosyasinda tutuluyor.
- Defter fotograflari sonraki fazda secilerek ayrica degerlendirilecek.

## 10. Acik Sorular



Ayrintili takip icin `tracking/open_questions.md` dosyasini kullan.



Kisa liste:

- [ ] Spesifikasyon tablosu donduruldu mu?

- [ ] Tum hesaplar kaynak ve birim kontrolu ile tekrar yazildi mi?

- [ ] Bootstrap hesaplari tekrar dogrulandi mi?

- [ ] Cikis kapasiteleri transient hedefini sagliyor mu?

- [ ] Giris kapasiteleri ripple ve RMS akisina gore dogrulandi mi?

- [ ] EMI icin ilk yerlesim kurallari yazildi mi?



## 11. Degisiklik Gunlugu



### 2026-03-20



- GitHub uyumlu ilk Markdown iskeleti olusturuldu.

- Bu belge, tez formati yerine yasayan muhendislik dokumani mantigina cevirdi.

- Ana calisma plani belgeye eklendi.

- `yeni.odt` icindeki giris ve genel yaklasim bolumu temizlenerek Markdown yapisina aktarilmaya baslandi.

- Specification tablosu G94 notlarina gore dolduruldu.

- `FB` divider ve duty / min on-off bolumu ilk turda yazildi.

- Bobin ve bootstrap bolumleri ilk teknik cerceveyle dolduruldu.

- MOSFET secimi ve kayiplar bolumu ilk teknik cerceveyle dolduruldu.

- Kontrolcu bolumunun modulator, K-factor ve ilk frekans yerlestirmesi iskeleti yazildi.

- Faz/kazanc marji icin ilk taslak hedef bandi eklendi.

- EMI, giris filtresi kararliligi ve layout bolumune ilk teknik omurga eklendi.

- LTspice/PSpice baslangic dosyalari taranarak simulasyon plani detaylandirildi.

- Cikis kapasiteleri bolumu, transient / output impedance / slew-rate mantigiyla genisletildi.

- Giris kapasiteleri bolumu, minimum Cin, RMS/thermal, ESL ve bulk mantigiyla genisletildi.

- Kontrolcu bolumune ilk donemden K-factor gecisi ve plant modelindeki ihmal notlari eklendi.

- `yeni.odt -> yeni.md` teknik aktarimi tamamlandi; acik kalan maddeler tasarim dogrulama fazina tasindi.

- Defter notlari fazi icin `foto/defter_raw/` ve `tracking/notebook_*` takip yapisi olusturuldu.

- Mevcut `foto/` arsivi icin ilk triage envanteri ve `B001` batch kaydi olusturuldu.

- Gorsel kullanim kurali netlestirildi: ilk secimler `yeni.odt` icindeki gomulu gorsellerden yapilacak, `/foto` simdilik kullanilmayacak.

- `yeni.odt` icindeki 26 gomulu gorsel `images/odt_embedded/` altina cikarildi.
- Bu 26 gorsel ilgili teknik bolumlere dagitildi; `/foto` arsivi ilk asamada disarida tutuldu.

- ODT'de kalan stability criterion ve compensator basliklari yeni.md icinde karsiliklandi.


























