# Bloaters (Şişkinlik)

Bloaters, üzerinde çalışılması zor olacak kadar devasa boyutlara ulaşan kodlar, yöntemler ve sınıflardır. Genellikle bu kokular hemen ortaya çıkmaz, program geliştikçe zamanla birikir (ve özellikle de hiç kimse onları yok etmek için çaba göstermediğinde).

## Uzun Metot (Long Method)

**🤢 Belirti ve Semptomlar**

Bir yöntem çok fazla kod satırı içeriyorsa bir sorun vardır. Genel olarak on satırdan uzun herhangi bir yöntem soru sormaya başlamanızı sağlamalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/long-method-01-2x.png)


**🤒 Sorunun Nedenleri**

Hotel California'da olduğu gibi, bir yönteme her zaman bir şeyler eklenir ama hiçbir şey çıkarılmaz. Kod yazmak okumaktan daha kolay olduğundan, bu "koku", yöntem çirkin, büyük boyutlu bir canavara dönüşene kadar fark edilmeden kalır.

Zihinsel olarak, yeni bir yöntem oluşturmak, mevcut bir yönteme eklemekten genellikle daha zordur: "Ama bu sadece iki satır, sırf bunun için bütün bir yöntem yaratmanın bir anlamı yok..." Bu düşünce, başka bir satırın eklendiği ve ardından yine de eklendiği anlamına gelir. Bir diğeri, bir spagetti kodu karmaşasını doğurur.

**💊 Tedavi**

Genel bir kural olarak, bir yöntemin içindeki bir şeye yorum yapma ihtiyacı hissederseniz, bu kodu alıp yeni bir yönteme koymalısınız. Açıklama gerektiriyorsa, tek bir satır bile ayrı bir yönteme bölünebilir ve bölünmelidir. Bununla beraber yöntemin açıklayıcı bir adı varsa, kimsenin ne yaptığını görmek için koda bakmasına gerek kalmayacaktır.

