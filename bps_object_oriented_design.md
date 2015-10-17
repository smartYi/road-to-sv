# Object Oriented Design

对于初级程序员的面试，最难的部分可能就是所谓的设计题。设计题可以分成两个类别：系统架构设计和利用面向对象编程原理进行程序设计。前者所涉及的技术往往包括数据库，并发处理和分布式系统等等，对于经验要求和知识要求比较高。我们先来看看系统设计的面试流程:

1. 题目描述
    + 往往非常简单，如：设计一个XX系统。或者：你有没有用过XXX，给你看一下它的界面和功能，你来设计一个。
2. 阐述题意
    + 面试者需向面试官询问系统的具体要求。如，需要什么功能，需要承受的流量大小，是否需要考虑可靠性，容错性等等。
3. 面试者提供一个初步的系统设计
4. 面试官这对初步的系统中提出一些后续的问题：如果要加某个功能怎么办，如果流量大了怎么办，如何考虑一致性，如果机器挂了怎么办。
5. 面试者根据面试官的后续问题逐步完善系统设计
6. 完成面试

总体特点是以交流为主，画图和代码为辅。

根据我们面试别人和参与面试的经验，先从面试官的角度给出一些考量标准：

+ 适应变化的需求(Adapt to the changing requirements )
+ 设计干净，优美，考虑周到的系统(Produce a system that is clean, elegant, well thought )
+ 解释为何这么实现(Explain why you choose this implementation )
+ 对自己的能力水平很熟练(Be familiar with your experience level to make decisions )
+ 在一些高层结构和复杂性方面有设计(Answer in high level of scale and complexity )

按照评分体系的化，分成下面4个等级

| Scoring | Candidate | Criteria |
| :--: | :--: | :--: |
| 1.0 | Bad | No sense of requirement, no scoping |
| 2.0 | Poor | Limited knowledge, common sense |
| 3.0 | Good | Reasonable Solution, explain clearly |
| 4.0 | Great | Out of expectation, well thoughtful, trade-off |

其实大家大可不必追求完美，在真正的面试中，没有人能对答如流，往往面试官也会给出善意的提示，就算你没回答某个子问题，在面试后的评价中也会综合衡量，跟其他的面试者比较，最终打出一个分数。我认为很多人在2到3分左右，当然我们目标是尽量在3分以上。

### 模拟面试

下面我就来做个模拟面试(Mock Interview)，那一到很经典的TinyURL举例：
TinyURL是说给你一个长URL，通过某一种编码，你把它压缩成5个字母(数字)长度的code，然后当给出短链接，要能还原出来原来的URL。举个例子：

+ 原始URL：http://www.zhihu.com/abc?user=12345
+ TinyURL: http://t.cn/xcef0
+ 当访问 TinyURL，会自动跳转到原始URL。

**等级1**

最直接想到的是用一个数据库，把原始URL存入数据库中，ID就是自增主键，避免重复。当访问TinyURL时候就去数据库查询，当查询成功就返回原始URL，没有就返回空。这是不是很简单？

想到这还是太容易了，虽然这是个正确的方法，但没考量全面。比如这里面编码是用数字，没有很好利用5个字节的表示空间，然后在性能方面，每次都是查询数据库这样效率不高。那么我们看下面的方法

**等级1.5**

上面使用数字能表达的URL最多就是0－99999, 10万条，那么使用Base 64编码，所谓base 64是有64个不同字符构成，比如a到z，A到Z，0到9，再加上2个特殊符号，比如_, -，这样能表示的就是64^5 = 2^30 是大约10亿的大小。这样就极大提高了空间容量。

然后为了提高性能，可以加上缓存，缓存大小是有限的，我们如何让一些不常用的查询从缓存中替换出去，这里面就涉及到缓存淘汰算法，常用的LRU(Least Recent Use)和LFU(Least Frequent Use)，一个是最近最少访问，一个是最不频繁访问，我们在这里使用LFU就比较合适。

到了这里，设计还是不够好，一个是性能还是可能出现瓶颈，当我们在使用单个数据库，如果查询依然很多，到每秒1万次以上怎么办？使用的是那种数据库，如果数据库当机怎么办？

**等级2.0**

使用key-value存储的数据库，所谓的Key-value pair就是NoSQL的一种经典应用，传统数据库为了支持事务，外键，有很多需要考虑。但key-value型数据库，就减少了这么多限制，对这种典型通过某个key查询value的，就直接用这个合适，它的吞吐量可以比传统MySQL 有10倍以上的提升。另外一个是关于如何快速生成短code，有一个md5的算法，它是用在单项加密上，最直接的把一段信息加密成128bit大小，如果我们算一下，128bits 是16个bytes，这样就超过了题目要求的5个，那么我们可以做一个尾部截取，但这样又会带来hash重合的问题，大家可以想想有没有更好的解决方法。

**等级3.0**

下面可以再考虑分布式，可靠性的问题。刚才说了一台机器可能导致挂了就失去服务和数据。那我们可以用多台机器，通过sharding的方式，就是把某个URL取一个hash值再％N，这样就把这个请求分配到某台具体机器上，然后再这台机器上进行读取或者存储。另外我们要注意一种分配不均的情况，你可以想象也许机器1就是非常热门，它的负载比其他的机器高很多，但总体负载又不高，如果我们简单的扩容，一方面浪费了机器，涉及到迁移成本，并且也不见得解决真正1号机负载高的问题。最好的做法是把1号机的旁边再放置一台同样规模和数据的机器，这个叫热等待(Hot standby), 通过流量的导流，实现负载均衡。对于可靠性，你想象某台机器当机，它如何做恢复呢？首先你要保证数据是有备份的，也许再另一个数据库中，然后当那台机器重启后，可以先载入上次备份的地方，也叫快照(snapshot)，然后再把日志中应该重新做的操作继续做一遍，这样就保证了数据的一致性。

**等级4.0**

