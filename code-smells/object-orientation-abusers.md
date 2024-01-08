# Object-Orientation Abusers (Nesne Yönelimini Kötüye Kullanma)

Aşağıdaki tüm kod kokular nesne yönelimli programlama ilkelerinin eksik veya yanlış uygulanmasından kaynaklanmaktadır.

## 1️⃣ Switch Statements

**🤢 Belirti ve Semptomlar**

Karmaşık bir `switch` operatörünüz veya `if` ifadeleri diziniz var.

![](https://refactoring.guru/images/refactoring/content/smells/switch-statements-01-2x.png)


**🤒 Sorunun Nedenleri**

`switch` ve `case` operatörlerinin nispeten nadir kullanımı, nesne yönelimli kodun belirgin özelliklerinden biridir. Genellikle tek bir `switch` için kod, programın farklı yerlerine dağılmış olabilir. Yeni bir koşul eklediğinizde, tüm `switch` kodlarını bulup değiştirmeniz gerekebilir.

Genel bir kural olarak, `switch` gördüğünüzde polimorfizmi düşünmelisiniz.

**💊 Tedavi**

- `switch`'i izole etmek ve doğru sınıfa koymak için **Extract Method** ve ardından **Move Method** kullanmanız gerekebilir.

- Eğer bir `switch`, tip koduna (type code) dayanıyorsa, örneğin programın çalışma modu değiştirildiğinde, **Replace Type Code with Subclasses** veya **Replace Type Code with State/Strategy** kullanın.

- Miras yapısını belirledikten sonra **Replace Conditional with Polymorphism** kullanın.

- Eğer operatörde çok fazla koşul yoksa ve hepsi farklı parametrelerle aynı metodunu çağırıyorsa, polimorfizm gereksiz olacaktır. Bu durumda, o metodunu **Replace Parameter with Explicit Methods** kullanarak birden çok küçük metoda bölebilir ve switch'i buna göre değiştirebilirsiniz.

- Koşullardan biri `null` ise, **Introduce Null Object** kullanın.



**💰 Hesaplaşma**

Geliştirilmiş kod organizasyonu sunar.

![](https://refactoring.guru/images/refactoring/content/smells/switch-statements-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

- Bir switch operatörü basit işlemleri gerçekleştiriyorsa, kod değişikliği yapmaya gerek yoktur.

- Sıkça switch operatörleri, fabrika tasarım desenleri (**Factory Method** veya **Abstract Factory**) tarafından oluşturulan bir sınıfı seçmek için kullanılır.


## 2️⃣ Temporary Field (Geçici Alan)

**🤢 Belirti ve Semptomlar**

Geçici alanlar değerlerini (ve dolayısıyla nesnelerin ihtiyaç duyduğu) yalnızca belirli koşullar altında alır. Bu koşullar dışında boşturlar.

![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-01-2x.png)

**🤒 Sorunun Nedenleri**

Çoğu zaman, büyük miktarda girdi gerektiren bir algoritmada kullanılmak üzere geçici alanlar oluşturulur. Dolayısıyla programcı, yöntemde çok sayıda parametre oluşturmak yerine sınıfta bu veriler için alanlar oluşturmaya karar verir. Bu alanlar yalnızca algoritmada kullanılır ve geri kalan zamanda kullanılmaz.

Bu tür bir kodu anlamak zordur. Nesne alanlarında veri görmeyi beklersiniz ancak bir nedenden dolayı bunlar neredeyse çoğu zaman boştur.

![](![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-02-2x.png)

**💊 Tedavi**

- Geçici alanlar ve onlar üzerinde çalışan tüm kodlar **Extract Class** kullanılarak ayrı bir sınıfa konabilir. Başka bir deyişle, bir metod nesnesi oluşturmuş olursunuz ve **Replace Method with Method Object** yöntemini kullanmak ile aynı sonuca ulaşıyorsunuz.

- **Introduce Null Object** kullanın ve geçici alan değerlerini kontrol etmek için kullanılan koşullu kodun yerine entegre edin.


![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-03-2x.png)

**💰 Hesaplaşma**

Daha iyi kod okunurluğu ve organizasyonu sağlar.

## 3️⃣ Refused Bequest (Reddedilen Miras)

**🤢 Belirti ve Semptomlar**

Bir alt sınıf, ebeveynlerinden miras alınan yöntem ve özelliklerin yalnızca bazılarını kullanıyorsa hiyerarşi bozulur. Gereksiz yöntemler kullanılmadan kalabilir veya yeniden tanımlanabilir, ayrıca istisnalar ortaya çıkabilir.

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-01-2x.png)

**🤒 Sorunun Nedenleri**

Birisi, yalnızca kodu bir üst sınıfta yeniden kullanma arzusuyla sınıflar arasında miras yaratmaya motive oldu. Ancak üst sınıf ve alt sınıf tamamen farklı olabilir.

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-02-2x.png)

**💊 Tedavi**

- Eğer mirasın anlam taşımadığı ve alt sınıfın gerçekten üst sınıf ile hiçbir ortak noktasının olmadığı durumdaysa, **Replace Inheritance with Delegation** lehine mirası ortadan kaldırın.

- Eğer miras uygunsa, alt sınıfta gereksiz alanları ve metotları ortadan kaldırın. Alt sınıf tarafından ihtiyaç duyulan tüm alanları ve metotları üst sınıftan çıkarın, bunları yeni bir üst sınıfa koyun ve her iki sınıfı da ondan miras alacak şekilde ayarlayın (**Extract Superclass**).

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-03-2x.png)

