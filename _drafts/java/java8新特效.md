java8新特效
===


# 1\. 简介

毫无疑问，[Java 8](http://www.oracle.com/technetwork/java/javase/8train-relnotes-latest-2153846.html)是Java自Java 5（发布于2004年）之后的最重要的版本。这个版本包含语言、编译器、库、工具和JVM等方面的十多个新特性。在本文中我们将学习这些新特性，并用实际的例子说明在什么场景下适合使用。

这个教程包含Java开发者经常面对的几类问题：

*   语言
*   编译器
*   库
*   工具
*   运行时（JVM）

# 2\. Java语言的新特性

Java 8是Java的一个重大版本，有人认为，虽然这些新特性领Java开发人员十分期待，但同时也需要花不少精力去学习。在这一小节中，我们将介绍Java 8的大部分新特性。

## 2.1 Lambda表达式和函数式接口

Lambda表达式（也称为闭包）是Java 8中最大和最令人期待的语言改变。它允许我们将函数当成参数传递给某个方法，或者把代码本身当作数据处理：[函数式开发者]
#### 语法
>  parameter -> expression body

*   **可选类型声明** - 无需声明参数的类型。编译器可以从该参数的值推断。
*   **可选圆括号参数 **- 无需在括号中声明参数。对于多个参数，括号是必需的。
*   **可选大括号** - 表达式主体没有必要使用大括号，如果主体中含有一个单独的语句。
*   **可选return关键字** - 编译器会自动返回值，如果主体有一个表达式返回的值。花括号是必需的，以表明表达式返回一个值。

Lambda的设计耗费了很多时间和很大的社区力量，最终找到一种折中的实现方案，可以实现简洁而紧凑的语言结构。最简单的Lambda表达式可由逗号分隔的参数列表、**->**符号和语句块组成，例如：

```
Arrays.asList( "a", "b", "d" ).forEach( e -> System.out.println( e ) );
```

在上面这个代码中的参数**e**的类型是由编译器推理得出的，你也可以显式指定该参数的类型，例如：

```
Arrays.asList( "a", "b", "d" ).forEach( ( String e ) -> System.out.println( e ) );
```

如果Lambda表达式需要更复杂的语句块，则可以使用花括号将该语句块括起来，类似于Java中的函数体，例如：

```
Arrays.asList( "a", "b", "d" ).forEach( e -> {
    System.out.print( e );
    System.out.print( e );
} );
```

Lambda表达式可以引用类成员和局部变量（会将这些变量隐式得转换成**final**的），例如下列两个代码块的效果完全相同：

```
String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
```

和

```
final String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
```

Lambda表达式有返回值，返回值的类型也由编译器推理得出。如果Lambda表达式中的语句块只有一行，则可以不用使用**return**语句，下列两个代码片段效果相同：

```
Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> e1.compareTo( e2 ) );
```

和

```
Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> {
    int result = e1.compareTo( e2 );
    return result;
} );
```

Lambda的设计者们为了让现有的功能与Lambda表达式良好兼容，考虑了很多方法，于是产生了**[函数接口](http://www.javacodegeeks.com/2013/03/introduction-to-functional-interfaces-a-concept-recreated-in-java-8.html)**这个概念。函数接口指的是只有一个函数的接口，这样的接口可以隐式转换为Lambda表达式。**java.lang.Runnable**和**java.util.concurrent.Callable**是函数式接口的最佳例子。在实践中，函数式接口非常脆弱：只要某个开发者在该接口中添加一个函数，则该接口就不再是函数式接口进而导致编译失败。为了克服这种代码层面的脆弱性，并显式说明某个接口是函数式接口，Java 8 提供了一个特殊的注解**@FunctionalInterface**（Java 库中的所有相关接口都已经带有这个注解了），举个简单的函数式接口的定义：

```
@FunctionalInterface
public interface Functional {
    void method();
}
```

不过有一点需要注意，[默认方法和静态方法](https://www.javacodegeeks.com/2014/05/java-8-features-tutorial.html#Interface_Default)不会破坏函数式接口的定义，因此如下的代码是合法的。

```
@FunctionalInterface
public interface FunctionalDefaultMethods {
    void method();

    default void defaultMethod() {            
    }        
}
```

Lambda表达式作为Java 8的最大卖点，它有潜力吸引更多的开发者加入到JVM平台，并在纯Java编程中使用函数式编程的概念。如果你需要了解更多Lambda表达式的细节，可以参考[官方文档](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)。

## 2.2 接口的默认方法和静态方法

Java 8使用两个新概念扩展了接口的含义：默认方法和静态方法。[默认方法](http://blog.csdn.net/yczz/article/details/50896975)使得接口有点类似traits，不过要实现的目标不一样。默认方法使得开发者可以在 不破坏二进制兼容性的前提下，往现存接口中添加新的方法，即不强制那些实现了该接口的类也同时实现这个新加的方法。

默认方法和抽象方法之间的区别在于抽象方法需要实现，而默认方法不需要。接口提供的默认方法会被接口的实现类继承或者覆写，例子代码如下：

```
private interface Defaulable {
    // Interfaces now allow default methods, the implementer may or 
    // may not implement (override) them.
    default String notRequired() { 
        return "Default implementation"; 
    }        
}

private static class DefaultableImpl implements Defaulable {
}

private static class OverridableImpl implements Defaulable {
    @Override
    public String notRequired() {
        return "Overridden implementation";
    }
}
```

**Defaulable**接口使用关键字**default**定义了一个默认方法**notRequired()**。**DefaultableImpl**类实现了这个接口，同时默认继承了这个接口中的默认方法；**OverridableImpl**类也实现了这个接口，但覆写了该接口的默认方法，并提供了一个不同的实现。

Java 8带来的另一个有趣的特性是在接口中可以定义静态方法，例子代码如下：

```
private interface DefaulableFactory {
    // Interfaces now allow static methods
    static Defaulable create( Supplier< Defaulable > supplier ) {
        return supplier.get();
    }
}
```

下面的代码片段整合了默认方法和静态方法的使用场景：

```
public static void main( String[] args ) {
    Defaulable defaulable = DefaulableFactory.create( DefaultableImpl::new );
    System.out.println( defaulable.notRequired() );

    defaulable = DefaulableFactory.create( OverridableImpl::new );
    System.out.println( defaulable.notRequired() );
}
```

这段代码的输出结果如下：

```
Default implementation
Overridden implementation
```

由于JVM上的默认方法的实现在字节码层面提供了支持，因此效率非常高。默认方法允许在不打破现有继承体系的基础上改进接口。该特性在官方库中的应用是：给**java.util.Collection**接口添加新方法，如**stream()**、**parallelStream()**、**forEach()**和**removeIf()**等等。

尽管默认方法有这么多好处，但在实际开发中应该谨慎使用：在复杂的继承体系中，默认方法可能引起歧义和编译错误。如果你想了解更多细节，可以参考[官方文档](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)。

注意： 
1\. 静态方法不可以被集成。 
2\. 默认方法可以被继承 
3\. 函数式接口可以实现另一个函数式接口 
4\. 默认方法可以被重写 
5\. 默认方法重新声明，则变成了普通方法

## 2.3 方法引用

方法引用使得开发者可以直接引用现存的方法、Java类的构造方法或者实例对象。方法引用和Lambda表达式配合使用，使得java类的构造方法看起来紧凑而简洁，没有很多复杂的模板代码。

*   **构造器引用** 
    语法是Class::new。请注意构造器没有参数。比如`HashSet::new`.下面详细实例中会有。
*   **静态方法引用** 
    语法是Class::static_method。这个方法接受一个**Class类型的参数**
*   **特定类的任意对象的方法引用** 
    语法是Class::method。**这个方法没有参数**
*   **特定对象的方法引用** 
    语法是instance::method。这个方法接受一个instance对应的Class类型的参数

西门的例子中，**Car**类是不同方法引用的例子，可以帮助读者区分四种类型的方法引用。

```
public static class Car {
    public static Car create( final Supplier< Car > supplier ) {
        return supplier.get();
    }              

    public static void collide( final Car car ) {
        System.out.println( "Collided " + car.toString() );
    }

    public void follow( final Car another ) {
        System.out.println( "Following the " + another.toString() );
    }

    public void repair() {   
        System.out.println( "Repaired " + this.toString() );
    }
}
```

第一种方法引用的类型是构造器引用，语法是**Class::new**，或者更一般的形式：**Class<T>::new**。注意：这个构造器没有参数。

```
final Car car = Car.create( Car::new );
final List< Car > cars = Arrays.asList( car );
```

第二种方法引用的类型是静态方法引用，语法是**Class::static_method**。注意：这个方法接受一个Car类型的参数。

```
cars.forEach( Car::collide );
```

第三种方法引用的类型是某个类的成员方法的引用，语法是**Class::method**，注意，这个方法没有定义入参：

```
cars.forEach( Car::repair );
```

第四种方法引用的类型是某个实例对象的成员方法的引用，语法是**instance::method**。注意：这个方法接受一个Car类型的参数：

```
final Car police = Car.create( Car::new );
cars.forEach( police::follow );
```

运行上述例子，可以在控制台看到如下输出（Car实例可能不同）：

```
Collided com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
Repaired com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
Following the com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
```

如果想了解和学习更详细的内容，可以参考[官方文档](http://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)

## 2.4 重复注解

自从Java 5中引入[注解](http://www.javacodegeeks.com/2012/08/java-annotations-explored-explained.html)以来，这个特性开始变得非常流行，并在各个框架和项目中被广泛使用。不过，注解有一个很大的限制是：在同一个地方不能多次使用同一个注解。Java 8打破了这个限制，引入了重复注解的概念，允许在同一个地方多次使用同一个注解。

在Java 8中使用**@Repeatable**注解定义重复注解，实际上，这并不是语言层面的改进，而是编译器做的一个trick，底层的技术仍然相同。可以利用下面的代码说明：

```
package com.javacodegeeks.java8.repeatable.annotations;

import java.lang.annotation.ElementType;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

public class RepeatingAnnotations {
    @Target( ElementType.TYPE )
    @Retention( RetentionPolicy.RUNTIME )
    public @interface Filters {
        Filter[] value();
    }

    @Target( ElementType.TYPE )
    @Retention( RetentionPolicy.RUNTIME )
    @Repeatable( Filters.class )
    public @interface Filter {
        String value();
    };

    @Filter( "filter1" )
    @Filter( "filter2" )
    public interface Filterable {        
    }

    public static void main(String[] args) {
        for( Filter filter: Filterable.class.getAnnotationsByType( Filter.class ) ) {
            System.out.println( filter.value() );
        }
    }
}
```

正如我们所见，这里的**Filter**类使用@Repeatable(Filters.class)注解修饰，而**Filters**是存放**Filter**注解的容器，编译器尽量对开发者屏蔽这些细节。这样，**Filterable**接口可以用两个**Filter**注解注释（这里并没有提到任何关于Filters的信息）。

另外，反射API提供了一个新的方法：**getAnnotationsByType()**，可以返回某个类型的重复注解，例如`Filterable.class.getAnnoation(Filters.class)`将返回两个Filter实例，输出到控制台的内容如下所示：

```
filter1
filter2
```

如果你希望了解更多内容，可以参考[官方文档](http://docs.oracle.com/javase/tutorial/java/annotations/repeating.html)。

## 2.5 更好的类型推断

Java 8编译器在类型推断方面有很大的提升，在很多场景下编译器可以推导出某个参数的数据类型，从而使得代码更为简洁。例子代码如下：

```
package com.javacodegeeks.java8.type.inference;

public class Value< T > {
    public static< T > T defaultValue() { 
        return null; 
    }

    public T getOrDefault( T value, T defaultValue ) {
        return ( value != null ) ? value : defaultValue;
    }
}
```

下列代码是**Value<String>**类型的应用：

```
package com.javacodegeeks.java8.type.inference;

public class TypeInference {
    public static void main(String[] args) {
        final Value< String > value = new Value<>();
        value.getOrDefault( "22", Value.defaultValue() );
    }
}
```

参数**Value.defaultValue()**的类型由编译器推导得出，不需要显式指明。在Java 7中这段代码会有编译错误，除非使用`Value.<String>defaultValue()`。

## 2.6 拓宽注解的应用场景

Java 8拓宽了注解的应用场景。现在，注解几乎可以使用在任何元素上：局部变量、接口类型、超类和接口实现类，甚至可以用在函数的异常定义上。下面是一些例子：

```
package com.javacodegeeks.java8.annotations;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.util.ArrayList;
import java.util.Collection;

public class Annotations {
    @Retention( RetentionPolicy.RUNTIME )
    @Target( { ElementType.TYPE_USE, ElementType.TYPE_PARAMETER } )
    public @interface NonEmpty {        
    }

    public static class Holder< @NonEmpty T > extends @NonEmpty Object {
        public void method() throws @NonEmpty Exception {            
        }
    }

    @SuppressWarnings( "unused" )
    public static void main(String[] args) {
        final Holder< String > holder = new @NonEmpty Holder< String >();        
        @NonEmpty Collection< @NonEmpty String > strings = new ArrayList<>();        
    }
}
```

**ElementType.TYPE_USER**和**ElementType.TYPE_PARAMETER**是Java 8新增的两个注解，用于描述注解的使用场景。Java 语言也做了对应的改变，以识别这些新增的注解。

# 3. Java编译器的新特性

## 3.1 参数名称

为了在运行时获得Java程序中方法的参数名称，老一辈的Java程序员必须使用不同方法，例如[Paranamer liberary](https://github.com/paul-hammant/paranamer)。Java 8终于将这个特性规范化，在语言层面（使用反射API和**Parameter.getName()方法**）和字节码层面（使用新的**javac**编译器以及**-parameters**参数）提供支持。

```
package com.javacodegeeks.java8.parameter.names;

import java.lang.reflect.Method;
import java.lang.reflect.Parameter;

public class ParameterNames {
    public static void main(String[] args) throws Exception {
        Method method = ParameterNames.class.getMethod( "main", String[].class );
        for( final Parameter parameter: method.getParameters() ) {
            System.out.println( "Parameter: " + parameter.getName() );
        }
    }
}
```

在Java 8中这个特性是默认关闭的，因此如果不带**-parameters**参数编译上述代码并运行，则会输出如下结果：

```
Parameter: arg0
```

如果带**-parameters**参数，则会输出如下结果（正确的结果）：

```
Parameter: args
```

如果你使用Maven进行项目管理，则可以在**maven-compiler-plugin**编译器的配置项中配置**-parameters**参数：

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerArgument>-parameters</compilerArgument>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
</plugin>
```

# 4. Java官方库的新特性

Java 8增加了很多新的工具类（date/time类），并扩展了现存的工具类，以支持现代的并发编程、函数式编程等。

**`java.util.Function`**包下都是函数式接口，我们可以直接拿来使用：

**BiConsumer**：表示接收两个输入参数和不返回结果的操作。 
**BiFunction**：表示接受两个参数，并产生一个结果的函数。 
**BinaryOperator**：表示在相同类型的两个操作数的操作，生产相同类型的操作数的结果。 
**BiPredicate**：代表两个参数谓词（布尔值函数）。 
**BooleanSupplier**：代表布尔值结果的提供者。 
**Consumer**：表示接受一个输入参数和不返回结果的操作。 
**DoubleBinaryOperator**：代表在两个double值操作数的运算，并产生一个double值结果。 
**DoubleConsumer**：表示接受一个double值参数，不返回结果的操作。 
**DoubleFunction**：表示接受double值参数，并产生一个结果的函数。 
**DoublePredicate**：代表一个double值参数谓词（布尔值函数）。 
**DoubleSupplier**：表示double值结果的提供者。 
**DoubleToIntFunction**：表示接受double值参数，并产生一个int值结果的函数。 
**DoubleToLongFunction**：代表接受一个double值参数，并产生一个long值结果的函数。 
**DoubleUnaryOperator**：表示上产生一个double值结果的单个double值操作数的操作。 
**Function**：表示接受一个参数，并产生一个结果的函数。 
**IntBinaryOperator**：表示对两个int值操作数的运算，并产生一个int值结果。 
**IntConsumer**：表示接受单个int值的参数并没有返回结果的操作。 
**IntFunction**：表示接受一个int值参数，并产生一个结果的函数。 
**IntPredicate**：表示一个整数值参数谓词（布尔值函数）。 
**IntSupplier**：代表整型值的结果的提供者。 
**IntToDoubleFunction**：表示接受一个int值参数，并产生一个double值结果的功能。 
**IntToLongFunction**：表示接受一个int值参数，并产生一个long值结果的函数。 
**IntUnaryOperator**：表示产生一个int值结果的单个int值操作数的运算。 
**LongBinaryOperator**： 
表示在两个long值操作数的操作，并产生一个long值结果。 
**LongConsumer**： 
表示接受一个long值参数和不返回结果的操作。 
**LongFunction**： 
表示接受long值参数，并产生一个结果的函数。 
**LongPredicate**： 
代表一个long值参数谓词（布尔值函数）。 
**LongSupplier**： 
表示long值结果的提供者。 
**LongToDoubleFunction**：表示接受double参数，并产生一个double值结果的函数。 
**LongToIntFunction**：表示接受long值参数，并产生一个int值结果的函数。 
**LongUnaryOperator**：表示上产生一个long值结果单一的long值操作数的操作。 
**ObjDoubleConsumer**：表示接受对象值和double值参数，并且没有返回结果的操作。 
**ObjIntConsumer**：表示接受对象值和整型值参数，并返回没有结果的操作。 
**ObjLongConsumer**：表示接受对象的值和long值的说法，并没有返回结果的操作。 
**Predicate**：代表一个参数谓词（布尔值函数）。 
**Supplier**：表示一个提供者的结果。 
**ToDoubleBiFunction**：表示接受两个参数，并产生一个double值结果的功能。 
**ToDoubleFunction**：代表一个产生一个double值结果的功能。 
**ToIntBiFunction**：表示接受两个参数，并产生一个int值结果的函数。 
**ToIntFunction**：代表产生一个int值结果的功能。 
**ToLongBiFunction**：表示接受两个参数，并产生long值结果的功能。 
**ToLongFunction**：代表一个产生long值结果的功能。 
**UnaryOperator**:表示上产生相同类型的操作数的结果的单个操作数的操作。

### 4.1Java 内置四大核心函数式接口
![](http://topweshare.qiniudn.com/1526209159.png?imageMogr2/thumbnail/!70p)




如果想了解更多的细节，请参考[官方文档](http://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)。
**Predicate接口** 
Predicate 接口只有一个参数，返回boolean类型。该接口包含多种默认方法来将Predicate组合成其他复杂的逻辑（比如：与，或，非）：

```
    public static void main(String[] args) {
        Predicate<String> predicate = (s) -> s.length() > 0;
        System.out.println(predicate.test("foo"));              // true
        System.out.println(predicate.negate().test("foo"));     // false
        Predicate<Boolean> nonNull = Objects::nonNull;
        Predicate<Boolean> isNull = Objects::isNull;
        Predicate<String> isEmpty = String::isEmpty;
        Predicate<String> isNotEmpty = isEmpty.negate();
        System.out.println(nonNull.test(null));
        System.out.println(isNull.test(null));
        System.out.println(isEmpty.test("sss"));
        System.out.println(isNotEmpty.test(""));
    }
```

**运行结果：** 
true 
false 
false 
true 
false 
false

**Function 接口** 
Function 接口有一个参数并且返回一个结果，并附带了一些可以和其他函数组合的默认方法（compose, andThen）：

```
        Function<String, Integer> toInteger = Integer::valueOf;
        System.out.println(toInteger.apply("123").getClass());
        Function<String, Object> toInteger2 = toInteger.andThen(String::valueOf);
        System.out.println(toInteger2.apply("123").getClass());
```

输出： 
class java.lang.Integer 
class java.lang.String

**Supplier 接口** 
Supplier 接口返回一个任意范型的值，和Function接口不同的是该接口没有任何参数

```
Supplier<Person> personSupplier = Person::new;
personSupplier.get();   // new Person
```

**Consumer 接口**

Consumer 接口表示执行在单个参数上的操作。接口只有一个参数，且无返回值

```
        Supplier<LambdaClassTest> personSupplier = LambdaClassTest::new;
        Consumer<LambdaClassTest> greeter = (lt) -> System.out.println("Hello, " + lt.getTest());
        greeter.accept(personSupplier.get());
```

**Comparator 接口**

Comparator 是老Java中的经典接口， Java 8在此之上添加了多种默认方法：

```
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);
Person p1 = new Person("John", "Doe");
Person p2 = new Person("Alice", "Wonderland");
comparator.compare(p1, p2);             // > 0
comparator.reversed().compare(p1, p2);  // < 0
```
## 4.2 Optional

Java应用中最常见的bug就是[空值异常](http://examples.javacodegeeks.com/java-basics/exceptions/java-lang-nullpointerexception-how-to-handle-null-pointer-exception/)。在Java 8之前，[Google Guava](http://code.google.com/p/guava-libraries/)引入了**Optionals**类来解决**NullPointerException**，从而避免源码被各种**null**检查污染，以便开发者写出更加整洁的代码。Java 8也将**Optional**加入了官方库。

**Optional**仅仅是一个容易：存放T类型的值或者null。它提供了一些有用的接口来避免显式的null检查，可以参考[Java 8官方文档](http://docs.oracle.com/javase/8/docs/api/)了解更多细节。
#### 常用的几个方法：

*   ofNullable 
    如果不为null, 则返回一个描述指定值的Optional；否则返回空的Optional
*   of 
    如果不为null, 则返回一个描述指定值的Optional；否则报异常
*   isPresent 
    如果有值存在，则返回TRUE，否则返回false。
*   orElse 
    如果有值，则返回当前值；如果没有，则返回`other`
*   get 
    如果有值，则返回当前值；否则返回`NoSuchElementException`

接下来看一点使用**Optional**的例子：可能为空的值或者某个类型的值：

```
Optional< String > fullName = Optional.ofNullable( null );
System.out.println( "Full Name is set? " + fullName.isPresent() );        
System.out.println( "Full Name: " + fullName.orElseGet( () -> "[none]" ) ); 
System.out.println( fullName.map( s -> "Hey " + s + "!" ).orElse( "Hey Stranger!" ) );
```

如果**Optional**实例持有一个非空值，则**isPresent()**方法返回true，否则返回false；**orElseGet()**方法，**Optional**实例持有null，则可以接受一个lambda表达式生成的默认值；**map()**方法可以将现有的**Opetional**实例的值转换成新的值；**orElse()**方法与**orElseGet()**方法类似，但是在持有null的时候返回传入的默认值。

上述代码的输出结果如下：

```
Full Name is set? false
Full Name: [none]
Hey Stranger!
```

再看下另一个简单的例子：

```
Optional< String > firstName = Optional.of( "Tom" );
System.out.println( "First Name is set? " + firstName.isPresent() );        
System.out.println( "First Name: " + firstName.orElseGet( () -> "[none]" ) ); 
System.out.println( firstName.map( s -> "Hey " + s + "!" ).orElse( "Hey Stranger!" ) );
System.out.println();
```

这个例子的输出是：

```
First Name is set? true
First Name: Tom
Hey Tom!
```
## 4.3 Streams

Stream 是 Java8 中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。使用Stream API 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。也可以使用 Stream API 来并行执行操作。简而言之，Stream API 提供了一种高效且易于使用的处理数据的方式。

### Stream

是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。 
“集合讲的是数据，流讲的是计算！” 
1\. Stream 自己不会存储元素。 
2\. Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。 
3\. Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

#### Stream 的操作三个步骤

*   创建 Stream 
    一个数据源（如：集合、数组），获取一个流
*   中间操作 
    一个中间操作链，对数据源的数据进行处理
*   终止操作(终端操作) 
    一个终止操作，执行中间操作链，并产生结果

### 创建 Stream

Java8 中的 Collection 接口被扩展，提供了两个获取流的方法： 
- default Stream stream() : 返回一个顺序流 
- default Stream parallelStream() : 返回一个并行流

Java8 中的 Arrays 的静态方法 stream() 可以获取数组流： 
- static Stream stream(T[] array): 返回一个流

可以使用静态方法 Stream.of(), 通过显示值创建一个流。它可以接收任意数量的参数。 
- public static Stream of(T… values) : 返回一个流

可以使用静态方法 Stream.iterate() 和Stream.generate(), 创建无限流。 
- 迭代 
public static Stream iterate(final T seed, final UnaryOperator f) 
- 生成 
public static Stream generate(Supplier s) :


```
    //创建Stream
    @Test
    public void test1(){
        //1.可以通过Collection系列集合提供的stream() 或parallelStream()
        List<String> list = new ArrayList<>();
        Stream<String> stream = list.stream();

        //2.通过Arrays中静态方法 stream() 获取数组流
        Person[] persons = new Person[10];
        Stream<Person> stream2 = Arrays.stream(persons);

        //3.通过Stream类中的静态方法 of()
        Stream<String> stream3 = Stream.of("a","b","c");

        //4.创建无限流
        //迭代
        Stream<Integer> stream4 = Stream.iterate(0, (x) -> x + 2);
        stream4.limit(8).forEach(System.out :: println);

        //生成
        Stream.generate(() -> Math.random()).limit(6)
            .forEach(System.out :: println);
    }       

```

### Stream 的中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。

#### 筛选与切片
![](http://topweshare.qiniudn.com/1526209375.png?imageMogr2/thumbnail/!70p)


#### 映射

![](http://i.imgur.com/LB1EtFk.png)

#### 排序

![](http://i.imgur.com/n1zBjpn.png)

```
    /**
     * Stream API的中间操作
     */
    public class TestSteamAPI2 {

        List<Person> persons = Arrays.asList(
                new Person(2, "钱四", 24),
                new Person(1, "张三", 33),
                new Person(2, "李四", 24),
                new Person(3, "王五", 65),
                new Person(4, "赵六", 26),
                new Person(4, "赵六", 26),
                new Person(5, "陈七", 27)
        );

        //内部迭代，由Stream API完成
        @Test
        public void test1(){
            //中间操作,不会执行任何操作
            Stream<Person> stream = persons.stream()
                                        .filter((e) -> {
                                            System.out.println("Stream的中间操作");
                                            return e.getAge() > 25;
                                        });
            //终止操作，一次性执行全部内容，即"惰性求值"
            stream.forEach(System.out :: println);
        }

        //外部迭代
        @Test
        public void test2(){
            Iterator<Person> iterator = persons.iterator();
            while (iterator.hasNext()) {
                System.out.println(iterator.next());
            }
        }

        //limit，截断
        @Test
        public void test3(){
            persons.stream()
                .filter((e) -> {
                    System.out.println("迭代操作"); //短路
                    return e.getAge() > 24;
                })
                .limit(2)
                .forEach(System.out :: println);
        }

        //跳过skip,distinct去重(要重写equals和hashcode)
        @Test
        public void test4(){
            persons.stream()
                    .filter((e) -> e.getAge() > 23)
                    .skip(2)
                    .distinct()
                    .forEach(System.out :: println);
        }

        //映射
        @Test
        public void test5(){
            List<String> list = Arrays.asList("a","bb","c","d","e");

            list.stream().map((str) -> str.toUpperCase())
                .forEach(System.out :: println);
            System.out.println("---------------");

            persons.stream().map((Person :: getName)).forEach(System.out :: println);
            System.out.println("---------------");

            Stream<Stream<Character>> stream = list.stream()
                .map(TestSteamAPI2 :: filterCharacter);

            stream.forEach((s) -> {
                s.forEach(System.out :: println);
            });
            System.out.println("-----------------");

            //flatMap
            Stream<Character> stream2 = list.stream()
                .flatMap(TestSteamAPI2 :: filterCharacter);
            stream2.forEach(System.out :: println);
        }

        //处理字符串
        public static Stream<Character> filterCharacter(String str){
            List<Character> list = new ArrayList<>();

            for (Character character : str.toCharArray()) {
                list.add(character);
            }
            return list.stream();
        }

        //排序
        @Test
        public void test6(){
            List<String> list = Arrays.asList("bb","c","aa","ee","ddd");

            list.stream()
                .sorted() //自然排序
                .forEach(System.out :: println);
            System.out.println("------------");

            persons.stream()
                    .sorted((p1,p2) -> {
                        if (p1.getAge() == p2.getAge()) {
                            return p1.getName().compareTo(p2.getName());
                        } else {
                            return p1.getAge() - p2.getAge();
                        }
                    }).forEach(System.out :: println);

        }

    }

```

### Stream 的终止操作

终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。

#### 查找与匹配

![](http://i.imgur.com/sOFwUKZ.png)

#### 归约

![](http://i.imgur.com/V60rHai.png)
![](http://i.imgur.com/vMjqEk2.png) 
map 和 reduce 的连接通常称为 map-reduce 模式。

#### 收集

![](http://i.imgur.com/J9US57R.png)
Collector 接口中方法的实现决定了如何对流执行收集操作(如收集到 List、Set、Map)。但是 Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例。

```
    /**
     * Stream API的终止操作
     */
    public class TestSteamAPI3 {

        List<Person> persons = Arrays.asList(
                new Person(2, "钱四", 24,Status.YOUNG),
                new Person(1, "张三", 23,Status.YOUNG),
                new Person(2, "李四", 24,Status.YOUNG),
                new Person(3, "王五", 65,Status.OLD),
                new Person(4, "赵六", 26,Status.MIDDLE),
                new Person(4, "赵六", 56,Status.OLD),
                new Person(5, "陈七", 27,Status.MIDDLE)
        );

        //查找与匹配
        @Test
        public void test1(){
            boolean b = persons.stream()
                                .allMatch((e) -> e.getStatus().equals(Status.YOUNG));
            System.out.println(b);

            boolean b2 = persons.stream()
                    .anyMatch((e) -> e.getStatus().equals(Status.YOUNG));
            System.out.println(b2);

            boolean b3 = persons.stream()
                    .noneMatch((e) -> e.getStatus().equals(Status.MIDDLE));
            System.out.println(b3);

            Optional<Person> op = persons.stream()
                    .sorted((e1,e2) -> Integer.compare(e1.getAge(), e2.getAge()))
                    .findFirst();
            System.out.println(op.get());

            Optional<Person> optional = persons.stream()
                    .filter((e) -> e.getStatus().equals(Status.OLD))
                    .findAny();
            System.out.println(optional.get());
        }

        //最大，最小
        @Test
        public void test2(){
            long count = persons.stream()
                    .count();
            System.out.println(count);

            Optional<Person> optional = persons.stream()
                    .max((e1,e2) -> Integer.compare(e1.getId(), e2.getId()));
            System.out.println(optional.get());

            Optional<Integer> op = persons.stream()
                    .map(Person :: getAge)
                    .min(Integer :: compare);
            System.out.println(op.get());
        }

        //归约
        @Test
        public void test3(){
            List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);

            Integer sum = list.stream()
                .reduce(0, (x,y) -> x + y);
            System.out.println(sum);
            System.out.println("------------");

            Optional<Integer> optional = persons.stream()
                    .map(Person :: getAge)
                    .reduce(Integer :: sum);
            System.out.println(optional.get());
        }

        //收集
        @Test
        public void test4(){
            List<String> list = persons.stream()
                    .map(Person :: getName)
                    .collect(Collectors.toList());
            list.forEach(System.out :: println);
            System.out.println("------------");

            Set<String> set = persons.stream()
                    .map(Person :: getName)
                    .collect(Collectors.toSet());
            set.forEach(System.out :: println);
            System.out.println("------------");

            HashSet<String> hashSet = persons.stream()
                    .map(Person :: getName)
                    .collect(Collectors.toCollection(HashSet :: new));
            hashSet.forEach(System.out :: println);
        }

        @Test
        public void test5(){
            Long count = persons.stream()
                    .collect(Collectors.counting());
            System.out.println("总人数="+count);
            System.out.println("----------------");

            //平均值
            Double avg = persons.stream()
                    .collect(Collectors.averagingInt(Person :: getAge));
            System.out.println("平均年龄="+avg);
            System.out.println("---------------");

            //总和
            Integer sum = persons.stream()
                    .collect(Collectors.summingInt(Person :: getAge));
            System.out.println("年龄总和="+sum);
            System.out.println("----------------");

            //最大值
            Optional<Person> max = persons.stream()
                    .collect(Collectors.maxBy((e1,e2) -> Integer.compare(e1.getAge(), e2.getAge())));
            System.out.println("最大年龄是"+max.get());
            System.out.println("----------------");

            //最小值
            Optional<Person> min = persons.stream()
                    .collect(Collectors.minBy((e1,e2) -> Integer.compare(e1.getAge(), e2.getAge())));
            System.out.println("最小年龄是"+min.get());
        }

        //分组
        @Test
        public void test6(){
            Map<Status, List<Person>> map = persons.stream()
                    .collect(Collectors.groupingBy(Person :: getStatus));//根据年龄层分组
            System.out.println(map);
        }

        //多级分组
        @Test
        public void test7(){
            Map<Status, Map<String, List<Person>>> map = persons.stream()
                    .collect(Collectors.groupingBy(Person :: getStatus ,Collectors.groupingBy((e) -> {
                        if (e.getId()%2 == 1) {
                            return "单号";
                        } else {
                            return "双号";
                        } 
                    })));
            System.out.println(map);
        }

        //分区
        @Test
        public void test8(){
            Map<Boolean, List<Person>> map = persons.stream()
                    .collect(Collectors.partitioningBy((e) -> e.getAge() > 30));
            System.out.println(map);
        }

        //IntSummaryStatistics
        @Test
        public void test9(){
            IntSummaryStatistics iss = persons.stream()
                    .collect(Collectors.summarizingInt(Person :: getAge));
            System.out.println(iss.getSum());
            System.out.println(iss.getAverage());
            System.out.println(iss.getMax());
        }

        @Test
        public void test10(){
            String str = persons.stream()
                    .map(Person :: getName)
                    .collect(Collectors.joining(",","人员名单：","等"));
            System.out.println(str);
        }
    }

```

### 并行流与串行流

并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。 
Java 8 中将并行进行了优化，我们可以很容易的对数据进行并行操作。Stream API 可以声明性地通过 parallel() 与sequential() 在并行流与顺序流之间进行切换。

```
    /**
     * FrokJoin框架
     */
    public class ForkJoinCalculate extends RecursiveTask<Long>{
        private static final long serialVersionUID = 1L;

        private long start;
        private long end;

        private static final long THRESHOLD = 10000;

        public ForkJoinCalculate() {

        }

        public ForkJoinCalculate(long start, long end) {
            this.start = start;
            this.end = end;
        }

        @Override
        protected Long compute() {
            long length = end - start ;
            if (length <= THRESHOLD) {
                long sum = 0;
                for (long i = start; i <= end; i++) {
                    sum += i;
                }
                return sum;
            }else {
                long middle = (start + end) / 2; 
                ForkJoinCalculate left = new ForkJoinCalculate();
                left.fork();//拆分子任务，同时压入线程队列

                ForkJoinCalculate right = new ForkJoinCalculate();
                right.fork();
                return left.join() + right.join();
            }
        }

    }

```

测试并行流

```
    public class TestForkJoin {

        /**
         * FrokJoin框架
         */
        @Test
        public void test1(){
            Instant start = Instant.now();

            ForkJoinPool pool = new ForkJoinPool();
            ForkJoinTask<Long> task = new ForkJoinCalculate(0,10000000000L);
            Long sum = pool.invoke(task);
            System.out.println(sum);

            Instant end = Instant.now();
            System.out.println(Duration.between(start,end).toMillis());
        }

        /**
         * for循环
         */
        @Test
        public void test2(){
            Instant start = Instant.now();
            long sum = 0L;

            for (long i = 0; i <= 10000000000L; i++) {
                sum += i;
            }
            System.out.println(sum);

            Instant end = Instant.now();
            System.out.println(Duration.between(start, end).toMillis());
        }

        /**
         * Java8并行流
         */
        @Test
        public void test3(){
            Instant start = Instant.now();
            LongStream.rangeClosed(0, 10000000000L)
                        .parallel()
                        .reduce(0,Long :: sum);
            Instant end = Instant.now();
            System.out.println(Duration.between(start, end).toMillis());
        }
    }
```


然后我们计算一下排序这个Stream要耗时多久， 
**串行排序：**

```
long t0 = System.nanoTime();
long count = values.stream().sorted().count();
System.out.println(count);
long t1 = System.nanoTime();
long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("sequential sort took: %d ms", millis));
```

// 串行耗时: 899 ms 
**并行排序：**

```
long t0 = System.nanoTime();
long count = values.parallelStream().sorted().count();
System.out.println(count);
long t1 = System.nanoTime();
long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("parallel sort took: %d ms", millis));
```

// 并行排序耗时: 472 ms 
上面两个代码几乎是一样的，但是并行版的快了50%之多，唯一需要做的改动就是**将stream()改为parallelStream()**；


### **steam在实际项目中使用的代码片段：**

```
//1、有list集合生成以productId为key值得map集合
Map<String, List<CartManager>> cartManagerGroup =
        carts.stream().collect(
                Collectors.groupingBy(CartManager::getProductId)
        );
//2、取得购物车中数量之和
IntStream  is = list.stream().mapToInt((CartManager c)->c.getQuantity()); 
is.sum();//数量之和

//3、所有订单中商品数量*订单金额求和
orderDetailsNew.parallelStream()
                            .mapToDouble(orderDetailMid -> orderDetailMid.getQuantity()*orderDetailMid.getFinalPrice()).sum()

//4、过滤出指定类型的订单，并生成新的集合
orderDetails.stream().
    filter(orderDetail ->    StringUtil.isEmpty(orderDetail.getPromotionsType())|| !orderDetail.getPromotionsType().equals(PromotionTypeEnum.ORDERGIFTPROMOTION.getType())).collect(Collectors.toList());

//5、过滤购物车未被选中商品并生成新的list
carts.stream().filter(cart -> cart.getSelectFlag()==1).collect(Collectors.toList());

//6、将list以商品促销委key转化为map
Map<String,List<PromotionsGiftProduct>> map = 
                promotionsGiftProducts.stream().collect(                    Collectors.groupingBy(PromotionsGiftProduct::getPromotionId));

//7、从list<Cart>中分离出只存储productId的列表list<String>
List<String> productIds = needUpdate.parallelStream()
                        .map(CartManager::getProductId)
                        .collect(Collectors.toList());
```

## 4.3 ## Java 8日期/时间API

Java 8日期/时间API是JSR-310的实现，它的实现目标是克服旧的日期时间实现中所有的缺陷，新的日期/时间API的一些设计原则是：

*   **不变性**：新的日期/时间API中，所有的类都是不可变的，这对多线程环境有好处。
*   **关注点分离**：新的API将人可读的日期时间和机器时间（unix timestamp）明确分离，它为日期（Date）、时间（Time）、日期时间（DateTime）、时间戳（unix timestamp）以及时区定义了不同的类。
*   **清晰**：在所有的类中，方法都被明确定义用以完成相同的行为。举个例子，要拿到当前实例我们可以使用now()方法，在所有的类中都定义了format()和parse()方法，而不是像以前那样专门有一个独立的类。为了更好的处理问题，所有的类都使用了工厂模式和策略模式，一旦你使用了其中某个类的方法，与其他类协同工作并不困难。
*   **实用操作**：所有新的日期/时间API类都实现了一系列方法用以完成通用的任务，如：加、减、格式化、解析、从日期/时间中提取单独部分，等等。
*   **可扩展性**：新的日期/时间API是工作在ISO-8601日历系统上的，但我们也可以将其应用在非IOS的日历上。 
    Java日期/时间API包

## Java日期/时间API包含以下相应的包。

*   **`java.time`**包：这是新的Java日期/时间API的基础包，所有的主要基础类都是这个包的一部分，如：LocalDate, LocalTime, LocalDateTime, Instant, Period, Duration等等。所有这些类都是不可变的和线程安全的，在绝大多数情况下，这些类能够有效地处理一些公共的需求。
*   **`java.time.chrono`**包：这个包为非ISO的日历系统定义了一些泛化的API，我们可以扩展AbstractChronology类来创建自己的日历系统。
*   **`java.time.format`**包：这个包包含能够格式化和解析日期时间对象的类，在绝大多数情况下，我们不应该直接使用它们，因为java.time包中相应的类已经提供了格式化和解析的方法。
*   **`java.time.temporal`**包：这个包包含一些时态对象，我们可以用其找出关于日期/时间对象的某个特定日期或时间，比如说，可以找到某月的第一天或最后一天。你可以非常容易地认出这些方法，因为它们都具有“withXXX”的格式。
*   **`java.time.zone`**包：这个包包含支持不同时区以及相关规则的类

## Clock

Clock类，它通过指定一个时区，然后就可以获取到当前的时刻，日期与时间。Clock可以替换`System.currentTimeMillis()`与`TimeZone.getDefault()`。

```java
Clock clock = Clock.systemUTC();
System.out.println( clock.instant() );
System.out.println( clock.millis() );123
```

输出：

```
2017-03-14T08:22:08.019Z
1489479728168
```

## 日期时间处理

> LocalDateTime类包含了LocalDate和LocalTime的信息，但是**不包含ISO-8601日历系统中的时区信息**

```java
@Test
    public void testLocalDate(){
        // Get the current date and time
        LocalDateTime currentTime = LocalDateTime.now();
        System.out.println("Current DateTime: " + currentTime);

        LocalDate date1 = currentTime.toLocalDate();
        System.out.println("date1: " + date1);
        Month month = currentTime.getMonth();
        int day = currentTime.getDayOfMonth();
        int seconds = currentTime.getSecond();
        System.out.println("Month: " + month
                +"  day: " + day
                +"  seconds: " + seconds
        );

        //指定时间
        LocalDateTime date2 = currentTime.withDayOfMonth(10).withYear(2012);
        System.out.println("date2: " + date2);

        //12 december 2014
        LocalDate date3 = LocalDate.of(2014, Month.DECEMBER, 12);
        System.out.println("date3: " + date3);

        //22 hour 15 minutes
        LocalTime date4 = LocalTime.of(22, 15);
        System.out.println("date4: " + date4);

        //parse a string
        LocalTime date5 = LocalTime.parse("20:15:30");
        System.out.println("date5: " + date5);
    }1234567891011121314151617181920212223242526272829303132
```

输出：

```
Current DateTime: 2017-03-14T16:23:08.997
date1: 2017-03-14
Month: MARCH  day: 14  seconds: 8
date2: 2012-03-10T16:23:08.997
date3: 2014-12-12
date4: 22:15
date5: 20:15:30
```

## 带时区日期时间处理–ZonedDateTime

```java
@Test
    public void testZonedDateTime(){
        // Get the current date and time
        ZonedDateTime now = ZonedDateTime.now();
        System.out.println("now: " + now);
        ZonedDateTime date1 = ZonedDateTime.parse("2007-12-03T10:15:30+05:30[Asia/Karachi]");
        System.out.println("date1: " + date1);
        ZoneId id = ZoneId.of("Europe/Paris");
        System.out.println("ZoneId: " + id);
        ZoneId currentZone = ZoneId.systemDefault();
        System.out.println("CurrentZone: " + currentZone);
    }123456789101112
```

输出：

```
now: 2017-03-15T09:33:10.108+08:00[Asia/Shanghai]
date1: 2007-12-03T10:15:30+05:00[Asia/Karachi]
ZoneId: Europe/Paris
CurrentZone: Asia/Shanghai
```

## ChronoUnit

可以代替`Calendar`的日期操作

```java
@Test
    public void testChromoUnits(){
        //Get the current date
        LocalDate today = LocalDate.now();
        System.out.println("Current date: " + today);
        //add 1 week to the current date
        LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS);
        System.out.println("Next week: " + nextWeek);
        //add 1 month to the current date
        LocalDate nextMonth = today.plus(1, ChronoUnit.MONTHS);
        System.out.println("Next month: " + nextMonth);
        //add 1 year to the current date
        LocalDate nextYear = today.plus(1, ChronoUnit.YEARS);
        System.out.println("Next year: " + nextYear);
        //add 10 years to the current date
        LocalDate nextDecade = today.plus(1, ChronoUnit.DECADES);
        //add 20 years to the current date
        LocalDate nextDecade20 = today.plus(2, ChronoUnit.DECADES);
        System.out.println("Date after 20 year: " + nextDecade20);
    }1234567891011121314151617181920
```

输出：

```
Current date: 2017-03-15
Next week: 2017-03-22
Next month: 2017-04-15
Next year: 2018-03-15
Date after 20 year: 2037-03-15
```

## Period/Duration

使用Java8，两个专门类引入来处理**时间差。**

*   Period - 处理有关基于时间的日期数量。
*   Duration - 处理有关基于时间的时间量。

```java
@Test
    public void testPeriod(){
        //Get the current date
        LocalDate date1 = LocalDate.now();
        System.out.println("Current date: " + date1);

        //add 1 month to the current date
        LocalDate date2 = date1.plus(3, ChronoUnit.DAYS);
        System.out.println("Next month: " + date2);

        Period period = Period.between(date1, date2);
        System.out.println("Period: " + period);
        System.out.println("Period.getYears: " + period.getYears());
        System.out.println("Period.getMonths: " + period.getMonths());
        System.out.println("Period.getDays: " + period.getDays());
    }
    @Test
    public void testDuration(){
        LocalTime time1 = LocalTime.now();
        Duration twoHours = Duration.ofHours(2);

        System.out.println("twoHours.getSeconds(): " + twoHours.getSeconds());

        LocalTime time2 = time1.plus(twoHours);
        System.out.println("time2: " + time2);

        Duration duration = Duration.between(time1, time2);
        System.out.println("Duration: " + duration);
        System.out.println("Duration.getSeconds: " + duration.getSeconds());
    }123456789101112131415161718192021222324252627282930
```

比较简单，读者自行运行查看结果

## TemporalAdjuster

TemporalAdjuster 是做日期数学计算。例如，要获得“本月第二个星期六”或“下周二”。

```java
@Test
    public void testAdjusters(){
        //Get the current date
        LocalDate date1 = LocalDate.now();
        System.out.println("Current date: " + date1);

        //get the next tuesday
        LocalDate nextTuesday = date1.with(TemporalAdjusters.next(DayOfWeek.TUESDAY));
        System.out.println("Next Tuesday on : " + nextTuesday);

//        获得当月的第二个周六
        LocalDate firstInMonth = LocalDate.of(date1.getYear(),date1.getMonth(), 1);
        LocalDate secondSaturday = firstInMonth.with(
                TemporalAdjusters.nextOrSame(DayOfWeek.SATURDAY)).with(
                TemporalAdjusters.next(DayOfWeek.SATURDAY));
        System.out.println("Second saturday on : " + secondSaturday);
    }1234567891011121314151617
```

输出：

```java
Current date: 2017-03-15
Next Tuesday on : 2017-03-21
Second saturday on : 2017-03-11123
```

## 旧版本Date类的toInstant方法

toInstant()方法被添加到可用于将它们转换到新的日期时间的API原始日期和日历对象。使用ofInstant(Insant,ZoneId)方法得到一个LocalDateTime或ZonedDateTime对象。

```java
@Test
    public void testBackwardCompatability(){
        //Get the current date
        Date currentDate = new Date();
        System.out.println("Current date: " + currentDate);

        //Get the instant of current date in terms of milliseconds
        Instant now = currentDate.toInstant();
        ZoneId currentZone = ZoneId.systemDefault();

        LocalDateTime localDateTime = LocalDateTime.ofInstant(now, currentZone);
        System.out.println("Local date: " + localDateTime);

        ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(now, currentZone);
        System.out.println("Zoned date: " + zonedDateTime);
    }12345678910111213141516
```

输出：

```
Current date: Wed Mar 15 09:51:17 CST 2017
Local date: 2017-03-15T09:51:17.745
Zoned date: 2017-03-15T09:51:17.745+08:00[Asia/Shanghai]
```

`java.util.Date#from`方法可以将`Instant`转化为旧版本的Date对象。这两个方法都是jdk8以后新加的。

## DateTimeFormatter

DateTimeFormatter可以替代以前的dateformat，使用起来方便一些。

> 注意DateTimeFormatter是**线程安全**的，因为他是final的类

```
@Test
    public void testDateTimeFormatter(){
        LocalDateTime currentDate = LocalDateTime.now();
        System.out.println("Current date: " + currentDate);

        System.out.println(currentDate.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));
        System.out.println(DateTimeFormatter.ISO_LOCAL_DATE_TIME.format(currentDate));

        System.out.println(currentDate.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG)));
        System.out.println(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG).format(currentDate));

        System.out.println(currentDate.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        System.out.println(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").format(currentDate));
    }
```

输出：

```
Current date: 2017-03-15T11:22:25.026
2017-03-15T11:22:25.026
2017-03-15T11:22:25.026
2017年3月15日 上午11时22分25秒
2017年3月15日 上午11时22分25秒
2017-03-15 11:22:25
2017-03-15 11:22:25
```
## 4.4 Nashorn JavaScript引擎

Java 8提供了新的[Nashorn JavaScript引擎](http://www.javacodegeeks.com/2014/02/java-8-compiling-lambda-expressions-in-the-new-nashorn-js-engine.html)，使得我们可以在JVM上开发和运行JS应用。Nashorn JavaScript引擎是javax.script.ScriptEngine的另一个实现版本，这类Script引擎遵循相同的规则，允许Java和JavaScript交互使用，例子代码如下：

```
ScriptEngineManager manager = new ScriptEngineManager();
ScriptEngine engine = manager.getEngineByName( "JavaScript" );

System.out.println( engine.getClass().getName() );
System.out.println( "Result:" + engine.eval( "function f() { return 1; }; f() + 1;" ) );
```

这个代码的输出结果如下：

```
jdk.nashorn.api.scripting.NashornScriptEngine
Result: 2
```

## 4.5 Base64

[对Base64编码的支持](http://www.javacodegeeks.com/2014/04/base64-in-java-8-its-not-too-late-to-join-in-the-fun.html)已经被加入到Java 8官方库中，这样不需要使用第三方库就可以进行Base64编码，例子代码如下：

```
package com.javacodegeeks.java8.base64;

import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class Base64s {
    public static void main(String[] args) {
        final String text = "Base64 finally in Java 8!";

        final String encoded = Base64
            .getEncoder()
            .encodeToString( text.getBytes( StandardCharsets.UTF_8 ) );
        System.out.println( encoded );

        final String decoded = new String( 
            Base64.getDecoder().decode( encoded ),
            StandardCharsets.UTF_8 );
        System.out.println( decoded );
    }
}
```

这个例子的输出结果如下：

```
QmFzZTY0IGZpbmFsbHkgaW4gSmF2YSA4IQ==
Base64 finally in Java 8!
```

新的Base64API也支持URL和MINE的编码解码。
(**Base64._getUrlEncoder_()** / **Base64._getUrlDecoder_()**, **Base64._getMimeEncoder_()** / **Base64._getMimeDecoder_()**)。

## 4.6 并行数组

Java8版本新增了很多新的方法，用于支持并行数组处理。最重要的方法是**parallelSort()**，可以显著加快多核机器上的数组排序。下面的例子论证了**parallexXxx**系列的方法：

```
package com.javacodegeeks.java8.parallel.arrays;

import java.util.Arrays;
import java.util.concurrent.ThreadLocalRandom;

public class ParallelArrays {
    public static void main( String[] args ) {
        long[] arrayOfLong = new long [ 20000 ];        

        Arrays.parallelSetAll( arrayOfLong, 
            index -> ThreadLocalRandom.current().nextInt( 1000000 ) );
        Arrays.stream( arrayOfLong ).limit( 10 ).forEach( 
            i -> System.out.print( i + " " ) );
        System.out.println();

        Arrays.parallelSort( arrayOfLong );        
        Arrays.stream( arrayOfLong ).limit( 10 ).forEach( 
            i -> System.out.print( i + " " ) );
        System.out.println();
    }
}
```

上述这些代码使用**parallelSetAll()**方法生成20000个随机数，然后使用**parallelSort()**方法进行排序。这个程序会输出乱序数组和排序数组的前10个元素。上述例子的代码输出的结果是：

```
Unsorted: 591217 891976 443951 424479 766825 351964 242997 642839 119108 552378 
Sorted: 39 220 263 268 325 607 655 678 723 793
```

## 4.7 并发性

基于新增的lambda表达式和steam特性，为Java 8中为**java.util.concurrent.ConcurrentHashMap**类添加了新的方法来支持聚焦操作；另外，也为**java.util.concurrentForkJoinPool**类添加了新的方法来支持通用线程池操作（更多内容可以参考[我们的并发编程课程](http://academy.javacodegeeks.com/course/java-concurrency-essentials/)）。

Java 8还添加了新的**java.util.concurrent.locks.StampedLock**类，用于支持基于容量的锁——该锁有三个模型用于支持读写操作（可以把这个锁当做是**java.util.concurrent.locks.ReadWriteLock**的替代者）。

在**java.util.concurrent.atomic**包中也新增了不少工具类，列举如下：

*   DoubleAccumulator
*   DoubleAdder
*   LongAccumulator
*   LongAdder
# CAS原子类增强：LongAddr

LongAddr是高性能的原子计数器，比atomicLong性能还要高。 
关于atomicLong请参照[java高并发：CAS无锁原理及广泛应用](http://blog.csdn.net/liubenlong007/article/details/53761730)。 
关于LongAddr的实现原理，简单来说就是解决了**伪共享**的问题。具体我会单独再写一篇文章讲解这个**伪共享**以及LongAddr的实现原理。

先忙将对多线程下普通加锁，atomicLong，atomicLong三种原子计数性能进行一下比较：

```
package LongAddr;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * Created by liubenlong on 2017/1/22.
 * LongAddr： 比AtomicLong性能更高的原子类
 *
 * 本例比较加锁，AtomicLong，LongAddr  三种无锁
 */
public class LongAddrTest {
    static long maxValue = 10000000;
    static CountDownLatch countDownLatch = new CountDownLatch(10);
    public volatile long count = 0;

    public synchronized void incr(){
        if(getValue() < maxValue) count++;
    }

    public long getValue(){
        return count;
    }

    public static void main(String[] args) throws InterruptedException {

        LongAddrTest longAddrTest = new LongAddrTest();

        long begin = System.currentTimeMillis();
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        for(int i = 0 ; i < 10 ; i ++){
            executorService.execute(() -> {
                while(longAddrTest.getValue() < maxValue){
                    longAddrTest.incr();
                }
                countDownLatch.countDown();
            });
        }

        countDownLatch.await();
        executorService.shutdown();

        System.out.println("总共耗时:"+(System.currentTimeMillis() - begin)+"毫秒, count="+longAddrTest.getValue());
    }
}

```

```
package LongAddr;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.AtomicLong;

/**
 * Created by liubenlong on 2017/1/22.
 * LongAddr： 比AtomicLong性能更高的原子类
 *
 * 本例比较加锁，AtomicLong，LongAddr  三种无锁
 */
public class LongAddrTest1 {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(10);
        AtomicLong atomicLong = new AtomicLong(0);

        long maxValue = 10000000;
        long begin = System.currentTimeMillis();
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        for(int i = 0 ; i < 10 ; i ++){
            executorService.execute(() -> {
                while(atomicLong.get() < maxValue){
                    atomicLong.getAndIncrement();
                }
                countDownLatch.countDown();
            });
        }

        countDownLatch.await();
        executorService.shutdown();

        System.out.println("总共耗时:"+(System.currentTimeMillis() - begin)+"毫秒, count="+atomicLong.get());
    }
}

```

```
package LongAddr;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.LongAdder;

/**
 * Created by liubenlong on 2017/1/22.
 * LongAddr： 比AtomicLong性能更高的原子类
 *
 * 本例比较加锁，AtomicLong，LongAddr  三种无锁
 */
public class LongAddrTest2 {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(10);
        LongAdder longAdder = new LongAdder();

        long maxValue = 10000000;
        long begin = System.currentTimeMillis();
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        for(int i = 0 ; i < 10 ; i ++){
            executorService.execute(() -> {
                while(longAdder.sum() < maxValue){
                    longAdder.increment();
                }
                countDownLatch.countDown();
            });
        }

        countDownLatch.await();
        executorService.shutdown();

        System.out.println("总共耗时:"+(System.currentTimeMillis() - begin)+"毫秒, count="+longAdder.sum());
    }
}

```

上面三个类执行结果可以看出来，AtomicLong，LongAddr两个比加锁性能提高了很多，LongAddr又比AtomicLong性能高。所以我们以后在遇到线程安全的原子计数器的时候，首先考虑LongAddr。
# 使用CompletableFuture构建异步应用

## Future 接口的局限性

`Future`接口可以构建异步应用，但依然有其局限性。它很难直接表述多个Future 结果之间的依赖性。实际开发中，我们经常需要达成以下目的：

*   将两个异步计算合并为一个——这两个异步计算之间相互独立，同时第二个又依赖于第一个的结果。
*   等待 Future 集合中的所有任务都完成。
*   仅等待 Future集合中最快结束的任务完成（有可能因为它们试图通过不同的方式计算同一个值），并返回它的结果。
*   通过编程方式完成一个Future任务的执行（即以手工设定异步操作结果的方式）。
*   应对 Future 的完成事件（即当 Future 的完成事件发生时会收到通知，并能使用 Future 计算的结果进行下一步的操作，不只是简单地阻塞等待操作的结果）

新的CompletableFuture类将使得这些成为可能。

## CompletableFuture

JDK1.8才新加入的一个实现类`CompletableFuture`，实现了`Future<T>`, `CompletionStage<T>`两个接口。

当一个Future可能需要显示地完成时，使用`CompletionStage`接口去支持完成时触发的函数和操作。

当两个及以上线程同时尝试完成、异常完成、取消一个`CompletableFuture`时，只有一个能成功。

`CompletableFuture`实现了`CompletionStage`接口的如下策略：

1.  为了完成当前的`CompletableFuture`接口或者其他完成方法的回调函数的线程，提供了非异步的完成操作。

2.  没有显式入参`Executor`的所有`async`方法都使用`ForkJoinPool.commonPool()`为了简化监视、调试和跟踪，所有生成的异步任务都是标记接口`AsynchronousCompletionTask`的实例。

3.  所有的`CompletionStage`方法都是独立于其他共有方法实现的，因此一个方法的行为不会受到子类中其他方法的覆盖。

`CompletableFuture`实现了`Futurre`接口的如下策略：

1.  `CompletableFuture`无法直接控制完成，所以`cancel`操作被视为是另一种异常完成形式。方法`isCompletedExceptionally`可以用来确定一个`CompletableFuture`是否以任何异常的方式完成。

2.  以一个`CompletionException`为例，方法`get()`和`get(long,TimeUnit)`抛出一个`ExecutionException`，对应`CompletionException`。为了在大多数上下文中简化用法，这个类还定义了方法`join()`和`getNow`，而不是直接在这些情况中直接抛出`CompletionException`。

`CompletableFuture`中4个异步执行任务静态方法：

```
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier) {
        return asyncSupplyStage(asyncPool, supplier);
    }

public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier,Executor executor) {
    return asyncSupplyStage(screenExecutor(executor), supplier);
}

public static CompletableFuture<Void> runAsync(Runnable runnable) {
    return asyncRunStage(asyncPool, runnable);
}

public static CompletableFuture<Void> runAsync(Runnable runnable, Executor executor) {
    return asyncRunStage(screenExecutor(executor), runnable);
}
```

其中`supplyAsync`用于有返回值的任务，`runAsync`则用于没有返回值的任务。`Executor`参数可以手动指定线程池，否则默认`ForkJoinPool.commonPool()`系统级公共线程池， 
注意：**这些线程都是Daemon线程，主线程结束Daemon线程不结束，只有JVM关闭时，生命周期终止**。

## 异常处理

CompletableFuture实现了Future接口，因此你可以像Future那样使用它。

其次，CompletableFuture并非一定要交给线程池执行才能实现异步，你可以像下面这样实现异步运行：

```
@Test
public void test1() throws ExecutionException, InterruptedException {
    CompletableFuture<String> completableFuture = new CompletableFuture<>();
    new Thread(() -> {
        // 模拟执行耗时任务
        System.out.println("task doing...");
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 告诉completableFuture任务已经完成
        completableFuture.complete("ok");
    }).start();
    // 获取任务结果，如果没有完成会一直阻塞等待
    String result = completableFuture.get();
    System.out.println("计算结果:" + result);
}
```

如果没有意外，上面发的代码工作得很正常。但是，如果任务执行过程中产生了异常会怎样呢？

非常不幸，这种情况下你会得到一个相当糟糕的结果：异常会被限制在执行任务的线程的范围内，最终会杀死该线程，而这会导致等待`get`方法返回结果的线程永久地被阻塞。

客户端可以使用重载版本的`get` 方法，它使用一个超时参数来避免发生这样的情况。这是一种值得推荐的做法，你应该尽量在你的代码中添加超时判断的逻辑，避免发生类似的问题。

使用这种方法至少能防止程序永久地等待下去，超时发生时，程序会得到通知发生了`TimeoutException` 。不过，也因为如此，你不能确定执行任务的线程内到底发生了什么问题。

为了能获取任务线程内发生的异常，你需要使用 
CompletableFuture的`completeExceptionally`方法将导致CompletableFuture内发生问题的异常抛出。这样，当执行任务发生异常时，调用`get()`方法的线程将会收到一个 `ExecutionException`异常，该异常接收了一个包含失败原因的Exception 参数。

```
@Test
public void test2() throws ExecutionException, InterruptedException {
    CompletableFuture<String> completableFuture = new CompletableFuture<>();
    new Thread(() -> {
        // 模拟执行耗时任务
        System.out.println("task doing...");
        try {
            Thread.sleep(3000);
            int i = 1/0;
        } catch (Exception e) {
            // 告诉completableFuture任务发生异常了
            completableFuture.completeExceptionally(e);
        }
        // 告诉completableFuture任务已经完成
        completableFuture.complete("ok");
    }).start();
    // 获取任务结果，如果没有完成会一直阻塞等待
    String result = completableFuture.get();
    System.out.println("计算结果:" + result);
}
```

## 举个栗子

JDK CompletableFuture 自带多任务组合方法allOf和anyOf

`allOf`是等待所有任务完成，构造后CompletableFuture完成

`anyOf`是只要有一个任务完成，构造后CompletableFuture就完成

其它方法的中文解释查看此文☞ [https://www.jianshu.com/p/6f3ee90ab7d3](https://www.jianshu.com/p/6f3ee90ab7d3)

```
public class CompletableFutureDemo {
    public static void main(String[] args) {
        Long start = System.currentTimeMillis();
        // 结果集
        List<String> list = new ArrayList<>();

        ExecutorService executorService = Executors.newFixedThreadPool(10);

        List<Integer> taskList = Arrays.asList(2, 1, 3, 4, 5, 6, 7, 8, 9, 10);
        // 全流式处理转换成CompletableFuture[]+组装成一个无返回值CompletableFuture，join等待执行完毕。返回结果whenComplete获取
        CompletableFuture[] cfs = taskList.stream()
                .map(integer -> CompletableFuture.supplyAsync(() -> calc(integer), executorService)
                                .thenApply(h->Integer.toString(h))
                                .whenComplete((s, e) -> {
                                    System.out.println("任务"+s+"完成!result="+s+"，异常 e="+e+","+new Date());
                                    list.add(s);
                                })
                ).toArray(CompletableFuture[]::new);
        // 封装后无返回值，必须自己whenComplete()获取
        CompletableFuture.allOf(cfs).join();
        System.out.println("list="+list+",耗时="+(System.currentTimeMillis()-start));
    }

    public static Integer calc(Integer i) {
        try {
            if (i == 1) {
                Thread.sleep(3000);//任务1耗时3秒
            } else if (i == 5) {
                Thread.sleep(5000);//任务5耗时5秒
            } else {
                Thread.sleep(1000);//其它任务耗时1秒
            }
            System.out.println("task线程：" + Thread.currentThread().getName()
                    + "任务i=" + i + ",完成！+" + new Date());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return i;
    }
}
```

## 常用多线程并发，取结果归集的几种实现方案

| 描述 | Future | FutureTask | CompletionService | CompletableFuture |
| --- | --- | --- | --- | --- |
| 原理 | Future接口 | 接口RunnableFuture的唯一实现类，RunnableFuture接口继承自Future+Runnable | 内部通过阻塞队列+FutureTask接口 | JDK8实现了Future, CompletionStage两个接口 |
| 多任务并发执行 | 支持 | 支持 | 支持 | 支持 |
| 获取任务结果的顺序 | 按照提交顺序获取结果 | 未知 | 支持任务完成的先后顺序 | 支持任务完成的先后顺序 |
| 异常捕捉 | 自己捕捉 | 自己捕捉 | 自己捕捉 | 原生API支持，返回每个任务的异常 |
| 建议 | CPU高速轮询，耗资源，可以使用，但不推荐 | 功能不对口，并发任务这一块多套一层，不推荐使用 | **推荐使用，没有JDK8CompletableFuture之前最好的方案，没有质疑** | **API极端丰富，配合流式编程，速度飞起，推荐使用！** |

### HashMap

HashMap:减少碰撞,位置相同时,条件达到链表上超过8个,总数超过64个时,数据结构改为红黑树。

ConcurrentHashMap:取消锁分段，与HashMap相同，达到条件时，数据结构改为红黑树。
# 5\. 新的Java工具

Java 8提供了一些新的命令行工具，这部分会讲解一些对开发者最有用的工具。

## 5.1 Nashorn引擎：jjs

**jjs**是一个基于标准Nashorn引擎的命令行工具，可以接受js源码并执行。例如，我们写一个**func.js**文件，内容如下：

```
function f() { 
     return 1; 
}; 

print( f() + 1 );
```

可以在命令行中执行这个命令：`jjs func.js`，控制台输出结果是：

```
2
```

如果需要了解细节，可以参考[官方文档](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/jjs.html)。

## 5.2 类依赖分析器：jdeps

**jdeps**是一个相当棒的命令行工具，它可以展示包层级和类层级的Java类依赖关系，它以**.class**文件、目录或者Jar文件为输入，然后会把依赖关系输出到控制台。

我们可以利用jedps分析下[Spring Framework库](http://blog.csdn.net/yczz/article/details/50896975)，为了让结果少一点，仅仅分析一个JAR文件：**org.springframework.core-3.0.5.RELEASE.jar**。

```
jdeps org.springframework.core-3.0.5.RELEASE.jar
```

这个命令会输出很多结果，我们仅看下其中的一部分：依赖关系按照包分组，如果在classpath上找不到依赖，则显示"not found".

```
org.springframework.core-3.0.5.RELEASE.jar -> C:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   org.springframework.core (org.springframework.core-3.0.5.RELEASE.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.lang.ref                                      
      -> java.lang.reflect                                  
      -> java.util                                          
      -> java.util.concurrent                               
      -> org.apache.commons.logging                         not found
      -> org.springframework.asm                            not found
      -> org.springframework.asm.commons                    not found
   org.springframework.core.annotation (org.springframework.core-3.0.5.RELEASE.jar)
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.lang.reflect                                  
      -> java.util
```

更多的细节可以参考[官方文档](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/jdeps.html)。

# 6\. JVM的新特性

使用**[Metaspace](http://www.javacodegeeks.com/2013/02/java-8-from-permgen-to-metaspace.html)**（[JEP 122](http://openjdk.java.net/jeps/122)）代替持久代（**PermGen** space）。在JVM参数方面，使用**-XX:MetaSpaceSize**和**-XX:MaxMetaspaceSize**代替原来的**-XX:PermSize**和**-XX:MaxPermSize**。


# 元空间（MetaSpace）一种新的内存空间诞生

JDK8 HotSpot JVM 将移除永久区，使用本地内存来存储类元数据信息并称之为：元空间（Metaspace）；这与Oracle JRockit 和IBM JVM’s很相似，如下图所示

![Java 8新特性探究（九）跟OOM：Permgen说再见吧 ](http://static.open-open.com/lib/uploadImg/20140401/20140401170001_215.png)

这意味着不会再有java.lang.OutOfMemoryError: PermGen问题，也不再需要你进行调优及监控内存空间的使用……但请等等，这么说还为时过早。在默认情况下，这些改变是透明的，接下来我们的展示将使你知道仍然要关注类元数据内存的占用。请一定要牢记，这个新特性也不能神奇地消除类和类加载器导致的内存泄漏。

java8中metaspace总结如下：

## PermGen 空间的状况

这部分内存空间将全部移除。

JVM的参数：PermSize 和 MaxPermSize 会被忽略并给出警告（如果在启用时设置了这两个参数）。

## Metaspace 内存分配模型

大部分类元数据都在本地内存中分配。

用于描述类元数据的“klasses”已经被移除。

## Metaspace 容量

默认情况下，类元数据只受可用的本地内存限制（容量取决于是32位或是64位操作系统的可用虚拟内存大小）。

新参数（MaxMetaspaceSize）用于限制本地内存分配给类元数据的大小。如果没有指定这个参数，元空间会在运行时根据需要动态调整。

## Metaspace 垃圾回收

对于僵死的类及类加载器的垃圾回收将在元数据使用达到“MaxMetaspaceSize”参数的设定值时进行。

适时地监控和调整元空间对于减小垃圾回收频率和减少延时是很有必要的。持续的元空间垃圾回收说明，可能存在类、类加载器导致的内存泄漏或是大小设置不合适。

## Java 堆内存的影响

一些杂项数据已经移到Java堆空间中。升级到JDK8之后，会发现Java堆 空间有所增长。

## Metaspace 监控

元空间的使用情况可以从HotSpot1.8的详细GC日志输出中得到。

Jstat 和 JVisualVM两个工具，在使用b75版本进行测试时，已经更新了，但是还是能看到老的PermGen空间的出现。

前面已经从理论上充分说明，下面让我们通过“泄漏”程序进行新内存空间的观察……

# PermGen vs. Metaspace 运行时比较

    为了更好地理解Metaspace内存空间的运行时行为，

    将进行以下几种场景的测试：

1.  使用JDK1.7运行Java程序，监控并耗尽默认设定的85MB大小的PermGen内存空间。

2.  使用JDK1.8运行Java程序，监控新Metaspace内存空间的动态增长和垃圾回收过程。

3.  使用JDK1.8运行Java程序，模拟耗尽通过“MaxMetaspaceSize”参数设定的128MB大小的Metaspace内存空间。

首先建立了一个模拟PermGen OOM的代码


```
public class ClassA {
 public void method(String name) {
  // do nothing
 }
}
```


上面是一个简单的ClassA，把他编译成class字节码放到D：/classes下面，测试代码中用URLClassLoader来加载此类型上面类编译成class


```
/**
 * 模拟PermGen OOM
 * @author benhail
 */
public class OOMTest {
    public static void main(String[] args) {
        try {
            //准备url
            URL url = new File("D:/classes").toURI().toURL();
            URL[] urls = {url};
            //获取有关类型加载的JMX接口
            ClassLoadingMXBean loadingBean = ManagementFactory.getClassLoadingMXBean();
            //用于缓存类加载器
            List<ClassLoader> classLoaders = new ArrayList<ClassLoader>();
            while (true) {
                //加载类型并缓存类加载器实例
                ClassLoader classLoader = new URLClassLoader(urls);
                classLoaders.add(classLoader);
                classLoader.loadClass("ClassA");
                //显示数量信息（共加载过的类型数目，当前还有效的类型数目，已经被卸载的类型数目）
                System.out.println("total: " + loadingBean.getTotalLoadedClassCount());
                System.out.println("active: " + loadingBean.getLoadedClassCount());
                System.out.println("unloaded: " + loadingBean.getUnloadedClassCount());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

虚拟机器参数设置如下：-verbose -verbose:gc

设置-verbose参数是为了获取类型加载和卸载的信息

设置-verbose:gc是为了获取垃圾收集的相关信息

## JDK 1.7 @64-bit – PermGen 耗尽测试

Java1.7的PermGen默认空间为85 MB（或者可以通过-XX:MaxPermSize=XXXm指定）

![Java 8新特性探究（九）跟OOM：Permgen说再见吧 ](http://static.open-open.com/lib/uploadImg/20140401/20140401170001_746.jpg)

可以从上面的JVisualVM的截图看出：当加载超过6万个类之后，PermGen被耗尽。我们也能通过程序和GC的输出观察耗尽的过程。

程序输出(摘取了部分)

......
[Loaded ClassA from file:/D:/classes/]
total: 64887
active: 64887
unloaded: 0
[GC 245041K->213978K(536768K), 0.0597188 secs]
[Full GC 213978K->211425K(644992K), 0.6456638 secs]
[GC 211425K->211425K(656448K), 0.0086696 secs]
[Full GC 211425K->211411K(731008K), 0.6924754 secs]
[GC 211411K->211411K(726528K), 0.0088992 secs]
...............
java.lang.OutOfMemoryError: PermGen space

## JDK 1.8 @64-bit – Metaspace大小动态调整测试

Java的Metaspace空间：不受限制 （默认）

![Java 8新特性探究（九）跟OOM：Permgen说再见吧 ](http://static.open-open.com/lib/uploadImg/20140401/20140401170002_384.png)

从上面的截图可以看到，JVM Metaspace进行了动态扩展，本地内存的使用由20MB增长到646MB，以满足程序中不断增长的类数据内存占用需求。我们也能观察到JVM的垃圾回收事件—试图销毁僵死的类或类加载器对象。但是，由于我们程序的泄漏，JVM别无选择只能动态扩展Metaspace内存空间。程序加载超过10万个类，而没有出现OOM事件。

## JDK 1.8 @64-bit – Metaspace 受限测试

Java的Metaspace空间：128MB（-XX:MaxMetaspaceSize=128m）

![Java 8新特性探究（九）跟OOM：Permgen说再见吧 ](http://static.open-open.com/lib/uploadImg/20140401/20140401170002_915.png)

可以从上面的JVisualVM的截图看出：当加载超过2万个类之后，Metaspace被耗尽；与JDK1.7运行时非常相似。我们也能通过程序和GC的输出观察耗尽的过程。另一个有趣的现象是，保留的原生内存占用量是设定的最大大小两倍之多。这可能表明，如果可能的话，可微调元空间容量大小策略，来避免本地内存的浪费。

从Java程序的输出中看到如下异常。

[Loaded ClassA from file:/D:/classes/]
total: 21393
active: 21393
unloaded: 0
[GC (Metadata GC Threshold) 64306K->57010K(111616K), 0.0145502 secs]
[Full GC (Metadata GC Threshold) 57010K->56810K(122368K), 0.1068084 secs]
java.lang.OutOfMemoryError: Metaspace

在设置了MaxMetaspaceSize的情况下，该空间的内存仍然会耗尽，进而引发“java.lang.OutOfMemoryError: Metadata space”错误。因为类加载器的泄漏仍然存在，而通常Java又不希望无限制地消耗本机内存，因此设置一个类似于MaxPermSize的限制看起来也是合理的。

# 总结

1.  之前不管是不是需要，JVM都会吃掉那块空间……如果设置得太小，JVM会死掉；如果设置得太大，这块内存就被JVM浪费了。理论上说，现在你完全可以不关注这个，因为JVM会在运行时自动调校为“合适的大小”；

2.  提高Full GC的性能，在Full GC期间，Metadata到Metadata pointers之间不需要扫描了，别小看这几纳秒时间；

3.  隐患就是如果程序存在内存泄露，像OOMTest那样，不停的扩展metaspace的空间，会导致机器的内存不足，所以还是要有必要的调试和监控



