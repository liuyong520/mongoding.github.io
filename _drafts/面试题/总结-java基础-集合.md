面试-java基础-集合
===

### **Java常见集合**

*   List 和 Set 区别

*   Set和hashCode以及equals方法的联系

*   List 和 Map 区别

*   Arraylist 与 LinkedList 区别

*   ArrayList 与 Vector 区别

*   HashMap 和 Hashtable 的区别

*   HashSet 和 HashMap 区别
13、Hashmap 线程不安全的出现场景

*   HashMap 和 ConcurrentHashMap 的区别

*   HashMap 的工作原理及代码实现，什么时候用到红黑树
*   详细讲一下集合，HashSet源码，HashMap源码，如果要线程安全需要怎么做？

*   多线程情况下HashMap死循环的问题

*   HashMap出现Hash DOS攻击的问题

*   ConcurrentHashMap 的工作原理及代码实现，如何统计所有的元素个数

*   手写简单的HashMap

*   看过那些Java集合类的源码
*   Arraylist与LinkedList默认空间是多少； 

*   Arraylist与LinkedList区别与各自的优势List 和 Map 区别； 

*   谈谈HashMap，哈希表解决hash冲突的方法；
*   HashMap在什么时候时间复杂度是O（1），什么时候是O（n），什么时候又是O（logn）；
*   ArrayList和LinkList的删除一个元素的时间复杂度；（ArrayList是O(N)，LinkList是O(1)）；
*   说一下TreeMap的实现原理？红黑树的性质？红黑树遍历方式有哪些？如果key冲突如何解决？setColor()方法在什么时候用？什么时候会进行旋转和颜色转换？
   Set内存放的元素为什么不可以重复，内部是如何保证和实现的？

*   CopyOnWriteArrayList是什么；


2，你们业务中concurrenthashmap的运用场景，你们是怎么运用的
- 我们自己实现的配置中心，基于zk 的，有控制面板，支持多版本化，支持事实更新，其中获取到的配置信息都存储在了concurrenthashmap中
- 我们的定时任务获取数据之后，进行分片，把分片后的数据，放到了
concurrenthashmap中，各线程从concurrenthashmap 中获取数据集合去处理
- 本地缓存，我们用guava 实现本地缓存，guava 虽然没有直接用concurrenthashmap，但是guava 是实现了ConcurrentMap 接口的，我记得dubbo 中缓存的实现中，提供了一个lru 的缓存实现，就是继承了concurrenthashmap
- 限频数据放到 concurrenthashmap 中，前提是nginx 根据ip hash
3，简单说说你认识的java 集合体系
如果从操作的数据的特性及功能上分可以分：
- list，数据有序
- set，数据无序
- map，keyvalue 对
- stack，后进先出
- queue，先进先出
如果从线程安全分：
- 不加锁的非线程安全：
- 加synchronize的线程安全：
- 读写分类的线程安全：
- 锁分段的线程安全：
- 无锁化的线程安全：
如果从存储结构上分：
- 数组实现：
- 链表实现：
- 树：


4，说说hashmap 是怎么实现的
数组，链表，及红黑树的综合运用
5，说说 concurrenthashmap 是怎么实现的？
- 1.8以前，分段锁segment，数组中套hashmap，读无锁
- 1.8 基于cas和synchronize，锁数组中的链表的头结点
6，concurrenthashmap 一定是线程安全的吗？
不是，concurrenthashmap提供的方法是原子的，是线程安全的，但是对于使用者，多个方法组合使用可能就不是线程安全的

ConcurrentHashMap深入分析
HashMap和HashSet的使用

你对集合那么熟悉，看过哪些源码？HashMap，HashTable，ConcurrentHashMap等等


1、Java的数据结构相关的类实现原理，比如LinkedList，ArrayList，HashMap，TreeMap这一类的。以下简单模拟一个数据结构的连环炮

5.HashMap高并发情况下会出现什么问题，（扩容问题）

3.ConcurrentHashMap如何扩容？
8.那ConcurrentHashMap内部是如何实现的？每个segment是个什么数据结构？（HashTable）

12.对链表了解吗？（我说是List吗）是，（了解ArrayList和LinkedList），那你说说他们的区别？
concurrent包你们都用了啥？怎么用的？
阻塞队列你们都怎么用的？用非阻塞队列不可以吗？

concurrent包了解吗？原理是啥？AQS和CAS具体如何使用的？
# 如何给ArrayList<String>集合里添加一个Integer的对象（注意：不类型装换）？
# HashMap的原理了解吗？

# LinkedList添加和删除的复杂度都是多少？
# LinkedHashMap和TreeMap有什么区别？LRU缓存通过哪种数据结构实现最简单？


**3、集合框架hashtable、hashmap、list、list初始化大小、hashmap自动增长**

答：一开始是直接问了解常用的集合框架么，这个一听到肯定说了解咯，于是就让说说hashtable、hashmap、list的区别，所以就大致展开说了下：

