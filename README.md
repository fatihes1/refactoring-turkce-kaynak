# 📝 Refactoring Rehberi

Bu repo, yazılım geliştiriciler için refactoring konusunda bir rehber niteliği taşımaktadır. Temiz kodun ne olduğundan, teknik borçtan, ne zaman ve nasıl refactoring yapılacağına kadar geniş bir yelpazede bilgi içermektedir. Ayrıca, kod kokuları olarak bilinen ve kod kalitesini düşüren sorunlara ve bu sorunların nasıl giderilebileceğine dair pratik tekniklere odaklanmaktadır.

## 🛠️ Refactoring Nedir?

Refactoring, mevcut bir kod tabanını değiştirerek onu daha temiz, daha okunabilir ve daha bakımı kolay hale getirme sürecidir. Refactoring'in amacı, kodun işlevselliğini değiştirmeden, ancak daha iyi tasarlanmış, daha modüler ve daha kaliteli bir şekilde olmasını sağlamaktır. Bu süreç, kod kokularını gidermeyi, tekrar eden kodu azaltmayı, karmaşıklığı azaltmayı ve kodun genel kalitesini artırmayı hedefler. Refactoring, yazılım geliştirme sürecinde sürekli bir iyileştirme ve optimizasyon çabası olarak görülmelidir.

## 📚 İçindekiler