其实到这一步，更多的是一些后续问题，比如你怎么解决生成全局唯一的token号，这里面可以用zookeeper作为工具，在集群中协作保证生成唯一ID，具体算法可以参考Paxos。此外，还可能会问如何防止TinyURL被爬取，刚才我们看到的例子如果是按正常顺序，就是1，2，3……9，a，b，这样很容易被机器抓取，有个简单办法，内部你做一个乱序对应表，比如z对应1, w对应2，b对应34，这样就比较难猜到你的生成规则。还可能接着问如何限制用户访问，当某个用户每分钟到达一定次数就禁止访问，可以使用一些计数器针对用户cookie做统计，当一分钟内到达阈值就触发屏蔽。还可能问如何实现URL的跳转服务，当你没有经验，可能就说自己根据HTTP协议做一个Web Server，但其实不必自己造轮子，现在apache\nginx等主流服务器早就自带重定向模块，你只需要打开这个模块配置一下就可。

---

上面说的详细解答就是想给大家一个直观认识，遇到这种面试题在不同的层次是答道什么深度，其实这些都是可以训练出来，大家不必太担心灵活性。在面试过程中，面试官一般不会对初级程序员提出这方面的问题。因此，面向对象编程原理(OOP)是设计题中需要重点准备的。下面就对一些OOP方面的设计题，多一些更为基本的补充解释。

### Abstractions, Object and Decoupling

通常，关于OOP，面试官会让面试者设计一个程序框架，该程序能够实现一些特定的功能。比如，如何实现一个音乐播放器，如何设计一个车库管理程序等等。对于此类问题，设计的关键过程一般包括抽象(abstraction)，设计对象(object)和设计合理的层次／接口(decoupling)。这里，我们举一个例子简单说明这些过程分别需要做些什么，在“模式识别”给出更为具体和完整的实例。

> 设计一个音乐播放器，能够管理专辑，播放歌曲。

**抽象**

抽象的意思是能够根据设计要求，得出程序运行的逻辑框架以及定义需要的功能。简单来说，就是构想自己是该程序的使用者， 列举实现程序某个功能需要哪些步骤。比如对于我们所说的例子，播放一首歌的运行流程可能包括：添加歌曲到音乐库，添加歌曲到播放列表，播放，删除等。对于某些系统，系统可以处于多个不同状态，每个状态的功能不同，这就需要构造有限状态机。关于有限状态机的一些参考资料请见“工具箱”。在抽象的过程中，往往需要和面试官进行沟通，确认需要实现什么功能。在抽象结束后，所得到的实体就应该成为一个个对象，而功能就是对象的函数接口。比如，在本例中，涉及的实体有播放器，专辑，歌曲，播放列表。函数包括添加歌曲，删除歌曲，播放，停止，暂停等等。之后，我们就要根据抽象的结果，进行对象设计，并且把所需的功能以函数接口的形式分配给不同的对象。

**设计对象**

通常而言，对于抽象出的每个实体，我们都应该构造一个类去描述它。对象之间的关系可能是继承，或者包含，具体分析请见“ 继承/组合/参数化类型”。对于这里所需要实现的音乐播放器，我们就可以构造播放器，专辑，歌曲，播放列表这几个类。

**设计接口**

接口用于与用户进行交互，以及对象之间的交互。设计接口的核心在于明确每个对象需要实现什么功能，当上层对象调用下层对象的接口时，只需要提供相应的参数，就能够期待获得对应的结果。这样，上层对象不需要知道对方如何获得结果，下层对象也不需要知道对方拿到结果会进行什么样的操作。如此，通过设计恰当的接口我们就实现了decoupling：让程序具有逻辑上的层次，每一层的对象实现特定的功能，对象可以独立地更改实现功能的方法，而不会影响上层和下层。

在设计接口的时候我们可能需要添加一些不那么明显的辅助类，使得程序更具有层次。比如说，考虑添加歌曲这个功能，用户会通过调用播放器的添加歌曲接口，传入一首歌。考虑到当音乐库变得很复杂的时候，我们需要判断诸如该歌曲是否已经存在，应当用怎样的数据库存储数据等等一系列问题。如果都由播放器对象来处理这些问题，会使得播放器这一层变得过为臃肿，减少代码的可读性和可维护性。所以我们可以引入另一个类，歌曲管理器，用来处理数据相关的操作。这样，用户通过播放器接口添加歌曲，播放器调用歌曲管理器的添加接口将新歌写到数据库，歌曲管理器负责打开数据库，判断数据库中是否存在重复的歌曲，写入数据等等。当之后出于某些原因需要更换储存方式的时候，只要更改歌曲管理器的代码即可。同时，如果发现播放器中有重复的歌曲，也只需要查看歌曲管理器的实现过程有什么漏洞，而不是漫无目的的找bug，这样也提升了程序的可维护性。

同时，在确定接口功能的时候，尽量把功能的具体实现向下层“推” 。比如，播放一首歌曲可以包括打开文件，访问文件等。当用户调用播放器接口要求播放一首歌时，我们可以让播放器打开文件，也可以让播放器调用歌曲对象的播放接口，由歌曲对象负责打开文件等操作。这样做的好处在于，用户可能在任何时候播放一首新的歌曲，播放器只需要有一个指针指向当前播放的歌曲对象，如果用户需要播放新的歌曲，播放器只需要：调用当前歌曲的停止接口，设置新的当前播放，调用新歌曲的播放接口即可，而不需要在播放器层反复地进行文件操作。这样，播放器层负责切换歌曲的逻辑，而歌曲层负责具体的播放操作，两者相互独立。这样的好处在于，当以后用户需要更复杂的切歌方式，只需改变播放器的实现即可；当存在更好的音乐编解码，只需要改变歌曲的播放操作即可。我们成功地decouple了切换和播放。

通常，接口的函数定义可以遵从如下模式：函数参数包括输入参数和输出参数，返回值为错误代码，或者是标示操作是否成功的布尔变量。函数需要对参数的有效性进行验证。输出参数通常传入地址，函数内可以将结果直接写到地址中。比如说，创建一个播放列表可以如下定义：

    ErrorResult createPlaylist(Vector<Song *> inSongsArray, Playlist *outPlaylist)

