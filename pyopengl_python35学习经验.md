## pyopengl

无条理的记录一些经验


第一个错误：

	glutCreateWindow(b"simple")

此处名字的字符串前面必须添加一个b，否则就会报`type erro`的错误



关于显示窗口的坐标：

	glutInitWindowPosition(0,0)

这一个坐标同样是左上角坐标系


关于窗口颜色的设置 ：

	glClearColor(1,1,1,1)

这个函数必须是四个参数，也就是说必须要有alpha参数，但似乎并没有什么价值


关于透明度的设置 ：

虽然说在`glColor4f()`中的最后一个参数是透明度，但是设置起来并不会起作用，因为在opengl中透明度需要设置其它的东西