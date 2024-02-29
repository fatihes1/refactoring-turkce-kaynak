# Vazgeçilebilir (Dispensables)

Kodunuz da vazgeçilebilir yapılar olabilir. Bu yapılar, yokluğu kod daha temiz, daha verimli ve anlaşılması daha kolay hale gelir. 

## 1️⃣ Yorumlar (Comments)

**🤢 Belirti ve Semptomlar**

Bir yöntem veya sınıf açıklayıcı yorumlarla dolu olduğunda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/comments-01-2x.png)

**🤒 Sorunun Nedenleri**

Geliştiriciler, genellikle en iyi niyetle, kodunun sezgisel veya açık olmadığını fark ettiğinde yorumları kodların eklerler. Bu gibi durumlarda yorumlar, iyileştirilebilecek şüpheli kod kokusunu maskeleyen bir deodorant gibidir.

*En iyi yorum, bir yöntem veya sınıf için iyi bir adlandırmadır.*

Bir kod parçasının yorum olmadan anlaşılamayacağını düşünüyorsanız kod yapısını yorumlara gerek duyulmayacak şekilde değiştirmeyi deneyin. Daha temiz bir kod yapısına aktarılmak, yorum ile açıklamaktan daha sürdürülebilirdir.

**💊 Tedavi**

- Bir yorum karmaşık bir ifadeyi açıklamak amacıyla kullanılıyorsa, ifade **Extract Variable** tekniği kullanılarak anlaşılır alt ifadelere bölmelisiniz.

- Bir yorum bir kod bölümünü açıklıyorsa, bu bölüm **Extract Method** tekniğini kullanılarak ayrı bir metoda dönüştürülebilir. Yeni metodun adı, büyük olasılıkla yorum metninden alınabilir. Böylelikle yorum satırına ihtiyaç duymayan, yeterince açıklayıcı bir yöntem adıyla sorunun üstesinden gelmiş olursunuz.

- Bir metod zaten çıkarılmışsa (extracted), ancak metodu ne yaptığını açıklamak için hala yorumlar gerekiyorsa, metoda açıklayıcı bir ad verin. Bunun için **Rename Method** tekniğini kullanın.

- Sistem çalışması için gerekli bir durum hakkında kuralları belirtmeniz gerekiyorsa, **Introduce Assertion** tekniğini kullanın.

**💰 Hesaplaşma**

Kod daha sezgisel ve anlaşılır hale gelir.

![](https://refactoring.guru/images/refactoring/content/smells/comments-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Yorumlar bazen yararlı olabilir:

- Bir şeyin neden belirli bir şekilde uygulandığını açıklarken.

- Karmaşık algoritmaları açıklarken (tabi ki algoritmayı basitleştirmeye yönelik diğer tüm yöntemler denendiğinde ve yetersiz kaldığında).

## 2️⃣ Tekrarlı/Kopya Kod (Duplicate Code)

**🤢 Belirti ve Semptomlar**

İki kod parçası neredeyse aynı göründüğü durumlarda kafanızda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-01-2x.png)

**🤒 Sorunun Nedenleri**

Tekrarlı kod genellikle birden fazla geliştiricinin aynı programın farklı bölümleri üzerinde aynı anda çalıştığında meydana gelir. Farklı görevler üzerinde çalıştıkları için, meslektaşlarının kendi ihtiyaçları doğrultusunda yeniden kullanılabilecek benzer bir kod yazdığından habersiz olabilirler.

Kodun belirli bölümleri farklı görünse de aslında aynı işi yaptığında, hemen fark edilemeyecek duplication da söz konusudur. Bu tür tekrarları bulmak ve düzeltmek zor olabilir.

Bazen tekrarlama bilerek yapılmış olabilir. Son teslim tarihlerine yetişmek için acele ederken ve mevcut kod iş için neredeyse doğru olduğunda, acemi programcılar ilgili kodu kopyalayıp yapıştırmanın cazibesine karşı koyamayabilirler. Ve bazı durumlarda programcı dağınıklığı gideremeyecek kadar tembeldir. Bu durumlarda kod iyice kirli hale gelir ve bir aksiyon alınmadığında, iş içinden çıkılmaz bir hal alabilir.


**💊 Tedavi**

- Aynı kod, aynı sınıfta iki veya daha fazla yöntemde bulunursa: 
**Extract Method** tekniğini kullanın ve her iki yere de yeni yöntem için çağrılar yapın.


![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-02-2x.png)