当ErrorResult为0时表示创建成功，outPlaylist指向该播放列表。

经过上述例子，我们作出如下总结：

+ 可以依照抽象，设计对象，设计接口的流程进行思考
+ 针对接口而不是实现来进行编程：不同对象之间保持接口
+ 一致，调用接口时不需要基于接口内的实现方式。

在之后的部分我们会给出一些常见的设计模式。但是，不要拘泥于具体的模式实现方式，我们更应该注重每种模式的意图是什么。

### 继承/组合/参数化类型

在面向对象中最常用的两种代码复用技术就是继承和组合。在设计对象的时候，“Is-A”表示一种继承关系。比如，班长“Is-A”学生，那么，学生就是基类，班长就是派生类。在确定了派生关系之后，我们需要分析什么是基类变量(base class variables)什么是子类变量(sub class variables)，并由此确定基类和派生类之间的联系。而“Has-A”表示一种从属关系，这就是组合。比如，班长“Has-A”眼镜，那就可以解释为班长实例中拥有一个眼镜实例变量(instance variable)。在具体实现的时候，班长类中定义一个眼镜的基类指针。“在生成班长实例的时候，同时生成一个眼镜实例，利用眼镜的基类指针指向这个实例。任何关于眼镜的操作函数都可以利用这个基类指针实现多态(polymorphism)。注意，多态是OOP相关的一个重要概念，也是面试常考的概念之一。关于多态的解释请见“工具箱”。

在通常情况下，我们更偏向于“Has-A”的设计模式。因为该模式减少了两个实例之间的相关性。对于继承的使用，通常情况下我们会定义一个虚基类，由此派生出多个不同的实例类。在业界的程序开发中，多重继承并不常见，Java甚至不允许从多个父类同时继承，产生一个子类。

此外，我们还要提及参数化类型。参数化类型，或者说模版类也是一种有效的代码复用技术。在C++的标准模版库中大量应用了这种方式。例如，在定义一个List<String>的变量时，List被另一个类型String所参数化。

设计模式着重于代码的复用，所以在选择复用技术上，有必要看看上述三种复用技术优劣。

**继承**

+ 通过继承方式，子类能够非常方便地改写父类方法，同时
+ 保留部分父类方法，可以说是能够最快速地达到代码复用。
+ 继承是在静态编译时候就定义了，所以无法再运行时刻改写父类方法。
+ 因为子类没有改写父类方法的话，就相当于依赖了父类这个方法的实现细节,被认为破坏封装性。
+ 并且如果父类接口定义需要更改时，子类也需要提更改响应接口。

**组合**

+ 对象组合通过获得其他对象引用而在运行时刻动态定义的。
+ 组合要求对象遵守彼此约定，进而要求更仔细地定义接口，而这些接口并不妨碍你将一个对象和另外一个对象一起使用。
+ 对象只能够通过接口来访问，所以我们并没有破坏封装性。
+ 而且只要抽象类型一致，对象是可以被替换的。
+ 使用组合方式，我们可以将类层次限制在比较小的范围内，不容易产生类的爆炸。
+ 相对于继承来说,组合可能需要编写“更多的代码。

**参数化类型**

+ 参数化类型方式是基于接口的编程，在一定程度上消除了类型给程序设计语言带来的限制。
+ 相对于组合方式来说，缺少的是动态修改能力。
+ 因为参数化类型本身就不是面向对象语言的一个特征，所以在面向对象的设计模式里面，没有一种模式是于参数化类型相关的。
+ 实践上我们方面是可以使用参数化类型来编写某种模式的。

**总结**

+ 对象组合技术允许你在运行时刻改变被组合的行为，但是它存在间接性，相对来说比较低效。
+ 继承允许你提供操作的缺省实现，通过子类来重定义这些操作，但是不能够在运行时改变。
+ 参数化允许你改变所使用的类型，同样不能够在运行时改变。

## 设计模式

所谓的设计模式是指人们在开发软件的过程中，对于一些普适需求而总结的设计模版。根据模式目的可以分为三类：

+ 创建型(Creational).创建型模式与对象的创建相关。
+ 结构型(Structural).结构型模式处理类或者是对象的组合。
+ 行为型(Behavioral).行为型模式对类或者是对象怎样交互和怎样分配职责进行描述。

下面我们对每种类型进行介绍。具体的模式请见“工具箱”。值得提醒的是，在面试或工作中不可盲目相信设计模式。设计模式更多地只是提供一些思路，能够直接套用设计模式的情况并不多，更多的时候是对现成设计模式的改进和组合。所以对于设计模式的学习更多应该着眼于模式的意图，而不是模式的具体实现方法。

### 创建型

一个类的创建型模式使用继承改变被实例化的类，而一个对象的创建型模式将实例化委托给另外一个对象。 在这些模式中有两种不断出现的主旋律：

+ 将该系统使用哪些具体的类封装起来
+ 隐藏了实例是如何被创建和存储的

总而言之，效果就是用户创建对象的结果是得到一个基类指针，用户通过基类指针调用继承类的方法。用户不需要知道在使用哪些继承类。

#### 单例模式

意图：单例模式(Singleton Pattern)是一种常见的设计模式。其目的在于保证一个类仅仅有一个实例并且提供一个访问它的全局访问点。

这个模式主要的对比对象就是全局变量。相对于全局变量，单例有下面这些好处：

+ 全局变量不能够保证只有一个实例。
+ 某些情况下面，我们需要稍微计算才能够初始化这个单例。全局变量也行但是不自然。
+ C++下面没有保证全局变量的初始化顺序.

比如，在我们之前说的音乐播放器设计中，我们引入了歌曲管理器实现数据的存储。歌曲管理器在整个程序中应当实例化一次，其他所有关于数据的操作都应该在这个实例上进行。所以，歌曲管理器应该应用单例模式。实现单例模式的关键在于利用静态变量(static variable)，通过判断静态变量是否已经初始化判断该类是否已经实例化。此外，还需要把构造函数设为私有函数，通过公共接口getSharedInstance进行调用。我们举例如下：

