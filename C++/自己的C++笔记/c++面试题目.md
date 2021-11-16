转自：http://blog.csdn.net/allen_fan_01/article/details/9713555
1.new、delete、malloc、free关系
delete会调用对象的析构函数,和new对应，free只会释放内存，new调用构造函数。malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。对于非内部数据类型的对象而言，光用malloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以及一个能完成清理与释放内存工作的运算符delete。注意new/delete不是库函数。
2.delete与 delete []区别
delete只会调用一次析构函数，而delete[]会调用每一个成员的析构函数。在More Effective  C++中有更为详细的解释：“当delete操作符用于数组时，它为每个数组元素调用析构函数，然后调用operator delete来释放内存。”delete与New配套，delete []与new []配套
MemTest*mTest1=newMemTest[10];  
MemTest*mTest2=newMemTest;  
int*pInt1=newint[10];  
int*pInt2=newint;  
delete[]pInt1;  //-1-  
delete[]pInt2;  //-2-  
delete[]mTest1;//-3-  
delete[]mTest2;//-4-  
  在-4-处报错。
这就说明：对于内建简单数据类型，delete和delete[]功能是相同的。对于自定义的复杂数据类型，delete和delete[]不能互用。delete[]删除一个数组，delete删除一个指针。简单来说，用new分配的内存用delete删除，用new[]分配的内存用delete[]删除。delete[]会调用数组元素的析构函数。内部数据类型没有析构函数，所以问题不大。如果你在用delete时没用括号，delete就会认为指向的是单个对象，否则，它就会认为指向的是一个数组。
3.C和C++ 的共同点？不同之处？
C是面向过程的编程语言 
而C++是面向对象的编程语言! 
C++在完全兼容C语言的基础上,增加了<strong>类,引用</strong>等许多概念,把编程的核心由函数转移到类 
C语言主要用语编系统。

4.继承的优缺点。
类继承是在编译时刻静态定义的，且可直接使用，类继承可以较方便地改变父类的实现。但是类继承也有一些不足之处。首先，因为继承在编译时刻就定义了，所以无法在运行时刻改变从父类继承的实现。更糟的是，父类通常至少定义了子类的部分行为，父类的任何改变都可能影响子类的行为。如果继承下来的实现不适合解决新的问题，则父类必须重写或被其他更适合的类替换。这种依赖关系限制了灵活性并最终限制了复用性。
5.C++有哪些性质（面向对象特点）
封装，继承和多态。
在面向对象程序设计语言中，封装是利用可重用成分构造软件系统的特性，它不仅支持系统的可重用性，而且还有利于提高系统的可扩充性；消息传递可以实现发送一个通用的消息而调用不同的方法；封装是实现信息隐蔽的一种技术，其目的是使类的定义和实现分离。
6.子类析构时要调用父类的析构函数吗？
析构函数调用的次序是先派生类的析构后基类的析构，也就是说在基类的的析构调用的时候,派生类的信息已经全部销毁了。定义一个对象时先调用基类的构造函数、然后调用派生类的构造函数；析构的时候恰好相反：先调用派生类的析构函数、然后调用基类的析构函数
Java无析构函数深拷贝和浅拷贝
7.多态，虚函数，纯虚函数
多态：是对于不同对象接收相同消息时产生不同的动作。
C++的多态性具体体现在运行和编译两个方面：
在程序运行时的多态性通过继承和虚函数来体现；
在程序编译时多态性体现在函数和运算符的重载上
虚函数：在基类中冠以关键字 virtual 的成员函数。 它提供了一种接口界面。允许在派生类中对基类的虚函数重新定义。
纯虚函数的作用：在基类中为其派生类保留一个函数的名字，以便派生类根据需要对它进行定义。作为接口而存在 纯虚函数不具备函数的功能，一般不能直接被调用。
从基类继承来的纯虚函数，在派生类中仍是虚函数。如果一个类中至少有一个纯虚函数，那么这个类被称为抽象类（abstract class）。
抽象类中不仅包括纯虚函数，也可包括虚函数。抽象类必须用作派生其他类的基类，而不能用于直接创建对象实例。
但仍可使用指向抽象类的指针支持运行时多态性。
8.求下面函数的返回值（微软）
int func(x) 
{ 
    int countx = 0; 
    while(x) 
    { 
          countx ++; 
          x = x&(x-1); 
     } 
    return countx; 
} 
假定x = 9999。 答案：8
思路：将x转化为2进制，看含有的1的个数。
9.什么是“引用”？申明和使用“引用”要注意哪些问题？
答：引用就是某个目标变量的“别名”(alias)，对应用的操作与对变量直接操作效果完全相同。
申明一个引用的时候，切记要对其进行初始化。引用声明完毕后，相当于目标变量名有两个名称，即该目标原名称和引用名，不能再把该引用名作为其他变量名的别名。声明一个引用，不是新定义了一个变量，它只表示该引用名是目标变量名的一个别名，它本身不是一种数据类型，因此引用本身不占存储单元，系统也不给引用分配存储单元。不能建立数组的引用。
10.将“引用”作为函数参数有哪些特点？
（1）传递引用给函数与传递指针的效果是一样的。这时，被调函数的形参就成为原来主调函数中的实参变量或对象的一个别名来使用，所以在被调函数中对形参变量的操作就是对其相应的目标对象（在主调函数中）的操作。
（2）使用引用传递函数的参数，在内存中并没有产生实参的副本，它是直接对实参操作；而使用一般变量传递函数的参数，当发生函数调用时，需要给形参分配存储单元，形参变量是实参变量的副本；如果传递的是对象，还将调用拷贝构造函数。因此，当参数传递的数据较大时，用引用比用一般变量传递参数的效率和所占空间都好。
（3）使用指针作为函数的参数虽然也能达到与使用引用的效果，但是，在被调函数中同样要给形参分配存储单元，且需要重复使用"*指针变量名"的形式进行运算，这很容易产生错误且程序的阅读性较差；另一方面，在主调函数的调用点处，必须用变量的地址作为实参。而引用更容易使用，更清晰。
11.在什么时候需要使用“常引用”？　
如果既要利用引用提高程序的效率，又要保护传递给函数的数据不在函数中被改变，就应使用常引用。
常引用声明方式：const 类型标识符 &引用名=目标变量名；
例1
int a ;
const int &ra=a;
ra=1; //错误
a=1; //正确
例2
string foo( );
void bar(string & s);
那么下面的表达式将是非法的：
bar(foo( ));
bar("hello world");
原因在于foo( )和"hello world"串都会产生一个临时对象，而在C++中，这些临时对象都是const类型的。因此上面的表达式就是试图将一个const类型的对象转换为非const类型，这是非法的。引用型参数应该在能被定义为const的情况下，尽量定义为const 。
12.将“引用”作为函数返回值类型的格式、好处和需要遵守的规则?
格式：类型标识符 &函数名（形参列表及类型说明）{ //函数体 }
好处：在内存中不产生被返回值的副本；（注意：正是因为这点原因，所以返回一个局部变量的引用是不可取的。因为随着该局部变量生存期的结束，相应的引用也会失效，产生runtime error! 
注意事项：
（1）不能返回局部变量的引用。这条可以参照Effective C++[1]的Item 31。主要原因是局部变量会在函数返回后被销毁，因此被返回的引用就成为了"无所指"的引用，程序会进入未知状态。
（2）不能返回函数内部new分配的内存的引用。这条可以参照Effective C++[1]的Item 31。虽然不存在局部变量的被动销毁问题，可对于这种情况（返回函数内部new分配内存的引用），又面临其它尴尬局面。例如，被函数返回的引用只是作为一个临时变量出现，而没有被赋予一个实际的变量，那么这个引用所指向的空间（由new分配）就无法释放，造成memory leak。
（3）可以返回类成员的引用，但最好是const。这条原则可以参照Effective C++[1]的Item 30。主要原因是当对象的属性是与某种业务规则（business rule）相关联的时候，其赋值常常与某些其它属性或者对象的状态有关，因此有必要将赋值操作封装在一个业务规则当中。如果其它对象可以获得该属性的非常量引用（或指针），那么对该属性的单纯赋值就会破坏业务规则的完整性。
（4）流操作符重载返回值申明为“引用”的作用：
流操作符<<和>>，这两个操作符常常希望被连续使用，例如：cout << "hello" << endl;　因此这两个操作符的返回值应该是一个仍然支持这两个操作符的流引用。可选的其它方案包括：返回一个流对象和返回一个流对象指针。但是对于返回一个流对象，程序必须重新（拷贝）构造一个新的流对象，也就是说，连续的两个<<操作符实际上是针对不同对象的！这无法让人接受。对于返回一个流指针则不能连续使用<<操作符。因此，返回一个流对象引用是惟一选择。这个唯一选择很关键，它说明了引用的重要性以及无可替代性，也许这就是C++语言中引入引用这个概念的原因吧。 赋值操作符=。这个操作符象流操作符一样，是可以连续使用的，例如：x = j = 10;或者(x=10)=100;赋值操作符的返回值必须是一个左值，以便可以被继续赋值。因此引用成了这个操作符的惟一返回值选择。
例3
＃i nclude <iostream.h>
int &put(int n);
int vals[10];
int error=-1;
void main()
{
put(0)=10; //以put(0)函数值作为左值，等价于vals[0]=10; 
put(9)=20; //以put(9)函数值作为左值，等价于vals[9]=20; 
cout<<vals[0]; 
cout<<vals[9];
} 
int &put(int n)
{
if (n>=0 && n<=9 ) return vals[n]; 
else { cout<<"subscript error"; return error; }
}
（5）在另外的一些操作符中，却千万不能返回引用：+-*/ 四则运算符。它们不能返回引用，Effective C++[1]的Item23详细的讨论了这个问题。主要原因是这四个操作符没有side effect，因此，它们必须构造一个对象作为返回值，可选的方案包括：返回一个对象、返回一个局部变量的引用，返回一个new分配的对象的引用、返回一个静态对象引用。根据前面提到的引用作为返回值的三个规则，第2、3两个方案都被否决了。静态对象的引用又因为((a+b) == (c+d))会永远为true而导致错误。所以可选的只剩下返回一个对象了。
13.“引用”与多态的关系？
引用是除指针外另一个可以产生多态效果的手段。这意味着，一个基类的引用可以指向它的派生类实例。
例4
Class A; Class B : Class A{...};  B b; A& ref = b;
14.“引用”与指针的区别是什么？
指针通过某个指针变量指向一个对象后，对它所指向的变量间接操作。程序中使用指针，程序的可读性差；而引用本身就是目标变量的别名，对引用的操作就是对目标变量的操作。此外，就是上面提到的对函数传ref和pointer的区别。
15.什么时候需要“引用”？
流操作符<<和>>、赋值操作符=的返回值、拷贝构造函数的参数、赋值操作符=的参数、其它情况都推荐使用引用。http://develop.csai.cn/c/NO0000021.htm
16.结构与联合有和区别？
(1). 结构和联合都是由多个不同的数据类型成员组成, 但在任何同一时刻,联合中只存放了一个被选中的成员（所有成员共用一块地址空间）, 而结构的所有成员都存在（不同成员的存放地址不同）。 
 (2). 对于联合的不同成员赋值, 将会对其它成员重写, 原来成员的值就不存在了, 而对于结构的不同成员赋值是互不影响的。
17.面关于“联合”的题目的输出？
a)
＃i nclude <stdio.h>
union
{
int i;
char x[2];
}a;
void main()
{
a.x[0] = 10; 
a.x[1] = 1;
printf("%d",a.i);
}
答案：int占4个字节，x占2个字节，内存对齐，总的大小为4个字节。先存10（即0A),再存1（即01），
内存占用情况是Ox 01 0A，i即为0x010A，换算结果为：1×16^2+10 = 266
 (低位低地址，高位高地址）
b)
main() 
     { 
          union{                   /*定义一个联合*/ 
               int i; 
               struct{             /*在联合中定义一个结构*/ 
                    char first; 
                    char second; 
               }half; 
          }number; 
          number.i=0x4241;         /*联合成员赋值*/ 
          printf("%c%cn", number.half.first, mumber.half.second); 
          number.half.first='a';   /*联合中结构成员赋值*/ 
          number.half.second='b'; 
          printf("%xn", number.i); 
          getch(); 
     } 
