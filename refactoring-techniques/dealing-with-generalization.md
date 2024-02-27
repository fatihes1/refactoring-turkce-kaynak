# Genellemeyle Başa Çıkmak (Dealing with Generalization)

Soyutlamanın, öncelikle işlevselliği sınıf kalıtım hiyerarşisi boyunca hareket ettirmek, yeni sınıflar ve arayüzler oluşturmak ve mirasın delegasyonla değiştirilmesi veya tam tersi ile ilişkili kendi yeniden düzenleme teknikleri grubu vardır.

## Pull Up Field

### 🙁 Problem

İki sınıf aynı alana sahiptir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Field%20-%20Before.png)
</div>

### 😊 Çözüm

Alanı alt sınıflardan kaldırın ve üst sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Field%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Alt sınıflar ayrı ayrı büyüyüp gelişti ve aynı (veya neredeyse aynı) alanların ve yöntemlerin ortaya çıkmasına neden oldu.

### ✅ Avantajları

Alt sınıflardaki alanların çoğaltılmasını ortadan kaldırır.

Varsa, yinelenen yöntemlerin daha sonra alt sınıflardan bir üst sınıfa taşınmasını kolaylaştırır.

### 🤯 Nasıl Refactor Edilir?

1. Alt sınıflarda alanların aynı ihtiyaçlar için kullanıldığından emin olun.

2. Alanların farklı adları varsa, onlara aynı adı verin ve alanlara yapılan tüm referansları mevcut kodla değiştirin.

3. Üst sınıfta aynı ada sahip bir alan oluşturun. Alanlar özelse üst sınıf alanın korunması gerektiğini unutmayın.

4. Alanları alt sınıflardan kaldırın.

5. Yeni alanı erişim yöntemlerinin arkasına gizlemek amacıyla, **Self Encapsulate Field** tekniğini kullanmayı düşünebilirsiniz.

## Pull Up Method

### 🙁 Problem

Alt sınıflarınızın benzer işleri gerçekleştiren yöntemleri vardır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Yöntemleri aynı yapın ve ardından bunları ilgili üst sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Pull%20Up%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Alt sınıflar birbirinden bağımsız olarak büyüyüp gelişti ve aynı (veya neredeyse aynı) alanlara ve yöntemlere neden oldu.


### ✅ Avantajları

- Yinelenen kodlardan kurtulur. Bir yöntemde değişiklik yapmanız gerekiyorsa, yöntemin tüm kopyalarını alt sınıflarda aramak zorunda kalmaktansa bunu tek bir yerde yapmak daha iyidir.

- Bu yeniden düzenleme tekniği, bir alt sınıfın bir üst sınıf yöntemini yeniden tanımlaması ancak temelde aynı işi gerçekleştirmesi durumunda da kullanılabilir.


### 🤯 Nasıl Refactor Edilir?

1. Üst sınıflardaki benzer yöntemleri araştırın. Aynı değillerse birbirleriyle eşleşecek şekilde biçimlendirin.

2. Yöntemler farklı bir parametre kümesi kullanıyorsa, parametreleri üst sınıfta görmek istediğiniz forma yerleştirin.

3. Yöntemi üst sınıfa kopyalayın. Burada yöntem kodunun yalnızca alt sınıflarda bulunan ve dolayısıyla üst sınıfta bulunmayan alanları ve yöntemleri kullandığını görebilirsiniz. Bunu çözmek için şunları yapabilirsiniz:

	- Alanlar için: alt sınıflarda alıcılar ve ayarlayıcılar oluşturmak için **Pull Up Field** veya **Self-Encapsulate Field ** tekniğini kullanın; daha sonra bu alıcıları soyut olarak üst sınıfta ilan edin.

	- Yöntemler için:**Pull Up Method** tekniğini kullanın veya üst sınıfta onlar için soyut yöntemler bildirin (sınıfınızın daha önce soyut değilse soyut hale geleceğini unutmayın).

4. Yöntemleri alt sınıflardan kaldırın.

5. Yöntemin çağrıldığı konumları kontrol edin. Bazı yerlerde bir alt sınıfın kullanımını üst sınıfla değiştirebilirsiniz.


## Pull Up Constructor Body

### 🙁 Problem

Alt sınıflarınızda çoğunlukla aynı koda sahip kurucular bulunur.

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

