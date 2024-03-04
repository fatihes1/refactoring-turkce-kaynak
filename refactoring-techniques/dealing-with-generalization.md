# Genellemeyle Başa Çıkmak (Dealing with Generalization)

Bu teknikler, öncelikle işlevselliği sınıf devralma hiyerarşisi boyunca taşımakla ilişkilidir. Ayrıca, yeni sınıflar ve arayüzler oluşturmayı içerirler. Bununla birlikte, soyutlamayı temsilci seçme yöntemiyle değiştirmek de mümkündür. Bu durumda, soyutlamadan türetilen sınıflar temsilcileri seçebilir. Özetle, soyutlama, hem yeni sınıflar ve arayüzler oluşturmayı hem de temsilci seçme yöntemiyle değişiklik yapmayı içeren bir dizi yeniden düzenleme tekniği sunar.

## Pull Up Field

### 🙁 Problem

İki farklı sınıf aynı field'a sahipse ortada bir kod tekrarı bulunmaktadır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Field%20-%20Before.png)
</div>

### 😊 Çözüm

Tekrar olan field'ı, alt sınıflardan kaldırın ve üst sınıfa taşıyın. Böylelikle kod tekrarının önüne geçmiş olursunuz.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Field%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Proje boyunca alt sınıflar ayrı ayrı büyüyüp geliştir. Bu durum aynı amaca hizmet eden field'ların ve yöntemlerin ortaya çıkmasına neden olabilir.

### ✅ Avantajları

Alt sınıflardaki fieldların tekrarlanmasını ortadan kaldırır.

Varsa, yinelenen yöntemlerin daha sonraki adımlarda, alt sınıflardan bir üst sınıfa taşınmasını kolaylaştırır.

### 🤯 Nasıl Refactor Edilir?

1. Alt sınıflarda buluanan field'ların aynı aynı amaca hizmet ettiğinden emin olun.

2. Fieldlar farklı adlara sahipse, onlara aynı adı verin. Daha sonra, bu field'lara yapılan tüm referansları mevcut kodla değiştirin.

3. Üst sınıfta aynı ada sahip bir field oluşturun. Field'lar private ise üst sınıf alanın korunması gerektiğini unutmayın.

4. Field'ları alt sınıflardan kaldırın.

5. Yeni field erişim yöntemlerinin arkasına gizlemek amacıyla, **Self Encapsulate Field** tekniğini kullanmayı düşünebilirsiniz.

## Pull Up Method

### 🙁 Problem

İki farklı alt sınıfın benzer işleri gerçekleştiren yöntemleri olması kod tekrarına sebep olacaktır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Method%20-%20Before.png)

</div>

### 😊 Çözüm

Yöntemleri ortaklaştırın ve ardından bu yöntemleri, üst sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Method%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Proje boyunca alt sınıflar ayrı ayrı büyüyüp geliştir. Bu durum aynı amaca hizmet eden field'ların ve yöntemlerin ortaya çıkmasına neden olabilir.


### ✅ Avantajları

- Tekrarlı kodlardan kurtulur. Bir yöntemde değişiklik yapmanız gerektiği durumda, yöntemin tüm kopyalarını alt sınıflarda aramak zorunda kalmaktansa, bu değişiklikleri tek bir yerde yapmak daha iyidir.

- Bu yeniden düzenleme tekniği, bir alt sınıfın bir üst sınıf yöntemini yeniden tanımlaması ancak temelde aynı işi gerçekleştirmesi durumunda da kullanılabilir.


### 🤯 Nasıl Refactor Edilir?

1. Üst sınıflardaki benzer yöntemleri araştırın. Aynı değillerse birbirleriyle eşleşecek şekilde biçimlendirin.

2. Yöntemler farklı bir parametre kümesi kullanıyorsa, parametreleri üst sınıfta görmek istediğiniz formda olacak şekilde yerleştirin.

3. Yöntemi üst sınıfa kopyalayın. Burada yöntem kodunun yalnızca alt sınıflarda bulunan ve dolayısıyla üst sınıfta bulunmayan alanları ve yöntemleri kullandığını görebilirsiniz. Bunu çözmek için aşağıda listelen yöntemlerden birini deneyebilirsiniz:

	- Field'lar için: alt sınıflarda alıcılar ve ayarlayıcılar (getter & setter) oluşturmak için **Pull Up Field** veya **Self-Encapsulate Field ** tekniğini kullanın; daha sonra bu alıcıları (getter) soyut (abstract) olarak üst sınıfta tanımlayın.

	- Yöntemler için:**Pull Up Method** tekniğini kullanın veya üst sınıfta onlar için soyut yöntemler tanımlayın  (sınıfınızın daha önce soyut değilse soyut hale geleceğini unutmayın).

