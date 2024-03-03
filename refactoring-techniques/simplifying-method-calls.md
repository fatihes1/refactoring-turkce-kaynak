# Yöntem Çağrılarını Basitleştirme (Simplifying Method Calls)

Bu teknikler yöntem çağrılarını daha basit ve anlaşılır hale getirir. Bu da sınıflar arasındaki etkileşim için arayüzleri basitleştirir.

## Rename Method

### 🙁 Problem

Bir yöntemin adı, yöntemin ne yaptığını açıklamaması durumunda, koda bakan bir geliştirici işlevin ne yaptığını anlamak için vakit kaybedecektir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Rename%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Yöntemi, yöntemin amacına uygun şekilde yeniden adlandırın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Rename%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Belki de bir yöntem ilk oluşturulduğunda kötü bir şekilde adlandırılmış olabilir; örneğin, birisi yöntemi aceleyle oluşturmuş ve onu iyi adlandırmaya gereken özeni göstermemiştir.

Diğer bir ihtimal ise, belki de yöntem ilk başta iyi adlandırılmıştı ancak işlevselliği arttıkça yöntem adı, yöntemin amacına uygun bir tanımlayıcı olmaktan çıkmış olabilir.

### 🤯 Nasıl Refactor Edilir?

1. Yöntem, bir üst sınıfta mı yoksa bir alt sınıfta mı tanımlandığına karar verin. Eğer bu iki durumdan biri var ise, burada listelenen tüm adımları diğer sınıflar için de tekrarlamanız gerekmektedir.

2. Bu madde, refactoring işlemi sırasında programın işlevselliğini korumak için önemlidir. Yeni bir adla yeni bir yöntem oluşturun. Eski yöntemin kodunu bu oluşturduğunuz yönteme kopyalayın. Eski yöntemdeki tüm kodu silin ve bunun yerine yeni yöntem için bir method çağrısı (method call) ekleyin.

3. Eski yönteme yapılan tüm referansları bulun ve bunları yeni oluşturduğunuz yönteme yapılan referanslarla değiştirin.

4. Eski yöntemi silin. Eski yöntem public bir arayüzün (interface) parçasıysa bu adımı gerçekleştirmeyin. Bunun yerine eski yöntemi kullanımdan kaldırılmış (deprecated) olarak işaretleyin.

## Add Parameter

### 🙁 Problem

Bir yöntemin hedeflenen amacı gerçekleştirmek için yeterli veriye sahip olmaması sorun oluşturur.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Add%20Parameter%20-%20Before.png)
</div>

### 😊 Çözüm

Gerekli verileri yönteme iletmek için yeni bir parametre daha oluşturun.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Add%20Parameter%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Gelen yeni talepler doğrusultusunda, bir yöntemde değişiklik yapmanız gerekebilir. Bu değişiklikler sırasında kullanmanız gereken veri, daha önce yöntemde bulunmayan bilgi veya verilerin eklenmesini gerektirecektir.


### ✅ Avantajları

Buradaki seçim, yönteme yeni bir parametre eklemek ile yöntemin ihtiyaç duyduğu verileri içeren yeni bir prive field eklemek arasındadır. Her zaman bir nesnede tutmanın bir anlamı olmayan, ara sıra veya sık sık değişen verilere ihtiyaç duyduğunuzda bir parametre tercih edilmesi daha mantıklı olacaktır. Bu durumda, refactoring işe yarayacaktır. Aksi takdirde, private bir field ekleyin ve yöntemi çağırmadan önce bu field'a gerekli değer atamasını yaptığınızdan emin olun.

### 🚫 Dezavantajları

- Yeni bir parametre eklemek her zaman var olan parametreyi kaldırmaktan daha kolaydır; bu nedenle parametre listeleri sıklıkla çok yüksek boyutlara ulaşır. Bu kod koku Uzun Parametre Listesi (**Long Parameter List**) olarak bilinir.

- Yeni bir parametre eklemeniz gerekiyorsa bu bazen sınıfınızın gerekli verileri içermediği veya mevcut parametrelerin ilgili verileri içermediği anlamına gelir. Her iki durumda da en iyi çözüm, verileri ana sınıfa veya nesnelerine yöntemin içinden zaten erişilebilen diğer sınıflara taşımayı düşünmek olakactır.