- 📒 **Giriş**
	-  📖 [Clean Code (Temiz Kod)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/introduction.md#temiz-kod-clean-code) 
	- 📖 [Technical Debt (Teknik Borç)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/introduction.md#teknik-bor%C3%A7-technical-debt) 
	- 📖 [Ne Zaman Refactoring Yapılmalı?](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/introduction.md#ne-zaman-refactoring-yap%C4%B1lmal%C4%B1) 
	- 📖 [Refactoring Nasıl Yapılır?](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/intro📌duction.md#refactoring-nas%C4%B1l-yap%C4%B1l%C4%B1r) 

- 📒 **Kod Kokuları (Code Smells)**
	- 📖 [Bloaters (Şişkinlik)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#bloaters-%C5%9Fi%C5%9Fkinlik) 
		- 📌 [Long Method (Uzun Metot)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#1%EF%B8%8F%E2%83%A3-uzun-metot-long-method) 
		- 📌 [Large Class (Büyük Sınıf)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#2%EF%B8%8F%E2%83%A3-large-class-b%C3%BCy%C3%BCk-s%C4%B1n%C4%B1f) 
		- 📌 [Primitive Obsession (İlkel Takıntı)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#3%EF%B8%8F%E2%83%A3-primitive-obsession-i%CC%87lkel-tak%C4%B1nt%C4%B1) 
		- 📌 [Long Parameter List (Uzun Parametre Listesi)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#4%EF%B8%8F%E2%83%A3-long-parameter-list-uzun-parametre-listesi) 
		- 📌 [Data Clumps (Veri Kümeleri)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/bloaters.md#5%EF%B8%8F%E2%83%A3-data-clumps-veri-k%C3%BCmeleri)

	- 📖 [Object-Orientation Abusers (Nesne Yönelimini Kötüye Kullanma)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/object-orientation-abusers.md#object-orientation-abusers-nesne-y%C3%B6nelimini-k%C3%B6t%C3%BCye-kullanma) 
		- 📌 [Switch Statements](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/object-orientation-abusers.md#1%EF%B8%8F%E2%83%A3-switch-statements) 
		- 📌 [Temporary Field (Geçici Alan)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/object-orientation-abusers.md#2%EF%B8%8F%E2%83%A3-temporary-field-ge%C3%A7ici-alan) 
		- 📌 [Refused Bequest (Reddedilen Miras)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/object-orientation-abusers.md#3%EF%B8%8F%E2%83%A3-refused-bequest-reddedilen-miras) 
		- 📌 [Alternative Classes with Different Interfaces (Farklı Arayüzlerle Alternatif Sınıflar)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/object-orientation-abusers.md#4%EF%B8%8F%E2%83%A3-alternative-classes-with-different-interfaces-farkl%C4%B1-aray%C3%BCzlerle-alternatif-s%C4%B1n%C4%B1flar)

	- 📖 [Change Preventers (Önleyicileri Değiştirme)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/change-preventers.md#change-preventers-%C3%B6nleyicileri-de%C4%9Fi%C5%9Ftirme) 
		- 📌 [Divergent Change (Iraksak Değişim)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/change-preventers.md#1%EF%B8%8F%E2%83%A3-iraksak-de%C4%9Fi%C5%9Fim-divergent-change) 
		- 📌 [Shotgun Surgery](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/change-preventers.md#2%EF%B8%8F%E2%83%A3-shotgun-surgery) 
		- 📌 [Parallel Inheritance Hierarchies](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/change-preventers.md#3%EF%B8%8F%E2%83%A3-parallel-inheritance-hierarchies) 

	- 📖 [Dispensables (Vazgeçilebilir Olanlar)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#vazge%C3%A7ilebilir-dispensables) 
		- 📌 [Comments (Yorumlar)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#1%EF%B8%8F%E2%83%A3-yorumlar-comments) 
		- 📌 [Duplicate Code (Tekrarlı/Kopya Kod)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#2%EF%B8%8F%E2%83%A3-tekrarl%C4%B1kopya-kod-duplicate-code) 
		- 📌 [Lazy Class (Tembel Sınıf)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#3%EF%B8%8F%E2%83%A3-tembel-s%C4%B1n%C4%B1f-lazy-class) 
		- 📌 [Data Class (Veri Sınıfı)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#4%EF%B8%8F%E2%83%A3-veri-s%C4%B1n%C4%B1f%C4%B1-data-class) 
		- 📌 [Dead Code (Ölü Kod)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#5%EF%B8%8F%E2%83%A3-%C3%B6l%C3%BC-kod-dead-code) 
		- 📌 [Speculative Generality (Spekülatif Genellik)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/dispensables.md#6%EF%B8%8F%E2%83%A3-spek%C3%BClatif-genellik-speculative-generality)

	- 📖 [Couplers (Bağlayıcılar)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/couplers.md#ba%C4%9Flay%C4%B1c%C4%B1lar-couplers) 
		- 📌 [Feature Envy (Özellik Kıskançlığı)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/couplers.md#1%EF%B8%8F%E2%83%A3-%C3%B6zellik-k%C4%B1skan%C3%A7l%C4%B1%C4%9F%C4%B1-feature-envy) 
		- 📌 [Inappropriate Intimacy (Uygunsuz Yakınlık)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/couplers.md#2%EF%B8%8F%E2%83%A3-uygunsuz-yak%C4%B1nl%C4%B1k-inappropriate-intimacy) 
		- 📌 [Message Chains (Mesaj Zincirleri)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/couplers.md#3%EF%B8%8F%E2%83%A3-mesaj-zincirleri-message-chains) 
		- 📌 [Middle Man (Orta Adam)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/couplers.md#4%EF%B8%8F%E2%83%A3-orta-adan-middle-man)

	- 📖 [Other Smells (Diğer Kokular)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/other-smells.md#di%C4%9Fer-kokular-other-smells)
		- 📌 [Incomplete Library Class (Tamamlanmamış Kütüphane Sınıfı)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/code-smells/other-smells.md#1%EF%B8%8F%E2%83%A3-tamamlanmam%C4%B1%C5%9F-k%C3%BCt%C3%BCphane-s%C4%B1n%C4%B1f%C4%B1-incomplete-library-class)

- 📒 **Refactoring Techniques (Refactoring Teknikleri)**

	- 📖 [Composing Methods (Oluşturma Yöntemleri)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#olu%C5%9Fturma-y%C3%B6ntemleri-composing-methods) 
		- 📌 [Extract Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#extract-method) 
		- 📌 [Inline Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#inline-method) 
		- 📌 [Extract Variable](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#extract-variable) 
		- 📌 [Inline Temp](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#inline-temp) 
		- 📌 [Replace Temp with Query](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#replace-temp-with-query) 
		- 📌 [Split Temporary Variable](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#split-temporary-variable) 
		- 📌 [Remove Assignments to Parameters](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#remove-assignments-to-parameters) 
		- 📌 [Replace Method with Method Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#replace-method-with-method-object) 
		- 📌 [Substitute Algorithm](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/composing-methods.md#substitute-algorithm)

	- 📖 [Moving Features between Objects (Özellikleri Nesneler Arasında Taşıma)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#%C3%B6zellikleri-nesneler-aras%C4%B1nda-ta%C5%9F%C4%B1ma-moving-features-between-objects)
		- 📌 [Move Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#move-method) 
		- 📌 [Move Field](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#move-field) 
		- 📌 [Extract Class](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#extract-class) 
		- 📌 [Inline Class](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#inline-class) 
		- 📌 [Hide Delegate](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#hide-delegate) 
		- 📌 [Remove Middle Man](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#remove-middle-man) 
		- 📌 [Introduce Foreign Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#introduce-foreign-method) 
		- 📌 [Introduce Local Extension](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/moving-features-between-objects.md#introduce-local-extension)

	- 📖 [Organizing Data (Verileri Düzenleme)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#verileri-d%C3%BCzenleme-organizing-data)
		- 📌 [Self Encapsulate Field](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#self-encapsulate-field) 
		- 📌 [Replace Data Value with Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-data-value-with-object) 
		- 📌 [Change Value to Reference](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#change-value-to-reference) 
		- 📌 [Change Reference to Value](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#change-reference-to-value) 
		- 📌 [Replace Array with Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-array-with-object) 
		- 📌 [Duplicate Observed Data](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#duplicate-observed-data) 
		- 📌 [Change Unidirectional Association to Bidirectional](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#change-unidirectional-association-to-bidirectional) 
		- 📌 [Change Bidirectional Association to Unidirectional](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#change-bidirectional-association-to-unidirectional) 
		- 📌 [Replace Magic Number with Symbolic Constant](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-magic-number-with-symbolic-constant) 
		- 📌 [Encapsulate Field](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#encapsulate-field) 
		- 📌 [Encapsulate Collection](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#encapsulate-collection) 
		- 📌 [Replace Type Code with Class](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-type-code-with-class) 
		- 📌 [Replace Type Code with Subclasses](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-type-code-with-subclasses) 
		- 📌 [Replace Type Code with State/Strategy](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-type-code-with-statestrategy) 
		- 📌 [Replace Subclass with Fields](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/organizing-data.md#replace-subclass-with-fields)

	- 📖 [Simplifying Conditional Expressions (Koşullu İfadeleri Basitleştirme)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#ko%C5%9Fullu-i%CC%87fadeleri-basitle%C5%9Ftirme-simplifying-conditional-expressions)
		- 📌 [Decompose Conditional](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#decompose-conditional) 
		- 📌 [Consolidate Conditional Expression](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#consolidate-conditional-expression) 
		- 📌 [Consolidate Duplicate Conditional Fragments](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#consolidate-duplicate-conditional-fragments) 
		- 📌 [Remove Control Flag](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#remove-control-flag) 
		- 📌 [Replace Nested Conditional with Guard Clauses](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#replace-nested-conditional-with-guard-clauses) 
		- 📌 [Replace Conditional with Polymorphism](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#replace-conditional-with-polymorphism) 
		- 📌 [Introduce Null Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#introduce-null-object) 
		- 📌 [Introduce Assertion](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-conditional-expressions.md#introduce-assertion)

	- 📖 [Simplifying Method Calls (Yöntem Çağrılarını Basitleştirme)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#y%C3%B6ntem-%C3%A7a%C4%9Fr%C4%B1lar%C4%B1n%C4%B1-basitle%C5%9Ftirme-simplifying-method-calls)
		- 📌 [Rename Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#rename-method) 
		- 📌 [Add Parameter](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#add-parameter) 
		- 📌 [Remove Parameter](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#remove-parameter) 
		- 📌 [Separate Query from Modifier](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#separate-query-from-modifier) 
		- 📌 [Parameterize Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#parameterize-method) 
		- 📌 [Replace Parameter with Explicit Methods](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#replace-parameter-with-explicit-methods) 
		- 📌 [Preserve Whole Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#preserve-whole-object) 
		- 📌 [Replace Parameter with Method Call](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#replace-parameter-with-method-call) 
		- 📌 [Introduce Parameter Object](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#introduce-parameter-object) 
		- 📌 [Remove Setting Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#remove-setting-method) 
		- 📌 [Hide Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#hide-method)  
		- 📌 [Replace Constructor with Factory Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#replace-constructor-with-factory-method)
		- 📌 [Replace Error Code with Exception](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#replace-error-code-with-exception) 
		- 📌 [Replace Exception with Test](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/simplifying-method-calls.md#replace-exception-with-test)
	- 📖 [Dealing with Generalization (Genellemeyle Başa Çıkmak)](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#genellemeyle-ba%C5%9Fa-%C3%A7%C4%B1kmak-dealing-with-generalization)
		- 📌 [Pull Up Field](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#pull-up-field)  
		- 📌 [Pull Up Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#pull-up-method) 
		- 📌 [Pull Up Constructor Body](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#pull-up-constructor-body) 
		- 📌 [Push Down Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#push-down-method) 
		- 📌 [Push Down Field](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#push-down-field) 
		- 📌 [Extract Subclass](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#extract-subclass) 
		- 📌 [Extract Superclass](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#extract-superclass) 
		- 📌 [Extract Interface](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#extract-interface)
		- 📌 [Collapse Hierarchy](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#collapse-hierarchy) 
		- 📌 [Form Template Method](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#form-template-method) 
		- 📌 [Replace Inheritance with Delegation](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#replace-inheritance-with-delegation) 
		- 📌 [Replace Delegation with Inheritance](https://github.com/fatihes1/refactoring-turkce-kaynak/blob/main/refactoring-techniques/dealing-with-generalization.md#replace-delegation-with-inheritance)


## 🤝 Katkıda Bulunma

Dökümanı oluştururken bazı yerlerde terimlerin Türkçe karşılıklarını bulmakta çok güçlük çektim. Haliyle bazı başlıklar yeteri kadar açıklayıcı ve net olmamış olabilir. Bu konularda ve elbette Türkçe çevirilere katkıda bulunmak isterseniz, lütfen aşağıdaki adımları izleyin:

1.  Bu depoyu fork'layın.
2.  İlgili dosyayı düzenleyin veya yeni bir bilgi ekleyin.
3.  Yanlış çeviri veya ekleme yapmak istediğin yerleri özgürce düzenleyebilirsiniz!
4.  Değişikliklerinizi görmem ve onaylayabilmem adına lütfen bir pull request oluşturun.

Katkı contribution'larını ve önerileri memnuniyetle karşılayacağımdan emin olabilirsiniz 🤗 !

## 📜 Lisans

Bu çeviriler Refactoring Guru'nun orijinal içeriği temel alınarak oluşturulmuştur. Elbette kendimce yorumlamaya ve açıklamaya çalıştığım bir o kadar alan bulunuyor. Bununla beraber, orijinal içeriğin lisansı geçerli olacaktır. Lütfen orijinal içeriğin lisans koşullarına uyun.

Orijinal İngilizce kaynağa [buradan](https://refactoring.guru/refactoring) erişebilirsiniz.
