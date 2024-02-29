# Oluşturma Yöntemleri (Composing Methods)

Refactoring'in büyük bir kısmı yöntemlerin doğru şekilde oluşturulmasına ayrılmıştır. Çoğu durumda, aşırı uzun yöntemler tüm kötülüklerin anası olarak düşünülebilir. Bu yöntemlerin içindeki kod değişkenleri, yürütme mantığını gizler ve yöntemin anlaşılmasını son derece zorlaştırır, hatta değiştirilmesini daha da zorlaştırır.

Bu gruptaki refactoring teknikleri yöntemlerin kullanımını kolaylaştırır, kod tekrarını ortadan kaldırır. Böylelikle gelecekteki iyileştirmelerin önünü açar.

## Extract Method

### 🙁 Problem

Birlikte gruplandırılabilecek bir kod parçanız var. Ancak bu kısımların ayrı ayrı kullanılması sorun olabilir.

```java
void printOwing() {
  printBanner();

  // Print details.
  System.out.println("name: " + name);
  System.out.println("amount: " + getOutstanding());
}
```

### 😊 Çözüm

Bu kodu ayrı bir yeni metoda (veya işleve) taşıyın ve eski kodu, method çağrısıyla değiştirin.

```java
void printOwing() {
  printBanner();
  printDetails(getOutstanding());
}

void printDetails(double outstanding) {
  System.out.println("name: " + name);
  System.out.println("amount: " + outstanding);
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bir yöntemde ne kadar çok satır bulunursa, yöntemin ne yaptığını anlamak o kadar zor olur. Bu refactoring'in temel nedeni budur.

Kodunuzdaki pürüzlü kısımları ortadan kaldırmanın yanı sıra, yöntemlerin çıkarılması yani extract edilmesi aynı zamanda diğer birçok refactoring yaklaşımının da bir adımıdır.

### ✅ Avantajları

- Elbette daha okunabilir kod! Yeni metoda, metodun amacını açıklayan bir isim vermek önemlidir: `createOrder()`, `renderCustomerInfo()`, vb.

- Daha az kod tekrarı. Genellikle bir metodun içinde bulunan kod, programınızın diğer yerlerinde tekrar kullanılabilir. Bu nedenle, duplicate edilmiş kodları, yeni metodunuza yapılan çağrılarla değiştirebilirsiniz.

- Bağımsız kod parçalarını izole eder. Böylelikle hataların daha az ortaya çıkmasını sağlar (örneğin yanlış değişkenin değiştirilmesi gibi).


### 🤯 Nasıl Refactor Edilir?

1. Yeni bir metod oluşturun ve amacını net bir şekilde yansıtan bir şekilde adlandırın.

2. İlgili kod parçasını yeni metodunuza kopyalayın. Parçayı eski yerinden silin ve yerine yeni oluşturmuş olduğunuz metodun çağrısını koyun.

Bu kod parçasında kullanılan tüm değişkenleri bulun. Eğer değişkenler parçanın içinde tanımlanmışsa ve dışında kullanılmıyorsa, onları değiştirmeyin. Bu değişkenler, yeni metod için yerel değişkenler olacaklardır.

3. Eğer değişkenler, çıkaracağınız (extract) kodun öncesinde tanımlanmışsa, bu değişkenleri yeni metodunuzun parametrelerine geçirmeniz gerekecek, böylece önceki değerlerini kullanabilirsiniz. Bu değişkenlerden kurtulmak için **Replace Temp with Query** yöntemini kullanmak bazen daha kolay olabilir.

4. Eğer çıkarılan (extract) kodun içinde bir yerel değişkenin bir şekilde değiştiğini görüyorsanız, bu değişen değerin ileride ana metodunuzda gerekebileceği anlamına gelebilir. İki kere kontrol edin! Eğer gerçekten değişebiliyorsa, bu değişkenin değerini ana metoda döndürerek her şeyin önceki halindeki gibi çalışmasını sürdürmesini sağlayın.

## Inline Method

### 🙁 Problem

Bir yöntemin gövdesi yöntemin kendisinden daha çok önem arz ettiği durumlar sorun olabilir.
```java
class PizzaDelivery {
  // ...
  int getRating() {
    return moreThanFiveLateDeliveries() ? 2 : 1;
  }
  boolean moreThanFiveLateDeliveries() {
    return numberOfLateDeliveries > 5;
  }
}
```

### 😊 Çözüm

Yönteme yapılan çağrıları yöntemin içeriğiyle değiştirin ve yöntemin kendisini silin.

```java
class PizzaDelivery {
  // ...
  int getRating() {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu sorunda bir yöntem basitçe başka bir yönteme yetki vermektedir. Bu delegasyonun kendi başına bir sorunu yoktur. Ancak bu tür birçok yöntem olduğunda, bunlar çözülmesi zor, kafa karıştırıcı bir karmaşa haline gelir. Koda bakan başka bir geliştirici yöntemi anlamakta vakit kaybedecektir.

Çoğunlukla yöntemler başlangıçta çok kısa değildir, bununla beraber programda değişiklikler yapıldıkça bu hale gelirler. Bu nedenle, kullanım süresi dolmuş yöntemlerden kurtulmaktan çekinmeyin.

### ✅ Avantajları

Gereksiz yöntemlerin sayısını en aza indirerek kodu daha anlaşılır hale getirirsiniz.

### 🤯 Nasıl Refactor Edilir?

1. Yöntemin alt sınıflarda yeniden tanımlanmadığından emin olun. Yöntem yeniden tanımlanmışsa bu teknikten kaçınmak daha sağlıklıdır.

2. Yönteme yapılan tüm çağrıları bulun. Bu çağrıları yöntemin içeriğiyle değiştirin.

3. Son olarak yöntemi silin.

## Extract Variable

### 🙁 Problem

Anlaşılması zor bir ifadenizin olması sorun olabilir.

```java
void renderBanner() {
  if ((platform.toUpperCase().indexOf("MAC") > -1) &&
       (browser.toUpperCase().indexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

### 😊 Çözüm

İfadenin sonucunu veya parçalarını kendi kendini açıklayan ayrı değişkenlere ayrırın/parçalayın.

```java
void renderBanner() {
  final boolean isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
  final boolean isIE = browser.toUpperCase().indexOf("IE") > -1;
  final boolean wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) {
    // do something
  }
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Değişkenleri çıkarmanın (extract) temel nedeni, karmaşık bir ifadeyi ara kısımlarına bölerek daha anlaşılır hale getirmektir. 

Bunlar şunlar olabilir:
- C tabanlı dillerde `if()` operatörünün durumu veya `? :` operatörünün bir kısmı
- Ara sonuçları olmayan uzun bir aritmetik ifade
- Uzun çok parçalı satırlar

Çıkarılan (extract) ifadenin kodunuzun başka yerlerinde kullanıldığını görürseniz, bir değişkenin çıkarılması, **Extract Method** tekniğini gerçekleştirmenin ilk adımı olabilir.

### ✅ Avantajları

Daha okunabilir kod! Çıkarılan yani ayrılan değişkenlere, değişkenin amacını net bir şekilde açıklayan iyi isimler vermeye çalışın. `customerTaxValue`, `cityUnemploymentRate`, `clientSalutationString` vb. gibi adları tercih edin.

### 🚫 Dezavantajları

- Kodunuzda daha fazla değişken mevcut olur. Ancak bu, kodunuzu okuma kolaylığı ile dengelenir.
- Koşullu ifadeleri refactor ederken, derleyicinin büyük olasılıkla, elde edilen değeri oluşturmak için gereken hesaplama miktarını en aza indirecek şekilde onu optimize edeceğini unutmayın. Diyelim ki şu ifadeye sahipsiniz `if (a() || b()) .... ` Eğer `a` yöntemi `true` değerini döndürürse program `b` yöntemini çağırmaz çünkü sonuçtaki değer, hangi değer döndürülürse döndürülsün yine de `true` olacaktır. Bu durum `b` değeri ne olursa olsun bu şekilde olacaktır.

Bununla birlikte, bu ifadenin bazı kısımlarını değişkenlere ayırırsanız, her iki yöntem de her zaman çağrılacaktır; bu da, özellikle bu yöntemler ağır işler yapıyorsa, programın performansına zarar verebilir. 😟

### 🤯 Nasıl Refactor Edilir?

1. İlgili ifadenin önüne yeni bir satır ekleyin ve orada yeni bir değişken tanımlayın. Karmaşık ifadenin bir kısmını bu değişkene atayın.

2. İfadenin bu bölümünü yeni değişkenle değiştirin.

3. İfadenin tüm karmaşık kısımları için işlemi tekrarlayın.


## Inline Temp

### 🙁 Problem

Basit bir ifadenin sonucunun atandığı geçici bir değişkeniniz var. Ayrı bir değişkende tutmak ne kadar mantıklı?

```java
boolean hasDiscount(Order order) {
  double basePrice = order.basePrice();
  return basePrice > 1000;
}
```

### 😊 Çözüm

Değişkene atamak yerine döndürülecek ifadeyi direkt kullanın.

```java
boolean hasDiscount(Order order) {
  return order.basePrice() > 1000;
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Satır içi yerel (inline temps) değişkenler neredeyse her zaman **Replace Temp with Query** tekniğinin bir parçası olarak veya **Extract Method** yönteminin önünü açmak için kullanılır.

### ✅ Avantajları

Bu yeniden düzenleme tekniği kendi başına neredeyse hiçbir fayda sağlamaz. Ancak değişkene bir yöntemin sonucu atanırsa, gereksiz değişkenden kurtularak programın okunabilirliğini marjinal olarak artırabilirsiniz.

### 🚫 Dezavantajları

Bazen görünüşte işe yaramayan geçici değişkenler, birkaç kez yeniden kullanılan pahalı bir işlemin sonucunu önbelleğe almak için kullanılır. Bu nedenle, bu yeniden düzenleme tekniğini kullanmadan önce basitliğin performansa mal olmayacağından emin olun.

### 🤯 Nasıl Refactor Edilir?

1. Değişkeni kullanan tüm yerleri bulun. Değişken yerine kendisine atanmış olan ifadeyi kullanın.

2. Değişkenin tanımlama satırını silin.


## Replace Temp with Query

### 🙁 Problem

Bir ifadenin sonucunu daha sonra kodunuzda kullanmak üzere yerel bir değişkene yerleştirmeniz bazen sorun olabilir.

```java
double calculateTotal() {
  double basePrice = quantity * itemPrice;
  if (basePrice > 1000) {
    return basePrice * 0.95;
  }
  else {
    return basePrice * 0.98;
  }
}
```

### 😊 Çözüm

İfadenin tamamını ayrı bir yönteme taşıyın ve sonucu ondan döndürün. Değişken kullanmak yerine yöntemi sorgulayın. Gerekirse yeni yöntemi diğer yöntemlere dahil edin.

```java
double calculateTotal() {
  if (basePrice() > 1000) {
    return basePrice() * 0.95;
  }
  else {
    return basePrice() * 0.98;
  }
}
double basePrice() {
  return quantity * itemPrice;
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu refactor, Extract Method tekniğinin çok uzun bir yöntemin bir kısmı için uygulanmasına zemin hazırlayabilir.

Method içerisinde kullanılan ifade bazen diğer yöntemlerde de bulunabilir, bu da ortak bir yöntem oluşturmayı düşünmenin nedenlerinden biridir.

### ✅ Avantajları

- Kod okunabilirliği. `GetTax()` yönteminin amacını anlamak, `orderPrice() * 0.2` satırından çok daha kolaydır.

- Değiştirilen satırın birden fazla yöntemde kullanılması durumunda birçok yerde değişikliğe gidilmek yerine method içerisindeki bir değişiklik her yeri etkileyecektir.

### 🤓 Bilinmesinde Yarar Var

**Performans**

Bu refactor yaklaşımın performansta bir düşüşe neden olup olmayacağı sorusunu gündeme getirebilir. Dürüst cevap şudur: evet performansta düşüş olacaktır. Çünkü ortaya çıkan kod yeni bir yöntemin çağrılmasıyla kullanılabilir. Ancak günümüzün hızlı CPU'ları ve güçlü derleyicileri sayesinde yük neredeyse her zaman minimum düzeyde olacaktır. Buna karşılık, okunabilir kod ve refactor yaklaşımı sayesinde bu yöntemi program kodunun başka yerlerinde yeniden kullanma yeteneği dikkat çekici faydalardır.

Bununla birlikte, geçici değişkeniniz gerçekten zaman alan bir ifadenin sonucunu önbelleğe almak için kullanılıyorsa, ifadeyi yeni bir yönteme çıkardıktan sonra bu refactoring'i durdurmak isteyebilirsiniz.

### 🤯 Nasıl Refactor Edilir?

1. Yöntem içinde, değişkene yalnızca bir kez değer atandığından emin olun. Eğer aksi bir durum varsa, değişkenin yalnızca ifadenizin sonucunu depolamak için kullanılacağından emin olmak için **Split Temporary Variable** tekniğini kullanın.

2. İlgili ifadeyi yeni bir yönteme yerleştirmek için **Extract Method** tekniğini kullanın. Bu yöntemin yalnızca bir değer döndürdüğünden ve nesnenin durumunu değiştirmediğinden emin olun. Yöntem nesnenin durumunu etkiliyorsa veya değiştiriyorsa **Separate Query from Modifier** yöntemini göz önünde bulundurun/deneyin.

3. Değişkenin bulunduğu yerleri, yeni oluşturduğunuz yönteminizi çağırmaya yönelik kod satırı ile değiştirin.

## Split Temporary Variable

### 🙁 Problem

Bir yöntemin içinde çeşitli ara değerleri depolamak için kullanılan bir yerel değişkeninizin olması sorun olabilir. Döngü değişkenlerini bu sınıfa dahil etmiyoruz 😅

```java
double temp = 2 * (height + width);
System.out.println(temp);
temp = height * width;
System.out.println(temp);
```

### 😊 Çözüm

Farklı değerler için farklı değişkenler kullanın. Her değişken yalnızca belirli bir şeyden sorumlu olmalıdır.

```java
final double perimeter = 2 * (height + width);
System.out.println(perimeter);
final double area = height * width;
System.out.println(area);
```

### 🤔 Neden Refactoring Uygulanmalı?

Bir fonksiyonun içinde barındırdığı değişkenlerin sayısını göz ardı ediyorsanız ve bu değişkenleri kendisi ile ilgisiz çeşitli amaçlarla yeniden kullandığınız bir seneryo var diyelim. Size kötü bir haberim var; değişkenleri içeren kodda değişiklik yapmanız gerektiğinde sorunlarla karşılaşacağınızdan emin olabilirsiniz. Doğru değerlerin kullanıldığından emin olmak için her değişken kullanımını yeniden kontrol etmeniz gerekecektir. Bu da size vakit kaybettirecektir.

### ✅ Avantajları

- Program kodunun her bileşeni sadece tek bir şeyden sorumlu olmalıdır. Bu yöntem, kodun bakımını çok daha kolay hale getirir. Çünkü tahmin edilemez etkilerden korkmadan herhangi bir kod satırını kolayca değiştirebilirsiniz.

- Kod daha okunabilir hale gelir. Eğer bir değişken uzun zaman önce aceleyle oluşturulduysa, muhtemelen hiçbir şey açıklamayan bir isme sahiptir: `k`, `a2`, `value`, vb. Ancak yeni değişkenleri anlaşılır ve kendiliğinden açıklayıcı bir şekilde adlandırarak bu durumu düzeltebilirsiniz. Örneğin; `customerTaxValue`, `cityUnemploymentRate`, `clientSalutationString`gibi.

- Bu refaktörleme tekniği, daha sonra **Extract Method** tekniğini kullanmayı planlıyorsanız faydalıdır.


### 🤯 Nasıl Refactor Edilir?

1. Kodda değişkene değer ataması yapılan ilk yeri bulun. Burada değişkeni, atanan değere karşılık gelen uygun bir isimle yeniden adlandırmalısınız.

2. Değişkenin bu değerinin kullanıldığı yerlerde eski adı yerine yeni adını kullanın.

3. Bu işlemi değişkene farklı bir değerin atandığı yerler için gerektiği kadar tekrarlayın.


## Remove Assignments to Parameters

### 🙁 Problem

Metodun gövdesindeki bir parametreye değer atamanız sorun oluşturabilir.

```java
int discount(int inputVal, int quantity) {
  if (quantity > 50) {
    inputVal -= 2;
  }
  // ...
}
```

### 😊 Çözüm

Parametre yerine yerel bir değişken kullanın.

```java
int discount(int inputVal, int quantity) {
  int result = inputVal;
  if (quantity > 50) {
    result -= 2;
  }
  // ...
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu yeniden düzenlemenin nedenleri **Split Temporary Variable** tekniği ile aynıdır. Ancak bu case'de yerel bir değişkenle değil bir parametreyle ilgileniyoruz.

Öncelikle referans yoluyla bir parametre iletilirse, yöntem içerisinde parametre değeri değiştirildikten sonra bu değer, yöntemin çağrılmasını talep eden argümana iletilir. Çoğu zaman bu kazara meydana gelir ve talihsiz sonuçlara yol açar. Programlama dilinizde parametreler genellikle değere göre (referansa göre değil) aktarılsa bile, bu garip kodlamayla buna alışkın olmayanların kafalarında soru işaretleri oluşturabilir.

İkincisi, tek bir parametreye farklı değerlerin birden çok kez ataması, belirli bir zamanda parametrede hangi verilerin bulunması gerektiğini bilmenizi zorlaştırır. Parametreniz ve içeriği belgelenmişse, ancak gerçek değer yöntem içinde beklenenden farklı olursa problem daha da kötüleşir.

### ✅ Avantajları

- Programın her öğesi yalnızca bir şeyden sorumlu olmalıdır. Bu, herhangi bir yan etki olmadan kodu güvenli bir şekilde değiştirebileceğiniz için kod bakımını ileriye dönük olarak çok daha kolay hale getirir.

- Bu yeniden düzenleme, tekrarlayan kodun ayrı yöntemlere çıkarılmasına (extract) yardımcı olur.

### 🤯 Nasıl Refactor Edilir?

1. Yerel bir değişken oluşturun ve parametrenizin başlangıç ​​değerini atayın.

2. Bu satırı izleyen tüm yöntem kodlarında parametreyi yeni yerel değişkeninizle değiştirin.

## Replace Method with Method Object

### 🙁 Problem

Yerel değişkenlerin çok iç içe geçtiği ve **Extract Methods** tekniğini uygulayamayacağınız uzun bir yönteminizin olması sorun olabilir.

```java
class Order {
  // ...
  public double price() {
    double primaryBasePrice;
    double secondaryBasePrice;
    double tertiaryBasePrice;
    // Perform long computation.
  }
}
```
### 😊 Çözüm

Yerel değişkenlerin sınıfın alanları (field) haline gelmesi için yöntemi ayrı bir sınıfa dönüştürün. Daha sonra yöntemi aynı sınıf içindeki birkaç yönteme bölebilirsiniz.

```java
class Order {
  // ...
  public double price() {
    return new PriceCalculator(this).compute();
  }
}

class PriceCalculator {
  private double primaryBasePrice;
  private double secondaryBasePrice;
  private double tertiaryBasePrice;
  
  public PriceCalculator(Order order) {
    // Copy relevant information from the
    // order object.
  }
  
  public double compute() {
    // Perform long computation.
  }
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bir yöntem çok uzun olabilir ve birbirinden izole edilmesi zor olan yerel değişkenlerin bir birine bağımlılıkları nedeniyle onu ayıramazsınız.

İlk adım, yöntemin tamamını ayrı bir sınıfa taşımak ve yerel değişkenlerini sınıfın alanlarına (fields) dönüştürmektir.

İlk olarak, bu, sorunun sınıf düzeyinde izole edilmesine olanak tanır. İkincisi, büyük ve hantal bir yöntemin, orijinal sınıfın amacına zaten uymayan daha küçük yöntemlere bölünmesinin önünü açar.

### ✅ Avantajları

Uzun bir yöntemi kendi sınıfında izole etmek, yöntemin boyutunun şişmesini durdurmaya olanak tanır. Bu aynı zamanda, orijinal sınıfı faydasız yöntemlerle kirletmeden, sınıf içinde alt yöntemlere bölünmesine de olanak tanır.

### 🚫 Dezavantajları

Programın genel karmaşıklığını artıran başka bir sınıf eklenir.

### 🤯 Nasıl Refactor Edilir?

1. Yeni bir sınıf oluşturun. Yeniden düzenlediğiniz yöntemin amacına göre sınıfı adlandırın.

2. Yeni sınıfta, yöntemin daha önce bulunduğu sınıfın bir örneğine referansı depolamak için private bir alan (fields) oluşturun. Bu tanımlamayla birlikte gerekirse orijinal sınıftan gerekli bazı verileri almak için kullanılabilir.

3. Yöntemin her yerel değişkeni için ayrı bir private alan oluşturun.

4. Yöntemin tüm yerel değişkenlerinin değerlerini parametre olarak kabul eden ve ayrıca karşılık gelen private alanları başlatan bir kurucu (constructor) oluşturun.

5. Ana yöntemi tanımlayın ve yerel değişkenleri private alanlarla değiştirerek orijinal yöntemin kodunu ona kopyalayın.

6. Bir yöntem nesnesi oluşturup ana yöntemini çağırarak, orijinal sınıftaki orijinal yöntemin gövdesini değiştirin.

## Substitute Algorithm

### 🙁 Problem

Mevcut bir algoritmayı yenisiyle değiştirmek mi istiyorsunuz. Bu süreçte sorunlarla karşılaşabilirsiniz.

```java
String foundPerson(String[] people){
  for (int i = 0; i < people.length; i++) {
    if (people[i].equals("Don")){
      return "Don";
    }
    if (people[i].equals("John")){
      return "John";
    }
    if (people[i].equals("Kent")){
      return "Kent";
    }
  }
  return "";
}
```

### 😊 Çözüm

Algoritmayı barındıran yöntemin gövdesini yeni bir algoritmayla değiştirin.

```java
String foundPerson(String[] people){
  List candidates =
    Arrays.asList(new String[] {"Don", "John", "Kent"});
  for (int i=0; i < people.length; i++) {
    if (candidates.contains(people[i])) {
      return people[i];
    }
  }
  return "";
}
```

### 🤔 Neden Refactoring Uygulanmalı?

- Kademeli refactoring, bir programı iyileştirmenin tek yöntemi değildir. Bazen bir yöntem sorunlarla o kadar karmaşık hale gelir ki, yöntemi yıkıp yeni bir başlangıç ​​yapmak daha kolay olur. Belki de çok daha basit ve daha etkili bir algoritma bulmuşsunuzdur. Bu durumda eski algoritmayı yenisiyle değiştirmeniz yeterlidir.

- Zaman geçtikçe algoritmanız iyi bilinen bir kütüphaneye veya framework'e dahil edilebilir. Bununla beraber, bakımı kolaylaştırmak için bağımsız uygulamanızdan kurtulmak isteyebilirsiniz.

- Programınızın gereksinimleri o kadar yoğun şekilde değişebilir ki, mevcut algoritmanız bu görev için yeterli kalmayabilir.

### 🤯 Nasıl Refactor Edilir?

1. Mevcut algoritmayı mümkün olduğunca basitleştirdiğinizden emin olun. Önemsiz kodu, **Extract Methods** tekniğini kullanarak başka yöntemlere taşıyın veya oluşturun. Algoritmanızda ne kadar az dinamik parça olursa, değiştirilmesi o kadar kolay olur.

2. Yeni algoritmanızı yeni bir yöntemle oluşturun. Eski algoritmayı yenisiyle değiştirin ve programı test etmeye başlayın.

3. Sonuçlar eşleşmiyorsa eski uygulamaya dönün ve sonuçları karşılaştırın. Farklılığın nedenlerini tanımlayın. Bunun nedeni genellikle eski algoritmadaki bir hata olsa da, yeni algoritmada çalışmayan bir şeyden kaynaklanma olasılığı daha yüksektir.

4. Tüm testler başarıyla tamamlandığında eski algoritmayı tamamen silin!