**ArrayList和Vector的区别： **

**共同点：**这两个类都实现了List接口（List接口继承了Collection接口），他们都是**有序集合**，即存储在这两个集合中的元素的位置都是有顺序的，相当于一种**动态的数组**，我们以后可以**按位置索引号**取出某个元素，并且其中的数据是允许重复的，这是与HashSet之类的集合的最大不同处，HashSet之类的集合不可以按索引号去检索其中的元素，也不允许有重复的元素。 
接着说ArrayList与Vector的区别，这主要包括两个方面：. 
**同步性：**Vector是线程安全的，也就是说是它的方法之间是线程同步的，而ArrayList是线程序不安全的，它的方法之间是线程不同步的。如果只有一个线程会访问到集合，那最好是使用ArrayList，因为它不考虑线程安全，效率会高些；如果有多个线程会访问到集合，那最好是使用Vector，因为不需要我们自己再去考虑和编写线程安全的代码。 
**注意：**这里谈到线程安全，同步问题，面试官少不了会多嘴说一句，让你讲讲线程安全是咋回事，如果不考虑，你听到这个问题估计会是一脸懵逼，我当初就是这样子的！所以这里我补充下线程安全的问题： **java中的线程安全**就是线程同步的意思，就是当一个程序对一个线程安全的方法或者变量进行访问的时候，其他的程序不能再对他进行操作了，必须等到这次访问结束以后才能对这个线程安全的方法进行访问，否则将会造成错误发生；**线程安全**就是说，如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。 线程安全问题都是由**全局变量及静态变量**引起的，定义在方法内部的局部私有变量是没有线程安全与否一说的。 
**备注：**对于Vector&ArrayList、Hashtable&HashMap，要记住线程安全的问题，记住Vector与Hashtable是旧的，是java一诞生就提供了的，它们是线程安全的，ArrayList与HashMap是java2时才提供的，它们是线程不安全的。所以，我们讲课时先讲老的。 
**数据增长**：ArrayList与Vector都有一个初始的容量大小，当存储进它们里面的元素的个数超过了容量时，就需要增加ArrayList与Vector的存储空间，每次要增加存储空间时，不是只增加一个存储单元，而是增加多个存储单元，每次增加的存储单元的个数在内存空间利用与程序效率之间要取得一定的平衡。Vector默认增长为原来两倍，而ArrayList的增长策略在文档中没有明确规定（从源代码看到的是增长为原来的1.5倍）。ArrayList与Vector都可以设置初始的空间大小，Vector还可以设置增长的空间大小，而ArrayList没有提供设置增长空间的方法。 
**总结：即Vector增长原来的一倍，ArrayList增加原来的0.5倍。** PS：因为在上面讲区别的时候提到了自动增长的问题，所以面试官顺带问了句，hashmap自动增长为多少，这个问题说实话，自己没有关注过，平时也没注意过，所以只能老实的说不知道咯，面后网上查了下，不确定是否正确，**有说1.6版本的是增长2倍！**

