# Bloaters (Şişkinlik)

Bloaters, üzerinde çalışılması zor olacak kadar devasa boyutlara ulaşmış kodlar, yöntemler ve sınıfları tanımlar. Genellikle bu kokular hemen ortaya çıkmaz, proje veya program geliştikçe zamanla birikir. Ayrıca kimsenin bu sorunun önüne geçmek için çaba göstermediğinde bu koku iyice kendini belli eder.

## 1️⃣ Uzun Metot (Long Method)

**🤢 Belirti ve Semptomlar**

Bir yöntem çok fazla kod satırı içeriyorsa bir sorun vardır. Genellikle on satırdan uzun herhangi bir yöntem ortaya çıktıysa bu yöntem ile ilgili soru işaretleriniz oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/long-method-01-2x.png)


**🤒 Sorunun Nedenleri**

Geliştirme sürecinde bir yönteme her zaman bir şeyler eklenir. Durumun tam aksi olarak ise kolay kolat hiçbir şey çıkarılmaz. Kod yazmak okumaktan daha kolaydır. Bundan dolayı bu koku, yöntem kontrol edilemez büyük boyutlu bir canavara dönüşene kadar fark edilmeden kalır.

Genellikle yeni bir yöntem oluşturmak, mevcut bir yönteme ekleme yapmaktan daha zordur: "Ama bu sadece birkaç satır kod, sırf bunun için bütün bir yöntem yaratmanın bir anlamı yok, var olana ekleyelim gitsin!" Bu düşünce, başka bir satırın eklendiği ve ardından yine birkaç satır eklendiği anlamına gelir. Aynı veya farklı geliştirici birer satır daha ekledikçe yöntem, bir spagetti kodu karmaşasına evrilir.

**💊 Tedavi**

Genel bir kural olarak durum şudur: Bir yöntemin içindeki bir alana yorum yapma ihtiyacı hissediyorsanız, bu kodu alıp yeni bir yönteme koymalısınız. Çünkü açıklama veya yorum gerektiriyorsa, o yöntem kendisine ait olmayan bir alan içeriyordur. Tek bir satır bile ayrı bir yönteme bölünebilir ve bölünmelidir de! Bununla beraber yöntemin açıklayıcı ve uygun bir adı varsa, kimsenin ne yaptığını görmek için yöntemin gövdesine bakmasına gerek kalmayacaktır.

