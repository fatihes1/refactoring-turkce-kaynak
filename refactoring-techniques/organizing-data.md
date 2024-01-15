
# Verileri Düzenleme (Organizing Data)

Bu refactoring teknikleri, ilkelleri zengin sınıf işlevselliğiyle değiştirerek veri işlemeye yardımcı olur.

Bir diğer önemli sonuç, sınıfları daha taşınabilir ve yeniden kullanılabilir hale getiren sınıf ilişkilerinin çözülmesidir.

## Self Encapsulate Field

Self Encapsulate Field tekniği, sıradan Encapsulate Field tekniğinden farklıdır: bu başlık altında verilen yeniden düzenleme tekniği private bir alanda gerçekleştirilir.


### 🙁 Problem

Bir sınıf içindeki private alanlara doğrudan erişimi kullanırsınız.

```java
class Range {
  private int low, high;
  boolean includes(int arg) {
    return arg >= low && arg <= high;
  }
}
```

### 😊 Çözüm

Alan (field) için bir alıcı (getter) ve ayarlayıcı (setter) oluşturun ve alana erişmek için yalnızca bunları kullanın.

```java
class Range {
  private int low, high;
  boolean includes(int arg) {
    return arg >= getLow() && arg <= getHigh();
  }
  int getLow() {
    return low;
  }
  int getHigh() {
    return high;
  }
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bazen bir sınıfın içindeki private bir alana doğrudan erişim yeterince esnek olmayabilir. İlk sorgu yapıldığında bir alan değeri başlatabilmek veya alanın yeni değerleri atandığında bunlar üzerinde belirli işlemler yapabilmek, hatta tüm bunları alt sınıflarda çeşitli şekillerde yapabilmek istiyorsanız doğrudan erişim size sorun çıkaracaktır.

### ✅ Avantajları

- Alanlara dolaylı erişim, bir alan üzerinde erişim yöntemleri (alıcılar - getter ve ayarlayıcılar- setter) aracılığıyla işlem yapılmasıdır. Bu yaklaşım alanlara doğrudan erişimden çok daha esnektir.
	- İlk olarak, alandaki veriler ayarlandığında veya alındığında karmaşık işlemleri gerçekleştirebilirsiniz. Alan değerlerinin tembel başlatılması ve doğrulanması, alan alıcıları ve ayarlayıcıları içinde kolayca uygulanır.
	- İkinci ve daha önemlisi, alt sınıflardaki alıcıları ve ayarlayıcıları yeniden tanımlayabilirsiniz.
- Bir alan için hiçbir ayarlayıcı uygulamama seçeneğiniz vardır. Alan değeri yalnızca yapıcıda belirtilecektir, böylece alan tüm nesne ömrü boyunca değiştirilemez hale gelecektir.

### 🚫 Dezavantajları

Alanlara doğrudan erişim kullanıldığında, esneklik azalsa da kod daha basit ve daha sunulabilir görünür.

### 🤯 Nasıl Refactor Edilir?

1. Alan için bir alıcı (getter) (ve isteğe bağlı olarak ayarlayıcı - setter) oluşturun. Bu işlevler `protected` ya da `public` olmalıdır.
2. Alanın tüm doğrudan çağrılarını bulun ve bunları alıcı (getter) ve ayarlayıcı (setter) çağrılarla değiştirin.

## Replace Data Value with Object

### 🙁 Problem

Bir sınıf (veya sınıflar grubu) bir veri alanı içerir. Alanın kendi davranışı ve ilgili verileri vardır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Data%20Value%20with%20Object%20-%20Before.png)
</div>

### 😊 Çözüm

Yeni bir sınıf oluşturun, eski alanı ve davranışını sınıfa yerleştirin ve sınıfın nesnesini orijinal sınıfta saklayın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Data%20Value%20with%20Object%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?


Bu yeniden düzenleme temelde **Extract Class** tekniğinin özel bir durumudur. Onu farklı kılan, yeniden düzenlemenin nedenidir.

**Extract Class**'ta farklı şeylerden sorumlu tek bir sınıfımız var ve onun sorumluluklarını bölmek istiyoruz.

Bir veri değerinin bir nesneyle değiştirilmesiyle, programın büyümesi nedeniyle artık o kadar basit olmayan ve artık ilişkili veriler ve davranışlara sahip olan ilkel bir alana (sayı, dize vb.) sahip oluyoruz. Bir yandan, bu alanların kendi başlarına korkutucu hiçbir yanı yok. Bununla birlikte, bu alanlar ve davranışlar ailesi aynı anda birden fazla sınıfta mevcut olabilir ve kopya kod oluşturabilir.

Dolayısıyla tüm bunlar için yeni bir sınıf oluşturup hem alanı hem de ilgili veri ve davranışları ona taşıyoruz.

### ✅ Avantajları

Sınıflar arası ilişkiyi geliştirir. Veriler ve ilgili davranışlar tek bir sınıfın içindedir.

### 🤯 Nasıl Refactor Edilir?

Yeniden düzenlemeye başlamadan önce sınıf içinden alana doğrudan referanslar olup olmadığına bakın. Öyleyse, orijinal sınıfta gizlemek için **Self Encapsulate Field** tekniğini kullanın.

1. Yeni bir sınıf oluşturun ve alanınızı ve ilgili alıcıyı (getter) ona kopyalayın. Ayrıca alanın basit değerini kabul eden bir kurucu (constructor) oluşturun. Orijinal sınıfa gönderilen her yeni alan değeri yeni bir değer nesnesi oluşturacağından bu sınıfın bir ayarlayıcısı olmayacaktır.

2. Orijinal sınıfta alan türünü yeni sınıfla değiştirin.

3. Orijinal sınıftaki alıcıda, ilişkili nesnenin alıcısını (getter) çağırın.

4. Ayarlayıcıda yeni bir değer nesnesi oluşturun. Daha önce alan için başlangıç ​​değerleri ayarlanmışsa, yapıcıda (constructor) yeni bir nesne oluşturmanız da gerekebilir.

### 👣 Sonraki Adım

Bu refactoring tekniğini uyguladıktan sonra, nesneyi içeren alanda **Change Value to Reference** tekniği uygulamak akıllıca olacaktır. Bu, bir ve aynı değer için düzinelerce nesneyi depolamak yerine, bir değere karşılık gelen tek bir nesneye referansın saklanmasına olanak tanır.

Çoğu zaman bu yaklaşıma, bir nesnenin gerçek dünyadaki bir nesneden (kullanıcılar, siparişler, belgeler vb.) sorumlu olmasını istediğinizde ihtiyaç duyulur. Aynı zamanda bu yaklaşım tarih, para, aralık vb. nesneler için de kullanışlı olmayacaktır.

## Change Value to Reference

### 🙁 Problem

Tek bir nesneyle değiştirmeniz gereken, tek bir sınıfın birçok özdeş örneğine sahipsiniz.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Value%20to%20Reference%20-%20Before.png)
</div>

### 😊 Çözüm

Aynı nesneleri tek bir referans nesnesine dönüştürün.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Value%20to%20Reference%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Birçok sistemde nesneler değerler veya referanslar olarak sınıflandırılabilir.

- Referanslar: Gerçek dünyadaki bir nesnenin programdaki yalnızca bir nesneye karşılık gelmesidir. Referanslar genellikle kullanıcı/sipariş/ürün/vb. şeklindedir. nesneler.
- Değerler: Gerçek dünyadaki bir nesne, programdaki birden çok nesneye karşılık gelir. Bu nesneler tarihler, telefon numaraları, adresler, renkler ve benzeri olabilir.

Referans ve değer seçimi her zaman net değildir. Bazen az miktarda değişmeyen veri içeren basit bir değer vardır. Daha sonra nesneye her erişildiğinde değiştirilebilir veriler eklemek ve bu değişiklikleri iletmek gerekli hale gelir. Bu durumda onu referansa dönüştürmek gerekli hale gelir.

### ✅ Avantajları

Bir nesne, belirli bir varlık hakkındaki en güncel bilgilerin tümünü içerir. Programın bir bölümünde nesne değiştirilirse, bu değişikliklere programın nesneyi kullanan diğer bölümlerinden erişilebilir.

### 🚫 Dezavantajları

Referansların uygulanması çok daha zordur.

### 🤯 Nasıl Refactor Edilir?

1. Referansların oluşturulacağı sınıfta **Replace Constructor with Factory Method** tekniğini kullanın.

2. Referanslara erişim sağlamaktan hangi nesnenin sorumlu olacağını belirleyin. İhtiyaç duyduğunuzda yeni bir nesne oluşturmak yerine artık onu bir depolama nesnesinden veya statik sözlük alanından almanız gerekiyor.

3. Referansların önceden mi yoksa dinamik olarak mı oluşturulacağını gerektiği şekilde belirleyin. Nesneler önceden oluşturulmuşsa, bunları kullanmadan önce yüklediğinizden emin olun.

4. Bir referans döndürecek şekilde fabrika yöntemini değiştirin. Nesneler önceden oluşturulmuşsa, var olmayan bir nesne istendiğinde hataların nasıl ele alınacağına karar verin. Ayrıca, yöntemin yalnızca mevcut nesneleri döndürdüğünü bildirmek için **Rename Method** tekniğini kullanmanız gerekebilir.


## Change Reference to Value

### 🙁 Problem

Yaşam döngüsünü yönetmeye değmeyecek kadar küçük ve nadiren değiştirilen bir referans nesneniz var.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Reference%20to%20Value%20-%20Before.png)

</div>


### 😊 Çözüm

Onu bir değer nesnesine dönüştürün.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Reference%20to%20Value%20-%20Before.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir referanstan bir değere geçiş yapma fikri, referansla çalışmanın zorluğundan gelebilir. Referanslar sizin tarafınızdan yönetim gerektirir:

- Her zaman gerekli nesnenin depodan talep edilmesini gerektirirler.

- Bellekteki referanslarla çalışmak zahmetli olabilir.

- Referanslarla çalışmak, dağıtılmış ve paralel sistemlerde değerlerle karşılaştırıldığında özellikle zordur.

Değerler (values), kullanım süreleri boyunca durumları değişebilecek nesneler yerine, değiştirilemez nesnelere sahip olmayı tercih ettiğinizde özellikle kullanışlıdır.

### ✅ Avantajları

- Nesnelerin önemli bir özelliği de değiştirilemez olmalarıdır. Nesne değeri döndüren her sorgu için aynı sonucun alınması gerekir. Eğer bu doğruysa, aynı şeyi temsil eden birçok nesne varsa hiçbir sorun ortaya çıkmaz.

- Değerlerin uygulanması çok daha kolaydır.

### 🚫 Dezavantajları

Bir değer değiştirilebilirse, herhangi bir nesnenin değişmesi durumunda aynı varlığı temsil eden diğer tüm nesnelerdeki değerlerin güncellendiğinden emin olun. Bu o kadar külfetli bir durumdur ki, bu amaçla referans oluşturmak daha kolaydır.

### 🤯 Nasıl Refactor Edilir?

1. Nesneyi değiştirilemez hale getirin. Nesnenin durumunu ve verilerini değiştiren ayarlayıcılar (setters) veya başka yöntemler olmamalıdır (**Remove Setting Method ** tekniği burada yardımcı olabilir). Verilerin bir değer nesnesinin alanlarına atanması gereken tek yer bir yapıcıdır yani constructor.

2. İki değeri karşılaştırabilmek için bir karşılaştırma (comparison) yöntemi oluşturun.

3. Fabrika (factory) yöntemini silip silemeyeceğinizi ve nesne yapıcısını public hale getirip getiremeyeceğinizi kontrol edin.


## Replace Array with Object

### 🙁 Problem

Çeşitli veri türlerini içeren bir diziniz (array) var.

```java
String[] row = new String[2];
row[0] = "Liverpool";
row[1] = "15";
```

### 😊 Çözüm

Diziyi, her öğe için ayrı alanlara sahip olacak bir nesneyle (object) değiştirin.

```java
Performance row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```

### 🤔 Neden Refactoring Uygulanmalı?

Diziler, tek türdeki verileri ve koleksiyonları depolamak için mükemmel bir araçtır. Ancak posta kutuları gibi bir dizi kullanırsanız, kullanıcı adını 1. kutuya ve kullanıcının adresini 14. kutuya kaydederseniz, bir gün bunu yaptığınız için çok mutsuz olacaksınız. Bu yaklaşım, birisi bir şeyi yanlış "kutuya" koyduğunda feci sorunlara yol açar ve aynı zamanda hangi verinin nerede saklandığını bulmak için zamanınızı boşa harcarsınız.

### ✅ Avantajları

- Ortaya çıkan sınıfa, daha önce ana sınıfta veya başka bir yerde depolanan tüm ilişkili davranışları yerleştirebilirsiniz.

- Bir sınıfın alanlarını belgelemek, bir dizinin (array) öğelerini belgelemekten çok daha kolaydır.

### 🤯 Nasıl Refactor Edilir?

1. Dizideki verileri içerecek yeni sınıfı oluşturun. Dizinin kendisini public alan olarak sınıfa yerleştirin.

2. Bu sınıfın nesnesini orijinal sınıfta depolamak için bir alan oluşturun. Veri dizisini başlattığınız yerde nesnenin kendisini de oluşturmayı unutmayın.

3. Yeni sınıfta, dizi öğelerinin her biri için erişim yöntemlerini birer birer oluşturun. Onlara ne yaptıklarını belirten açıklayıcı isimler verin. Aynı zamanda, ana koddaki bir dizi öğesinin her kullanımını karşılık gelen erişim yöntemiyle değiştirin.

4. Tüm öğeler için erişim yöntemleri oluşturulduğunda diziyi private yapın.

5. Dizinin her öğesi için sınıfta private bir alan oluşturun ve ardından erişim yöntemlerini, dizi yerine bu alanı kullanacak şekilde değiştirin.

6. Tüm veriler taşındığında diziyi silin.

## Duplicate Observed Data

### 🙁 Problem

Etki alanı (domain data) verileri GUI'den sorumlu sınıflarda mı saklanıyor?

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Duplicate%20Observed%20Data%20-%20Before.png)

</div>

### 😊 Çözüm

Verileri ayrı sınıflara ayırarak etki alanı (domain) sınıfı ile GUI arasında bağlantı ve senkronizasyon sağlamak iyi bir fikirdir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Duplicate%20Observed%20Data%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Aynı veriler için birden fazla arayüz görünümüne sahip olmak istiyorsunuz (örneğin, hem masaüstü uygulamanız hem de mobil uygulamanız var). GUI'yi etki alanından ayırmayı başaramazsanız eğer, kod tekrarından ve çok sayıda hatadan kaçınmak çok zor olacaktır.

### ✅ Avantajları

- Sorumluluğu iş mantığı sınıfları ve sunum sınıfları arasında paylaştırırsınız (bkz. Tek Sorumluluk İlkesi - Single Responsibility Principle), bu da programınızı daha okunabilir ve anlaşılır kılar.

- Yeni bir arayüz görünümü eklemeniz gerekiyorsa yeni sunum (presentation) sınıfları oluşturun; iş mantığının koduna dokunmanıza gerek yoktur (bkz. Açık/Kapalı Prensibi - Open/Closed Principle).

- Artık iş mantığı ve kullanıcı arayüzleri üzerinde farklı kişiler çalışabilir.

### 🖐🏼 Ne Zaman Kullanılmamalı?

- Klasik biçiminde Observer şablonu kullanılarak gerçekleştirilen bu refactoring tekniği, web sunucusuna yapılan sorgular arasında tüm sınıfların yeniden oluşturulduğu web uygulamaları için geçerli değildir.

- Yine de, iş mantığını ayrı sınıflara ayırma genel ilkesi web uygulamaları için de haklı gösterilebilir. Ancak bu, sisteminizin nasıl tasarlandığına bağlı olarak farklı refactoring teknikleri kullanılarak uygulanacaktır.

### 🤯 Nasıl Refactor Edilir?

1. GUI sınıfındaki etki alanı verilerine doğrudan erişimi gizleyin. Bunun için Self Encapsulate Field tekniğini kullanmak en iyisidir. Böylece bu veriler için alıcıları ve ayarlayıcıları yaratırsınız.

2. GUI sınıfı olaylarına yönelik işleyicilerde, yeni alan değerlerini ayarlamak için ayarlayıcıları kullanın. Bu, bu değerleri ilişkili etki alanı nesnesine aktarmanıza olanak tanır.

3. Bir etki alanı (domain) sınıfı oluşturun ve gerekli alanları GUI sınıfından ona kopyalayın. Tüm bu alanlar için alıcılar (getter) ve ayarlayıcılar (setter) oluşturun.

4. Bu iki sınıf için bir Gözlemci modeli oluşturun:
	- Etki alanı sınıfında, gözlemci nesnelerini (GUI nesneleri) depolamak için bir dizinin yanı sıra bunları kaydetme, silme ve bildirme yöntemleri oluşturun.
	- GUI sınıfında, nesnedeki değişikliklere tepki verecek ve GUI sınıfındaki alanların değerlerini güncelleyecek `update()` yönteminin yanı sıra etki alanı sınıfına yapılan referansları depolamak için bir alan oluşturun. Özyinelemeyi (recursion) önlemek için değer güncellemelerinin doğrudan yöntemde oluşturulması gerektiğini unutmayın.
	- GUI sınıfı yapıcısında (constructor), etki alanı sınıfının bir örneğini oluşturun ve bunu oluşturduğunuz alana kaydedin. GUI nesnesini etki alanı nesnesine gözlemci olarak kaydedin.
	- Etki alanı sınıfı alanlarına ilişkin ayarlayıcılarda (setters), yeni değerleri GUI'ye iletmek için gözlemciyi bilgilendirme yöntemini (başka bir deyişle, GUI sınıfında güncelleme yöntemini) çağırın.
	- GUI sınıfı alanlarının ayarlayıcılarını, yeni değerleri doğrudan etki alanı nesnesinde ayarlayacak şekilde değiştirin. Değerlerin bir etki alanı sınıfı ayarlayıcısı (setter) aracılığıyla ayarlanmadığından emin olun; aksi takdirde sonsuz yineleme ortaya çıkar.


## Change Unidirectional Association to Bidirectional

### 🙁 Problem

Her birinin diğerinin özelliklerini kullanması gereken iki sınıfınız var, ancak aralarındaki ilişki yalnızca tek yönlü.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Unidirectional%20Association%20to%20Bidirectional%20-%20Before.png)

</div>

### 😊 Çözüm

Eksik ilişkilendirmeyi ihtiyaç duyan sınıfa ekleyin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Unidirectional%20Association%20to%20Bidirectional%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?


Başlangıçta sınıfların tek yönlü bir ilişkisi vardı. Ancak zamanla müşteri kodunun ilişkinin her iki tarafına da erişmesi gerektiği durumlarda refactoring uygulanmalıdır.

### ✅ Avantajları

Bir sınıfın ters ilişkilendirmeye ihtiyacı varsa bunu kolayca hesaplayabilirsiniz. Ancak bu hesaplamalar karmaşıksa ters ilişkiyi sürdürmek daha iyidir.

### 🚫 Dezavantajları

- Çift yönlü ilişkilerin uygulanması ve sürdürülmesi, tek yönlü olanlara göre çok daha zordur.

- Çift yönlü ilişkiler sınıfları birbirine bağımlı hale getirir. Tek yönlü ilişkilendirme ile biri diğerinden bağımsız olarak kullanılabilir.

### 🤯 Nasıl Refactor Edilir?

1. Ters ilişkilendirmeyi tutmak için bir alan (field) ekleyin.

2. Hangi sınıfın baskın yani dominant olacağına karar verin. Bu sınıf, öğeler eklendikçe veya değiştikçe ilişkilendirmeyi oluşturan veya güncelleyen, kendi sınıfında ilişkilendirmeyi kuran ve ilişkili nesnede ilişkilendirmeyi kurmak için yardımcı yöntemleri çağıran yöntemleri içerecektir.

3. Baskın olmayan yani non-dominant sınıfta ilişkilendirmeyi kurmak için bir yardımcı yöntem oluşturun. Yöntem, alanı tamamlamak için parametrelerde verilenleri kullanmalıdır. Daha sonra başka amaçlarla kullanılmaması için yönteme açık bir ad verin.

4. Tek yönlü ilişkilendirmeyi kontrol etmeye yönelik eski yöntemler baskın sınıftaysa, bunları ilgili nesneden yardımcı yöntemlere yapılan çağrılarla tamamlayın.

5. İlişkilendirmeyi kontrol etmeye yönelik eski yöntemler baskın olmayan sınıftaysa, baskın sınıfta yöntemleri oluşturun, onları çağırın ve yürütmeyi onlara devredin.

## Change Bidirectional Association to Unidirectional

### 🙁 Problem

Sınıflar arasında çift yönlü bir ilişkiniz var ancak sınıflardan biri diğerinin özelliklerini kullanmıyor.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Bidirectional%20Association%20to%20Unidirectional%20-%20Before.png)

</div>


### 😊 Çözüm

Yerel değişkenlerin sınıfın alanları haline gelmesi için yöntemi ayrı bir sınıfa dönüştürün. Daha sonra yöntemi aynı sınıf içindeki birkaç yönteme bölebilirsiniz.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Change%20Bidirectional%20Association%20to%20Unidirectional%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Çift yönlü bir ilişkilendirmenin sürdürülmesi genellikle tek yönlü olandan daha zordur ve ilgili nesnelerin düzgün bir şekilde oluşturulması ve silinmesi için ek kod gerektirir. Bu, programı daha karmaşık hale getirir.

Buna ek olarak, yanlış uygulanan çift yönlü bir ilişkilendirme, çöp toplama konusunda sorunlara neden olabilir (bu da kullanılmayan nesneler nedeniyle hafızanın şişmesine yol açar).

Örnek: çöp toplayıcı (garbage collector), artık başka nesneler tarafından başvurulmayan nesneleri bellekten kaldırır. Diyelim ki bir Kullanıcı-Siparişi (`User` - `Order`) nesne çifti oluşturuldu, kullanıldı ve sonra bir kenarda öylece bırakıldı. Ancak bu nesneler hâlâ birbirlerine referans verdikleri için hafızadan silinmeyecektir. Bununla birlikte, artık kullanılmayan nesne referanslarını otomatik olarak tanımlayan ve bunları bellekten kaldıran programlama dillerindeki ilerlemeler sayesinde bu sorunun önemi azalıyor.

Sınıflar arasında karşılıklı bağımlılık sorunu da var. Çift yönlü bir ilişkide, iki sınıfın birbirini bilmesi gerekir; bu da ayrı ayrı kullanılamayacakları anlamına gelir. Bu ilişkilerin birçoğu mevcutsa, programın farklı bölümleri birbirine aşırı bağımlı hale gelir. Bundan dolayı bir bileşendeki herhangi bir değişiklik diğer bileşenleri etkileyebilir.

### ✅ Avantajları

- İlişkiye ihtiyaç duymayan sınıfı basitleştirir. Daha az kod, daha az kod bakımı anlamına gelir.

- Sınıflar arasındaki bağımlılığı azaltır. Bir sınıfta yapılan herhangi bir değişiklik yalnızca o sınıfı etkilediğinden bağımsız sınıfların bakımı daha kolaydır.


### 🤯 Nasıl Refactor Edilir?

1. Sınıflarınız için aşağıdakilerden birinin doğru olduğundan emin olun:

	- Hiçbir ilişkilendirme kullanılmaz.

	- İlişkili nesneyi almanın, örneğin bir veritabanı sorgusu yoluyla başka bir yolu vardır.

	- İlişkili nesne, onu kullanan yöntemlere argüman olarak iletilebilir.

2. Durumunuza bağlı olarak, başka bir nesneyle ilişki içeren bir alanın kullanımı, nesneyi farklı bir şekilde elde etmek için bir parametre veya yöntem çağrısıyla değiştirilmelidir.

3. İlgili nesneyi alana atayan kodu silin.

4. Artık kullanılmayan alanı silin.

## Replace Magic Number with Symbolic Constant

### 🙁 Problem

Kodunuz, kendisi için belirli bir anlamı olan ancak kodu okuyan kişi tarafından anlamlandırılamayan bir sayı kullanıyor.

```java
double potentialEnergy(double mass, double height) {
  return mass * height * 9.81;
}
```

### 😊 Çözüm

Bu sayıyı, sayının anlamını açıklayan, insan tarafından okunabilen bir ada sahip bir sabitle değiştirin.

```java
static final double GRAVITATIONAL_CONSTANT = 9.81;