**ArrayList,Vector, LinkedList的存储性能和特性： **
ArrayList和Vector都是使用**数组方式存储数据**，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接**按序号索引元素**，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快而插入数据慢，Vector由于使用了synchronized方法（线程安全），通常性能上较ArrayList差，而LinkedList使用双向链表实现存储，按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。LinkedList也是线程不安全的，LinkedList提供了一些方法，使得LinkedList可以被当作堆栈和队列来使用。 
**List和Map的区别： **
一个是存储单列数据的集合，另一个是存储**键和值**这样的双列数据的集合，List中存储的数据是有顺序，并且允许重复；Map中存储的数据是没有顺序的，其键（key）是不能重复的，它的值（value）是可以有重复的，存值采用 put（key，value）。Map中取值：**value=m.get(key)**（这个面试官常问，虽然不难，但也得注意） 
**HashMap和Hashtable的区别： **
HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。因为contains方法容易让人引起误解。 
一.历史原因:Hashtable是基于陈旧的Dictionary类的，HashMap是[Java ](http://lib.csdn.net/base/java "Java 知识库")1.2引进的Map接口的一个实现 
二.同步性:Hashtable是线程安全的，也就是说是同步的，而HashMap是线程序不安全的，不是同步的 
三.值：只有HashMap可以让你将空值作为一个表的条目的key或value，即HashMap允许将null作为一个entry的key或者value，而Hashtable不允许。

PS：最后面试官有问到list初始化大小为多少，也就是说默认是多少，这个其实问的是 List list = new ArrayList() 这里初始化 数组的大小是多少，根据源码我们知道，默认大小为10.

HashMap&ConcurrentHashMap原理、区别 
ConcurrentHashMap加锁原理

1、hashmap的实现原理

30.自己集合实现一个队列
5、HashMap和HashTable的区别，并说明其底层实现数据结构。
6、HashMap满了之后怎么扩容？


**3、集合： HashMap底层实现，怎么实现HashMap线程安全** 
  我讲了一下HashMap底层是数组加单链表实现，Node内部类，add的过程，Hash冲突解决办法，扩容，三种集合视图。HashMap线程安全的实现方式主要讲了HashTable、ConcurrentHashMap以及Collections中的静态方法SynchronizedMap可以对HashMap进行封装。以及这三种方式的区别，效率表现。 



14、往set里面put一个学生对象，然后将这个学生对象的学号改了，再put进去，可以放进set么？并讲出为什么
6、Hashmap的原理
7、hashmap容量为什么是2的幂次
8、hashset的源码
1.说一下 java中的队列 set map 区别，java里的数据结构。讲讲它们的实现。
5、ArrayListBlockingQueue 和LinkedListBlockingQueue 的区别


2、ArrayList 初始长度， HashMap初始长度。
3、HashMap 扰动函数
4、HashMap1.8 和之前有哪些不同。


9、arrayList 和linkedList的区别。

HashMap的并发问题
了解LinkedHashMap的应用吗


2谈谈你对HashMap的理解，底层原理的基本实现，HashMap怎么解决碰撞问题的？

这些数据结构中是线程安全的吗？假如你回答HashMap是线程安全的，接着问你有没有线程安全的map，接下来问了conurren包。

8、HashMap的key可以重复吗？
5、cincurrentMap的机制；TreeMap；Volatil关键字

9、List、Map、Set三个接口，存取元素时，各有什么特点？

List 以特定次序来持有元素，可有重复元素。Set 无法拥有重复元素,内部排序。Map 保存key-value值，value可多值 。
3.List、set、Map的底层实现原理

*   集合类以及集合框架；HashMap与HashTable实现原理，线程安全性，hash冲突及处理算法；ConcurrentHashMap；

HashMap扩容，ConcurrentHashMap扩容 
一些线程在写的过程中，如何扩容？（用synchronize实现，锁住每个链表头结点，一个线程在复制过程中，如果遇到锁住的节点，那么就会卡住，必须等释放锁，才能继续执行，这样可以复制完全部节点） 
Java并发编程 
1、set集合从原理上如何保证不重复

1）在往set中添加元素时，如果指定元素不存在，则添加成功。也就是说，如果set中不存在(e==null ? e1==null : e.queals(e1))的元素e1,则e1能添加到set中。

2）具体来讲：当向HashSet中添加元素的时候，首先计算元素的hashcode值，然后用这个（元素的hashcode）%（HashMap集合的大小）+1计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加。

2、HashMap和HashTable的主要区别是什么？，两者底层实现的数据结构是什么？

![](http://t11.baidu.com/it/u=2478456406,3828629005&fm=173&s=4800E41389D844C81C5DC1DA000080B1&w=637&h=340&img.JPEG)

3、HashMap何时扩容，扩容的算法是什么？

HashMap何时扩容：

当向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值---即当前数组的长度乘以加载因子的值的时候，就要自动扩容

扩容的算法是什么：

扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。当然Java里的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组

*   Collections 中 遗留类 (HashTable、Vector) 和 现有类的区别

LinkedHashMap

*   LinkedHashMap 和 PriorityQueue 的区别是什么                                                                                                                                                   答案：PriorityQueue 保证最高或者最低优先级的的元素总是在队列头部，但是 LinkedHashMap 维持的顺序是元素插入的顺序。当遍历一个 PriorityQueue 时，没有任何顺序保证，但是 LinkedHashMap 课保证遍历顺序是元素插入的顺序。

List

*   List, Set, Map三个接口，存取元素时各有什么特点

*   List, Set, Map 是否继承自 Collection 接口

*   遍历一个 List 有哪些不同的方式

*   LinkedList

1.  LinkedList 是单向链表还是双向链表

2.  LinkedList 与 ArrayList 有什么区别

3.  描述下 Java 中集合（Collections），接口（Interfaces），实现（Implementations）的概念。LinkedList 与 ArrayList 的区别是什么？

4.  插入数据时，ArrayList, LinkedList, Vector谁速度较快？

*   ArrayList

1.  ArrayList 和 HashMap 的默认大小是多数                                                                                                                                                                      答案：hashMap为16，ArrayList为10.
    但是ArrayList比较特殊，只是初始化了10个空的数组。详情参考[点击打开链接](http://blog.csdn.net/jdsjlzx/article/details/52675726)                                                                                                                                                                                   

2.  ArrayList 和 LinkedList 的区别，什么时候用 ArrayList？

3.  ArrayList 和 Set 的区别？

4.  ArrayList, LinkedList, Vector的区别

5.  ArrayList是如何实现的，ArrayList 和 LinkedList 的区别

6.  ArrayList如何实现扩容                                                                                                                                                                                                  答案：
    在JDK1.7中，如果通过无参构造的话，初始数组容量为0，当真正对数组进行添加时，才真正分配容量。
    每次按照1.5倍（位运算）的比率通过copeOf的方式扩容。 
    在JKD1.6中，如果通过无参构造的话，初始数组容量为10.每次通过copeOf的方式扩容后容量为原来的1.5倍加1.以上就是动态扩容的原理。               

7.  Array 和 ArrayList 有何区别？什么时候更适合用Array

8.  说出ArraList,Vector, LinkedList的存储性能和特性

Map

*   Map, Set, List, Queue, Stack

*   Map 接口提供了哪些不同的集合视图                                                                                                                                                                            答案：
    Map接口在Java集合中提供三个集合视图：
    （1）Set keyset()：返回map中包含的所有key的一个Set视图。集合是受map支持的，map的变化会在集合中反映出来，反之亦然。当一个迭代器正在 遍历一个集合时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为未定义。 
    （2）Collection values()：返回一个map中包含的所有value的一个Collection视图。这个collection受map支持的，map的变化会在 collection中反映出来，反之亦然。当一个迭代器正在遍历一个collection时，若map被修改了（除迭代器自身的移除操作以外），迭代器 的结果会变为未定义。 
    （3）Set<Map.Entry<K,V>> entrySet()：返回一个map钟包含的所有映射的一个集合视图。这个集合受map支持的，map的变化会在collection中反映出来，反之 亦然。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作，以及对迭代器返回的entry进行setValue外），迭代器的结 果会变为未定义。
    以上集合都支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素 移除，从map中移除对应的映射。它不支持add和addAll操作。                                                                                                                                                                                                             

*   为什么 Map 接口不继承 Collection 接口                                                                                                                                                                        答案：
    尽管Map接口和它的实现也是集合框架的一部分，但Map不是集合，集合也不是Map。因此，Map继承Collection毫无意义，反之亦然。
    如果Map继承Collection接口，那么元素去哪儿？Map包含key-value对，它提供抽取key或value列表集合的方法，但是它不适合“一组对象”规范。

Collections

*   介绍Java中的Collection FrameWork。集合类框架的基本接口有哪些

*   Collections类是什么？Collection 和 Collections的区别？Collection、Map的实现

*   集合类框架的最佳实践有哪些

*   为什么 Collection 不从 Cloneable 和 Serializable 接口继承

*   说出几点 Java 中使用 Collections 的最佳实践？                                                                                                                                                           答案：
    a）使用正确的集合类，例如，如果不需要同步列表，使用 ArrayList 而不是 Vector。
    b）优先使用并发集合，而不是对集合进行同步。并发集合提供更好的可扩展性。
    c）使用接口代表和访问集合，如使用List存储 ArrayList，使用 Map 存储 HashMap 等等。
    d）使用迭代器来循环集合。
    e）使用集合的时候使用泛型。                                                                                                                                                                                      

队列

*   队列和栈是什么，列出它们的区别                                                                                                                                                                                 答案：
    队列（Queue）：是限定只能在表的一端进行插入和在另一端进行删除操作的线性表；
    栈（Stack）：是限定只能在表的一端进行插入和删除操作的线性表。
    一、规则不同
           1\. 队列：先进先出（First In First Out）FIFO
           2\. 栈：先进后出（First In Last Out ）FILO
    二、对插入和删除操作的限定不同
           1\. 队列：只能在表的一端进行插入，并在表的另一端进行删除；
           2\. 栈：只能在表的一端插入和删除。
    三、遍历数据速度不同
           1\. 队列：基于地址指针进行遍历，而且可以从头部或者尾部进行遍历，但不能同时遍历，无需开辟空间，因为在遍历的过程中不影响数据结构，所以遍历速度要快；
           2\. 栈：只能从顶部取数据，也就是说最先进入栈底的，需要遍历整个栈才能取出来，而且在遍历数据的同时需要为数据开辟临时空间，保持数据在遍历前的一致性。                                                                                                                                                                                              

*   BlockingQueue是什么                                                                                                                                                                                                   答案：
    1.BlockingQueue队列和平常队列一样都可以用来作为存储数据的容器，但有时候在线程当中涉及到数据存储的时候就会出现问题，而BlockingQueue是空的话，如果一个线程要从BlockingQueue里取数据的时候，该线程将会被阻断，并进入等待状态，直到BlockingQueue里面有数据存入了后，就会唤醒线程进行数据的去除。若BlockingQueue是满的，如果一个线程要将数据存入BlockQueue，该线程将会被阻断，并进入等待状态，直到BlcokQueue里面的数据被取出有空间后，线程被唤醒后在将数据存入
    2.BlockingQueue定义的常用方法详解: 
            1)add(anObject):把anObject加到BlockingQueue里,即如果BlockingQueue可以容纳,则返回true,否则报异常 
            2)offer(anObject):表示如果可能的话,将anObject加到BlockingQueue里,即如果BlockingQueue可以容纳,则返回true,否则返回false. 
            3)put(anObject):把anObject加到BlockingQueue里,如果BlockQueue没有空间,则调用此方法的线程被阻断直到BlockingQueue里面有空间再继续. 
            4)poll(time):取走BlockingQueue里排在首位的对象,若不能立即取出,则可以等time参数规定的时间,取不到时返回null 
            5)take():取走BlockingQueue里排在首位的对象,若BlockingQueue为空,阻断进入等待状态直到Blocking有新的对象被加入为止 
    3.BlockingQueue有四个具体的实现类,根据不同需求,选择不同的实现类 
            1)ArrayBlockingQueue:规定大小的BlockingQueue,其构造函数必须带一个int参数来指明其大小.其所含的对象是以FIFO(先入先出)顺序排序的. 
            2)LinkedBlockingQueue:大小不定的BlockingQueue,若其构造函数带一个规定大小的参数,生成的 BlockingQueue有大小限制,若不带大小参数,所生成的BlockingQueue的大小由Integer.MAX_VALUE来决定.其所含 的对象是以FIFO(先入先出)顺序排序的 
            3)PriorityBlockingQueue:类似于LinkedBlockQueue,但其所含对象的排序不是FIFO,而是依据对象的自然排序顺序或者是构造函数的Comparator决定的顺序. 
            4)SynchronousQueue:特殊的BlockingQueue,对其的操作必须是放和取交替完成的. 
    4.LinkedBlockingQueue和ArrayBlockingQueue比较起来,它们背后所用的数据结构不一样,导致 LinkedBlockingQueue的数据吞吐量要大于ArrayBlockingQueue,但在线程数量很大时其性能的可预见性低于 ArrayBlockingQueue.                                                                                            