答案：先存half中的first，后存second，内存中的情况为：second - first（“-”以示区别）
  AB   (0x41对应'A',是低位；Ox42对应'B',是高位）
       6261 (number.i和number.half共用一块地址空间）
18.关联、聚合(Aggregation)以及组合(Composition)的区别？
涉及到UML中的一些概念：关联是表示两个类的一般性联系，比如“学生”和“老师”就是一种关联关系；聚合表示has-a的关系，是一种相对松散的关系，聚合类不需要对被聚合类负责，如下图所示，用空的菱形表示聚合关系：从实现的角度讲，聚合可以表示为:
class A {...}  class B { A* a; .....}
而组合表示contains-a的关系，关联性强于聚合：组合类与被组合类有相同的生命周期，组合类要对被组合类负责，采用实心的菱形表示组合关系：实现的形式是:
class A{...} class B{ A a; ...}
补充：Reference：http://blog.bandao.cn/archive/35755/blogs-679502.aspx
UML中的五种关系
<1>依赖
依赖关系用虚线加箭头表示,如图所示:

上图表示:Animal类依赖于Water类（动物依赖于水）。
 依赖是类的五种关系中耦合最小的一种关系。因为依赖关系在生成代码的时候，这两个关系类都不会增加属性。
这种微弱的关系可以用类之间的相互了解的程度来说明。(下图为代码生成图)
                       
由上图可见生成的代码中Animal类的属性中没有增加Water类。那么Animal类如何使用Water类呢，有三种方式：
依赖关系的三种表现形式：
①     Water类是全局的，则Animal类可以调用它
②     Water类是Animal类的某个方法中的变量，则Animal类可以调用它。代码演示如下：

  PS：Animal有一个长大（GrownUp）方法，Water类作为该方法的变量来使用。
          请注意Water类的生命期，它是当Animal类的GrounUp方法被调用的时候，才被实例化。
         持有Water类的是Animal的一个方法而不是Animal类，这点是最重要的。
③     Water类是作为Animal类中某个方法的参数或者返回值时。代码演示如下

无用多说，Water类被Animal类的一个方法持有。生命期随着方法的执行结束而结束。在依赖关系中，必须采用这三种方法之一。
<2>关联
关联是实线加箭头表示。表示类之间的耦合度比依赖要强。
例：水与气候是关联的，表示图如下

生成代码如下：

可见生成的代码中，Water类的属性中增加了Climate类。
关联既有单向关联又有双向关联。
单向关联：Water类和Climate类单向关联（如下图），则Water类称为源类，Climate类称为目标类。
源类了解目标类的所有的属性和方法，但目标类并不了解源类的信息。

双向关联：源类和目标类相互了解彼此的信息。如将Water类和Climate类之间改为双向关联，如下图


依赖和关联的区别：
①     从类的属性是否增加的角度看：
发生依赖关系的两个类都不会增加属性。其中的一个类作为另一个类的方法的参数或者返回值，或者是某个方法的变量而已。
发生关联关系的两个类，其中的一个类成为另一个类的属性，而属性是一种更为紧密的耦合，更为长久的持有关系。
②     从关系的生命期角度看：
依赖关系是仅当类的方法被调用时而产生，伴随着方法的结束而结束了。
关联关系是当类实例化的时候即产生，当类销毁的时候，关系结束。相比依赖讲，关联关系的生存期更长。

关联关系的细化
关联关系又可以细化为聚合关系和组合关系
聚合关系图：


组合关系图：

 

l       聚合和组合的区别：
由于聚合和组合都是特殊的关联关系，在生成的代码上看不出区别，都是关联的形式。那到底这两种关系如何来区分呢。
区分的关键有两点：
①     构造函数不同
聚合关系下：雁群类（GooseGroup）和大雁类（Goose）代码如下：


组合关系下：大雁类（Goose）和翅膀类（Wings）代码如下：


这两种关系的区别在于：
①构造函数不同
     聚合类的构造函数中包含了另一个类作为参数。
雁群类（GooseGroup）的构造函数中要用到大雁（Goose）作为参数传递进来。大雁类（Goose）可以脱离雁群类而独立存在。
     组合类的构造函数中包含了另一个类的实例化。
表明大雁类在实例化之前，一定要先实例化翅膀类（Wings），这两个类紧密的耦合在一起，同生共灭。翅膀类（Wings）是不可以脱离大雁类（Goose）而独立存在
②     信息的封装性不同
在聚合关系中，客户端可以同时了解雁群类和大雁类，因为他们都是独立的
而在组合关系中，客户端只认识大雁类，根本就不知道翅膀类的存在，因为翅膀类被严密的封装在大雁类中。
<3>泛化
泛化就是一个类继承另一个类所有的描述，并且可以根据需要对父类进行拓展，是面向对象的重要特征之一。
泛化使用一根实线加箭头，泛化关系图


泛化的用处：①实现了代码的复用
                        ②实现了多态
<4>实现
 主要针对接口和抽象类而言，实现接口和抽象类的类必须要实现他们的方法。
实现关系表示为：虚线加箭头，关系图如下：


接口只包含方法、委托或事件的签名。方法的实现是在实现接口的类中完成的。
19.面向对象的三个基本特征，并简单叙述之？
1. 封装：将客观事物抽象成类，每个类对自身的数据和方法实行protection(private, protected,public)
2. 继承：广义的继承有三种实现形式：实现继承（指使用基类的属性和方法而无需额外编码的能力）、可视继承（子窗体使用父窗体的外观和实现代码）、接口继承（仅使用属性和方法，实现滞后到子类实现）。前两种（类继承）和后一种（对象组合=>接口继承以及纯虚函数）构成了功能复用的两种方式。
3. 多态：是将父对象设置成为和一个或更多的他的子对象相等的技术，赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。简单的说，就是一句话：允许将子类类型的指针赋值给父类类型的指针。
20.重载（overload)和重写(overried，有的书也叫做“覆盖”）的区别？
常考的题目。从定义上来说：
重载：是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。
重写：是指子类重新定义父类虚函数的方法。
从实现原理上来说：
重载：编译器根据函数不同的参数表，对同名函数的名称做修饰，然后这些同名函数就成了不同的函数（至少对于编译器来说是这样的）。如，有两个同名函数：function func(p:integer):integer;和function func(p:string):integer;。那么编译器做过修饰后的函数名称可能是这样的：int_func、str_func。对于这两个函数的调用，在编译器间就已经确定了，是静态的。也就是说，它们的地址在编译期就绑定了（早绑定），因此，重载和多态无关！
重写：和多态真正相关。当子类重新定义了父类的虚函数后，父类指针根据赋给它的不同的子类指针，动态的调用属于子类的该函数，这样的函数调用在编译期间是无法确定的（调用的子类的虚函数的地址无法给出）。因此，这样的函数地址是在运行期绑定的（晚绑定）
21.多态的作用？
主要是两个：
1. 隐藏实现细节，使得代码能够模块化；扩展代码模块，实现代码重用；
2. 接口重用：为了类在继承和派生的时候，保证使用家族中任一类的实例的某一属性时的正确调用。
22.Ado与Ado.net的相同与不同？
除了“能够让应用程序处理存储于DBMS 中的数据“这一基本相似点外，两者没有太多共同之处。但是Ado使用OLE DB 接口并基于微软的COM 技术，而ADO.NET 拥有自己的ADO.Net 接口并且基于微软的.NET 体系架构。众所周知.NET 体系不同于COM 体系，ADO.NET 接口也就完全不同于ADO和OLE DB 接口，这也就是说ADO.NET 和ADO是两种数据访问方式。ADO.net 提供对XML 的支持
Reference：http://sunxitao88.blog.163.com/blog/static/68314439200882272036951/
23.New delete 与malloc free 的联系与区别?
答案：都是在堆(heap)上进行动态的内存操作。用malloc函数需要指定内存分配的字节数并且不能初始化对象，new 会自动调用对象的构造函数。delete 会调用对象的destructor，而free 不会调用对象的destructor.
24.#define DOUBLE(x) x+x ，i = 5*DOUBLE(5)； i 是多少？
答案：i 为30。如果是#define double(x) (x+x), i = 5*double(5);结果就是50
25.有哪几种情况只能用intialization list（初始化列表） 而不能用assignment?
答案：当类中含有const、reference 成员变量；基类的构造函数都需要初始化表。
26. C++是不是类型安全的？
答案：不是。两个不同类型的指针之间可以强制转换（用reinterpret cast)。C#是类型安全的。
27. main 函数执行以前，还会执行什么代码？
答案：全局对象的构造函数会在main 函数之前执行。
28. 描述内存分配方式以及它们的区别?
1） 从静态存储区域分配。内存在程序编译的时候就已经分配好，这块内存在程序的整个运行期间都存在。例如全局变量，static 变量。
2） 在栈上创建。在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束时这些存储单元自动被释放。栈内存分配运算内置于处理器的指令集。
3） 从堆上分配，亦称动态内存分配。程序在运行的时候用malloc 或new 申请任意多少的内存，程序员自己负责在何时用free 或delete 释放内存。动态内存的生存期由程序员决定，使用非常灵活，但问题也最多。
29.struct 和 class 的区别
答案：
struct 的成员默认是公有的，而类的成员默认是私有的。
struct 和 class 在其他方面是功能相当的。从感情上讲，大多数的开发者感到类和结构有很大的差别。感觉上结构仅仅象一堆缺乏封装和功能的开放的内存位，而类就象活的并且可靠的社会成员，它有智能服务，有牢固的封装屏障和一个良好定义的接口。既然大多数人都这么认为，那么只有在你的类有很少的方法并且有公有数据（这种事情在良好设计的系统中是存在的!）时，你也许应该使用 struct 关键字，否则，你应该使用 class 关键字
30.当一个类A 中没有任何成员变量与成员函数,这时sizeof(A)的值是多少？
答案：如果不是零，请解释一下编译器为什么没有让它为零。（Autodesk）肯定不是零。举个反例，如果是零的话，声明一个class A[10]对象数组，而每一个对象占用的空间是零，这时就没办法区分A[0],A[1]…了。
理论上不是0，跟编辑器有关，windows下vc是1，给他1个字节的地址，以区别于其他的类，不然实例化的时候找不到地址。
31. 在8086 汇编下，逻辑地址和物理地址是怎样转换的？（Intel）
答案：通用寄存器给出的地址，是段内偏移地址，相应段寄存器地址*10H+通用寄存器内地址，就得到了真正要访问的地址。
32. 比较C++中的4种类型转换方式？
重点是static_cast, dynamic_cast和reinterpret_cast的区别和应用。
dynamic_casts在帮助你浏览继承层次上是有限制的。它不能被用于缺乏虚函数的类型上，它被用于安全地沿着类的继承关系向下进行类型转换。如你想在没有继承关系的类型中进行转换，你可能想到static_cast。（可参考：http://blog.csdn.net/allen_fan_01/article/details/8993075）
33.分别写出BOOL,int,float,指针类型的变量a 与“零”的比较语句。
答案：
BOOL :    if ( !a ) or if(a)
int :     if ( a == 0)
float :   const EXPRESSION EXP = 0.000001
          if ( a < EXP && a >-EXP)
pointer : if ( a != NULL) or if(a == NULL)
注：
它明确了告诉编码者，说EXP是一个精度类型。这个精度类型是通过
typedef float EXPRESSION;定义的。
const EXPRESSION EXP = 0.000001 这是定义一个精度，这个根据类型能达到的精度或应用需求的精度而定，小数位多精度高。
34.请说出const与#define 相比，有何优点？
Const作用：定义常量、修饰函数参数、修饰函数返回值三个作用。
被Const修饰的东西都受到强制保护，可以预防意外的变动，能提高程序的健壮性。
1） const 常量有数据类型，而宏常量没有数据类型。编译器可以对前者进行类型安全检查。而对后者只进行字符替换，没有类型安全检查，并且在字符替换可能会产生意料不到的错误。
2） 有些集成化的调试工具可以对const 常量进行调试，但是不能对宏常量进行调试。
35.简述数组与指针的区别？
数组要么在静态存储区被创建（如全局数组），要么在栈上被创建。指针可以随时指向任意类型的内存块。
(1)修改内容上的差别
char a[] = “hello”;
a[0] = ‘X’;
char *p = “world”; // 注意p 指向常量字符串
p[0] = ‘X’; // 编译器不能发现该错误，运行时错误
(2) 用运算符sizeof 可以计算出数组的容量（字节数）。sizeof(p),p 为指针得到的是一个指针变量的字节数，而不是p 所指的内存容量。C++/C 语言没有办法知道指针所指的内存容量，除非在申请内存时记住它。注意当数组作为函数的参数进行传递时，该数组自动退化为同类型的指针。
char a[] = "hello world";
char *p = a;
cout<< sizeof(a) << endl; // 12 字节
cout<< sizeof(p) << endl; // 4 字节
计算数组和指针的内存容量
void Func(char a[100])
{
cout<< sizeof(a) << endl; // 4 字节而不是100 字节
}
36.类成员函数的重载、覆盖和隐藏区别？
a.成员函数被重载的特征：
（1）相同的范围（在同一个类中）；
（2）函数名字相同；
（3）参数不同；
（4）virtual 关键字可有可无。
b.覆盖是指派生类函数覆盖基类函数，特征是：
（1）不同的范围（分别位于派生类与基类）；
（2）函数名字相同；
（3）参数相同；
（4）基类函数必须有virtual 关键字。
 c.“隐藏”是指派生类的函数屏蔽了与其同名的基类函数，规则如下：
（1）如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无virtual关键字，基类的函数将被隐藏（注意别与重载混淆）。
（2）如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual 关键字。此时，基类的函数被隐藏（注意别与覆盖混淆）
（覆盖与隐藏：覆盖要求都相同，有virtual；隐藏要求都相同，但无virtual，派生类始终不同与基类。）
37.求出两个数中的较大者
There are two int variables: a and b, don’t use “if”, “? :”, “switch”or other judgement statements, find out the biggest one of the two numbers.
答案：( ( a + b ) + abs( a - b ) ) / 2
如：a = 5 ,b  = 3 ，则：（（5+3）+ （5-3）） / 2 = 5；反过来，因为有绝对值，所以结果一样。
38.如何打印出当前源文件的文件名以及源文件的当前行号？
答案：
cout << __FILE__ ;
cout << __LINE__ ;
__FILE__和__LINE__是系统预定义宏，这种宏并不是在某个文件中定义的，而是由编译器定义的。
39. main 主函数执行完毕后，是否可能会再执行一段代码，给出说明？
答案：可以，可以用_onexit 注册一个函数，它会在main 之后执行int fn1(void), fn2(void), fn3(void), fn4 (void);
void main( void )
{
String str("zhanglin");
_onexit( fn1 );
_onexit( fn2 );
_onexit( fn3 );
_onexit( fn4 );
printf( "This is executed first.\n" );
}
int fn1()
{
printf( "next.n" );
return 0;
}
int fn2()
{
printf( "executed " );
return 0;
}
int fn3()
{
printf( "is " );
return 0;
}
int fn4()
{
printf( "This " );
return 0;
}
The _onexit function is passed the address of a function (func) to be called when the program terminates normally. Successive calls to _onexit create a register of functions that are executed in LIFO (last-in-first-out) order. The functions passed to _onexit cannot take parameters.
Reference：http://technet.microsoft.com/zh-cn/library/aa298513
40.如何判断一段程序是由C 编译程序还是由C++编译程序编译的？
答案：
#ifdef __cplusplus
cout<<"c++";
#else
cout<<"c";
#endif
41.文件中有一组整数，要求排序后输出到另一个文件中
#include<iostream>  
#include<fstream>  
using namespace std;  
void Order(vector<int>& data) //bubble sort  
{  
int count = data.size() ;  
for ( int i = 0 ; i < count ; i++)  
{  
for ( int j = 0 ; j < count - i - 1 ; j++)  
{  
if ( data[j] > data[j+1])  
{  
int temp = data[j] ;  
data[j] = data[j+1] ;  
data[j+1] = temp ;  
}  
}  
}  
void main( void )  
{  
vector<int>data;  
ifstream in("c:\data.txt");  
if ( !in)  
{  
cout<<"file error!";  
exit(1);  
}  
int temp;  
while (!in.eof())  
{  
in>>temp;  
data.push_back(temp);  
}  
in.close(); //关闭输入文件流  
Order(data);  
ofstream out("c:\result.txt");  
if ( !out)  
{  
cout<<"file error!";  
exit(1);  
}  
for ( i = 0 ; i < data.size() ; i++)  
out<<data[i]<<" ";  
out.close(); //关闭输出文件流  
}  
42.链表题：一个链表的结点结构
struct Node  
{  
int data ;  
Node *next ;  
};  
typedef struct Node Node ;  
(1)已知链表的头结点head,写一个函数把这个链表逆序 ( Intel)
Node * ReverseList(Node *head) //链表逆序  
{  
if ( head == NULL || head->next == NULL )  
return head;  
Node *p1 = head ;  
Node *p2 = p1->next ;  
Node *p3 = p2->next ;  
p1->next = NULL ;  
while ( p3 != NULL )  
{  
p2->next = p1 ;  
p1 = p2 ;  
p2 = p3 ;  
p3 = p3->next ;  
}  
p2->next = p1 ;  
head = p2 ;  
return head ;  
}  
(2)已知两个链表head1 和head2 各自有序，请把它们合并成一个链表依然有序。(保留所有结点，即便大小相同）
Node * Merge(Node *head1 , Node *head2)  
{  
if ( head1 == NULL)  
return head2 ;  
if ( head2 == NULL)  
return head1 ;  
Node *head = NULL ;  
Node *p1 = NULL;  
Node *p2 = NULL;  
if ( head1->data < head2->data )  
{  
head = head1 ;  
p1 = head1->next;  
p2 = head2 ;  
}  
else  
{  
head = head2 ;  
p2 = head2->next ;  
p1 = head1 ;  
}  
Node *pcurrent = head ;  
while ( p1 != NULL && p2 != NULL)  
{  
if ( p1->data <= p2->data )  
{  
pcurrent->next = p1 ;  
pcurrent = p1 ;  
p1 = p1->next ;  
}  
else  
{  
pcurrent->next = p2 ;  
pcurrent = p2 ;  
p2 = p2->next ;  
}  
}  
if ( p1 != NULL )  
pcurrent->next = p1 ;  
if ( p2 != NULL )  
pcurrent->next = p2 ;  
return head ;  
}  
(3)已知两个链表head1 和head2 各自有序，请把它们合并成一个链表依然有序，这次要求用递归方法进行。 (Autodesk)
Node * MergeRecursive(Node *head1 , Node *head2)  
{  
if ( head1 == NULL )  
return head2 ;  
if ( head2 == NULL)  
return head1 ;  
Node *head = NULL ;  
if ( head1->data < head2->data )  
{  
head = head1 ;  
head->next = MergeRecursive(head1->next,head2);  
}  
else  
{  
head = head2 ;  
head->next = MergeRecursive(head1,head2->next);  
}  
return head ;  
43.写一个函数找出一个整数数组中，第二大的数 （microsoft）
思路：取两个参数变量，一个保留最大值maxnumber，一个保留第二大值sec_max。
每次用data[i]与maxnumber比，如果maxnumber小，sec_max = maxnumber ; maxnumber = data[i] ;
否则，比较data[i]是否比sec_max大，如果是，则sec_max = data[i] ；
const int MINNUMBER = -32767 ;  
int find_sec_max( int data[] , int count)  
{  
int maxnumber = data[0] ;  
int sec_max = MINNUMBER ;  
for ( int i = 1 ; i < count ; i++)  
{  
if ( data[i] > maxnumber )  
{  
sec_max = maxnumber ;  
maxnumber = data[i] ;  
}  
else  
{  
if ( data[i] > sec_max )  
sec_max = data[i] ;  
}  
}  
return sec_max ;  
}  
44.写一个在一个字符串(n)中寻找一个子串(m)第一个位置的函数。
KMP算法效率最好，时间复杂度是Ｏ(n+m),
45. 分析一下这段程序的输出 (Autodesk)
class B
{
public:
B()
{
cout<<"default constructor"<<endl;
}
~B()
{
cout<<"destructed"<<endl;
}
B(int i):data(i)    //B(int) works as a converter ( int -> instance of  B)
{
cout<<"constructed by parameter " << data <<endl;
}
private:
int data;
};

B Play( B b) 
{
return b ;
}
(1)                                                             results:
int main(int argc, char* argv[])                            constructed by parameter 5
{                                                                         destructed  B(5)形参析构
B t1 = Play(5); B t2 = Play(t1);   　                     destructed  t1形参析构
return 0;　　　　　　　　　　　　　　            destructed  t2　注意顺序！
}                                                                         destructed  t1
(2)                                                             results:
int main(int argc, char* argv[])                            constructed by parameter 5
{                                                                          destructed  B(5)形参析构
B t1 = Play(5); B t2 = Play(10);   　                     constructed by parameter 10
return 0;　　　　　　　　　　　　　　             destructed  B(10)形参析构
}                                                                          destructed  t2　注意顺序！
                                                                           destructed  t1
46.多重继承的内存分配问题
比如有class A : public class B, public class C {} 那么A的内存结构大致是怎么样的？
这个是compiler-dependent的, 不同的实现其细节可能不同。如果不考虑有虚函数、虚继承的话就相当简单；否则的话，相当复杂。可以参考《深入探索C++对象模型》Reference：http://blog.csdn.net/allen_fan_01/article/details/8991603
47.如何判断一个单链表是有环的？（注意不能用标志位，最多只能用两个额外指针）
struct node { char val; node* next;}  
bool check(const node* head) {} //return false : 无环；true: 有环一种O（n）的办法就是（搞两个指针，一个每次递增一步，一个每次递增两步，如果有环的话两者必然重合，反之亦然）：  
bool check(const node* head)  
{  
    if(head==NULL)  return false;  
    node *low=head, *fast=head->next;  
    while(fast!=NULL && fast->next!=NULL)  
    {  
        low=low->next;  
        fast=fast->next->next;  
        if(low==fast) return true;  
    }  
    return false;  
}  
48.指针找错题
分析这些面试题，本身包含很强的趣味性;而作为一名研发人员，通过对这些面试题的深入剖析则可进一步增强自身的内功。
　　试题1：
以下是引用片段：
void test1()  //数组越界
　　{
　　char string[10];
　　char* str1 = "0123456789";
　　strcpy( string, str1 );
　　}
　　试题2：　
以下是引用片段：
　void test2()
　　{
　　char string[10], str1[10]; 
　　int i;
　　for(i=0; i<10; i++)
　　{
　　str1= 'a';
　　}
　　strcpy( string, str1 );
　　}
　　试题3：　　
以下是引用片段：
void test3(char* str1)
　　{
　　char string[10];
　　if( strlen( str1 ) <= 10 )
　　{
　　strcpy( string, str1 );
　　}
　　}
　　解答：
试题1：字符串str1需要11个字节才能存放下(包括末尾的’\0’)，而string只有10个字节的空间，strcpy会导致数组越界;
试题2：如果面试者指出字符数组str1不能在数组内结束可以给3分；
            如果面试者指出strcpy(string,str1)调用使得从str1内存起复制到string内存起所复制的字节数具有不确定性可以给7分；
            在此基础上指出库函数strcpy工作方式的给10分;
试题3：if(strlen(str1) <= 10)应改为if(strlen(str1) <10)，因为strlen的结果未统计’\0’所占用的1个字节。剖析：考查对基本功的掌握
　　(1)字符串以’\0’结尾;
　　(2)对数组越界把握的敏感度;
　　(3)库函数strcpy的工作方式，
49.如果编写一个标准strcpy函数
总分值为10，下面给出几个不同得分的答案：
2分 以下是引用片段：
void strcpy( char *strDest, char *strSrc )
　　{
　　while( (*strDest++ = * strSrc++) != ‘\0’ );
　　}
　　4分　以下是引用片段：
　void strcpy( char *strDest, const char *strSrc )
　　//将源字符串加const，表明其为输入参数，加2分
　　{
　　while( (*strDest++ = * strSrc++) != ‘\0’ );
　　}
　　7分 以下是引用片段：
void strcpy(char *strDest, const char *strSrc)
　　{
　　//对源地址和目的地址加非0断言，加3分
　　assert( (strDest != NULL) &&(strSrc != NULL) );
　　while( (*strDest++ = * strSrc++) != ‘\0’ );
　　}
　　10分 以下是引用片段：
//为了实现链式操作，将目的地址返回，加3分!
　　char * strcpy( char *strDest, const char *strSrc )
　　{
　　assert( (strDest != NULL) &&(strSrc != NULL) ); 
　　char *address = strDest;
　　while( (*strDest++ = * strSrc++) != ‘\0’ );
　　return address;
　　}
　　从2分到10分的几个答案我们可以清楚的看到，小小的strcpy竟然暗藏着这么多玄机，真不是盖的!
需要多么扎实的基本功才能写一个完美的strcpy啊!
>>> 对strlen的掌握，它没有包括字符串末尾的'\0'。
>>> 　　读者看了不同分值的strcpy版本，应该也可以写出一个10分的strlen函数了，完美的版本为： 
>>> int strlen( const char *str ) //输入参数const　以下是引用片段：
>>> 　{
>>> 　　assert( strt != NULL ); //断言字符串地址非0
>>> 　　int len=0; //注，一定要初始化。
>>> 　　while( (*str++) != '\0' )
>>> 　　{
>>> 　　len++;
>>> 　　}
>>> 　　return len;
>>> 　　}

试题4：以下是引用片段：
void GetMemory( char *p )
　　{
　　p = (char *) malloc( 100 );
　　}
　　void Test( void )
　　{
　　char *str = NULL;
　　GetMemory( str );
　　strcpy( str, "hello world" );
　　printf( str );
　　} 
试题5：　以下是引用片段：
char *GetMemory( void )
　　{
　　char p[] = "hello world";
　　return p;
　　}
　　void Test( void )
　　{
　　char *str = NULL;
　　str = GetMemory();
　　printf( str );
　　}
试题6：以下是引用片段：
void GetMemory( char **p, int num )
　　{
　　*p = (char *) malloc( num );
　　}
　　void Test( void )
　　{
　　char *str = NULL;
　　GetMemory( &str, 100 );
　　strcpy( str, "hello" );
　　printf( str );
　　}
试题7：以下是引用片段：
　void Test( void )
　　{
　　char *str = (char *) malloc( 100 );
　　strcpy( str, "hello" );
　　free( str );
　　... //省略的其它语句
　　}
解答：
试题4
传入中GetMemory( char *p )函数的形参为字符串指针，在函数内部修改形参并不能真正的改变传入形参的值，执行完
　　char *str = NULL; 
　　GetMemory( str ); 
　　后的str仍然为NULL;
试题5
　　char p[] = "hello world";
　　return p;
　　p[]数组为函数内的局部自动变量，在函数返回后，内存已经被释放。这是许多程序员常犯的错误，其根源在于不理解变量的生存期。
试题6
GetMemory避免了试题4的问题，传入GetMemory的参数为字符串指针的指针，但是在GetMemory中执行申请内存及赋值语句 tiffanybracelets
　　*p = (char *) malloc( num );
　　后未判断内存是否申请成功，应加上：
　　if ( *p == NULL )
　　{
　　...//进行申请内存失败处理
　　}
试题7
存在与试题6同样的问题，在执行
　　char *str = (char *) malloc(100);
　　后未进行内存是否申请成功的判断;另外，在free(str)后未置str为空，导致可能变成一个“野”指针，应加上：
　　str = NULL;
　　试题6的Test函数中也未对malloc的内存进行释放。
　　剖析：
　　试题4～7考查面试者对内存操作的理解程度，基本功扎实的面试者一般都能正确的回答其中50~60的错误。但是要完全解答正确，却也绝非易事。
对内存操作的考查主要集中在：
(1)指针的理解;
(2)变量的生存期及作用范围;
(3)良好的动态内存申请和释放习惯。
　　再看看下面的一段程序有什么错误：　　
以下是引用片段：
swap( int* p1,int* p2 )
　　{
　　int *p;
　　*p = *p1;
　　*p1 = *p2;
　　*p2 = *p;
　　}
在swap函数中，p是一个“野”指针，有可能指向系统区，导致程序运行的崩溃。
在VC++中DEBUG运行时提示错误“Access Violation”。该程序应该改为以下是引用片段：
swap( int* p1,int* p2 )
　　{
　　int p;
　　p = *p1;
　　*p1 = *p2;
　　*p2 = p;
　　}
50.String 的具体实现
已知String类定义如下：
class String
{
public:
String(const char *str = NULL); // 通用构造函数
String(const String &another); // 拷贝构造函数
~ String(); // 析构函数
String & operater =(const String &rhs); // 赋值函数
private:
char *m_data; // 用于保存字符串
};

尝试写出类的成员函数实现。
/通用构造函数  
String::String(const char *str)  
{  
if ( str == NULL ) //strlen在参数为NULL时会抛异常才会有这步判断  
{  
m_data = new char[1] ;  
m_data[0] = '\0' ;  
}  
else  
{  
m_data = new char[strlen(str) + 1];  
strcpy(m_data,str);  
}  
}   
//拷贝构造函数  
String::String(const String &another)  
{  
m_data = new char[strlen(another.m_data) + 1];  
strcpy(m_data,other.m_data);  
}  
//赋值操作符重载  
String& String::operator =(const String &rhs)  
{  
if ( this == &rhs)  
    return *this ;  
delete []m_data; //删除原来的数据，新开一块内存  
m_data = new char[strlen(rhs.m_data) + 1];  
strcpy(m_data,rhs.m_data);  
return *this ;  
}  
//析构函数  
String::~String()  
{  
delete []m_data ;  
}  
51.h头文件中的ifndef/define/endif 的作用？
答：防止该头文件被重复引用。
52.#include<file.h> 与 #include "file.h"的区别？
答：前者是从Standard Library的路径寻找和引用file.h，而后者是从当前工作路径搜寻并引用file.h。
53.在C++ 程序中调用被C 编译器编译后的函数，为什么要加extern “C”？
C++语言支持函数重载，C语言不支持函数重载。C++提供了C连接交换指定符号extern “C”，解决名字匹配问题。
首先，extern是C/C++语言中表明函数和全局变量作用范围（可见性）的关键字，该关键字告诉编译器，其声明的函数和变量可以在本模块或其它模块中使用。
通常，在模块的头文件中对本模块提供给其它模块引用的函数和全局变量以关键字extern声明。例如，如果模块B欲引用该模块A中定义的全局变量和函数时只需包含模块A的头文件即可。这样，模块B中调用模块A中的函数时，在编译阶段，模块B虽然找不到该函数，但是并不会报错；它会在连接阶段中从模块A编译生成的目标代码中找到此函数。
extern "C"是连接申明(linkage declaration),被extern "C"修饰的变量和函数是按照C语言方式编译和连接的,来看看C++中对类似C的函数是怎样编译的：作为一种面向对象的语言，C++支持函数重载，而过程式语言C则不支持。函数被C++编译后在符号库中的名字与C语言的不同。
例如，假设某个函数的原型为：void foo( int x, int y );
该函数被C编译器编译后在符号库中的名字为_foo，而C++编译器则会产生像_foo_int_int之类的名字（不同的编译器可能生成的名字不同，但是都采用了相同的机制，生成的新名字称为“mangled name”）。
_foo_int_int 这样的名字包含了函数名、函数参数数量及类型信息，C++就是靠这种机制来实现函数重载的。例如，在C++中，函数void foo( int x, int y )与void foo( int x, float y )编译生成的符号是不相同的，后者为_foo_int_float。
同样地，C++中的变量除支持局部变量外，还支持类成员变量和全局变量。用户所编写程序的类成员变量可能与全局变量同名，我们以"."来区分。而本质上，编译器在进行编译时，与函数的处理相似，也为类中的变量取了一个独一无二的名字，这个名字与用户程序中同名的全局变量名字不同。
未加extern "C"声明时的连接方式
假设在C++中，模块A的头文件如下：
// 模块A头文件　moduleA.h
#ifndef MODULE_A_H
#define MODULE_A_H
int foo( int x, int y );
#endif　　
在模块B中引用该函数：
// 模块B实现文件　moduleB.cpp
＃include "moduleA.h"
foo(2,3);
加extern "C"声明后的编译和连接方式
加extern "C"声明后，模块A的头文件变为：
// 模块A头文件　moduleA.h
#ifndef MODULE_A_H
#define MODULE_A_H
extern "C" int foo( int x, int y );
#endif　　
在模块B的实现文件中仍然调用foo( 2,3 )，其结果是：
（1）模块A编译生成foo的目标代码时，没有对其名字进行特殊处理，采用了C语言的方式；
（2）连接器在为模块B的目标代码寻找foo(2,3)调用时，寻找的是未经修改的符号名_foo。
如果在模块A中函数声明了foo为extern "C"类型，而模块B中包含的是extern int foo( int x, int y ) ，则模块B找不到模块A中的函数；反之亦然。
所以，可以用一句话概括extern “C”这个声明的真实目的（任何语言中的任何语法特性的诞生都不是随意而为的，来源于真实世界的需求驱动。我们在思考问题时，不能只停留在这个语言是怎么做的，还要问一问它为什么要这么做，动机是什么，这样我们可以更深入地理解许多问题）：实现C++与C及其它语言的混合编程。　　

明白了C++中extern "C"的设立动机，我们下面来具体分析extern "C"通常的使用技巧：
extern "C"的惯用法
（1）在C++中引用C语言中的函数和变量，在包含C语言头文件（假设为cExample.h）时，需进行下列处理：
extern "C"
{
＃include "cExample.h"
}
而在C语言的头文件中，对其外部函数只能指定为extern类型，C语言中不支持extern "C"声明，在.c文件中包含了extern "C"时会出现编译语法错误。

C++引用C函数例子工程中包含的三个文件的源代码如下：
/* c语言头文件：cExample.h */
#ifndef C_EXAMPLE_H
#define C_EXAMPLE_H
extern int add(int x,int y);
#endif

/* c语言实现文件：cExample.c */
＃include "cExample.h"
int add( int x, int y )
{
return x + y;
}

// c++实现文件，调用add：cppFile.cpp
extern "C"
{
＃include "cExample.h"
}
int main(int argc, char* argv[])
{
add(2,3);
return 0;
}
如果C++调用一个C语言编写的.DLL时，当包括.DLL的头文件或声明接口函数时，应加extern "C" {　}。
（2）在C中引用C++语言中的函数和变量时，C++的头文件需添加extern "C"，但是在C语言中不能直接引用声明了extern "C"的该头文件，应该仅将C文件中将C++中定义的extern "C"函数声明为extern类型。

C引用C++函数例子工程中包含的三个文件的源代码如下：
//C++头文件 cppExample.h
#ifndef CPP_EXAMPLE_H
#define CPP_EXAMPLE_H
extern "C" int add( int x, int y );
#endif

//C++实现文件 cppExample.cpp
＃include "cppExample.h"
int add( int x, int y )
{
return x + y;
}

//C实现文件 cFile.c
＃include "cExample.h" 
extern int add(int x, int y);
int main( int argc, char* argv[] )
{
add( 2, 3 );
return 0;
}
请参考《C++中extern “C”含义深层探索》注解：

几道c笔试题(含参考答案)
1. 
What is displayed when f() is called given the code:
class Number {
public:
string type; 

Number(): type(“void”) { }
explicit Number(short) : type(“short”) { } 
Number(int) : type(“int”) { }
};
void Show(const Number& n) { cout << n.type; }
void f()
{
short s = 42;
Show(s); 
}
a) void
b) short
c) int
d) None of the above
选C；虽然传入的是short类型，但是short类型的构造函数被生命被explicit，也就是只能显示类型转换，不能使用隐式类型转换。
关键字explicit
可以阻止不应该允许的经过转换构造函数进行的隐式转换的发生。声明为explicit的构造函数不能在隐式转换中使用。
explicit构造函数是用来防止隐式转换的，必须显示。