### 🤯 Nasıl Refactor Edilir?

1. Yöntem, bir üst sınıfta mı yoksa bir alt sınıfta mı tanımlandığına karar verin. Eğer bu iki durumdan biri var ise, burada listelenen tüm adımları diğer sınıflar için de tekrarlamanız gerekmektedir.

2. Bu ve aşağıdaki adım, refactoring işlemi sırasında programınızı işlevsel tutmak için kritik öneme sahiptir. Eski yöntemi kopyalayarak yeni bir yöntem oluşturun. Sonrasında, oluşturmuş olduğunuz yönteme gerekli parametreyi ekleyin. Eski yöntemin kod body'sini yeni yöntemin çağrısıyla değiştirin. Yeni parametreye bir varsayılan (default) bir değeri ekleyebilirsiniz.

3. Eski yönteme yapılan tüm referansları bulun ve bunları yeni oluşturulan yönteme yapılan referanslarla değiştirin.

4. Eski yöntemi silin. Eski yöntem public bir arayüzün (interface) parçasıysa bu adımı gerçekleştirmeyin. Bunun yerine eski yöntemi kullanımdan kaldırılmış (deprecated) olarak işaretleyin.


## Remove Parameter

### 🙁 Problem

Bir yöntemin gövdesinde, yöntemin parametrelerinden birinin kullanılmaması bir sorundur. Geliştici, parametre listesi ve yöntem gövdesi arasında bağlantıyı kurmaya çalışırken vakit kaybedebilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Parameter%20-%20Before.png)
</div>

### 😊 Çözüm

Kullanılmayan parametreyi yöntemden kaldırın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Parameter%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir yöntem çağrısındaki her parametre, onu okuyan programcıyı bu parametrede hangi bilgilerin bulunduğunu ve ne için kullanıldığını bulmaya zorlar. Ve eğer bir parametre yöntem gövdesinde kullanılmıyorsa, bu durumda geliştiricinin kafasının karışması boşunadır.

Ve her durumda, ek parametreler çalıştırılması gereken ekstra kodlardır. Bir parametre belrtildiye, bu metod gövdesinde kullanılmalıdır.

Bazen parametreye ihtiyaç duyulabilecek yöntemde yapılacak değişiklikleri öngörerek geleceğe yönelik parametreler ekleyebiliriz. Yine de deneyimler, bir parametreyi yalnızca gerçekten ihtiyaç duyulduğunda eklemenin daha iyi olduğunu göstermektedir. Sonuçta, beklenen değişiklikler genellikle sadece beklenen olarak kalabilir. Veya, çok uzak bir gelecekte hayata geçebilir.

### ✅ Avantajları

Bir yöntem yalnızca gerçekten ihtiyaç duyduğu parametreleri içermelidir.

### 🖐🏼 Ne Zaman Kullanılmamalı?

Yöntem alt sınıflarda veya üst sınıfta farklı şekillerde kullanılıyorsa, ayrıca parametreniz bu kullanımlarda aktif rol alıyorsa parametreyi olduğu gibi bırakın.

### 🤯 Nasıl Refactor Edilir?

1. Yöntem, bir üst sınıfta mı yoksa bir alt sınıfta mı tanımlandığına karar verin. Eğer bu iki durumdan biri var ise, burada listelenen tüm adımları diğer sınıflar için de tekrarlamanız gerekmektedir.

2. Bir sonraki adım, yeniden düzenleme işlemi sırasında programın işlevsel kalması açısından önemlidir. Eskisini kopyalayarak yeni bir yöntem oluşturun ve ilgili parametreyi oluşturduğunuz yöntemden silin. Eski yöntemin kodunu yenisine yapılan bir çağrıyla değiştirin.

3. Eski yönteme yapılan tüm referansları bulun ve bunları yeni yönteme yapılan referanslarla değiştirin.

4. Eski yöntemi silin. Eski yöntem public bir arayüzün (interface) parçasıysa bu adımı gerçekleştirmeyin. Bunun yerine eski yöntemi kullanımdan kaldırılmış (deprecated) olarak işaretleyin.