- Eğer aynı kod, aynı seviyedeki iki alt sınıfta bulunuyorsa:
	- Her iki sınıf için de **Extract Method** tekniğini kullanın, ardından üst sınıfa çıkardığınız metotta kullanılan alanlar için **Pull Up Field** tekniğini kullanın.
	- Yineleme kodu bir yapıcı (constructor) içindeyse, **Pull Up Constructor Body** tekniğini kullanın.
	- Yineleme kodu benzer ancak tamamen aynı değilse, **Form Template Method** tekniğini kullanın.
	- İki metodun aynı şeyi yaptığı ancak farklı algoritmalar kullandığı durumlarda, en iyi algoritmayı seçin ve **Substitute Algorithm** tekniğini uygulayın.

- Eğer aynı kod farklı iki sınıfta bulunuyorsa:
	- Eğer sınıflar bir hiyerarşinin bir parçası değilse, bu sınıflar için tüm önceki işlevselliği koruyan tek bir üst sınıf oluşturmak için **Extract Superclass** tekniğii kullanın.
	- Eğer bir üst sınıf oluşturmak zor veya imkansızsa, bir sınıfta **Extract Class** tekniğini kullanın. Sonrasında yeni bileşeni diğer sınıfta kullanın.

- Eğer çok sayıda koşullu ifadeler bulunuyorsa ve aynı amaca hizmet ediyorsa (sadece koşullarında farklılık gösteriyor), bu operatörleri **Consolidate Conditional Expression** tekniğini kullanarak tek bir koşula birleştirin. Bu işlemi yaparken, koşulu anlaşılır bir adı olan ayrı bir metoda yerleştirmek için **Extract Method** tekniğini kullanın.

- Eğer koşullu bir ifadenin tüm dallarında aynı kod gerçekleştiriliyorsa: **Consolidate Duplicate Conditional Fragments** tekniği kullanarak, aynı kodu koşul ağacının dışına yerleştirin.

**💰 Hesaplaşma**

- Yinelenen kodu birleştirmek, kodunuzun yapısını basitleştirir ve kısaltır.

- Basitleştirme + kısalık = basitleştirilmesi daha kolay ve desteklenme maliyeti az olan kod.