2. Which is the correct output for the following code
double dArray[2] = {4, 8}, *p, *q;
p = &dArray[0];
q = p + 1;
cout << q – p << endl; 
cout << (int)q - (int)p << endl;
a) 1 and 8
b) 8 and 4
c) 4 and 8
d) 8 and 1
选A；
第一个是指针加减，按照的是指向地址类型的加减，只跟类型位置有关，q和p指向的数据类型以实际数据类型来算差一个位置，因此是1。
而第二个加减是实际指针值的加减（即p和q的地址由double转换成了int，变成了数值，相减为8），在内存中一个double类型占据8个字节，因此是8。
54.Sony笔试题
　　1．完成下列程序 
　　* 
　　*.*. 
　　　　*..*..*.. 
　　　　*...*...*...*... 
　　　　*....*....*....*....*.... 
　　　　*.....*.....*.....*.....*.....*..... 
　　　　*......*......*......*......*......*......*...... 
　　　　*.......*.......*.......*.......*.......*.......*.......*....... 
　　　　#include 
　　　　#define N 8 
　　　　int main() 
　　　　{ 
　　　　 int i; 
　　　　 int j; 
　　　　 int k; 
---------------------------------------------------------
　　 | | 
　　 | | 
　　 | | 
---------------------------------------------------------
　　 return 0; 
　　} 
　　2．完成程序，实现对数组的降序排序 
　　#include 
　　void sort( ); 
　　int main() 
　　{ 
　　 int array[]={45，56，76，234，1，34，23，2，3}； //数字任//意给出 
　　 sort( ); 
　　 return 0; 
　　} 
　　void sort( ) 
　　{ 
____________________________________
　　 | | 
　　 | | 
　　 |-----------------------------------------------------| 
　　} 
　　3．费波那其数列，1，1，2，3，5……编写程序求第十项。可以用递归，也可以用其 
他方法，但要说明你选择的理由。 
　　#include 
　　int Pheponatch(int); 
　　int main() 
　　{ 
　　 printf("The 10th is %d",Pheponatch(10)); 
　　 return 0; 
　　} 
　　int Pheponatch(int N) 
　　{ 
--------------------------------
　　| | 
　　| | 
--------------------------------
　　} 
　　4．下列程序运行时会崩溃，请找出错误并改正，并且说明原因。 
　　#include 
　　#include 
　　typedef struct{ 
　　 TNode* left; 
　　 TNode* right; 
　　 int value; 
　　} TNode; 
　　TNode* root=NULL; 
　　void append(int N); 
　　int main() 
　　{ 
　　 append(63); 
　　 append(45); 
　　 append(32); 
　　 append(77); 
　　 append(96); 
　　 append(21); 
　　 append(17); // Again, 数字任意给出 
　　} 
　　void append(int N) 
　　{ 
　　 TNode* NewNode=(TNode *)malloc(sizeof(TNode)); 
　　 NewNode->value=N; 


　　 if(root==NULL) 
　　 { 
　　 root=NewNode; 
　　 return; 
　　 } 
　　 else 
　　 { 
　　 TNode* temp; 
　　 temp=root; 

　　 while((N>=temp.value && temp.left!=NULL) || (N !=NULL 
　　)) 
　　 { 
　　 while(N>=temp.value && temp.left!=NULL) 
　　 temp=temp.left; 
　　 while(N 　　 temp=temp.right; 
　　 } 
　　 if(N>=temp.value) 
　　 temp.left=NewNode; 
　　 else 
　　 temp.right=NewNode; 
　　 return; 
　　 } 
　　} 
55请你分别画出OSI的七层网络结构图和TCP/IP的五层结构图。
应用层：为应用程序提供服务
表示层：处理在两个通信系统中交换信息的表示方式
会话层：负责维护两个结点间会话连接的建立、管理和终止，以及数据交换
传输层：向用户提供可靠的端到端服务。UDP TCP协议。
网络层：通过路由选择算法为分组通过通信子网选择最适当的路径，以及实现拥塞控制、网络互联等功能。数据传输单元是分组。IP地址，路由器，IP协议。
数据链路层：在物理层提供的服务基础上，数据链路层在通信的实体间建立数据链路连接，传输一帧为单位的数据包（，并采用差错控制与流量控制方法，使有差错的物理线路变成无差错的数据链路。）
物理层：传输比特流。传输单元是比特。调制解调器。
56请你详细地解释一下IP协议的定义，在哪个层上面？主要有什么作用？TCP与UDP呢 ？
网络层。
57.请问交换机和路由器各自的实现原理是什么？分别在哪个层次上面实现的？
交换机：数据链路层。路由器：网络层。
58.全局变量和局部变量有什么区别？是怎么实现的？操作系统和编译器是怎么知道的 ？
 全局变量的生命周期是整个程序运行的时间，而局部变量的生命周期则是局部函数或过程调用的时间段。
其实现是由编译器在编译时采用不同内存分配方法。全局变量在main函数调用后，就开始分配，如果是静态变量则是在main函数前就已经初始化了。而局部变量则是在用户栈中动态分配的（还是建议看编译原理中的活动记录这一块）
59.8086是多少位的系统？在数据总线上是怎么实现的？ 
8086微处理器共有4个16位的段寄存器，在寻址内存单元时，用它们直接或间接地存放段地址。
　　代码段寄存器CS：存放当前执行的程序的段地址。
　　数据段寄存器DS：存放当前执行的程序所用操作数的段地址。
　　堆栈段寄存器SS：存放当前执行的程序所用堆栈的段地址。
　　附加段寄存器ES：存放当前执行程序中一个辅助数据段的段地址。
由cs:ip构成指令地址，ss:sp构成堆栈的栈顶地址指针。DS和ES用作数据段和附加段的段地址（段起始地址或段值）

8086／8088微处理器的存储器管理
   1.地址线（码）与寻址范围：N条地址线     寻址范围=2^N
   2.8086有20地址线     寻址范围为1MB  由 00000H～FFFFFH
   3. 8086微处理器是一个16位结构，用户可用的寄存器均为16位：寻址64KB
   4. 8086／8088采用分段的方法对存储器进行管理。具体做法是：把1MB的存储器空间分成若干段，每段容量为64KB，每段存储器的起始地址必须是一个能被16整除的地址码，即在20位的二进制地址码中最低4位必须是“0”。每个段首地址的高16位二进制代码就是该段的段号(称段基地址)或简称段地址，段号保存在段寄存器中。我们可对段寄存器设置不同的值来使微处理器的存储器访问指向不同的段。
      5.段内的某个存储单元相对于该段段首地址的差值，称为段内偏移地址(也叫偏移量)用16位二进制代码表示。
      6.物理地址是由8086／8088芯片地址引线送出的20位地址码，它用来参加存储器的地址译码，最终读／写所访问的一个特定的存储单元。
      7.逻辑地址由某段的段地址和段内偏移地址(也叫偏移量)两部分所组成。写成：

     段地址：偏移地址(例如，1234H：0088H)。
   8.在硬件上起作用的是物理地址，物理地址＝段基地址×10H十偏移地址

联想笔试题 
　　1．设计函数 int atoi(char *s)。 //字符-》int
　　2．int i=(j=4,k=8,l=16,m=32); printf(“%d”, i); 输出是多少？ （结果为32，即m值多少，结果就是多少）
60.解释局部变量、全局变量和静态变量的含义。

61.论述含参数的宏与函数的优缺点。 
宏是编译期的，函数是运行期的；
宏不是实体，而函数是一个可寻址的实体；
宏只是编译期替换，在程序里每遇到S(a,b)，就用a*b代替，a和b两个实体并没有由宏实际产生；函数S会在栈中定义两个对象a和b。
宏没有生存期、作用域之类的概念，而函数就有。
<span style="font-size: 18px; ">函数是内置的，执行效率高，速度快。宏可以自己定制，灵活性较大，但执行速度相对慢<span style="color: rgb(51, 51, 51); font-family: arial, 'courier new', courier, 宋体, monospace; line-height: 24px; background-color: rgb(241, 254, 221); ">。</span></span>
1.函数调用时，先求出实参表达式的值，然后带入形参。而使用带参的宏只是进行简单的字符替换。
2.函数调用是在程序运行时处理的，分配临时的内存单元；而宏展开则是在编译时进行的，在展开时并不分配内存单元，不进行值的传递处理，也没有“返回值”的概念。
3.对函数中的实参和形参都要定义类型，二者的类型要求一致，如不一致，应进行类型转换；而宏不存在类型问题，宏名无类型，它的参数也无类型，只是一个符号代表，展开时带入指定的字符即可。宏定义时，字符串可以是任何类型的数据。
4.调用函数只可得到一个返回值，而用宏可以设法得到几个结果。
5.使用宏次数多时，宏展开后源程序长，因为每展开一次都使程序增长，而函数调用不使源程序变长。
6.宏替换不占运行时间。而函数调用则占运行时间（分配单元、保留现场、值传递、返回）。
一般来说，用宏来代表简短的表达式比较合适。

普天C++笔试题 
　　1．实现双向链表删除一个节点P，在节点P后插入一个节点，写出这两个函数。 
template<class type> void list<type>::delnode(int p)  
{  
 int k=1;  
 listnode<type> *ptr,*t;  
 ptr=first;  

 while(ptr->next!=NULL&&k!=p)  
 {  
  ptr=ptr->next;  
     k++;  
 }  
    t=ptr->next;  
 cout<<"你已经将数据项 "<<t->data<<"删除"<<endl;  

 ptr->next=ptr->next->next;  
 length--;  
 delete t;  
}  
//在节点P后插入一个节点：  
template<class type> bool list<type>::insert(type t,int p)  
{  
 listnode<type> *ptr;  
 ptr=first;  

 int k=1;  
 while(ptr!=NULL&&k<p)   
 {  
  ptr=ptr->next;  
  k++;  
 }  
 if(ptr==NULL&&k!=p)  
  return false;  
 else  
 {  
   listnode<type> *tp;  
   tp=new listnode<type>;  
   tp->data=t;  
   tp->next=ptr->next;  
   ptr->next=tp;  
   length++;  
     
   return true;  
 }  
}  
　　2．写一个函数，将其中的\t都转换成4个空格。

61.Windows程序的入口是哪里？写出Windows消息机制的流程。 
入口点是WinMain函数.
A. 操作系统接收应用程序的窗口消息，将消息投递到该应用程序的消息队列中 
B. 应用程序在消息循环中调用GetMessage函数从消息队列中取出一条一条的消息，取出消息后，应用程序可以对消息进行一些预处理。 
C. 应用程序调用DispatchMessage，将消息回传给操作系统。 
D. 系统利用WNDCLASS结构体的lpfnWndProc成员保存的窗口过程函数的指针调用窗口过程，对消息进行处理

注意：消息映射为什么不是虚函数呢？
答：C++有一个名为vtable的虚函数分发表。如果用虚函数发送消息，CWnd将为超过100个消息来申明虚函数。对于每个虚函数，vtable中对应有4个字节，那么应用程序将需要400多个字节的表来支持虚拟消息处理函数。所以为了避免大型的vtable，MFC使用宏来把WINDOWS消息连接到C++成员函数。MFC消息处理程序需要函数原型，函数体和在消息映射中的输入项（宏调用），ClassWizard帮助我们将消息处理程序添加到类中。

Windows消息机制的流程:
1.Windows中有一个系统消息队列，对于每一个正在执行的Windows应用程序,系统为其建立一个“消息队列”，即应用程序队列，用来存放该程序可能创建的各种窗口的消息。应用程序中含有一段称作“消息循环”的代码，用来从消息队列中检索这些消息并把它们分发到相应的窗口函数中。
2.Windows为当前执行的每个Windows程序维护一个「消息队列」。在发生输入事件之后，Windows将事件转换为一个「消息」并将消息放入程序的消息队列中。程序通过执行一块称之为「消息循环」的程序代码从消息队列中取出消息：
while(GetMessage (&msg, NULL, 0, 0))
{
TranslateMessage (&msg) ;
DispatchMessage (&msg) ;
}
TranslateMessage(&msg);将msg结构传给Windows，进行一些键盘转换。
DispatchMessage (&msg);又将msg结构回传给Windows。然后，Windows将该消息发送给适当的窗口消息处理程序，让它进行处理。
SendMessage()与PostMessage()之间的区别是什么？
它们两者是用于向应用程序发送消息的。PostMessagex()将消息直接加入到应用程序的消息队列中，不等程序返回就退出；而SendMessage()则刚好相反，应用程序处理完此消息后，它才返回。

62.C++里面是不是所有的动作都是main()引起的？如果不是，请举例。 
不是，比如中断引起的中断处理不是直接由main()引起的，而是由外部事件引起的。
在运行c++程序时，通常从main()函数开始执行。因此如果没有main(),程序将不完整，编译器将指出未定义main()函数。
例外情况：如，在windows编程中，可以编写一个动态连接库（dll)模块，这是其他windows程序可以使用的代码。由于DLL模块不是独立的程序，因此不需要main().用于专用环境的程序--如机器人中的控制器芯片--可能不需要main().但常规的独立程序都需要main().
比如全局变量的初始化，就不是由main函数引起的.但是这个初始化动作并不能为编译器的断点所截断

