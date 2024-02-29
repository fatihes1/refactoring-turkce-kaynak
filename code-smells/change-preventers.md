# Change Preventers (Önleyicileri Değiştirme)

Bu başlık altındaki kokular, kodunuzun bir yerinde bir şeyi değiştirmeniz gerekiyorsa, diğer yerlerde de birçok değişiklik yapmanız gerektiği anlamına gelir. Sonuç olarak program geliştirme çok daha karmaşık ve maliyetli hale gelir.

## 1️⃣ Iraksak Değişim (Divergent Change)

Divergent Change, Shotgun Surgery'e benzese de aslında tam tersi bir kokudur. Iraksak Değişim (Divergent Change), tek bir sınıfta birçok değişikliğin yapılmasıdır. Shotgun Surgery  ise, birden fazla sınıfta aynı anda tek bir değişiklik yapılması anlamına gelir.

**🤢 Belirti ve Semptomlar**

Bir sınıfta değişiklik yaptığınızda, ilgisiz birçok yöntemi değiştirmek zorunda kalabilirsiniz. Örneğin, yeni bir ürün türü eklerken ürünleri bulma, sergileme ve sipariş etme yöntemlerini değiştirmeniz gerekir. Böyle bir durumla karşı karşıya kaldığınızda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/divergent-change-01-2x.png)


**🤒 Sorunun Nedenleri**

Çoğu zaman bu farklı değişiklikler zayıf program yapısından veya kopya yapıştır geliştirmelerden kaynaklanmaktadır.

**💊 Tedavi**

- Sınıfın davranışını **Extract Class** tekniğini kullanarak parçalara bölebilirsiniz.

- Farklı sınıfların aynı davranışa sahip olması durumunda, sınıfları miras yoluyla birleştirmek isteyebilirsiniz (**Extract Superclass** ve **Extract Subclass**).


**💰 Hesaplaşma**

- Kod organizasyonunu geliştirir.
- Kod tekrarını azaltır.
- Desteği basitleştirir.


## 2️⃣ Shotgun Surgery

**🤢 Belirti ve Semptomlar**

Herhangi bir değişiklik yaptığınızda, birçok farklı sınıfta birçok küçük değişiklik yapmanızı gerekebilir. Bu tarz durumlarda soru işaretleriniz oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-01-2x.png)

**🤒 Sorunun Nedenleri**

Tek bir sorumluluk çok sayıda sınıfa dağıtıldığı durumlarda ortaya çıkar. Bu, Iraksak Değişim (Divergent Change.) tekniğini aşırı agrasif bir şekilde kullanılmasından sonra gerçekleşebilir.

![](![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-02-2x.png)

**💊 Tedavi**

- Sınıfın davranışını **Extract Class** tekniğini kullanarak parçalara ayırabilirsiniz.

- Farklı sınıfların aynı davranışa sahip olması durumunda, sınıfları miras yoluyla birleştirmek isteyebilirsiniz (**Extract Superclass** ve **Extract Subclass**).



![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-03-2x.png)

**💰 Hesaplaşma**

- Daha iyi organizasyon.
- Daha az kod kopyası.
- Daha kolay bakım.

## 3️⃣ Parallel Inheritance Hierarchies

**🤢 Belirti ve Semptomlar**

Bir sınıf için bir alt sınıf oluşturduğunuzda, kendinizi başka bir sınıf için bir alt sınıf yaratmaya ihtiyaç duyarken bulabilirsiniz. Böyle bir durumla karşılaştığınızda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/parallel-inheritance-hierarchies-01-2x.png)

**🤒 Sorunun Nedenleri**

Hiyerarşi küçük kaldığı sürece her şey yolundadır. Ancak proje/program geliştiçe bu hiyeraşi büyüyecektir. Yeni sınıfların eklenmesiyle değişiklik yapmak giderek zorlaşır.

**💊 Tedavi**

- İki adımda paralel sınıf hiyerarşilerini tekrarsız hale getirebilirsiniz. Bu adımlar şu şekildedir:
  - İlk olarak, bir hiyerarşinin örneklerinin başka bir hiyerarşinin örneklerine (instance) gönderme (referans) yapmasını sağlayın.
  - Sonra, **Move Method** ve **Move Field** tekniklerini kullanarak atıfta bulunan sınıftaki hiyerarşiyi kaldırın.


**💰 Hesaplaşma**

- Kod tekrarını azaltır.
- Kodun organizasyonunu geliştirebilir.

![](https://refactoring.guru/images/refactoring/content/smells/parallel-inheritance-hierarchies-02-2x.png)


**🤫 Ne Zaman Yok Sayılmalı?**

Bazen paralel sınıf hiyerarşilerine sahip olmak, program mimarisinde daha da büyük karışıklıkları önlemenin bir yoludur. Hiyerarşileri tekilleştirme girişimlerinizin daha da kirli kod ürettiğini fark edebilirsiniz. Böyle bir durumda dışarı çıkın, temiz hava alın ve geri döndüğünüzde tüm değişikliklerinizi geri alın ve bu koda alışın.