```
// Example for singleton pattern
// class definition
class MySingleton {
private:
// Private Constructor
    MySingleton();
// Stop the compiler generating methods of copy the object
    MySingleton(const MySingleton &copy);    // Not Implemented
    MySingleton &operator=(const MySingleton &copy);    // Not Implemented
    static MySingleton *m_pInstance;
public:
    static MySingleton *getSharedInstance() {
        if (!m_pInstance) {
            m_pInstance = new MySingleton;
        }
        return m_pInstance;
    }
};

// in the source file
MySingleton *MySingleton::m_pInstance = NULL;
```

注意，本例中的实现方式针对非多线程的情况。如果有过个线程想要同时调用getSharedInstance函数，则需要用mutex保护下列代码：

    pthread_mutex_lock(&mutex);
    if (!m_pInstance) {
        m_pInstance = new MySingleton;
    }
    pthread_mutex_unlock(&mutex);

#### 工厂模式

意图：抽象类需要创建一个对象时，让子类决定实例化哪一个类

所谓的工厂模式(Factory Pattern)，就是指定义一个创建对象的接口，但让实现这个接口的类来决定实例化哪个类。通常，接口提供传入参数，用以决定实例化什么类。工厂模式常见于工具包和框架中，当需要生成一系列类似的子类时，可以考虑使用工厂模式。举例如下：

```
// class for factory pattern
enum ImageType{
    GIF,
    JPEG
};

class ImageReader {
    // implementation for image reader base class
};

class GIFReader : public ImageReader {
    // implementation for GIF reader derived class
};

class JPEGReader : public ImageReader {
    // implementation for JPEG reader derived class
};

class ImageReaderFactory {
public:
    static ImageReader *imageReaderFactoryMethod(ImageType imageType) {
        ImageReader *product = NULL;
        switch (imageType) {
            case GIF:
                product = new GIFReader();
            case JPEG:
                product = new JPEGReader();
                //...
        }
        return product;
    }
};
```

### 结构型

类的结构型模式采用继承机制来组合接口。对象的结构型模式不是对接口进行组合， 而是描述如何对一些对象进行组合，从而实现新功能。

#### 适配器

意图：适配器(Adapter)将一个类的接口转化成为客户希望的另外一个接口。

假设A实现了Foo()接口，但是B希望A同样实现一个Bar()接口，事实上Foo()基本实现了Bar()接口功能。 Adapter模式就是设计一个新类C，C提供Bar()接口，但实现的方式是内部调用 A的Foo()。

在实现层面上可以通过继承和组合两种方式达到目的：C可以继承A，或者C把A作为自己的成员变量。两者孰优孰劣需要视情况而定。

### 行为型

行为型涉及到算法和对象之间职责的分配。行为模式不仅描述对象或者类的功能行为，还描述它们之间的通信模式。 这些模式刻画了在运行时难以追踪的控制流，它们将你的注意从控制流转移到对象之间的联系上来。

#### 观察者

意图：观察者模式(observer)定义对象之间的依赖关系，当一个对象“状态发生改变的话，所有依赖这个对象的对象都会被通知并且进行更新。

被观察的对象需要能够动态地增删观察者对象，这就要求观察者提供一个公共接口比如Update()。然后每个观察者实例注册到被观察对象里面去，在被观察对象状态更新时候能够遍历所有注册观察者并且调用Update()。

至于观察者和被观察之间是采用push还是pull模式完全取决于应用。对于观察这件事情来说的话， 我们还可以引入方面(Aspect)这样一个概念，在注册观察者的时候不仅仅只是一个观察者对象， 还包括一个Aspect参数，可以以此告诉被观察者仅在发生某些变化时通过调用Update()通知我。

#### 状态

意图：状态模式(state)允许一个对象在其内部状态改变时改变它的行为。

这里状态模式意图是，对于实例A，当A的状态改变时，将A可能改变的行为封装成为一个类S(有多少种可能的状态就有多少个S的子类,比如S1,S2,S3等)。当A的状态转换时，在A内部切换S的实例。从A的用户角度来看，A的接口不变，但A的行为因A的状态改变而改变，这是因为行为的具体实现由S完成。

## 模式识别

> Design a parking lot using object-oriented principles.

解题分析：我们尝试使用抽象->设计对象->设计接口的流程。

**抽象**

在这一步，我们重现现实中车库的工作原理。通常，车库可能有不同的层次，每层都有若干个车位，汽车可以停在车位上。车库的主要功能在于实现车辆入库和车辆出库，根据汽车在车库中停留的时间收费。

**设计对象**

经过抽象分析，我们发现了车位、层次、车库、汽车这些实体。对于每个实体，我们都应该构造一个类去描述它。我们考虑各个实体之间的从属，继承关系：很明显，车库拥有不同层次，每个层次拥有一些车位。因此，车库、层次、车位属于“Has-A”的关系。考虑我们可能希望车库是一个全局都可以访问的变量，而且程序中应该只有一个车库实例，所以我们可以利用单例模式，把车库作为一个单例。进一步考虑汽车实体，现实中，有各种类型的汽车，不同类型的车辆对车位的要求也不一样。然而，汽车具有一些共同属性，比如车长，车宽等等，特别地，对于本例，每辆车都需要记录停车的状态。所以，我们可以考虑从一个汽车基类派生出不同类型的汽车派生类。

**设计接口**

车库需要与用户进行交互，因此应该提供车辆入库和车辆出库的接口。车辆入库时，需要从最底层依次向上寻找可用的车位，因此，寻找车位应该一个是由层次提供的接口，返回一个车位。车库把这个车位提供给一个汽车实例，并且标记车位不可用。当车辆出库时，我们需要汽车提供它停车位置的信息(因此，车辆需要提供接口返回它停车的位置)，车库需要计算停车费，并且标记车位可用。

参考解答：