## Separate Query from Modifier

### 🙁 Problem

Değer döndüren ama aynı zamanda nesnenin içindeki bir şeyi değiştiren bir yönteminiz var mı? Bir yöntemin iki işlemi aynı anda yapması sorun oluşturabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Separate%20Query%20from%20Modifier%20-%20Before.png)
</div>

### 😊 Çözüm

Yönteminizi iki ayrı yönteme bölün. Tahmin edebileceğiniz gibi, bu yöntemlerden biri değeri döndürmesi, diğer yöntem ise nesneyi değiştirmesi gerekiyor.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Separate%20Query%20from%20Modifier%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?
Bu faktoring tekniği, Komut ve Sorgu Sorumluluğu Ayrımını (Command and Query Responsibility Segregation) referans alır. Bu prensip bize, veri almaktan sorumlu kodu, bir nesnenin içindeki bir şeyi değiştiren koddan ayırmamızı önerir.

Veri almak için kullanılan koda sorgu (query) adı verilir. Bir nesnenin görünür durumundaki şeyleri değiştirmeye yarayan koda değiştirici (modifier) adı verilir. Bir sorgu ve değiştirici birleştirildiğinde, state üzerinde değişiklik yapmadan veri almanın bir yolu yoktur. Başka bir deyişle, bir soru sorarsınız ve yanıtı alınırken bile state değiştirmiş olabilirsiniz. Sorguyu çağıran kişi yöntemin yan etkilerini bilmediğinde bu sorun daha da ciddi hale gelir ve bu da genellikle çalışma zamanı hatalarına yol açar.

Ancak yan etkilerin yalnızca bir nesnenin görünür durumunu değiştiren değiştiriciler olması durumunda tehlikeli olduğunu unutmayın. Bunlar, örneğin bir nesnenin genel arayüzünden erişilebilen alanlar, bir veritabanındaki giriş, dosyalar vb. olabilir. Eğer bir değiştirici yalnızca karmaşık bir işlemi önbelleğe alır ve bunu bir sınıfın özel alanı içine kaydederse, bu durum neredeyse hiç bir yan etki yaratmaz. Etkileri.

### ✅ Avantajları

Programınızın durumunu değiştirmeyen bir sorgunuz varsa, yalnızca yöntemi çağırmanızın neden olduğu sonuçtaki istenmeyen değişiklikler konusunda endişelenmenize gerek kalmadan, onu istediğiniz kadar çağırabilirsiniz.

### 🚫 Dezavantajları

Bazı durumlarda bir komutu gerçekleştirdikten sonra veri almak daha uygun olur. Örneğin, bir veritabanından bir şey silerken kaç satırın silindiğini bilmek istersiniz.

### 🤯 Nasıl Refactor Edilir?

1. Orijinal yöntemin yaptığını döndürmek için yeni bir sorgu yöntemi oluşturun.

2. Orijinal yöntemi, yalnızca yeni sorgu yönteminin çağrılmasının sonucunu döndürecek şekilde değiştirin.

3. Orijinal yönteme yapılan tüm referansları sorgu yöntemine yapılan bir çağrıyla değiştirin. Bu satırın hemen öncesinde değiştirici yönteme bir çağrı yapın. Bu, orijinal yöntemin koşullu bir operatör veya döngü durumunda kullanılması durumunda sizi yan etkilerden kurtaracaktır.

4. Artık uygun bir değiştirici yöntem haline gelen orijinal yöntemdeki değer döndüren koddan kurtulun.


## Parameterize Method

### 🙁 Problem

Birden çok yöntem, yalnızca iç değerleri, sayıları veya işlemleri açısından farklı olan benzer eylemleri gerçekleştirir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Parameterize%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Gerekli özel değeri iletecek bir parametre kullanarak bu yöntemleri birleştirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Parameterize%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Benzer yöntemleriniz varsa, muhtemelen bunun gerektirdiği tüm sonuçlarla birlikte yinelenen kodunuz vardır.

Üstelik bu işlevselliğin başka bir versiyonunu eklemeniz gerekiyorsa başka bir yöntem daha oluşturmanız gerekecektir. Bunun yerine mevcut yöntemi farklı bir parametreyle çalıştırabilirsiniz.

