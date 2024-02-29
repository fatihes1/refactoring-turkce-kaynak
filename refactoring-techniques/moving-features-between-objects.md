# Özellikleri Nesneler Arasında Taşıma (Moving Features between Objects)

İşlevselliği farklı sınıflar arasında ideal olmayan bir şekilde dağıtmış olsanız bile hala umut var.

Bu yeniden düzenleme teknikleri, işlevselliğin sınıflar arasında güvenli bir şekilde nasıl taşınacağını, yeni sınıfların nasıl oluşturulacağını ve uygulama ayrıntılarının genel erişimden nasıl gizleneceğini gösterir.

## Move Method

### 🙁 Problem

Bir yöntem başka bir sınıfta kendi sınıfından daha fazla kullanması sorun oluşturabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Move%20Method%20-%20Before.png)
</div>

### 😊 Çözüm

Yöntemi en çok kullanan sınıfta, yeni bir yöntem oluşturun. Ardından kodu eski yöntemden oluşturduğunuz yönteme taşıyın. Orijinal yöntemin kodunu diğer sınıftaki yeni yönteme referans olacak şekilde güncelleyin veya tamamen kaldırın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Move%20Method%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

- Bir yöntemi, yöntem tarafından kullanılan verilerin çoğunu içeren bir sınıfa taşıyabilirsiniz. Bu, sınıfları kendi içinde daha tutarlı hale getirir.

- Yöntemi çağıran sınıfın, bulunduğu sınıfa bağımlılığını azaltmak veya ortadan kaldırmak için bir yöntemi taşıyabilirsiniz. Çağıran sınıf zaten yöntemi taşımayı planladığınız sınıfa bağımlıysa bu yararlı olabilir. Bu yöntem, sınıflar arasındaki bağımlılığı azaltır.

### 🤯 Nasıl Refactor Edilir?

1. Kendi sınıfında eski yöntemin kullandığı tüm özellikleri taşıdığını doğrulayın. Bunları taşımak da iyi bir fikir olabilir. Kural olarak, eğer bir özellik yalnızca söz konusu yöntem tarafından kullanılıyorsa, özelliği mutlaka yöntemle birlikte taşımalısınız. Özellik başka yöntemler tarafından da kullanılıyorsa bu yöntemleri de taşımalısınız. Bazen çok sayıda yöntemi taşımak, farklı sınıflarla aralarında ilişkiler kurmaktan çok daha kolaydır.

Yöntemin üst sınıflarda ve alt sınıflarda bildirilmediğinden yani kullanılmadığından emin olun. Bu durumda, donör sınıflar arasında bölünmüş bir yöntemin değişen işlevselliğini sağlamak için ya taşımaktan kaçınmanız ya da alıcı sınıfta bir tür çok biçimlilik (polymorphism) uygulamanız gerekecektir.

2. Alıcı sınıfında yeni yöntemi tanımlayın. Yeni sınıfta metoda daha uygun olan yeni bir isim vermeyi de göz önünde bulundurabilirsiniz.

3. Alıcı sınıfına nasıl referans vereceğinize karar verin. Uygun bir nesneyi döndüren bir alanınız veya yönteminiz zaten olabilir, ancak yoksa alıcı sınıfın nesnesini depolamak için yeni bir yöntem veya alan oluşturmanız gerekecektir.

Artık alıcı nesneye ve onun sınıfındaki yeni bir yönteme referans vermenin bir yoluna sahipsiniz. Tüm bunları elinizin altında tutarak eski yöntemi yeni yöntemin referansına dönüştürebilirsiniz.

4. Bir göz atın: eski yöntemi tamamen silebilir misiniz? Eğer öyleyse, eski yöntemin kullanıldığı her yere yeni yönteme bir referans oluşturun.

## Move Field

### 🙁 Problem

Bir alan (field) kendi sınıfından ziyade başka bir sınıfta daha fazla kullanılıyorsa sorun oluşturabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Move%20Field%20-%20Before.png)
</div>

### 😊 Çözüm

Yeni bir sınıfta bir alan (field) oluşturun ve eski alanın tüm kullanımlarını yeni oluşturulan field'a yönlendirin.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Move%20Field%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Genellikle alanlar (fields), **Extract Class** tekniğinin bir parçası olarak taşınır. Alanı hangi sınıfta bırakacağınıza karar vermek zor olabilir. Temel kuralımız şu: Alanı, onu kullanan yöntemlerle aynı yere veya bu yöntemlerin çoğunun bulunduğu yere konumlandırmak daha sağlıklı olacaktır.

Bu kural, alanın yanlış yere yerleştirildiği diğer durumlarda kurtarıcınız olacaktır.

