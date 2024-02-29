# Refactoring Nedir?

## Temiz Kod (Clean Code)

Refactoringin ana amacı teknik borçla (technical debt) mücadele etmektir. Refactoring, bir kodda bulunan karışıklığı temiz bir koda ve basit bir tasarıma dönüştürmemize yardımcı olur.

Bu kısmı cebe koyduk! Peki, herkesin dilinden düşmeyen bu temiz kod nedir? Bazı özelliklerini kısaca saymak gerekirse:

**🚿 Temiz kod, diğer programcılar için de oldukça nettir.**

Burada süper karmaşık algoritmalardan bahsetmiyoruz. Yanlış değişken isimlendirmesi, içeriği şişmiş sınıflar ve metotlar, sihirli (magic) sayılar ve değişkenler tüm bunlar kodu kirli ve anlaşılması zor hale getirir.

------------

**🚿 Temiz kod, tekrarlı kod (duplication) içermez.**

Birka yerde yinelenen yani tekrarlanan bir kod parçası olduğunu düşünün. Bu tekrarlanan kod üzerinde her değişiklik yapmanız gerektiğinde, bu değişikliği her tekrarlanan kod üzerinde yapmayı ihmal etmemeniz gerekir. Bu durum bilişsel yükü artırır ve projenizin ilerlemesini yavaşlatır.

------------

**🚿 Temiz kod minimum sayıda sınıf, method, fonskiyon ve değişken içerir.**

Az kod, aklınızda tutmanız gereken az şey demektir. Az kod, daha az bakım demektir. Az kod, daha az hata demektir. Kod bir yükümdür, kodu kısa ve basit tutmak hem siz hem de projede çalışan diğer yazılımcılar için verimi arttıracaktır.

------------

**🚿 Temiz kod, tüm testleri geçer.**

Testlerinizin yalnızca %95'i başarıyla testi geçtiğinde kodunuzda bir sorun olduğunu bilirsiniz. Test kapsamınız %0 olduğunda da yanıldığınızı bilirsiniz.

------------

**🚿 Temiz kodun bakımı daha kolay ve  maliyetsizdir!**


##  Teknik Borç (Technical Debt)

Herkes başlangıçta harika bir kod yazmak için elinden gelenin en iyisini yapar. Projeye zarar vermek amacıyla kasıtlı olarak kirli kod yazan bir programcı olmadığı söylenebilir. Ancak temiz kod ne zaman kirli hale gelir? Geliştirme aşamasında hangi süreçler projedeki temiz kodu bozar?

Temiz olmayan kodla ilgili **teknik borç** (technical debt) metaforu aslında Ward Cunningham tarafından önerilmiştir.

Bir bankadan kredi alırsanız, bu size daha hızlı alışveriş yapma imkanı sunar. Alışveriş sürecini hızlandırmak için ekstra (banka faizi) ödeme yaparsınız. Sadece ana parayı değil, aynı zamanda kredi üzerinden bankanın belirlediği ek faizleri de ödemeniz gerekir. Elbette ki, alışverişle birlikte birleşen faiz miktarı toplam gelirinizi aşacak kadar çok olabilir. Bu da borcunuzu tam olarak geri ödemenizi yani borcun tamamını ödemeyi imkansız hale getirir.

Aynı şey kodda da olabilir. Yeni özellikler için test yazmadan geliştirme sürecini geçici olarak hızlanabilirsiniz. Ancak bu durum, sonunda testler yazarak borcunuzu ödeyene kadar ilerlemenizi her gün parça parça yavaşlatacaktır.

### Technical Debt Nedenleri

**▶️ İş Baskısı**

Bazen iş koşulları, sizi yeni özellikleri (feature) tamamen bitmeden devreye sokmanıza yani kullanıma sunmaya zorlayabilir.. Bu durumda, projenin tamamlanmamış kısımlarını gizlemek için kod içinde yamalar ve geçici çözümler ortaya kullanırsınız.

--------

**▶️ Teknik Borcun Sonuçlarının Anlaşılmaması**