Bir üst sınıf yapıcısı oluşturun ve alt sınıflarda aynı olan kodu ona taşıyın. Alt sınıf yapıcılarında üst sınıf yapıcısını çağırın.

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

1. Java'da, alt sınıflar bir kurucuyu miras alamaz, bu nedenle alt sınıf kurucusuna **Pull Up Method**  uygulayamaz ve üst sınıftaki tüm kurucu kodunu kaldırdıktan sonra onu silemezsiniz. Üst sınıfta bir kurucu yaratmanın yanı sıra, üst sınıf kurucuya basit yetki verme ile alt sınıflarda da kurucuların olması gerekir.

2. C++ ve Java'da (eğer üst sınıf yapıcısını açıkça çağırmadıysanız), üst sınıf yapıcısı otomatik olarak alt sınıf yapıcısından önce çağrılır, bu da ortak kodun yalnızca alt sınıf yapıcılarının başlangıcından itibaren taşınmasını gerekli kılar (çünkü 'kazanırsınız' (bir alt sınıf kurucusundaki herhangi bir yerden üst sınıf kurucusunu çağıramazsınız).

3. Çoğu programlama dilinde, bir alt sınıf yapıcısı, üst sınıfın parametrelerinden farklı olarak kendi parametre listesine sahip olabilir. Bu nedenle, yalnızca gerçekten ihtiyaç duyduğu parametrelerle bir üst sınıf kurucu oluşturmalısınız.


### 🤯 Nasıl Refactor Edilir?

1. Bir üst sınıfta bir kurucu oluşturun.

2. Her alt sınıfın yapıcısının başlangıcındaki ortak kodu üst sınıf yapıcısına çıkarın. Bunu yapmadan önce mümkün olduğu kadar çok ortak kodu yapıcının başlangıcına taşımaya çalışın.

3. Üst sınıf yapıcısının çağrısını alt sınıf yapıcılarının ilk satırına yerleştirin.


## Push Down Method

### 🙁 Problem

Davranış yalnızca bir (veya birkaç) alt sınıf tarafından kullanılan bir üst sınıfta mı uygulanıyor?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Bu davranışı alt sınıflara taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

İlk başta belirli bir yöntemin tüm sınıflar için evrensel olması amaçlanmıştı ancak gerçekte yalnızca bir alt sınıfta kullanılıyordu. Bu durum, planlanan özelliklerin gerçekleşmemesi durumunda ortaya çıkabilir.

Bu tür durumlar, bir sınıf hiyerarşisinden işlevselliğin kısmen çıkarılmasından (veya kaldırılmasından) sonra da yalnızca bir alt sınıfta kullanılan bir yöntem bırakılarak ortaya çıkabilir.

Bir yöntemin birden fazla alt sınıf için gerekli olduğunu, ancak bunların hepsinin gerekli olmadığını görürseniz, bir ara alt sınıf oluşturup yöntemi ona taşımak yararlı olabilir. Bu, bir yöntemin tüm alt sınıflara itilmesinden kaynaklanacak kod çoğaltmasının önlenmesine olanak tanır.

### ✅ Avantajları

Sınıf tutarlılığını artırır. Bir yöntem görmeyi beklediğiniz yerde bulunur.


### 🤯 Nasıl Refactor Edilir?

1. Yöntemi bir alt sınıfta bildirin ve kodunu üst sınıftan kopyalayın.

2. Yöntemi üst sınıftan kaldırın.

3. Yöntemin kullanıldığı tüm yerleri bulun ve gerekli alt sınıftan çağrıldığını doğrulayın.


## Push Down Field

### 🙁 Problem

Bir alan yalnızca birkaç alt sınıfta mı kullanılıyor?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Field%20-%20Before.png)
</div>

### 😊 Çözüm

Alanı bu alt sınıflara taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Push%20Down%20Field%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir alanın tüm sınıflar için evrensel olarak kullanılması planlanmış olsa da gerçekte alan yalnızca bazı alt sınıflarda kullanılmaktadır. Bu durum, örneğin planlanan özelliklerin ortaya çıkmaması durumunda ortaya çıkabilir.

Bu aynı zamanda sınıf hiyerarşilerinin işlevselliğinin bir kısmının çıkarılması (veya kaldırılması) nedeniyle de ortaya çıkabilir.

