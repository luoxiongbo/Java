通过设置元素的border-radius值，可以给元素设置圆角边框，甚至实现绘制圆、半圆、四分之一圆等各种圆角图形。

# 一、border-radius使用

border-radius的数值有三种表示方法：px、%、em，对于border-radius的值的设置，我们常用的有三种写法：

## **（1）仅设置一个值**

第一种方法，应该是我们最常用的一种情况了，常用来给按钮图标加圆角边框，或者画一个圆形按钮，仅需设置一个数值，即可给元素的四个边角设置统一的圆角弧度，例如：



![img](https://img-blog.csdnimg.cn/042e87cb8ef44093a95951f694823e36.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_17,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑![img](https://img-blog.csdnimg.cn/a4deb809bcda45af97bf3a9fdc297eb2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_13,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

代码如下：

```
1 #test1 {
2     border: 3px solid red;
3     height: 100px;
4     width: 200px;
5     border-radius: 30px;
6 }
1 #test2 {
2 　  border: 3px solid red;
3　　 height: 100px;
4 　　width: 100px;
5 　　border-radius: 53px;
6 }
```

## **（2）设置四个方向的值**

border-radius属性其实是border-top-left-radius、border-top-right-radius、border-bottom-right-radius、border-bottom-left-radius四个属性的简写模式，因此，border-radius : 30px;，其实等价于border-radius : 30px 30px 30px 30px;（ps：与padding和margin一样，各个数字之间用空格隔开）。

　　这里要注意四个数值的书写顺序，不同于padding和margin的“上、右、下、左”的顺序，border-radius采用的是左上角、右上角、右下角、左下角的顺序（如图1，2，3，4的顺序），如下图所示：

![img](https://img-blog.csdnimg.cn/289748a005ee4ccb91b727d28e4a62a0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_9,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## **（3）省略部分值**

与padding和margin一样，border-radius同样可以省略部分值（对角线的值），省略时同样是采用对角线相等的原则，例如

 ![img](https://img-blog.csdnimg.cn/4721b6332c204d4e9b5a126a154b0da9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_18,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

```
1 #test3 {
2     border: 3px solid red;
3     height: 100px;
4     width: 200px;
5     border-radius: 50px 0px;
6 }
```

#  二、border-radius数值设置及显示效果的理解

## **（1）使用px表示数值的情况**

　　在使用px来表示圆角值的时候，圆角的弧度一般都是一个圆形的部分弧形，具体呈现的显示效果我是按如下方法与预估和理解的：

　　假设一个长200px，高150px的div对象，设置它的border-radius的值为30px，那么实际呈现的圆角，其实就是一个以30px为半径的圆顶格放置在四个边角后所呈现的弧度，语言表达的可能不够透彻，看下面的例子应该可以明白。

![img](https://img-blog.csdnimg.cn/cb6f8c3f85ec4bf5b43e085c4fdb80c9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



```
 1 #test4 {
 2     height: 150px;
 3     width: 200px;
 4     border-radius: 30px;
 5     background-color: cornflowerblue;
 6 }
 7 #circle {
 8     width: 60px;
 9     height: 60px;
10     border-radius: 30px;
11     background-color: chartreuse;
12 }
1 <div id="test4">
2     <div id="circle"></div>
3 </div>
```

这么去理解以后，下次再需要绘制圆形、各个方向的半圆、四分之一圆的时候，再也不需要去百度或者记住什么宽和高的比例必须是1比2之类的了，想象一下，大概就能知道比例和数值该怎么去设置了。哈哈，感觉有点乱，也不知道我描述的内容，除了我自己，你能不能看懂。

## **（2）使用%表示数值的情况**

　　使用%来表示圆角值的时候，如果对象的宽和高是一样的，那判断方法与第一点一致，只不过想象的时候，需要将宽高乘以百分数换算一下；

　　如果宽高不一致，那产生的效果，其实就是以对象的宽高乘以百分数后得到的值r1和r2，作为两条半径绘制出来的椭圆产生的弧度。

![img](https://img-blog.csdnimg.cn/46c6fc202b584d4e82ce37a64d219abc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



```
1 #test5 {
2     height: 100px;
3     width: 200px;
4     border-radius: 50%;
5     background-color: cornflowerblue;
6 }
```

## **（3）需要注意的情况**

 　在设置对象为圆形的时候，如果使用了border、padding等情况时，对象的实际显示宽高已经不再是设置的width和height的数值，如果border-radius设置的值仍为原本的width和height的一半，可能达不到预期的真正的圆形的效果。

　　如下面这个例子，给div加了10px的边框，但是border-radius仍是以100px的一半来设置的，显示出来的效果显然就不是一个真正的圆形。

 ![img](https://img-blog.csdnimg.cn/92bc7170796640f7a96268c8d8a670fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

```
1 #test6 {
2     height: 100px;
3     width: 100px;
4     border-radius: 50px;
5     border: 10px solid #CCCCCC;
6     background-color: cornflowerblue;
7 }
```

 可以通过设置百分比50%的方式来解决这一情况，或者计算一下实际的高度再来设置border-radius的数值。上面这个例子，div实际的宽高应该是100 + 10 * 2 = 120px，所以border-radius应该设置为60px。

![img](https://img-blog.csdnimg.cn/da1ee21fdf06447d9bdf7e6e7c9975d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY291bnRlcm9mZmVuc2l2ZSBMVU8=,size_19,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



```
1 #test7 {
2     height: 100px;
3     width: 100px;
4     border-radius: 60px;
5     border: 10px solid #CCCCCC;
6     background-color: cornflowerblue;
7 }
```

以上就是该属性的应用，希望能帮助到大家，顺便感谢吃火鸡的馒头！！！