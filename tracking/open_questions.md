# Open Questions

Bu dosya, yeniden bakilacak teknik noktalar icin kullanilir.

## Specifications

- [ ] Giris gerilimi araligi tek tabloda kesinlestirildi
- [ ] Cikis gerilimi ve akim hedefleri donduruldu
- [ ] Ripple ve transient limitleri tek kaynaga baglandi
- [ ] EMI ve termal hedefler acik yazildi

## Power Stage

- [ ] Duty cycle sinirlari tekrar kontrol edildi
- [ ] `tON(min)` ve `tOFF(min)` etkileri hesaplandi
- [ ] Bootstrap hesabinda kullanilan duty degerleri ile ideal duty hesabi uzlastirildi
- [ ] Bobin ripple akimi tekrar dogrulandi
- [ ] Secilen bobin adayinin L, DCR ve saturation degerleri datasheet ile dogrulandi
- [ ] Cikis kapasiteleri load transient acisindan tekrar kontrol edildi
- [ ] Cikis sapmasi ile kapali-cevrim cikis empedansi baglantisi netlestirildi
- [ ] Cikis bulk kapasitör eklenirse plant ve kompanzasyon etkisi sayisal olarak kontrol edildi
- [ ] Giris kapasiteleri RMS ve dc-bias etkileriyle tekrar kontrol edildi
- [ ] Kucuk degerli yardimci MLCC / dusuk ESL stratejisi simulasyonla dogrulandi

## MOSFET / Bootstrap

- [ ] Iletim ve switching kayiplari tekrar hesaplandi
- [ ] Secilen MOSFET adayinin RDS(on), Qg, Qgd ve termal verileri datasheet ile teyit edildi
- [ ] MOSFET current rating yorumlari termal varsayimlardan ayrilarak yeniden yazildi
- [ ] Bootstrap direnci ve kondansator secimi tekrar dogrulandi
- [ ] Bootstrap icin standart RC zaman sabiti ve direnç guc hesabi yeniden turetildi
- [ ] Termal varsayimlar sadece EVM cikarimina dayanmaktan cikarildi

## Control

- [ ] Modulator kazanci netlestirildi
- [ ] Power stage transfer fonksiyonu son haliyle yazildi
- [ ] Gercek plant transfer fonksiyonu secilen L/C/ESR/DCR ile yeniden kuruldu
- [ ] K-factor yerlestirmesi son kez kontrol edildi
- [ ] K-factor hesaplari ornek scriptlerden ayrilip bu tasarimin gercek sayilari ile tekrarlandi
- [ ] Faz ve kazanc marjlari hedefe gore dogrulandi
- [ ] Crossover frekansi fsw/10 tahmininden sonra gercek plant ile yeniden optimize edildi

## EMI / Layout

- [ ] Hot-loop alani tanimlandi
- [ ] Differential-mode kaynaklari listelendi
- [ ] Common-mode kaynaklari listelendi
- [ ] Giris filtresi ile Middlebrook uyumu kontrol edildi
- [ ] Giris EMI filtresi rezonans frekansi ve damping sayisal olarak hesaplandi
- [ ] Differential-mode zayiflatma hedefi simulasyon veya olcum mantigina baglandi
- [ ] Switch-node keep-out ve analog hat ayrimi gercek yerlesim uzerinde kontrol edildi
- [ ] Yerlesim kurallari acik maddeler halinde yazildi

## Simulation

- [ ] Steady-state simulasyonu hazir
- [ ] Startup simulasyonu hazir
- [ ] Load transient simulasyonu hazir
- [ ] Line transient simulasyonu hazir
- [ ] Parazitik etkiler icin ikinci tur model hazir