![](https://refactoring.guru/images/refactoring/content/smells/duplicate-code-03-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Çok nadir durumlarda, iki özdeş kod parçasını birleştirmek, kodu daha az sezgisel ve anlaşılır hale getirebilir. Bu tarz durumlarda yok sayılabilir.

## 3️⃣ Tembel Sınıf (Lazy Class)

**🤢 Belirti ve Semptomlar**

Sınıfları anlamak ve sürdürmek her zaman zamana ve maliyete sebep olacaktır. Dolayısıyla bir sınıf dikkatinizi çekecek kadar yeterli yerde kullanılmıyorsa göstermiyorsa silinmelidir.

![](https://refactoring.guru/images/refactoring/content/smells/lazy-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Belki bir sınıf, eski bir süreçte tamamen işlevsel olacak şekilde tasarlanmıştı. Ancak bazı refactoring süreçlerinden sonra gülünç derecede küçük hale geldi.

Veya belki de gelecekte hiç yapılmayacak geliştirme çalışmalarını desteklemek için tasarlandı. Yani henüz olmayan özelllikler için önceden hazırlandı.

**💊 Tedavi**

- Neredeyse kullanışsız olan bileşenlere **Inline Class** tekniği uygulanmalıdır.

- Az fonksiyona sahip alt sınıflar için **Collapse Hierarchy** yöntemini deneyin.

![](https://refactoring.guru/images/refactoring/content/smells/lazy-class-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azaltır.
- Daha kolay bakım sunar.

**🤫 Ne Zaman Yok Sayılmalı?**

Bazen gelecekteki gelişime yönelik niyetleri belirlemek için bir Tembel Sınıf (Lazy Class) oluşturulur. Bu durumda, kodunuzda açıklık ve basitlik arasında bir denge kurmaya çalışmalısınız.


## 4️⃣ Veri Sınıfı (Data Class)

**🤢 Belirti ve Semptomlar**

Bir veri sınıfı (data class), yalnızca alanları ve bunlara erişim için kaba yöntemleri (getters ve setters) içeren bir sınıfı ifade eder. Bunlar yalnızca diğer sınıflar tarafından kullanılan veriler için kullanılan conteynerlerdir. Bu sınıflar herhangi bir ek işlevsellik içermez ve sahip oldukları veriler üzerinde bağımsız olarak çalışamazlar. Böyle bir durumla karşılaştığınızda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/data-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Yeni oluşturulan bir sınıfın yalnızca birkaç ortak alan (ve hatta belki bir avuç getter/setter) içermesi normal bir şeydir. Ancak nesnelerin gerçek gücü, verileri üzerinde davranış türlerini veya işlemleri içerebilmeleridir.

**💊 Tedavi**

- Bir sınıf public alanlar içeriyorsa, bu alanları doğrudan erişimden gizlemek ve erişimi sadece getter ve setter'lar aracılığıyla yapılmasını sağlamak için **Encapsulate Field** tekniğini kullanın.

- Verilerin saklandığı koleksiyonlar (örneğin diziler) için **Encapsulate Collection** tekniğini kullanın.

- Sınıfı kullanan istemci kodunu gözden geçirin. Burada, işlevselliğin veri sınıfında daha iyi yer alabileceği durumları bulabilirsiniz. Eğer durum böyleyse, bu işlevselliği veri sınıfına göç ettirmek için **Move Method** ve **Extract Method** tekniklerini kullanın.

- Sınıf, düşünce ile oluşturulmuş metodlarla doldurulduktan sonra, sınıf verisine aşırı geniş erişim sağlayan eski veri erişimi metodlarından kurtulmak isteyebilirsiniz. Bu durumda, **Remove Setting Method** ve **Hide Method** yöntemleri yardımcı olabilir.


![](https://refactoring.guru/images/refactoring/content/smells/data-class-02-2x.png)

**💰 Hesaplaşma**

- Kodun anlaşılmasını ve organizasyonunu geliştirir. Belirli veriler üzerindeki işlemler artık kod boyunca gelişigüzel bir şekilde yerine tek bir yerde toplanır. Böylelikle bir standarta sahip olursunuz.
- İstemci kodunun kopyalarını tespit etmenize yardımcı olur.

## 5️⃣ Ölü Kod (Dead Code)

**🤢 Belirti ve Semptomlar**

Bir değişken, parametre, alan, yöntem veya sınıf artık kullanılmamaktadır. Bu durum genellikle eski olduğu için ortaya çıkar. Böyle bir durumda soru işaretleri oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/dead-code-01-2x.png)

**🤒 Sorunun Nedenleri**

Yazılımın gereksinimleri değiştiğinde ya da düzeltmeler yapıldığında kimsenin eski kodu temizlemeye vakti olmaz.

Bu tür kod, dallardan birine ulaşılamadığında (hata veya başka koşullar nedeniyle) karmaşık koşul ifadelerinde de bulunabilir.

**💊 Tedavi**

- Kullanılmayan kodları ve gereksiz dosyaları silin.

- Gereksiz bir sınıf durumunda, bir alt sınıf veya üst sınıf kullanılıyorsa **Inline Class** veya **Collapse Hierarchy** teknikleri uygulanabilir.

- Gereksiz parametreleri kaldırmak için **Remove Parameter** yöntemini kullanın.


![](https://refactoring.guru/images/refactoring/content/smells/dead-code-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azalır.
- Daha kolay destek sağlanır.


## 6️⃣ Spekülatif Genellik (Speculative Generality)

**🤢 Belirti ve Semptomlar**

Kullanılmayan bir sınıf, yöntem, alan veya parametre var ise soru işaretleriniz oluşmalıdır.

![](https://refactoring.guru/images/refactoring/content/smells/speculative-generality-01-2x.png)


**🤒 Sorunun Nedenleri**

Bazen kod, hiçbir zaman uygulamaya konmayan, gelecekte beklenen özellikleri desteklemek için "her ihtimale karşı" diye düşünülerek oluşturulur. Sonuç olarak bu düşünce, kodun anlaşılması ve desteklenmesi zorlaştıracaktır.

**💊 Tedavi**

- Kullanılmayan soyut (abstract) sınıfları kaldırmak için **Collapse Hierarchy** yöntemini deneyin.

- Başka bir sınıfa işlevsellik aktarmanın gereksiz olduğu durumları **Inline Class** tekniği ile ortadan kaldırabilirsiniz.

- Kullanılmayan metodlar mı? Onlardan kurtulmak için **Inline Method** tekniğini kullanın.

- Kullanılmayan parametrelere sahip metodlar, **Remove Parameter** tekniğinin yardımıyla incelenmelidir.

- Kullanılmayan alanlar basitçe silinebilir.


![](https://refactoring.guru/images/refactoring/content/smells/speculative-generality-02-2x.png)

**💰 Hesaplaşma**

- Kod boyutu azalır. 
- Daha kolay destek.

**🤫 Ne Zaman Yok Sayılmalı?**

Bir framework üzerinde çalışıyorsanız, framework kullanıcıları tarafından ihtiyaç duyulduğu sürece, çerçevenin kendisinde kullanılmayan bir işlevsellik yaratmak son derece mantıklıdır.

Öğeleri silmeden önce birim testlerde kullanılmadıklarından emin olun. Bu, testlerin bir sınıftan belirli dahili bilgileri almanın veya testle ilgili spesifik eylemleri gerçekleştirmenin bir yoluna ihtiyaç duyması durumunda gerçekleşir.