### ✅ Avantajları

- Dahili sınıf tutarlılığını artırır. Gerçekte kullanıldığı yerde bir alan bulunur.

- Aynı anda birkaç alt sınıfa geçerken alanları birbirinden bağımsız olarak geliştirebilirsiniz. Bu, kod çoğaltmasına neden olur, evet, bu nedenle alanları yalnızca gerçekten farklı şekillerde kullanmayı düşündüğünüzde aşağı doğru itin.

### 🤯 Nasıl Refactor Edilir?

1. Gerekli tüm alt sınıflarda bir alan bildirin.

2. Alanı üst sınıftan kaldırın.

## Extract Subclass

### 🙁 Problem

Bir sınıfın yalnızca belirli durumlarda kullanılan özellikleri vardır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Subclass%20-%20Before.png)
</div>

### 😊 Çözüm

Bir alt sınıf oluşturun ve bu durumlarda onu kullanın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Subclass%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Ana sınıfınız, sınıf için belirli bir nadir kullanım durumunu uygulamaya yönelik yöntemlere ve alanlara sahiptir. Nadir de olsa, bundan sınıf sorumludur ve ilişkili tüm alanları ve yöntemleri tamamen ayrı bir sınıfa taşımak yanlış olur. Ancak bunlar bir alt sınıfa taşınabilirler ve biz de bu yeniden düzenleme tekniğinin yardımıyla bunu yapacağız.

### ✅ Avantajları

- Hızlı ve kolay bir şekilde alt sınıf oluşturur.

- Ana sınıfınız halihazırda birden fazla özel durum uyguluyorsa, birkaç ayrı alt sınıf oluşturabilirsiniz.

### 🚫 Dezavantajları

Görünen basitliğine rağmen, birkaç farklı sınıf hiyerarşisini ayırmanız gerekiyorsa, Miras çıkmaza yol açabilir. Örneğin, köpeklerin (`Dogs`) boyutuna ve kürküne bağlı olarak farklı davranışlara sahip Köpekler sınıfınız varsa, iki hiyerarşiyi ortaya çıkarabilirsiniz:

- boyuta göre: Büyük (`Large`), Orta (`Medium`) ve Küçük (`Small`)

- kürkle: Pürüzsüz (`Smooth`) ve Shaggy (`Shaggy`)

Ve her şey iyi görünebilir, ancak hem büyük (`Large`) hem de Pürüzsüz  (`Smooth`) bir köpek yaratmanız gerektiğinde sorunların ortaya çıkması dışında, yalnızca bir sınıftan bir nesne oluşturabildiğiniz için. Bununla birlikte, Devralmak yerine Oluştur'u kullanarak bu sorunu önleyebilirsiniz (**Strategy** tasarım desenine bakın). Başka bir deyişle, Köpek (`Dog`) sınıfının beden ve kürk olmak üzere iki bileşen alanı olacaktır. Bu alanlara gerekli sınıflardan bileşen nesneleri ekleyeceksiniz. Böylece `LargeSize` ve `ShaggyFur` özelliklerine sahip bir Köpek  (`Dog`) yaratabilirsiniz.


### 🤯 Nasıl Refactor Edilir?

1. İlgilendiğiniz sınıftan yeni bir alt sınıf oluşturun.

2. Bir alt sınıftan nesneler oluşturmak için ek verilere ihtiyacınız varsa, bir kurucu oluşturun ve ona gerekli parametreleri ekleyin. Yapıcının ana uygulamasını çağırmayı unutmayın.

3. Ana sınıfın yapıcısına yapılan tüm çağrıları bulun. Bir alt sınıfın işlevselliği gerekli olduğunda ana kurucuyu alt sınıf kurucuyla değiştirin.

4. Gerekli yöntemleri ve alanları ana sınıftan alt sınıfa taşıyın. Bunu **Push Down Method** ve **Push Down Method** teknikleri aracılığıyla yapın. Önce yöntemleri taşıyarak başlamak daha kolaydır. Bu şekilde alanlar tüm süreç boyunca erişilebilir kalır: taşıma öncesinde ana sınıftan ve taşıma tamamlandıktan sonra alt sınıfın kendisinden.