**💰 Hesaplaşma**

Kodun netliğini ve organizasyonunu geliştirir. Artık Köpek (`Dog`) sınıfının neden Sandalye (`Chair`) sınıfından miras alındığını merak etmenize gerek kalmayacak (her ikisinin de 4 bacağı olmasına rağmen).

## 4️⃣ Alternative Classes with Different Interfaces (Farklı Arayüzlerle Alternatif Sınıflar)

**🤢 Belirti ve Semptomlar**

İki sınıf aynı işlevleri yerine getirir ancak farklı yöntem adlarına sahiptir.

![](https://refactoring.guru/images/refactoring/content/smells/alternative-classes-with-different-interfaces-01-2x.png)

**🤒 Sorunun Nedenleri**

Sınıflardan birini oluşturan programcı muhtemelen işlevsel olarak aynı işlemi yapan eşdeğer bir sınıfın hali hazırda var olduğunu bilmiyordu.

**💊 Tedavi**

Sınıfların arayüzünü ortak bir paydada ifade etmeye çalışın:

- Metodları aynı hale getirmek için **Rename Methods** kullanın.

- Metodların imzasını ve uygulamasını aynı yapmak için **Move Method**, **Add Parameter** ve **Parameterize Method** kullanın.

- Sınıfların sadece bir kısmı tarafından kullanılan işlevsellik varsa, **Extract Superclass** kullanmayı deneyin. Bu durumda, mevcut sınıflar, alt sınıflar haline gelecektir.

- Hangi tedavi yöntemini kullanacağınızı belirledikten ve uyguladıktan sonra, sınıflardan birini silebilirsiniz.

**💰 Hesaplaşma**

- Gereksiz yinelenen kodlardan kurtulursunuz, böylece ortaya çıkan kod az hacimli olur.

- Kod daha okunabilir ve anlaşılır hale gelir (artık birinciyle aynı işlevleri yerine getiren ikinci bir sınıfın yaratılmasının nedenini tahmin etmenize gerek yoktur).

![](https://refactoring.guru/images/refactoring/content/smells/alternative-classes-with-different-interfaces-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen sınıfları birleştirmek imkansızdır veya anlamsız olacak kadar zordur. Bunun bir örneği, alternatif sınıfların her birinin kendi sınıf sürümüne sahip farklı kitaplıklarda olmasıdır.