4. Yöntemleri alt sınıflardan kaldırın.

5. Yöntemin çağrıldığı konumları kontrol edin. Bazı yerlerde bir alt sınıfın kullanımını üst sınıfla değiştirmeniz gerekebilir.


## Pull Up Constructor Body

### 🙁 Problem

Alt sınıflarınızda neredeyse aynı koda sahip kurucular (constructors) bulunuyorsa bu kod tekrarına sebep olacaktır.

```java
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    this.name = name;
    this.id = id;
    this.grade = grade;
  }
  // ...
}
```

### 😊 Çözüm

Bir üst sınıf yapıcısı (constructor) oluşturun. Daha sonra alt sınıflarda aynı olan kodu üst sınıftaki constructor'a taşıyın. Alt sınıfların yapıcılarında (constructors) üst sınıf yapıcısını (`super()`) çağırın.

```java
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    super(name, id);
    this.grade = grade;
  }
  // ...
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu refactoring tekniğinin **Pull Up Method** tekniğinden farkı nedir?

1. Java'da, alt sınıflar bir kurucuyu (constructor) miras alamaz, bu nedenle alt sınıf kurucusuna **Pull Up Method**  uygulayamaz. Üst sınıftaki tüm kurucu (constructor) kodunu kaldırdıktan sonra onu silemezsiniz. Üst sınıfta bir kurucu yaratmanın yanı sıra, üst sınıf kurucuya basit yetki verme ile alt sınıflarda da kurucuların olması gerekir.

2. C++ ve Java'da (eğer üst sınıf yapıcısını (constructor) açıkça çağırmadıysanız), üst sınıf yapıcısı otomatik olarak alt sınıf yapıcısından önce çağrılır, bu da ortak kodun yalnızca alt sınıf yapıcılarının başlangıcından itibaren taşınmasını gerekli kılar..

3. Çoğu programlama dilinde, bir alt sınıf yapıcısı, üst sınıfın parametrelerinden farklı olarak kendi parametre listesine sahip olabilir. Bu nedenle, yalnızca gerçekten ihtiyaç duyduğu parametrelerle birlikte bir üst sınıf kurucu constructor oluşturmalısınız.


### 🤯 Nasıl Refactor Edilir?

1. Bir üst sınıfta bir kurucu (constructor) oluşturun.

2. Her alt sınıfın yapıcısının başlangıcındaki ortak kodu üst sınıf yapıcısına taşıyın. Bunu yapmadan önce mümkün olduğu kadar çok ortak kodu yapıcının başlangıcına taşımaya çalışın.

3. Üst sınıf yapıcısının yapılan çağrıyı alt sınıf yapıcılarının ilk satırına yerleştirin.


## Push Down Method

### 🙁 Problem

Bir method yalnızca bir (veya birkaç) alt sınıf tarafından kullanılan bir üst sınıfta mı uygulanmış? Bu yöntemi kullanmayacak olan alt sınıfların, bu yöntemi miras olarak alması ne kadar mantıklı?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Method%20-%20Before.png)

</div>

### 😊 Çözüm

Bu yöntemi, üst sınıftan yönteme ihtiyacı olan alt sınıflara taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Method%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

İlk başta belirli bir yöntemin tüm alt sınıflar için evrensel olması amaçlanmış olabilir. Ancal gerçekte yalnızca bir veya birkaç  alt sınıfta kullanılabilir. Bu durum, özellikle planlanan yapıların hayata geçmemesi durumunda ortaya çıkabilir.

Bu tür durumlar, bir sınıf hiyerarşisinden işlevselliğin kısmen kaldırılmasından sonra da yalnızca bir alt sınıfta kullanılan bir yöntem bırakılması durumunda ortaya çıkabilir.

Bir yöntemin birden fazla alt sınıf için gerekli olduğunu, ancak tüm alt sınıflar gerekli olmadığını görürseniz, bir ara alt sınıf oluşturup yöntemi bu alt sınıfa taşımak yararlı olabilir. Böylelikle, bir yöntemin tüm alt sınıflara iletilmesinden kaynaklanacak kod çoğaltmasının önlenmesine olanak tanıyacaktır.

### ✅ Avantajları

Sınıf tutarlılığını artırır. Bir yöntem görmeyi beklediğiniz yerde bulunacaktır.


### 🤯 Nasıl Refactor Edilir?

1. Yöntemi bir alt sınıfta tanımlayın ve yöntemin gövde kodunu üst sınıftan kopyalayın.

2. Yöntemi üst sınıftan kaldırın.

3. Yöntemin kullanıldığı tüm yerleri bulun ve yeni tanımlanan alt sınıftan çağrıldığından emin olun.


## Push Down Field

### 🙁 Problem

Bir parent sınıfta bulunan field yalnızca birkaç alt sınıfta mı kullanılıyor? Böyle bir durumda **Push Down Method** daki gibi gereksiz miras durumu söz konusu olacaktır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Field%20-%20Before.png)

</div>

### 😊 Çözüm

Field'ı sadece ihtiyacı olan alt sınıflara taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Field%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

İlk başta belirli bir field'ın tüm alt sınıflar için evrensel olması amaçlanmış olabilir. Ancal gerçekte yalnızca bir veya birkaç  alt sınıfta kullanılabilir. Bu durum, özellikle planlanan yapıların hayata geçmemesi durumunda ortaya çıkabilir.

Bu tür durumlar, bir sınıf hiyerarşisinden işlevselliğin kısmen kaldırılmasından sonra da yalnızca bir alt sınıfta kullanılan bir field bırakılması durumunda ortaya çıkabilir.

Bir field birden fazla alt sınıf için gerekli olduğunu, ancak tüm alt sınıflar gerekli olmadığını görürseniz, bir ara alt sınıf oluşturup field'ı bu alt sınıfa taşımak yararlı olabilir. Böylelikle, bir field'ın tüm alt sınıflara iletilmesinden kaynaklanacak kod çoğaltmasının önlenmesine olanak tanıyacaktır.

### ✅ Avantajları

- Dahili sınıf tutarlılığını artırır. Field gerçekten bulunması gereken yerde tanımlı olur.

- Aynı anda birkaç alt sınıfa geçerken field'ları birbirinden bağımsız olarak geliştirebilirsiniz. Bu, kod çoğaltmasına neden olur, bu nedenle field'ları yalnızca gerçekten farklı şekillerde kullanmayı düşündüğünüzde aşağı doğru iletin.

### 🤯 Nasıl Refactor Edilir?

1. Gerekli tüm alt sınıflarda bir field tanımlayın.

2. Field'ı üst sınıftan kaldırın.

## Extract Subclass

### 🙁 Problem

Bir ana sınıfın yalnızca belirli durumlarda kullanılan methodları ve field'ları varsa kod karmaşıklaşır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Subclass%20-%20Before.png)

</div>

### 😊 Çözüm

Bir alt sınıf oluşturun ve bu yöntem ve field'ları sadece bu alt sınfıta kullanın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Subclass%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Ana sınıfınız, sınıf için belirli bir nadir kullanım durumunu uygulamaya yönelik yöntemlere ve field'lara sahip olabilir. Nadir de olsa, bundan sınıf sorumludur ve ilişkili tüm field'ları ve yöntemleri tamamen ayrı bir sınıfa taşımak yanlış olacaktır. Ancak bunlar bir alt sınıfa taşınabilirler ve biz de bu refactoring tekniğinin yardımıyla bunu yapacağız.

### ✅ Avantajları

- Hızlı ve kolay bir şekilde alt sınıf oluşturur.

- Ana sınıfınız halihazırda birden fazla özel durum uyguluyorsa, birkaç ayrı alt sınıf oluşturabilirsiniz.

### 🚫 Dezavantajları

Görünen basitliğine rağmen, birkaç farklı sınıf hiyerarşisini ayırmanız gerekiyorsa, Miras çıkmaza yol açabilir. Örneğin, köpeklerin boyutuna ve kürküne bağlı olarak farklı davranışlara sahip Köpekler (`Dogs`) sınıfınız varsa, iki hiyerarşiyi ortaya çıkarabilirsiniz:

- boyuta göre: Büyük (`Large`), Orta (`Medium`) ve Küçük (`Small`)

- kürkle: Pürüzsüz (`Smooth`) ve Shaggy (`Shaggy`)

Ve her şey iyi görünebilir, ancak hem büyük (`Large`) hem de Pürüzsüz  (`Smooth`) bir köpek yaratmanız gerektiğinde sorunların ortaya çıkması dışında, yalnızca bir sınıftan bir nesne oluşturabildiğiniz için. Bununla birlikte, Devralmak yerine Oluştur'u (Compose instead of Inherit) tekniğini kullanarak bu sorunu önleyebilirsiniz (**Strategy** tasarım desenine bakın). Başka bir deyişle, Köpek (`Dog`) sınıfının beden ve kürk olmak üzere iki bileşen alanı olacaktır. Bu alanlara gerekli sınıflardan bileşen nesneleri ekleyeceksiniz. Böylece `LargeSize` ve `ShaggyFur` özelliklerine sahip bir Köpek  (`Dog`) üretebilirsiniz.


### 🤯 Nasıl Refactor Edilir?

1. İlgilendiğiniz sınıftan yeni bir alt sınıf oluşturun.

2. Bir alt sınıftan nesneler oluşturmak için ek verilere ihtiyacınız varsa, bir kurucu (constructor) oluşturun ve kurucuya gerekli parametreleri ekleyin. Kurucunun ana uygulamasını çağırmaası gerektiğini unutmayın.

3. Ana sınıfın yapıcısına (constructor) yapılan tüm çağrıları bulun. Bir alt sınıfın işlevselliği gerekli olduğunda ana kurucuyu alt sınıf kurucuyla değiştirin.

4. Gerekli yöntemleri ve alanları ana sınıftan alt sınıfa taşıyın. Bunu **Push Down Method** ve **Push Down Method** teknikleri aracılığıyla yapın. Önce yöntemleri taşıyarak başlamak daha kolaydır. Bu şekilde alanlar tüm süreç boyunca erişilebilir kalır.

5. Alt sınıf hazır olduktan sonra, işlevsellik seçimini kontrol eden tüm eski field'ları bulun. Field'ları kullanıldığı tüm operatörleri değiştirmek için polimorfizmi kullanarak bu alanları silin. Basit bir örnek: Araba (`Car`) sınıfında, `isElectricCar` alanınız vardı ve buna bağlı olarak, `refuel()` yönteminde araca ya gaz dolduruluyor ya da elektrik yükleniyor. Yeniden düzenleme sonrasında `isElectricCar` alanı kaldırılır ve `Car` ve `ElectricCar` sınıfları, `refuel()` yönteminin kendi uygulamalarına (implementation) sahip olur.


## Extract Superclass

### 🙁 Problem

Ortak field ve yöntemleri sahip olan iki farklı sınıfınız var. Madem ortak değerlere sahip, neden farklı iki sınıf ki?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Superclass%20-%20Before.png)

</div>

### 😊 Çözüm

Bu iki sınıfın alt sınıf olacağı, bir üst sınıf oluşturun ve tüm aynı field ve yöntemleri bu parent sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Superclass%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bu tür kod tekrarı, iki sınıf benzer görevleri aynı şekilde gerçekleştirdiğinde veya benzer görevleri farklı şekillerde gerçekleştirdiğinde ortaya çıkar. Nesneler bu tür durumları kalıtım yoluyla basitleştirmek için yerleşik bir mekanizma sunar. Ancak çoğu zaman bu benzerlik, sınıflar oluşturulana kadar fark edilmeden kalır ve daha sonra bir miras yapısının oluşturulmasını gerekli kılar.

### ✅ Avantajları

Kod tekilleştirme. Ortak alanlar ve yöntemler artık yalnızca tek bir yerde bulunur.

### 🖐🏼 Ne Zaman Kullanılmamalı?

Bu tekniği halihazırda bir üst sınıfa sahip olan sınıflara uygulayamazsınız.

### 🤯 Nasıl Refactor Edilir?

1. Soyut (abstract) bir üst sınıf oluşturun.

2. Ortak işlevselliği bir üst sınıfa taşımak için **Pull Up Field**, **Pull Up Method** ve **Pull Up Constructor Body** tekniklerini kullanın. Field'lar ile başlayın, çünkü ortak alanlara ek olarak ortak yöntemlerde kullanılan alanları da taşımanız gerekecektir.

3. İstemci kodunda, alt sınıfların kullanımının yeni sınıfınızla değiştirilebileceği yerleri arayın.


## Extract Interface

### 🙁 Problem

Birden fazla istemci bir sınıf arayüzünün (interface) aynı bölümünü kullanıyor. Bununla beraber iki sınıftaki arayüzün bir kısmı aynı olabilir. Bu bir tür kod tekrarı değil mi?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Interface%20-%20Before.png)

</div>

### 😊 Çözüm

Bu sınıflardaki özdeş kısmı kendi arayüzüne taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Interface%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

1. Sınıflar farklı durumlarda özel işlevler uyguladığında arayüzler çok uygundur. Hangi rolü kullanacağını açıkça belirtmek için **Extract Interface** kullanın.

2. Bir sınıfın sunucusunda gerçekleştirdiği işlemleri tanımlamanız gerektiğinde başka bir uygun durum ortaya çıkar. Sonunda birden fazla türdeki sunucuların kullanımına izin verilmesi planlanıyorsa tüm sunucuların arayüzü uygulaması gerekir.

### 🤓 Bilinmesinde Yarar Var

**Extract Superclass** ve **Extract Interface** teknikleri arasında belirli bir benzerlik vardır.

Bir arayüzün çıkarılması, yalnızca ortak arayüzlerin yalıtılmasına olanak tanır. Başka bir deyişle, eğer sınıflar **Duplicate Code** içeriyorsa, arayüzün çıkarılması yani extract edilmesi tekilleştirmenize yardımcı olmayacaktır.

Yine de, tekrarlı kodu içeren davranışı ayrı bir bileşene taşımak ve tüm işi ona devretmek için **Extract Class** tekniğini uygulanarak bu sorun biraz da olsa azaltılabilir. Ortak davranışın boyutu büyükse, her zaman **Extract Superclass** tekniğini kullanabilirsiniz. Bu elbette daha da kolay olacaktır. Ancak bu yolu seçerseniz yalnızca bir parent sınıfı kullanabileceğinizi unutmayın.

### 🤯 Nasıl Refactor Edilir?

1. Boş bir arayüz (interface) oluşturun.

2. Arayüzdeki ortak işlemleri tanımlayın.

3. Arayüzü uygulayan olarak gerekli sınıfları oluşturun.

4. Yeni arayüzü kullanmak için istemci kodundaki tür tanımlamalarını (type declarations) değiştirin.


## Collapse Hierarchy

### 🙁 Problem

Bir alt sınıfın neredeyse üst sınıfıyla aynı olduğu bir sınıf hiyerarşiniz var. O halde alt sınıf neden var?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Collapse%20Hierarchy%20-%20Before.png)

</div>

### 😊 Çözüm

Alt sınıf ve üst sınıfı birleştirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Collapse%20Hierarchy%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Programınız zamanla büyür ve alt sınıf ile üst sınıf neredeyse aynı hale gelebilir. Bir alt sınıftan bir özellik kaldırılabilir, bir yöntem üst sınıfa taşınabilir... ve artık iki benzer sınıfınız olmuş olur. Bu ne kadar gereklidir kş?

### ✅ Avantajları

- Program karmaşıklığı azalır. Daha az sınıf, aklınızda tutmanız gereken daha az şey ve gelecekteki kod değişiklikleri sırasında endişelenecek daha az bozulabilir dinmik parça anlamına gelir.

- Yöntemler bir sınıfta erken tanımlandığında kodunuzda gezinmek ve kodu anlamak daha kolaydır. Belirli bir yöntemi bulmak için tüm hiyerarşiyi taramanıza gerek kalmaz.

### 🖐🏼 Ne Zaman Kullanılmamalı?

- Refactoring ettiğiniz sınıf hiyerarşisinde birden fazla alt sınıf mı var? Eğer öyleyse, refactoring sürecini tamamlandıktan sonra geri kalan alt sınıflar, hiyerarşinin çöktüğü sınıfın mirasçıları haline gelmelidir.

- Ancak bunun Liskov ikame ilkesinin ihlaline yol açabileceğini unutmayın. Örneğin, programınız şehir içi ulaşım ağlarını taklit ediyorsa ve Taşıma (`Transport`) üst sınıfını yanlışlıkla Araba (`Car`) alt sınıfına daraltırsanız, Plane (`Uçak`) sınıfı Araba'nın (`Car`) mirasçısı olabilir. Hata! Bu tarz durumlar dönüşü olmayan sorunlara yol açabilir.

### 🤯 Nasıl Refactor Edilir?

1. Hangi sınıfın kaldırılmasının daha kolay olduğunu seçin: üst sınıf mı yoksa alt sınıf mı?

2. Alt sınıftan kurtulmaya karar verirseniz **Pull Up Field** ve **Pull Up Method** tekniklerini kullanın. Üst sınıfı ortadan kaldırmayı seçerseniz, **Push Down Field** ve **Push Down Method** tekniklerini tercih edin.

3. Sildiğiniz sınıfın tüm kullanımlarını, field listelerini ve yöntemlerini taşınacağı sınıfla aktarın.

5. Boş sınıfı silin.


## Form Template Method

### 🙁 Problem

Alt sınıflarınız, benzer adımları aynı sırayla içeren algoritmalar içeriyorsa burada refactoring için çanlar çalıyor olabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Form%20Template%20Method%20-%20Before.png)

</div>

### 😊 Çözüm

Algoritma yapısını ve aynı adımları bir üst sınıfa taşıyın ve farklı adımların uygulanmasını alt sınıflarda yapacak şekilde oldukları yerde bırakın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Form%20Template%20Method%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Alt sınıflar paralel olarak, bazen farklı kişiler tarafından geliştirilir, bu da her değişikliğin tüm alt sınıflarda yapılması gerektiğinden kod tekrarına, hatalara ve kod bakımında zorluklara yol açar.

### ✅ Avantajları

- Kod çoğaltma her zaman basit kopyalama/yapıştırma durumlarını ifade etmez. Çoğunlukla çoğaltma daha yüksek düzeyde gerçekleşir; örneğin sayıları sıralamak için bir yönteminiz ve yalnızca öğelerin karşılaştırılmasıyla farklılaşan nesne koleksiyonlarını sıralamak için bir yönteminiz olduğu durumlar gibi. Bir şablon yöntemi oluşturmak, paylaşılan algoritma adımlarını bir üst sınıfta birleştirerek ve yalnızca alt sınıflardaki farklılıkları bırakarak bu çoğaltmayı ortadan kaldırır.

- Bir şablon yöntemi oluşturmak, Açık/Kapalı Prensibinin uygulamalı bir örneğidir. Yeni bir algoritma sürümü göründüğünde yalnızca yeni bir alt sınıf oluşturmanız gerekir; Mevcut kodda herhangi bir değişiklik yapılmasına gerek yoktur.

### 🤯 Nasıl Refactor Edilir?

1. Alt sınıflardaki algoritmaları, ayrı yöntemlerde açıklanan kurucu parçalarına ayırın. **Extract Method** tekniği bu konuda yardımcı olabilir.

2. Sonuçta ortaya çıkan tüm alt sınıflar için aynı olan yöntemler, **Pull Up Method** tekniği kullanılarak bir üst sınıfa taşınabilir.

3. Benzer olmayan yöntemlere **Rename Method** tekniği ile tutarlı adlar verilebilir.

4. Benzer olmayan yöntemlerin imzalarını **Pull Up Method** tekniği kullanarak soyut olarak bir üst sınıfa taşıyın. Uygulamalarını alt sınıflarda bırakın.

5. Ve son olarak, algoritmanın ana yöntemini üst sınıfa çekin. Artık hem gerçek hem de soyut olarak üst sınıfta açıklanan yöntem adımlarıyla çalışmalıdır.

## Replace Inheritance with Delegation

### 🙁 Problem

Üst sınıfının yöntemlerinin yalnızca bir kısmını kullanan bir alt sınıfınız varsa neden bu sınıf bir alt sınıf olarak miras alıyor?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Inheritance%20with%20Delegation%20-%20Before.png)

</div>

### 😊 Çözüm

Bir field oluşturun ve içine bir üst sınıf nesnesi koyun, yöntemleri üst sınıf nesnesine devredin ve mirastan kurtulun.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Inheritance%20with%20Delegation%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Kalıtımı kompozisyonla değiştirmek, aşağıdaki durumlarda sınıf tasarımını önemli ölçüde iyileştirebilir:

- Alt sınıfınız Liskov ikame ilkesini ihlal ediyor; yani, kalıtım yalnızca ortak kodu birleştirmek için uygulandıysa, ancak alt sınıfın üst sınıfın bir uzantısı olması nedeniyle uygulanmadıysa.

- Alt sınıf, üst sınıfın yöntemlerinin yalnızca bir kısmını kullanır. Bu durumda, birinin aramaması gereken bir süper sınıf yöntemini çağırması an meselesidir.

Temelde, bu yeniden düzenleme tekniği her iki sınıfı da böler ve üst sınıfı alt sınıfın ebeveyni değil yardımcısı yapar. Tüm üst sınıf yöntemlerini miras almak yerine, alt sınıf yalnızca üst sınıf nesnesinin yöntemlerine yetki vermek için gerekli yöntemlere sahip olacaktır.

### ✅ Avantajları

- Bir sınıf, üst sınıftan miras alınan gereksiz yöntemleri içermez.

- Delege alanına çeşitli uygulamalara sahip çeşitli nesneler yerleştirilebilir. Aslında **Strategy** tasarım modelini elde edersiniz.

### 🚫 Dezavantajları

Birçok basit yetki verme yöntemi yazmanız gerekir.

### 🤯 Nasıl Refactor Edilir?

1. Üst sınıfı tutmak için alt sınıfta bir field oluşturun. İlk aşamada mevcut nesneyi içine yerleştirin.

2. Alt sınıf yöntemlerini,`this` yerine üst sınıf nesnesini kullanacak şekilde değiştirin.

3. İstemci kodunda çağrılan üst sınıftan miras alınan yöntemler için, alt sınıfta basit yetki verme yöntemleri oluşturun.

4. Miras tanımlamalarını alt sınıftan kaldırın.

5. Yeni bir nesne oluşturarak, eski üst sınıfın depolandığı field'ın başlatma kodunu değiştirin.

## Replace Delegation with Inheritance

### 🙁 Problem

Bir sınıf, başka bir sınıfın tüm yöntemlerine yetki veren birçok basit yöntem içerir. Bu ne kadar doğru olabilir ki?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Delegation%20with%20Inheritance%20-%20Before.png)

</div>

### 😊 Çözüm

Sınıfı bir temsilci mirasçısı (delegate inheritor) yapın, bu da yetki verme yöntemlerini gereksiz kılar.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Delegation%20with%20Inheritance%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Yetki devri (Delegation), mirasa göre daha esnek bir yaklaşımdır çünkü yetki devrinin nasıl uygulandığını değiştirmeye ve diğer sınıfları da buraya yerleştirmeye olanak tanır. Bununla birlikte, eylemleri yalnızca bir sınıfa ve onun tüm genel yöntemlerine devrederseniz, yetki vermenin yararlılığı sona erer.

Böyle bir durumda, delegasyonu kalıtımla değiştirirseniz, sınıfı çok sayıda delege etme yönteminden temizlersiniz ve her yeni temsilci sınıf yöntemi için bunları oluşturma zorunluluğundan kurtulursunuz.

### ✅ Avantajları

Kod uzunluğunu azaltır. Tüm bu yetki verme yöntemlerine artık gerek yok.

### 🖐🏼 Ne Zaman Kullanılmamalı?

- Sınıf, temsilci sınıfının genel yöntemlerinin yalnızca bir kısmına yetki içeriyorsa bu tekniği kullanmayın. Bunu yaparak Liskov ikame ilkesini ihlal etmiş olursunuz.

- Bu teknik yalnızca sınıfın hala ebeveynleri yoksa kullanılabilir.


### 🤯 Nasıl Refactor Edilir?

1. Sınıfı, temsilci sınıfının bir alt sınıfı yapın.

2. Geçerli nesneyi, temsilci nesnesine referans içeren bir field'a yerleştirin.

3. Basit delegasyona sahip yöntemleri birer birer silin. Adları farklıysa tüm yöntemlere tek bir ad vermek için **Rename Method** tekniğini kullanın.

4. Temsilci alanına yapılan tüm referansları geçerli nesneye yapılan referanslarla değiştirin.

5. Temsilci alanını kaldırın.

