# Refactoring Nedir?

## Temiz Kod (Clean Code)

Refactoringin ana amacı teknik borçla (technical debt) mücadele etmektir. Refactoring, bir karışıklığı temiz bir koda ve basit bir tasarıma dönüştürür.

Bunu cebe koyduk! Peki, temiz kod nedir? Bazı özelliklerini saymak gerekirse:

**🚿 Temiz kod, diğer programcılar için de oldukça nettir.**

Burada süper karmaşık algoritmaları kastetmiyoruz. Yanlış değişken isimlendirmesi, şişmiş sınıflar ve metotlar, sihirli sayılar (magic) tüm bunlar kodu dağınık ve anlaşılması zor hale getirir.

------------

**🚿 Temiz kod, tekrarlı kod (duplication) içermez.**

Yinelenen bir kodda her değişiklik yapmanız gerektiğinde, aynı değişikliği her örnekte yapmayı hatırlamanız gerekir. Bu durum bilişsel yükü artırır ve ilerlemeyi yavaşlatır.

------------

**🚿 Temiz kod minimum sayıda sınıf ve diğer hareketli parçaları (method, fonskiyon, değişken) içerir.**

Az kod, aklınızda tutmanız gereken az şey demektir. Az kod, daha az bakım demektir. Az kod, daha az hata demektir. Kod bir yükümdür, kodu kısa ve basit tutun.

------------

**🚿 Temiz kod, tüm testleri geçer.**

Testlerinizin yalnızca %95'i geçtiğinde kodunuzun kirli olduğunu bilirsiniz. Test kapsamınız %0 olduğunda yanıldığınızı bilirsiniz.

------------

**🚿 Temiz kodun bakımı daha kolay ve  maliyetsizdir!**


##  Teknik Borç (Technical Debt)

Herkes baştan harika bir kod yazmak için elinden gelenin en iyisini yapar. Projeye zarar vermek amacıyla kasıtlı olarak kirli kod yazan bir programcı olmadığı söylenebilir. Ancak temiz kod ne zaman kirli hale gelir?

Temiz olmayan kodla ilgili "teknik borç" metaforu aslında Ward Cunningham tarafından önerilmiştir.

Eğer bir bankadan kredi alırsanız, bu size daha hızlı alışveriş yapma imkanı tanır. Süreci hızlandırmak için ekstra ödeme yaparsınız. Sadece ana parayı değil, aynı zamanda kredi üzerinden bankanın belirlediği ek faizleri de ödersiniz. Tabii ki, faiz miktarı toplam gelirinizi aşacak kadar çok olabilir, bu da tam geri ödemeyi yani borcun tamamını ödemeyi imkansız hale getirir.

Aynı şey kodda da olabilir. Yeni özellikler için test yazmadan geçici olarak hızlanabilirsiniz, ancak bu durum, sonunda testler yazarak borcunuzu ödeyene kadar ilerlemenizi her gün parça parça yavaşlatacaktır.

### Technical Debt Nedenleri

**▶️ İş Baskısı**

Bazen iş koşulları, özellikleri tamamen bitmeden devreye sokmanızı yani kullanıma sunmaya zorlayabilir.. Bu durumda, projenin tamamlanmamış kısımlarını gizlemek için kod içinde yamalar ve geçici çözümler ortaya çıkabilir.

--------

**▶️ Teknik Borcun Sonuçlarının Anlaşılmaması**

Bazen işvereniniz, teknik borcun birikmesiyle birlikte gelişimin hızını yavaşlattığı durumu yani "faiz" konseptini anlamayabilir. Bu durum, yönetimin bunun değerini görmemesi nedeniyle ekip zamanını refaktöre etmeye ayırmayı zorlaştırabilir.

--------

**▶️ Bileşenlerin (Components) Katı Tutarlılığıyla Mücadele Edememek**

Bu, projenin bireysel modüllerin değil, bir monolit gibi görünmesi durumudur. Bu durumda, projenin bir bölümünde yapılan herhangi bir değişiklik, projenin/kodun diğer kısımları üzerinde etkili olacaktır. Bireysel üye çalışmasını izole etmek zor olduğu için ekip geliştirmesi daha zor hale gelir.

--------

**▶️ Test Eksikliği**

Anında geri bildirimin eksikliği, hızlı ancak riskli çözümler veya geçici çözümlere teşvik eder. En kötü durumlarda, bu değişiklikler önceden test edilmeden doğrudan production'a yani yayındaki projeye uygulanabilir ve dağıtılabilir yani deploy alınabilir. Sonuçlar felaket olabilir. Örneğin, masum görünen bir hata düzeltmesi, binlerce müşteriye tuhaf bir test e-postası gönderebilir veya daha da kötüsü, bütün bir veritabanını silebilir veya bozabilir.

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