### 🤯 Nasıl Refactor Edilir?

1. Eğer alan public ise, alanı private yapıp public erişim yöntemleri sağlarsanız (bunun için **Encapsulate Field** tekniğini kullanabilirsiniz) refactoring çok daha kolay olacaktır.

2. Alıcı sınıfında erişim yöntemleriyle aynı yeni bir alanı oluşturun.

3. Alıcı sınıfına nasıl erişeceğinize karar verin. Uygun nesneyi döndüren bir alanınız veya yönteminiz zaten olabilir; değilse, alıcı sınıfın nesnesini depolamak için yeni bir yöntem veya alan oluşturmanız gerekecektir.

4. Eski alana yapılan tüm referansları, alıcı sınıfındaki yöntemlere yapılan uygun çağrılarla değiştirin. Alan private değilse üst sınıfta ve alt sınıflarda bu konuyla ilgilenin.

5. Orijinal sınıftaki alanı silin.

## Extract Class

### 🙁 Problem

Bir sınıf iki sınıfın işini aynı anda yapmaya çalıştığında sorun ortaya çıkabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Class%20-%20Before.png)
</div>

### 😊 Çözüm

Yeni bir sınıf oluşturun ve ilgili işlevsellikten sorumlu alanları ve yöntemleri yeni oluşturduğunuz bu sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Extract%20Class%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Sınıflar her zaman net ve anlaşılması kolay başlar. Diğer sınıfların çalışmalarına karışmadan işlerini yaparlar ve kendi işleriyle ilgilenirler. Ancak program genişledikçe, birer birer yöntemler ve alanlar (fields) eklenir... ve sonunda bazı sınıflar, her zamankinden daha fazla sorumluluk yerine getirir. Hatta bazen birkaç sınıfın işini tek bir sınıfın sorumluluğuna bırakmış olabiliriz.

### ✅ Avantajları

- Bu refactoring yöntemiyle, Tek Sorumluluk İlkesine (Single Responsibility Principle) bağlılığın korunmasına yardımcı olacaktır. Sınıflarınız kodları daha açık ve anlaşılır hale gelecektir.

- Tek sorumluluk sınıfları daha güvenilirdir ve değişikliklere karşı daha dayanıklıdır. Örneğin on farklı şeyden sorumlu bir sınıfınız olduğunu varsayalım. Bu sınıfı bir şeyi daha iyi hale getirmek için değiştirdiğinizde, diğer dokuz konuda onu bozma riskiyle karşı karşıya kalırsınız.

### 🚫 Dezavantajları

Bu refactoring tekniğini kullanırken aşırıya kaçarsanız **Inline Class** yöntemine başvurmak zorunda kalabilirsiniz.

### 🤯 Nasıl Refactor Edilir?

Başlamadan önce sınıfın sorumluluklarını tam olarak nasıl bölmek istediğinize karar verin.

1. İlgili işlevselliği içerecek yeni bir sınıf oluşturun.

2. Eski sınıf ile yeni sınıf arasında bir ilişki oluşturun. İdeal durumda bu ilişki tek yönlüdür. Bu tek yönlü ilişki, ikinci sınıfın herhangi bir sorun olmadan yeniden kullanılmasına olanak tanır. Ancak iki yönlü bir ilişkinin gerekli olduğunu düşünüyorsanız bu da elbette her zaman kurulabilir. Sizi engelleyen bir kural veya durum yoktur.

3. Yeni sınıfa taşımaya karar verdiğiniz her alan ve yöntem için **Move Field** ve **Move Method** tekniklerini kullanın. Yöntemler için, hata yapma riskini azaltmak amacıyla private olanlarla başlayın. En sonunda hata-düzeltme zincirinden kaçınmak için, her seferinde biraz yer değiştirmeye çalışın ve her taşımadan sonra sonuçları test edin.

Taşıma işlemini tamamladıktan sonra ortaya çıkan sınıfları bir kez daha kontrol edin. Sorumlulukları değişen eski bir sınıf, daha fazla netlik sağlamak amacıyla yeniden adlandırılabilir. Varsa, iki yönlü sınıf ilişkilerinden kurtulup kurtulamayacağınızı görmek için tekrar kontrol edin.

4. Ayrıca yeni sınıfa dışarıdan erişilebilirliği de düşünün. Sınıfı private hale getirerek, eski sınıfın alanları aracılığıyla yöneterek, sınıfı tamamen istemciden gizleyebilirsiniz. Alternatif olarak, istemcinin değerleri doğrudan değiştirmesine izin vererek bunu herkese açık (public) hale getirebilirsiniz. Buradaki kararınız, yeni sınıftaki değerlerde beklenmeyen doğrudan değişiklikler yapıldığında eski sınıfın davranışı açısından ne kadar güvenli olduğuna bağlıdır.


