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

| Cikis akimi, turetilmis | `3.571 A - 8.929 A` | `Iout = Pout / Vout` |

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

$$
V_{in,\min} = 7.5\,\text{V} + 0.72\,\text{V} = 8.22\,\text{V}
$$

$$
V_{CC} \approx V_{in} - V_{dropout}
$$

Ornegin tipik dropout ile `V_{in} = 6\,\text{V}` icin:

$$
V_{CC} \approx 6.00\,\text{V} - 0.25\,\text{V} = 5.75\,\text{V}
$$

Bu bolgede teknik olarak su riskler one cikar:

- dusuk `VGS` nedeniyle `R_{DS(on)}` artisi ve iletim kayiplari
- gate sarj / desarj surelerinin uzamasi nedeniyle switching kayiplari
- koruma ve akim algilama marjlarinin degismesi
- gerekirse logic-level MOSFET veya harici `VCC` kullanim ihtiyaci

Bu nedenle tasarimda iki yol acik tutulmalidir:

- sistemi yalnizca `V_{in} \ge 8.22\,\text{V}` bolgesinde optimize etmek
- veya `DVCC` uzerinden harici `8\,\text{V} - 13\,\text{V}` `VCC` rayi ile LDO'yu baypas etmek



#### 5.1.3 Cikis gerilimi setpoint direncleri FB



ODT'den aktarilan metin (`4.1. Çıkış gerilimi setpoint dirençleri FB`):

> = 14V yapmak istiyoruz.  
> 0402 boyutlarında.  
> R14 Ohm R14 = 16.5 kΩ (RFB1)  
> R10 = 1.00 kΩ (RFB2)  
> Dirençleri % 0.1 tolerance’lı seçmek….



Ek not:

Bu parca, geri besleme bolucusunun `14 V` cikis hedefi icin secildigini ve fiziksel paket hedefinin `0402` oldugunu kaydediyor. GitHub uyumlu matematik bicimiyle temel setpoint iliskisi su sekilde yazilabilir:

$V_{out} = V_{FB}\left(1 + \frac{R_{FB1}}{R_{FB2}}\right)$

Burada $V_{FB} = 0.8\,\text{V}$, $R_{FB1} = 16.5\,\text{k}\Omega$ ve $R_{FB2} = 1.00\,\text{k}\Omega$ alinirse:

$V_{out} = 0.8\left(1 + \frac{16.5}{1.0}\right) = 14.0\,\text{V}$

Bu secim, hedef cikis gerilimi ile uyumludur. Ayrica `%0.1` toleransli direnç secimi, setpoint hatasini dusuk tutmak icin mantikli bir tercihtir.



### 5.2 Duty cycle, `tON(min)`, `tOFF(min)` ve sinir durumlari



#### 5.2.1 Birinci yaklasim ideal duty hesabi



Nominal ilk yaklasimda senkron buck donusturucu icin duty orani su sekilde alinabilir:



`D ~= Vout / Vin`



Buna gore mevcut spesifikasyonlarla:

- `Dmax,ideal = 14 / 24 = 0.583`

- `Dmin,ideal = 14 / 36 = 0.389`



Bu degerler, nominal `24 V - 36 V` giris araliginda high-side anahtarin iletim orani icin ilk taslak sinirlari verir.



#### 5.2.2 `332 kHz` icin zaman alanina ceviri



Mevcut tasarim seciminde:



`fsw = 332 kHz`



Buna gore periyot:



`Tsw = 1 / fsw ~= 3.01 us`



Ideal duty degerleri zaman alanina cevrilirse:

- `tON,max,ideal = 0.583 x 3.01 us ~= 1.76 us`

- `tON,min,ideal = 0.389 x 3.01 us ~= 1.17 us`

- `tOFF,min,ideal = (1 - 0.583) x 3.01 us ~= 1.25 us`

- `tOFF,max,ideal = (1 - 0.389) x 3.01 us ~= 1.84 us`



#### 5.2.3 `D_{max}` donanimsal sinirlari ve minimum on/off sureleri



ODT'den aktarilan metin (`6.1.4. Dmax,donanımsal..?`):