5. Alt sınıf hazır olduktan sonra, işlevsellik seçimini kontrol eden tüm eski alanları bulun. Alanların kullanıldığı tüm operatörleri değiştirmek için polimorfizmi kullanarak bu alanları silin. Basit bir örnek: Araba (`Car`) sınıfında, `isElectricCar` alanınız vardı ve buna bağlı olarak, `refuel()` yönteminde araca ya gaz dolduruluyor ya da elektrik yükleniyor. Yeniden düzenleme sonrasında `isElectricCar` alanı kaldırılır ve `Car` ve `ElectricCar` sınıfları, `refuel()` yönteminin kendi uygulamalarına sahip olur.


## Extract Superclass

### 🙁 Problem

Ortak alanları ve yöntemleri olan iki sınıfınız var.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Superclass%20-%20Before.png)
</div>

### 😊 Çözüm

Onlar için paylaşılan bir üst sınıf oluşturun ve tüm aynı alanları ve yöntemleri ona taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Superclass%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir tür kod çoğaltması, iki sınıf benzer görevleri aynı şekilde gerçekleştirdiğinde veya benzer görevleri farklı şekillerde gerçekleştirdiğinde ortaya çıkar. Nesneler bu tür durumları kalıtım yoluyla basitleştirmek için yerleşik bir mekanizma sunar. Ancak çoğu zaman bu benzerlik, sınıflar oluşturulana kadar fark edilmeden kalır ve daha sonra bir miras yapısının oluşturulmasını gerektirir.

### ✅ Avantajları

Kod tekilleştirme. Ortak alanlar ve yöntemler artık yalnızca tek bir yerde "yaşıyor"

### 🖐🏼 Ne Zaman Kullanılmamalı?

Bu tekniği halihazırda bir üst sınıfa sahip olan sınıflara uygulayamazsınız.

### 🤯 Nasıl Refactor Edilir?

1. Soyut bir üst sınıf oluşturun.

2. Ortak işlevselliği bir üst sınıfa taşımak için **Pull Up Field**, **Pull Up Method** ve **Pull Up Constructor Body** kullanın. Alanlarla başlayın, çünkü ortak alanlara ek olarak ortak yöntemlerde kullanılan alanları da taşımanız gerekecektir.

3. İstemci kodunda, alt sınıfların kullanımının yeni sınıfınızla değiştirilebileceği yerleri arayın (örneğin tür bildirimlerinde).


## Extract Interface

### 🙁 Problem

Birden fazla istemci bir sınıf arayüzünün aynı bölümünü kullanıyor. Başka bir durum: iki sınıftaki arayüzün bir kısmı aynıdır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Interface%20-%20Before.png)
</div>

### 😊 Çözüm

Bu özdeş kısmı kendi arayüzüne taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Interface%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

1. Sınıflar farklı durumlarda özel roller oynadığında arayüzler çok uygundur. Hangi rolü açıkça belirtmek için **Extract Interface** kullanın.

2. Bir sınıfın sunucusunda gerçekleştirdiği işlemleri tanımlamanız gerektiğinde başka bir uygun durum ortaya çıkar. Sonunda birden fazla türdeki sunucuların kullanımına izin verilmesi planlanıyorsa tüm sunucuların arayüzü uygulaması gerekir.

### 🤓 Bilinmesinde Yarar Var

**Extract Superclass** ve **Extract Interface** teknikleri arasında belirli bir benzerlik vardır.

Bir arayüzün çıkarılması, ortak kodun değil, yalnızca ortak arayüzlerin yalıtılmasına olanak tanır. Başka bir deyişle, eğer sınıflar **Duplicate Code** içeriyorsa, arayüzün çıkarılması tekilleştirmenize yardımcı olmayacaktır.

Yine de, çoğaltmayı içeren davranışı ayrı bir bileşene taşımak ve tüm işi ona devretmek için **Extract Class** tekniğini uygulanarak bu sorun azaltılabilir. Ortak davranışın boyutu büyükse, her zaman **Extract Superclass** tekniğini kullanabilirsiniz. Bu elbette daha da kolaydır, ancak bu yolu seçerseniz yalnızca bir ebeveyn sınıfı alacağınızı unutmayın.

### 🤯 Nasıl Refactor Edilir?

1. Boş bir arayüz oluşturun.

2. Arayüzdeki ortak işlemleri bildirin.

3. Arayüzü uygulayan olarak gerekli sınıfları bildirin.