4．如何定义和实现一个类的成员函数为回调函数？
所谓的回调函数，就是预先在系统的对函数进行注册，让系统知道这个函数的存在，以后，当某个事件发生时，再调用这个函数对事件进行响应。
定义一个类的成员函数时在该函数前加CALLBACK即将其定义为回调函数，函数的实现和普通成员函数没有区别
回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数
如果试图直接使用C++的成员函数作为回调函数将发生错误，甚至编译就不能通过。通过查询资料发现，其错误是普通的C++成员函数都隐含了一个传递函数作为参数，亦即“this”指针，C++通过传递一个指向自身的指针给其成员函数从而实现程序函数可以访问C++的数据成员。这也可以理解为什么C++类的多个实例可以共享成员函数但是确有不同的数据成员。由于this指针的作用，使得将一个CALLBACK型的成员函数作为回调函数安装时就会因为隐含的this指针使得函数参数个数不匹配，从而导致回调函数安装失败。要解决这一问题的关键就是不让this指针起作用，通过采用以下两种典型技术可以解决在C++中使用回调函数所遇到的问题。这种方法具有通用性，适合于任何C++。   
　　1).   不使用成员函数，直接使用普通C函数，为了实现在C函数中可以访问类的成员变量，可以使用友元操作符(friend)，在C++中将该C函数说明为类的友元即可。这种处理机制与普通的C编程中使用回调函数一样。   
　　2).   使用静态成员函数，静态成员函数不使用this指针作为隐含参数，这样就可以作为回调函数了。静态成员函数具有两大特点：其一，可以在没有类实例的情况下使用；其二，只能访问静态成员变量和静态成员函数，不能访问非静态成员变量和非静态成员函数。由于在C++中使用类成员函数作为回调函数的目的就是为了访问所有的成员变量和成员函数，如果作不到这一点将不具有实际意义。解决的办法也很简单，就是使用一个静态类指针作为类成员，通过在类创建时初始化该静态指针，如pThis=this，然后在回调函数中通过该静态指针就可以访问所有成员变量和成员函数了。这种处理办法适用于只有一个类实例的情况，因为多个类实例将共享静态类成员和静态成员函数，这就导致静态指针指向最后创建的类实例。为了避免这种情况，可以使用回调函数的一个参数来传递this指针，从而实现数据成员共享。这种方法稍稍麻烦，这里就不再赘述。
举例：
class Test
{
public:
    static void callBackFun(void){}; //因为callBackFun默认有一个const Test* 的指针
};
typedef void (*FPtr)(void);
//typedef   void   (*PFV)();是一个类型定义，定义了一种函数指针，这种指针指向的函数不待参数，无返回类型。   
//typedef   struct   bt{int   i; int   j; }BT;  BT   *tt;
//其中，BT代替了struct   bt   *tt;这就是typedef的用法
void Fun(FPtr ptr)
{
    ptr();
}
void main(void)
{
    Fun(Test::callBackFun); 
}

5．解释堆和栈的区别。
6．C++里面如何声明const void f(void)函数为C程序中的库函数？ 
在该函数前添加extern “C”声明。
　　7．下列哪两个是等同的 
　　int b; 
　　A const int* a = &b; 
　　B const* int a = &b; 
　　C const int* const a = &b; 
　　D int const* const a = &b; 
各式表示的意思分别为：
A const int* a = &b; //*a是const,但指针a可变
B const* int a = &b; //a是const，但*a可变
C const int* const a = &b; //a和*a都是const,常量和指针的值都不能改变
D int const* const a = &b; //a和*a都是const,常量和指针的值都不能改变
因此C，D两者是相同的。
总结个技巧：如果const位于星号的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；如果const位于星号的右侧，const就是修饰指针本身，即指针本身是常量。
　　8．内联函数在编译时是否做参数类型检查？ 
　　做类型检查，因为内联函数就是在程序编译时，编译器将程序中出现的内联函数的调用表达式用内联函数的函数体来代替。
void g(base & b){
　　 b.play;
　　}
　　void main(){
　　 son s;
　　 g(s);
　　 return;
　　}

大唐电信 
　　DTT笔试题 
　　考试时间一小时，第一部分是填空和选择： 
　　1．数列6，10，18，32，“？”，问“？”是几？ 
　　2．某人出70买进一个x，80卖出，90买回，100卖出，这桩买卖怎么样？ 
　　3．月球绕地球一圈，至少要多少时间？ 
　　4．7个人用7小时挖了7米的沟，以同样的速度在50小时挖50米的沟要多少人？ 
　　5．鱼头长9，鱼尾等于鱼头加半个鱼身，鱼身等于鱼头加鱼尾，问鱼全长多少？ 
　　6．一个小姐买了一块手表，回家发现手表比她家的表慢了两分钟，晚上看新闻的时候 
又发现她家的表比新闻里的时间慢了两分钟，则 。 
　　A 手表和新闻里的时间一样 
　　B 手表比新闻里的时间慢 
　　C 手表比新闻里的时间快 
　　7．王先生看到一则招聘启事，发现两个公司除了以下条件不同外，其他条件都相同 