> LM5146-Q1’nun
> - 40-ns tON(min) for high VIN / VOUT ratio
> - 140-ns tOFF(min) for low dropout
> Main mosfet’ın iletimde olma süresi 40ns’den daha kısa olmaz.
> … Dikkat burası tam doğru olmayabilir. *** Bakarsın bir video vardı. Eklersin.
> Arada başka zamanlar da var. Ölü zaman gibi…
>
> {D} rsub {MAX,donanımsal} ≈ {{T} rsub {sw} "-"" " {t} rsub {off left (min right )}} over {{T} rsub {sw}} " "
> ≈" " {{{1} over {f}} rsub {sw} "-"" " {t} rsub {off left (min right )}} over {{{1} over {f}} rsub {sw}} " "≈" " {{1} over {332kHz} "-"" " 140nsaniye} over {{1} over {332kHz}}
> ≈" " {{{1} over {f}} rsub {sw} "-"" " {t} rsub {off left (min right )}} over {{{1} over {f}} rsub {sw}} " "≈" " {{1} over {332kHz} "-"" " 140nsaniye} over {{1} over {332kHz}}



Ek not:

Bu parca, denetleyicinin ideal duty hesabindan ayri olarak donanimsal zaman sinirlarini hatirlatiyor. ODT notunda da isaret edildigi gibi bu hesap, olu zaman ve diger ayrintilar ihmal edilirse yalnizca ilk yaklasim olarak kullanilmalidir.

GitHub uyumlu matematik bicimiyle donanimsal ust duty siniri su sekilde yazilabilir:

$$
D_{\max,\text{donanimsal}} \approx \frac{T_{sw} - t_{OFF(\min)}}{T_{sw}}
$$

$$
D_{\max,\text{donanimsal}} \approx \frac{\frac{1}{f_{sw}} - t_{OFF(\min)}}{\frac{1}{f_{sw}}}
$$

`f_{sw} = 332\,\text{kHz}` ve `t_{OFF(\min)} = 140\,\text{ns}` icin:

$$
D_{\max,\text{donanimsal}} \approx
\frac{\frac{1}{332\,\text{kHz}} - 140\,\text{ns}}{\frac{1}{332\,\text{kHz}}}
\approx 0.953
$$

Ayni yaklasimla en kucuk on-time sinirindan bir alt duty tahmini de elde edilebilir:

$$
D_{\min,\text{donanimsal}} \approx \frac{t_{ON(\min)}}{T_{sw}}
= t_{ON(\min)} f_{sw}
\approx 40\,\text{ns} \times 332\,\text{kHz}
\approx 0.013
$$

Bu nedenle nominal `24\,\text{V} - 36\,\text{V}` calisma araliginda gereken duty degerleri ilk bakista bu donanimsal sinirlarin icinde gorunmektedir; ancak olu zaman, propagation delay ve kontrolcuya ozgu diger zamanlar nihai yorumdan once ayrica dogrulanmalidir.



#### 5.2.4 Taslaktaki duty notlari ile fark



Bootstrap hesabinin icinde ayrica su duty notlari bulunuyor:

- `Dutyhigh,max = 0.648`

- `Dutyhigh,min = 0.432`

- buradan `Dlow,min = 1 - 0.648 = 0.352`



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

- bobin akiminin yukselme hizi birinci yaklasimda `diL/dt ~= VL/L` ile sinirlanir



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

$$
\Delta V_{out} \approx \Delta I_{load}\,\bigl|Z_{out}(f_c)\bigr|
$$

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
> ( Hata: Başvuru kaynağı bulunamadı.5 )



Ek not:

Bu parca, `Vripple` dusuncesini iki ayri tasarim koluna ayiriyor:

- bobin akimi dalgalanmasini azaltmak icin `L` degerini buyutmek
- anahtarlama frekansi civarinda `Zout`'u dusurmek

GitHub uyumlu matematik bicimiyle ilk sezgi su sekilde yazilabilir:

$$
\Delta I_L \propto \frac{1}{L}
$$

Yani `L` arttikca bobin akiminin tepeden tepeye dalgalanmasi azalir. Ancak ODT'de dogru sezildigi gibi, bunun karsiliginda hacim, maliyet ve transient cevap hizi etkilenebilir.

Ikinci yol ise `f_{sw}` civarindaki cikis empedansini dusurmektir. Bunun pratik anlami sunlardir:

- daha yuksek etkin kapasitans
- daha dusuk `ESR`
- uygun yerlesim ile daha dusuk parasitikler

Bu nedenle bu alt bolumde korunacak ana fikir sunudur: `Vripple` yalnizca bobin akim dalgalanmasi problemi degil, ayni zamanda `f_{sw}` civarindaki `Zout`'un sekillendirilmesi problemidir.



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


ODT'den aktarilan metin (G71 transient yorumu):

> G71’deki t1 anına kadar, inductor akımı yükü karşılayamadı, ancak t1 anından sonra karşılayabildi. delta_Vout un türevinin 0 (dVOUT/dt) olduğu yer aslında. Gerilim azalmasının durduğu ana karşılık geliyor bu t1 anı, aynı zamanda bu t1 anı, t_undershoot süresinin sonu anlamına da gelir.

![G71 cikis transient yontemi civari](images/odt_embedded/fig_03_g71_transient_yontemi.png)

Ani bir yuk akimi degisimi sirasinda `Vout`'un minimum noktasi ve `t1` ani.



Ek not:

Bu yorum, transient sirasinda cikis kapasitörlerinin enerji tamponu gorevini fiziksel olarak acikliyor. Yuk aniden arttiginda bobin akimi hemen yetisemedigi icin ilk fark cikis kapasitörlerinden cekilir. `t1` ani, gerilim dususunun durdugu ve matematiksel olarak su kosulun saglandigi an olarak yorumlanabilir:

$$
\frac{dV_{out}}{dt} = 0
$$

Bu an ayni zamanda `t_{undershoot}` suresinin sonu olarak da dusunulebilir. Yani `t1` sonrasinda bobin akimi, yuk talebini karsilayacak seviyeye yaklasmis olur ve kapasitörlerin tek basina akim verme rolu azalmaya baslar.



ODT gorselleri:

![Slew-rate hesabi civari - 1](images/odt_embedded/fig_04_slew_rate_gecici_davranis.png)

![Slew-rate hesabi civari - 2](images/odt_embedded/fig_05_slew_rate_notu.jpg)




#### 5.4.7 Acik teknik dogrulamalar



Bu bolumde daha sonra eklenecekler:

- cikis kapasitörleri icin etkin kapasitans hesabi

- ESR ve ESL etkisi

- cikis sapmasi ile kapali-cevrim cikis empedansi arasindaki bag

- MLCC sayisi ve de-rating hesabi

- G71 benzeri hizli-yuk yaklasimlarinin bu tasarima ne kadar gerekli oldugu

- cikista bulk kapasitör gerekip gerekmedigine dair nihai karar



### 5.5 Giris kapasiteleri



#### 5.5.1 Gecis yaklasimi



Taslakta cikis ve giris kapasiteleri bilerek farkli felsefelerle ele alinmis:

- cikista bulk kapasitörden kacinmak, kontrol tasarimini sade tutmak icin tercih edilmis

- giriste ise kontrol dongusu dogrudan etkilenmedigi icin bulk kapasitör kullanimi daha acik degerlendirilmis



Bu ayrim dogru bir muhendislik refleksi gosteriyor. Giris tarafinda karar yalnizca ripple ile degil, ayni zamanda EMI, RMS akim dayanimı ve giris transient dayanimi ile de ilgilidir.



#### 5.5.2 Yeni eklenen giris gereksinimleri



Taslakta bu iterasyonda eklenen yeni gereksinimler acikca yazilmis:

- `DeltaVIN_PP <= 0.24 V`

- `DeltaVIN_Tran <= 0.36 V`



Bu iki gereksinim, giris kapasitelerini onceki tasarima gore daha kritik hale getirmektedir.



#### 5.5.3 Secilen genel strateji



Taslak notlarin ozeti su stratejiye isaret ediyor:

- yuksek frekansli ripple bastirma icin MLCC kullan

- etkin kapasitans dususu nedeniyle dc-bias etkisini mutlaka hesaba kat

- RMS akim dayanimini yalnizca kapasitansla karistirma; termal dayanimi da kontrol et

- enerji depolama ve transient dayanimi icin bulk elektrolitik / polimer kapasitörleri degerlendir

