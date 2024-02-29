# Bağlayıcılar (Couplers)

Bu tür kokular sınıflar arasında fazla bağlantıya neden olabilir veya bağlantının aşırı derecede delege etmeye çalıştığında ortaya çıkar. Yani bu durum şu iki şekilde orytaya çıkabilir: 
- Bir sistemdeki farklı parçalar arasındaki ilişkilerin ya çok sıkı olduğu ve bu durumun da kodun bakımını ve değişiklik yapmayı zorlaştırdığı, - İlişkilerin çok zayıf olduğu ve bu durumun da kodun karmaşık hale gelmesine neden olur.

## 1️⃣ Özellik Kıskançlığı (Feature Envy)

**🤢 Belirti ve Semptomlar**

Bir yöntem kendi verisinden çok başka bir nesnenin verisine erişiyorsa soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-01-2x.png)

**🤒 Sorunun Nedenleri**

Bu koku, alanlar bir veri sınıfına (data class) taşındıktan sonra ortaya çıkabilir. Eğer durum buysa veriler üzerindeki işlemleri de bu sınıfa taşımak isteyebilirsiniz.

**💊 Tedavi**

Temel bir kural olarak, eğer bir şeyler aynı anda değişiyorsa, onları aynı yerde tutmalısınız. İstisnaları olsa da genellikle bu veriyi kullanan fonksiyonlar birlikte değiştirilir.

- Bir metodun açıkça başka bir yere taşınması gerekiyorsa **Move Method** tekniğini kullanın.

- Eğer bir metodun sadece bir kısmı başka bir nesnenin verisine erişiyorsa, bu kısmı taşımak için **Extract Method** tekniğini kullanın.

- Bir metodun birkaç başka sınıftan fonksiyonları kullandığı durumlarda, önce kullanılan verinin çoğunu içeren sınıfı belirleyin. Sonra metodunuzu bu sınıfa diğer verilerle birlikte yerleştirin. Alternatif olarak, metodu farklı sınıflardaki farklı yerlere yerleştirilebilecek birkaç parçaya ayırmak için **Extract Method** tekniğini de kullanabilirsiniz.

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-02-2x.png)


**💰 Hesaplaşma**

- Daha az kod kopyası (duplication).

- Daha iyi kod organizasyonu.

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen davranışlar, verileri tutan sınıftan kasıtlı olarak ayrı tutulur. Bunun genel avantajı davranışı dinamik olarak değiştirebilme yeteneğidir (bkz. Strateji (Strategy), Ziyaretçi (Visitor) ve diğer tasarım desenleri).

## 2️⃣ Uygunsuz Yakınlık (Inappropriate Intimacy)

**🤢 Belirti ve Semptomlar**

Bir sınıf başka bir sınıfın dahili (internal) alanlarını ve yöntemlerini kullandığı durumlarda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/inappropriate-intimacy-01-2x.png)

**🤒 Sorunun Nedenleri**

Birlikte çok fazla zaman geçiren sınıfları yakından inceleyin. İyi bir sınıf yapısında, sınıflar birbirleri hakkında mümkün olduğunca az bilgiye sahibi olmalıdır. Bu tür sınıfların bakımı ve yeniden kullanımı daha kolaydır.

**💊 Tedavi**

- En basit çözüm, bir sınıfın bir kısmını gerçekten ihtiyaç duymadığı bir sınıfa taşımak için **Move Method** ve **Move Field** tekniklerini kullanmaktır. Ancak bu, ilk sınıfın bu parçalara gerçekten ihtiyaç duymadığı durumdlarda işe yaracaktır.

- Başka bir çözüm, kod ilişkilerini resmi (official) hale getirmek için **Extract Class** ve **Hide Delegate** teknikleri kullanmaktır.

- Eğer sınıflar karşılıklı bağımlıysa, **Change Bidirectional Association to Unidirectional** tekniği kullanmalısınız.

- Bu sıkı bağ bir alt sınıf ile üst sınıf arasındaysa, **Replace Delegation with Inheritance** yöntemi düşünülebilir.


![](https://refactoring.guru/images/refactoring/content/smells/inappropriate-intimacy-03-2x.png)


**💰 Hesaplaşma**

- Kod organizasyonunu geliştirir.
- Desteği ve kodun yeniden kullanımını basitleştirir.

## 3️⃣ Mesaj Zincirleri (Message Chains)

**🤢 Belirti ve Semptomlar**

Kodda şuna benzeyen bir dizi çağrı görüyorsunuz:

```
$a->b()->c()->d()
```

soru işaretleri oluşmaya başlamalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/message-chains-01-2x.png)

**🤒 Sorunun Nedenleri**

Bir istemci başka bir nesne istediğinde, o nesne başka bir nesne istediğinde vb. bir mesaj zinciri oluşur. Bu zincirler, istemcinin sınıf yapısı boyunca gezinmeye bağımlı olduğu anlamına gelir. Bu ilişkilerdeki herhangi bir değişiklik istemcinin değiştirilmesini gerektirir.

**💊 Tedavi**

- Bir mesaj zincirini silmek için **Hide Delegate** yöntemi kullanın.

- Bazı durumlarda, neden son nesnenin kullanıldığını düşünmek daha iyidir. Belki de bu işlevselliği **Extract Method** yöntemi kullanarak çıkarmak ve **Move Method** yöntemi kullanarak zincirin başına taşımak mantıklı olabilir.


![](https://refactoring.guru/images/refactoring/content/smells/message-chains-02-2x.png)

**💰 Hesaplaşma**

- Bir zincirin sınıfları arasındaki bağımlılıkları azaltır.
- Şişirilmiş kod miktarını azaltır.

![](https://refactoring.guru/images/refactoring/content/smells/message-chains-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Aşırı agresif temsilci (delegate) gizleme, işlevselliğin gerçekte nerede gerçekleştiğini görmenin zor olduğu kodlara neden olabilir. Temelde bu durum, Orta Adam (Middle Man) kokusundan da kaçının demenin başka bir yoludur.


## 4️⃣ Orta Adan (Middle Man)

**🤢 Belirti ve Semptomlar**

Bir sınıf, işi başka bir sınıfa devrederek yalnızca tek bir eylem gerçekleştiriyorsa, o sınıf neden var? Bu tarz bir soru aklınıza geldiyse, varsa diğer gelecek soru işaretlerine hazır olun.

![](https://refactoring.guru/images/refactoring/content/smells/middle-man-01-2x.png)

**🤒 Sorunun Nedenleri**

Bu koku, Mesaj Zincirleri yönteminin aşırı agrasif bir şekilde ortadan kaldırılmasının bir sonucu olabilir.

Diğer durumlarda, bir sınıfın faydalı çalışmasının kademeli olarak diğer sınıflara taşınmasının sonucu olabilir. Sınıf, yetki vermekten başka hiçbir şey yapmayan boş bir kabuk olarak kalır.

**💊 Tedavi**

Bir yöntemin sınıflarının çoğu başka bir sınıfa devrediliyorsa **Remove Middle Man** işlemi uygundur.


**💰 Hesaplaşma**

- Daha az hacimli kod sağlar.

![](https://refactoring.guru/images/refactoring/content/smells/middle-man-02.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Aşağıdaki nedenlerden dolayı oluşturulan orta adamı silmenizi tavsiye etmem:

- Sınıflar arası bağımlılıkları önlemek için bir orta adam eklenmiş olabilir.

- Bazı tasarım desenleri kasıtlı olarak orta adam yaratır (Proxy veya Dekoratör gibi).

