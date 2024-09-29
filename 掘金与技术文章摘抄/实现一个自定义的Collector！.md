## 背景

当前有多个用户，产品提出一个需求，根据userStatus分组，然后将每个分组中的用户按照age进行累加得到不同userStatus组下的age总和， 以`map<userStatus, age总和>`类型返回，而且要求使用stream.collect(Collector.groupingBy())方法一行写完；

## 解决思路

Collector.groupingBy()方法，默认可以轻松完成将用户按status分组，生成`<Key, List<Val>>`的map。但是，需求需要我们额外实现对于List中对象的age属性进行累加的操作，此时就需要我们自定义一个collector来完成该操作并传入Collector.groupingBy()方法中~

## 实操

先上代码：
```java
@Data  
@Builder  
@NoArgsConstructor  
@AllArgsConstructor  
public class User implements Serializable {  
    /**  
     *     */  
    @TableId(type = IdType.AUTO)  
    private Long id;  
  
    /**  
     * 年龄  
     */  
    private Integer age;  
  
  
    /**  
     * 用户状态 0-正常  1-异常
     */  
    private Integer userStatus;
```

```java
@Test  
void userCombine() {  
    User user1 = new User();  
    user1.setUserName("user1");  
    user1.setAge(10);  
    user1.setUserStatus(1);  
    User user2 = new User();  
    user2.setUserName("user2");  
    user2.setAge(3);  
    user2.setUserStatus(1);  
  
    User user3 = new User();  
    user3.setUserName("user3");  
    user3.setAge(13);  
    user3.setUserStatus(6);  
  
    List<User> users = Arrays.asList(user1, user2, user3);  
  
    // 根据userStatus分组，然后将每个分组中的用户按照age进行累加得到不同userStatus组下的age总和 以map类型返回，使用stream.collect(Collector.groupBy())方法  
    Map<Integer, Integer> collect = users.stream().collect(Collectors.groupingBy(User::getUserStatus,  
            Collector.of(() -> new int[1],  // 注意此处不能直接返回0，其并不是可变容器
                    (sum, user) -> sum[0] += user.getAge(),  // 自定义acculturator
                    (sum1, sum2) -> { sum1[0] += sum2[0];  // 自定义combiner
                        return sum1; }, 
                    sum -> sum[0])));  // 自定义finisher  
    System.out.println(collect);  
}
```

### collector是什么？

首先，collector是什么？

在 Java Stream API 中，Collector 接口用于收集流中的元素，并将其转换为另一种形式，比如转换成集合、计算汇总统计信息等。

我们来看看Collector的构造函数，`Collector.of()`：
```java
  
/**  
 * Returns a new {@code Collector} described by the given {@code supplier},  
 * {@code accumulator}, {@code combiner}, and {@code finisher} functions.  
 * * @param supplier The supplier function for the new collector  
 * @param accumulator The accumulator function for the new collector  
 * @param combiner The combiner function for the new collector  
 * @param finisher The finisher function for the new collector  
 * @param characteristics The collector characteristics for the new  
 *                        collector * @param <T> The type of input elements for the new collector  
 * @param <A> The intermediate accumulation type of the new collector  
 * @param <R> The final result type of the new collector  
 * @throws NullPointerException if any argument is null  
 * @return the new {@code Collector}  
 */public static<T, A, R> Collector<T, A, R> of(Supplier<A> supplier,  
                                             BiConsumer<A, T> accumulator,  
                                             BinaryOperator<A> combiner,  
                                             Function<A, R> finisher,  
                                             Characteristics... characteristics) {
                                            
......

}
```

可以看到Collector 接口有三个重要的方法：supplier、accumulator 和 combiner，它们分别对应着收集器的不同阶段。在实现collector的过程中，这三个函数都需要我们自己实现，下面详细分析各个函数~

#### supplier函数是什么？

Supplier 是 Java 8 中引入的一个函数式接口，它位于 java.util.function 包中。Supplier 接口只有一个抽象方法 get()，该方法不接受任何参数，并返回一个类型为 T 的结果。Supplier 主要用于生成或提供一个对象实例，通常用于延迟初始化或创建对象。

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

示例
假设你需要创建一个 User 对象，你可以使用 Supplier 如下所示：

```java
Supplier<User> userSupplier = () -> new User("John Doe", 30);
User user = userSupplier.get(); // 创建并返回一个User对象
```