　　A 半年年薪50万，每半年涨5万 
　　B 一年年薪100万，每一年涨20万 
　　王先生想去一家待遇比较优厚的公司，他会去哪家？ 
　　10．问哪个袋子里有金子？ 
　　A袋子上的标签是这样写的：B袋子上的话是对的，金子在A袋子。 
　　B袋子上的标签是这样写的：A袋子上的话是错的，金子在A袋子里。 
　　11．3个人住酒店30块钱，经理找回5块钱，服务生从中藏了2块钱，找给每人1块钱， 
3×（101）+2=29，问这是怎么回事？ 
　　12．三篇写作，均为书信形式。 
　　（1）一片中文的祝贺信，祝贺某男当了某公司xx 
　　（2）两篇英文的，一是说有事不能应邀，派别人去；另一篇是讨债的，7天不给钱就 
走人（主要考business letter格式）。 
　　大唐面试试题 
　　1．什么是中断？中断发生时CPU做什么工作？ 
　　2．CPU在上电后，进入操作系统的main()之前必须做什么工作？ 
　　3．简述ISO OSI的物理层Layer1，链路层Layer2，网络层Layer3的任务。 
　　4．有线电话和无线电话有何区别？无线电话特别需要注意的是什么？ 
63.软件开发五个主要step是什么？
<u>1.问题的定义及规划</u>
此阶段是软件开发与需求放共同讨论，主要确定软件的开发目标及其可行性。
<u>2.需求分析</u>
在确定软件开发可行性的情况下，对软件需要实现的各个功能进行详细需求分析。需求分析阶段是一个很重要的阶段，这一阶段做的好，将为整个软件项目的开发打下良好的基础。“唯一不变的是变化本身”，同样软件需求也是在软件爱你开发过程中不断变化和深入的，因此，我们必须定制需求变更计划来应付这种变化，以保护整个项目的正常进行。
<u>3.软件设计</u>
此阶段中偶要根据需求分析的结果，对整个软件系统进行设计，如系统框架设计、数据库设计等。软件设计一般分为总体设计和详细设计。还的软件设计将为软件程序编写打下良好的基础。
<u>4.程序编码</u>
此阶段是将软件设计的结果转化为计算机可运行的程序代码。在程序编码中必定要制定统一、符合标准的编写规范。以保证程序的可读性、易维护性。提高程序的运行效率。
<u>5.软件测试</u>
在软件设计完成之后要进行严密的测试，一发现软件在整个软件设计过程中存在的问题并加以纠正。整个测试阶段分为单元测试、组装测试、系统测试三个阶段进行。测试方法主要有白盒测试和黑盒测试。

　　6．你在开发软件的时候，这5个step分别占用的时间百分比是多少？ 
各步骤所占整体开发的时间比大概是：30%－－20%－－15%－－30%－－5%
　　7．makefile文件的作用是什么？ 
其作用如下：
       makefile关系到了整个工程的编译规则。一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。
    makefile带来的好处就是——“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率
　　8．UNIX显示文件夹中，文件名的命令是什么？能使文件内容显示在屏幕的命令是什么 ？ 
l , lc  ,  ls  ,  这三个命令都可以显示文件名,只是显示方式不同
cat  ,  more  ,  vi  ,  都能显示文件内容,也是显示方式不同
用echo * ,同样能显示当前目录的文件,只不过它们是挤在一起的
　　9．（选做）手机用户在从一个基站漫游到另一个基站的过程中，都会发生什么? 
在退出当前基站前，在下一个基站注册，当注册完毕开启新的信道后，从上一个基站退出！
──────────────────────────────────────── 
网通笔试题 
　　选择题（每题5分，只有一个正确答案） 
　　1．中国1号信令协议属于 的协议。 
　　A ccs B cas C ip D atm 
　　2．isdnpri协议全称是 。 
　　A 综合业务模拟网基速协议 
　　B 综合业务模拟网模拟协议 
　　C 综合业务数字网基率协议 
　　D 综合业务数字网基次协议 
　　3．路由协议中， 协议是用距离作为向量的。 
　　A ospf B bgp C is-is D rip 
　　4．中国智能网中，ssp与scp间最上层的ss7协议是 。 
　　A incs B is41b C is41c D inap 
　　5．dtmf全称是 。 
　　A 双音多频 B多音双频 C多音三频 D三音多频 
　　6．计算机的基本组成部分中，不包含下面设备的是 。 
　　A cpu B输入设备 C存储器 D接口 
　　7．脉冲编码调制的简称是 。 
　　A pcm B pam C (delta)M D atm 
　　8．普通电话线接口专业称呼是 。 
　　A rj11 B rj45 C rs232 D bnc 
　　9．现有的公共数据网都采用 。 
　　A电路交换技术 B报文交换技术 
　　C语音插空 D分组交换 
　　10．ss7协议中的制止市忙消息简写为 。 
　　A stb B slb C sub D spb 
　　简答题（每题10分） 
　　1．简述普通电话与IP电话的区别。 
　　2．简述随路信令与公路信令的根本区别。 
　　3．说明掩码的主要作用。 
　　4．ss7协议中，有三大要素决定其具体定位，哪三大要素？ 
　　5．描述ss7的基本通话过程。 
　　6．简述通信网的组成结构。 
　　7．面向连接与面向非连接各有何利弊？ 
　　8．写出爱尔兰的基本计算公式。 
　　9．数据网主要有哪些设备？ 
　　10．中国一号协议是如何在被叫号码中插入主叫号码的？ 

东信笔试题目 
　　笔试：30分钟。 
　　1．压控振荡器的英文缩写。 
　　2．动态随机存储器的英文缩写。 
　　3．选择电阻时要考虑什么？ 
　　4．单片机上电后没有运转，首先要检查什么？ 
　　5．计算机的基本组成部分及其各自的作用。 
　　6．怎样用D触发器、与或非门组成二分频电路？
64.static有什么用途？（请至少说明两种）
答 、1.限制变量的作用域(文件级的)。
 2.设置变量的存储域(全局数据区)。
65.引用与指针有什么区别？
答 、1) 引用必须被初始化，指针不必。
2) 引用初始化以后不能被改变，指针可以改变所指的对象。
3) 不存在指向空值的引用，但是存在指向空值的指针。
66.描述实时系统的基本特性
答 、在特定时间内完成特定的任务，实时性与可靠性。
所谓“实时操作系统”，实际上是指操作系统工作时，其各种资源可以根据需要随时进行动态分配。由于各种资源可以进行动态分配，因此其处理事务的能力较强、速度较快。  
应该说，实时操作系统是在早期的操作系统基础上发展起来的，早期的操作系统的各种资源都是事先已经分配好的，工作期间这些资源不能再重新进行分配。因此其处理事务的能力较差、速度较慢，现在则称之为“非实时操作系统”。但“非实时操作系统”诞生时，其功能、性能等在当时也是非常强的，人们在未认识到更好的操作系统之前并不将其这样称呼。将来如果新的、功能更强的、实时性能更高的操作系统出现，也许现在称之为“实时”的操作系统则可能将让位于新的“实时操作系统”了。从这方面讲“实时操作系统”是一个相对的概念的
通常，实时操作系统必须有以下特征：
1）多任务；
2）有线程优先级
3）多种中断级别
小的嵌入式操作系统经常需要实时操作系统。内核要满足实时操作系统的要求。但其它部件，如设备驱动程序也是需要的，因此，一个实时操作系统常比内核大。
67.全局变量和局部变量在内存中是否有区别？如果有，是什么区别？
答 、全局变量储存在静态数据区，局部变量在堆栈中。
68.什么是平衡二叉树？
答 、左右子树都是平衡二叉树 且左右子树的深度差值的绝对值不大于1。
69.堆栈溢出一般是由什么原因导致的？
答 、1.没有回收垃圾资源
        2.层次太深的递归调用     
70.什么函数不能声明为虚函数？
答 、constructor  构造函数
       Deconstructor 可以声明为虚函数。
       系统为一个空类创建的成员函数有那些。
71.冒泡排序算法的时间复杂度是什么？
答 、O(n^2)
72.写出float x 与“零值”比较的if语句。
答 ：typedef float EXPRESSION ;EXPRESSION x; if(x>0.000001&&x<-0.000001)
73.Internet采用哪种网络协议？该协议的主要层次结构？
答 、tcp/ip 应用层/传输层/网络层/数据链路层/物理层
74.Internet物理地址和IP地址转换采用什么协议？
答 、ARP (Address Resolution Protocol)（地址解析协议）
75.IP地址的编码分为哪俩部分？
答 、IP地址由两部分组成，网络号和主机号。不过是要和“子网掩码”按位与之后才能区分哪些是网络位哪些是主机位。
76.用户输入M,N值，从1至N开始顺序循环数数，每数到M输出该数值，直至全部输出。写出C程序。
答 、循环链表，用取余操作做
77.不能做switch()的参数类型是：
答 、switch的参数不能为实型。
华为
78.局部变量能否和全局变量重名？
答、能，局部会屏蔽全局。要用全局变量，需要使用"::"
局部变量可以与全局变量同名，在函数内引用这个变量时，会用到同名的局部变量，而不会用到全局变量。对于有些编译器而言，在同一个函数内可以定义多个同名的局部变量，比如在两个循环体内都定义一个同名的局部变量，而那个局部变量的作用域就在那个循环体内
79.如何引用一个已经定义过的全局变量？
答 、可以用引用头文件的方式，也可以用extern关键字。
如果用引用头文件方式来引用某个在头文件中声明的全局变理，假定你将那个变写错了，那么在编译期间会报错，如果你用extern方式引用时，假定你犯了同样的错误，那么在编译期间不会报错，而在连接期间报错
80.全局变量可不可以定义在可被多个.C文件包含的头文件中？为什么？
答 、可以，在不同的C文件中以static形式来声明同名全局变量。
可以在不同的C文件中声明同名的全局变量，前提是其中只能有一个C文件中对此变量赋初值，此时连接不会出错
81.语句for( ；1 ；)有什么问题？它是什么意思？
答 、和while(1)相同。无限循环下去
82.do……while和while……do有什么区别？
答 、前一个循环一遍再判断，后一个判断以后再循环
83.请写出下列代码的输出内容
#i nclude
main()
{
int a,b,c,d;
a=10;
b=a++;
c=++a;
d=10*a++;
printf("b，c，d：%d，%d，%d"，b，c，d）;
return 0;
}
答 、10，12，120
84.static 全局变量、局部变量、函数与普通全局变量、局部变量、函数
static全局变量与普通的全局变量有什么区别？static局部变量和普通局部变量有什么区别？static函数与普通函数有什么区别？
答 、全局变量(外部变量)的说明之前再冠以static 就构成了静态的全局变量。全局变量本身就是静态存储方式， 静态全局变量当然也是静态存储方式。 这两者在存储方式上并无不同。这两者的区别虽在于非静态全局变量的作用域是整个源程序， 当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。 而静态全局变量则限制了其作用域， 即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用， 因此可以避免在其它源文件中引起错误。
从以上分析可以看出， 把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域， 限制了它的使用范围。
static函数与普通函数作用域不同，仅在本文件。只在当前源文件中使用的函数应该说明为内部函数(static)，内部函数应该在当前源文件中说明和定义。对于可在当前源文件以外使用的函数，应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件
static全局变量与普通的全局变量有什么区别：static全局变量只初使化一次，防止在其他文件单元中被引用;
static局部变量和普通局部变量有什么区别：static局部变量只被初始化一次，下一次依据上一次结果值；
static函数与普通函数有什么区别：static函数在内存中只有一份，普通函数在每个被调用中维持一份拷贝
程序的局部变量存在于（堆栈）中，全局变量存在于（静态区 ）中，动态申请数据存在于（ 堆）中。
85.设有以下说明和定义：
typedef union {long i; int k[5]; char c;} DATE;
struct data { int cat; DATE cow; double dog;} too;
DATE max;
则语句 printf("%d",sizeof(struct date)+sizeof(max));的执行结果是？
答 、结果是：___52____。DATE是一个union, 变量公用空间. 里面最大的变量类型是int[5], 占用20个字节. 所以它的大小是20
data是一个struct, 每个变量分开占用空间. 依次为int4 + DATE20 + double8 = 32.
所以结果是 20 + 32 = 52.
当然...在某些16位编辑器下, int可能是2字节,那么结果是 int2 + DATE10 + double8 = 20
86.-1,2,7,28,,126请问28和126中间那个数是什么？为什么？
答 、应该是4^3-1=63
规律是n^3-1(当n为偶数0，2，4)
n^3+1(当n为奇数1，3，5)
87.用两个栈实现一个队列的功能？要求给出算法和思路！
答 、设2个栈为A,B, 一开始均为空.
入队:
将新元素push入栈A;
出队:
(1)判断栈B是否为空；
(2)如果不为空，则将栈A中所有元素依次pop出并push到栈B；
(3)将栈B的栈顶元素pop出；
这样实现的队列入队和出队的平摊复杂度都还是O(1),。
88.在c语言库函数中将一个字符转换成整型的函数是atool()吗，这个函数的原型是什么？
答 、函数名: atol
功 能: 把字符串转换成长整型数
用 法: long atol(const char *nptr);
程序例:
＃include
＃include
int main(void)
{
    long l;
    char *str = "98765432";
    l = atol(lstr);
    printf("string = %s integer = %ld\n", str, l);
    return(0);
}
89.对于一个频繁使用的短小函数,在C语言中应用什么实现,在C++中应用什么实现?
答 、c用宏定义，c++用inline
90.用预处理指令#define 声明一个常数，用以表明1年中有多少秒（忽略闰年问题）
#define SECONDS_PER_YEAR (60 * 60 * 24 * 365)UL
我在这想看到几件事情：
1). #define 语法的基本知识（例如：不能以分号结束，括号的使用，等等）
2). 懂得预处理器将为你计算常数表达式的值，因此，直接写出你是如何计算一年中有多少秒而不是计算出实际的值，是更清晰而没有代价的。
3). 意识到这个表达式将使一个16位机的整型数溢出-因此要用到长整型符号L,告诉编译器这个常数是的长整型数。
4). 如果你在你的表达式中用到UL（表示无符号长整型），那么你有了一个好的起点。记住，第一印象很重要。
91.写一个“标准”宏MIN，这个宏输入两个参数并返回较小的一个。
#define MIN(A,B) ((A) <= (B) ?(A) : (B))
这个测试是为下面的目的而设的：
1). 标识#define在宏中应用的基本知识。这是很重要的，因为直到嵌入(inline)操作符变为标准C的一部分，宏是方便产生嵌入代码的唯一方法，
对于嵌入式系统来说，为了能达到要求的性能，嵌入代码经常是必须的方法。
2). 三重条件操作符的知识。这个操作符存在C语言中的原因是它使得编译器能产生比if-then-else更优化的代码，了解这个用法是很重要的。
3). 懂得在宏中小心地把参数用括号括起来
4). 我也用这个问题开始讨论宏的副作用，例如：当你写下面的代码时会发生什么事？
least = MIN(*p++, b);
92.预处理器标识#error的目的是什么？
如果你不知道答案，请看参考文献1。这问题对区分一个正常的伙计和一个书呆子是很有用的。只有书呆子才会读C语言课本的附录去找出象这种
问题的答案。当然如果你不是在找一个书呆子，那么应试者最好希望自己不要知道答案。死循环（Infinite loops）
# 空指令，无任何效果 
#include 包含一个源代码文件 
#define 定义宏 
#undef 取消已定义的宏 
#if 如果给定条件为真，则编译下面代码 
#ifdef 如果宏已经定义，则编译下面代码 
#ifndef 如果宏没有定义，则编译下面代码 
#elif 如果前面的#if给定条件不为真，当前条件为真，则编译下面代码 
#endif 结束一个#if……#else条件编译块 
#error 停止编译并显示错误信息
93.嵌入式系统中经常要用到无限循环，你怎么样用C编写死循环呢？
这个问题用几个解决方案。我首选的方案是：
while(1)
{
}
一些程序员更喜欢如下方案：
for(;;)
{
}
这个实现方式让我为难，因为这个语法没有确切表达到底怎么回事。如果一个应试者给出这个作为方案，我将用这个作为一个机会去探究他们这样做的
基本原理。如果他们的基本答案是：“我被教着这样做，但从没有想到过为什么。”这会给我留下一个坏印象。
第三个方案是用 goto
Loop:
...
goto Loop;
应试者如给出上面的方案，这说明或者他是一个汇编语言程序员（这也许是好事）或者他是一个想进入新领域的BASIC/FORTRAN程序员。
94.用变量a给出下面的定义
a) 一个整型数（An integer）
b) 一个指向整型数的指针（A pointer to an integer）
c) 一个指向指针的的指针，它指向的指针是指向一个整型数（A pointer to a pointer to an integer）
d) 一个有10个整型数的数组（An array of 10 integers）
e) 一个有10个指针的数组，该指针是指向一个整型数的（An array of 10 pointers to integers）
f) 一个指向有10个整型数数组的指针（A pointer to an array of 10 integers）
g) 一个指向函数的指针，该函数有一个整型参数并返回一个整型数（A pointer to a function that takes an integer as an argument and returns an integer）
h) 一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数（ An array of ten pointers to functions that take an integer
argument and return an integer ）
答案是：
a) int a; // An integer
b) int *a; // A pointer to an integer
c) int **a; // A pointer to a pointer to an integer
d) int a[10]; // An array of 10 integers
e) int *a[10]; // An array of 10 pointers to integers
f) int (*a)[10]; // A pointer to an array of 10 integers
g) int (*a)(int); // A pointer to a function a that takes an integer argument and returns an integer
h) int (*a[10])(int); // An array of 10 pointers to functions that take an integer argument and return an integer
人们经常声称这里有几个问题是那种要翻一下书才能回答的问题，我同意这种说法。当我写这篇文章时，为了确定语法的正确性，我的确查了一下书。
但是当我被面试的时候，我期望被问到这个问题（或者相近的问题）。因为在被面试的这段时间里，我确定我知道这个问题的答案。应试者如果不知道
所有的答案（或至少大部分答案），那么也就没有为这次面试做准备，如果该面试者没有为这次面试做准备，那么他又能为什么出准备呢？
95.关键字static的作用是什么？
这个简单的问题很少有人能回答完全。在C语言中，关键字static有三个明显的作用：
1). 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。
2). 在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。
3). 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。
大多数应试者能正确回答第一部分，一部分能正确回答第二部分，同是很少的人能懂得第三部分。这是一个应试者的严重的缺点，因为他显然不懂得本地化数据和代码范围的好处和重要性。
96.关键字const是什么含意？
我只要一听到被面试者说：“const意味着常数”，我就知道我正在和一个业余者打交道。去年Dan Saks已经在他的文章里完全概括了const的所有用法，因此ESP(译者：Embedded Systems Programming)的每一位读者应该非常熟悉const能做什么和不能做什么.如果你从没有读到那篇文章，只要能说出const意味着“只读”就可以了。尽管这个答案不是完全的答案，但我接受它作为一个正确的答案。（如果你想知道更详细的答案，仔细读一下Saks的文章吧。）如果应试者能正确回答这个问题，我将问他一个附加的问题：下面的声明都是什么意思？
const int a;
int const a;
const int *a;
int * const a;
int const * a const;
前两个的作用是一样，a是一个常整型数。第三个意味着a是一个指向常整型数的指针（也就是，整型数是不可修改的，但指针可以）。第四个意思a是一个指向整型数的常指针（也就是说，指针指向的整型数是可以修改的，但指针是不可修改的）。最后一个意味着a是一个指向常整型数的常指针（也就是说，指针指向的整型数是不可修改的，同时指针也是不可修改的）。如果应试者能正确回答这些问题，那么他就给我留下了一个好印象。顺带提一句，也许你可能会问，即使不用关键字 const，也还是能很容易写出功能正确的程序，那么我为什么还要如此看重关键字const呢？我也如下的几下理由：
1). 关键字const的作用是为给读你代码的人传达非常有用的信息，实际上，声明一个参数为常量是为了告诉了用户这个参数的应用目的。如果你曾花很多时间清理其它人留下的垃圾，你就会很快学会感谢这点多余的信息。（当然，懂得用const的程序员很少会留下的垃圾让别人来清理的。）
2). 通过给优化器一些附加的信息，使用关键字const也许能产生更紧凑的代码。
3). 合理地使用关键字const可以使编译器很自然地保护那些不希望被改变的参数，防止其被无意的代码修改。简而言之，这样可以减少bug的出现。
97.关键字volatile有什么含意 并给出三个不同的例子。
一个定义为volatile的变量是说这变量可能会被意想不到地改变，这样，编译器就不会去假设这个变量的值了。精确地说就是，优化器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。下面是volatile变量的几个例子：
1). 并行设备的硬件寄存器（如：状态寄存器）
2). 一个中断服务子程序中会访问到的非自动变量(Non-automatic variables)
3). 多线程应用中被几个任务共享的变量
回答不出这个问题的人是不会被雇佣的。我认为这是区分C程序员和嵌入式系统程序员的最基本的问题。嵌入式系统程序员经常同硬件、中断、RTOS等等打交道，所用这些都要求volatile变量。不懂得volatile内容将会带来灾难。
假设被面试者正确地回答了这是问题（嗯，怀疑这否会是这样），我将稍微深究一下，看一下这家伙是不是直正懂得volatile完全的重要性。
1). 一个参数既可以是const还可以是volatile吗？解释为什么。
2). 一个指针可以是volatile 吗？解释为什么。
3). 下面的函数有什么错误：
int square(volatile int *ptr)
{
return *ptr * *ptr;
}
下面是答案：
1). 是的。一个例子是只读的状态寄存器。它是volatile因为它可能被意想不到地改变。它是const因为程序不应该试图去修改它。
2). 是的。尽管这并不很常见。一个例子是当一个中服务子程序修该一个指向一个buffer的指针时。
3). 这段代码的有个恶作剧。这段代码的目的是用来返指针*ptr指向值的平方，但是，由于*ptr指向一个volatile型参数，编译器将产生类似下面的代码：
int square(volatile int *ptr)
{
int a,b;
a = *ptr;
b = *ptr;
return a * b;
}
由于*ptr的值可能被意想不到地该变，因此a和b可能是不同的。结果，这段代码可能返不是你所期望的平方值！正确的代码如下：
long square(volatile int *ptr)
{
int a;
a = *ptr;
return a * a;
}
98.下面的代码输出是什么，为什么？
void foo(void)
{
unsigned int a = 6;
int b = -20;
(a+b > 6) puts("> 6") : puts("<= 6");
}
这个问题测试你是否懂得C语言中的整数自动转换原则，我发现有些开发者懂得极少这些东西。不管如何，这无符号整型问题的答案是输出是“>6”。原因是当表达式中存在有符号类型和无符号类型时所有的操作数都自动转换为无符号类型。因此-20变成了一个非常大的正整数，所以该表达式计算出的结果大于6。这一点对于应当频繁用到无符号数据类型的嵌入式系统来说是丰常重要的。如果你答错了这个问题，你也就到了得不到这份工作的边缘。
99.C语言同意一些令人震惊的结构,下面的结构是合法的吗，如果是它做些什么？
int a = 5, b = 7, c;
c = a+++b;
这个问题将做为这个测验的一个愉快的结尾。不管你相不相信，上面的例子是完全合乎语法的。问题是编译器如何处理它？水平不高的编译作者实际上会争论这个问题，根据最处理原则，编译器应当能处理尽可能所有合法的用法。因此，上面的代码被处理成：
c = a++ + b;
因此, 这段代码持行后a = 6, b = 7, c = 12。
如果你知道答案，或猜出正确答案，做得好。如果你不知道答案，我也不把这个当作问题。我发现这个问题的最大好处是:这是一个关于代码编写风格，代码的可读性，代码的可修改性的好的话题
今天早上的面试题9道，比较难，
100.线形表a、b为两个有序升序的线形表，编写一程序，使两个有序线形表合并成一个有序升序线形表h；
答案在  严锐敏《数据结构第二版》第二章例题，数据结构当中，这个叫做：两路归并排序
Linklist *unio(Linklist *p,Linklist *q){  
linklist *R,*pa,*qa,*ra;  
pa=p;  
qa=q;  
R=ra=p;  
while(pa->next!=NULL&&qa->next!=NULL){  
if(pa->data>qa->data){  
ra->next=qa;  
qa=qa->next;  
}  
else{  
ra->next=pa;  
pa=pa->next;  
}  
}  
if(pa->next!=NULL)  
ra->next=pa;  
if(qa->next!=NULL)  
ra->next==qa;  
return R;  
}  
101.用递归算法判断数组a[N]是否为一个递增数组。
递归的方法，记录当前最大的，并且判断当前的是否比这个还大，大则继续，否则返回false结束：
bool fun( int a[], int n )  
{  
if( n= =1 )  
return true;  
if( n= =2 )  
return a[n-1] >= a[n-2];  
return fun( a,n-1) && ( a[n-1] >= a[n-2] );  
}  
102.编写算法，从10亿个浮点数当中，选出其中最大的10000个。
用外部排序，在《数据结构》书上有《计算方法导论》在找到第n大的数的算法上加工
先将数据进行分割成数据量小的一些文件，如1000000个数据为一个文件，然后将每个文件数据进行排序，用快速排序法排序，然后使用K路合并法将其合并到一个文件下，取出排序好的最大的10000个数据）
另解：
（1）读一次所有数据，得出最大和最小。
（2）用最大和最小，分100个区间
dx = (x_max - x_min) / 100
每区间范围： dx * (i-1) 到 dx * i，i=1 到 100.
(3) 读一次所有数据，统计落入各区间的数据个数。
(4) 从最大区间起，把数据个数依次加起来，找到正好大于1万个数的区间。
(5) 如果这个区间的数很多，可以再细分100个区间查找，如数据不太多，则读一次所有数据，对大小范围在此区间的数排队，找出需要的数。
（6）读一次所有数据，把大于此区间上限的数取出来。
5,6 步找到的数据合在一起是 1 万个数。
103.编写一unix程序，防止僵尸进程的出现.
<span style="background-color: rgb(51, 255, 51); ">子进程结束后，如果它的结束状态没有被父进程用wait,waitpid等系统调用来获取了话，此时这个子进程就叫zombie process.</span><span style="background-color: rgb(241, 254, 221); ">

