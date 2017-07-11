# Java’da super, static, final anahtar kelimeleri

### Super Anahtar Kelimesi

**Java**’da **super** anahtar kelimesi kalıtım alınan üst sınıfa erişim için kullanılır. Bu anahtar kelime yardımıyla üst sınıfın **kurucusuna, metotlarına, değişkenlerine** erişim sağlanır.

* #### Üst sınıfın değişkenlerine erişim

Üst sınıfın ve alt sınıfın aynı isme sahip değişkenlerini ayırt etmede ya da direkt olarak üst sınıfın değişkenine erişim için kullanılır.

```java
class A {
    String var = "Parent";
}

class B extends A {
    String var = "Child";

    B() {
        System.out.println(super.var);
        System.out.println(this.var);
    }
}
```
* #### Üst sınıfın metotlarına erişim

Üst sınıfın ve alt sınıfın aynı isme sahip metotlarını ayırt etmede ya da direkt olarak üst sınıfın metotlarına erişim için kullanılır.

```java
class A {
    public void func() {
        System.out.println("Parent");
    }
}

class B extends A {

    @Override
    public void func() {
        super.func();
        System.out.println("Child");
    }
}
```

* #### Üst sınıfın kurucusuna erişim

Kurucu metotlar hakkında kısa bir bilgi vermek gerekirse; nesne oluşturulduğunda çalışan **ilk fonksiyondur**. Biz sınıflarımıza eklemesek bile derleme zamanında otomatik olarak parametresiz bir kurucu oluşturulur. Aralarında kalıtım ilişkisi olan sınıflar için kalıtım alan sınıfın(child) kurucusunun ilk satırına otomatik olarak, üst sınıf kurucusunu çağırma kodu olan **super()** eklenir.

```java
class A {
    A() {
        System.out.println("Parent");
    }
}

class B extends A {
    B() {
        super();
        System.out.println("Child");
    }
}
```

### Sınıf blokları ({})

Sınıflar içerisinde değişkenlere ilk değer atamada kullanılabilir. Derlenme zamanında bu blokların arasına yazılan kodlar **kurucu fonksiyonun içerisine** aktarılır.

```java
class A {
    A() {
        System.out.println("Constructor");
    }
    {
        System.out.println("Block");
    }
}

class Test {
    public static void main(String args[]) {
        A a = new A();
    }
}
```

Kodu çalıştırdığımızda çıktımız şu şekilde olmaktadır.

```
Block
Constructor
```

Eğer derlenmiş **A** sınıfının içeriğine bakacak olursak şu şekildedir.

```java
class A {
    A() {
        System.out.println("Block");
        System.out.println("Constructor");
    }
}
```

### Final Anahtar Kelimesi

**Final** anahtar kelime **değişkenlerde, sınıflarda ve metotlarda** kullanılabilir. 

* **Final Değişken**: Bir değişkeni bu anahtar kelime ile işaretlersek nesnenin oluşma anından yıkılma anına kadar değişken **değeri aynı kalacaktır**. Değişken değerini sadece  değişkeni tanımlarken ya da kurucu içerisinde atayabiliriz. 
```java
class A {
    final String finalVariable = "Final";
}
```

* **Final Metot**: Bir fonksiyonu bu anahtar kelime ile işaretlersek sınıfın kalıtım alındığı takdirde bu fonksiyonun **override** edilemeyeceğini belirtmiş oluruz. **Abstract** metotlar **final** olarak işaretlenemez. Bir sınıfın kurucusu final olamaz.

```java
class A {
    final void func() {
        System.out.println("Final Function");
    }
}
```
* **Final Sınıf**: Bir sınıfı bu anahtar kelime ile işaretlersek sınıfın kalıtım alınamayacağını belirtmiş oluruz.** Interface** ve ya **abstract** sınıflar final olarak işaretlenemez.

### Static Anahtar Kelimesi

**Static** anahtar kelimesi ile o değişkenin ya da metodun **nesne yerine sınıfa ait olduğunu** ifade ederiz. Sınıfa ait olan bir değişken ya da metoda nesne oluşturmadan erişilebilir. **Static** sınıf olmaz(iç sınıflar hariç).

* **Static Değişken**:  **Static** veriler **private** olmadığı sürece içinde bulunduğu sınıfın bir nesnesini oluşturmadan erişilebilir. Yaratılan her nesne için ayrı ayrı oluşturulmaz. Sadece sınıf yüklenirken bir kez oluşturulur ve hep o kullanılır. **Static** olmayan **Inner sınıflarda** static veri yer alamaz. **Static** değişkenlerin kullanım amacı hafızayı korumasıdır.

* **Static Metot**: **Static** değişken gibi oluşturulan nesne yerine sınıfa ait olur. **Private** olmadığı sürece nesne oluşturmadan erişilebilir. Static metotlar sadece static değişkenlere erişebilip değerlerini değiştirebilirler(direk olarak erişmek istendiğinde). Doğal olarak **static** olmayan metotlara da doğrudan erişemez. Önce nesne oluşturup nesne üzerinden erişmesi gerekir. Nesne oluşturmadan erişilebildikleri için **main metodu** da static olarak tanımlanır. **JVM** nesne oluşturmadan direk olarak main metoda erişir.

* **Static blok**: Sınıf yüklenirken çalıştırılır. Static üyelere ilk değer atamada kullanılabilir. Static bloklar, sınıf belleğe yüklenirken çalıştırıldığı için bir kurucu fonksiyondan daha önce çalışır.

* **Static iç sınıf**: İç sınıfın static olmayan metotlarına erişim için iç sınıf tipinde bir nesne oluşturmak gerekir. Static olan metotlara doğrudan erişim sağlanabilir.

```java
class A {
    static {
        var = "Static Variable in Static Block";
    }

    static String var = "Static Variable";

    static {
        var = "Static Variable in Static Block";
    }

    static void func() {
        System.out.println("Static Function");
    }

    static class InnerStaticClass {
        static void staticMethod() {
            System.out.println("Inner Static Method");
        }
    }

}

class Test {
    public static void main(String args[]) {
        System.out.println(A.var);
        A.func();
        A.InnerStaticClass.staticMethod();
    }
}
```

