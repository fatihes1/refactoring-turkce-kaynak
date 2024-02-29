# Object-Orientation Abusers (Nesne Yönelimini Kötüye Kullanma)

Aşağıdaki tüm kod kokuları nesne yönelimli programlama (OOP) ilkelerinin eksik veya yanlış uygulanmasından kaynaklanmaktadır.

## 1️⃣ Switch Statements

**🤢 Belirti ve Semptomlar**

Karmaşık bir `switch` operatörünüz veya `if` ifadeleri diziniz var ise soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/switch-statements-01-2x.png)


**🤒 Sorunun Nedenleri**

`switch` ve `case` operatörlerinin nispeten nadir kullanımı, nesne yönelimli kodun belirgin özelliklerinden biridir. Genellikle tek bir `switch` için kod, programın farklı yerlerine dağılmış olabilir. Yeni bir koşul eklediğinizde, tüm `switch` kodlarını bulup değiştirmeniz gerekebilir.

Genel bir düşünce şu kanıdadır; `switch` gördüğünüzde polimorfizmi düşünmelisiniz.

**💊 Tedavi**

- `switch`'i izole etmek ve doğru sınıfa koymak için **Extract Method** tekniğini ve ardından **Move Method** tekniği kullanmanız gerekebilir.

- Eğer bir `switch`, tip koduna (type code) dayanıyorsa, **Replace Type Code with Subclasses** veya **Replace Type Code with State/Strategy** tekniğini kullanın.

- Miras yapısını belirledikten sonra **Replace Conditional with Polymorphism** tekniğini kullanın.

- Eğer operatörde çok fazla koşul yoksa ve hepsi farklı parametrelerle aynı metodunu çağırıyorsa, polimorfizm gereksiz olacaktır. Bu durumda, o metodunu **Replace Parameter with Explicit Methods** tekniğini kullanarak birden çok küçük metoda bölebilir ve switch'i buna göre değiştirebilirsiniz.

- Koşullardan biri `null` ise, **Introduce Null Object** tekniğini kullanın.



**💰 Hesaplaşma**

Geliştirilmiş kod organizasyonu sunar.