double potentialEnergy(double mass, double height) {
  return mass * height * GRAVITATIONAL_CONSTANT;
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Magic sayı, kaynakta karşılaşılan ancak açık bir anlamı olmayan sayısal bir değerdir. Bu "anti-pattern" programın anlaşılmasını ve kodun yeniden düzenlenmesini zorlaştırır.

Ancak bu magic sayıyı değiştirmeniz gerektiğinde daha fazla zorluk ortaya çıkar. Bul ve değiştir bu durumda işe yaramaz: aynı numara farklı yerlerde farklı amaçlar için kullanılabilir. Bu durum, bu numarayı kullanan her kod satırını doğrulamanız gerekeceği anlamına gelir.

### ✅ Avantajları

- Sembolik sabit, değerinin anlamının herkes tarafından anlaşılabilecek şekilde belgelenmesi olarak hizmet edebilir.

- Bir sabitin değerini değiştirmek, başka bir yerde farklı bir amaç için kullanılan aynı sayıyı yanlışlıkla değiştirme riski olmadan, bu sayıyı tüm kod tabanı boyunca aramaktan çok daha kolaydır.

- Kodda bir sayının veya dizenin yinelenen kullanımını azaltın. Değer karmaşık ve uzun olduğunda (`3.14159` veya `0xCAFEBABE` gibi) bu özellikle önemlidir.

### 🤓 Bilinmesinde Yarar Var

**Tüm sayılar sihirli (magical) değildir.**

Bir sayının amacı açıksa onu değiştirmeye gerek yoktur. Klasik bir örnek:

```java
for (i = 0; i < сount; i++) { ... }
```

**Alternatifler**

1. Bazen sihirli bir sayı yöntem çağrılarıyla değiştirilebilir. Örneğin, bir koleksiyondaki öğelerin sayısını belirten sihirli bir numaranız varsa, bunu koleksiyonun son öğesini kontrol etmek için kullanmanıza gerek yoktur. Bunun yerine koleksiyon uzunluğunu elde etmek için standart yöntemi kullanın.
2. Sihirli sayılar bazen tür kodu olarak kullanılır. Diyelim ki iki tür kullanıcınız var ve bir sınıfta hangisinin hangisi olduğunu belirtmek için bir sayı alanı kullandığınızı varsayalım: yöneticiler `1` ve sıradan kullanıcılar `2`'dir.
Bu durumda, tür kodundan kaçınmak için yeniden düzenleme yöntemlerinden birini kullanmalısınız:
	- **Replace Type Code with Class**
	- **Replace Type Code with Subclasses**
	- **Replace Type Code with State/Strategy**

### 🤯 Nasıl Refactor Edilir?

1. Bir sabit bildirin ve ona sihirli sayının değerini atayın.

2. Sihirli sayının kullanıldığı tüm alanları bulun.

3. Bulduğunuz sayıların her biri için, bu özel durumdaki sihirli sayının sabitin amacına uygun olup olmadığını bir kez daha kontrol edin. Cevabınız evet ise, sayıyı sabitinizle değiştirin. Bu önemli bir adımdır, çünkü aynı sayı tamamen farklı anlamlara gelebilir (ve duruma göre farklı sabitlerle değiştirilebilir).


## Encapsulate Field

### 🙁 Problem

Public bir alanınız (field) var.

```java
class Person {
  public String name;
}
```

### 😊 Çözüm

Alanı (field) private yapın ve bunun için erişim yöntemleri oluşturun.

```java
class Person {
  private String name;

  public String getName() {
    return name;
  }
  public void setName(String arg) {
    name = arg;
  }
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Nesne yönelimli programlamanın temel direklerinden biri, nesne verilerini gizleme yeteneği olan kapsüllemedir (Encapsulation). Aksi takdirde, tüm nesneler herkese açık yani public hale gelir ve diğer nesneler, herhangi bir kontrol ve denge olmaksızın nesnenizin verilerini alıp değiştirebilir! Durum kontrolden çıkmadan bir el atmak iyi olacaktır.  Veriler, bu verilerle ilişkili davranışlardan ayrılır, program bölümlerinin modülerliği tehlikeye girer ve bakım karmaşık hale gelir.

### ✅ Avantajları

- Bir bileşenin verileri ve davranışı birbiriyle yakından ilişkiliyse ve kodda aynı yerde bulunuyorsa bu bileşenin bakımını ve geliştirmesini yapmanız çok daha kolay olur.

- Ayrıca nesne alanlarına erişimle ilgili karmaşık işlemleri de gerçekleştirebilirsiniz.

### 🖐🏼 Ne Zaman Kullanılmamalı?

Bazı durumlarda, performans hususları nedeniyle kapsülleme önerilmez. Bu durumlar nadirdir ancak gerçekleştiğinde bu durum çok önemlidir.

Diyelim ki x ve y koordinatlarına sahip nesneleri içeren bir grafik düzenleyiciniz var. Bu alanların gelecekte değişmesi muhtemel değildir. Üstelik program, bu alanların bulunduğu pek çok farklı nesneyi de içeriyor. Dolayısıyla koordinat alanlarına doğrudan erişim, aksi takdirde erişim yöntemlerinin çağrılmasıyla harcanacak önemli CPU döngülerinden tasarruf sağlar.

Bu alışılmadık duruma örnek olarak Java'daki **Point** sınıfı verilebilir. Bu sınıfın tüm alanları herkese açıktır.

### 🤯 Nasıl Refactor Edilir?

1. Alan için bir alıcı (getter) ve ayarlayıcı (setter) oluşturun.

2. Alanın tüm çağrılarını bulun. Alan değerinin alınmasını alıcıyla (getter) değiştirin ve yeni alan değerlerinin ayarını ayarlayıcıyla (setter) değiştirin.

3. Tüm alan çağrıları değiştirildikten sonra alanı (field) private yapın.

### 👣 Sonraki Adım

**Encapsulate Field**, verileri ve bu verileri içeren davranışları birbirine yakınlaştırmanın yalnızca ilk adımıdır. Erişim alanlarına yönelik basit yöntemler oluşturduktan sonra bu yöntemlerin çağrıldığı yerleri tekrar kontrol etmelisiniz. Bu alanlardaki kodun erişim yöntemlerinde daha uygun görünmesi oldukça olasıdır.


## Encapsulate Collection

### 🙁 Problem

Bir sınıf, bir koleksiyon alanı ve koleksiyonla çalışmak için basit bir alıcı ve ayarlayıcı içerir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Encapsulate%20Collection%20-%20Before.png)

</div>

### 😊 Çözüm

Alıcının (getter) döndürdüğü değeri salt okunur (read-only) yapın ve koleksiyonun öğelerini eklemek/silmek için yöntemler oluşturun.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Encapsulate%20Collection%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir sınıf, nesnelerin koleksiyonunu içeren bir alan içerir. Bu koleksiyon bir dizi, liste, küme veya vektör olabilir. Koleksiyonla çalışmak için normal bir alıcı (getter) ve ayarlayıcı (setter) oluşturuldu.

Ancak koleksiyonların, diğer veri türlerinin kullandığından biraz farklı bir protokol tarafından kullanılması gerekir. Getter yöntemi, koleksiyon nesnesinin kendisini döndürmemelidir; çünkü bu, istemcilerin, sahip sınıfının bilgisi olmadan koleksiyon içeriğini değiştirmesine olanak tanır. Ayrıca bu, istemcilere nesne verilerinin iç yapılarının çoğunu gösterecektir. Koleksiyon öğelerini alma yöntemi, koleksiyonun değiştirilmesine veya yapısıyla ilgili aşırı verinin ifşa edilmesine izin vermeyen bir değer döndürmelidir.

Ayrıca koleksiyona değer atayan bir yöntem olmamalıdır. Bunun yerine eleman ekleme ve silme işlemleri olmalıdır. Bu sayede nesne sahibi, koleksiyon öğelerinin eklenmesi ve silinmesi üzerinde kontrol sahibi olur.

Böyle bir protokol, bir koleksiyonu uygun şekilde kapsüller ve sonuçta sahip sınıfı ile müşteri kodu arasındaki ilişkinin derecesini azaltır.

### ✅ Avantajları

- Toplama alanı bir sınıfın içinde kapsüllenmiştir. Alıcı çağrıldığında koleksiyonun bir kopyasını döndürür; bu, koleksiyonu içeren sınıfın bilgisi olmadan koleksiyon öğelerinin yanlışlıkla değiştirilmesini veya üzerine yazılmasını önler.

- Koleksiyon öğeleri dizi gibi temel bir türün içinde yer alıyorsa koleksiyonla çalışmak için daha uygun yöntemler oluşturursunuz.

- Koleksiyon öğeleri ilkel olmayan bir kap (standart koleksiyon sınıfı) içinde yer alıyorsa, koleksiyonu kapsülleyerek koleksiyonun istenmeyen standart yöntemlerine erişimi kısıtlayabilirsiniz (örneğin, yeni öğelerin eklenmesini kısıtlayarak).

### 🤯 Nasıl Refactor Edilir?

1. Koleksiyon öğelerini eklemek ve silmek için yöntemler oluşturun. Parametrelerinde koleksiyon öğelerini kabul etmeleri gerekir.

2. Sınıf yapıcısında (constructor) bu yapılmazsa, alana başlangıç ​​değeri olarak boş bir koleksiyon atayın.

3. Koleksiyon değeri ayarlayıcısının (setter) çağrılarını bulun. Ayarlayıcıyı, öge ekleme ve silme işlemlerini kullanacak şekilde değiştirin veya bu işlemlerin istemci kodunu çağırmasını sağlayın.

Ayarlayıcıların yalnızca tüm koleksiyon öğelerini diğerleriyle değiştirmek için kullanılabileceğini unutmayın. Bu nedenle, ayarlayıcı adının (**Rename Method**) değiştirilmesi (`replace`) önerilebilir.

4. Koleksiyonun değiştirildiği koleksiyon alıcısının tüm çağrılarını bulun. Kodu, koleksiyondaki öğeleri eklemek ve silmek için yeni yöntemlerinizi kullanacak şekilde değiştirin.

5. Alıcıyı, koleksiyonun salt okunur (read-only) bir temsilini döndürecek şekilde değiştirin.

6. Koleksiyonu kullanan istemci kodunu, koleksiyon sınıfının içinde daha iyi görünecek kod açısından inceleyin.


## Replace Type Code with Class

Tür kodu (Type Code) nedir? Tür kodu, ayrı bir veri türü yerine, bazı varlıklar için izin verilen değerlerin listesini oluşturan bir dizi sayı veya dizeye sahip olduğunuzda oluşur. Çoğu zaman bu belirli sayılara ve dizelere sabitler aracılığıyla anlaşılır adlar verilir, bu tür kodlarla bu kadar çok karşılaşılmasının nedeni budur.

### 🙁 Problem

Bir sınıfın tür kodunu içeren bir alanı vardır. Bu türdeki değerler operatör koşullarında kullanılmaz ve programın davranışını etkilemez.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20Class%20-%20Before.png)

</div>

### 😊 Çözüm

Yeni bir sınıf oluşturun ve tür kodu değerleri yerine sınıfa ait nesneleri kullanın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20Class%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Tür kodunun en yaygın nedenlerinden biri, bir veritabanında bazı karmaşık kavramların bir sayı veya dize ile kodlandığı alanlar bulunduğunda, veritabanlarıyla çalışmaktır.

Örneğin, `user_role` alanına sahip `User` sınıfınız var; bu sınıf, yönetici (Admin), editör (Editor) veya sıradan kullanıcı (User) olsun  her kullanıcının erişim ayrıcalıkları hakkında bilgi içerir. Yani bu durumda bu bilgi alana sırasıyla `A`, `E` ve `U` olarak kodlanır.

Bu yaklaşımın eksiklikleri nelerdir? Alan ayarlayıcılar genellikle hangi değerin gönderildiğini kontrol etmez; bu durum, birisi bu alanlara istenmeyen veya yanlış değerler gönderdiğinde büyük sorunlara neden olabilir.

Ayrıca bu alanlar için tür doğrulaması yapılamaz. Onlara, IDE'niz tarafından tip kontrolü yapılmayacak ve hatta programınızın çalışmasına (ve daha sonra çökmesine) izin vermeyecek herhangi bir sayı veya dize göndermek mümkündür.

### ✅ Avantajları

- İlkel değer kümelerini (kodlanmış türler budur) nesne yönelimli programlamanın sunduğu tüm avantajlara sahip tam teşekküllü sınıflara dönüştürmek istiyoruz.

- Tür kodunu sınıflarla değiştirerek, programlama dili düzeyinde yöntemlere ve alanlara iletilen değerler için tür ipuçlarına izin veriyoruz.

Örneğin, derleyici daha önce bir yönteme bir değer iletildiğinde sayısal sabitiniz ile rastgele bir sayı arasındaki farkı görmezken, artık belirtilen tür sınıfına uymayan veriler iletildiğinde, şu konuda uyarılırsınız: IDE'nizin içindeki hata.

- Böylece kodu türün sınıflarına taşımayı mümkün kılıyoruz. Tüm program boyunca tür değerleriyle karmaşık manipülasyonlar yapmanız gerekiyorsa, artık bu kod bir veya daha fazla tür sınıfının içinde "yaşabilir".

### 🖐🏼 Ne Zaman Kullanılmamalı?

Kodlanmış bir türün değerleri kontrol akışı yapılarında (`if`, `switch` vb.) kullanılıyorsa ve bir sınıf davranışını kontrol ediyorsa, tür kodu için iki yeniden düzenleme tekniğinden birini kullanmalısınız:

- **Replace Type Code with Subclasses**

- **Replace Type Code with State/Strategy**


### 🤯 Nasıl Refactor Edilir?

1. Yeni bir sınıf oluşturun ve ona kodlanan türün amacına uygun yeni bir ad verin. Burada buna tip sınıfı (type class) diyeceğiz.

2. Tür kodunu içeren alanı tür sınıfına kopyalayın ve private yapın. Daha sonra alan için bir alıcı yani getter oluşturun. Bu alan için yalnızca yapıcıdan (constructor) bir değer ayarlanacaktır.

3. Kodlanmış türün her değeri için tür sınıfında statik bir yöntem oluşturun. Kodlanmış türün bu değerine karşılık gelen yeni bir tür sınıfı nesnesi oluşturacaktır.

4. Orijinal sınıfta, kodlanmış alanın türünü type sınıfıyla değiştirin. Yapıcıda (constructor) ve alan ayarlayıcıda (setter) bu türden yeni bir nesne oluşturun. Alan alıcısını (getter), tür sınıfı alıcısını çağıracak şekilde değiştirin.

5. Kodlanmış türdeki değerlerden söz edilenleri, ilgili tür sınıfı statik yöntemlerinin çağrılarıyla değiştirin.

6. Kodlanmış tür sabitlerini orijinal sınıftan kaldırın.


## Replace Type Code with Subclasses

Tür kodu (Type Code) nedir? Tür kodu, ayrı bir veri türü yerine, bazı varlıklar için izin verilen değerlerin listesini oluşturan bir dizi sayı veya dizeye sahip olduğunuzda oluşur. Çoğu zaman bu belirli sayılara ve dizelere sabitler aracılığıyla anlaşılır adlar verilir, bu tür kodlarla bu kadar çok karşılaşılmasının nedeni budur.

### 🙁 Problem

Kodlanan türün her değeri için alt sınıflar oluşturun. Daha sonra ilgili davranışları orijinal sınıftan bu alt sınıflara çıkarın. Kontrol akışı kodunu polimorfizmle değiştirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20Subclasses%20-%20Before.png)

</div>

### 😊 Çözüm

Yeni bir sınıf oluşturun ve tür kodu değerleri yerine sınıfa ait nesneleri kullanın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20Subclasses%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bu refactoring tekniği, **Replace Type Code with Class** tekniğinin daha karmaşık bir versiyonudur.

İlk refactoring yönteminde olduğu gibi, bir alan için izin verilen tüm değerleri oluşturan bir dizi basit değere sahipsiniz. Bu değerler genellikle sabit olarak belirtilmesine ve anlaşılır adlara sahip olmasına rağmen, bunların kullanımı kodunuzu hataya çok açık hale getirir, çünkü bunlar hala ilkeldir. Örneğin parametrelerde bu değerlerden birini kabul eden bir yönteminiz var. Belirli bir anda, `"ADMIN"` değerine sahip `USER_TYPE_ADMIN` sabiti yerine, yöntem aynı dizeyi küçük harflerle `("admin")` alır, bu da yazarın (sizin) amaçlamadığı başka bir şeyin yürütülmesine neden olur.

Burada `if`, `switch` ve `?:` gibi koşullu ifadeler gibi kontrol akış koduyla ilgileniyoruz. Yani bu operatörlerin koşulları içerisinde kodlanmış değerleri olan alanlar (`$user->type === self::USER_TYPE_ADMIN` gibi) kullanılır. Burada **Replace Type Code with Class** tekniğini kullanacak olsaydık, tüm bu kontrol akışı yapılarının veri türünden sorumlu bir sınıfa taşınması en iyisi olurdu. Sonuçta bu elbette orijinaline çok benzeyen, aynı problemlere sahip bir tip sınıfı yaratacaktır.

### ✅ Avantajları

- Kontrol akış kodunu silin. Orijinal sınıfta büyük bir `switch` yerine kodu uygun alt sınıflara taşıyın. Bu, Tek Sorumluluk İlkesine bağlılığı artırır ve programı genel olarak daha okunabilir hale getirir.

- Kodlanmış bir tür için yeni bir değer eklemeniz gerekiyorsa tek yapmanız gereken mevcut koda dokunmadan yeni bir alt sınıf eklemektir (bkz. Açık/Kapalı Prensibi).

- Tip kodunu sınıflarla değiştirerek, programlama dili düzeyinde metodlar ve alanlar için tip ipuçlarının önünü açıyoruz. Kodlanmış bir türün içerdiği basit sayısal veya dize değerleri kullanılarak bu mümkün olmaz.

### 🖐🏼 Ne Zaman Kullanılmamalı?

 - Zaten bir sınıf hiyerarşiniz varsa bu teknik uygulanamaz. Nesne yönelimli programlamada miras yoluyla ikili hiyerarşi oluşturamazsınız. Yine de tür kodunu miras yerine kompozisyon yoluyla değiştirebilirsiniz. Bunu yapmak için **Replace Type Code with State/Strategy.** tekniğini kullanın.
 - Bir nesne oluşturulduktan sonra tür kodunun değerleri değişebiliyorsa bu teknikten kaçının. Bir şekilde nesnenin sınıfını anında değiştirmemiz gerekir ki bu mümkün değildir. Yine de bu durumda da bir alternatif **Replace Type Code with State/Strategy** tekniği olacaktır.

### 🤯 Nasıl Refactor Edilir?

1. Tür kodunu içeren alan için bir alıcı oluşturmak amacıyla **Self Encapsulate Field** tekniğini kullanın.

2. Süper sınıf yapıcıyı (superclass constructor) private yapın. Üst sınıf yapıcıyla aynı parametrelere sahip statik bir fabrika yöntemi oluşturun. Kodlanan türün başlangıç ​​değerlerini alacak parametreyi içermelidir. Bu parametreye bağlı olarak fabrika yöntemi çeşitli alt sınıflardan nesneler yaratacaktır. Bunu yapmak için kodunda büyük bir koşul oluşturmalısınız, ancak en azından gerçekten gerekli olduğunda tek koşul bu olacaktır; aksi takdirde alt sınıflar ve polimorfizm işe yarayacaktır.

3. Kodlanmış türün her değeri için benzersiz bir alt sınıf oluşturun. İçinde, kodlanmış türün alıcısını, kodlanmış türün karşılık gelen değerini döndürecek şekilde yeniden tanımlayın.

4. Tür kodunun bulunduğu alanı üst sınıftan silin. Alıcısını (getter) soyut (abstract) yapın.

5. Artık alt sınıflarınız olduğuna göre, alanları ve yöntemleri üst sınıftan karşılık gelen alt sınıflara taşımaya başlayabilirsiniz (**Push Down Field** ve **Push Down Method** tekniklerinin yardımıyla).

6. Mümkün olan her şey taşındığında, tür kodunu kullanan koşullardan tamamen kurtulmak için **Replace Conditional with Polymorphism ** tekniğini kullanın.



## Replace Type Code with State/Strategy

Tür kodu (Type Code) nedir? Tür kodu, ayrı bir veri türü yerine, bazı varlıklar için izin verilen değerlerin listesini oluşturan bir dizi sayı veya dizeye sahip olduğunuzda oluşur. Çoğu zaman bu belirli sayılara ve dizelere sabitler aracılığıyla anlaşılır adlar verilir, bu tür kodlarla bu kadar çok karşılaşılmasının nedeni budur.

### 🙁 Problem

Davranışı etkileyen kodlanmış bir türünüz var ancak ondan kurtulmak için alt sınıfları kullanamazsınız.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20State-Strategy%20-%20Before.png)

</div>

### 😊 Çözüm

Tür kodunu bir durum nesnesiyle değiştirin. Bir alan değerinin tür koduyla değiştirilmesi gerekiyorsa başka bir durum nesnesi takılıdır yani bağlıdır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Type%20Code%20with%20State-Strategy%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Tür kodunuz var ve bu bir sınıfın davranışını etkiliyor, bu nedenle **Replace Type Code with Class** yöntemini kullanamıyoruz.

Tür kodu bir sınıfın davranışını etkiliyor ancak mevcut sınıf hiyerarşisi veya başka nedenlerden dolayı kodlanan tür için alt sınıflar oluşturamıyoruz. Bu, **Replace Type Code with Subclasses** uygulayamayacağımız anlamına gelir.

### ✅ Avantajları

- Bu refactoring tekniği, kodlanmış türe sahip bir alanın, nesnenin ömrü boyunca değerini değiştirdiği durumlardan kurtulmanın bir yoludur. Bu durumda değerin değiştirilmesi, orijinal sınıfın atıfta bulunduğu durum nesnesinin değiştirilmesi yoluyla yapılır.

- Kodlanmış türde yeni bir değer eklemeniz gerekiyorsa tek yapmanız gereken, mevcut kodu değiştirmeden yeni bir durum alt sınıfı eklemektir (bkz. Açık/Kapalı Prensibi).

### 🚫 Dezavantajları

Basit bir tür kodu durumunuz varsa ancak yine de bu yeniden düzenleme tekniğini kullanıyorsanız, birçok ekstra (ve gereksiz) sınıfınız olacaktır.

### 🤓 Bilinmesinde Yarar Var

Bu refactoring tekniğinin uygulanması sırasında iki tasarım modelinden birini kullanabilir: Durum (**State**) veya Strateji (**Strategy**). Hangi modeli seçerseniz seçin uygulama aynıdır. Peki belirli bir durumda hangi modeli seçmelisiniz?

Algoritma seçimini kontrol eden bir koşulu bölmeye çalışıyorsanız Strateji tasarım desenini kullanın.

Ancak kodlanmış türün her değeri yalnızca bir algoritmanın seçiminden değil aynı zamanda sınıfın tüm durumundan, sınıf durumundan, alan değerlerinden ve diğer birçok eylemden sorumluysa, State bu iş için daha iyidir.

### 🤯 Nasıl Refactor Edilir?

1. Tür kodunu içeren alan için bir alıcı oluşturmak amacıyla **Self Encapsulate Field** tekniğini kullanın.

2. Yeni bir sınıf oluşturun ve ona tür kodunun amacına uygun, anlaşılır bir ad verin. Bu sınıf devlet (veya strateji) rolünü oynayacaktır. İçinde soyut kodlanmış bir alan alıcısı oluşturun.

3. Kodlanmış türün her değeri için durum sınıfının alt sınıflarını oluşturun. Her alt sınıfta, kodlanmış alanın alıcısını, kodlanmış türün karşılık gelen değerini döndürecek şekilde yeniden tanımlayın.

4. Soyut durum sınıfında, kodlanmış türün değerini parametre olarak kabul eden statik bir fabrika yöntemi oluşturun. Bu parametreye bağlı olarak fabrika yöntemi çeşitli durumlardaki nesneler yaratacaktır. Bunun için kodunda büyük bir koşul oluşturun; Yeniden düzenleme tamamlandığında tek kişi bu olacak.

5. Orijinal sınıfta kodlanmış alanın türünü durum sınıfı olarak değiştirin. Alanın ayarlayıcısında, yeni durum nesnelerini almak için fabrika durumu yöntemini çağırın.

6. Artık alanları ve yöntemleri üst sınıftan ilgili durum alt sınıflarına taşımaya başlayabilirsiniz (**Push Down Field** ve **Push Down Method** kullanarak).

7. Taşınabilir her şey taşındığında, tür kodunu kullanan koşullu ifadelerden tamamen kurtulmak için **Replace Conditional with Polymorphism** tekniğini kullanın.



## Replace Subclass with Fields

### 🙁 Problem

Yalnızca constant-returning (sabit dönen) yöntemlerinde farklılık gösteren alt sınıflarınız var.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Subclass%20with%20Fields%20-%20Before.png)

</div>

### 😊 Çözüm

Yöntemleri üst sınıftaki alanlarla değiştirin ve alt sınıfları silin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Replace%20Subclass%20with%20Fields%20-%20After.png)

</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bazen refactoring, tür koddan (type code) kaçınmak için yalnızca bir bilettir.

Böyle bir durumda, alt sınıfların hiyerarşisi yalnızca belirli yöntemlerin döndürdüğü değerlerde farklı olabilir. Bu yöntemler hesaplamanın sonucu bile değildir; yöntemlerin kendisinde veya yöntemlerin döndürdüğü alanlarda kesin olarak belirtilmiştir. Sınıf mimarisini basitleştirmek için bu hiyerarşi, duruma bağlı olarak gerekli değerlere sahip bir veya daha fazla alan içeren tek bir sınıfa sıkıştırılabilir.

Bu değişiklikler, büyük miktarda işlevselliğin bir sınıf hiyerarşisinden başka bir yere taşınmasından sonra gerekli olabilir. Mevcut hiyerarşi artık o kadar değerli değil ve alt sınıfları artık sadece ölü ağırlık.

### ✅ Avantajları

Sistem mimarisini basitleştirir. Tek yapmak istediğiniz, farklı yöntemlerde farklı değerler döndürmekse, alt sınıflar oluşturmak aşırıya kaçmak demektir.

### 🤯 Nasıl Refactor Edilir?

1. Alt sınıflara **Replace Constructor with Factory Method** tekniğini uygulayın.

2. Alt sınıf yapıcı çağrılarını üst sınıf fabrika yöntemi çağrılarıyla değiştirin.

3. Üst sınıfta, sabit değerler döndüren alt sınıf yöntemlerinin her birinin değerlerini depolamak için alanlar bildirin.

4. Yeni alanları başlatmak için korumalı bir üst sınıf oluşturucusu (superclass constructor) oluşturun.

5. Mevcut alt sınıf oluşturucularını, üst sınıfın yeni oluşturucusunu çağıracak ve ilgili değerleri ona iletecek şekilde oluşturun veya değiştirin.

6. İlgili alanın değerini döndürecek şekilde her sabit yöntemi üst sınıfa uygulayın. Daha sonra yöntemi alt sınıftan kaldırın.

7. Alt sınıf yapıcısının ek işlevleri varsa yapıcıyı üst sınıf fabrika yöntemine dahil etmek için ** Inline Method** tekniğini kullanın.

8. Alt sınıfı silin.