- EMI ve yerlesim acisindan giris kapasitörlerini high-side drain ile low-side source arasindaki sicak donguye cok yakin yerlestir



#### 5.5.4 Minimum etkin `Cin` dusuncesi



ODT taslaginda `MLCC Cin Hesabi` altinda hedef, `DeltaVIN_PP <= 0.24 V` sinirini saglayacak minimum etkin kapasitansi bulmaktir. Burada dikkat edilmesi gereken mantik dogru sekilde not edilmis:

- once gerekli olan etkin `Cin` bulunur

- sonra bu deger dc-bias, tolerans ve gerilim sinifi altinda gercek component degerine cevrilir

- yani katalog `uF` degeri dogrudan kullanilmaz



Taslakta bir ara yaklasim olarak en kotu durumun `D ~= 0.5` civarinda alinmasi fikri geciyor. Bu kullanisli bir ilk muhendislik yaklasimi olabilir; ancak nihai yazimda bunun gercek `Vin/Vout` araligi icin tekrar kontrol edilmesi gerekir.



#### 5.5.5 RMS akimi ve termal bakis



ODT tarafinda dogru olarak vurgulanan ikinci konu, MLCC'lerin yalnizca kapasitans acisindan degil, RMS akim ve sicaklik artisi acisindan da boyutlandirilmasidir.



Taslakta bulunan ilk kaba notlar:

- full-load durumda `Iload ~= 9 A`

- ilk yaklasim olarak `Iin_rms ~= 0.5 x Iload`

- buna gore `Iin_rms ~= 4.5 Arms`



Bu sonuc simdilik kaba boyutlandirma notu olarak tutulabilir. ODT'de ayrica EVM uzerindeki sicaklik gozlemlerinden yararlanarak kart sicakligi ve komponent termal yukleri hakkinda ilk cikarimlar yapildigi goruluyor. Bu cikarimlar yararli olabilir; fakat final secim sadece EVM benzetimine degil, secilen komponent datasheet'lerine ve gercek RMS dagilimina dayanmalidir.



ODT gorselleri:

![MLCC RMS akimi kaba hesap](images/odt_embedded/fig_06_input_mlcc_rms_kaba_hesap.jpg)

![MLCC sicaklik / X7R-X5R civari](images/odt_embedded/fig_07_input_mlcc_temp_x7r_x5r.png)

![MLCC sicaklik civari - 1](images/odt_embedded/fig_08_input_mlcc_temp_grafik_01.png)

![MLCC sicaklik civari - 2](images/odt_embedded/fig_09_input_mlcc_temp_grafik_02.png)

![MLCC sicaklik civari - 3](images/odt_embedded/fig_10_input_mlcc_temp_grafik_03.png)




#### 5.5.6 ESL ve kucuk degerli yardimci MLCC'ler



ODT'de iki ek fikir daha var ve ikisi de degerli:

- MLCC'leri paralellemek etkin ESL'yi dusurmeye yardim eder

- `1 uF`, `0.1 uF` gibi daha kucuk ve fiziksel olarak daha kucuk paketli seramikler, yuksek frekansli spike ve ringing bastirma isinde yararli olabilir



Bu nedenle giris kapasitör agi tek tip ve tek degerli dusunulmemelidir. Buyuk degerli ana MLCC'lere ek olarak, kucuk degerli dusuk-ESL yardimci kapasitörler de anahtarlama kenarlarindaki cok yuksek frekansli akis icin faydali olabilir.



Ancak bu kisim tamamen yerlesime bagimlidir; dolayisiyla final karar PCB veya LTspice parasitik modeli ile birlikte verilmelidir.



ODT gorselleri:

![MLCC ESL ve paralelleme civari - 1](images/odt_embedded/fig_11_input_esl_parallel_mlcc.png)

![MLCC ESL ve paralelleme civari - 2](images/odt_embedded/fig_12_input_esl_not_01.png)

![MLCC ESL ve paralelleme civari - 3](images/odt_embedded/fig_13_input_esl_not_02.png)

![MLCC ESL ve paralelleme civari - 4](images/odt_embedded/fig_14_input_esl_not_03.png)

![Kucuk MLCC ekleme civari - 1](images/odt_embedded/fig_15_small_mlcc_high_freq_01.png)

