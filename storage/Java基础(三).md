[

#### 1.21 说一说hashCode()和equals()的关系

 **参考答案**

hashCode()用于获取哈希码（散列码），eauqls()用于比较两个对象是否相等，它们应遵守如下规定：

  * 如果两个对象相等，则它们必须有相同的哈希码。

  * 如果两个对象有相同的哈希码，则它们未必相等。

 **扩展阅读**

在Java中，Set接口代表无序的、元素不可重复的集合，HashSet则是Set接口的典型实现。

当向HashSet中加入一个元素时，它需要判断集合中是否已经包含了这个元素，从而避免重复存储。由于这个判断十分的频繁，所以要讲求效率，绝不能采用遍历集合逐个元素进行比较的方式。实际上，HashSet是通过获取对象的哈希码，以及调用对象的equals()方法来解决这个判断问题的。

HashSet首先会调用对象的hashCode()方法获取其哈希码，并通过哈希码确定该对象在集合中存放的位置。假设这个位置之前已经存了一个对象，则HashSet会调用equals()对两个对象进行比较。若相等则说明对象重复，此时不会保存新加的对象。若不等说明对象不重复，但是它们存储的位置发生了碰撞，此时HashSet会采用链式结构在同一位置保存多个对象，即将新加对象链接到原来对象的之后。之后，再有新添加对象也映射到这个位置时，就需要与这个位置中所有的对象进行equals()比较，若均不相等则将其链到最后一个对象之后。

#### 1.22 为什么要重写hashCode()和equals()？

 **参考答案**

Object类提供的equals()方法默认是用==来进行比较的，也就是说只有两个对象是同一个对象时，才能返回相等的结果。而实际的业务中，我们通常的需求是，若两个不同的对象它们的内容是相同的，就认为它们相等。鉴于这种情况，Object类中equals()方法的默认实现是没有实用价值的，所以通常都要重写。

由于hashCode()与equals()具有联动关系（参考“说一说hashCode()和equals()的关系”一题），所以equals()方法重写时，通常也要将hashCode()进行重写，使得这两个方法始终满足相关的约定。

#### 1.23 ==和equals()有什么区别？

 **参考答案**

==运算符：

  * 作用于基本数据类型时，是比较两个数值是否相等；

  * 作用于引用数据类型时，是比较两个对象的内存地址是否相同，即判断它们是否为同一个对象；

equals()方法：

  * 没有重写时，Object默认以 == 来实现，即比较两个对象的内存地址是否相同；

  * 进行重写后，一般会按照对象的内容来进行比较，若两个对象内容相同则认为对象相等，否则认为对象不等。

#### 1.24 String类有哪些方法？

 **参考答案**

String类是Java最常用的API，它包含了大量处理字符串的方法，比较常用的有：

  * char charAt(int index)：返回指定索引处的字符；

  * String substring(int beginIndex, int endIndex)：从此字符串中截取出一部分子字符串；

  * String[] split(String regex)：以指定的规则将此字符串分割成数组；

  * String trim()：删除字符串前导和后置的空格；

  * int indexOf(String str)：返回子串在此字符串首次出现的索引；

  * int lastIndexOf(String str)：返回子串在此字符串最后出现的索引；

  * boolean startsWith(String prefix)：判断此字符串是否以指定的前缀开头；

  * boolean endsWith(String suffix)：判断此字符串是否以指定的后缀结尾；

  * String toUpperCase()：将此字符串中所有的字符大写；

  * String toLowerCase()：将此字符串中所有的字符小写；

  * String replaceFirst(String regex, String replacement)：用指定字符串替换第一个匹配的子串；

  * String replaceAll(String regex, String replacement)：用指定字符串替换所有的匹配的子串。

 **注意事项**

String类的方法太多了，你没必要都记下来，更不需要一一列举。面试时能说出一些常用的方法，表现出对这个类足够的熟悉就可以了。另外，建议你挑几个方法仔细看看源码实现，面试时可以重点说这几个方法。

#### 1.25 String可以被继承吗？

 **参考答案**

String类由final修饰，所以不能被继承。

 **扩展阅读**

在Java中，String类被设计为不可变类，主要表现在它保存字符串的成员变量是final的。

  * Java 9之前字符串采用char[]数组来保存字符，即 private final char[] value；

  * Java 9做了改进，采用byte[]数组来保存字符，即 private final byte[] value；

之所以要把String类设计为不可变类，主要是出于安全和性能的考虑，可归纳为如下4点。

  * 由于字符串无论在任何 Java 系统中都广泛使用，会用来存储敏感信息，如账号，密码，网络路径，文件处理等场景里，保证字符串 String 类的安全性就尤为重要了，如果字符串是可变的，容易被篡改，那我们就无法保证使用字符串进行操作时，它是安全的，很有可能出现 SQL 注入，访问危险文件等操作。

  * 在多线程中，只有不变的对象和值是线程安全的，可以在多个线程中共享数据。由于 String 天然的不可变，当一个线程”修改“了字符串的值，只会产生一个新的字符串对象，不会对其他线程的访问产生副作用，访问的都是同样的字符串数据，不需要任何同步操作。

  * 字符串作为基础的数据结构，大量地应用在一些集合容器之中，尤其是一些散列集合，在散列集合中，存放元素都要根据对象的 hashCode() 方法来确定元素的位置。由于字符串 hashcode 属性不会变更，保证了唯一性，使得类似 HashMap，HashSet 等容器才能实现相应的缓存功能。由于 String 的不可变，避免重复计算 hashcode，只要使用缓存的 hashcode 即可，这样一来大大提高了在散列集合中使用 String 对象的性能。

  * 当字符串不可变时，字符串常量池才有意义。字符串常量池的出现，可以减少创建相同字面量的字符串，让不同的引用指向池中同一个字符串，为运行时节约很多的堆内存。若字符串可变，字符串常量池失去意义，基于常量池的 String.intern() 方法也失效，每次创建新的字符串将在堆内开辟出新的空间，占据更多的内存。

因为要保证String类的不可变，那么将这个类定义为final的就很容易理解了。如果没有final修饰，那么就会存在String的子类，这些子类可以重写String类的方法，强行改变字符串的值，这便违背了String类设计的初衷。

#### 1.26 说一说String和StringBuffer有什么区别

 **参考答案**

String类是不可变类，即一旦一个String对象被创建以后，包含在这个对象中的字符序列是不可改变的，直至这个对象被销毁。

StringBuffer对象则代表一个字符序列可变的字符串，当一个StringBuffer被创建以后，通过StringBuffer提供的append()、insert()、reverse()、setCharAt()、setLength()等方法可以改变这个字符串对象的字符序列。一旦通过StringBuffer生成了最终想要的字符串，就可以调用它的toString()方法将其转换为一个String对象。

#### 1.27 说一说StringBuffer和StringBuilder有什么区别

 **参考答案**

StringBuffer、StringBuilder都代表可变的字符串对象，它们有共同的父类
AbstractStringBuilder，并且两个类的构造方法和成员方法也基本相同。不同的是，StringBuffer是线程安全的，而StringBuilder是非线程安全的，所以StringBuilder性能略高。一般情况下，要创建一个内容可变的字符串，建议优先考虑StringBuilder类。

#### 1.28 使用字符串时，new和""推荐使用哪种方式？

 **参考答案**

先看看 "hello" 和 new String("hello") 的区别：

  * 当Java程序直接使用 "hello" 的字符串直接量时，JVM将会使用常量池来管理这个字符串；

  * 当使用 new String("hello") 时，JVM会先使用常量池来管理 "hello" 直接量，再调用String类的构造器来创建一个新的String对象，新创建的String对象被保存在堆内存中。

显然，采用new的方式会多创建一个对象出来，会占用更多的内存，所以一般建议使用直接量的方式创建字符串。

#### 1.29 说一说你对字符串拼接的理解

 **参考答案**

拼接字符串有很多种方式，其中最常用的有4种，下面列举了这4种方式各自适合的场景。

  1. + 运算符：如果拼接的都是字符串直接量，则适合使用 + 运算符实现拼接；

  2. StringBuilder：如果拼接的字符串中包含变量，并不要求线程安全，则适合使用StringBuilder；

  3. StringBuffer：如果拼接的字符串中包含变量，并且要求线程安全，则适合使用StringBuffer；

  4. String类的concat方法：如果只是对两个字符串进行拼接，并且包含变量，则适合使用concat方法；

 **扩展阅读**

采用 + 运算符拼接字符串时：

  * 如果拼接的都是字符串直接量，则在编译时编译器会将其直接优化为一个完整的字符串，和你直接写一个完整的字符串是一样的，所以效率非常的高。

  * 如果拼接的字符串中包含变量，则在编译时编译器采用StringBuilder对其进行优化，即自动创建StringBuilder实例并调用其append()方法，将这些字符串拼接在一起，效率也很高。但如果这个拼接操作是在循环中进行的，那么每次循环编译器都会创建一个StringBuilder实例，再去拼接字符串，相当于执行了 new StringBuilder().append(str)，所以此时效率很低。

采用StringBuilder/StringBuffer拼接字符串时：

  * StringBuilder/StringBuffer都有字符串缓冲区，缓冲区的容量在创建对象时确定，并且默认为16。当拼接的字符串超过缓冲区的容量时，会触发缓冲区的扩容机制，即缓冲区加倍。

  * 缓冲区频繁的扩容会降低拼接的性能，所以如果能提前预估最终字符串的长度，则建议在创建可变字符串对象时，放弃使用默认的容量，可以指定缓冲区的容量为预估的字符串的长度。

采用String类的concat方法拼接字符串时：

  * concat方法的拼接逻辑是，先创建一个足以容纳待拼接的两个字符串的字节数组，然后先后将两个字符串拼到这个数组里，最后将此数组转换为字符串。

  * 在拼接大量字符串的时候，concat方法的效率低于StringBuilder。但是只拼接2个字符串时，concat方法的效率要优于StringBuilder。并且这种拼接方式代码简洁，所以只拼2个字符串时建议优先选择concat方法。

#### 1.30 两个字符串相加的底层是如何实现的？

 **参考答案**

如果拼接的都是字符串直接量，则在编译时编译器会将其直接优化为一个完整的字符串，和你直接写一个完整的字符串是一样的。

如果拼接的字符串中包含变量，则在编译时编译器采用StringBuilder对其进行优化，即自动创建StringBuilder实例并调用其append()方法，将这些字符串拼接在一起。

#### 1.31 String a = "abc"; ，说一下这个过程会创建什么，放在哪里？

 **参考答案**

JVM会使用常量池来管理字符串直接量。在执行这句话时，JVM会先检查常量池中是否已经存有"abc"，若没有则将"abc"存入常量池，否则就复用常量池中已有的"abc"，将其引用赋值给变量a。

#### 1.32 new String("abc") 是去了哪里，仅仅是在堆里面吗？

 **参考答案**

在执行这句话时，JVM会先使用常量池来管理字符串直接量，即将"abc"存入常量池。然后再创建一个新的String对象，这个对象会被保存在堆内存中。并且，堆中对象的数据会指向常量池中的直接量。

#### 1.33 接口和抽象类有什么区别？

 **参考答案**

从设计目的上来说，二者有如下的区别：

接口体现的是一种规范。对于接口的实现者而言，接口规定了实现者必须向外提供哪些服务；对于接口的调用者而言，接口规定了调用者可以调用哪些服务，以及如何调用这些服务。当在一个程序中使用接口时，接口是多个模块间的耦合标准；当在多个应用程序之间使用接口时，接口是多个程序之间的通信标准。

抽象类体现的是一种模板式设计。抽象类作为多个子类的抽象父类，可以被当成系统实现过程中的中间产品，这个中间产品已经实现了系统的部分功能，但这个产品依然不能当成最终产品，必须有更进一步的完善，这种完善可能有几种不同方式。

从使用方式上来说，二者有如下的区别：

  * 接口里只能包含抽象方法、静态方法、默认方法和私有方法，不能为普通方法提供方法实现；抽象类则完全可以包含普通方法。

  * 接口里只能定义静态常量，不能定义普通成员变量；抽象类里则既可以定义普通成员变量，也可以定义静态常量。

  * 接口里不包含构造器；抽象类里可以包含构造器，抽象类里的构造器并不是用于创建对象，而是让其子类调用这些构造器来完成属于抽象类的初始化操作。

  * 接口里不能包含初始化块；但抽象类则完全可以包含初始化块。

  * 一个类最多只能有一个直接父类，包括抽象类；但一个类可以直接实现多个接口，通过实现多个接口可以弥补Java单继承的不足。

 **扩展阅读**

接口和抽象类很像，它们都具有如下共同的特征：

  * 接口和抽象类都不能被实例化，它们都位于继承树的顶端，用于被其他类实现和继承。

  * 接口和抽象类都可以包含抽象方法，实现接口或继承抽象类的普通子类都必须实现这些抽象方法。

#### 1.34 接口中可以有构造函数吗？

 **参考答案**

由于接口定义的是一种规范，因此接口里不能包含构造器和初始化块定义。接口里可以包含成员变量（只能是静态常量）、方法（只能是抽象实例方法、类方法、默认方法或私有方法）、内部类（包括内部接口、枚举）定义。

#### 1.35 谈谈你对面向接口编程的理解

 **参考答案**

接口体现的是一种规范和实现分离的设计哲学，充分利用接口可以极好地降低程序各模块之间的耦合，从而提高系统的可扩展性和可维护性。基于这种原则，很多软件架构设计理论都倡导“面向接口”编程，而不是面向实现类编程，希望通过面向接口编程来降低程序的耦合。

]

