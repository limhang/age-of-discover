近期，做组件化，敏捷开发，需要做很多的封装和集成，也真正正正的又好好写了一遍oc代码，嗯嗯，非常认真，认真的目的是，我想这是我最后一次这么认真写了，以后就是玩logo积木了，这个过程中，也引发了我的量变到质变，我这过去一年学习（未深入）太多技术，接触了太多思想（表面），我对iOS和软件有了更高的认识。这一年外语的不间断学习，前端，移动端，后台，网络，网络安全，算法数据结构，编译，爬虫，长期的ic硬件积累，终于顿悟了。以后的目标，就是详尽的记录工作日常，学一门技术，就弄完一门，不一道错题，做2遍，高中数学的小段子。


runtime这个问题，我看过很多资料，但是今天我终于搞懂它了
1.入门须知
关于属性和方法
一个软件（可交互界面），代码的实现，主要分2类，一类是数据，一类是内部逻辑，下面将大量使用【数据】【内部逻辑】这2个我自创的词语。

数据：组件，数值
内部逻辑：组件变换，数值调整
是不是很熟悉，对，这就是算法和数据结构啊，数据就是--数据结构，内部逻辑就是--算法。
所以说！！！数据结构和算法是一切软件学习的基石，
很多同学说，学习数据结构和算法，在工作中用不上，那是因为，我们每个人，都会一些基本的算法，加减乘除，简单比较排序，其实我们处理内部逻辑中，也用算法了的，只是我们工作中，大部分都用不上，数据结构和算法中nb的高阶算法。但是有些人的工作中用的到，就像小学生做小学算数，觉得简单，感觉算数就是这么回事，就认为不需要学习微积分，一样可以做求导算法一样。

我们展开来说下，
iOS中的数据是什么
在iOS开发中，【数据】就是属性，就是一个存储单元，它可以是组件UI，可以是NSArray，NSString
iOS中的内部逻辑是什么
在iOS开发中，【内部逻辑】就是方法，就是对【数据】各种操作的一种集合

2.金针度人
01.runtime中【数据】使用 -- 分类中添加属性
```
-(HETTextFieldAgent *)agent{
    return objc_getAssociatedObject(self, @selector(setAgent:));
}  //get方法定义，这段代码是分类中，添加属性的方式

-(void)setAgent:(HETTextFieldAgent *)agent{
    [self addTarget:self action:@selector(textFieldChanged) forControlEvents:UIControlEventEditingChanged];
    objc_setAssociatedObject(self, @selector(setAgent:), agent, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}  //set方法定义，这段代码是分类中，添加属性的方式
```

02.runtime中【数据】使用 -- 将属性或者说数据全局化（由于可以加block属性，故可全局化代码块）
```
typedef void(^viewDidLoad)(UIViewController<HETPublicVCProtocol> *vc);
//block声明
```

```
+(void)het_viewDidLoad:(viewDidLoad)viewDidLoad{
    objc_setAssociatedObject(self, @selector(het_viewDidLoad:), viewDidLoad, OBJC_ASSOCIATION_COPY_NONATOMIC);  
}  //全局定义的工具类，这个是类方法，生成(set方法)自定义属性【数据】
```

```
+(viewDidLoad)start_het_viewDidLoad{
    return objc_getAssociatedObject(self, @selector(het_viewDidLoad:));
}  //全局定义的工具类，这个是类方法，获取(get方法)上述自定义属性【数据】
```

```
    !HETPublicUIConfig.start_het_viewDidLoad?:[HETPublicUIConfig start_het_viewDidLoad](self);
    //使用该block属性
```



代码复用，组件化，代码解耦，高内聚，低耦合我这段时间也明白了


mvc和mvvm这2个概念，也困扰了我好久，我现在也终于明白了