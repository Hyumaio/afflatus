# pillow


## ImageFont

+ ImageFont.truetype(*FontFile, FontSize*)
	+ FontFile 参数指定字体文件。仅支持 truetype 类型的字体文件。
	+ FontSize 参数指定字体大小，仅支持 interger。暂未知道此参数的值与字体 pt 值的对应关系。


## ImageDraw

+ ImageDraw.Draw(*Image*)，此函数定义了一个针对 Image 图像的 Draw 句柄。
+ Draw.text(*xy, text, fill=None, font=None*)
	+ xy 参数指定文字的横纵起始坐标。即左上角坐标。
	+ text 参数指定文字的内容。**注意在 python2 中如果含有中文需要使用 unicode 类型**。
	+ fill 参数指定文字的颜色。对于 RGB 模式的图片(以红色为例子)，可使用三元素元组 *(255,0,0)*，也可直接使用整型单值 *255* (计算公式：**INT = R + G\*255 + B\*255\*255**)，还可使用十六进制色值 *"#ff0000"*。
	+ font 参数指定文字的字体。实际上是使用了 ImageFont。



## 其他

+ 当需要粘贴一张带有 alpha 通道的图到一张底图上时，pillow 并不会将 alpha 通道自动粘上，也就是说没有透明效果。需要提取 alpha 通道，使用 Image.paste 的 mask 参数，将 alpha 通道手动粘上。代码如下：

	
	```python
	# 打开两张图片
	background = Image.open('./background.jpg').convert('RGB')
	alpha_pic = Image.open('./pic.png')  # 此图为带透明通道的 png 图片
	pic_size = alpha_pic.size
	
	# 提取 alpha 通道
	r, g, b, a = alpha_pic.split()
	
	# 粘贴到指定位置
	box = (100, 200, 100 + pic_size[0], 200 + pic_size[1])
	background.paste(alpha_pic, box=box, mask=a)
	```
	[相关链接请点击这里](https://blog.csdn.net/jacke121/article/details/80425144)

+ 在 resize 时，输出图片锯齿现象严重，使用 Image.ANTIALIAS 参数抗锯齿。