![](https://refactoring.guru/images/refactoring/content/smells/long-method-02-2x.png)

- Bir metodun gövdesinin uzunluğunu azaltmak için **Extract Method** tekniği kullanın.

- Yerel değişkenler ve parametreler bir metodu ayrı bir metoda çıkarmayı engelliyorsa, **Replace Temp with Query**, **Introduce Parameter Object** veya **Preserve Whole Object** tekniklerinden uygun olanı/olanları kullanın.

- Eğer önceki yöntemlerin hiçbiri işe yaramazsa, tüm metodu **Replace Method with Method Object** tekniği kullanarak ayrı bir nesneye taşımaya çalışın.

- Koşullu operatörler ve döngülerin bulunması, kodun ayrı bir metoda taşınabileceğine dair birer ipucudur. Koşullular için **Decompose Conditional** tekniği kullanabilirsiniz. Döngüler engelliyorsa, **Extract Method** tekniğini deneyin.

**💰 Hesaplaşma**

- Tüm nesne yönelimli (OOP) kod türleri arasında, kısa metotlara sahip sınıflar her zaman en uzun ömürlü olanlardır. Bir metot veya fonksiyon ne kadar uzunsa, onu anlamak ve sürdürülebilir kılmak o kadar zor olur.

- Ayrıca, uzun metotlar istenmeyen kopya yani duplicate kodlar için mükemmel saklanma alanları sunar. Bundan sakınmalıyız.

**📈 Performans/Verim**

Birçok kaynakta iddia edildiği gibi, metot sayısındaki artış performansa zarar verir mi? Hemen hemen tüm durumlar göz önüne alındığında etki o kadar önemsizdir ki endişelenmeye bile değmez.

Ayrıca, bu yöntemle net ve anlaşılır bir kod elde ettiğinizden dolayı, gerektiğinde gerçek performans kazanımları elde etmek için kodu yeniden yapılandırmanın gerçekten etkili yollarını bulma olasılığınız daha yüksektir.


## 2️⃣ Large Class (Büyük Sınıf)

**🤢 Belirti ve Semptomlar**

Bir sınıf birçok alan (field), yöntem ve kod satırı içerdiğinde soru işaretleriniz oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/large-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Sınıflar genellikle küçük yapılar olarak başlar. Ancak program büyüdükçe büyüdükçe büyür ve zamanla şişerler.

Uzun yöntemler case'inde de olduğu gibi, programcılar genellikle mevcut bir sınıfa yeni bir özellik eklemeyi, o özellik için yeni bir sınıf oluşturmaya tercih ederler.

![](![](https://refactoring.guru/images/refactoring/content/smells/large-class-02.png)

**💊 Tedavi**

Bir sınıf çok fazla metot içeriyorsa, o sınıfı farklı sınıflara ayırmayı düşünün:

- **Extract Class** tekniği, büyük sınıfın davranışının bir kısmının ayrı bir bileşene/sınıfa dönüştürülebilmesi durumunda yardımcı olur.
- **Extract Subclass** tekniği, üst sınıfın davranışının bir kısmının farklı şekillerde alt sınıfta uygulanabilmesine veya nadir durumlarda kullanılmasına yardımcı olur.
- **Extract Interface** tekniği, istemcinin kullanabileceği işlem ve davranışların bir listesinin gerekli olması durumunda yardımcı olur.
- Projenin grafiksel arayüzden büyük bir sınıf sorumluysa, bazı verilerini ve davranışlarını ayrı bir etki alanı nesnesine (domain object) taşımayı deneyebilirsiniz. Bunu yaparken bazı verilerin kopyalarını iki yerde saklamak ve verilerin tutarlı olmasını sağlamak gerekebilir. **Duplicate Observed Data** tekniği bunu yapmanın bir yolunu sunar.

![](https://refactoring.guru/images/refactoring/content/smells/large-class-03-2x.png)

**💰 Hesaplaşma**

- Bu sınıfların yeniden düzenlenmesi yani refactor edilmesi, geliştiricilerin bir sınıf için çok sayıda özelliği hatırlama ihtiyacını ortadan kaldırır. Bu da geliştirme sürecine pozitif etki olarak yansıyacaktır.

- Çoğu durumda, büyük sınıfları parçalara ayırmak kod ve işlevlerin tekrarlanmasının yani duplicate olmasının önüne geçer.

## 3️⃣ Primitive Obsession (İlkel Takıntı)

**🤢 Belirti ve Semptomlar**

Aşağıdaki listelenmiş durumlarda soru işaretleriniz oluşmalıdır:

- Basit görevler için küçük nesneler yerine ilkel öğelerin (primitives) kullanılması (para birimi, aralıklar, telefon numaraları için özel dizeler vb.)
- Değişkenler için sabitlerin kullanılması (yönetici haklarına sahip kullanıcılara atıfta bulunmak için `USER_ADMIN_ROLE = 1` sabiti gibi.)
- Veri dizilerinde (data array) kullanılmak üzere field adları olarak dize (string) sabitlerinin kullanılması.

![](https://refactoring.guru/images/refactoring/content/smells/primitive-obsession-01-2x.png)

**🤒 Sorunun Nedenleri**

Repoda bulunan çoğu diğer koku gibi, ilkel (primitive) obsessions zayıf anlarda ortaya çıkar. Bir programcı "Sadece bazı verileri depolamak için bir alan!" diye düşünebilir. Bir ilkel alan oluşturmak, tamamen yeni bir sınıf yapmaktan çok daha kolaydır. Öyle değil mi? Ve öyle de olur. Sonra başka bir alan gerektiğinde yine aynı şekilde bir primitive değer daha eklenmiş olur. Böylelikel zamanla sınıf büyük ve kullanışsız hale evrilecektir.

İlkel veriler sıklıkla türleri (type) simule etmek için kullanılır. Yani ayrı bir veri türü yerine, bir varlık için izin verilen değerler listesini oluşturan bir dizi sayı veya dizeye (string) sahip olursunuz. Anlaşılması kolay isimler daha sonra bu belirli sayılara ve dizelere sabitler aracılığıyla verilir, bu yüzden geniş bir alana yayılırlar.

İlkel kullanımının kötü bir örneği, alan taklidi (field simulation) yapmaktır. Sınıf, çeşitli veri ve dize sabitlerini içeren büyük bir diziyi içerir. Bu verilere erişmek için sınıfta belirtilenler/tanımlanan dize sabitleri dizi indisleri olarak kullanılır.

**💊 Tedavi**

- Çok çeşitli ilkel (primitive) alanlarınız varsa, bunlardan bazılarını mantıksal olarak kendi sınıflarında gruplandırmak daha sağlıklı olabilir. Daha da iyisi, bu verilerle ilişkili davranışları da ayrı bir sınıfa taşıyabilirsiniz. Bu görev için **Replace Data Value with Object** tekniğine bir göz atabilirsinz.

![](https://refactoring.guru/images/refactoring/content/smells/primitive-obsession-02-2x.png)


- İlkel alanların değerleri metot parametrelerinde kullanılıyorsa, **Introduce Parameter Object** veya **Preserve Whole Object** tekniklerini kullanın.

- Karmaşık veriler değişkenlerde kodlandığında, **Replace Type Code with Class**, **Replace Type Code with Subclasses** veya **Replace Type Code with State/Strategy** kullanın.

- Değişkenler arasında diziler varsa, **Replace Array with Object** kullanın.

**💰 Hesaplaşma**

- İlkel veriler yerine, nesnelerin tercih edilmesi sayesinde kod daha esnek hale gelir.
- Kodun daha iyi anlaşılabilir ve düzenli hale gelir. Belirli veriler üzerindeki operasyonlar, dağınık olmaktansa aynı yerde toplanıt. Böylelikle bu  sabitlerin neden dizide oldukları hakkında daha fazla tahmin yapmaya gerek olmayacaktır, her şey oldukça net hale gelir.
- Kopya (duplicate) kodun daha kolay bulunması.

## 4️⃣ Long Parameter List (Uzun Parametre Listesi)

**🤢 Belirti ve Semptomlar**

Bir yöntem için üç veya dörtten fazla parametre bulunması durumunda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/long-parameter-list-01-2x.png)

**🤒 Sorunun Nedenleri**

Birkaç algoritma türü tek bir yöntemde birleştirildiğinde uzun bir parametre listesi ortaya çıkabilir. Hangi algoritmanın nasıl çalıştırılacağını kontrol etmek için uzun bir liste ile karşı karşıya gelebilirsiniz.

Uzun parametre listeleri sorunu, sınıfları birbirinden daha bağımsız hale getirirken bir yan etki olarak da oluşabilir. Örneğin, bir yöntemde ihtiyaç duyulan belirli nesneleri oluşturmaya yönelik kodunuz, yöntemden yöntemi çağırmaya yönelik bir koda taşıdınız. Aynı zamanda oluşturulan nesneler yönteme parametre olarak aktarıldı. Böylece orijinal sınıf artık nesneler arasındaki ilişkileri bilmez ve bağımlılıklar azalır. Ancak bu nesnelerden birkaçı oluşturulursa, her biri kendi parametresine ihtiyaç duyacaktır. Bu da daha uzun bir parametre listesi anlamına gelir. Oldukça beyin yakıcı bir hal aldı değil mi!

Uzadıkça çelişkili hale gelen ve kullanımı zorlaşan bu tür listeleri anlamak zordur. Uzun bir parametre listesi yerine, bir yöntem kendi nesnesinin verilerini kullanabilir. Geçerli nesne gerekli tüm verileri içermiyorsa, yöntem parametresi olarak (gerekli verileri alacak olan) başka bir nesne daha iletilebilir.

**💊 Tedavi**

- Parametrelere hangi değerlerin aktarıldığını kontrol edin. Eğer bazı argümanlar sadece başka bir nesnenin metot çağrılarının sonuçları ise, **Replace Parameter with Method Call** tekniğini kullanın. Bu nesne, kendi sınıfının alanına yerleştirilebilir veya bir metot parametresi olarak aktarılabilir.

- Başka bir nesneden alınan bir veri grubunu parametre olarak geçirmek yerine, **Preserve Whole Object** tekniğini kullanarak nesnenin kendisini yönteme geçirin.

- Diğer yandan, bu parametreler farklı kaynaklardan geliyorsa, **Introduce Parameter Object** tekniği kullanarak bunları tek bir parametre nesnesi olarak geçirebilirsiniz.

![](https://refactoring.guru/images/refactoring/content/smells/long-parameter-list-02-2x.png)

**💰 Hesaplaşma**

- Daha okunabilir, daha kısa kod sunar.
- Refactoring, daha önce fark edilmeyen yinelenen kodu ortaya çıkarabilir böylelikle daha temiz bir kod base'e ulaşırsınız.

**🤫 Ne Zaman Yok Sayılmalı?**

Sınıflar arasında istenmeyen bağımlılığa neden olacaksa parametrelerden kurtulmayın.

## 5️⃣ Data Clumps (Veri Kümeleri)

**🤢 Belirti ve Semptomlar**

Bazen kodun farklı bölümleri bir veritabanına bağlanma parametreleri gibi aynı değişken gruplarını içerebilir. Bu kümelerin kendi sınıflarına dönüştürülmesi gerekmektedir.

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-01-2x.png)

**🤒 Sorunun Nedenleri**

Genellikle bu veri grupları zayıf program yapısından veya kopyala yapıştır geliştirmelerden kaynaklanmaktadır.

Bazı verilerin veri kümesi (data clump) olup olmadığından emin olmak istiyorsanız veri değerlerinden birini silin ve diğer değerlerin hâlâ anlamlı olup olmadığına bakın. Durum böyle değilse, bu değişken grubunun bir nesnede birleştirilmesi gerektiğine dair iyi bir işaret sunar.

**💊 Tedavi**

- Tekrar eden veri, bir sınıfın alanlarını (field) oluşturuyorsa, **Extract Class** tekniğini kullanarak bu alanları kendi sınıflarına taşıyın.

- Aynı veri yığınları metod parametrelerinde geçiyorsa, **Introduce Parameter Object** tekniğini kullanarak bunları bir sınıf olarak ayırın.

- Yalnızca bireysel alanlar değil genel olarak verilerin bazıları diğer metotlara geçiriliyorsa, , tüm veri nesnesini metoda geçirmeyi düşünün. **Preserve Whole Object** tekniği bu konuda yardımcı olacaktır.

- Bu alanların kullandığı kodlara bakın. Bu kodu bir veri sınıfına taşımak iyi bir fikir olabilir.

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-02-2x.png)

**💰 Hesaplaşma**

- Kodun anlaşılmasını ve organizasyonunu geliştirir. Belirli veriler üzerindeki işlemler artık kod boyunca gelişigüzel bir şekilde yerine tek bir yerde toplanır. Böylelikle gerektiğinde düzenlemeler daha kolay yapılacaktır.

- Kod boyutunu azaltır. Daha az kod, daha az koku demektir!

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-03-2x.png)


**🤫 Ne Zaman Yok Sayılmalı?**

Bir nesnenin yalnızca değerlerini (ilkel türler) iletmek yerine, bir yöntemin parametrelerinde tamamını geçirmek, iki sınıf arasında istenmeyen bir bağımlılık yaratabilir.





