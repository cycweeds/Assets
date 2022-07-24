# **iOS 优化**

## **内容**
- [UI优化](#-UIoptimize)
- [代码优化](#-daimayouhua)
- [逻辑优化](#-luojiyouhua)
- [性能优化](#-xingnengyouhua)
- [其他优化](#-qitayouhua)


## **UI优化**
1.布局推荐，纯Frame模式采用xib+autolayout模式布局  
2.使用  ```intrinsicSize``` 去优化宽高显示,可以不用在外部指定宽高。  
3.cell 复用的时候，有需要在 ```prepareForReuse```执行一些方法。比如图片下载的取消，数据的恢复等等。  
4.使用 iOS 9之后推出的 ```UIStackView``` 去进行UI布局。  
5.使用UIView 的 抗压缩 和抗拉伸 机制，去实现 关键label被压缩的问题。


## **代码优化**
1.判断条件 type == 1 == 2 用枚举去代替
type == Car   type == House  
2.注释规范 用系统 cmd+/ 代替，原来的无法快速查看注释。  
3.所有新增Model必须填写注释，接手之前的需要补全。类中一些Int类型不具体不要通过注释的形式去知道，
通过定义枚举Int类型去快速定位。   
4.http 请求参数3个及3个以上，用一个参数params代替 （老的暂时不改动，继续往上加）  
5.NSString类型 全用copy属性（有地方会看到 strong 或者retain）     delegate用weak属性， 这个看到可以修改一下  
6.少用UIView  tag这个逻辑, 遇到能够取到对象的就直接取对象
7.类名统一不能缩写，VC和VM 这些必须全名，
范围：成员变量、类名
（局部变量命名可以缩写）  
8.代码风格规范问题，采用工具去监测。  
OC: OCLint  Swift: SwiftLint。   


## **逻辑优化**
1.刷新逻辑。  
先加入refresh header，刷新头开始刷新，没有更多数据则不添加refresh footer。
有更多数据的时候，添加 refresh footer。
刷新和下拉的时候pageIndex，这个参数需要在成功之后才去赋值。
pageSize 通用20为一页。  
2.搜索逻辑。  
请求新的搜索接口，需要取消对之前的老的搜索接口，会有网络延迟问题，后请求的先处理，导致数据异常。  
3.UI显示逻辑。  
刷新数据的时候通过移除全部subview，再添加的方式，可能会导致UI卡顿。
通过显示隐藏的方式去控制。


## **性能优化**
1.使用 ```AutoReleasePool ``` 去进行内存优化。  
2.使用 ```TimerProfile``` 去监测执行时间过长的函数，并优化。


## **其他优化**
1.包体积优化：  
冗余代码删除 [classunref](https://github.com/xuezhulian/classunref)  
多余图片资源删除 [LSUnusedResources](https://github.com/tinymind/LSUnusedResources)

2.CI/CD 接入jenkins。完成自动化,并通知到钉钉群，无需手动喊测试。

3.编译优化， oc 编译慢 可以采用更换mac，或者接入 [ccache](https://ccache.dev/)