Bazen işvereniniz, teknik borcun birikmesiyle birlikte gelişimin hızını düşürdüğü durumu yani **faiz** konseptini anlamayabilir. Bu durum, yönetimin bunun değerini görmemesi nedeniyle, ekip zamanını refaktöre etmeye ayıramaz.

--------

**▶️ Bileşenlerin (Components) Katı Tutarlılığıyla Mücadele Edememek**

Bu, projenin bireysel modüllerin değil, bir monolit gibi görünmesi durumudur. Bu durumda, projenin bir bölümünde yapılan herhangi bir değişiklik, projenin/kodun diğer kısımları üzerinde etkili olacaktır. Bireysel üye çalışmasını izole etmek zor olduğu için ekip geliştirmesi daha zor hale gelir.

--------

**▶️ Test Eksikliği**

Anında geri bildirimin eksikliği, geliştiriciyi hızlı ancak riskli çözümler veya geçici çözümlere sürükler. En kötü durumlarda, bu değişiklikler önceden test edilmeden doğrudan production'a yani yayında olan projeye uygulanabilir, dağıtılabilir yani deploy alınabilir. Böyle bir durumda sonuçlar felaket olabilir. Örneğin, masum görünen bir hata düzeltmesi, binlerce müşteriye tuhaf bir test e-postası gönderebilir veya daha da kötüsü, bütün bir veritabanını bozabilir hatta silebilir.

--------

**▶️ Belgeleme Eksikliği**

Bu, yeni insanların projeye tanıtılmasını yavaşlatır ve projeden önemli kişiler ayrıldığında gelişimi duraklatabilir.

--------

**▶️ Ekip Üyeleri Arasındaki Etkileşim Eksikliği**

Eğer bilgi tabanı şirket genelinde dağılmamışsa, insanlar süreçler ve projeye ilişkin bilgilerle güncel olmayan bir anlayışla çalışma eğiliminde olabilirler. Bu durum, genç geliştiricilerin mentörleri tarafından yanlış eğitildiğinde daha da kötüleşebilir.

--------

**▶️ Aynı Anda Birkaç Dalda (Branch) Uzun Vadeli Geliştirme**

Bu, teknik borcun birikmesine yol açabilir ve değişiklikler birleştirildiğinde bu borç artar. İzolasyon içinde yapılan değişiklikler ne kadar fazlaysa, toplam teknik borç o kadar büyük olur.

--------

**▶️ Refaktörün Gecikmesi/Ertelenmesi**

Projelerin gereksinimleri sürekli olarak değişiyor ve bir noktada kodun bazı kısımlarının eski moda, ağır ve yeni gereksinimlere uyacak şekilde yeniden tasarlanması gerektiği açık hale gelir.

Öte yandan, projenin programcıları her gün eski parçalarla çalışan yeni kod yazacaklardır. Bu nedenle, refaktör geciktikçe, gelecekte değiştirilmesi gereken alana bağımlı kod parçaları kar topu etkisiyle büyümüş olacaktır.

--------

**▶️ Uyumluluk İzleme Eksikliği**

Bu, proje üzerinde çalışan herkesin uygun gördüğü şekilde (yani son projeyi yazdığı şekilde) kod yazdığında gerçekleşir.

--------

**▶️ Yetersizlik**

Bu, geliştiricinin sadece düzgün kod yazmayı bilmediği durumdur.


## Ne Zaman Refactoring Yapılmalı?

Bu konuda oldukça popüler olan ve takip edilen bir kural vardır. Rule of Three:

<div align="center">