![Kucuk MLCC ekleme civari - 2](images/odt_embedded/fig_16_small_mlcc_high_freq_02.png)




#### 5.5.7 Bulk kapasitör secimi mantigi



Taslakta giris bulk kapasitörlerinin rolü su sekilde ciziliyor:

- MLCC'ler yuksek frekansta ve dusuk ESR tarafinda gucludur

- fakat dc-bias ve sicaklik altinda etkin kapasitanslari duser

- bu nedenle daha yavas enerji ihtiyaci, transient dayanimı ve damping tarafinda bulk kapasitörler destekleyici rol oynar



Bu mantik tutarlidir. Final secimde bulk kapasitörler, yuksek frekansli current loop'un ana parcasi gibi degil; enerji tamponu ve filtre / damping stratejisinin bir parcasi gibi ele alinmalidir.



ODT gorselleri:

![Giris bulk capacitor secimi - 1](images/odt_embedded/fig_17_input_bulk_selection_01.png)

![Giris bulk capacitor secimi - 2](images/odt_embedded/fig_18_input_bulk_selection_02.png)

![Giris bulk capacitor secimi - 3](images/odt_embedded/fig_19_input_bulk_selection_03.png)

![Giris bulk capacitor secimi - 4](images/odt_embedded/fig_20_input_bulk_selection_note.jpg)




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

- `RDS(on) ~= 8.2 mOhm`

- `Qg,total ~= 16 nC`

- `Cgd ~= 54 pF`

- `Qgd ~= 1.2 nC` yaklasimi

- `Vdrive ~= 7.5 V`

- `Rgate = 2.2 ohm` notu



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



#### 5.7.1 Taslaktaki secim mantigi



`yeni.odt` taslaginda bootstrap agi icin oldukca ayrintili bir ilk boyutlandirma notu bulunuyor. Temel mantik su sekilde kurulmus:

- bootstrap kondansatoru low-side iletim araliginda sarj olur

- bu nedenle en kritik durum, high-side iletim suresinin en uzun oldugu ve low-side sarj penceresinin en kisa kaldigi durumdur

- `Rboot` cok buyurse sarj yavaslar

- `Rboot` cok kuculurse pik akim ve ringing artar



Bu cerceve dogru bir muhendislik baslangicidir.



#### 5.7.2 Taslaktan alinan sayisal degerler



Taslakta kullanilan degerler su sekildedir:

- `Dutyhigh,max = 0.648`

- `Dutyhigh,min = 0.432`

- `Dlow,min = 1 - 0.648 = 0.352`

- `fsw = 332 kHz`

- `Tsw ~= 3.01 us`

- `tcharge,min = 0.352 x 3.01 us ~= 1.06 us`

- `Rboot = 2.2 ohm`

- `Cboot = 0.1 uF`

- `VDD = 8.4 V`

- `VBootDiode ~= 0.5 V`

- ilk yaklasimla `VCboot ~= 7.9 V`



Ayni taslakta tepe bootstrap sarj akimi de su sekilde hesaplanmis:



`Ipk ~= (8.4 - 0.5) / 2.2 ~= 3.6 A`



Bu deger, bootstrap diyodu ve kisa sarj dongusu uzerindeki PCB izleri icin anlamli bir ilk boyutlandirma kontroludur.



#### 5.7.3 Ilk fiziksel sanity check



Taslaktaki degerlerle standart RC zaman sabiti kullanilirsa:



`tau_std = Rboot x Cboot = 2.2 x 0.1 uF = 0.22 us`



Worst-case sarj penceresi ise:



`tcharge,min ~= 1.06 us`



Buna gore:



`tcharge,min / tau_std ~= 1.06 / 0.22 ~= 4.8`



Bu ilk kontrol, bootstrap kondansatorunun worst-case low-side iletim araliginda anlamli olcude sarj olabildigini gosterir. Dolayisiyla `Rboot = 2.2 ohm` ve `Cboot = 0.1 uF` secimi ilk bakista UVLO riski acisindan makul gorunmektedir.



#### 5.7.4 Yeniden turetilmesi gereken kisimlar



Taslak notlardaki bazi esitsizlikler ve formuller son hal olarak kabul edilmemelidir:

- taslakta yazilan `tau = Rboot x Cboot / Dlow,min` ifadesi standart RC zaman sabiti tanimi degildir