```
// define some constants
enum ErrorCode {
    NO_ERROR,
    ERROR
};

enum SpotType {
    COMPACT,
    SUV,
    RESERVED
};
#define NO_PARKING (-1)

class Spot {
public:
    bool     available;
    SpotType type;
};

class Vehicle {
private:
    int     length;
    int     width;
    bool    parked;
    Spot    *spot;
public:
    // omit some setters / getters
    // virtual function here because subclasses will have different behavior
    virtual SpotType getRequiredSpotType() = 0;
    // no need for virtual functions here because subclasses will have the same "behavior"
    bool isParked();
    void parkVehicle(Spot *s);  // park at spot S;
    Spot *removeVehicle();      // move the vehicle away, return parked spot
};

//every type of vehicle has default value of length and width;
class Motor:public Vehicle{};
class Car:public Vehicle{};
class SUV:public Vehicle{};


class Level {
private:
    vector<Spot> spots;
public:
    // find an available spot for a vehicle
    // return NULL if all spots are taken
    Spot *findASpot(Vehicle *v);    
};

class ParkingLot {
private:
    vector<Level> levels;
    static ParkingLot *pInstance;
    unordered_map<Vehicle *, time_t> parkingInfo;

    ParkingLot();
    // Stop the compiler generating methods of copy the object
    ParkingLot(const ParkingLot &copy);    // Not Implemented
    ParkingLot& operator = (const ParkingLot & copy);    // Not Implemented

    time_t getCurrentTime();
    double calculateFee(Vehicle *v);

public:
    static ParkingLot *getInstance();
    // NOTE: vehicleEnter and leave is not thread safe!
    ErrorCode vehicleEnter(Vehicle *v);
    ErrorCode vehicleLeave(Vehicle *v, double *fee);
};

ErrorCode ParkingLot::vehicleEnter(Vehicle *v) {
    Spot *spot = NULL;
    for (int i = 0; i < levels.size();i++) {
        spot = levels[i].findASpot(v);
        if (spot)
            break;
    }
    if (!spot) {
        return ERROR;
    }
    v->parkVehicle(spot);
    spot->available = false;
    parkingInfo[v] = getCurrentTime();
    return NO_ERROR;
}

ErrorCode ParkingLot::vehicleLeave(Vehicle *v, double *fee) {
    *fee = 0;
    if (!v->isParked()) {
        return ERROR;
    }
    Spot *spot = v->removeVehicle();
    spot->available = true;
    *fee = calculateFee(v);
    parkingInfo.erase(v);
    return NO_ERROR;
}
```

进一步讨论：

我们提供的代码并不是线程安全的，当多个线程同时调用enter和leave的时候，可能造成停车状态的不一致，需要通过加锁解决。其次，查找车位的时候我们实现了最简单的线性查找。事实上，我们可以用一个队列记录可用的车位，每次只需要弹出一个即可。那对于不同的车位类型怎么处理？我们可以用多个队列记录可用的车位，每个队列对应一个车型。

> Design an elevator bank for a building, with multiple elevators

解题分析：我们尝试使用抽象->设计对象->设计接口的流程。

**抽象**

在这一步，我们重现现实中电梯间的工作原理：电梯间拥有多部电梯，当有用户需要乘坐电梯时，电梯间分配一个最优的电梯去用户所需的楼层。当电梯需要维修时，电梯间可以停止某部电梯。

**设计对象**

经过抽象分析，我们发现了电梯间和电梯这两个实体。对于每个实体，我们都应该构造一个类去描述它。我们考虑各个实体之间的从属关系：很明显，电梯间拥有一些电梯。因此，电梯间和电梯属于“Has-A”的关系 。对于一部电梯，它应该具有当前的运行状态，包括停止在某一层，向上运行和向下运行。电梯间可以根据当前各个电梯的位置和运行情况，选择最优的电梯去满足用户需求，可以采用下述选择逻辑， 计算一个“距离分”，距离分越低，则优先级越高：

1. 当电梯处于停止状态，距离分等于当前楼层和需求楼层的差
2. 当电梯处于上升状态，如果需求楼层高于当前楼层，则距离分等于当前楼层和需求楼层的差， 否则不考虑当前电梯
3. 当电梯处于下降状态，如果需求楼层低于当前楼层，则距离分等于当前楼层和需求楼层的差， 否则不考虑当前电梯
4.  算法开始时先随机选择一部电梯，当存在更好的电梯选择时，替代当前的候选电梯，这样确保至少会有一部电梯响应(在少数情况下这样做不一定是最优的，可以设想更好的算法解决这个问题)。

**设计接口**

用户所有的请求都应该与电梯间进行交互。电梯间再选择最恰当的电梯，去满足用户的请求。电梯也需要提供接口，以供电梯间设置一个请求。同时，电梯间依赖于电梯的当前位置和运行状况实现选择算法，所以电梯应该提供getter函数。此外，电梯间还应该提供启用／停止某台电梯的接口，以供管理员进行维护。至于电梯间选择电梯的算法函数等等，用户不需要知道这些信息，故应该作为私有函数。

**多线程**

电梯需要在后台不停地自动运行，同时还需要响应电梯间设置的用户请求。因此，我们需要考虑多线程。我们可以用主线程响应用户请求，同时建立另一个线程模拟电梯的运行过程。注意，由于使用了多线程，所有的线程共享变量都需要用锁保护起来，避免产生竞争(racing condition)。同时，考虑到打印调试信息也可能产生竞争，所以应该放到一个特定的打印线程处理所有输出。在参考解答中，我们给出在MacOS下利用dispatch_queue的实现方式。

参考解答：