![](https://refactoring.guru/images/refactoring/content/smells/long-method-02-2x.png)

- Bir metot gövdesinin uzunluğunu azaltmak için **Extract Method** kullanın.

- Yerel değişkenler ve parametreler bir metodu ayrı bir metoda çıkarmayı engelliyorsa, **Replace Temp with Query**, **Introduce Parameter Object** veya **Preserve Whole Object** kullanın.

- Eğer önceki yöntemlerin hiçbiri işe yaramazsa, tüm metodu **Replace Method** with **Method Object** kullanarak ayrı bir nesneye taşımaya çalışın.

- Koşullu operatörler ve döngüler, kodun ayrı bir metoda taşınabileceğine dair iyi bir ipucudur. Koşullular için **Decompose Conditional** kullanın. Döngüler engelliyorsa, **Extract Method**'i deneyin.

**💰 Hesaplaşma**

- Tüm nesne yönelimli kod türleri arasında, kısa metotlara sahip sınıflar en uzun ömürlü olanlardır. Bir metot veya fonksiyon ne kadar uzunsa, onu anlamak ve sürdürmek o kadar zor olur.

- Ayrıca, uzun metotlar istenmeyen kopya yani duplicate kodların mükemmel saklanma yerini sunar.

**📈 Performans/Verim**

Birçok kişinin iddia ettiği gibi, metod sayısındaki artış performansa zarar verir mi? Hemen hemen tüm durumlarda etki o kadar önemsizdir ki endişelenmeye bile değmez.

Ayrıca, şimdi net ve anlaşılır bir kodunuz olduğundan, gerektiğinde gerçek performans kazanımları elde etmek için kodu yeniden yapılandırmanın gerçekten etkili yöntemlerini bulma olasılığınız daha yüksektir.


## Large Class (Büyük Sınıf)

**🤢 Belirti ve Semptomlar**

Bir sınıf birçok alan/yöntem/kod satırı içerir.

![](https://refactoring.guru/images/refactoring/content/smells/large-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Sınıflar genellikle küçük başlar. Ancak zamanla program büyüdükçe şişerler.

Uzun yöntemlerde de olduğu gibi, programcılar genellikle mevcut bir sınıfa yeni bir özellik yerleştirmeyi, o özellik için yeni bir sınıf oluşturmaktan daha az yorucu bulurlar.

![](![](https://refactoring.guru/images/refactoring/content/smells/large-class-02.png)

**💊 Tedavi**

Bir sınıf çok fazla fonksiyon içeriyorsa, onu bölmeyi düşünün:

- **Extract Class**, büyük sınıfın davranışının bir kısmının ayrı bir bileşene dönüştürülebilmesi durumunda yardımcı olur.
- **Extract Subclass**, büyük sınıfın davranışının bir kısmının farklı şekillerde uygulanabilmesine veya nadir durumlarda kullanılmasına yardımcı olur.
- **Extract Interface**, istemcinin kullanabileceği işlem ve davranışların bir listesinin gerekli olması durumunda yardımcı olur.
- Grafiksel arayüzden büyük bir sınıf sorumluysa, bazı verilerini ve davranışlarını ayrı bir etki alanı nesnesine taşımayı deneyebilirsiniz. Bunu yaparken bazı verilerin kopyalarını iki yerde saklamak ve verilerin tutarlı olmasını sağlamak gerekebilir. **Duplicate Observed Data**  bunu yapmanın bir yolunu sunar.

![](https://refactoring.guru/images/refactoring/content/smells/large-class-03-2x.png)

**💰 Hesaplaşma**

- Bu sınıfların yeniden düzenlenmesi, geliştiricilerin bir sınıf için çok sayıda özelliği hatırlama ihtiyacını ortadan kaldırır.

- Çoğu durumda, büyük sınıfları parçalara ayırmak kod ve işlevlerin tekrarlanmasını önler.

## Primitive Obsession (İlkel Takıntı)

**🤢 Belirti ve Semptomlar**

- Basit görevler için küçük nesneler yerine ilkel öğelerin (primitives) kullanılması (para birimi, aralıklar, telefon numaraları için özel dizeler vb.)
- Kodlama bilgileri için sabitlerin kullanılması (yönetici haklarına sahip kullanıcılara atıfta bulunmak için `USER_ADMIN_ROLE = 1` sabiti gibi.)
- Veri dizilerinde (data array) kullanılmak üzere alan adları olarak dize (string) sabitlerinin kullanılması.

![](https://refactoring.guru/images/refactoring/content/smells/primitive-obsession-01-2x.png)

**🤒 Sorunun Nedenleri**

Çoğu diğer "koku" gibi, ilkel takıntılar zayıf anlarda ortaya çıkar. "Sadece bazı verileri depolamak için bir alan!" diye düşünebilir bir programcı. Bir ilkel alan oluşturmak, tamamen yeni bir sınıf yapmaktan çok daha kolaydır, değil mi? Ve öyle de olur. Sonra başka bir alan gerekiyorsa ve aynı şekilde eklenmiş olur. İşte sınıf büyük ve kullanışsız hale gelmiş oldu.

İlkel veriler sıklıkla türleri (type) "taklit etmek" için kullanılır. Yani ayrı bir veri türü yerine, bir varlık için izin verilen değerler listesini oluşturan bir dizi sayı veya dizeye (string) sahip olursunuz. Bu belirli sayılar ve dizeler, sabitler aracılığıyla bu özel sayılara ve dizelere anlaşılır isimler verilerek geniş bir alana yayılır.

İlkel kullanımının kötü bir örneği, alan taklidi yapmaktır. Sınıf, çeşitli veri ve dize sabitlerini içeren büyük bir diziyi içerir ve bu verilere erişmek için dize sabitleri (sınıfta belirtilenler) dizi indisleri olarak kullanılır.

**💊 Tedavi**

- Çok çeşitli ilkel alanlarınız varsa, bunlardan bazılarını mantıksal olarak kendi sınıflarında gruplandırmak mümkün olabilir. Daha da iyisi, bu verilerle ilişkili davranışı da sınıfa taşıyın. Bu görev için **Replace Data Value with Object** deneyin.

![](https://refactoring.guru/images/refactoring/content/smells/primitive-obsession-02-2x.png)


- İlkel alanların değerleri metod parametrelerinde kullanılıyorsa, Introduce Parameter Object veya Preserve Whole Object kullanın.

- Karmaşık veriler değişkenlerde kodlandığında, **Replace Type Code with Class**, **Replace Type Code with Subclasses** veya **Replace Type Code with State/Strategy** kullanın.

- Değişkenler arasında diziler varsa, **Replace Array with Object** kullanın.

**💰 Hesaplaşma**

- Nesnelerin ilkel verilere tercih edilmesi sayesinde kod daha esnek hale gelir.
- Kodun daha iyi anlaşılabilir ve düzenli olması. Belirli veriler üzerindeki operasyonlar, dağınık olmaktansa aynı yerde bulunur. Bu tuhaf sabitlerin ve neden dizide olduklarının nedeni hakkında daha fazla tahmin yapmaya gerek yok.
- Kopya (duplicate) kodun daha kolay bulunması.

## # Long Parameter List (Uzun Parametre Listesi)

**🤢 Belirti ve Semptomlar**

Bir yöntem için üç veya dörtten fazla parametre bulunması.

![](https://refactoring.guru/images/refactoring/content/smells/long-parameter-list-01-2x.png)

**🤒 Sorunun Nedenleri**

Birkaç algoritma türü tek bir yöntemde birleştirildiğinde uzun bir parametre listesi ortaya çıkabilir. Hangi algoritmanın nasıl çalıştırılacağını kontrol etmek için uzun bir liste oluşturulmuş olabilir.

Uzun parametre listeleri aynı zamanda sınıfları birbirinden daha bağımsız hale getirme çabalarının bir yan ürünü de olabilir. Örneğin, bir yöntemde ihtiyaç duyulan belirli nesneleri oluşturmaya yönelik kod, yöntemden yöntemi çağırmaya yönelik koda taşındı, ancak oluşturulan nesneler yönteme parametre olarak aktarıldı. Böylece orijinal sınıf artık nesneler arasındaki ilişkileri bilmiyor ve bağımlılık azaldı. Ancak bu nesnelerden birkaçı oluşturulursa, her biri kendi parametresine ihtiyaç duyacaktır, bu da daha uzun bir parametre listesi anlamına gelir.

Uzadıkça çelişkili hale gelen ve kullanımı zorlaşan bu tür listeleri anlamak zordur. Uzun bir parametre listesi yerine, bir yöntem kendi nesnesinin verilerini kullanabilir. Geçerli nesne gerekli tüm verileri içermiyorsa, yöntem parametresi olarak (gerekli verileri alacak olan) başka bir nesne iletilebilir.

**💊 Tedavi**

- Parametrelere hangi değerlerin geçirildiğini kontrol edin. Eğer bazı argümanlar sadece başka bir nesnenin metod çağrılarının sonuçları ise, **Replace Parameter with Method Call** kullanın. Bu nesne, kendi sınıfının alanına yerleştirilebilir veya bir metod parametresi olarak geçirilebilir.

- Başka bir nesneden alınan bir veri grubunu parametre olarak geçirmek yerine, **Preserve Whole Object** kullanarak nesneyi kendisini metoda geçirin.

- Diğer yandan, bu parametreler farklı kaynaklardan geliyorsa, **Introduce Parameter Object** kullanarak bunları tek bir parametre nesnesi olarak geçirebilirsiniz.

![](https://refactoring.guru/images/refactoring/content/smells/long-parameter-list-02-2x.png)

**💰 Hesaplaşma**

- Daha okunabilir, daha kısa kod.
- Refactoring, daha önce fark edilmeyen yinelenen kodu ortaya çıkarabilir.

**🤫 Ne Zaman Yok Sayılmalı?**

Sınıflar arasında istenmeyen bağımlılığa neden olacaksa parametrelerden kurtulmayın.

## Data Clumps (Veri Kümeleri)

**🤢 Belirti ve Semptomlar**

Bazen kodun farklı bölümleri aynı değişken gruplarını (bir veritabanına bağlanma parametreleri gibi) içerir. Bu kümelerin kendi sınıflarına dönüştürülmesi gerekmektedir.

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-01-2x.png)

**🤒 Sorunun Nedenleri**

Genellikle bu veri grupları zayıf program yapısından veya "kopyala yapıştır programlamasından" kaynaklanmaktadır.

Bazı verilerin veri kümesi olup olmadığından emin olmak istiyorsanız veri değerlerinden birini silin ve diğer değerlerin hâlâ anlamlı olup olmadığına bakın. Durum böyle değilse, bu değişken grubunun bir nesnede birleştirilmesi gerektiğine dair iyi bir işarettir.

**💊 Tedavi**

- Tekrar eden veri, bir sınıfın alanlarını oluşturuyorsa, **Extract Class** kullanarak bu alanları kendi sınıflarına taşıyın.

- Aynı veri yığınları metod parametrelerinde geçiyorsa, **Introduce Parameter Object** kullanarak bunları bir sınıf olarak ayırın.

- Verilerin bazıları diğer metodlara geçiriliyorsa, yalnızca bireysel alanlar değil, tüm veri nesnesini metoda geçirmeyi düşünün. **Preserve Whole Object** bunun için yardımcı olacaktır.

- Bu alanların kullandığı kodlara bakın. Bu kodu bir veri sınıfına taşımak iyi bir fikir olabilir.

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-02-2x.png)

**💰 Hesaplaşma**

- Kodun anlaşılmasını ve organizasyonunu geliştirir. Belirli veriler üzerindeki işlemler artık kod boyunca gelişigüzel bir şekilde yerine tek bir yerde toplanır.

- Kod boyutunu azaltır.

![](https://refactoring.guru/images/refactoring/content/smells/data-clumps-03-2x.png)


**🤫 Ne Zaman Yok Sayılmalı?**

Bir nesnenin yalnızca değerlerini (ilkel türler) iletmek yerine, bir yöntemin parametrelerinde tamamını geçirmek, iki sınıf arasında istenmeyen bir bağımlılık yaratabilir.





