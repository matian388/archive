# 这是关于AVR学习过程中的记录

___

说是要安装proteus,用于仿真；安装atmel studio用于编写代码，编译产生Hex文件；还说要安装WinAVR,但是并不知道为什么


* 关于atmel studio下的编译

	![](http://i.imgur.com/SIru9P3.png)

* 在输出文件夹的Debug子文件夹下可以找到.Hex文件

* 从上面的图片上可以看到，第一个要包含的标准头文件即为第一个avr/io.h，下面的那一个可以使用定义好的`_delay_ms_`函数，图片中可以看到的`F_CPU`是用来设置时钟频率的，后面的UL即为单位Hz，但是呢？和图像显示的不一样，为了不报错，必须要把define写在第一行，在头文件之前，我有点糊涂，似乎C语言的规范就是如此

* 然后还有其他的头文件，完成了对中断等东西的包装，具体的估计还是要查资料

* 另外就是proteus使用的时候，对于单片机，可以调入程序文件，是.Hex文件，在调入界面里还可以设置熔丝位，具体的不多说，忘了自己查资料
* 熔丝位的配置还不算完全清楚，查资料吧

* 仿真的时候，元件接口如果忘记怎么用了，还是要查资料的，接错了就不会显示，例如，四位七段数码管，就是分共阳极和共阴极的


> 以上是大概的软件用法上的说明

___

##关于编程

图片上也可以看到一点，要使用`DDRC(A/B)`,来设置A,B,C号端口的输入还是输出，某一位为1代表对应的端口是输出模式，为0代表输入
设置完输入输出之后，使用`PORTC`来设置所有的C端口的电平，例如不断右移位可以实现流水灯什么的，我们无法直接设置单独的某一位，没有直接的命令。至少暂时据我所知是这样。

> 16.10.27 先写到这里吧


## AD转换

首先声明，在进行AD转换时没有必要一定要先设置端口为输入或者输出。

然后，要知道的事情是：与AD转换相关的有四个寄存器，分别为：`ADMUX`,`ADCSRA`及`ADCH`和`ADCL`，下面来解释一下设置。

* ADMUX :

		寄存器共8位，最高两位为：REFS1,REFS0,
		这两位负责控制参考电压的选择，具体细节查资料，我推荐00
		接下来的ADLAR负责设置结果左对齐，还是右对齐，
		在这里提前把最后两个寄存器ADCH,ADCL也说了，结果是10位的，
		但最后两个寄存器共16位，因此其中有6位是无效的，左对齐即结果的
		最高位为ADCH的最高位，ADCL最低6位空出来；右对齐即最低位为ADCL
		的最低位，至于怎么使用左右移位把结果拼接起来自己考虑吧
		ADMUX的最低的4位为MUX3,MUX2,MUX1,MUX0，它们负责设置AD转换的通道
		是哪一个，值为0~7
		这样8位中的7位都有了特定意义，有一位是空余出来，毫无意义的，至于
		具体的排列如果有不清楚，查资料

* ADCSRA ：

		寄存器共8位，自高至低分别为：ADEN,ADSC,ADATE,ADIF,ADIE,ADPS2,ADPS1,ADPS0
		ADEN为AD使能位，要使用AD转换，此位必须设置为1
		ADSC为转换的启动位，总之也要设置为1
		ADATE是转换的自动触发位，一般设为0
		ADIF这一位可以设置为0，当转换开始后，转换也是要耗费时间的，一旦转换完成，
		机器将自动将此位设置为1，检测到1后说明转换完成了，可以开始读结果了。
		ADIE是与中断相关的，可以设置为0
		最后三位负责设置分频因子，总之算是控制转换的速度，可以调快调慢
		
AD转换就大概是这样的吧

> 中文手册是一个比较好的资料

---

