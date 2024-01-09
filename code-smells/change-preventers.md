# Change Preventers (Önleyicileri Değiştirme)

Bu kokular, kodunuzun bir yerinde bir şeyi değiştirmeniz gerekiyorsa, diğer yerlerde de birçok değişiklik yapmanız gerektiği anlamına gelir. Sonuç olarak program geliştirme çok daha karmaşık ve maliyetli hale gelir.

## 1️⃣ Iraksak Değişim (Divergent Change)

Divergent Change, Shotgun Surgery'e benzese de aslında tam tersi bir kokudur. Iraksak Değişim (Divergent Change), tek bir sınıfta birçok değişikliğin yapılmasıdır. Shotgun Surgery  ise, birden fazla sınıfta aynı anda tek bir değişiklik yapılması anlamına gelir.

**🤢 Belirti ve Semptomlar**

Bir sınıfta değişiklik yaptığınızda, ilgisiz birçok yöntemi değiştirmek zorunda kalırsınız. Örneğin, yeni bir ürün türü eklerken ürünleri bulma, sergileme ve sipariş etme yöntemlerini değiştirmeniz gerekir.

![](https://refactoring.guru/images/refactoring/content/smells/divergent-change-01-2x.png)


**🤒 Sorunun Nedenleri**

Çoğu zaman bu farklı değişiklikler zayıf program yapısından veya "kopya yapıştır programlamasından" kaynaklanmaktadır.

**💊 Tedavi**

- Sınıfın davranışını **Extract Class** kullanarak bölebilirsiniz.

- Farklı sınıfların aynı davranışa sahip olması durumunda, sınıfları miras yoluyla birleştirmek isteyebilirsiniz (**Extract Superclass** ve **Extract Subclass**).


**💰 Hesaplaşma**

- Kod organizasyonunu geliştirir.
- Kod tekrarını azaltır.
- Desteği basitleştirir.


## 2️⃣ Shotgun Surgery

**🤢 Belirti ve Semptomlar**

Herhangi bir değişiklik yapmak, birçok farklı sınıfta birçok küçük değişiklik yapmanızı gerektirir.

![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-01-2x.png)

**🤒 Sorunun Nedenleri**

Tek bir sorumluluk çok sayıda sınıfa dağıtılmıştır. Bu, Iraksak Değişim (Divergent Change.) tekniğini aşırı hevesli bir şekilde kullanılmasından sonra gerçekleşebilir.

![](![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-02-2x.png)

**💊 Tedavi**

- Sınıfın davranışını **Extract Class** kullanarak bölebilirsiniz.

- Farklı sınıfların aynı davranışa sahip olması durumunda, sınıfları miras yoluyla birleştirmek isteyebilirsiniz (**Extract Superclass** ve **Extract Subclass**).



![](https://refactoring.guru/images/refactoring/content/smells/shotgun-surgery-03-2x.png)

**💰 Hesaplaşma**

- Daha iyi organizasyon.
- Daha az kod kopyası.
- Daha kolay bakım.

## 3️⃣ Parallel Inheritance Hierarchies

**🤢 Belirti ve Semptomlar**

Bir sınıf için bir alt sınıf oluşturduğunuzda, kendinizi başka bir sınıf için bir alt sınıf yaratmaya ihtiyaç duyarken bulabilirsiniz.

![](https://refactoring.guru/images/refactoring/content/smells/parallel-inheritance-hierarchies-01-2x.png)

**🤒 Sorunun Nedenleri**

Hiyerarşi küçük kaldığı sürece her şey yolundadır. Ancak yeni sınıfların eklenmesiyle değişiklik yapmak giderek zorlaşır.

**💊 Tedavi**

- İki adımda paralel sınıf hiyerarşilerini tekrarsız hale getirebilirsiniz. İlk olarak, bir hiyerarşinin örneklerinin başka bir hiyerarşinin örneklerine (instance) gönderme (referans) yapmasını sağlayın. Sonra, **Move Method** ve **Move Field** kullanarak atıfta bulunan sınıftaki hiyerarşiyi kaldırın.


**💰 Hesaplaşma**

- Kod tekrarını azaltır.
- Kodun organizasyonunu geliştirebilir.

![](https://refactoring.guru/images/refactoring/content/smells/parallel-inheritance-hierarchies-02-2x.png)


**🤫 Ne Zaman Yok Sayılmalı?**

Bazen paralel sınıf hiyerarşilerine sahip olmak, program mimarisinde daha da büyük karışıklıkları önlemenin bir yoludur. Hiyerarşileri tekilleştirme girişimlerinizin daha da çirkin kod ürettiğini fark ederseniz, dışarı çıkın, tüm değişikliklerinizi geri alın ve bu koda alışın.


