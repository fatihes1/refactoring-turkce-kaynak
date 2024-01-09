# Vazgeçilebilir (Dispensables)

Vazgeçilebilir yapılar, yokluğu kodu daha temiz, daha verimli ve anlaşılması daha kolay hale getirecek anlamsız ve gereksiz bir şeydir.

## 1️⃣ Yorumlar (Comments)

**🤢 Belirti ve Semptomlar**

Bir yöntem veya sınıf açıklayıcı yorumlarla doludur.

![](https://refactoring.guru/images/refactoring/content/smells/comments-01-2x.png)

**🤒 Sorunun Nedenleri**

Yorumlar genellikle en iyi niyetle, yazar kodunun sezgisel veya açık olmadığını fark ettiğinde oluşturulur. Bu gibi durumlarda yorumlar, iyileştirilebilecek şüpheli kod kokusunu maskeleyen bir deodorant gibidir.

*En iyi yorum, bir yöntem veya sınıf için iyi bir adlandırmadır.*

Bir kod parçasının yorum olmadan anlaşılamayacağını düşünüyorsanız kod yapısını yorumlara gerek duyulmayacak şekilde değiştirmeyi deneyin.

**💊 Tedavi**

- Bir yorum karmaşık bir ifadeyi açıklamak amacıyla kullanılıyorsa, ifade **Extract Variable** kullanılarak anlaşılır alt ifadelere bölünmelidir.

- Bir yorum bir kod bölümünü açıklıyorsa, bu bölüm **Extract Method** kullanılarak ayrı bir metoda dönüştürülebilir. Yeni metodun adı, büyük olasılıkla yorum metninden alınabilir.

- Bir metod zaten çıkarılmışsa (extracted), ancak metodu ne yaptığını açıklamak için hala yorumlar gerekiyorsa, metoda açıklayıcı bir ad verin. Bunun için **Rename Method** kullanın.

- Sistem çalışması için gerekli bir durum hakkında kuralları belirtmeniz gerekiyorsa, **Introduce Assertion** kullanın.

**💰 Hesaplaşma**

Kod daha sezgisel ve anlaşılır hale gelir.

![](https://refactoring.guru/images/refactoring/content/smells/comments-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Yorumlar bazen yararlı olabilir:

- Bir şeyin neden belirli bir şekilde uygulandığını açıklarken.

- Karmaşık algoritmaları açıklarken (algoritmayı basitleştirmeye yönelik diğer tüm yöntemler denendiğinde ve yetersiz kaldığında).

## 2️⃣ Tekrarlı/Kopya Kod (Duplicate Code)

**🤢 Belirti ve Semptomlar**

İki kod parçası neredeyse aynı göründüğü durumlardır.

![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-01-2x.png)

**🤒 Sorunun Nedenleri**

Tekrarlı kod genellikle birden fazla programcı aynı programın farklı bölümleri üzerinde aynı anda çalıştığında meydana gelir. Farklı görevler üzerinde çalıştıkları için, meslektaşlarının kendi ihtiyaçları doğrultusunda yeniden kullanılabilecek benzer bir kod yazdığından habersiz olabilirler.

Kodun belirli bölümleri farklı görünse de aslında aynı işi yaptığında, daha incelikli duplication da söz konusudur. Bu tür tekrarları bulmak ve düzeltmek zor olabilir.

Bazen tekrarlama bilerek yapılmış olabilir. Son teslim tarihlerine yetişmek için acele ederken ve mevcut kod iş için "neredeyse doğru" olduğunda, acemi programcılar ilgili kodu kopyalayıp yapıştırmanın cazibesine karşı koyamayabilirler. Ve bazı durumlarda programcı dağınıklığı gideremeyecek kadar tembeldir.


**💊 Tedavi**

- Aynı kod, aynı sınıfta iki veya daha fazla yöntemde bulunursa: 
**Extract Method** kullanın ve her iki yere de yeni yöntem için çağrılar yapın.


![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-02-2x.png)

- Eğer aynı kod, aynı seviyedeki iki alt sınıfta bulunuyorsa:
	- Her iki sınıf için de **Extract Method** kullanın, ardından yukarı çektiğiniz metotta kullanılan alanlar için **Pull Up Field** kullanın.
	- Yineleme kodu bir yapıcı (constructor) içindeyse, **Pull Up Constructor Body** kullanın.
	- Yineleme kodu benzer ancak tamamen aynı değilse, **Form Template Method** kullanın.
	- İki metodun aynı şeyi yaptığı ancak farklı algoritmalar kullandığı durumlarda, en iyi algoritmayı seçin ve **Substitute Algorithm** uygulayın.

- Eğer aynı kod farklı iki sınıfta bulunuyorsa:
	- Eğer sınıflar bir hiyerarşinin bir parçası değilse, bu sınıflar için tüm önceki işlevselliği koruyan tek bir üst sınıf oluşturmak için **Extract Superclass** kullanın.
	- Eğer bir üst sınıf oluşturmak zor veya imkansızsa, bir sınıfta **Extract Class** kullanın ve yeni bileşeni diğer sınıfta kullanın.

- Eğer çok sayıda koşullu ifadeler bulunuyorsa ve aynı kodu gerçekleştiriyorsa (sadece koşullarında farklılık gösteriyor), bu operatörleri **Consolidate Conditional Expression** kullanarak tek bir koşula birleştirin ve koşulu anlaşılır bir adı olan ayrı bir metoda yerleştirmek için **Extract Method** kullanın.

- Eğer koşullu bir ifadenin tüm dallarında aynı kod gerçekleştiriliyorsa: **Consolidate Duplicate Conditional Fragments** kullanarak, aynı kodu koşul ağacının dışına yerleştirin.

**💰 Hesaplaşma**

- Yinelenen kodu birleştirmek, kodunuzun yapısını basitleştirir ve kısaltır.

- Basitleştirme + kısalık = basitleştirilmesi daha kolay ve desteklenmesi daha ucuz olan kod.

![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Çok nadir durumlarda, iki özdeş kod parçasını birleştirmek, kodu daha az sezgisel ve anlaşılır hale getirebilir.

## 3️⃣ Tembel Sınıf (Lazy Class)

**🤢 Belirti ve Semptomlar**

Sınıfları anlamak ve sürdürmek her zaman zamana ve maliyete sebep olur. Dolayısıyla bir sınıf dikkatinizi çekecek kadar yeterli çaba göstermiyorsa silinmelidir.

![](https://refactoring.guru/images/refactoring/content/smells/lazy-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Belki bir sınıf tamamen işlevsel olacak şekilde tasarlanmıştı, ancak bazı yeniden düzenlemelerden sonra gülünç derecede küçük hale geldi.

Veya belki de gelecekte hiç yapılmayacak geliştirme çalışmalarını desteklemek için tasarlandı.

**💊 Tedavi**

- Neredeyse kullanışsız olan bileşenlere **Inline** Class tedavisi uygulanmalıdır.

- Az fonksiyona sahip alt sınıflar için **Collapse Hierarchy** yöntemini deneyin.

![](https://refactoring.guru/images/refactoring/content/smells/lazy-class-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azaltıldı. 
- Daha kolay bakım.

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen gelecekteki gelişime yönelik niyetleri belirlemek için bir Tembel Sınıf (Lazy Class) oluşturulur. Bu durumda, kodunuzda açıklık ve basitlik arasında bir denge kurmaya çalışın.


## 4️⃣ Veri Sınıfı (Data Class)

**🤢 Belirti ve Semptomlar**

Bir veri sınıfı, yalnızca alanları ve bunlara erişim için kaba yöntemleri (getters ve setters) içeren bir sınıfı ifade eder. Bunlar yalnızca diğer sınıflar tarafından kullanılan veriler için kullanılan kaplardır. Bu sınıflar herhangi bir ek işlevsellik içermez ve sahip oldukları veriler üzerinde bağımsız olarak çalışamazlar.

![](https://refactoring.guru/images/refactoring/content/smells/data-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Yeni oluşturulan bir sınıfın yalnızca birkaç ortak alan (ve hatta belki bir avuç getter/setter) içermesi normal bir şeydir. Ancak nesnelerin gerçek gücü, verileri üzerinde davranış türlerini veya işlemleri içerebilmeleridir.

**💊 Tedavi**

- Bir sınıf public alanlar içeriyorsa, bu alanları doğrudan erişimden gizlemek ve erişimi sadece getter ve setter'lar aracılığıyla yapılmasını sağlamak için **Encapsulate Field** kullanın.

- Verilerin saklandığı koleksiyonlar (örneğin diziler) için **Encapsulate Collection** kullanın.

- Sınıfı kullanan istemci kodunu gözden geçirin. Burada, işlevselliğin veri sınıfında daha iyi yer alabileceği durumları bulabilirsiniz. Eğer durum böyleyse, bu işlevselliği veri sınıfına göç ettirmek için **Move Method** ve **Extract Method** kullanın.

- Sınıf, düşünce ile oluşturulmuş metodlarla doldurulduktan sonra, sınıf verisine aşırı geniş erişim sağlayan eski veri erişimi metodlarından kurtulmak isteyebilirsiniz. Bu durumda, **Remove Setting Method** ve **Hide Method** yöntemleri yardımcı olabilir.


![](https://refactoring.guru/images/refactoring/content/smells/data-class-02-2x.png)

**💰 Hesaplaşma**

- Kodun anlaşılmasını ve organizasyonunu geliştirir. Belirli veriler üzerindeki işlemler artık kod boyunca gelişigüzel bir şekilde yerine tek bir yerde toplanır.
- İstemci kodunun kopyalarını tespit etmenize yardımcı olur.


## 5️⃣ Ölü Kod (Dead Code)

**🤢 Belirti ve Semptomlar**

Bir değişken, parametre, alan, yöntem veya sınıf artık kullanılmamaktadır (genellikle eski olduğu için).

![](https://refactoring.guru/images/refactoring/content/smells/dead-code-01-2x.png)

**🤒 Sorunun Nedenleri**

Yazılımın gereksinimleri değiştiğinde ya da düzeltmeler yapıldığında kimsenin eski kodu temizlemeye vakti olmaz.

Bu tür kod, dallardan birine ulaşılamadığında (hata veya başka koşullar nedeniyle) karmaşık koşul ifadelerinde de bulunabilir.

**💊 Tedavi**

- Kullanılmayan kodları ve gereksiz dosyaları silin.

- Gereksiz bir sınıf durumunda, bir alt sınıf veya üst sınıf kullanılıyorsa **Inline Class** veya **Collapse Hierarchy** uygulanabilir.

- Gereksiz parametreleri kaldırmak için **Remove Parameter** yöntemini kullanın.


![](https://refactoring.guru/images/refactoring/content/smells/dead-code-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azaltıldı.
- Daha basit destek.


## 6️⃣ Spekülatif Genellik (Speculative Generality)

**🤢 Belirti ve Semptomlar**

Kullanılmayan bir sınıf, yöntem, alan veya parametre var.

![](https://refactoring.guru/images/refactoring/content/smells/speculative-generality-01-2x.png)


**🤒 Sorunun Nedenleri**

Bazen kod, hiçbir zaman uygulamaya konmayan, gelecekte beklenen özellikleri desteklemek için "her ihtimale karşı" oluşturulur. Sonuç olarak bu düşünce, kodun anlaşılması ve desteklenmesi zorlaşır.

**💊 Tedavi**

- Kullanılmayan soyut sınıfları kaldırmak için **Collapse Hierarchy** yöntemini deneyin.

- Başka bir sınıfa işlevsellik aktarmanın gereksiz olduğu durumları **Inline Class** ile ortadan kaldırabilirsiniz.

- Kullanılmayan metodlar mı? Onlardan kurtulmak için **Inline Method** kullanın.

- Kullanılmayan parametrelere sahip metodlar, **Remove Parameter**  yardımıyla incelenmelidir.

- Kullanılmayan alanlar basitçe silinebilir.


![](https://refactoring.guru/images/refactoring/content/smells/speculative-generality-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azaltıldı. 
- Daha kolay destek.

**🤫 Ne Zaman Yok Sayılmalı?**

Bir framework üzerinde çalışıyorsanız, framework kullanıcıları tarafından ihtiyaç duyulduğu sürece, çerçevenin kendisinde kullanılmayan bir işlevsellik yaratmak son derece mantıklıdır.

Öğeleri silmeden önce birim testlerde kullanılmadıklarından emin olun. Bu, testlerin bir sınıftan belirli dahili bilgileri almanın veya testle ilgili özel eylemleri gerçekleştirmenin bir yoluna ihtiyaç duyması durumunda gerçekleşir.

