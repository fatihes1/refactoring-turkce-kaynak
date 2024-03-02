# Koşullu İfadeleri Basitleştirme (Simplifying Conditional Expressions)

Koşullu ifadelerin mantıkları projeniz/programınız büyüdükçe giderek daha karmaşık hale gelir. Bu durumla mücadele edebileceğimiz oldukça fazla refactoring tekniği bulunmaktadır.

## Decompose Conditional

### 🙁 Problem

Karmaşık bir koşulunuz var (`if-then`/`else` veya `switch`) bu durumda projedeki diğer geliştiriciler bu koşullu ifadeyi anlamak için zaman kaybedecektir.

```java
if (date.before(SUMMER_START) || date.after(SUMMER_END)) {
  charge = quantity * winterRate + winterServiceCharge;
}
else {
  charge = quantity * summerRate;
}
```

### 😊 Çözüm

Koşulun karmaşık kısımlarını ayrı yöntemlere ayırabilirsiniz. Bu okunurluğu arttıracaktır.

```java
if (isSummer(date)) {
  charge = summerCharge(quantity);
}
else {
  charge = winterCharge(quantity);
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bir kod parçası ne kadar uzun olursa anlaşılması da o kadar zor olur. Kodunuz, koşullarla doldurulduğunda anlaşılması daha da gü. hale gelir:

- `then` bloğundaki kodun ne yaptığını bulmakla meşgulken ilgili koşulun ne olduğunu unutabilirsiniz. Ki uzun koşulları ifadelerde bu çok olağan bir durumdur.

- `else` kısnını ayrıştırmakla meşgulken, kodun `then` kısmının ne yaptığını unutursunuz.

### ✅ Avantajları

- Koşullu kodu açıkça adlandırılmış yöntemlere çıkararak, kodda daha sonra değişiklik veya bakımını yapacak olan kişinin (örneğin, iki ay sonraki siz) hayatını kolaylaştırırsınız.

- Bu refactoring tekniği aynı zamanda koşullardaki kısa ifadeler için de geçerlidir. `isSalaryDay()` işlevi, tarihleri ​​karşılaştırmaya yönelik koddan çok daha güzel ve daha açıklayıcıdır.

### 🤯 Nasıl Refactor Edilir?

1. Koşulu, **Extract Method.** tekniği aracılığıyla ayrı bir yönteme çıkarın.

2. `then` ve `else` blokları için işlemi tekrarlayın.

## Consolidate Conditional Expression

### 🙁 Problem

Aynı sonuca veya eyleme yol açan birden fazla koşulunuz olduğu durumlarda bazı sorunla karşılaşabilirsiniz. Sonuçta koşullu ifadeniz yanlış koşulda takılabilir.

```java
double disabilityAmount() {
  if (seniority < 2) {
    return 0;
  }
  if (monthsDisabled > 12) {
    return 0;
  }
  if (isPartTime) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

### 😊 Çözüm

Tüm bu koşul cümlelerini tek bir ifadede birleştirin.

```java
double disabilityAmount() {
  if (isNotEligibleForDisability()) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Kodunuz aynı eylemleri gerçekleştiren birçok alternatif operatör içerir. Operatörlerin neden bölündüğü belli bile olmayacaktır. Anlaması ise oldukça güçleşecektir.

Birleştirmenin temel amacı, daha fazla netlik sağlamak için koşulu ayrı bir yönteme çıkarmaktır.

### ✅ Avantajları

- Yinelenen kontrol akış kodunu, ortadan kaldırır. Böylelikle daha net bir kod oluşur. Aynı amaca sahip yani aynı değeri döndüren birden fazla koşulu birleştirmek, tek bir eyleme yol açan yalnızca tek bir karmaşık kontrol yaptığınızı göstermenize yardımcı olur.

- Tüm operatörleri birleştirerek artık bu karmaşık ifadeyi, koşulun amacını açıklayan bir isimle yeni bir yöntemde izole edebilirsiniz. Böylelikle diğer geliştiriciler yöntemin adına bakarak bu koşulun ne yaptığını anlayabilir.

### 🤯 Nasıl Refactor Edilir?

Refactoring yapmadan önce, yalnızca değerleri döndürmek yerine, koşullu ifadelerin herhangi bir yan etkisi (side effects) olmadığından veya herhangi bir değeri/değişkeni değiştirmediğinden emin olun. Bir koşulun sonuçlarına göre bir değişkende bir değişiklik olduğu gibi, operatörün içinde yürütülen kodda yan etkiler gizleniyor olabilir. Tüm bunlara refactoring sırasında dikkat etmelisiniz.

1. Koşullu ifadeleri `and` ve `or` kullanarak tek bir ifadede birleştirin. Konsolidasyon sırasında genel bir kural olarak:
	- İç içe geçmiş koşullar `and` kullanılarak birleştirilir.
	- Ardışık koşul cümleleri `or` ile birleştirilir.
2. Operatör koşullarında **Extract Methods** tekniği uygulayın ve yönteme, ifadenin amacını yansıtan bir ad verin.

## Consolidate Duplicate Conditional Fragments

### 🙁 Problem

Bir koşullunun tüm dallarında aynı kod bulunabilir, bu durum kodu okuyan kişinin koşullu ifadenin amacını anlamasını zorlaştırır.

```java
if (isSpecialDeal()) {
  total = price * 0.95;
  send();
}
else {
  total = price * 0.98;
  send();
}
```

### 😊 Çözüm

Kodu koşulun dışına taşıyın.

```java
if (isSpecialDeal()) {
  total = price * 0.95;
}
else {
  total = price * 0.98;
}
send();
```

### 🤔 Neden Refactoring Uygulanmalı?

Bir koşulun tüm dallarında yinelenen kod bulunur; bu, genellikle koşulun dalları içindeki kodun evriminin bir sonucudur. Takım gelişimi buna katkıda bulunan bir faktör olabilir.

### ✅ Avantajları

Kod tekrarını azaltır.

### 🤯 Nasıl Refactor Edilir?

1. Duplicate edilmiş kod koşullu ifadelerin başındaysa, kodu koşullu dallanmadan önceki bir yere taşıyın.

2. Eğer kod dalların sonunda çalıştırılıyorsa, onu koşuldan dışarı çıkarın. Sonrasında koşullu ifadelerin sonra yerleştirin.

3. Yinelenen kod dalların içinde rastgele yerleştirilmişse, sonraki kodun sonucunu değiştirip değiştirmediğine bağlı olarak ilk önce kodu dalın başına veya sonuna taşımayı deneyin.

4. Uygunsa ve duplicate edilmiş kod bir satırdan uzunsa, **Extract Methods** tekniği kullanmayı deneyin. Böylelikle kod daha temiz hale gelir.


## Remove Control Flag

### 🙁 Problem

Birden çok boolean ifadesi için kontrol bayrağı görevi gören bir boolean değişkeniniz varsa bu durum kodun okunmasını zorlaştırabilir.

### 😊 Çözüm

Değişken yerine `break`, `continue` ve `return` anahtar kelimelerini kullanın. Böylelikle koşullu ifadeye bakan herkes aynı durumu hızlıca anlayabilir.

### 🤔 Neden Refactoring Uygulanmalı?

Modern programlama dillerinde, döngülerdeki ve diğer karmaşık yapılardaki kontrol akışını değiştirmek için özel operatörlerimiz olduğundan, bu tarz kullanımlar kodunuzu daha temiz gösterecektir. Ekstra bir bayrak eklemek, kodu karmaşık kılar.

- `break`: Döngüyü durdurur
- `continue`: Geçerli döngünün yürütülmesini durdurur ve bir sonraki yinelemede döngü koşullarını kontrol etmeye gider
- `return`: Tüm fonksiyonun yürütülmesini durdurur ve operatörde verilmişse sonucunu döndürür.

### ✅ Avantajları

Kontrol bayrağı kodu genellikle kontrol akışı operatörleriyle yazılan koddan çok daha hantaldır.

### 🤯 Nasıl Refactor Edilir?

1. Döngüden veya geçerli yinelemeden çıkışa neden olan kontrol bayrağına değer atamasını bulun.

2. Eğer bu bir döngüden çıkışsa, bunu `break` ile değiştirin; Bu bir yinelemeden çıkışsa `continue` veya bu değeri işlevden döndürmeniz gerekiyorsa `return` değerlerini kullanın.

3. Kontrol bayrağıyla ilişkili kalan kodu ve kontrolleri kaldırın.


## Replace Nested Conditional with Guard Clauses

### 🙁 Problem

Bir grup iç içe geçmiş koşul cümleniz var ve kod yürütmenin normal akışını belirlemek zor olması kod içerisinde karmaşıklık oluşturur. Sonradan projeye dahil olan bir kişi belkide aynı amaca hizmet edeceğini anlayamayacağı için kod tekrarına sebep olabilir.

```java
public double getPayAmount() {
  double result;
  if (isDead){
    result = deadAmount();
  }
  else {
    if (isSeparated){
      result = separatedAmount();
    }
    else {
      if (isRetired){
        result = retiredAmount();
      }
      else{
        result = normalPayAmount();
      }
    }
  }
  return result;
}
```

### 😊 Çözüm

Tüm özel kontrolleri ve uç durumları (edge cases) ayrı maddelere ayırın ve bunları ana kontrollerin önüne yerleştirin. İdeal olarak, birbiri ardına gelen düz bir koşul listesine sahip olmalısınız.

```java
public double getPayAmount() {
  if (isDead){
    return deadAmount();
  }
  if (isSeparated){
    return separatedAmount();
  }
  if (isRetired){
    return retiredAmount();
  }
  return normalPayAmount();
}
```

### 🤔 Neden Refactoring Uygulanmalı?

 Her bir iç içelik düzeyinin girintileri, acı ve keder yönünü sağa doğru işaret eden bir ok oluşturur:

```java
if () {
    if () {
        do {
            if () {
                if () {
                    if () {
                        ...
                    }
                }
                ...
            }
            ...
        }
        while ();
        ...
    }
    else {
        ...
    }
}
```

Kod yürütmenin normal akışı hemen belli olmadığından, her koşulun ne yaptığını ve nasıl yaptığını anlamak zordur. Bu koşullar, her bir koşulun, genel yapıyı optimize etmeye yönelik herhangi bir düşünce olmaksızın geçici bir önlem olarak eklendiği, karışık bir evrimi gösterir.

Durumu basitleştirmek için özel durumları, yürütmeyi hemen sonlandıran ve koruma hükümleri doğruysa boş değer döndüren ayrı koşullara ayırın. Aslında buradaki göreviniz yapıyı düz (flat) hale getirmektir. Böylelikle koşullu ifade daha anlaşılabilir olur.

### 🤯 Nasıl Refactor Edilir?

Kodu yan etkilerden (side effects) kurtarmaya çalışın; **Separate Query from Modifier** tekniği bu amaç için yararlı olabilir. Bu çözüm aşağıda açıklanan reshuffling için gerekli olacaktır.

1. Bir hatanın çağrılmasına veya yöntemden bir değerin anında döndürülmesine yol açan tüm koruma cümlelerini ayırın. Bu koşulları yöntemin başına yerleştirin. Böylelikle kod tüm koşulları takip etmeden erkenden işlemini sonlandırabilir.

2. Yeniden düzenleme tamamlandıktan ve tüm testler başarıyla tamamlandıktan sonra, aynı istisnalara veya döndürülen değerlere yol açan koruma cümleleri için **Consolidate Conditional Expression** tekniğini kullanıp kullanamayacağınıza bakın.


## Replace Conditional with Polymorphism

### 🙁 Problem

Nesne türüne veya özelliklerine bağlı olarak çeşitli eylemleri gerçekleştiren bir koşulunuz olduğunu varsayalım. Koşullu ifade zamanla uzadıkça uzayabilir, bu durum da anlaşılmasını güçlendirir.

```java
class Bird {
  // ...
  double getSpeed() {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new RuntimeException("Should be unreachable");
  }
}
```

### 😊 Çözüm

Koşulun dallarıyla eşleşen alt sınıflar oluşturun. Bunlarda, paylaşılan bir yöntem oluşturun ve kodu, koşulun karşılık gelen dalından ona taşıyın. Daha sonra koşullu ifadeyi, ilgili yöntem çağrısıyla değiştirin. Sonuç olarak, nesne sınıfına bağlı olarak polimorfizm yoluyla uygun uygulamaya ulaşılacaktır.

```java
abstract class Bird {
  // ...
  abstract double getSpeed();
}

class European extends Bird {
  double getSpeed() {
    return getBaseSpeed();
  }
}
class African extends Bird {
  double getSpeed() {
    return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  double getSpeed() {
    return (isNailed) ? 0 : getBaseSpeed(voltage);
  }
}

// Somewhere in client code
speed = bird.getSpeed();
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu refactoring tekniği, kodunuz aşağıda listelenenlere göre değişen çeşitli görevleri gerçekleştiren, operatörler içeriyorsa yardımcı olabilir:

- Uyguladığı nesnenin veya arayüzün sınıfı
- Bir nesnenin alanının değeri
- Bir nesnenin yöntemlerinden birinin çağrılmasının sonucu

Yeni bir nesne özelliği veya türü ortaya çıkarsa, tüm benzer koşullardaki kodu aramanız ve eklemeniz gerekecektir. Dolayısıyla, bir nesnenin tüm yöntemlerine dağılmış birden fazla koşulu varsa, bu tekniğin faydası katlanarak artar.

### ✅ Avantajları

- Bu teknik, Söyle-Sorma (Tell-Don’t-Ask) ilkesine bağlıdır: Bir nesneye durumu hakkında soru sormak ve ardından buna dayalı eylemler gerçekleştirmek yerine, nesneye ne yapması gerektiğini basitçe söylemek ve nasıl yapılacağına kendisinin karar vermesine izin vermek çok daha kolaydır.

- Yinelenen kodu kaldırır. Neredeyse aynı olan birçok koşuldan kurtulursunuz. Daha temiz bir kod source elde edilir.

- Yeni bir yürütme varyantı eklemeniz gerekiyorsa tek yapmanız gereken mevcut koda dokunmadan yeni bir alt sınıf eklemektir (Açık/Kapalı Prensibi).


### 🤯 Nasıl Refactor Edilir?

**Refactoring'e Hazırlık**

Bu refactoring tekniği için alternatif davranışları içerecek hazır bir sınıf hiyerarşisine sahip olmalısınız. Eğer böyle bir hiyerarşiniz yoksa bir tane oluşturun. Diğer teknikler bunun gerçekleşmesine yardımcı olacaktır:

- **Replace Type Code with Subclasses:** Belirli bir nesne özelliğinin tüm değerleri için alt sınıflar oluşturulacaktır. Bu yaklaşım basittir ancak nesnenin diğer özellikleri için alt sınıflar oluşturamayacağınız için daha az esnektir.

- **Replace Type Code with State/Strategy:** Belirli bir nesne özelliği için bir sınıf ayrılacak ve özelliğin her değeri için ondan alt sınıflar oluşturulacaktır. Geçerli sınıf, bu türdeki nesnelere referanslar içerecek ve yürütmeyi onlara devredecektir.

Aşağıdaki adımlar, hiyerarşiyi zaten oluşturduğunuzu varsayarak listelenmiştir.

** Refactoring Adımları**

1. Koşul başka eylemleri de gerçekleştiren bir yöntemdeyse, **Extract Methods** tekniğini uygulayın.

2. Her hiyerarşi alt sınıfı için, koşulluyu içeren yöntemi yeniden tanımlayın ve karşılık gelen koşullu dalın kodunu bu konuma kopyalayın.

3. Bu dalı koşuldan silin.

4. Koşullu boşalıncaya kadar değiştirmeyi tekrarlayın. Daha sonra koşulluyu silin ve yöntemin özetini bildirin.


## Introduce Null Object

### 🙁 Problem

Bazı yöntemler gerçek nesneler yerine `null` değerini döndürdüğünden, kodunuzda `null` için birçok kontrol bulunur. Bu kontroller bir süre sonra tekrarlı koda ve karmaşıklığa neden olacaktır.

```java
if (customer == null) {
  plan = BillingPlan.basic();
}
else {
  plan = customer.getPlan();
}
```

### 😊 Çözüm

`null` yerine, varsayılan davranışı sergileyen null bir nesne döndürün.

```java
class NullCustomer extends Customer {
  boolean isNull() {
    return true;
  }
  Plan getPlan() {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
customer = (order.customer != null) ?
  order.customer : new NullCustomer();

// Use Null-object as if it's normal subclass.
plan = customer.getPlan();
```

### 🤔 Neden Refactoring Uygulanmalı?

Düzinelerce `null` kontrolü, kodunuzu daha uzun ve daha çirkin hale getirir.

###  🚫 Dezavantajları

- Koşullu ifadelerden kurtulmanın bedeli yeni bir sınıf daha yaratmaktır. Ama bu sınıfı oluşturmaya duruma göre deyecektir.

### 🤯 Nasıl Refactor Edilir?

1. Söz konusu sınıftan `null` nesne rolünü üstlenecek bir alt sınıf oluşturun.

2. Her iki sınıfta da, boş bir nesne için `true` değerini ve gerçek bir sınıf için `false` değerini döndüren `isNull()` yöntemini oluşturun.

3. Kodun gerçek bir nesne yerine `null` değerini döndürebileceği tüm yerleri bulun. Kodu, boş bir nesne döndürecek şekilde değiştirin.

4. Gerçek sınıfın değişkenlerinin `null` ile karşılaştırıldığı tüm yerleri bulun. Bu kontrolleri `isNull()` çağrısıyla değiştirin.

	- Bir değer `null` değerine eşit olmadığında orijinal sınıfın yöntemleri bu koşullar altında çalıştırılıyorsa, bu yöntemleri null sınıfında yeniden tanımlayın ve koşulun `else` kısmındaki kodu buraya ekleyin. Daha sonra tüm koşulu silebilirsiniz ve farklı davranışlar polimorfizm yoluyla uygulanacaktır.
	- Eğer işler o kadar basit değilse ve yöntemler yeniden tanımlanamıyorsa, boş bir değer durumunda gerçekleştirilmesi gereken işlemleri, `null` nesnenin yeni yöntemlerine basitçe ayıklayıp çıkaramayacağınıza bakın. Varsayılan olarak işlemler olarak `else`'deki eski kod yerine bu yöntemleri çağırın.

## Introduce Assertion

### 🙁 Problem

Kodun bir kısmının doğru çalışması için belirli koşulların veya değerlerin doğru olması gerekir. Bu olağan bir durumdur, ama kodu daha temiz hale getirebilecekken neden bu şekilde ilerleyesiniz ki?

```java
double getExpenseLimit() {
  // Should have either expense limit or
  // a primary project.
  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit :
    primaryProject.getMemberExpenseLimit();
}
```
### 😊 Çözüm

Bu varsayımları belirli assertion kontrolleriyle değiştirin.

```java
double getExpenseLimit() {
  Assert.isTrue(expenseLimit != NULL_EXPENSE || primaryProject != null);

  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.getMemberExpenseLimit();
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Kodun bir kısmının, örneğin bir nesnenin mevcut durumu veya bir parametrenin veya yerel değişkenin değeri hakkında bir şeyler varsaydığını varsayalım. Genellikle bu varsayım, bir hata durumu dışında her zaman geçerli olacaktır.

İlgili assertionları ekleyerek bu varsayımları açık hale getirin. Yöntem parametrelerindeki tür ipuçlarında olduğu gibi, bu assertionlar kodunuz için canlı belgeler görevi görebilir. Böylelikle bu kısımda çalışacak başka bir geliştirici olduğunda anlama sırasında vakit kaybetmeyecektir.

Kodunuzun nerede assertionlara ihtiyaç duyduğunu görmek için bir kılavuz olarak, belirli bir yöntemin çalışacağı koşulları açıklayan yorumları kontrol edin.

### ✅ Avantajları

Bir varsayım doğru değilse ve bu nedenle kod yanlış sonuç veriyorsa, büyük sonuçlara yol açabilir. Bu durumda, veri bozulmasına yol açmadan yürütmeyi durdurmak daha iyidir. Bu aynı zamanda programın testini gerçekleştirmenin yollarını tasarlarken gerekli testi yazmayı ihmal ettiğiniz anlamına da gelir.

### 🚫 Dezavantajları

- Bazen bir hata, basit bir assertiondan daha uygundur. İstisnanın gerekli sınıfını seçebilir ve geri kalan kodun bunu doğru şekilde işlemesini sağlayabilirsiniz.

- Bir istisna ne zaman basit bir assertiondan daha iyidir? İstisna, kullanıcının veya sistemin eylemlerinden kaynaklanabiliyorsa ve bu durmu istisnayla (exception) halledebilirsiniz. Öte yandan, sıradan isimsiz ve işlenmemiş istisnalar temel olarak basit assertionlara eşdeğerdir; onları ele almazsınız ve bunlar yalnızca hiçbir zaman meydana gelmemesi gereken bir program hatasının sonucu olarak ortaya çıkar.

### 🤯 Nasıl Refactor Edilir?

1. Bir koşulun varsayıldığını gördüğünüzde emin olmak için bu koşula bir assertion ekleyin.

2. İddiayı eklemek programın davranışını değiştirmemelidir.

3. Kodunuzdaki her şey için assertionlar kullanarak aşırıya kaçmayın. Yalnızca kodun doğru çalışması için gerekli olan koşulları kontrol edin. Belirli bir assertion yanlış olsa bile kodunuz normal şekilde çalışıyorsa, assertionu güvenle kaldırabilirsiniz.

