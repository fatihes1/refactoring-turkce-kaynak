# Bağlayıcılar (Couplers)

Bu gruptaki tüm kokular, sınıflar arasında aşırı eşleşmeye katkıda bulunur veya eşleşmenin yerini aşırı delegasyon alırsa ne olacağını gösterir.

## 1️⃣ Özellik Kıskançlığı (Feature Envy)

**🤢 Belirti ve Semptomlar**

Bir yöntem kendi verisinden çok başka bir nesnenin verisine erişir.

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-01-2x.png)

**🤒 Sorunun Nedenleri**

Bu koku, alanlar bir veri sınıfına taşındıktan sonra ortaya çıkabilir. Eğer durum buysa veriler üzerindeki işlemleri de bu sınıfa taşımak isteyebilirsiniz.

**💊 Tedavi**

Temel bir kural olarak, eğer bir şeyler aynı anda değişiyorsa, onları aynı yerde tutmalısınız. Genellikle bu veriyi kullanan fonksiyonlar birlikte değiştirilir (ancak istisnalar mümkündür).

- Bir metodun açıkça başka bir yere taşınması gerekiyorsa **Move Method** kullanın.

- Eğer bir metodun sadece bir kısmı başka bir nesnenin verisine erişiyorsa, bu kısmı taşımak için **Extract Method** kullanın.

- Bir metodun birkaç başka sınıftan fonksiyonları kullandığı durumlarda, önce kullanılan verinin çoğunu içeren sınıfı belirleyin. Sonra metodunuzu bu sınıfa diğer verilerle birlikte yerleştirin. Alternatif olarak, metodu farklı sınıflardaki farklı yerlere yerleştirilebilecek birkaç parçaya ayırmak için **Extract Method** kullanın.

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-02-2x.png)


**💰 Hesaplaşma**

- Daha az kod kopyası- duplication (veri işleme kodu merkezi bir yere konulursa).

- Daha iyi kod organizasyonu (verileri işleme yöntemleri gerçek verilerin yanındadır).

![](https://refactoring.guru/images/refactoring/content/smells/feature-envy-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen davranışlar, verileri tutan sınıftan kasıtlı olarak ayrı tutulur. Bunun genel avantajı davranışı dinamik olarak değiştirebilme yeteneğidir (bkz. Strateji (Strategy), Ziyaretçi (Visitor) ve diğer tasarım desenleri).

## 2️⃣ Uygunsuz Yakınlık (Inappropriate Intimacy)

**🤢 Belirti ve Semptomlar**

Bir sınıf başka bir sınıfın dahili alanlarını ve yöntemlerini kullanır.

![](https://refactoring.guru/images/refactoring/content/smells/inappropriate-intimacy-01-2x.png)

**🤒 Sorunun Nedenleri**

Birlikte çok fazla zaman geçiren sınıfları yakından takip edin. İyi bir sınıf yapısında, sınıflar birbirleri hakkında mümkün olduğunca az bilgi sahibi olmalıdır. Bu tür sınıfların bakımı ve yeniden kullanımı daha kolaydır.

**💊 Tedavi**

- En basit çözüm, bir sınıfın bir kısmını gerçekten ihtiyaç duymadığı bir sınıfa taşımak için **Move Method** ve **Move Field** kullanmaktır. Ancak bu, ilk sınıfın bu parçalara gerçekten ihtiyaç duymadığı durumda çalışır.

- Başka bir çözüm, kod ilişkilerini "resmi" hale getirmek için **Extract Class** ve **Hide Delegate** kullanmaktır.

- Eğer sınıflar karşılıklı bağımlıysa, **Change Bidirectional Association to Unidirectional** tekniği kullanmalısınız.

- Bu "sıkı bağ" bir alt sınıf ile üst sınıf arasındaysa, **Replace Delegation with Inheritance** yöntemi düşünülebilir.


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

![](https://refactoring.guru/images/refactoring/content/smells/message-chains-01-2x.png)

**🤒 Sorunun Nedenleri**

Bir istemci başka bir nesne istediğinde, o nesne başka bir nesne istediğinde vb. bir mesaj zinciri oluşur. Bu zincirler, istemcinin sınıf yapısı boyunca gezinmeye bağımlı olduğu anlamına gelir. Bu ilişkilerdeki herhangi bir değişiklik istemcinin değiştirilmesini gerektirir.

**💊 Tedavi**

- Bir mesaj zincirini silmek için **Hide Delegate** yöntemi kullanın.

- Bazı durumlarda, neden son nesnenin kullanıldığını düşünmek daha iyidir. Belki de bu işlevselliği **Extract Method** kullanarak çıkarmak ve **Move Method** kullanarak zincirin başına taşımak mantıklı olabilir.


![](https://refactoring.guru/images/refactoring/content/smells/message-chains-02-2x.png)

**💰 Hesaplaşma**

- Bir zincirin sınıfları arasındaki bağımlılıkları azaltır.
- Şişirilmiş kod miktarını azaltır.

![](https://refactoring.guru/images/refactoring/content/smells/message-chains-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Aşırı agresif temsilci gizleme, işlevselliğin gerçekte nerede gerçekleştiğini görmenin zor olduğu kodlara neden olabilir. Bu, Orta Adam (Middle Man) kokusundan da kaçının demenin başka bir yolu.


## 4️⃣ Orta Adan (Middle Man)

**🤢 Belirti ve Semptomlar**

Bir sınıf, işi başka bir sınıfa devrederek yalnızca tek bir eylem gerçekleştiriyorsa, o sınıf neden var?

![](https://refactoring.guru/images/refactoring/content/smells/middle-man-01-2x.png)

**🤒 Sorunun Nedenleri**

Bu koku, Mesaj Zincirleri yönteminin aşırı hevesli bir şekilde ortadan kaldırılmasının bir sonucu olabilir.

Diğer durumlarda, bir sınıfın faydalı çalışmasının kademeli olarak diğer sınıflara taşınmasının sonucu olabilir. Sınıf, yetki vermekten başka hiçbir şey yapmayan boş bir kabuk olarak kalır.

**💊 Tedavi**

Bir yöntemin sınıflarının çoğu başka bir sınıfa devrediliyorsa **Remove Middle Man** işlemi uygundur.


**💰 Hesaplaşma**

- Daha az hacimli kod.

![](https://refactoring.guru/images/refactoring/content/smells/middle-man-02.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Aşağıdaki nedenlerden dolayı oluşturulan orta adamı silmeyin:

- Sınıflar arası bağımlılıkları önlemek için bir orta adam eklenmiş olabilir.

- Bazı tasarım desenleri kasıtlı olarak orta adam yaratır (Proxy veya Dekoratör gibi)