![](https://refactoring.guru/images/refactoring/content/pages/r1.svg)

</div>

- Bir şeyi ilk kez yapıyorsanız, hemen yapın.
- Benzer bir şeyi ikinci kez yaptığınızda, tekrarlamak zorunda kaldığınız için utanın ama yine de aynı şeyi yapın.
- Bir şeyi üçüncü kez yaptığınızda refaktör etmeye başlayın.

### Yeni Bir Özellik Eklerken

<div align="center">

![](https://refactoring.guru/images/refactoring/content/pages/r2.svg)

</div>

- Refaktör, başkalarının kodunu anlamanıza yardımcı olur. Başkalarının kirli koduyla karşı karşıya kalırsanız, önce onu refaktör etmeye çalışın. Temiz kodu anlamak çok daha kolaydır. Bunu sadece kendiniz için değil, sizden sonra kullananlar için de iyileştireceksiniz bunu unutmayın.

- Refaktör, yeni özellik eklemeyi kolaylaştırır. Temiz kod üzerinde değişiklik yapmak çok daha kolaydır.


### Bir Hatayı Düzeltirken

<div align="center">

![](https://refactoring.guru/images/refactoring/content/pages/r3.svg)

</div>

Kodun içindeki hatalar (bugs), gerçek hayattaki gibi davranır: kodun en karanlık, en kirli yerlerinde yaşarlar. Kodunuzu temizleyin ve hatalar neredeyse kendiliğinden ortaya çıkacaktır.

Yöneticiler, proaktif refaktörü, sonradan özel refaktör görevlerine gerek bırakmadığı için daha çok severler. Ayrıca takdir ederler. Mutlu patronlar, mutlu programcılar demektir!

### Kod İncelemesi (Review) Sırasında

- Kod incelemesi, kodun genel kullanıma sunulmadan önce düzenlenmesi için son şanstır.
- Bu incelemeleri geliştirmeyi yapan programcı ile birlikte çift olarak yapmak en iyisidir. Bu şekilde basit problemleri hızlı bir şekilde çözebilir ve daha zor olanları düzeltmek için zamanı planlayabilirsiniz.

## Refactoring Nasıl Yapılır?

Refaktör, mevcut kodu küçük değişiklikler serisi olarak ele almalıdır. Her bir değişiklik, mevcut programı çalışır durumda bırakarak mevcut kodu biraz daha iyi hale getirmelidir.

### Doğru Yapılan Refaktör İçin Kontrol Listesi

**✅ Kod temiz hale gelmelidir.**

Kod, yeniden düzenleme sonrasında da aynı şekilde kirli kalırsa... eh, üzgünüm ama hayatınızın bir saatini boşa harcadınız. Bu durumun neden olduğunu anlamaya çalışın.

Bu durum sıklıkla, küçük değişikliklerle yeniden düzenlemeden uzaklaştığınızda ve bir sürü yeniden düzenlemeyi tek bir büyük değişiklikte karıştırdığınızda meydana gelir. Bu nedenle aklınızı kaybetmeniz çok kolaydır, özellikle de zaman konusunda endişeniz varsa.

Bununla beraber aynı zamanda, son derece dağınık bir kodla çalışırken de gerçekleşebilir. Ne kadar iyileştirme yaparsanız yapın, genel olarak kod hâlâ bir felaket olabilir.
    
Bu durumda, kodun bazı bölümlerini tamamen yeniden yazmayı düşünmek faydalı olabilir. Ancak önce testler yazmalı ve yeterli bir süre ayırmalısınız. Aksi halde, ilk paragrafta konuştuğumuz türden sonuçlar elde edebilirsiniz.

**✅ Refaktör sırasında yeni işlevsellik oluşturulmamalıdır.**

Refaktörü, yeni özelliklerin doğrudan geliştirilmesiyle karıştırmayın. Bu süreçleri mümkünse en azından bireysel taahhütler içinde ayırmaya çalışın.

**✅ Refaktör sonrasında mevcut tüm testlerin geçmesi gerekir.**

Refactoring sonrasında testlerin bozulabileceği iki durum söz konusu olabilir:

- Yeniden düzenleme sırasında bir hata yapmış olabilirsiniz. Bu hiç akıllıca değil: devam etmeli ve bir şekilde hatayı düzeltmelisiniz.
- Testleriniz düşük düzeyde (low-level) olabilir. Örneğin, sınıfların private yöntemlerini test etmeyi denemiş olabilir ve testler başarısızlıkla sonuçlanmış olabilir.

Bu durumda, sorun testlerdedir. Bu durumda iki seçeneğiniz var: Testleri kendiniz yeniden düzenleyebilir veya tamamen yeni bir dizi üst düzey yani kapsamlı test yazabilirsiniz. Bu tür bir durumdan kaçınmanın harika bir yolu BDD (Behavior-driven Development) prensibine dayalı testler yazmaktır.




