Projelerin gereksinimleri sürekli olarak değişiyor ve bir noktada kodun bazı kısımlarının eski moda, ağır ve yeni gereksinimlere uyacak şekilde yeniden tasarlanması gerektiği açık hale geliyor.

Öte yandan, projenin programcıları her gün eski parçalarla çalışan yeni kod yazıyorlar. Bu nedenle, refaktör geciktikçe, gelecekte bağımlı olacak kodun yeniden çalıştırılması gerekecek.

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
- Benzer bir şeyi ikinci kez yaptığınızda, tekrarlamaktan utanın ama yine de aynı şeyi yapın.
- Bir şeyi üçüncü kez yaptığınızda refactoring'e başlayın.

### Yeni Bir Özellik Eklerken

<div align="center">

![](https://refactoring.guru/images/refactoring/content/pages/r2.svg)

</div>

- Refaktör, başkalarının kodunu anlamanıza yardımcı olur. Başkalarının kirli koduyla uğraşmak zorunda kalırsanız, önce onu refaktör etmeye çalışın. Temiz kodu anlamak çok daha kolaydır. Onu sadece kendiniz için değil, sizden sonra kullananlar için de iyileştireceksiniz bunu unutmayın.

- Refaktör, yeni özellik eklemeyi kolaylaştırır. Temiz kod üzerinde değişiklik yapmak çok daha kolaydır.


### Bir Hatayı Düzeltirken

<div align="center">

![](https://refactoring.guru/images/refactoring/content/pages/r3.svg)

</div>

Kodun içindeki hatalar (bugs), gerçek hayattaki gibi davranır: kodun en karanlık, en kirli yerlerinde yaşarlar. Kodunuzu temizleyin ve hatalar neredeyse kendiliğinden ortaya çıkacaktır.

Yöneticiler, proaktif refaktörü, sonradan özel refaktör görevlerine gerek bırakmadığı için daha çok severler ve takdir ederler. Mutlu patronlar, mutlu programcılar demektir!

### Kod İncelemesi (Review) Sırasında

- Kod incelemesi, kodun genel kullanıma sunulmadan önce düzenlenmesi için son şanstır.
-   Bu incelemeleri geliştirmeyi yapan programcı ile birlikte çift olarak yapmak en iyisidir. Bu şekilde basit problemleri hızlı bir şekilde çözebilir ve daha zor olanları düzeltmek için zamanı planlayabilirsiniz.

## Refactoring Nasıl Yapılır?

Refaktör, mevcut kodu küçük değişiklikler serisi olarak ele almalıdır. Her bir değişiklik, mevcut programı çalışır durumda bırakarak mevcut kodu biraz daha iyi hale getirmelidir.

### Doğru Yapılan Refaktör İçin Kontrol Listesi

**✅ Kod temiz hale gelmelidir.**

Kod, yeniden düzenleme sonrasında da aynı şekilde kirli kalırsa... eh, üzgünüm ama hayatınızın bir saatini boşa harcadınız. Bunun neden olduğunu anlamaya çalışın.

Bu durum sıklıkla, küçük değişikliklerle yeniden düzenlemeden uzaklaştığınızda ve bir sürü yeniden düzenlemeyi tek bir büyük değişiklikte karıştırdığınızda meydana gelir. Bu nedenle aklınızı kaybetmeniz çok kolaydır, özellikle de zaman sınırınız varsa.

Bununla beraber aynı zamanda, son derece dağınık bir kodla çalışırken de gerçekleşebilir. Ne kadar iyileştirme yaparsanız yapın, genel olarak kod hâlâ bir felaket olabilir.
    
Bu durumda, kodun bazı bölümlerini tamamen yeniden yazmayı düşünmek faydalı olabilir. Ancak önce testler yazmalı ve yeterli bir süre ayırmalısınız. Aksi halde, ilk paragrafta konuştuğumuz türden sonuçlar elde edebilirsiniz.

**✅ Refaktör sırasında yeni işlevsellik oluşturulmamalıdır.**

Refaktörü, yeni özelliklerin doğrudan geliştirilmesiyle karıştırmayın. Bu süreçleri mümkünse en azından bireysel taahhütler içinde ayırmaya çalışın.

**✅ Refaktör sonrasında mevcut tüm testlerin geçmesi gerekir.**

Yeniden düzenleme sonrasında testlerin bozulabileceği iki durum vardır:

- Yeniden düzenleme sırasında bir hata yaptınız. Bu hiç akıllıca değil: devam edin ve hatayı düzeltin.
- Testleriniz çok düşük düzeyde (low-level) olabilir. Örneğin, sınıfların private yöntemlerini test etmeyi denemiş olabilirsiniz.

Bu durumda, testler suçludur. Testleri kendiniz yeniden düzenleyebilir veya tamamen yeni bir dizi üst düzey test yazabilirsiniz. Bu tür bir durumdan kaçınmanın harika bir yolu BDD (Behavior-driven Development) tarzı testler yazmaktır.




