4. Yeni arayüzü kullanmak için istemci kodundaki tür bildirimlerini değiştirin.


## Collapse Hierarchy

### 🙁 Problem

Bir alt sınıfın neredeyse üst sınıfıyla aynı olduğu bir sınıf hiyerarşiniz var.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Collapse%20Hierarchy%20-%20Before.png)
</div>

### 😊 Çözüm

Alt sınıf ve üst sınıfı birleştirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Collapse%20Hierarchy%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Programınız zamanla büyüdü ve alt sınıf ile üst sınıf neredeyse aynı hale geldi. Bir alt sınıftan bir özellik kaldırıldı, bir yöntem üst sınıfa taşındı... ve artık iki benzer sınıfınız var.

### ✅ Avantajları

- Program karmaşıklığı azalır. Daha az sınıf, aklınızda tutmanız gereken daha az şey ve gelecekteki kod değişiklikleri sırasında endişelenecek daha az kırılabilir hareketli parça anlamına gelir.

- Yöntemler bir sınıfta erken tanımlandığında kodunuzda gezinmek daha kolaydır. Belirli bir yöntemi bulmak için tüm hiyerarşiyi taramanıza gerek yoktur.

### 🖐🏼 Ne Zaman Kullanılmamalı?

- Yeniden düzenlediğiniz sınıf hiyerarşisinde birden fazla alt sınıf var mı? Eğer öyleyse, yeniden düzenleme tamamlandıktan sonra geri kalan alt sınıflar, hiyerarşinin çöktüğü sınıfın mirasçıları haline gelmelidir.

- Ancak bunun Liskov ikame ilkesinin ihlaline yol açabileceğini unutmayın. Örneğin, programınız şehir içi ulaşım ağlarını taklit ediyorsa ve Taşıma (`Transport`) üst sınıfını yanlışlıkla Araba (`Car`) alt sınıfına daraltırsanız, Plane (`Uçak`) sınıfı Araba'nın (`Car`) mirasçısı olabilir. Hata!

### 🤯 Nasıl Refactor Edilir?

1. Hangi sınıfın kaldırılmasının daha kolay olduğunu seçin: üst sınıf mı yoksa alt sınıf mı?

2. Alt sınıftan kurtulmaya karar verirseniz **Pull Up Field** ve **Pull Up Method** tekniklerini kullanın. Üst sınıfı ortadan kaldırmayı seçerseniz, **Push Down Field** ve **Push Down Method** tekniklerini tercih edin.

3. Sildiğiniz sınıfın tüm kullanımlarını alanların ve yöntemlerin taşınacağı sınıfla değiştirin. Genellikle bu, sınıf oluşturmaya yönelik kod, değişken ve parametre yazımı ve kod yorumlarındaki belgeler olacaktır.

4. Boş sınıfı silin.


## Form Template Method

### 🙁 Problem

Alt sınıflarınız, benzer adımları aynı sırayla içeren algoritmalar uygular.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Form%20Template%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Algoritma yapısını ve aynı adımları bir üst sınıfa taşıyın ve farklı adımların uygulanmasını alt sınıflarda bırakın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Form%20Template%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Alt sınıflar paralel olarak, bazen farklı kişiler tarafından geliştirilir, bu da her değişikliğin tüm alt sınıflarda yapılması gerektiğinden kod çoğaltılmasına, hatalara ve kod bakımında zorluklara yol açar.

### ✅ Avantajları

- Kod çoğaltma her zaman basit kopyalama/yapıştırma durumlarını ifade etmez. Çoğunlukla çoğaltma daha yüksek düzeyde gerçekleşir; örneğin sayıları sıralamak için bir yönteminiz ve yalnızca öğelerin karşılaştırılmasıyla farklılaşan nesne koleksiyonlarını sıralamak için bir yönteminiz olduğunda. Bir şablon yöntemi oluşturmak, paylaşılan algoritma adımlarını bir üst sınıfta birleştirerek ve yalnızca alt sınıflardaki farklılıkları bırakarak bu çoğaltmayı ortadan kaldırır.

- Bir şablon yöntemi oluşturmak, Açık/Kapalı Prensibinin uygulamalı bir örneğidir. Yeni bir algoritma sürümü göründüğünde yalnızca yeni bir alt sınıf oluşturmanız gerekir; Mevcut kodda herhangi bir değişiklik yapılmasına gerek yoktur.