*   简述 ConcurrentLinkedQueue LinkedBlockingQueue 的用处和不同之处。                                                                                                                  答案：
    1.由于LinkedBlockingQueue实现是线程安全的，实现了先进先出等特性，是作为生产者消费者的首选，LinkedBlockingQueue 可以指定容量，也可以不指定，不指定的话，默认最大是Integer.MAX_VALUE，其中主要用到put和take方法，put方法在队列满的时候会阻塞直到有队列成员被消费，take方法在队列空的时候会阻塞，直到有队列成员被放进来。
    2.LinkedBlockingQueue是一个线程安全的阻塞队列，它实现了BlockingQueue接口，BlockingQueue接口继承自java.util.Queue接口，并在这个接口的基础上增加了take和put方法，这两个方法正是队列操作的阻塞版本。
    3.ConcurrentLinkedQueue是无阻塞的队列,可伸缩性比BlockingQueue高。ConcurrentLinkedQueue是Queue的一个安全实现．Queue中元素按FIFO原则进行排序．采用CAS操作，来保证元素的一致性。
    4.两个都是线程安全的                

ArrayList、Vector、LinkedList的存储性能和特性

String

StringBuffer

*   ByteBuffer 与 StringBuffer有什么区别

HashMap

*   HashMap的工作原理是什么                                                                                                                                                                                          答案：
    HashMap的底层是用hash数组和单向链表实现的 ，当调用put方法是，首先计算key的hashcode，定位到合适的数组索引，然后再在该索引上的单向链表进行循环遍历用equals比较key是否存在，如果存在则用新的value覆盖原值，如果没有则插入到链表linkedlist的头部。HashMap的两个重要属性是容量capacity和加载因子loadfactor，默认值分布为16和0.75，当容器中的元素个数大于 capacity*loadfactor时，容器会进行扩容resize 为2n，在初始化Hashmap时可以对着两个值进行修改，负载因子0.75被证明为是性能比较好的取值，通常不会修改，那么只有初始容量capacity会导致频繁的扩容行为，这是非常耗费资源的操作，所以，如果事先能估算出容器所要存储的元素数量，最好在初始化时修改默认容量capacity，以防止频繁的resize操作影响性能。
    HashMap基于hashing原理，我们通过put()和get()方法储存和获取对象。当我们将键值对传递给put()方法时，它调用键对象的hashCode()方法来计算hashcode，让后找到bucket位置来储存值对象。当获取对象时，通过键对象的equals()方法找到正确的键值对，然后返回值对象。HashMap使用LinkedList来解决碰撞问题，当发生碰撞了，对象将会储存在LinkedList的下一个节点中。 HashMap在每个LinkedList节点中储存键值对对象。                                                                                                                                                                                                                              