下面的程序就演示了如何通过wait调用来获取子进程退出状态。在用户第一次回车之前，通过ps命令，应该可以看到子进程是 defunct 状态 （也就是zombie态），而一旦按下回车，wait被调用，此时通过ps命令就无法看到那个zombie了。</span>
<span style="background-color: rgb(241, 254, 221); "><span style="color: rgb(51, 51, 51); "><u>所以防止zombie process的方法就是通过wait/waitpid等调用来解决</u></span><span style="color: rgb(51, 51, 51); ">。
</span></span><div class="dp-highlighter bg_cpp" style="font-family: Consolas, 'Courier New', Courier, mono, serif; border: 1px dashed rgb(153, 153, 153); background-color: rgb(245, 245, 245); font-size: 12px; width: 936.5333251953125px; overflow: auto; padding-top: 1px; margin: 18px 0px !important; "><ol start="1" class="dp-cpp" style="margin: 0px; padding: 5px 0px; border: none; position: relative; list-style-position: initial; list-style-image: initial; color: rgb(92, 92, 92); "><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="preprocessor" style="margin: 0px; padding: 0px; border: none; color: gray; background-color: inherit; ">#include <stdio.h></span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="preprocessor" style="margin: 0px; padding: 0px; border: none; color: gray; background-color: inherit; ">#include <unistd.h></span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="preprocessor" style="margin: 0px; padding: 0px; border: none; color: gray; background-color: inherit; ">#include <stdlib.h></span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="datatypes" style="margin: 0px; padding: 0px; border: none; color: rgb(46, 139, 87); background-color: inherit; font-weight: bold; ">int</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; "> main()  </span></span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">{  </span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    <span class="datatypes" style="margin: 0px; padding: 0px; border: none; color: rgb(46, 139, 87); background-color: inherit; font-weight: bold; ">int</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; "> pid;  </span></span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    pid = fork();  </span></li></ol></div><div class="dp-highlighter bg_cpp" style="font-family: Consolas, 'Courier New', Courier, mono, serif; border: 1px dashed rgb(153, 153, 153); background-color: rgb(245, 245, 245); font-size: 12px; width: 936.5333251953125px; overflow: auto; padding-top: 1px; margin: 18px 0px !important; "><ol start="1" class="dp-cpp" style="margin: 0px; padding: 5px 0px; border: none; position: relative; list-style-position: initial; list-style-image: initial; color: rgb(92, 92, 92); "><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; "><span style=</span><span class="string" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 153, 0); background-color: inherit; ">"color:#333333;"</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">></span><span class="comment" style="margin: 0px; padding: 0px; border: none; color: rgb(153, 153, 153); background-color: inherit; ">//</span><span style="color: rgb(51, 51, 51); font-family: arial, 宋体, sans-serif; font-size: 14px; line-height: 24px; text-indent: 30px;">一个现有进程可以调用fork函数创建一个新进程。由fork创建的新进程被称为子进程（child process）。fork函数被调用一次但返回两次。两次返回的//唯一区别是子进程中返回0值，而</span><a target="_blank" href="http://baike.baidu.com/view/8507578.htm" style="text-decoration: none; font-family: arial, 宋体, sans-serif; font-size: 14px; line-height: 24px; text-indent: 30px;"><span style="color:#000000;">父进程</span></a><span style="color: rgb(51, 51, 51); font-family: arial, 宋体, sans-serif; font-size: 14px; line-height: 24px; text-indent: 30px;">中返回子进程ID。</span><span style="color:#333333;"></span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    <span class="keyword" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 0, 255); background-color: inherit; font-weight: bold; ">if</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; "> (pid == 0)  </span></span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    {  </span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">        <span class="comment" style="margin: 0px; padding: 0px; border: none; color: rgb(153, 153, 153); background-color: inherit; ">/* child process */</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">        printf(<span class="string" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 153, 0); background-color: inherit; ">"This is child process,pid=%d,bye!\n"</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">, getpid());  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">        exit(0);  </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    }  </span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    sleep(1);  </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    printf(<span class="string" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 153, 0); background-color: inherit; ">"Press return to remove zombie process\n"</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">);  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    getchar();  </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    <span class="comment" style="margin: 0px; padding: 0px; border: none; color: rgb(153, 153, 153); background-color: inherit; ">/* parent process must wait on child process to prevent zombie</span> </span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="comment" style="margin: 0px; padding: 0px; border: none; color: rgb(153, 153, 153); background-color: inherit; ">     * process.</span> </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; "><span class="comment" style="margin: 0px; padding: 0px; border: none; color: rgb(153, 153, 153); background-color: inherit; ">     */</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    wait(NULL);  </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    printf(<span class="string" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 153, 0); background-color: inherit; ">"Press return to exit\n"</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; ">);  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    getchar();  </span></li><li class="alt" style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; color: inherit; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">    <span class="keyword" style="margin: 0px; padding: 0px; border: none; color: rgb(0, 0, 255); background-color: inherit; font-weight: bold; ">return</span><span style="margin: 0px; padding: 0px; border: none; background-color: inherit; "> 0;  </span></span></li><li style="margin: 0px !important; padding: 0px 3px 0px 10px !important; border: none; list-style: decimal-leading-zero outside; line-height: 13px; "><span style="margin: 0px; padding: 0px; border: none; color: black; background-color: inherit; ">}</span>  </span></li></ol></div>

同学的4道面试题，应聘的职位是搜索引擎工程师，后两道超级难，（希望大家多给一些算发）
1.给两个数组和他们的大小，还有一动态开辟的内存，求交集，把交集放到动态内存dongtai，并且返回交集个数
long jiaoji(long* a[],long b[],long* alength,long blength,long* dongtai[])
long jiaoji(long* a[],long b[],long alength,long blength,long* dongtai[])  
{  
    long i,j,k;  
    i = j = k = 0;  
    while (i < alength && j < blength)  
    {  
        if (a[i] < b[j])  
        {  
            i++;  
        }  
        else if (a[i] > b[j])  
        {  
            j++;  
        else  
        {  
            c[k] = a[i];  
            k++;  
            i++;  
            j++;  
        }  
    }  
    return k;  
}  
2.单连表的建立，把'a'--'z'26个字母插入到链表中，并且倒叙，还要打印！
方法1：
typedef struct val
{   int date_1;
    struct val *next;
}*p;

void main(void)
{   char c;
    for(c=122;c>=97;c--)
       { p.date=c;
         p=p->next;
        }
    p.next=NULL;
}
}
方法2：
node *p = NULL;
node *q = NULL;

node *head = (node*)malloc(sizeof(node));
head->data = ' ';head->next=NULL;

node *first = (node*)malloc(sizeof(node));
first->data = 'a';first->next=NULL;head->next = first;
p = first;

int longth = 'z' - 'b';
int i=0;
while ( i<=longth )
{
node *temp = (node*)malloc(sizeof(node));
temp->data = 'b'+i;temp->next=NULL;q=temp;

head->next = temp; temp->next=p;p=q;
i++;
}
print(head);
104.可怕的题目终于来了
象搜索的输入信息是一个字符串，统计300万输入信息中的最热门的前十条，我们每次输入的一个字符串为不超过255byte,内存使用只有1G,
请描述思想，写出算发（c语言），空间和时间复杂度，
7.国内的一些帖吧，如baidu,有几十万个主题，假设每一个主题都有上亿的跟帖子，怎么样设计这个系统速度最好，请描述思想，写出算发（c语言），空间和时间复杂度，
#include   string.h
main(void)
{   char   *src="hello,world";
    char   *dest=NULL;
    dest=(char   *)malloc(strlen(src));
    int   len=strlen(str);
    char   *d=dest;
    char   *s=src[len];
    while(len--!=0)
      d++=s--;
    printf("%s",dest);
}
找出错误！！
#include   "string.h"
#include "stdio.h"
#include "malloc.h"
main(void)
{  
char   *src="hello,world";
    char   *dest=NULL;
    dest=(char   *)malloc(sizeof(char)*(strlen(src)+1));
    int   len=strlen(src);
    char   *d=dest;
    char   *s=src+len-1;
    while(len--!=0)
      *d++=*s--;
*d='\0';
    printf("%s",dest);
}
105.判断字符串是否为回文
bool IsSymmetry(const char* p)  
     {  
         assert(p!=NULL);  
         const char* q=p;        
         int len=0;  
         while(*q++!='\0')  
         {  
              len++;  
         }         
         bool bSign=true;  
         q=p+len-1;  
         if (0<len)  
         {  
              for (int i=0;i<len/2;i++)  
              {  
                   if(*p++!=*q--){ bSign=false;break;};  
              }  
         }  
         if(bSign==true)  
         {  
              printf("Yes!\n");  
         }  
         else  
         {  
              printf("No!\n");  
         }  
         return bSign;  
     }  
107.ASDL使用的是什么协议？并进行简单描述？
  什么是ADSL? adsl介绍 
