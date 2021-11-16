# static

![image-20210611162431200](C:\Users\10858\AppData\Roaming\Typora\typora-user-images\image-20210611162431200.png)

```c++
double real() const {return this->re;}	// this-> 可写可不写
```

## （1）static data members

1. 例如：银行的利率。
2. 在类外定义。

## （2）static member functions

1. 没有this指针！
2. 用于处理static 数据。

## （3）例子

![image-20210611162923172](C:\Users\10858\AppData\Roaming\Typora\typora-user-images\image-20210611162923172.png)

## (4) 单例Singleton

### 不完美单例

![image-20210611163324589](C:\Users\10858\AppData\Roaming\Typora\typora-user-images\image-20210611163324589.png)

  1. private：定义 static 类对象 a。

  2. 所有构造在private，外部无法创建对象，只能通过 static 的 getInstance 返回该类的唯一对象。

     // A::getInstance().setup();

		3. 不完美：如果外界不需要用到，那么对象a仍然存在，有点浪费！

     ### 完美单例

![image-20210611164132933](C:\Users\10858\AppData\Roaming\Typora\typora-user-images\image-20210611164132933.png)