*   内部的数据结构是什么

*   HashMap 的 table的容量如何确定？loadFactor 是什么？ 该容量如何变化？这种变化会带来什么问题？                                                                      答案：
    HashMap使用的是懒加载，构造完HashMap对象后，只要不进行put 方法插入元素之前，HashMap并不会去初始化或者扩容table。这个问题可以跟踪一下HashMap的源码就知道了，根据输入的初始化容量（门槛？）的值（先了解HashMap中容量和负载因子的概念，其实这个和HashMap确定存储地址的算法有关），先判断是否大于最大容量，最大容量2的30次方，1<<30 =（1073741824），如果大于此数，初始化容量赋值为1<<30，如果小于此数，调用tableSizeFor方法 使用位运算将初始化容量修改为2的次方数，都是向大的方向运算，比如输入13，小于2的4次方，那面计算出来桶的初始容量就是16.                                                                                                                                                                                             

*   HashMap 实现的数据结构是什么？如何实现

*   HashMap 和 HashTable、ConcurrentHashMap 的区别

*   HashMap的遍历方式及效率

*   HashMap、LinkedMap、TreeMap的区别                                                                                                                                                                     Hashmap 是一个最常用的Map,它根据键的HashCode 值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null;HashMap不支持线程的同步，即任一时刻可以有多个线程同时写HashMap;可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。
    Hashtable与 HashMap类似,它继承自Dictionary类，不同的是:它不允许记录的键或者值为空;它支持线程的同步，即任一时刻只有一个线程能写Hashtable,因此也导致了 Hashtable在写入时会比较慢。
    LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。
    TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。
    一般情况下，我们用的最多的是HashMap,HashMap里面存入的键值对在取出的时候是随机的,它根据键的HashCode值存储数据,根据键可以直接获取它的值，具有很快的访问速度。在Map 中插入、删除和定位元素，HashMap 是最好的选择。
    LinkedHashMap 是HashMap的一个子类，如果需要输出的顺序和输入的相同,那么用LinkedHashMap可以实现,它还可以按读取顺序来排列，像连接池中可以应用。
    TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。                                                              