```
// define some constants
dispatch_queue_t printingQueue;
enum ElevatorState {
    UP,           // moving up
    DOWN,    // moving down
    STAND    // stay at a level, waiting for request
};

class Elevator {
private:
    ElevatorState state;
    int id;
    int currentLevel;
    int maxLevel;
    int minLevel;
    // floor requests from low level to high level
    vector<int> requests;    
    pthread_mutex_t lock;
    // runLoop is called on this thread
    pthread_t runThread;    

    // utility methods to lock and unlock
    void mutexLock();       
    void mutexUnlock();

    // check if need to stop on current floor
    bool needStop();
    // switch to STAND if no request pending.
    // go DOWN if currently going UP, and no further higher level requests
    // go UP if currently going DOWN, and no further lower level requests
    void updateState();    

    // move one floor up/down. Open door if needed. Update state
    void move(); 
    // open door and close door, delete request from array
    void openDoor(); 
    // thread safe operation, main logic for elevator operation (move, open door)
    void runLoop();    
    
    static void *elevatorProc(void *parameter);

public:
    Elevator(int id, int minLevel, int maxLevel);
    ~Elevator();

    // thread safe operation, caller adds a new stop request
    ErrorCode addRequest(int newRequest);   
    // thread safe operation, start elevatorProc to activate elevator
    void startOperation();  
    // thread safe operation, stop elevatorProc to deactivate elevator
    void stopOperation();  
    // thread safe operation, get elevator state
    ElevatorState getState();  
    // thread safe operation, get elevator floor
    int getCurrentLevel();    

#pragma -mark Test Methods
    void printRequests() {
        // caller should have the data lock
        dispatch_async(printingQueue, ^{
            cout << "Elevator " << id << " current requests: ";
            for (int i = 0 ; i < requests.size(); i++) {
                cout << requests[i] << ' ';
            }
            cout << endl;
        });
    }
};

#pragma -mark Private Methods

bool Elevator::needStop() {
    for (int i = 0; i < requests.size(); i++) {
        if (requests[i] == currentLevel) {
            return true;
        }
    }
    return false;
}

void Elevator::updateState() {
    switch (state) {
        case UP:
            if (requests.size() == 0) {
                state = STAND;
            } else {
                if (requests.back() < currentLevel) {
                    state = DOWN;
                }
            }
            break;
        case DOWN:
            if (requests.size() == 0) {
                state = STAND;
            } else {
                if (requests.front() > currentLevel) {
                    state = UP;
                }
            }
            break;
        default:
            break;
    }

}

void Elevator::move() {
    switch (state) {
        case UP:
            if (needStop()) {
                openDoor();
                // flip state
                updateState();
            } else {
                dispatch_async(printingQueue, ^{
                    cout << "Elevator " << id << " move from " << currentLevel
                    << " to " << currentLevel + 1 << endl;
                });
                currentLevel++;
            }
            break;

        case DOWN:
            if (needStop()) {
                openDoor();
                // flip state
                updateState();
            } else {
                dispatch_async(printingQueue, ^{
                    cout << "Elevator " << id << " move from " << currentLevel
                    << " to " << currentLevel - 1 << endl;
                });
                currentLevel--;
            }
            break;
        default:
            break;
    }
}

void Elevator::openDoor() {
    dispatch_async(printingQueue, ^{
        cout << "Elevator " << id << " arriving at " << currentLevel << endl;
“        cout << "Elevator " << id << " door open" << endl;
    });
    for (vector<int>::iterator it = requests.begin(); it != requests.end(); ) {
        if (*it == currentLevel) {
            it = requests.erase(it);
        } else {
            ++it;
        }
    }
    dispatch_async(printingQueue, ^{
        cout << "Elevator " << id << " door close" << endl;
    });
    printRequests();
}

void Elevator::runLoop() {
    mutexLock();
    switch (state) {
        case UP:
        case DOWN:
            move();
            break;
        default:
            break;
    }
    mutexUnlock();
}

void *Elevator::elevatorProc(void *parameter) {
    Elevator *elevator = (Elevator *)parameter;
    while (1) {
        elevator->runLoop();
        usleep(4000000);
    }
    return NULL;
}

#pragma -mark Public Methods

Elevator::Elevator(int inID, int inMinLevel, int inMaxLevel) {
    state = STAND;
    id = inID;
    maxLevel = inMaxLevel;
    minLevel = inMinLevel;
    currentLevel = inMinLevel;
    pthread_mutex_init(&lock, NULL);
}

Elevator::~Elevator() {
    stopOperation();
    pthread_mutex_destroy(&lock);
}

void Elevator::mutexLock() {
    pthread_mutex_lock(&lock);
}

void Elevator::mutexUnlock() {
    pthread_mutex_unlock(&lock);
}

void Elevator::startOperation() {
    mutexLock();
    pthread_create(&runThread, NULL, elevatorProc, this);
    mutexUnlock();
}

void Elevator::stopOperation() {
    mutexLock();
    if (runThread) {
        pthread_cancel(runThread);
    }
    mutexUnlock();
}

ErrorCode Elevator::addRequest(int newRequest) {
    mutexLock();
    if (newRequest > maxLevel || newRequest < minLevel) {
        cerr << "Invalid request " << newRequest << endl;
        return ERROR;
    }
    if (requests.size() == 0) {
        requests.push_back(newRequest);
        if (newRequest > currentLevel) {
            dispatch_async(printingQueue, ^{
                cout << "Elevator " << id << " moving up" << endl;
            });
            state = UP;
        } else if (newRequest == currentLevel) {
            state = STAND;
            openDoor();
        } else {
            dispatch_async(printingQueue, ^{
                cout << "Elevator " << id << " moving down" << endl;
            });
            state = DOWN;
        }
        goto Done;
    }

    for (vector<int>::iterator i = requests.begin(); i != requests.end(); i++) {
        if (newRequest == *i) {
            goto Done;
        }
        if(*i > newRequest) {
            requests.insert(i, newRequest);
            goto Done;
        }
    }
    requests.push_back(newRequest);
Done:
    printRequests();
    mutexUnlock();
    return NO_ERROR;
}

ElevatorState Elevator::getState() {
    mutexLock();
    ElevatorState currentState = state;
    mutexUnlock();
    return currentState;
}

int Elevator::getCurrentLevel() {
    mutexLock();
    int level = currentLevel;
    mutexUnlock();
    return level;
}

class ElevatorBank {
private:
    int numberOfElevators;
    vector<Elevator> elevators;
    int calculateScore(int index, int level);
    bool isBetterNewCandidate(int currentCandidateIndex, int newCandidateIndex, int level);
    int selectAnElevator(int level);
“public:
    ElevatorBank(int numberOfElevators, int minFloor, int maxFloor);
    ~ElevatorBank();
    // index start from 0
    ErrorCode startAnElevator(int index);   
    // index start from 0
    ErrorCode stopAnElevator(int index);        
    ErrorCode setARequest(int level);
};

#pragma -mark Private Methods
int ElevatorBank::calculateScore(int index, int level) {
    if (elevators[index].getState() == STAND) {
        return abs(elevators[index].getCurrentLevel() - level);
    }

    if (elevators[index].getCurrentLevel() > level
        && elevators[index].getState() == DOWN) {
        return elevators[index].getCurrentLevel() - level;
    }

    if (elevators[index].getCurrentLevel() < level
        && elevators[index].getState() == UP) {
        return level - elevators[index].getCurrentLevel();
    }
    return INT_MAX;
}

bool ElevatorBank::isBetterNewCandidate(int currentCandidateIndex, int newCandidateIndex, int level) {
    return calculateScore(currentCandidateIndex, level) > calculateScore(newCandidateIndex, level);
}

int ElevatorBank::selectAnElevator(int level) {
    int selectedIndex = 0;
    for (int i = 1; i < numberOfElevators; i++) {
        if (isBetterNewCandidate(selectedIndex, i, level)) {
            selectedIndex = i;
        }
    }
    return selectedIndex;
}

#pragma -mark Public Methods

ElevatorBank::ElevatorBank(int inNumberOfElevators, int minFloor, int maxFloor) {
    numberOfElevators = inNumberOfElevators;
    for (int i = 0; i < inNumberOfElevators; i++) {
        elevators.push_back(Elevator(i, minFloor, maxFloor));
    }
}

ElevatorBank::~ElevatorBank() {
    elevators.clear();
}

ErrorCode ElevatorBank::startAnElevator(int index) {
    if (index > numberOfElevators || index < 0) {
        cerr << "Index " << index << " invalid!" << endl;
        return ERROR;
    }
    elevators[index].startOperation();
    return NO_ERROR;
}

ErrorCode ElevatorBank::stopAnElevator(int index) {
    if (index > numberOfElevators || index < 0) {
        cerr << "Index " << index << " invalid!" << endl;
        return ERROR;
    }
    elevators[index].stopOperation();
    return NO_ERROR;
}

ErrorCode ElevatorBank::setARequest(int level) {
    int index = selectAnElevator(level);
    return elevators[index].addRequest(level);
}

int main() {
    printingQueue = dispatch_queue_create("elevatorBank.printingQueue", NULL);
    ElevatorBank elevatorBank = ElevatorBank(2, 1, 10);
    for (int i = 0; i < 2; i++) {
        elevatorBank.startAnElevator(i);
    }
    int request;
    while (cin >> request) {
        elevatorBank.setARequest(request);
    };
    return 0;
}
```

