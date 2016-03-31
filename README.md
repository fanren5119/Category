# Category
Category是Objective-C2.0之后添加的新特性，Category是一种为现有类添加新方法的方式
，利用Objective-C动态运行时分配机制，Category提供了一种比继承更简洁的方式来对class
进行扩展，无需创建子类就能为现有的类添加新的方法，可以为任何已经存在class添加方法
包括哪些没有源代码的类（如某些框架类）。
##1.Category的三大优点
        ① 分散类的实现：可以把类的实现分散到几个文件中；
        ② 声明私有方法；
        ③ 重载原始类的方法。
##2.运行期的Category
        runtime层的Category是用结构体category_t表示的，他包含了：
        ① 类的名字（name）；
        ② 类（cls）；
        ③ category中所有添加的实例方法（instanceMethod）；
        ④ category中所有添加的类方法的列表（classMethod）；
        ⑤ category中实现的所有协议的列表（protocols）；
        ⑥ category中所有的添加的属性（instanceProperties）。
        从category的定义可看出category的行为，可以添加实例方法，类方法，实现协议
    和添加属性，不可以添加实例变量
##3.Category如何加载
        ① 把Category中的实例方法、协议和属性添加到类上；
        ② 把Category中的类方法添加到类的metaClass上；
        ③ Category并没有完全替换掉原有类的方法，也就是说，如果Category和原有类都有
    methodA，那么Category附加完成后，会有两个methodA方法；
        ④ Category的方法放到了新方法列表的前边，运行时，在查找方法的时候，是顺着方
    法列表找的，一旦找到了就会停止，所有就造成了方法的覆盖。
##4.为何Category不能添加实例变量。
        ① Category是运行期决议的
        ② 编译后的类已经注册到runtime中，类结构中的objc_ivar_list实例变量的链表和
    instance_size实例变量的内存大小已经确定，同时runtime会调用class_setIvarLayout
    或者class_setWeadIvarLayout来处理strong、weak引用，所以不能向存在的类中添加实
    例变量。
##5.Category增加属性
        使用关联对象
            objc_setAssociatedObject
            objc_getAssociatedObject
##6.关联对象的管理
        所有的关联对象都是由AssociationosManager来管理的
        AssociationManager里有一个静态的AssociationsHashMap来存储所有关联对象的；
    这就相当于把所有的关联对象都存到一个全局的map里面，而这个map的key是这个对象
    的指针地址，而这个map的value是另一个AssociationsHashMap，里面保存了关联对象
    的key-value对。