**通俗的来说，他就是java定义的一个不需要输入，只管生成的方法~**

**在这次的需求中，就是生成一个只包含一个元素的可变容器 `new int[1]`，用来储存每次的累加和 


supplier 是一个 Supplier 函数，它负责提供一个初始的可变结果容器。这个容器用于存储流中的元素或计算的结果。

为什么需要：在收集过程开始之前，需要有一个初始的容器来存储结果。supplier 方法提供了这样一个容器的实例。例如，如果你想要收集元素到一个列表中，supplier 可能会返回一个空列表。

#### BiConsumer函数是什么？
BiConsumer 是 Java 8 中引入的一个函数式接口，它位于 java.util.function 包中。BiConsumer 接口代表了一个接受两个输入参数且不返回任何内容的操作。它主要用于消费两个输入参数而不产生任何结果。
```java
@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);
    
    default BiConsumer<T, U> andThen(BiConsumer<? super T, ? super U> after) {
        Objects.requireNonNull(after);
        return (T t, U u) -> { accept(t, u); after.accept(t, u); };
    }
}
```

BiConsumer 接口的主要方法
accept(T t, U u)：这是一个功能性方法，它接受两个参数 t 和 u，并执行某些操作。此方法不返回任何值。

**在collecor中该方法充当accumulator的角色**，它负责将流中的元素累积到一个可变的结果容器中（supplier函数生成的容器）。每当流中的一个元素被处理时，accumulator 就会被调用来更新结果容器。

在这次的需求中，定义的函数为`(sum, user) -> sum[0] += user.getAge()`，就是将一个个新进来的元素累加到sum中。

例如在将一个个user放到List中时，此时T就是list 和 U 就是user，而accumulator就需要定义user是如何放入list中，是否需要什么额外的操作，对应常见List的add操作~
#### BinaryOperator函数是什么？
BinaryOperator 是 Java 8 中引入的一个函数式接口，它位于 java.util.function 包中。BinaryOperator 接口代表了一个接受两个同类型的输入参数，并返回相同类型的单个结果的操作。它通常用于处理两个输入参数，并返回一个与输入参数类型相同的输出。

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
    T apply(T t, T u);
    
    default BinaryOperator<T> andThen(BinaryOperator<? super T> after) {
        Objects.requireNonNull(after);
        return (T t, T u) -> after.apply(apply(t, u), u);
    }
    
    static <T> BinaryOperator<T> minBy(Comparator<? super T> comparator) {
        Objects.requireNonNull(comparator);
        return (a, b) -> comparator.compare(a, b) <= 0 ? a : b;
    }
    
    static <T> BinaryOperator<T> maxBy(Comparator<? super T> comparator) {
        Objects.requireNonNull(comparator);
        return (a, b) -> comparator.compare(a, b) >= 0 ? a : b;
    }
}
```

BinaryOperator 接口的主要方法就是 apply(T t, T u)：这是一个功能性方法，它接受两个同类型的参数 t 和 u，并返回一个同类型的值。

**在collecor中该方法充当combiner的角色**，它负责将分开的两个流进行合并。例如当数据量很大时，流可能会被并行处理，这意味着流会被分成多个部分，每个部分都独立地进行处理。combiner 被用来合并这些部分的结果。

在这次的需求中，定义的函数为`(sum1, sum2) -> { sum1[0] += sum2[0];  return sum1; }`，就是将两个sum进行处理，并返回处理后的sum。

#### Function 函数是什么？
Function 同样是 Java 8 中引入的一个函数式接口，同样位于 java.util.function 包中。Function 接口有一个抽象方法 apply()，该方法接受一个类型为 T 的参数，并返回一个类型为 R 的结果。Function 主要用于将一个输入转换为另一个输出。

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

示例
假设你需要将一个字符串转换为大写，你可以使用 Function 如下所示：

```java
Function<String, String> toUpperCaseFunction = String::toUpperCase;
String upperCaseString = toUpperCaseFunction.apply("hello world"); // "HELLO WORLD"
```

**通俗的来说，他就是用于将一个输入转换为另一个输出，接受一个参数，并返回一个结果，类似于stream.map()~**

**在collecor中该方法充当finisher的角色**，它负责将中间结果转换成最终我们需要的对象~

在这次的需求中，定义的函数为`sum -> sum[0]`，我们预期是获取累加的值，是Integer类型，而不是可变容器int[1]， 所以返回数组的第一位即可。