　　　ADSL是非对称数字用户线路（Asymmetric Digital Subscriber Line）的缩写，有时也作非对称数字用户环路（Asymmetric Digital Subscriber Loop）。它是一种在电话铜缆上进行较高速率数据传输的方法，是DSL 的一种形式。它以普通电话线路做为传输介质，既在普通双绞铜线上实现下行高达8Mbit/s传输速度；上行高达640Kbit/s的传输速度，我们只要在普通线路两端加装ADSL设备，既可使用ADSL提供的高带宽服务，通过一条电话线，便可以比普通MODEM快一百倍速度。　(看到这你也许会问:什么是DSL?下面我们来介绍DSL)
　　DSL（数字用户线路）
　　　DSL（数字用户线路，Digital Subscriber Line）是以铜质电话线为传输介质的传输技术组合，它包括HDSL、SDSL 、VDSL 、ADSL和RADSL等，一般称之为xDSL。它们主要的区别就是体现在信号传输速度和距离的不同以及上行速率和下行速率对称性的不同这两个方面。
　　　HDSL与SDSL支持对称的T1/E1（1.544Mbps/2.048Mbps）传输。其中HDSL的有效传输距离为3－4公里，且需要两至四对铜质双绞电话线；SDSL最大有效传输距离为3公里，只需一对铜线。比较而言，对称DSL更适用于企业点对点连接应用，如文件传输、视频会议等收发数据量大致相应的工作。同非对称DSL相比，对称DSL的市场要少得多。
　　　VDSL、ADSL和RADSL属于非对称式传输。其中VDSL技术是xDSL技术中最快的一种，在一对铜质双绞电话线上，上行数据的速率为13到52Mbps，下行数据的速率为1.5到2.3 Mbps，但是VDSL的传输距离只在几百米以内，VDSL可以成为光纤到家庭的具有高性价比的替代方案，目前深圳的VOD（Video on demand）就是采用这种接入技术实现的；ADSL 在一对铜线上支持上行速率640Kbps到1Mbps，下行速率1Mbps到8Mbps，有效传输距离在3－5公里范围以内；RADSL能够提供的速度范围与ADSL基本相同，但它可以根据双绞铜线质量的优劣和传输距离的远近动态地调整用户的访问速度。正是RADSL的这些特点使RADSL成为用于网上高速冲浪、视频点播（IAV）、远程局域网络（LAN）访问的理想技术，因为在这些应用中用户下载的信息往往比上载的信息（发送指令）要多得多。
　　ADSL的原理
　　　传统的电话系统使用的是铜线的低频部分（4kHz以下频段）。而ADSL采用DMT（离散多音频）技术，将原先电话线路20kHz到1.1MHz频段划分成 256个频宽为4.3kHz的子频带。其中，4kHz以下频段仍用于传送POTS（传统电话业务），20kHz到138kHz的频段用来传送上行信号， 138kHz到1.1MHz的频段用来传送下行信号。DMT技术可根据线路的情况调整在每个信道上所调制的比特数，以便更充分地利用线路。一般来说，子信道的信噪比越大，在该信道上调制的比特数越多。如果某个子信道的信噪比很差，则弃之不用。
　　　ADSL在调制方式上采用离散多音复用技术（DMT），在DMT技术中，一对铜线上0~4Khz用来传输电话音频，用20Khz~1.1Mhz频段传数据,并把它以4Khz的宽度划分为25个上行子通道和249个下行子通道，输入的数据经过TCM编码及QAM调制后，送往子信道，所以理论上上行速率可达 1.5Mbps, 下行速率可达14.9Mbps,考虑到干扰等情况，实际上传输速率一般为上行640Kbps,下行8Mbps。当我们用 ADSL上网的时侯，ADSL MODEM便产生了三个信息通道：一个为标准电话通道、一个为640Kbps~1Mbps上行通道、还有一个为1Mbps~8Mbps高速下行通道。实现方法是：经ADSL MODEM编码后的信号通过电话线传到数据机房后经过一个分离器如果是语音信号就传到程控机房，是数据信号就留在数据设备上，最后接入INTERNET 。
　　　由上可看到，对于原先的电话信号而言，仍使用原先的频带，而基于ADSL的业务，使用的是话音以外的频带。所以，原先的电话业务不受任何影响。
　　ADSL接入类型
　　　专线入网方式：用户拥有固定的静态IP地址，24小时在线。
　　　虚拟拨号入网方式：并非是真正的电话拨号，而是用户输入帐号、密码，通过身份验证，获得一个动态的IP地址。
108.Static 作用是什么
首先static的最主要功能是隐藏，其次因为static变量存放在静态存储区，所以它具备持久性和默认值0。
109.什么是预编译,何时需要预编译?
预编译又称为预处理,是做些代码文本的替换工作。处理#开头的指令,比如拷贝#include包含的文件代码，#define宏定义的替换,条件编译等，就是为编译做的预备工作的阶段，主要处理#开始的预编译指令，预编译指令指示了在程序正式编译前就由编译器进行的操作，可以放在程序中的任何位置。
c编译系统在对程序进行通常的编译之前，先进行预处理。c提供的预处理功能主要有以下三种：1）宏定义　2）文件包含　3）条件编译
１、总是使用不经常改动的大型代码体。 
２、程序由多个模块组成，所有模块都使用一组标准的包含文件和相同的编译选项。在这种情况下，可以将所有包含文件预编译为一个预编译头。
110.进程和线程的区别
什么是进程（Process）：普通的解释就是，进程是程序的一次执行，而什么是线程（Thread），线程可以理解为进程中的执行的一段程序片段。在一个多任务环境中下面的概念可以帮助我们理解两者间的差别：

进程间是独立的，这表现在内存空间，上下文环境；线程运行在进程空间内。 一般来讲（不使用特殊技术）进程是无法突破进程边界存取其他进程内的存储空间；而线程由于处于进程空间内，所以同一进程所产生的线程共享同一内存空间。同一进程中的两段代码不能够同时执行，除非引入线程。线程是属于进程的，当进程退出时该进程所产生的线程都会被强制退出并清除。线程占用的资源要少于进程所占用的资源。 进程和线程都可以有优先级。在线程系统中进程也是一个线程。可以将进程理解为一个程序的第一个线程。

线程是指进程内的一个执行单元,也是进程内的可调度实体.与进程的区别:
(1)地址空间:进程内的一个执行单元;进程至少有一个线程;它们共享进程的地址空间;而进程有自己独立的地址空间;
(2)进程是资源分配和拥有的单位,同一个进程内的线程共享进程的资源
(3)线程是处理器调度的基本单位,但进程不是.
(4)二者均可并发执行.
111.插入排序和选择排序
插入排序基本思想：（假定从大到小排序）依次从后面拿一个数和前面已经排好序的数进行比较，比较的过程是从已经排好序的数中最后一个数开始比较，如果比这个数，继续往前面比较，直到找到比它大的数，然后就放在它的后面，如果一直没有找到，肯定这个数已经比较到了第一个数，那就放到第一个数的前面。那么一般情况下，对于采用插入排序法去排序的一组数，可以先选 取第一个数做为已经排好序的一组数。然后把第二个放到正确位置。
选择排序(Selection Sort)是一种简单直观的排序算法。它的工作原理如下。首先在未排序序列中找到最小元素,存放到排序序列的起始位置,然后,再从剩余未排序元素中继续寻找最小元素,然后放到排序序列末尾。以此类推,直到所有元素均排序完毕。
112.运算符优先级问题
能正确表示a和b同时为正或同时为负的逻辑表达式是(D )。
A、(a>=0||b>=0)＆＆(a<0||b<0) 
B、(a>=0＆＆b>=0)&&(a<0&＆b<0) 
C、(a+b>0)＆＆(a+b<=0) 
D、a*b>0
以下关于运算符优先顺序的描述中正确的是(C)。 
A、关系运算符<算术运算符<赋值运算符<逻辑与运算符 
B、逻辑与运算符<关系运算符<算术运算符<赋值运算符 
C、赋值运算符<逻辑与运算符<关系运算符<算术运算符 
D、算术运算符<关系运算符<赋值运算符<逻辑与运算符
113.字符串倒序
写一个函数将"tom is cat" 倒序打印出来，即 "cat is tom"
//a.ch
#define SPACE ' '
#define ENDL '\0'

char* str = "Tom is cat"; // 字符串
char* p1 = str+strlen(str)-1;
char* p2 = p1; // 开始时，p1,p2都指向字符串结尾处
char t=0; // 临时变量，用来保存被临时替换为ENDL的字符
while(str!=p1--)
{
  if(SPACE!=*p1){
     for(p2=p1+1;SPACE!=*p1; p1--, t=*p2, *p2=ENDL);

     // p1+1指向单词的第一个字母,p2指向单词的结尾,此时输出这个单词
                printf("%s ",p1+1);
                *p2=t;
                p2=p1;
         }
}
Output:
cat is Tom
----------------------------------------------------------------------
1)写一个递归函数将内存中的字符串翻转"abc"->"cba"
2)写一个函数将"tom is cat" 将内存中的字符串翻转，即 "cat is tomm" 

#include <stdio.h>
#define SPACE ' '
#define ENDL '\0'
char* s = "The quick brown fox jumps over the lazy dog";
void str_reverse(char* p1,char* p2){
      if(p1==p2)return;
    *p1 = (*p1)+(*p2);
    *p2 = (*p1)-(*p2);
    *p1 = (*p1)-(*p2);
      if(p1==p2-1)return;
      else str_reverse(++p1,--p2);
}
void str_word_reverse(char* str){
      char *q1=str, *q2=str, *t;
      while(*q1==SPACE)q1++;
      if(*q1==ENDL)return; //!
      else q2=q1+1;
      while( (*q2!=SPACE) && (*q2!=ENDL) )q2++;
    
     t=q2--; 
    str_reverse(q1,q2);
      if(*t==ENDL)return;
      else str_word_reverse(t);
}
int
main(int a ,char** b)
{
    printf("%s\n",s);
    str_reverse(s,s+strlen(s)-1);
    printf("%s\n",s);
    str_word_reverse(s);
    printf("%s\n",s);
           return 0;
}
Output:
The quick brown fox jumps over the lazy dog
god yzal eht revo spmuj xof nworb kciuq ehT
dog lazy the over jumps fox brown quick The
----------------------------------------------------------------------
今天同学又问一道题,和上面有些类似，但是要求更严格了一些：
写一个递归函数将内存中的字符串翻转"abc"->"cba",并且函数原型已确定：void reverse(char* p)

其实，要求越多，思路越确定，我的解如下：

#include <stdio.h>
#include <string.h>
char* s = "0123456789";
#define ENDL '\0'
void reverse(char* p){
       //这是这种方法的关键，使用static为的是能用str_reverse的思路，但是不好
       static char* x=0;
       if(x==0)x=p;
       char* q = x+strlen(p)-1; 
       if(p==q)return;
       *q=(*p)^(*q);
       *p=(*p)^(*q);
       *q =(*p)^(*q);
       if(q==p+1)return;
       reverse(++p);
}

//这种方法就直观多了，但是当字符串很长的时候就很低效
void reverse2(char* p){
       if(*(p+1)==ENDL)return;
       for(char* o=p+strlen(p)-1,char t=*o;o!=p;o--)
          *o=*(o-1);
       *p=t;
       reverse2(p+1);
}
int main(int c,char** argv){
       reverse2(s);
       printf("%s\n",s);
       return 0;
}
114.交换两个数的宏定义
交换两个参数值的宏定义为：. #define SWAP(a,b) (a)=(a)+(b);(b)=(a)-(b);(a)=(a)-(b);
115.Itearator各指针的区别
游标和指针
我说过游标是指针，但不仅仅是指针。游标和指针很像，功能很像指针，但是实际上，游标是通过重载一元的”*”和”->”来从容器中间接地返回一个值。将这些值存储在容器中并不是一个好主意，因为每当一个新值添加到容器中或者有一个值从容器中删除，这些值就会失效。在某种程度上，游标可以看作是句柄（handle）。通常情况下游标（iterator）的类型可以有所变化，这样容器也会有几种不同方式的转变：

iterator——对于除了vector以外的其他任何容器，你可以通过这种游标在一次操作中在容器中朝向前的方向走一步。这意味着对于这种游标你只能使用“++”操作符。而不能使用“--”或“+=”操作符。而对于vector这一种容器，你可以使用“+=”、“—”、“++”、“-=”中的任何一种操作符和“<”、“<=”、“>”、“>=”、“==”、“!=”等比较运算符。
116. C++中的class和struct的区别
从语法上，在C++中（只讨论C++中）。class和struct做类型定义时只有两点区别：
（一）默认继承权限。如果不明确指定，来自class的继承按照private继承处理，来自struct的继承按照public继承处理；
（二）成员的默认访问权限。class的成员默认是private权限，struct默认是public权限。
除了这两点，class和struct基本就是一个东西。语法上没有任何其它区别。
不能因为学过C就总觉得连C++中struct和class都区别很大，下面列举的说明可能比较无聊，因为struct和class本来就是基本一样的东西，无需多说。但这些说明可能有助于澄清一些常见的关于struct和class的错误认识：
（1）都可以有成员函数；包括各类构造函数，析构函数，重载的运算符，友元类，友元结构，友元函数，虚函数，纯虚函数，静态函数；
（2）都可以有一大堆public/private/protected修饰符在里边；
（3）虽然这种风格不再被提倡，但语法上二者都可以使用大括号的方式初始化：
A a = {1, 2, 3};不管A是个struct还是个class，前提是这个类/结构足够简单，比如所有的成员都是public的，所有的成员都是简单类型，没有显式声明的构造函数。
（4）都可以进行复杂的继承甚至多重继承，一个struct可以继承自一个class，反之亦可；一个struct可以同时继承5个class和5个struct，虽然这样做不太好。
（5）如果说class的设计需要注意OO的原则和风格，那么没任何理由说设计struct就不需要注意。
（6）再次说明，以上所有说法都是指在C++语言中，至于在C里的情况，C里是根本没有“class”，而C的struct从根本上也只是个包装数据的语法机制。
---------------------------------------------------------------
最后，作为语言的两个关键字，除去定义类型时有上述区别之外，另外还有一点点：“class”这个关键字还用于定义模板参数，就像“typename”。但关键字“struct”不用于定义模板参数。

关于使用大括号初始化
　　class和struct如果定义了构造函数的话，都不能用大括号进行初始化
　　如果没有定义构造函数，struct可以用大括号初始化。
　　如果没有定义构造函数，且所有成员变量全是public的话，可以用大括号初始化。
　　关于默认访问权限
　　class中默认的成员访问权限是private的，而struct中则是public的。
　　关于继承方式
　　class继承默认是private继承，而struct继承默认是public继承。
　　关于模版

　　在模版中，类型参数前面可以使用class或typename，如果使用struct，则含义不同，struct后面跟的是“non-type template parameter”，而class或typename后面跟的是类型参数。

class中有个默认的this指针，struct没有
不同点：构造函数，析构函数 this 指针
117.有关重载函数
返回值类型不同构不成重载 
参数参数顺序不同能构成重载
c++函数同名不同返回值不算重载！函数重载是忽略返回值类型的。 
成员函数被重载的特征有： 
1) 相同的范围（在同一个类中）； 
2) 函数名字相同； 
3) 参数不同； 
4) virtual关键字可有可无。
5) 成员函数中 有无const (函数后面) 也可判断是否重载
118.数据库与T-SQL语言
关系数据库是表的集合，它是由一个或多个关系模式定义。SQL语言中的数据定义功能包括对数据库、基本表、视图、索引的定义。
<span style="background-color: rgb(241, 254, 221); ">SQL全称是“</span><span style="background-color: rgb(51, 255, 51); ">结构化查询语言(Structured Query Language)</span><span style="background-color: rgb(241, 254, 221); ">”
SQL(Structured Query Language)是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。

参考网址：
</span><a target=_blank target="_blank" href="http://baike.baidu.com/view/34.html?wtp=tt" rel="nofollow" style="color: rgb(45, 100, 179); text-decoration: none; background-color: rgb(241, 254, 221); ">http://baike.baidu.com/view/34.html?wtp=tt</a><span style="background-color: rgb(241, 254, 221); ">
***********************************************
T-SQL是标准 SQL 程式设计语言的增强版，它是用来让<strong>应用程式与 SQL Server 沟通</strong>的主要语言。T-SQL 提供标准 SQL的DDL 和 DML 功能，加上延伸的函数、系统预存程序以及程式设计结构(例如 IF 和 WHILE)让程式设计更有弹性。

参考网址：
</span><a target=_blank target="_blank" href="http://baike.baidu.com/view/1387454.html?wtp=tt" rel="nofollow" style="color: rgb(45, 100, 179); text-decoration: none; background-color: rgb(241, 254, 221); ">http://baike.baidu.com/view/1387454.html?wtp=tt</a><span style="background-color: rgb(241, 254, 221); ">