- `Eres ~= 3 x Cboot x Rboot` ifadesi boyutsal olarak suphelidir ve nihai enerji hesabi olarak kullanilmamalidir

- buna bagli direnç watt yorumu tekrar kurulmalidir



Bu nedenle bootstrap bolumunde su ayirim korunmalidir:

- sarj penceresi, pik akim ve ilk RC kontrolu faydali bir taslak hesap olarak saklanabilir

- enerji ve direnç guc derecelendirmesi ise fiziksel olarak tutarli bir modelle yeniden hesaplanmalidir



#### 5.7.5 Sonraki dogrulamalar



Bu bolume daha sonra su kontroller eklenecek:

- `Dutyhigh,max` degerinin nereden geldiginin acikca gosterilmesi

- `VDD` seviyesinin ve diyot dusumunun datasheet ile teyidi

- bootstrap diyotunun peak akim ve ters gerilim uygunlugu

- `Rboot` seciminin ringing ve sarj hizi acisindan simulasyonla gozlenmesi

- gerekiyorsa PSPICE / LTspice ile bootstrap dugumu dalga seklinin kontrolu



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



`yeni.odt` taslaginda modulator blogu icin su temel gozlem yapiliyor:

- modulator duty cycle'i belirler

- kompanzator cikisi arttikca high-side iletim suresi artar

- bu da duty cycle'in artmasi ve enerji transferinin buyumesi anlamina gelir



Yerel script notlarinda LM5146-Q1 icin modulator kazanci, ramp genligi uzerinden tanimlanmis:



`M = Vin / Vramp`



Ayni scriptte `Vramp = Vin / 15` alinmis ve boylece modulator kazanci fiilen sabit `15` olarak kullanilmistir. Bu, taslaktaki `input voltage feedforward` yorumuyla uyumludur: giris gerilimi degisirken ramp genligi de uygun sekilde degisiyorsa modulator kazanci yaklasik sabit tutulabilir.



Bu nokta cok onemlidir, cunku kontrol tasarimini sadeleştirir:



ODT gorseli:



![Modulator blogu](images/odt_embedded/fig_22_voltage_mode_modulator.png)



- `Vin` degisse bile modulator kazanci buyuk olcude sabit kalir

- bu da plant kazancindaki degisimi kismen telafi eder



#### 6.1.3 Power stage modeli



Yerel scriptte power stage, cikis filtresi ve yukun ikinci dereceden bir transfer fonksiyonu olarak ele alindigi goruluyor. Bu modelde su buyuklukler birlikte dusunuluyor:

- bobin `L`

- bobin seri direnci `r_L`

- cikis kapasiteleri ve etkin ESR etkisi

- yuk direnci `R`



Bu, taslaktaki anlatimla da uyumludur: cikis filtresi ve yuk birlikte kontrol dongusunun frekans cevabini belirler ve kompanzator secimi bu modele bakilarak yapilir.



ODT taslaginda ayrica cikista birden fazla kapasitör turu oldugu ve bunlardan cok kucuk degerli bir yuksek frekans kapasitörun ana transfer fonksiyonu hesabinda ihmal edildigi not edilmis. Bu yaklasim, eger ihmal edilen kapasitör yalnizca cok yuksek frekans bolgesinde etkiliyse ve crossover civarini anlamli degistirmiyorsa makul olabilir.



Ancak bu ihmal karari final raporda otomatik kabul edilmemelidir; secilen `fc` civarinda ve ust kutup / sifir konumlarinda gercekten etkisiz kaldigi ya analitik olarak ya da bode karsilastirmasi ile gosterilmelidir.



ODT gorselleri:

![Cikis filtresi ve yuk](images/odt_embedded/fig_23_output_filter_and_load.png)

![Cikis filtresi ve yuk / capacitorler](images/odt_embedded/fig_24_output_capacitor_bank.png)




#### 6.1.4 Bu bolumde korunacak ayrim



Su an icin iki ayri katman oldugu acik yazilmalidir:

- genel yontem ve blok diyagram mantigi

- gercek tasarima ait sayisal transfer fonksiyonu