*   如何决定选用HashMap还是TreeMap

*   如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办

*   HashMap 是线程安全的吗？并发下使用的 Map 是什么，它们内部原理分别是什么，比如存储方式、 hashcode、扩容、 默认容量等

HashSet

*   HashSet和TreeSet有什么区别                                                                                                                                                                                      答案：1、TreeSet 是二叉树实现的,Treeset中的数据是自动排好序的，不允许放入null值。 TreeSet是SortedSet接口的唯一实现类，向TreeSet中加入的应该是同一个类的对象。
    2、HashSet 是哈希表实现的,HashSet中的数据是无序的，可以放入null，但只能放入一个null，两者中的值都不能重复，就如数据库中唯一约束。 
    3、HashSet要求放入的对象必须实现HashCode()方法，放入的对象，是以hashcode码作为标识的，而具有相同内容的 String对象，hashcode是一样，所以放入的内容不能重复。但是同一个类的对象可以放入不同的实例 。                                                                                                       

*   HashSet 内部是如何工作的                                                                                                                                                                                          答案：HashSet 的实现其实非常简单，它只是封装了一个 HashMap 对象来存储所有的集合元素，所有放入 HashSet 中的集合元素实际上由 HashMap 的 key 来保存，而 HashMap 的 value 则存储了一个 PRESENT，它是一个静态的 Object 对象。                                                          

*   WeakHashMap 是怎么工作的？                                                                                                                                                                                   答案：WeakHashMap当系统内存不足时，垃圾收集器会自动的清除没有在任何其他地方被引用的键值对，因此可以作为简单缓存表的解决方案。而HashMap就没有上述功能。但是，如果WeakHashMap的key在系统内持有强引用，那么WeakHashMap就退化为了HashMap，所有的表项都不会被垃圾回收器回收。但是在WeakHashMap中会删除那些已经被GC的键值对在源码中是通过调用expungeStaleEntries函数来完成的，而这个函数只在WeakHashMap的put、get、size()等方法中才进行了调用。

Set

*   Set 里的元素是不能重复的，那么用什么方法来区分重复与否呢？是用 == 还是 equals()？ 它们有何区别?                                                               答案：当使用HashSet时，hashCode方法就会得到调用，判断已经存储在集合中的对象的hash code值是否与增加的对象的hash code值一致：
    1\. 如果不一致，直接加进去；
    2\. 如果一致，再进行equals方法的比较，equals如果返回true，表示对象已经加进去了，就不会再增加新的对象；否则加进去。                           