### 🤯 Nasıl Refactor Edilir?

1. Alt sınıflardaki algoritmaları, ayrı yöntemlerde açıklanan kurucu parçalarına ayırın. **Extract Method** tekniği bu konuda yardımcı olabilir.

2. Sonuçta ortaya çıkan tüm alt sınıflar için aynı olan yöntemler, **Pull Up Method** tekniği kullanılarak bir üst sınıfa taşınabilir.

3. Benzer olmayan yöntemlere **Rename Method** tekniği ile tutarlı adlar verilebilir.

4. Benzer olmayan yöntemlerin imzalarını **Pull Up Method** tekniği kullanarak soyut olarak bir üst sınıfa taşıyın. Uygulamalarını alt sınıflarda bırakın.

5. Ve son olarak, algoritmanın ana yöntemini üst sınıfa çekin. Artık hem gerçek hem de soyut olarak üst sınıfta açıklanan yöntem adımlarıyla çalışmalıdır.

## Replace Inheritance with Delegation

### 🙁 Problem

Üst sınıfının yöntemlerinin yalnızca bir kısmını kullanan bir alt sınıfınız var (veya üst sınıf verilerini miras almak mümkün değil).

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Inheritance%20with%20Delegation%20-%20Before.png)
</div>

### 😊 Çözüm

Bir alan oluşturun ve içine bir üst sınıf nesnesi koyun, yöntemleri üst sınıf nesnesine devredin ve mirastan kurtulun.

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

1. Üst sınıfı tutmak için alt sınıfta bir alan oluşturun. İlk aşamada mevcut nesneyi içine yerleştirin.

2. Alt sınıf yöntemlerini,`this` yerine üst sınıf nesnesini kullanacak şekilde değiştirin.

3. İstemci kodunda çağrılan üst sınıftan miras alınan yöntemler için, alt sınıfta basit yetki verme yöntemleri oluşturun.

4. Miras bildirimini alt sınıftan kaldırın.

5. Yeni bir nesne oluşturarak, eski üst sınıfın depolandığı alanın başlatma kodunu değiştirin.

## Replace Delegation with Inheritance

### 🙁 Problem

Bir sınıf, başka bir sınıfın tüm yöntemlerine yetki veren birçok basit yöntem içerir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Delegation%20with%20Inheritance%20-%20Before.png)
</div>

### 😊 Çözüm

Sınıfı bir temsilci mirasçısı yapın, bu da yetki verme yöntemlerini gereksiz kılar.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Delegation%20with%20Inheritance%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Yetki devri, mirasa göre daha esnek bir yaklaşımdır çünkü yetki devrinin nasıl uygulandığını değiştirmeye ve diğer sınıfları da buraya yerleştirmeye olanak tanır. Bununla birlikte, eylemleri yalnızca bir sınıfa ve onun tüm genel yöntemlerine devrederseniz, yetki vermenin yararlılığı sona erer.

Böyle bir durumda, delegasyonu kalıtımla değiştirirseniz, sınıfı çok sayıda delege etme yönteminden temizlersiniz ve her yeni temsilci sınıf yöntemi için bunları oluşturma zorunluluğundan kurtulursunuz.

### ✅ Avantajları

Kod uzunluğunu azaltır. Tüm bu yetki verme yöntemlerine artık gerek yok.

### 🖐🏼 Ne Zaman Kullanılmamalı?

- Sınıf, temsilci sınıfının genel yöntemlerinin yalnızca bir kısmına yetki içeriyorsa bu tekniği kullanmayın. Bunu yaparak Liskov ikame ilkesini ihlal etmiş olursunuz.

- Bu teknik yalnızca sınıfın hala ebeveynleri yoksa kullanılabilir.


### 🤯 Nasıl Refactor Edilir?

1. Sınıfı, temsilci sınıfının bir alt sınıfı yapın.

2. Geçerli nesneyi, temsilci nesnesine referans içeren bir alana yerleştirin.

3. Basit delegasyona sahip yöntemleri birer birer silin. Adları farklıysa tüm yöntemlere tek bir ad vermek için **Rename Method** tekniğini kullanın.

4. Temsilci alanına yapılan tüm referansları geçerli nesneye yapılan referanslarla değiştirin.

5. Temsilci alanını kaldırın.