![](https://refactoring.guru/images/refactoring/content/smells/switch-statements-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

- Bir switch operatörü basit işlemleri gerçekleştiriyorsa, kod değişikliği yapmaya gerek yoktur.

- Sıkça switch operatörleri, fabrika tasarım desenleri (**Factory Method** veya **Abstract Factory**) tarafından oluşturulan bir sınıfı seçmek için kullanılır. Bu durumda yok sayılabilir.


## 2️⃣ Temporary Field (Geçici Alan)

**🤢 Belirti ve Semptomlar**

Geçici alanlar yalnızca belirli koşullar altında değer alır. Bu koşullar dışında boş kalırlar.

![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-01-2x.png)

**🤒 Sorunun Nedenleri**

Çoğu zaman, büyük miktarda girdi gerektiren bir algoritmada kullanılmak üzere geçici alanlar oluşturulur. Dolayısıyla programcı, yöntemde çok sayıda parametre oluşturmak yerine sınıfta bu veriler için alanlar oluşturmaya karar verir. Bu alanlar yalnızca algoritmada kullanılır ve geri kalan zamanda kullanılmaz.

Bu tür bir kodu anlamak zordur. Nesne alanlarında veri görmeyi beklersiniz ancak çeşitli nedenden dolayı bu nesneler neredeyse çoğu zaman boştur.

![](![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-02-2x.png)

**💊 Tedavi**

- Geçici alanlar ve onlar üzerinde çalışan tüm kodlar **Extract Class** tekniği kullanılarak ayrı bir sınıfa taşınabilir. Başka bir deyişle, bir metod nesnesi oluşturmuş olursunuz ve **Replace Method with Method Object** yöntemini kullanmak ile aynı sonuca ulaşmış olursunuz.

- **Introduce Null Object** tekniğini kullanın ve geçici alan değerlerini kontrol etmek için kullanılan koşullu kodun yerine entegre edin.


![](https://refactoring.guru/images/refactoring/content/smells/temporary-field-03-2x.png)

**💰 Hesaplaşma**

Daha iyi kod okunurluğu ve organizasyonu sağlar.

## 3️⃣ Refused Bequest (Reddedilen Miras)

**🤢 Belirti ve Semptomlar**

Bir alt sınıf, ebeveynlerinden miras alınan yöntem ve özelliklerin yalnızca bazılarını kullanıyorsa hiyerarşi bozulur. OOP gereği alt sınıf, üst sınıftan aldığı neredeyse tüm işlevleri kullanmalıdır. Gereksiz yöntemler kullanılmadan kalabilir veya yeniden tanımlanabilir, ayrıca hatalar (exceptions) ortaya çıkabilir. Böyle durumlarda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-01-2x.png)

**🤒 Sorunun Nedenleri**

Birisi, yalnızca kodu bir üst sınıfta yeniden kullanma arzusuyla sınıflar arasında miras yaratmayı mantıklı bulduğunu düşünün. Ancak üst sınıf ve alt sınıf tamamen farklı bir hale evrildi. Alt sınıf, üst sınıftan miras aldığı birkaç yöntem hariç hiçbir değeri kullanmıyor.

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-02-2x.png)

**💊 Tedavi**

- Eğer mirasın anlam taşımadığı ve alt sınıfın gerçekten üst sınıf ile hiçbir ortak noktasının olmadığı bir durum var ise, **Replace Inheritance with Delegation** tekniği ile lehine mirası ortadan kaldırın.

- Eğer miras uygunsa, alt sınıfta bulunan gereksiz alanları ve metotları ortadan kaldırın. Alt sınıf tarafından ihtiyaç duyulan tüm değerleri ve metotları üst sınıftan kaldırım, bunları yeni bir üst sınıfa koyun ve her iki sınıfı da ondan miras alacak şekilde ayarlayın (**Extract Superclass**). 

![](https://refactoring.guru/images/refactoring/content/smells/refused-bequest-03-2x.png)

**💰 Hesaplaşma**

Kodun netliğini ve organizasyonunu geliştirir. Artık her ikisinin de 4 bacağı olmasına rağmen, Köpek (`Dog`) sınıfının neden Sandalye (`Chair`) sınıfından miras alındığını merak etmenize gerek kalmayacak!

## 4️⃣ Alternative Classes with Different Interfaces (Farklı Arayüzlerle Alternatif Sınıflar)

**🤢 Belirti ve Semptomlar**

İki sınıf aynı işlevleri yerine getirmesine rağmen farklı yöntem adlarına sahipse soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/alternative-classes-with-different-interfaces-01-2x.png)

**🤒 Sorunun Nedenleri**

Sınıflardan birini oluşturan programcı muhtemelen işlevsel olarak aynı işlemi yapan eşdeğer bir sınıfın hali hazırda var olduğunu bilmiyordu. Bu yüzden aynı amaca hizmet eden iki metot oluşmuş oldu.

**💊 Tedavi**

Sınıfların arayüzünü ortak bir paydada ifade etmeye çalışın:

- Metodları aynı hale getirmek için **Rename Methods** tekniğini kullanın.

- Metodların imzasını ve uygulamasını aynı yapmak için **Move Method**, **Add Parameter** ve **Parameterize Method** tekniklerini kullanın.

- Sınıfların sadece bir kısmı tarafından kullanılan işlevsellik varsa, **Extract Superclass** tekniğini kullanmayı deneyin. Bu durumda, mevcut sınıflar, alt sınıflar haline gelecektir. Böylelikle duplicate yöntem sorunu ortadan kalkar.

- Hangi tedavi yöntemini kullanacağınızı belirledikten ve uyguladıktan sonra, sınıflardan birini silebilirsiniz.

**💰 Hesaplaşma**

- Gereksiz yinelenen kodlardan kurtulursunuz, böylece ortaya çıkan kod az hacimli ve daha temiz olur.

- Kod daha okunabilir ve anlaşılır hale gelir. Böylelikle artık birinciyle aynı işlevleri yerine getiren ikinci bir sınıfın yaratılmasının nedenini tahmin etmenize gerek yoktur. Çünkü ikinci sınıf çoktan ortadan kalkmış olacaktır!

![](https://refactoring.guru/images/refactoring/content/smells/alternative-classes-with-different-interfaces-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen sınıfları birleştirmek imkansızdır veya anlamsız olacak kadar zordur. Bunun bir örneği, alternatif sınıfların her birinin kendi sınıf sürümüne sahip farklı kitaplıklarda olmasıdır. Böyle bir durumda bu koku yok sayılabilir.