*   TreeMap：TreeMap 是采用什么树实现的？TreeMap、HashMap、LindedHashMap的区别。TreeMap和TreeSet在排序时如何比较元素？Collections工具类中的sort()方法如何比较元素？                                                                                                                                                      答案：TreeSet要求存放的对象所属的类必须实现Comparable接口，该接口提供了比较元素的compareTo()方法，当插入元素时会回调该方法比较元素的大小。TreeMap要求存放的键值对映射的键必须实现Comparable接口从而根据键对元素进行排序。                                                         三者区别参考文章：[三者区别](https://www.cnblogs.com/acm-bingzi/p/javaMap.html)                                                                                                                                                                   

*   TreeSet：一个已经构建好的 TreeSet，怎么完成倒排序。                                                                                                                                           答案：A:自然排序：要在自定义类中实现Comparerable<T>接口  ，并且重写compareTo方法
    B:比较器排序：在自定义类中实现Comparetor<t>接口，重写compare方法                                                                                                              

*   EnumSet 是什么                                                                                                                                                                                                        答案：Enumset是个虚类，我们只能通过它提供的静态方法来返回Enumset的实现类的实例。使用noneOf方法创建空的EnumSet，使用EnumSet.allOf方法创建一个拥有所有枚举类元素的EnumSet，使用EnumSet.of方法返回拥有部分元素的EnumSet，使用addAll方法，添加一个EnumSet中的所有元素到另外一个EnumSet，使用toArray方法，将EnumSet中的元素存放到数组中去
**1）Java的数据结构相关的类实现原理，比如LinkedList，ArrayList，HashMap，TreeMap这一类的。以下简单模拟一个数据结构的连环炮。**

比如，面试官先问你HashMap是不是有序的？

你肯定回答说，不是有序的。那面试官就会继续问你，有没有有顺序的Map实现类？

你如果这个时候说不知道的话，那这个问题就到此结束了。如果你说有TreeMap和LinkedHashMap。

那么面试官接下来就可能会问你，TreeMap和LinkedHashMap是如何保证它的顺序的？

如果你回答不上来，那么到此为止。如果你依然回答上来了，那么面试官还会继续问你，你觉得它们两个哪个的有序实现比较好？

如果你依然可以回答的话，那么面试官会继续问你，你觉得还有没有比它更好或者更高效的实现方式？

如果你还能说出来的话，那么就你所说的实现方式肯定依然可以问你很多问题。

**以上就是一个面试官一步一步提问的例子。所以，如果你了解的不多，千万不要敷衍，因为可能下一个问题你就暴露了，还不如直接说不会，把这个问题结束掉，赶紧切换到你熟悉的领域。**

HashMap1.8优化在哪些方面？


讲到ArrayList，讲一下初始长度，扩容机制。

说一下ArrayList和LinkedList区别，然后讲了大量数据下在LinkedList前1/10处插入效率高，在ArrayList中部以及后部插入效率高，解释原因。

楼主故意引线程安全的CopyOnWriteArrayList，然后把源码吹了一下。
多个线程同时去访问同一个代码块，我想知道最后一个线程什么时候执行完，怎么做。答CountDownLatch。
1、HashMap和Hashtable的区别

2、HashMap的数据结构，为什么新添加的节点要添加到链表头部？

3、ConcurrentHashMap支持高并发的原理，段锁为什么要采用重入锁而不是synchronized？
Current

*   ConcurrentHashMap 和 Hashtable的区别

*   ArrayBlockingQueue, CountDownLatch的用法

*   ConcurrentHashMap的并发度是什么

*   如果让你实现一个并发安全的链表，你会怎么做
1.怎样设计实现一个高效的线程安全的hashmap 。

方法一：通过Collections.synchronizedMap()返回一个新的Map，这个新的map就是线程安全的。 这个要求大家习惯基于接口编程，因为返回的并不是HashMap，而是一个Map的实现。

方法二：重新改写了HashMap，具体的可以查看java.util.concurrent.ConcurrentHashMap. 这个方法比方法一有了很大的改进。（锁分离）

方法一使用的是的synchronized方法,是一种悲观锁.在进入之前需要获得锁,确保独享当前对象,然后做相应的修改/读取.

方法二使用的是乐观锁,只有在需要修改对象时,比较和之前的值是否被人修改了,如果被其他线程修改了,那么就会返回失败.锁的实现,使用的是 NonfairSync. 这个特性要确保修改的原子性,互斥性,无法在JDK这个级别得到解决,JDK在此次需要调用JNI方法,而JNI则调用CAS指令来确保原子性与互斥性.
2.jdk源码看过吗？把arraylist的实现写一下


## 2\. 了解HashMap吗

a. resize为啥是2倍，什么时候会扩容，参数是多少 
b. 底层数据结构 
c. get和put过程 volatile是什么 
d. null key处理， hashtable又怎么处理

## 7\. 手写Java链表插入删除修改

22.  `HashMap`的操作中，直接使用`keySet()`遍历有什么问题？

1.  HashMap的put怎么实现，如何解决hash冲突。
    调用putval，计算相应hash码，然后初始化（默认64的capacity）或调用resize函数调整大小，判断bucket是否有值，若没有在数组初始化改值。若有则以拉链法（链表的形式）解决hash冲突，这里和ThreadLocalMap不一样，ThreadLocalMap使用的是线性探测法，接着将相应节点加入链表头部。如果超过8个元素会进化为RBtree，防止hash攻击。
2.  RBtree是怎样的数据结构，有什么性质？
    二叉树，有序的，四种性质。从而推得路径最长2n，最短n。复杂度为log2N.（此处省略n多话，感兴趣的同学请自行Google）
3.  RBtree什么时候会变色？
    旋转时，共有四种旋转方式。一般是为了保持平衡，如左边太长，右边太短这样。（打哈哈过去，具体记不清了）
4.  hashmap什么时候会调整大小？
    根据负载因子来搞事，默认为0.75。
5.  什么是负载因子？
    根据capacity来，举个例子，当capacity为100时，如果HashMap的ele的数量到了75就会resize，resize后的大小为原来的2倍，这样可以直接使用位运算得到原来的元素新的hash值。
6.  扩容存在什么问题？
    （楞了一会，发现应该是说多线程的情况）然后说了多线程会有死循环问题。如果要解决可以使用concurrentHashMap。
7.  为什么有死循环？
    扯了半天，发现不画图，只通过电话根本扯不清。然后说就是因为1.7扩容后链表会逆序，1.8不会，所以1.8没这个问题，1.7就是两个线程同时扩容，一个扩到一半，到另一个了开始并完成扩容，之前那个再继续，就会出现。（然后说小姐姐，有机会我当面画给你看，开个玩笑）

  8.你刚才提到concurrentHashMap，你知道怎么实现吗？
     1.7使用分段锁，分为16个，每个segment可以视为一个hashtable，然后一次一个线程只锁一个segment，减小了锁的粒度，提高了并发。1.7使用的是Lock的实现类，可重入锁来同步的。1.8使用的是CAS和synchronized。如果已有元素，需要解决hash冲突，会使用synchronized锁住相应的bucket，然后再添加，同样元素在八个以上会转化为RBtree。
  15.  `ArrayList`的实现原理，如何测试`ArrayList`动态分配内存中带来的内存、cpu变化

**2、谈谈你对HashMap的理解，底层原理的基本实现，HashMap怎么解决碰撞问题的？
这些数据结构中是线程安全的吗？假如你回答HashMap是线程安全的，接着问你有没有线程安全的map，接下来问了conurren包。**
**7、****使用无界阻塞队列会出现什么问题？**

3\. concurrenhashmap求size是如何加锁的，如果刚求完一段后这段发生了变化该如何处理
113\. LinkedHashmap的底层实现
4.针对HashMap中某个Entry链太长，查找的时间复杂度可能达到O(n)，怎么优化？

目前在jdk1.8中，采用了新的红黑树的结构来实现，当链表的数量大于8的时，就会将冲突的节点保存在红黑树里。
40.如果想实现一个线程安全的队列，可以怎么实现？

        JUC包里的ArrayBlockingQueue 还有LinkedBlockingQueue啥的又结合源码说了一通
10.HashMap如果有很多相同key，后面的链很长的话，你会怎么优化？或者你会用什么数据结构来存储？我说了一个SkipList

如果是hash冲突，可以通过（1）设置大一些的table值；（2）改进hashCode算法；

Skip List是一种随机化的数据结构（必须有序），基于并联的链表，其效率可比拟于二叉查找树（对于大多数操作需要O(log n)平均时间）。基本上，跳跃列表是对有序的链表增加上附加的前进链接，增加是以随机化的方式进行的，所以在列表中的查找可以快速的跳过部分列表(因此得 名)。所有操作都以对数随机化的时间进行。Skip List可以很好解决有序链表查找特定值的困难。

再向跳跃表中插入新的结点时候，我们需要生成该结点的层数，使用的就是随机数生成器，随机的生成一个层数。

5.HashMap高并发情况下会出现什么问题，（扩容问题）？
1.怎样设计实现一个高效的线程安全的hashmap 。

方法一：通过Collections.synchronizedMap()返回一个新的Map，这个新的map就是线程安全的。 这个要求大家习惯基于接口编程，因为返回的并不是HashMap，而是一个Map的实现。

方法二：重新改写了HashMap，具体的可以查看java.util.concurrent.ConcurrentHashMap. 这个方法比方法一有了很大的改进。（锁分离）

方法一使用的是的synchronized方法,是一种悲观锁.在进入之前需要获得锁,确保独享当前对象,然后做相应的修改/读取.

方法二使用的是乐观锁,只有在需要修改对象时,比较和之前的值是否被人修改了,如果被其他线程修改了,那么就会返回失败.锁的实现,使用的是 NonfairSync. 这个特性要确保修改的原子性,互斥性,无法在JDK这个级别得到解决,JDK在此次需要调用JNI方法,而JNI则调用CAS指令来确保原子性与互斥性.

1.  如何用一个数组实现一个队列？如果满了怎么办（扩容），扩容怎么实现？
2.  如果实现循环队列，怎么操作？怎么样实现扩容？