## Inline Class

### 🙁 Problem

Bir sınıf neredeyse hiçbir şey yapmaz ve hiçbir şeyden sorumlu değildir. Yani neredeyse görevi olmayan kullanılmayan bir sınıf vardır. Bu projeniz için sorun olabilir.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Inline%20Class%20-%20Before.png)
</div>

### 😊 Çözüm

Tüm özellikleri sınıftan diğer bir sınıfa taşıyın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Inline%20Class%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Çoğu zaman bu tekniğe, bir sınıfın özellikleri diğer sınıflara aktarıldıktan ve o sınıfa yapacak çok az şey bırakıldıktan sonra ihtiyaç duyulur.

### ✅ Avantajları

Gereksiz sınıfları ortadan kaldırmak, bilgisayardaki işletim belleğini ve kafanızdaki bant genişliğini serbest bırakır.

### 🤯 Nasıl Refactor Edilir?

1. Alıcı sınıfında, verici sınıfında (neredeyse iş yapmayan sınıf) bulunan ortak alanları ve yöntemleri oluşturun. Yöntemler, verici sınıfının eşdeğer yöntemlerine atıfta bulunmalıdır.

2. Verici sınıfına yapılan tüm referansları, alıcı sınıfın alanlarına ve yöntemlerine yapılan referanslarla değiştirin.

3. Şimdi programı test edin ve herhangi bir hata eklenmediğinden emin olun. Testler her şeyin yolunda gittiğini gösteriyorsa, tüm işlevleri orijinal sınıftan alıcı sınıfına tamamen aktarmak için **Move Method** ve **Move Field** tekniklerini kullanmaya başlayın. Orijinal sınıf tamamen boşalana kadar bunu yapmaya devam edin.

4. Orijinal sınıfı silin.


## Hide Delegate

### 🙁 Problem

İstemci B nesnesini A nesnesinin bir alanından veya yönteminden alır. Daha sonra istemci B nesnesinin bir yöntemini çağırır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Hide%20Delegate%20-%20Before.png)
</div>

### 😊 Çözüm

A sınıfında çağrıyı B nesnesine devreden yeni bir yöntem oluşturun. Artık istemci B sınıfını bilmiyor veya ona bağlı değil.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Hide%20Delegate%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?


Başlangıç ​​olarak terminolojiye bakalım:

- Sunucu (server), istemcinin doğrudan erişime sahip olduğu nesnedir.

- Temsilci (delegate), istemcinin ihtiyaç duyduğu işlevselliği içeren son nesnedir.

Bir istemci başka bir nesneden bir nesne talep ettiğinde, ardından ikinci nesne başka bir nesne talep ettiğinde ve bu şekilde devam ettiğinde bir çağrı zinciri ortaya çıkar. Bu çağrı dizileri, istemcinin sınıf yapısı boyunca gezinmesini içerir. Bu karşılıklı ilişkilerdeki herhangi bir değişiklik, istemci tarafında da değişiklik yapılmasını gerektirecektir.

### ✅ Avantajları

Temsilciyi istemciden gizleyin. İstemci kodunun nesneler arasındaki ilişkilerin ayrıntıları hakkında ne kadar az bilgiye ihtiyacı olursa, programınızda değişiklik yapmak o kadar kolay olur.

### 🚫 Dezavantajları

Aşırı sayıda temsilci (delegation) yöntemi oluşturmanız gerekiyorsa, sunucu sınıfı gereksiz bir aracı olma riskiyle karşı karşıya kalır ve bu da **Middle Man** fazlalığına yol açar.

### 🤯 Nasıl Refactor Edilir?

1. İstemci tarafından çağrılan temsilci sınıfının her yöntemi için, sunucu sınıfında, çağrıyı temsilci sınıfına devreden bir yöntem oluşturun.

2. İstemci kodunu, sunucu sınıfının yöntemlerini çağıracak şekilde değiştirin.

3. Değişiklikleriniz istemcinin temsilci sınıfına ihtiyaç duymasını engelliyorsa, sunucu sınıfından temsilci sınıfına erişim yöntemini kaldırabilirsiniz (başlangıçta temsilci sınıfını almak için kullanılan yöntem).

## Remove Middle Man

### 🙁 Problem

Bir sınıfın, diğer nesnelere yetki veren çok fazla yöntemi vardır.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Middle%20Man%20-%20Before.png)
</div>

### 😊 Çözüm

Bu yöntemleri silin ve istemciyi doğrudan son yöntemleri çağırmaya zorlayın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Remove%20Middle%20Man%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?
Bu tekniği açıklamak için **Hide Delegate** tekniğini terimlerini kullanacağız:

- Sunucu (server),  istemcinin doğrudan erişime sahip olduğu nesnedir.

- Temsilci (delegate), istemcinin ihtiyaç duyduğu işlevselliği içeren son nesnedir.

İki tür sorun vardır:

1. Sunucu sınıfı hiçbir şey yapmaz ve yalnızca gereksiz karmaşıklık yaratır. Bu durumda bu sınıfa ihtiyaç olup olmadığı üzerine düşünmek gerekir.
2. Temsilciye (delegate) her yeni özellik eklendiğinde, sunucu sınıfında bunun için bir yetki verme (delegating) yöntemi oluşturmanız gerekir. Çok fazla değişiklik yapılırsa bu oldukça yorucu olacaktır.

### 🤯 Nasıl Refactor Edilir?

1. Sunucu sınıfı nesnesinden temsilci sınıfı nesnesine erişmek için bir alıcı (getter) oluşturun.

2. Sunucu sınıfındaki yetki verme yöntemlerine yapılan çağrıları, temsilci sınıfındaki yöntemlere yönelik doğrudan çağrılarla değiştirin.


## Introduce Foreign Method

### 🙁 Problem

Yardımcı program sınıfı ihtiyacınız olan yöntemi içermez ve yöntemi sınıfa ekleyemezsiniz.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Local%20Extension%20-%20Before.png)
</div>

### 😊 Çözüm

Yöntemleri içeren yeni bir sınıf oluşturun ve onu yardımcı program sınıfının alt öğesi veya sarmalayıcısı yapın.

<div align="center">

![](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Local%20Extension%20-%20After.png)
</div>

### 🤔 Neden Refactoring Uygulanmalı?

Kullandığınız sınıf ihtiyacınız olan yöntemlere sahip değil. Daha da kötüsü, bu yöntemleri ekleyemezsiniz (çünkü sınıflar örneğin üçüncü taraf bir kütüphanededir). İki çıkış yolu var:

1. İlgili sınıftan, yöntemleri içeren ve diğer her şeyi ana sınıftan miras alan bir alt sınıf oluşturun. Bu yol daha kolaydır ancak bazen yardımcı program sınıfının kendisi tarafından engellenir (`final` anahtar kelimesi nedeniyle).
2. Tüm yeni yöntemleri içeren bir sarmalayıcı (wrapper) sınıf oluşturun ve başka bir yerde ilgili nesneye yardımcı program sınıfından yetki verin. Bu yöntem daha fazla iş gerektirir çünkü yalnızca sarmalayıcı ve yardımcı program nesnesi arasındaki ilişkiyi sürdürmek için koda değil, aynı zamanda yardımcı program sınıfının genel arayüzünü taklit etmek için çok sayıda basit yetki verme yöntemine de ihtiyacınız vardır.

### ✅ Avantajları

Ek yöntemleri ayrı bir uzantı sınıfına (sarmalayıcı veya alt sınıf) taşıyarak, istemci sınıflarını uymayan kodlarla doldurmaktan kaçınırsınız. Program bileşenleri daha tutarlıdır ve daha fazla yeniden kullanılabilir.

### 🤯 Nasıl Refactor Edilir?

1. Yeni bir uzantı sınıfı oluşturun:
	-	Seçenek A: Onu yardımcı program sınıfının bir çocuğu (child-class) yapın.
	-	Seçenek B: Eğer bir sarmalayıcı yapmaya karar verdiyseniz, içinde delegasyonun yapılacağı yardımcı program sınıfı nesnesini depolamak için bir alan oluşturun. Bu seçeneği kullanırken, yardımcı program sınıfının genel yöntemlerini tekrarlayan ve yardımcı program nesnesinin yöntemlerine basit temsilci atama içeren yöntemler de oluşturmanız gerekecektir.

2. Fayda sınıfının yapıcısının parametrelerini kullanan bir yapıcı oluşturun.

3. Ayrıca parametrelerinde yalnızca orijinal sınıfın nesnesini alan alternatif bir dönüştürme yapıcısı (constructor) oluşturun. Bu, orijinal sınıfın nesnelerinin yerine uzantının yerleştirilmesine yardımcı olacaktır.

4. Sınıfta yeni genişletilmiş yöntemler oluşturun. Yabancı yöntemleri diğer sınıflardan bu sınıfa taşıyın veya yabancı yöntemlerin işlevleri uzantıda zaten mevcutsa silin.

5. Yardımcı program sınıfının kullanımını, işlevselliğinin gerekli olduğu yerlerde yeni uzantı sınıfıyla değiştirin.