### 🚫 Dezavantajları

- Bazen bu yeniden düzenleme tekniği çok ileri götürülebilir ve birden fazla basit yöntem yerine uzun ve karmaşık bir ortak yöntem ortaya çıkabilir.

- Ayrıca işlevselliğin etkinleştirilmesini/devre dışı bırakılmasını bir parametreye taşırken dikkatli olun. Bu, sonuçta **Replace Parameter with Explicit Methods** tekniği aracılığıyla ele alınması gereken büyük bir koşullu operatörün oluşturulmasına yol açabilir.

### 🤯 Nasıl Refactor Edilir?

1. Bir parametre ile yeni bir yöntem oluşturun ve **Extract Method** yöntemi uygulayarak onu tüm sınıflar için aynı olan koda taşıyın. Bazen yöntemlerin yalnızca belirli bir kısmının aslında aynı olduğunu unutmayın. Bu durumda yeniden düzenleme, yalnızca aynı parçanın yeni bir yönteme çıkarılmasından oluşur.

2. Yeni yöntemin kodunda özel/farklı değeri bir parametre ile değiştirin.

3. Her eski yöntem için, çağrıldığı yerleri bulun ve bu çağrıları, parametre içeren yeni yönteme yapılan çağrılarla değiştirin. Daha sonra eski yöntemi silin.

## Replace Parameter with Explicit Methods

### 🙁 Problem

Bir yöntem, her biri bir parametrenin değerine bağlı olarak çalıştırılan parçalara bölünür.

```java
void setValue(String name, int value) {
  if (name.equals("height")) {
    height = value;
    return;
  }
  if (name.equals("width")) {
    width = value;
    return;
  }
  Assert.shouldNeverReachHere();
}
```

### 😊 Çözüm

Yöntemin ayrı bölümlerini kendi yöntemlerine çıkarın ve bunları orijinal yöntem yerine çağırın.