## 工具箱

### 有限状态机

参见[这里](http://en.wikipedia.org/wiki/Finite-state_machine)

### 多态

在C++中，最常见的多态指的是用基类指针指向一个派生类的实例，当用该指针调用一个基类中的虚函数时，实际调用的是派生类的函数实现，而不是基类函数。如果该指针指向另一个派生类实例，则调用另一个派生类的函数实现。因此，比如工厂模式返回一个实例，上层函数不需要知道实例来自哪个派生类，只需要用一个基类指针指向它，就可以直接获得需要的行为。从编译的角度来看，函数的调用地址并不是在编译阶段静态决定，而是在运行阶段，动态地决定函数的调用地址。

多态是通过虚函数表实现的。当基类中用virtual关键字定义函数时，系统自动分配一个指针，指向该类的虚函数表。虚函数表中存储的是函数指针。在生成派生类的时候，会将派生类中对应的函数的地址写到虚函数表。之后，当利用基类指针调用函数时，先通过虚函数表指针找到对应的虚函数表，再通过表内存储的函数指针调用对应函数。由于函数指针指向派生类的实现，因此函数行为自然也就是派生类中定义的行为了。

### 创建型设计模式补充

#### Builder

**意图：将一个复杂对象构建过程和元素表示分离。**

假设我们需要创建一个复杂对象，而这个复杂对象是由很多元素构成的。这些元素的组合逻辑可能非常复杂， 但是逻辑组合和创建这些元素是无关的，独立于这些元素本身的。

那么我们可以将元素的组合逻辑以及元素构建分离，元素构建我们单独放在Builder这样一个类里面，而元素的组合逻辑通过Director来指导，Director内部包含Builder对象。创建对象是通过Director来负责组合逻辑部分的， Director内部调用Builder来创建元素并且组装起来。最终通过Builder的GetResult来获得最终复杂对象。

### 结构型设计模式补充

#### Bridge

**意图：将抽象部分和具体实现相分离，使得它们之间可以独立变化。**

一个很简单的例子就是类Shape,有个方法Draw[抽象]和DrawLine[具体]和DrawText[具体],而Square和SquareText 继承于Shape实现Draw()这个方法，Square调用DrawLine()，而SquareText调用DrawLine()+DrawText()。而且假设DrawLine和DrawText分别有LinuxDrawLine,LinuxDrawText和Win32DrawLine和Win32DrawText。如“果我们简单地 使用子类来实现的话，比如构造LinuxSquare,LinuxSquareText,Win32Square和Win32SquareText，那么很快就会类爆炸。

事实上我们没有必要在Shape这个类层面跟进变化，即通过继承Shape类实现跨平台，而只需要在实现底层跟进变化。为此我们就定义一套接口，如例子中的DrawLine和DrawText，然后在Linux和Win32下实现一个这样接口实例(比如称为跨平台GDI)，最终 Shape内部持有这个GDI对象，Shape的DrawLine和DrawText只是调用GDI的接口而已。这样，我们把Shape及其子类的DrawLine和DrawText功能Bridge到GDI，GDI可以通过工厂模式在不同平台下实现不同的实例。

例子中Shape成为了完全抽象的部分，具体实现完全交给GDI类，若以后需要增加更多的平台支持，开发者也不需要添加更多的Shape子类，只需要扩展GDI即可。总之，抽象部分是和具体实现部分需要独立开来的时候，就可以使用Bridge模式。

#### Composite

**意图：将对象组合成为树形以表示层级结构，对于叶子和非叶子节点对象使用需要有一致性。**

Composite模式强调在这种层级结构下，叶子和非叶子节点需要一致对待，所以关键是需要定义一个抽象类，作为叶节点的子节点。 然后对于叶子节点操作没有特殊之处，而对于非叶子节点操作不仅仅需要操作自身，还要操作所管理的子节点。 至于遍历子节点和处理顺序是由应用决定的，在Composite模式里面并不做具体规定。

#### Decorator

**意图：动态地给对象添加一些额外职责，通过组合而非继承方式完成。**

给对象添加一些额外职责，例如增加新的方法，很容易会考虑使用子类方式来实现。使用子类方式实现很快但是却不通用，考虑一个抽象类X，子类有SubX1,SubX2等。现在需要为X提供一个附加方法echo，如果用继承的方式添加，那么需要为每个子类都实现echo方法，并且代码往往是重复的。我们可以考虑Decorator模式，定义一个新类，使其持有持有指向X基类的指针，并且新类只需要单独实现echo方法，而其他方法直接利用X基类指针通过多态调用即可。

值得注意的是，装饰出来的对象必须包含被装饰对象的所有接口。所以很明显这里存在一个问题， 那就是X一定不能够有过多的方法，不然Echo类里面需要把X方法全部转发一次(理论上说Echo类可以仅转发X的部分方法，但Decorator默认需要转发被装饰类的全部方法)。

#### Façade

**意图：为子系统的一组接口提供一个一致的界面。**

编译器是一个非常好的的例子。对于编译器来说，有非常多的子系统包括词法语法解析，语义检查,中间代码生成，代码优化，以及代码生成这些逻辑部件。但是对于大多数用户来说，不关心这些子系统，而只是关心编译这一个过程。

所以我们可以提供Compiler的类，里面只有很简单的方法比如Compile()，让用户直接使用Compile()这个接口。 一方面用户使用起来简单，另外一方面子系统和用户界面耦合性也降低了。

Facade模式对于大部分用户都是满足需求的。对于少部分不能够满足需求的用户，可以让他们绕过Facade模式提供的界面， 直接控制子系统即可。就好比GCC提供了很多特殊优化选项来让高级用户来指定，而不是仅仅指定-O2这样的选项。

#### Proxy

**意图：为其他对象提供一种代理以控制对这个对象的访问。**

通常使用Proxy模式是想针对原本要访问的对象做一些手脚，以达到一定的目的，包括访问权限设置，访问速度优化，或者是加入一些自己特有的逻辑。至于实现方式上，不管是继承还是组合都行，可能代价稍微有些不同，视情况而定。但是偏向组合方式，因为对于Proxy而言，完全可以定义一套新的访问接口。

Adapter,Decorator以及Proxy之间比较相近，虽然说意图上差别很大，但是对于实践中， 三者都是通过引用对象来增加一个新类来完成的，但是这个新类在生成接口方面有点差别：

+ Adapter模式的接口一定要和对接的接口相同。
+ Decorator模式的接口一定要包含原有接口，通常来说还要添加新接口。
+ Proxy模式完全可以重新定义一套新的接口

### 行为型设计模式补充

#### Chain of Responsibility

**意图：将对象连成一条链并沿着链传递某个请求，直到有某个对象处理它为止。**

大部分情况下连接起来的对象本身就存在一定的层次结构关系，少数情况下面这些连接起来的对象是内部构造的。 职责链通常与Composite模式一起使用，一个构件的父构件可以作为它的后继结点。许多类库使用职责链模式来处理事件， 比如在UI部分的话View本来就是相互嵌套的，一个View对象可能存在Parent View对象。如果某个UI不能够处理事件的话， 那么完全可以交给Parent View来完成事件处理以此类推。 

#### Command

**意图：将一个请求封装成为一个对象。**

Command模式可以说是回调机制(Callback)的一个面向对象的替代品。对于回调函数来说需要传递一个上下文参数(context)， 同时内部附带一些逻辑。将上下文参数以及逻辑包装起来的话那么就是一个Command对象。 Command对象接口可以非常简单只有Execute/UnExecute，但是使用Command对象来管理请求之后， 就可以非常方便地实现命令的复用，排队，重做，撤销，事务等。

#### Iterator

**意图：提供一种方法顺序访问一个聚合对象中各个元素，但是又不需要暴露该对象内部表示。**

将遍历机制与聚合对象表示分离，使得我们可以定义不同的迭代器来实现不同的迭代策略，而无需在聚合对象接口上面列举他们。 一个健壮的迭代器,应该保证在聚合对象上面插入和删除操作不会干扰遍历，“同时不需要copy这个聚合对象。 一种实现方式就是在聚合对象上面注册某个迭代器，一旦聚合对象发生改变的话，需要调整迭代器内部的状态。

#### Template Method

**意图：定义一个操作里面算法的骨架，而将一些步骤延迟到子类。**

假设父类A里面有抽象方法Step1(),Step2(),默认方法Step3()。并且A提供一个操作X()，分别依次使用Step1(),Step2(),Step3()。对于A的子类，通过实现自己的Step1(),Step2() (选择性地实现Step3())，提供属于子类的X具体操作。 这里操作X()就是算法的骨架，子类需要复写其中部分step，但不改变X的执行流程。

很重要的一点是模板方法必须指明哪些操作是钩子操作(可以被重定义的，比如Step3),以及哪些操作是抽象操作“(必须被重定义，比如Step1和Step2)。要有效地重用一个抽象类，子类编写者必须明确了解哪些操作是设计为有待重定义的。