*****************
补充：
你<strong><u>可以理解成T-sql是SqlServer支持的sql语法</u></strong>，他不是软件，装了SqlServer就ok了。</span>

119.关系模型的基本概念
 关系数据库以关系模型为基础，它有以下三部分组成：
    ●数据结构——模型所操作的对象、类型的集合
    ●完整性规则——保证数据有效、正确的约束条件
    ●数据操作——对模型对象所允许执行的操作方式
    关系（Relation）是一个由行和列组成的二维表格，表中的每一行是一条记录（Record），每一列是记录的一个字段（Field）。表中的每一条记录必须是互斥的，字段的值必须具有原子性。
120.SQL语言概述
    SQL（结构化查询语言）是关系数据库语言的一种国际标准，它是一种非过程化的语言。通过编写SQL，我们可以实现对关系数据库的全部操作。
    ●数据定义语言（DDL）——建立和管理数据库对象
    ●数据操纵语言（DML）——用来查询与更新数据
    ●数据控制语言（DCL）——控制数据的安全性
听起来是一个很简单的问题，每一个使用过RDBMS的人都会有一个概念。
事务处理系统的典型特点是具备ACID特征。ACID指的是Atomic（原子的）、Consistent（一致的）、Isolated（隔离的）以及Durable（持续的），它们代表着事务处理应该具备的四个特征：
原子性：组成事务处理的语句形成了一个逻辑单元，不能只执行其中的一部分
一致性：在事务处理执行之前和之后，数据是一致的。
隔离性：一个事务处理对另一个事务处理没有影响。
持续性：当事务处理成功执行到结束的时候，其效果在数据库中被永久纪录下来。
121.C语言中结构化程序设计的三种基本控制结构
顺序结构 
选择结构 
循环结构
122.CVS是什么
cvs（Concurrent Version System） 是一个版本控制系统。使用它，可以记录下你的源文件的历史。 
例如，修改软件时可能会不知不觉混进一些 bug，而且可能过了很久你才会察觉到它们的存在。有了 cvs，你可以很容易地恢复旧版本，并从中看出到底是哪个修改导致了这个 bug。有时这是很有用的。
CVS服务器端对每个文件维护着一个修订号,每次对文件的更新，都会使得文件的修订号加1。在客户端中也对每个文件维护着一个修订号,CVS通过这两个修订号的关系，来进行Update,Commit和发现冲突等操作操作
123.三种基本的数据模型
按照数据结构类型的不同，将数据模型划分为层次模型、网状模型和关系模型。
<strong>层次模型</strong>
    层次模型是以记录类型为结点的<strong>树型结构</strong>，下层记录是上层记录中某元素的细化。
    层次模型的记录类型间只有简单的层次关系，且满足以下条件：<u>有一个记录类型没有父结点；其他记录类型有且只有一个父结点</u>。
<strong>网状模型</strong>
    <u>有一个以上记录类型没有父结点；至少有一个记录类型多于一个父结点</u>。用这种网络结构表示记录类型之间联系的模型称为网状模型。
<strong>关系模型</strong>
    关系模型的基本思想是把事物与事物之间的<u>联系用二维表格的形式描述。</u>一个关系可以看作一个<u>二维表，表中每一行是一个记录，每一列是一个字段。</u>
    关系模型可用离散数学中的关系代数来描述，因而关系数据库管理系统能够用严格的数学理论来描述数据库的组织和操作，且具有简单灵活、数据独立性高等特点，应用十分广泛。
124.设计模式：工厂模式 和 单例模式 介绍一下？
工厂模式即将对象创建过程封装即为工厂模式。
单例模式即整个类只有一个对象，并且不允许显示创建。
单例模式

      单例模式保证一个类仅有一个实例，并提供一个访问它的全局访问点。通常我们可以让一个全局变量使得一个对象被访问，但它不能阻止你实例化多个对象。一个最好的办法是，让类自身负责保存它的唯一实例。这个类可以保证没有其他实例可以被创建，并且它可以提供一个访问该实例的方法。
     
      也就是说，很多时候我们需要全局的对象，如一个工程中，数据库访问对象只有一个，这时，可以考虑使用单例模式。单例模式比全局对象好还包括：单例类可以继承，如下例中的C++代码。
     
     单例模式的关键点在于：构造函数私有，静态的GetInstance。
     
     另外，在C++中必须注意内存的释放。C++、Java、C#中还要注意多线程时的同步问题，另外在多线程可以以合适的方式保证共享变量仅初始化一次

以下列出懒汉式的单例模式

C++实现：

复制代码
class Singleton  
{  
  public:  
    static Singleton * GetInstance()  
    {  
        if(NULL == m_pInstance)  
            m_pInstance = new Singleton();  
        return m_pInstance;  
    }  
    static void Release()                    //必须，否则会导致内存泄露   
    {  
        if(NULL != m_pInstance)  
        {  
            delete m_pInstance;  
            m_pInstance = NULL;  
        }  
    }  
      
   protected:  
     Singleton()  
     {  
         cout<<"C++ Singleton"<<endl;  
     };  
     static Singleton * m_pInstance;  
}; 

Singleton* Singleton::m_pInstance = NULL;  

class SingleDraw:public Singleton  
{  
public:  
    static SingleDraw* GetInstance()  
    {  
        if(NULL == m_pInstance)  
            m_pInstance = new SingleDraw();  
        return (SingleDraw*)m_pInstance;  
    }  
protected:  
    SingleDraw()  
    {  
        cout<<"C++ SingleDraw"<<endl;  
    }  
};  

int main()  
{  
    SingleDraw* s1 = SingleDraw::GetInstance();  
    SingleDraw* s2 = SingleDraw::GetInstance();  
    s2->Release();  
    return 0;  
}  
工厂模式

首先需要说一下工厂模式。工厂模式根据抽象程度的不同分为三种：简单工厂模式（也叫静态工厂模式）、这里所讲述的工厂方法模式、以及抽象工厂模式。工厂模式是编程中经常用到的一种模式。它的主要优点有：

可以使代码结构清晰，有效地封装变化。在编程中，产品类的实例化有时候是比较复杂和多变的，通过工厂模式，将产品的实例化封装起来，使得调用者根本无需关心产品的实例化过程，只需依赖工厂即可得到自己想要的产品。
对调用者屏蔽具体的产品类。如果使用工厂模式，调用者只关心产品的接口就可以了，至于具体的实现，调用者根本无需关心。即使变更了具体的实现，对调用者来说没有任何影响。
降低耦合度。产品类的实例化通常来说是很复杂的，它需要依赖很多的类，而这些类对于调用者来说根本无需知道，如果使用了工厂方法，我们需要做的仅仅是实例化好产品类，然后交给调用者使用。对调用者来说，产品所依赖的类都是透明的。
C++实现：

复制代码
#include <iostream>

using namespace std;

class IProduct
{
public:
    virtual void productMethod() = 0;
};


class Product:public IProduct
{
public:
    void productMethod() 
    {  
        cout<<"C++ productMethod call"<<endl;  
    }  
};


class IFactory
{
public:
    virtual IProduct* CreateProduct() = 0;
};


class ConcreateFactory:public IFactory
{
public:
   IProduct* CreateProduct()
   {
        IProduct* p = new Product();
        return p;
   }
};

int main()
{
   IFactory* pFactory = new ConcreateFactory();

   IProduct* p = pFactory->CreateProduct();

   p->productMethod();

   delete p;

   return 1;
}
reference:http://www.cnblogs.com/biyeymyhjob/archive/2012/07/19/2599136.html
125.vector 和 list的区别？
vector内部使用数组，访问速度快，但是删除数据比较耗性能
list内部使用链表，访问速度慢，但是删除数据比较快
126.纯虚函数是怎样实现的？在编译原理上讲一下？
在类内部添加一个虚拟函数表指针，该指针指向一个虚拟函数表，该虚拟函数表包含了所有的虚拟函数的入口地址，每个类的虚拟函数表都不
一样，在运行阶段可以循此脉络找到自己的函数入口。
纯虚函数相当于占位符，先在虚函数表中占一个位置，由派生类实现后再把真正的函数指针填进去。除此之外和普通的虚函数没什么区别。
127.抽象类为什么不能实例化？
抽象类中的纯虚函数没有具体的实现，所以没办法实例化。
128.在函数后面加个const是怎么理解的？
在函数后面加个const一般在类的成员函数中使用，表示这个函数不修改数据成员的值。
另：void set_prt_val(int val)const {*ptr= val}理解：指针ptr指向的内容不是类的数据成员，所以这可这么写：*ptr = val；但这个指针在这个函数中不能修改。如果写成这样：ptr = &i（假设i是另外一个整形变量）就不对了，因为改变了指针的内容。
129.进程间通信类型
（1）环境变量、文件描述符 一般Unix环境下的父进程执行fork（），生成的子进程拥有了父进程当前设置的环境变量以及文件描述符；由于通信是一个单向的、一次性的通信，随后的父进程以及子进程后续的内容不能再能共享；
（2）命令行参数 大多数用户都使用过ShellExec相关的命令，此API可以打开新的进程，并可以通过接口里的输入参数进行信息共享；同样，他也是一个单项、一次性的通信；
（3）管道 使用文件和写方式访问公用的数据结构；管道分为匿名管道和命名管道，前者是用作关联进程间用，后者为无关联的进程使用；前者通过文件描述符或文件句柄提供对命名管道的访问，后者需要知道管道名称才能读写管道；一般来讲，读写的内容是字节流，需要转换为有意义的结构才有意义；
（4）共享内存 进程需要可以被其他进程访问浏览的进程块；进程间共享内存的关系与函数间共享全局变量的关系类似
（5）DDE 动态数据交互
线程间通信类型：
（1）全局数据；
（2）全局变量；
（3）全局数据结构；
（4）线程间通信的参数：pThread_create这类API接口中的参数
130.关于内存对齐的问题以及sizof()的输出
答：编译器自动对齐的原因：为了提高程序的性能，数据结构（尤其是栈）应该尽可能地在自然边界上对齐。原因在于，为了访问未对齐的内存，处理器需要作两次内存访问；然而，对齐的内存访问仅需要一次访问。
131.winsock建立连接的主要实现步骤?
TCP:服务器端：1.socket()建立套接字，2将套接字绑定到本地地址和端口上，绑定（bind）3.将套接字设为监听模式，准备接收客户端，监听（listen）;4.等待客户端请求到来，请求到来后，连接请求，并返回一个新的对应此连接的套接字，accept（）5.用返回的套接字和客户端进行通讯（send/recv）;6.返回并等待另一客户请求。7.关闭套接字。
客户端：1.socket()建立套接字2.向服务器发出连接请求，（connect）2。和服务器进行通信，send()和recv（）,在套接字上写读数据，直至数据交换完毕;4closesocket()关闭套接字。
UDP:1服务器端：1.创建套接字（socekt）2.将套接字绑定到本地地址和端口上（bind);3.等待接收数据（recvfrom）;4.closesocket()关闭套接字。
客户端:1.创建套接字（socekt）2,向服务器端发送数据（sendto）3.closesocket()关闭套接字。
132.C++中为什么用模板类。
  (1） 可用来创建动态增长和减小的数据结构
（2）它是类型无关的，因此具有很高的可复用性。
（3）它在编译时而不是运行时检查数据类型，保证了类型安全
（4）它是平台无关的，可移植性
（5）可用于基本数据类型
133.动态连接库的两种方式?
动态链接库英文为DLL，是Dynamic Link Library 的缩写形式，DLL是一个包含可由多个程序同时使用的代码和数据的库，DLL不是可执行文件。动态链接提供了一种方法，使进程可以调用不属于其可执行代码的函数。函数的可执行代码位于一个 DLL 中，该 DLL 包含一个或多个已被编译、链接并与使用它们的进程分开存储的函数。DLL 还有助于共享数据和资源。多个应用程序可同时访问内存中单个DLL 副本的内容。DLL 是一个包含可由多个程序同时使用的代码和数据的库。
答：调用一个DLL中的函数有两种方法：
1．载入时动态链接（load-time dynamic linking），模块非常明确调用某个导出函数，使得他们就像本地函数一样。这需要链接时链接那些函数所在DLL的导入库，导入库向系统提供了载入DLL时所需的信息及DLL函数定位。
2．运行时动态链接（run-time dynamic linking），运行时可以通过LoadLibrary或LoadLibraryEx函数载入DLL。DLL载入后，模块可以通过调用GetProcAddress获取DLL函数的出口地址，然后就可以通过返回的函数指针调用DLL函数了。如此即可避免导入库文件了。
reference：http://blog.sina.com.cn/s/blog_725dd1010100ug2z.html
深入了解：http://www.cnblogs.com/chio/archive/2007/11/03/948480.html
134.CSingleLock是干什么的。
答：同步多个线程对一个数据类的同时访问
CSingleLock没有基类。
一个CSingleLock类对象代表一种访问控制机制，这种机制用于控制在一个多线程程序中对一个资源的访问。为了使用同步类CSemaphore，CMutex，CCriticalSection，和CEvent。
你必须创建一个CSingleLock或CMultiLock对象来等待和释放这个同步对象。当你只需要每次等待一个对象时，可以使用CSingleLock。当在一个特别的时候你可以使用多个对象时，可以使用CMultiLock。
要使用一个CSingleLock对象，在被控制资源的类中的一个成员函数内部调用CSingleLock的构造函数。然后调用ISLock成员函数来确定这个资源是否可用。如果资源是可用的，则继续该成员函数的其余部分。如果资源不能使用，可以在一个指定的时间内等待资源被释放，或者是返回失败。在使用完资源后，如果CSingleLock对象要被再次使用，可以调用Unlock函数，或者销毁CSingleLock对象。
CSingleLock对象需要有一个从CSyncObject派生的对象存在。这通常是一个被控制资源的类的数据成员。
#include 
请参阅：CMultiLock
类成员 CSingleLock类成员
构造 CSingleLock 构造一个CSingleLock对象 
方法 IsLocked 确定此对象是否被加锁 
Lock 等待一个同步对象 
Unlock 释放一个同步对象CSingleLock构造函数 CSingleLock::CSingleLock
CSingleLock( CSyncObject* pObject, BOOL bInitialLock = FALSE );
参数： pObject 指向要被访问的同步对象。不能是NULL。 
bInitialLock 指示是否要在最初尝试访问所提供的对象。 
说明：
此成员函数用来构造一个CSingleLock对象。通常是从被控制资源的一个访问成员函数中来调用这个函数。
reference：
http://hi.baidu.com/hooplee/item/d19228ce2edcf00fc710b2ac
http://msdn.microsoft.com/zh-cn/library/fw63hszf.aspx
 135.编写strcat函数(6分)
已知strcat函数的原型是char *strcat (char *strDest, const char *strSrc);
其中strDest 是目的字符串，strSrc 是源字符串。
（1）不调用C++/C 的字符串库函数，请编写函数 strcat
答：
VC源码：
char * __cdecl strcat (char * dst, const char * src)
{
char * cp = dst;
while( *cp )
cp++; /* find end of dst */
while( *cp++ = *src++ ) ; /* Copy src to end of dst */
return( dst ); /* return dst */
}
（2）strcat能把strSrc 的内容连接到strDest，为什么还要char * 类型的返回值？
答：方便赋值给其他变量
136.编写类String 的构造函数、析构函数和赋值函数（25 分）
已知类String 的原型为：
class String  
{  
public:  
String(const char *str = NULL); // 普通构造函数  
String(const String &other); // 拷贝构造函数  
~ String(void); // 析构函数  
String & operate =(const String &other); // 赋值函数  
private:  
char *m_data; // 用于保存字符串  
};  
请编写String 的上述4 个函数。
标准答案：
// String 的析构函数  
String::~String(void) // 3 分  
{  
delete [] m_data;  
// 由于m_data 是内部数据类型，也可以写成 delete m_data;  
}  
// String 的普通构造函数  
String::String(const char *str) // 6 分  
{  
if(str==NULL)  
{  
m_data = new char[1]; // 若能加 NULL 判断则更好  
*m_data = ‘\0’;  
}  
else  
{  
int length = strlen(str);  
m_data = new char[length+1]; // 若能加 NULL 判断则更好  
strcpy(m_data, str);  
}  
}  
// 拷贝构造函数  
String::String(const String &other) // 3 分  
{  
int length = strlen(other.m_data);  
m_data = new char[length+1]; // 若能加 NULL 判断则更好  
strcpy(m_data, other.m_data);  
}  
// 赋值函数  
String & String::operate =(const String &other) // 13 分  
{  
// (1) 检查自赋值 // 4 分  
if(this == &other)  
return *this;  
// (2) 释放原有的内存资源 // 3 分  
delete [] m_data;  
// （3）分配新的内存资源，并复制内容 // 3 分  
int length = strlen(other.m_data);  
m_data = new char[length+1]; // 若能加 NULL 判断则更好  
strcpy(m_data, other.m_data);  
// （4）返回本对象的引用 // 3 分  
return *this;  
}  