```java
void setHeight(int arg) {
  height = arg;
}
void setWidth(int arg) {
  width = arg;
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Parametreye bağlı değişkenler içeren bir yöntem çok büyük bir hızla büyüdü. Her dalda önemsiz olmayan kod çalıştırılır ve çok nadiren yeni varyantlar eklenir.

### ✅ Avantajları

Kodun okunabilirliğini artırır. startEngine() işlevinin amacını anlamak setValue("engineEnabled", true) yönteminden çok daha kolaydır.

### 🖐🏼 Ne Zaman Kullanılmamalı?

Bir yöntem nadiren değiştiriliyorsa ve içine yeni değişkenler eklenmemişse, parametreyi açık yöntemlerle değiştirmeyin.

### 🤯 Nasıl Refactor Edilir?

1. Yöntemin her çeşidi için ayrı bir yöntem oluşturun. Bu yöntemleri, ana yöntemdeki bir parametrenin değerine göre çalıştırın.
2. Orijinal yöntemin çağrıldığı tüm yerleri bulun. Bu yerlerde parametreye bağlı yeni değişkenlerden biri için çağrı yapın.

3. Orijinal yönteme çağrı kalmadığında onu silin.


## Preserve Whole Object

### 🙁 Problem

Bir nesneden birkaç değer alırsınız ve bunları parametre olarak bir yönteme aktarırsınız.

```java
int low = daysTempRange.getLow();
int high = daysTempRange.getHigh();
boolean withinPlan = plan.withinRange(low, high);
```

### 😊 Çözüm

Bunun yerine nesnenin tamamını aktarmayı deneyin.

```java
boolean withinPlan = plan.withinRange(daysTempRange);
```

### 🤔 Neden Refactoring Uygulanmalı?

Sorun şu ki, yönteminiz her çağrılmadan önce, gelecekteki parametre nesnesinin yöntemlerinin çağrılması gerekir. Bu yöntemler veya yöntem için elde edilen veri miktarı değiştirilirse, programda bu tür bir düzine yeri dikkatlice bulmanız ve bu değişiklikleri her birinde uygulamanız gerekecektir.

Bu yeniden düzenleme tekniğini uyguladıktan sonra, gerekli tüm verileri elde etmek için gereken kod tek bir yerde, yani yöntemin kendisinde depolanacaktır.

### ✅ Avantajları
- Karmaşık parametreler yerine anlaşılır bir isme sahip tek bir nesne görüyorsunuz.

- Yöntemin bir nesneden daha fazla veriye ihtiyacı varsa, yöntemin kullanıldığı tüm yerleri yeniden yazmanız gerekmez; yalnızca yöntemin kendi içinde yazmanız gerekir.

### 🚫 Dezavantajları

Bazen bu dönüşüm bir yöntemin daha az esnek hale gelmesine neden olur: önceden yöntem birçok farklı kaynaktan veri alabiliyordu ancak şimdi yeniden düzenleme nedeniyle kullanımını yalnızca belirli bir arayüze sahip nesnelerle sınırlıyoruz.

### 🤯 Nasıl Refactor Edilir?

1. Gerekli değerleri alabileceğiniz nesne için yöntemde bir parametre oluşturun.

2. Şimdi eski parametreleri yöntemden birer birer kaldırmaya başlayın ve bunları parametre nesnesinin ilgili yöntemlerine yapılan çağrılarla değiştirin. Bir parametrenin her değiştirilmesinden sonra programı test edin.

3. Yöntem çağrısından önce gelen parametre nesnesinden alıcı kodunu silin.


## Replace Parameter with Method Call

### 🙁 Problem

Bir sorgu yöntemini çağırmak ve sonuçlarını başka bir yöntemin parametreleri olarak iletmek, bu yöntem ise sorguyu doğrudan çağırabilir.

```java
int basePrice = quantity * itemPrice;
double seasonDiscount = this.getSeasonalDiscount();
double fees = this.getFees();
double finalPrice = discountedPrice(basePrice, seasonDiscount, fees);
```

### 😊 Çözüm

Değeri bir parametre aracılığıyla iletmek yerine, yöntem gövdesinin içine bir sorgu çağrısı yerleştirmeyi deneyin.

```java
int basePrice = quantity * itemPrice;
double finalPrice = discountedPrice(basePrice);
```

### 🤔 Neden Refactoring Uygulanmalı?

Uzun bir parametre listesini anlamak zordur. Buna ek olarak, bu tür yöntemlere yapılan çağrılar genellikle bir dizi kademeye benzer; gezinmesi zor olan ancak yönteme aktarılması gereken dolambaçlı ve canlandırıcı değer hesaplamaları vardır. Yani eğer bir parametre değeri bir yöntem yardımıyla hesaplanabiliyorsa bunu yöntemin kendi içinde yapın ve parametreden kurtulun.

### ✅ Avantajları

Gereksiz parametrelerden kurtuluyoruz ve yöntem çağrılarını basitleştiriyoruz. Bu tür parametreler genellikle şu anki proje için değil, asla gelmeyecek gelecekteki ihtiyaçlar dikkate alınarak oluşturulur.

### 🚫 Dezavantajları

Yöntemi yeniden yazmanıza neden olacak diğer ihtiyaçlar için yarın parametreye ihtiyacınız olabilir.

### 🤯 Nasıl Refactor Edilir?

1. Değer elde eden kodun geçerli yöntemdeki parametreleri kullanmadığından emin olun çünkü bu parametreler başka bir yöntemin içinden kullanılamayacaktır. Bu durumda kodun taşınması mümkün değildir.

2. İlgili kod tek bir yöntem veya işlev çağrısından daha karmaşıksa, bu kodu yeni bir yöntemde yalıtmak ve çağrıyı basitleştirmek için **Extract Method** yöntemini kullanın.

3. Ana yöntemin kodunda, değiştirilen parametreye yapılan tüm referansları, değeri alan yönteme yapılan çağrılarla değiştirin.

4. Artık kullanılmayan parametreyi ortadan kaldırmak için **Remove Parameter** tekniğini kullanın.


## Introduce Parameter Object

### 🙁 Problem

Yöntemleriniz yinelenen bir parametre grubu içeriyor.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Parameter%20Object%20-%20Before.png)
</div>

### 😊 Çözüm

Bu parametreleri bir nesneyle değiştirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Parameter%20Object%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Birden fazla yöntemde sıklıkla aynı parametre gruplarıyla karşılaşılır. Bu, hem parametrelerin hem de ilgili işlemlerin kod çoğaltılmasına neden olur. Parametreleri tek bir sınıfta birleştirerek, bu verileri işlemeye yönelik yöntemleri de oraya taşıyabilir ve diğer yöntemleri bu koddan kurtarabilirsiniz.

### ✅ Avantajları

- Daha okunabilir kod. Karmaşık parametreler yerine anlaşılır bir isme sahip tek bir nesne görüyorsunuz.

- Oraya buraya dağılmış aynı parametre grupları, kendi türlerinde kod çoğaltmaları yaratır: Aynı kod çağrılmazken, sürekli olarak aynı parametre ve argüman gruplarıyla karşılaşılır.

### 🚫 Dezavantajları

Yalnızca verileri yeni bir sınıfa taşırsanız ve herhangi bir davranışı veya ilgili işlemi oraya taşımayı planlamıyorsanız, bu bir **Data Class** kokusu almaya başlar.

### 🤯 Nasıl Refactor Edilir?

1. Parametre grubunuzu temsil edecek yeni bir sınıf oluşturun. Sınıfı değişmez hale getirin.

2. Yeniden düzenlemek istediğiniz yöntemde, parametre nesnenizin aktarılacağı **Add Parameter** tekniğini kullanın. Tüm yöntem çağrılarında eski yöntem parametrelerinden oluşturulan nesneyi bu parametreye iletin.

3. Şimdi eski parametreleri yöntemden birer birer silmeye başlayın ve bunları kodda parametre nesnesinin alanlarıyla değiştirin. Her parametre değişiminden sonra programı test edin.

4. İşiniz bittiğinde, yöntemin bir bölümünü (hatta bazen yöntemin tamamını) bir parametre nesne sınıfına taşımanın herhangi bir anlamı olup olmadığına bakın. Eğer öyleyse, **Move Method** veya **Extract Method** tekniğini kullanın.


## Remove Setting Method

### 🙁 Problem

Bir alanın değeri yalnızca oluşturulduğunda ayarlanmalı ve daha sonra hiçbir zaman değiştirilmemelidir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Setting%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Bu nedenle alanın değerini ayarlayan yöntemleri kaldırın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Setting%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Bir alanın değerinde herhangi bir değişiklik yapılmasını önlemek istiyorsunuz.

### 🤯 Nasıl Refactor Edilir?

1. Bir alanın değeri yalnızca yapıcıda değiştirilebilir olmalıdır. Yapıcı değeri ayarlamak için bir parametre içermiyorsa bir tane ekleyin.

2. Tüm ayarlayıcı (`setter`) aramalarını bulun.

	- Geçerli sınıfın yapıcısına (`constructor`) yönelik bir çağrının hemen ardından bir ayarlayıcı (`setter`) çağrısı bulunursa, argümanını yapıcı (`constructor`) çağrısına taşıyın ve ayarlayıcıyı kaldırın.

	- Yapıcıdaki (`constructor`) ayarlayıcı (`setter`) çağrılarını, alana doğrudan erişimle değiştirin.

3. Ayarlayıcıyı (`setter`) silin.

## Hide Method

### 🙁 Problem

Bir yöntem diğer sınıflar tarafından kullanılmaz veya yalnızca kendi sınıf hiyerarşisi içinde kullanılır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Hide%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Yöntemi özel veya korumalı yapın.
<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Hide%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Çoğu zaman, değerleri alma ve ayarlamaya yönelik yöntemleri gizleme ihtiyacı, özellikle yalnızca veri kapsüllemenin ötesinde çok az şey ekleyen bir sınıfla başladıysanız, ek davranış sağlayan daha zengin bir arayüzün geliştirilmesinden kaynaklanmaktadır.

Sınıfta yeni davranışlar yerleşik hale geldikçe, genel alıcı ve ayarlayıcı yöntemlerin artık gerekli olmadığını ve gizlenebileceğini görebilirsiniz. Alıcı veya ayarlayıcı yöntemlerini özel yaparsanız ve değişkenlere doğrudan erişim uygularsanız yöntemi silebilirsiniz.

### ✅ Avantajları

- Yöntemleri gizlemek kodunuzun gelişmesini kolaylaştırır. Özel bir yöntemi değiştirdiğinizde, yöntemin başka hiçbir yerde kullanılamayacağını bildiğiniz için yalnızca mevcut sınıfın nasıl bozulmayacağı konusunda endişelenmeniz gerekir.

- Yöntemleri private yaparak sınıfın genel arayüzünün ve genel olarak kalan yöntemlerin önemini vurgulamış olursunuz.

### 🤯 Nasıl Refactor Edilir?

1. Düzenli olarak özel hale getirilebilecek yöntemleri bulmaya çalışın. Statik kod analizi ve iyi birim testi kapsamı büyük bir avantaj sağlayabilir.

2. Her yöntemi mümkün olduğunca private yapın.

## Replace Constructor with Factory Method

### 🙁 Problem

Nesne alanlarındaki parametre değerlerini ayarlamaktan daha fazlasını yapan karmaşık bir kurucunuz var.

```java
class Employee {
  Employee(int type) {
    this.type = type;
  }
  // ...
}
```

### 😊 Çözüm

Bir fabrika yöntemi oluşturun ve bunu yapıcı çağrılarını değiştirmek için kullanın.

```java
class Employee {
  static Employee create(int type) {
    employee = new Employee(type);
    // do some heavy lifting.
    return employee;
  }
  // ...
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Bu yeniden düzenleme tekniğini kullanmanın en belirgin nedeni **Replace Type Code with Subclasses** tekniği ile ilgilidir.

Daha önce bir nesnenin oluşturulduğu ve kodlanan türün değerinin ona aktarıldığı kodunuz var. Yeniden düzenleme yöntemini kullandıktan sonra birkaç alt sınıf ortaya çıktı ve kodlanan türün değerine bağlı olarak bunlardan nesneler oluşturmanız gerekiyor. Orijinal kurucuyu, alt sınıf nesnelerini döndürecek şekilde değiştirmek imkansızdır; bunun yerine, gerekli sınıfların nesnelerini döndürecek ve ardından orijinal kurucuya yapılan tüm çağrıların yerini alacak statik bir fabrika yöntemi yaratırız.

Fabrika yöntemleri, inşaatçıların göreve uygun olmadığı diğer durumlarda da kullanılabilir. **Change Value to Reference** tekniği kullanırken önemli olabilirler. Ayrıca parametre sayısı ve türünün ötesine geçen çeşitli oluşturma modlarını ayarlamak için de kullanılabilirler.

### ✅ Avantajları

- Bir fabrika yöntemi mutlaka çağrıldığı sınıfın bir nesnesini döndürmez. Genellikle bunlar, yönteme verilen argümanlara göre seçilen alt sınıflar olabilir.

- Bir fabrika yönteminin neyi ve ne yaptığını nasıl döndürdüğünü açıklayan daha iyi bir adı olabilir; örneğin `Troops::GetCrew(myTank)`.

- Her zaman yeni bir örnek oluşturan yapıcının aksine, fabrika yöntemi önceden oluşturulmuş bir nesneyi döndürebilir.


### 🤯 Nasıl Refactor Edilir?

1. Bir fabrika yöntemi oluşturun. Mevcut kurucuya bir çağrı yapın.

2. Tüm yapıcı çağrılarını fabrika yöntemine yapılan çağrılarla değiştirin.

3. Yapıcıyı private olarak bildirin.

4. Yapıcı kodunu araştırın ve geçerli sınıftan bir nesnenin oluşturulmasıyla doğrudan ilgili olmayan kodu izole etmeye çalışın ve bu kodu fabrika yöntemine taşıyın.

## Replace Error Code with Exception

### 🙁 Problem

Bir yöntem, hatayı belirten özel bir değer mi döndürüyor?

```java
int withdraw(int amount) {
  if (amount > _balance) {
    return -1;
  }
  else {
    balance -= amount;
    return 0;
  }
}
```

### 😊 Çözüm

Bunun yerine bir hata atın.

```java
void withdraw(int amount) throws BalanceException {
  if (amount > _balance) {
    throw new BalanceException();
  }
  balance -= amount;
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Hata kodlarının döndürülmesi, prosedürel programlamanın artık kullanılmayan bir özelliğidir. Modern programlamada hata işleme, istisna adı verilen özel sınıflar tarafından gerçekleştirilir. Bir sorun ortaya çıkarsa, bir hata "atarsınız" ve bu daha sonra istisna işleyicilerden biri tarafından "yakalanır". Normal şartlarda göz ardı edilen özel hata işleme kodu devreye girerek yanıt verir.

### ✅ Avantajları

- Çeşitli hata kodlarını kontrol etmek için kodu çok sayıda koşul koşulundan kurtarır. İstisna işleyicileri, normal yürütme yollarını anormal olanlardan ayırmanın çok daha kısa ve öz bir yoludur.

- İstisna sınıfları kendi yöntemlerini uygulayabilir, dolayısıyla hata işleme işlevselliğinin bir kısmını (hata mesajları göndermek gibi) içerir.

- İstisnalardan farklı olarak, bir yapıcının yalnızca yeni bir nesne döndürmesi gerektiğinden hata kodları bir yapıcıda kullanılamaz.

### 🚫 Dezavantajları

Bir hata işleyicisi goto benzeri bir koltuk değneğine dönüşebilir. Bundan kaçının! Kod yürütmeyi yönetmek için istisnalar kullanmayın. İstisnalar yalnızca bir hata veya kritik durum hakkında bilgi vermek için atılmalıdır.

### 🤯 Nasıl Refactor Edilir?

Bu refactoring adımlarını aynı anda yalnızca bir hata kodu için gerçekleştirmeye çalışın. Bu, tüm önemli bilgileri kafanızda tutmanızı ve hataları önlemenizi kolaylaştıracaktır.

1. Hata kodları döndüren bir yönteme yapılan tüm çağrıları bulun ve hata kodunu kontrol etmek yerine onu `try/catch` bloklarıyla sarın.

2. Yöntemin içinde bir hata kodu döndürmek yerine bir istisna atın.

3. Yöntem imzasını, atılan istisna hakkında (`@throws` bölümü) bilgi içerecek şekilde değiştirin.


## Replace Exception with Test

### 🙁 Problem

Basit bir testin işi yapabileceği bir yere hata mı atıyorsunuz?

```java
double getValueForPeriod(int periodNumber) {
  try {
    return values[periodNumber];
  } catch (ArrayIndexOutOfBoundsException e) {
    return 0;
  }
}
```

### 😊 Çözüm

İstisnayı bir durum testiyle değiştirin.

```java
double getValueForPeriod(int periodNumber) {
  if (periodNumber >= values.length) {
    return 0;
  }
  return values[periodNumber];
}
```

### 🤔 Neden Refactoring Uygulanmalı?

Beklenmeyen bir hatayla ilgili düzensiz davranışları ele almak için istisnalar kullanılmalıdır. Testin yerine geçmemelidirler. Çalıştırmadan önce bir koşulun doğrulanmasıyla bir istisnadan kaçınılabiliyorsa, bunu yapın. Gerçek hatalara istisnalar ayrılmalıdır.

Mesela bir mayın tarlasına girdiniz ve orada mayını tetiklediniz, sonuçta bir istisna oluştu; istisna başarılı bir şekilde ele alındı ​​​​ve siz, mayın tarlasının ötesindeki güvenli bir yere havaya kaldırıldınız. Ancak başlangıçta mayın tarlasının önündeki uyarı tabelasını okuyarak tüm bunlardan kaçınabilirdiniz.

### ✅ Avantajları

Basit bir koşul bazen istisna işleme kodundan daha belirgin olabilir.

### 🤯 Nasıl Refactor Edilir?

1. Uç durumu için bir koşul oluşturun ve onu `try/catch` bloğunun önüne taşıyın.

2. Kodu bu koşulun içindeki `catch` bölümünden taşıyın.

3. `catch` bölümünde, olağan isimsiz bir istisnayı atmak için gereken kodu yerleştirin ve tüm testleri çalıştırın.

4. Testler sırasında herhangi bir istisna oluşturulmadıysa `try/catch` operatöründen kurtulun.
