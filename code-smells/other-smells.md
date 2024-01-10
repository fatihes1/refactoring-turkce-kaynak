# Diğer Kokular (Other Smells)

Aşağıda herhangi bir geniş kategoriye girmeyen kokular bulunmaktadır.

## 1️⃣ Tamamlanmamış Kütüphane Sınıfı (Incomplete Library Class)

**🤢 Belirti ve Semptomlar**

Er ya da geç kütüphaneler kullanıcı ihtiyaçlarını karşılamayı bırakır. Sorunun tek çözümü, yani kitaplığı değiştirmek olacaktır. Ancak kitaplık salt okunur olduğundan çoğu zaman imkansızdır.

![](https://refactoring.guru/images/refactoring/content/smells/incomplete-library-class-01-2x.png)

**🤒 Sorunun Nedenleri**

Kütüphanenin yazarı ihtiyacınız olan özellikleri sağlamadı veya bunları uygulamayı reddettiği durumlarda bu sorunla karşılaşılabilir.

**💊 Tedavi**

- Bir kütüphane sınıfına birkaç yöntem tanıtmak için **Introduce Foreign Method** tekniği kullanılabilir.

- Sınıf kitaplığında büyük değişiklikler için **Introduce Local Extension** yöntemini kullanabilirsiniz.

**💰 Hesaplaşma**

- Kod tekrarını azaltır (kendi kitaplığınızı sıfırdan oluşturmak yerine, mevcut kitaplığınızı yine de kullanabilirsiniz).

![](https://refactoring.guru/images/refactoring/content/smells/incomplete-library-class-02-2x.png)

**🤫 Ne Zaman Yok Sayılmalı?**

Kitaplıkta yapılan değişiklikler kodda değişiklik içeriyorsa, kitaplığı genişletmek ek iş üretebilir.