`G103_comp.txt` icindeki sayilar faydali bir ornek / calisma dosyasidir; ancak bunlarin tamami dogrudan bu proje icin son deger olarak alinmamalidir. Nihai transfer fonksiyonu, bu projede secilen gercek `L`, `C`, `ESR`, `DCR`, yuk ve modulator parametrelerinden yeniden kurulacaktir.



### 6.2 K-factor yaklasimi



#### 6.2.1 Neden bu yontem?



`yeni.odt` taslagina gore ilk donemde kontrolcu tasarimi farkli bir yaklasimla yapilmisti. Ilk calismada, gerilim kontrollu Type-III / PID benzeri klasik kompanzator akisi izlenmisti. Bu iterasyonda ise K-factor yonteminin kullanilmasi hedefleniyor.



Bu gecis onemlidir cunku yalnizca farkli bir formulu denemek anlamina gelmez; kompanzator tasarimini daha sistematik, faz katkisi odakli ve tekrar kontrol edilebilir bir akisa tasir.



Bu degisim onemli cunku K-factor yaklasimi:

- hedef crossover frekansi icin gereken kompanzator kazancini sistematik kurar

- gerekli faz katkisini acikca hesaplamaya zorlar

- Type-III kompanzator komponentlerini daha izlenebilir bir akisla cikarir



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

- crossover frekansi kabaca `f_t = 0.1 x f_s`

- gerekli kompanzator kazanci, modulator ve plant buyukluklerinden turetiliyor

- `K = tan^2((phi_comp + 90 deg)/4)` tipinde standart K-factor iliskisi kullaniliyor



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



#### 6.2.5 Kompanzator topolojisi notu



ODT taslagindaki `Compensator` basligi ve onceki donem notlari birlikte okundugunda, bu tasarimda Type-III kompanzator topolojisinin korundugu anlasiliyor. Bu tercih mantiklidir cunku voltage-mode buck yapida guc katinin kutuplari ve istenen crossover seviyesi, ek faz katkisini gerekli kilabilir.



Burada onemli ayirim sunlardir:

- topolojinin Type-III olmasi, komponent degerlerinin de otomatik olarak dogru oldugu anlamina gelmez

- ODT'de kompanzator devresine ait temiz sayisal sonuclar yok; bu nedenle sayisal `R/C` degerleri sonraki turda gercek plant ile yeniden kurulacaktir

- yani kompanzatorun yapisi ODT'den aktarilmis kabul edilebilir, ama nihai boyutlandirma halen tasarim gorevidir



ODT gorselleri:

![Compensator / Type-III devre](images/odt_embedded/fig_25_type3_compensator.png)

![Compensator civari - 2](images/odt_embedded/fig_26_compensator_related.png)




### 6.3 Hedef frekans yerlestirmesi



#### 6.3.1 Ilk ilke



Yerel scriptlerde ve notlarda ilk yaklasim olarak crossover frekansinin anahtarlama frekansinin yaklasik onda biri civarinda secilmesi fikri kullaniliyor.



Bu tasarimda secilen:

- `fsw = 332 kHz`



olduguna gore ilk muhendislik tahmini su olur:



`fc,ilk ~= fsw / 10 ~= 33.2 kHz`



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



ODT'de `Kararlilik Kriteri` ayri bir baslik olarak geciyor. Eldeki temiz metin sinirli olsa da bu basligin yeni.md icindeki karsiligi su sekilde korunabilir:

- tek bir bode grafiginde sadece guzel gorunen egri aramak yeterli degildir

- loop'un uygun crossover bolgesinde `0 dB` gecisi, pozitif faz marji ve pozitif kazanc marji birlikte degerlendirilmelidir

- giris filtresi, parazitikler ve coklu kutup/sifir yapilari varsa stabilite yorumu sadece kabaca `fsw/10` kuralina birakilmamalidir



Bu nedenle bu projede kararlilik kriteri, yalnizca "simulasyon patlamadi" seviyesinde degil; margin, transient davranisi ve filtre etkisi birlikte kabul edilirse saglanmis sayilacaktir.



ODT gorseli:



![Kararlilik kriteri](images/odt_embedded/fig_21_stability_criterion.png)





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

- `Vin = 24 V`, minimum yuk

- `Vin = 24 V`, maksimum yuk

- `Vin = 36 V`, minimum yuk

- `Vin = 36 V`, maksimum yuk



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



























