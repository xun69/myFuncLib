文档生成时间：2023-8-6 23:39:10
## 基本信息
| 项目 | 信息 |
| --- | --- |
| 文件名 | Ctl.gd |
| Godot版本 | 4.1.1-stable (official) |
| is_tool | false |
| extend | RefCounted |
| class_name | Ctl |
| 所在项目名称 | 自定义节点 |
| 文件路径 | res://class/Ctl.gd |
| 绝对路径 | C:/Users/巽星石/Documents/【Godot4项目】/自定义节点/class/Ctl.gd |
| 作者 | 巽星石 |

## 概述
控件生成函数+布局函数库。通过调用该函数库，可以用简单的函数创建常见的控件示例，通过layout函数，可以使用简单的二维数组创建控件的水平和垂直布局。利用其他的容器函数，也可以创建相应的实例。
例如：
```swift
@tool
extends EditorScript

func _run():
	var layout = Ctl.layout([
		[Ctl.label("测试",HORIZONTAL_ALIGNMENT_CENTER)],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.layout([
			[Ctl.label("布局嵌套",HORIZONTAL_ALIGNMENT_CENTER)],
			[Ctl.label("试试"),Ctl.text("请输入123")]
		])],
		[Ctl.button("提交"),Ctl.button("清除")],
		[Ctl.link("在线说明","https://www.bilibili.com/")]
	])
	
	
	Ctl.fill_y(layout)
	var txt = Ctl.textArea("请输入数据")
	txt.custom_minimum_size.y = 100
	var layout_hsplit = Ctl.vsplit(layout,txt)
	
	
	get_scene().add_child(layout_hsplit)
	Ctl.set_layout_owner(layout_hsplit,get_scene())
```
上面的脚本会创建如下的简单表单。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691322097928-37559d9b-2f4d-4e3c-9378-e79aad1c902a.png#averageHue=%23649f59&clientId=u1712aea1-5ed4-4&from=paste&height=480&id=ubfe66205&originHeight=1200&originWidth=2375&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=249186&status=done&style=none&taskId=u1f4343dd-4df0-4454-910b-45058aad63b&title=&width=950)
## 控件生成方法
### select(items:PackedStringArray,name:String) -> OptionButton
生成并返回一个由`items`定义的选项所组成，名称为`name`（默认为“select”）的下拉选框控件（`OptionButton`）实例。
比如在`EditorScript`中可以如下使用：
```swift
@tool
extends EditorScript

func _run():
	var sel = Ctl.select(["男","女"])
	
	get_scene().add_child(sel)
	Ctl.set_layout_owner(sel,get_scene())
```
运行后添加如下节点：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691328091993-f905031c-e942-4e1a-8e81-ba104efc495c.png#averageHue=%233d3d3d&clientId=u1712aea1-5ed4-4&from=paste&height=128&id=ub8c8c1cc&originHeight=320&originWidth=298&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=16545&status=done&style=none&taskId=u91f457fe-1eb5-4e54-a6b7-cf107c933ec&title=&width=119.2)
在普通场景和节点中可以如下使用：
```swift
extends Control

func _ready():
	var sel = Ctl.select(["男","女"])
	add_child(sel)
```
### spin(min:float,max:float,step:float,name:String) -> SpinBox
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| min | float | 0.0 | 最小值 |
| max | float | 100.0 | 最大值 |
| step | float | 1.0 | 单次调整步幅 |
| name | String | "spin" | 节点名称 |

生成并返回一个名称为`name`的数字微调框控件（`SpinBox`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.spin()
	
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691328807655-844fd3ca-c282-4aae-b2d6-69c9c2f9dfa7.png#averageHue=%234d4d4c&clientId=u1712aea1-5ed4-4&from=paste&height=54&id=u7cbdccfd&originHeight=135&originWidth=273&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=5892&status=done&style=none&taskId=u952768b2-75b6-46d3-a996-a53db4e517b&title=&width=109.2)
### text(placeholder:String,text:String,name:String) -> LineEdit
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| placeholder | String | "" | 占位文本 |
| text | String | "" | 文本 |
| name | String | "text" | 节点名称 |

生成并返回一个名称为`name`的单行文本框控件（`LineEdit`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.text("这里是占位文本") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691328952592-c58ebac9-bf0c-45aa-8c98-ec5d984c701c.png#averageHue=%234d4c4b&clientId=u1712aea1-5ed4-4&from=paste&height=55&id=uf7826a01&originHeight=137&originWidth=595&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=13738&status=done&style=none&taskId=u23bca002-506a-4944-999a-acd98ef7f79&title=&width=238)
### textArea(placeholder:String,text:String,name:String) -> TextEdit
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| placeholder | String | "" | 占位文本 |
| text | String | "" | 文本 |
| name | String | "textArea" | 节点名称 |

生成并返回一个名称为`name`的多行文本框控件（`TextEdit`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.textArea("这里是占位文本") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691329162475-d8f4c050-91a1-4d6d-92ea-f88ce460aa48.png#averageHue=%23373636&clientId=u1712aea1-5ed4-4&from=paste&height=175&id=ubd592591&originHeight=438&originWidth=874&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=16817&status=done&style=none&taskId=u01f5f375-3a07-4dfd-9428-01512513e44&title=&width=349.6)
### label(text:String,h_align:int,v_align:int,tool_tip:String,name:String) -> Label
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| text | String | 
 | 文本 |
| h_align | int | HORIZONTAL_ALIGNMENT_LEFT | 水平对齐方式 |
| v_align | int | VERTICAL_ALIGNMENT_TOP | 垂直对齐方式 |
| tool_tip | String | "" | 鼠标提示文本 |
| name | String | "label" | 节点名称 |

生成并返回一个名称为`name`的标签控件（`Label`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.label("这是一个Label") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691329386089-6d9ec97f-3f14-4fa4-920a-026afa73778f.png#averageHue=%23666665&clientId=u1712aea1-5ed4-4&from=paste&height=38&id=uf59595fc&originHeight=94&originWidth=356&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=9266&status=done&style=none&taskId=ueae25dd0-8a34-4234-9539-286b7e91367&title=&width=142.4)
### rich_label(text:String,tool_tip:String,name:String) -> RichTextLabel
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| text | String | 
 | 文本 |
| tool_tip | String | "" | 鼠标提示文本 |
| name | String | "rtxlab" | 节点名称 |

生成并返回一个名称为`name`的富文本标签控件（`RichTextLabel`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.rich_label("这是一个[b]RichTextLabel[/b]") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691329663192-59418e6f-0119-4970-847d-663f2d37038a.png#averageHue=%23686868&clientId=u1712aea1-5ed4-4&from=paste&height=32&id=u8d228530&originHeight=79&originWidth=679&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=17936&status=done&style=none&taskId=uaef77a16-3af2-4949-8e86-691aa66587a&title=&width=271.6)
:::warning
**注意：**
暂时未实现对BBcode的支持。（2023年8月6日21:47:56）
:::
### button(text:String,tool_tip:String,name:String) -> Button
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| text | String | 
 | 按钮文本 |
| tool_tip | String | "" | 鼠标提示文本 |
| name | String | "button" | 节点名称 |

生成并返回一个名称为`name`的按钮控件（`Button`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.button("确定") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691329805572-b3eb1510-1998-4199-a050-054aa5fc2b2d.png#averageHue=%23535251&clientId=u1712aea1-5ed4-4&from=paste&height=46&id=u46044381&originHeight=114&originWidth=151&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=5961&status=done&style=none&taskId=uf9779bcd-b4fd-4229-97e8-af46a3cec07&title=&width=60.4)
### link(text:String,url:String,tool_tip:String,name:String) -> LinkButton
| **参数** | **类型** | **默认值** | **描述** |
| --- | --- | --- | --- |
| text | String | 
 | 按钮文本 |
| url | String | 
 | 链接地址 |
| tool_tip | String | "" | 鼠标提示文本 |
| name | String | "link" | 节点名称 |

生成并返回一个名称为`name`的超链接按钮控件（`LinkButton`）实例。
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.link("B站","https://www.bilibili.com/") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691330042222-0f9ea4b6-3f4c-4510-883f-60e4d679a999.png#averageHue=%23646362&clientId=u1712aea1-5ed4-4&from=paste&height=35&id=u588a048a&originHeight=87&originWidth=142&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=4133&status=done&style=none&taskId=u94cb6d44-e5b8-4c03-b558-798063dfca1&title=&width=56.8)
其中url参数信息被作为元数据添加到控件实例。可以通过`get_meta("url")`，获取链接地址。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691330255583-e262b51b-289d-4035-9bf1-02430bbf3f66.png#averageHue=%23292e36&clientId=u1712aea1-5ed4-4&from=paste&height=72&id=u5b338063&originHeight=179&originWidth=915&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=24106&status=done&style=none&taskId=u8121d417-9d20-49aa-b7b3-9ba55dec423&title=&width=366)
通过连接和处理`pressed()`信号，使用`OS.shell_open()`传入url，就可以用系统浏览器打开网址了。
```swift
extends Control
@onready var link = $link


func _on_link_pressed():
	OS.shell_open(link.get_meta("url"))
```
### checkBox(text:String,tool_tip:String,name:String) -> CheckBox
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.checkBox("删除所有游戏存档") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691330825895-5943bc93-e5ac-42f4-aa36-33199d943e59.png#averageHue=%235f5f5f&clientId=u1712aea1-5ed4-4&from=paste&height=39&id=ubc2aee32&originHeight=97&originWidth=498&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=17200&status=done&style=none&taskId=u5d72bccb-5c59-490b-b7ee-0f70367c189&title=&width=199.2)
### onOff(text:String,tool_tip:String,name:String) -> CheckButton
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.onOff("高分辨率") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331129343-5acb8b42-ffb8-4d97-b0c2-a0df7d0cce5f.png#averageHue=%23606060&clientId=u1712aea1-5ed4-4&from=paste&height=40&id=u8434e4fb&originHeight=99&originWidth=368&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=12698&status=done&style=none&taskId=ucabb8c8b-9e62-405e-9be3-3d02c6c69b6&title=&width=147.2)
### colorButton(color:Color,tool_tip:String,name:String) -> ColorPickerButton
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.colorButton() 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331359053-624b7f90-aeae-4715-acfc-9b3983bb79ed.png#averageHue=%23a4a4a3&clientId=u1712aea1-5ed4-4&from=paste&height=42&id=ub06395ce&originHeight=106&originWidth=299&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=1199&status=done&style=none&taskId=u042cce24-df25-49ea-9ab6-12aac80fa23&title=&width=119.6)
### colorRect(color:Color,tool_tip:String,name:String) -> ColorRect
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.colorRect() 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331491253-692ab8e9-43c3-4cc2-8431-73e69a95eb7f.png#averageHue=%23c5c5c4&clientId=u1712aea1-5ed4-4&from=paste&height=80&id=u450174be&originHeight=200&originWidth=184&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=917&status=done&style=none&taskId=u60cd58b9-723d-480f-bf79-d632751bc91&title=&width=73.6)
### picture(src:String,tool_tip:String,name:String) -> TextureRect
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.picture("res://icon.svg") 
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331541659-c6319bf6-5604-42aa-a03c-7d36f2f95d3b.png#averageHue=%233d5976&clientId=u1712aea1-5ed4-4&from=paste&height=172&id=u69e683ab&originHeight=429&originWidth=417&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=53421&status=done&style=none&taskId=u6899e258-ffc4-4e0f-bf72-c19b7e93ff0&title=&width=166.8)
:::warning
暂定：需要改进。
:::
### tree(tool_tip:String,name:String) -> Tree
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.tree() 
	ctl.size = Vector2(100,200)
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331680072-ba0ac5f9-a84f-4e86-93c7-cf6d3fecc67a.png#averageHue=%23393938&clientId=u1712aea1-5ed4-4&from=paste&height=264&id=u39c82568&originHeight=660&originWidth=326&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=4500&status=done&style=none&taskId=u455a43ea-5e88-4ec5-ae2b-27557457e7d&title=&width=130.4)
:::warning
暂定：需要改进。
:::
### itemList(items:PackedStringArray,icon:Object,tool_tip:String,name:String) -> ItemList
```swift
@tool
extends EditorScript

func _run():
	var ctl = Ctl.itemList(["itm1","itm2","itm3"],preload("res://icon.svg")) 
	ctl.size = Vector2(450,450)
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331806564-b661f5ff-fa45-4a9f-8462-9b2e97662816.png#averageHue=%23bab5ad&clientId=u1712aea1-5ed4-4&from=paste&height=260&id=u516cf4af&originHeight=1404&originWidth=1417&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=193287&status=done&style=none&taskId=u7c612647-3757-4a6a-986a-8c15bb35c05&title=&width=262.8000183105469)
:::warning
暂定：需要改进。
:::
### progress(min:float,max:float,step:float,tool_tip:String,name:String) -> ProgressBar
```swift
var ctl = Ctl.progress() 
ctl.size = Vector2(100,30)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691331986862-2e1687ec-e3dd-422b-91b7-1cea3fcfa3ce.png#averageHue=%23474747&clientId=u1712aea1-5ed4-4&from=paste&height=46&id=ua666f3b3&originHeight=116&originWidth=345&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=5340&status=done&style=none&taskId=ucd7f0e02-5c0c-4975-9bcd-447d95ed49e&title=&width=138)
### hslider(min:float,max:float,step:float,tool_tip:String,name:String) -> HSlider
```swift
var ctl = Ctl.hslider() 
ctl.size = Vector2(100,30)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691332060058-777e3082-20ff-4697-8c25-63b2570f4e57.png#averageHue=%23515151&clientId=u1712aea1-5ed4-4&from=paste&height=38&id=u63da8746&originHeight=94&originWidth=339&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=2922&status=done&style=none&taskId=ua96e5637-923e-4b6d-911d-15a8a2a1fad&title=&width=135.6)
### vslider(min:float,max:float,step:float,tool_tip:String,name:String) -> VSlider
```swift
var ctl = Ctl.vslider() 
ctl.size = Vector2(30,100)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691332115543-cff83927-8ce4-4570-988b-0084fb5699ed.png#averageHue=%234b4b4b&clientId=u1712aea1-5ed4-4&from=paste&height=140&id=uffc06d61&originHeight=349&originWidth=107&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=3431&status=done&style=none&taskId=uee4e3f25-f5a8-49d9-8568-9d6b2469b33&title=&width=42.8)
### hline(name:String) -> HSeparator
```swift
var ctl = Ctl.hline() 
ctl.size = Vector2(100,10)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691332358243-17ceea54-f184-4293-bcf4-e1e546327d8a.png#averageHue=%23686766&clientId=u1712aea1-5ed4-4&from=paste&height=20&id=udf94f845&originHeight=50&originWidth=341&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=387&status=done&style=none&taskId=ubaf7c04d-948d-4bfb-9560-785e283b060&title=&width=136.4)
### vline(name:String) -> VSeparator
```swift
var ctl = Ctl.vline() 
ctl.size = Vector2(10,100)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691332411578-339bbf13-1735-4670-88d9-5053588dd292.png#clientId=u1712aea1-5ed4-4&from=paste&height=139&id=u49ab1371&originHeight=347&originWidth=61&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=685&status=done&style=none&taskId=ubd0e9c3e-fea2-4e0c-b1f9-3796ec1cb4c&title=&width=24.4)
## 布局和容器生成方法
### layout(ctls:Array,scroll:bool,name:String) -> void
布局函数，本函数库最核心和重要的函数之一。其原理是通过传入二维控件数组，来定义布局：

- 整个布局是存放在一个`VBoxContainer`中
- 二维数组的每行，如果只有一个控件，则直接放入`VBoxContainer`中，如果有两个及以上控件，则添加一个`HboxContainer`，然后将他们全部添加为子节点。
- 通过`VBoxContainer`和`HboxContainer`，实现一种类似于HTML表格的布局

示例：
```swift
@tool
extends EditorScript

func _run():
	var layout = Ctl.layout([
		[Ctl.label("测试")],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.button("提交"),Ctl.button("清除")]
	])
	get_scene().add_child(layout)
	Ctl.set_layout_owner(layout,get_scene())
```
上面的EditorScript运行后就会为当前场景根节点添加如下的控件结构，它看起来就是一个简单的表单。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691332635992-46c77580-b1bf-4ca2-9cf9-da26c07cf997.png#averageHue=%2391e297&clientId=u1712aea1-5ed4-4&from=paste&height=321&id=u10851103&originHeight=936&originWidth=1752&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=272851&status=done&style=none&taskId=uf06e3f19-1d21-4d47-a11b-286d25a0ae3&title=&width=600.7999877929688)
#### layout嵌套
`layout()`方法可以嵌套，这就意味着你可以用它和控件创建函数，以及下方的其他容器函数，轻松的用一个数组来描述一个复杂的UI嵌套结构。
```swift
@tool
extends EditorScript

func _run():
	var layout = Ctl.layout([
		[Ctl.label("测试")],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.layout([
			[Ctl.label("布局嵌套")],
			[Ctl.label("试试"),Ctl.text("请输入123")]
		])],
		[Ctl.button("提交"),Ctl.button("清除")],
		[Ctl.link("在线说明","https://www.bilibili.com/")]
	])
	
	
	get_scene().add_child(layout)
	Ctl.set_layout_owner(layout,get_scene())
```
#### 显示滚动条
`scroll`参数用于控制布局的外部是否包裹一个`ScrollContainer`，用于在窗体缩放等时，显示滚动条。
```swift
@tool
extends EditorScript

func _run():
	var layout = Ctl.layout([
		[Ctl.label("测试")],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.button("提交"),Ctl.button("清除")]
	],true)
	get_scene().add_child(layout)
	Ctl.set_layout_owner(layout,get_scene())

```
![在布局外包裹ScrollContainer](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691333359536-8b499a11-d09c-435f-934a-777d34ae092d.png#averageHue=%2394e69a&clientId=u1712aea1-5ed4-4&from=paste&height=254&id=ufc4e9d93&originHeight=911&originWidth=2087&originalType=binary&ratio=2.5&rotation=0&showTitle=true&size=292178&status=done&style=none&taskId=u2a9d28af-d0a1-4a9e-9c67-132bb9c0c6b&title=%E5%9C%A8%E5%B8%83%E5%B1%80%E5%A4%96%E5%8C%85%E8%A3%B9ScrollContainer&width=583 "在布局外包裹ScrollContainer")
### hsplit(left_ctl:Object,right_ctl:Object,name:String) -> HSplitContainer
左右可调节布局。
```swift
@tool
extends EditorScript

func _run():
	var right = Ctl.layout([
		[Ctl.label("测试",HORIZONTAL_ALIGNMENT_CENTER)],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.button("提交"),Ctl.button("清除")],
		[Ctl.link("在线说明","https://www.bilibili.com/")]
	],true,"right")
	
	var tree = Ctl.tree()
	tree.custom_minimum_size.x = 200
	var layout_hsplit = Ctl.hsplit(tree,right)
	
	
	get_scene().add_child(layout_hsplit)
	Ctl.set_layout_owner(layout_hsplit,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691333779842-cbc4068f-f709-4026-abed-a7109fc0a4de.png#averageHue=%23476324&clientId=u1712aea1-5ed4-4&from=paste&height=470&id=ubd8dceda&originHeight=1175&originWidth=2212&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=197793&status=done&style=none&taskId=ud1d7c789-068d-4ce4-8ce0-e67a5889494&title=&width=884.8)
### vsplit(up_ctl:Object,bottom_ctl:Object,name:String) -> VSplitContainer
上下可调节布局。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691333935555-d74a2fda-b102-4b59-9d5f-f8e2b9edbe72.png#averageHue=%23649a54&clientId=u1712aea1-5ed4-4&from=paste&height=432&id=u49668f0f&originHeight=1079&originWidth=2195&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=196166&status=done&style=none&taskId=uc004c188-c456-4051-b9a9-2994c17b9df&title=&width=878)
### LRBox(left_ctl:Object,right_ctl:Object,name:String) -> HBoxContainer
左右固定布局。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691334014395-0e8bc4d1-06d6-406e-8664-cf5c8fad431d.png#averageHue=%2398ec9f&clientId=u1712aea1-5ed4-4&from=paste&height=438&id=u83e09c90&originHeight=1094&originWidth=2224&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=213636&status=done&style=none&taskId=u554a173c-1cee-48d2-836c-cf011c18381&title=&width=889.6)
### UDBox(up_ctl:Object,bottom_ctl:Object,name:String) -> VBoxContainer
上下固定布局。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691334072477-00ff2479-113d-4e82-8622-f6e8a740ff38.png#averageHue=%2398ec9f&clientId=u1712aea1-5ed4-4&from=paste&height=444&id=uc0d4d83d&originHeight=1111&originWidth=2225&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=213313&status=done&style=none&taskId=u9363f976-ffdd-4b24-ae89-f008ff5287b&title=&width=890)
### hbox(items:Array,name:String) ->  HBoxContainer
不受个数限制的`HBoxContainer`布局。
```swift
@tool
extends EditorScript

func _run():
	var main = Ctl.layout([
		[Ctl.label("测试",HORIZONTAL_ALIGNMENT_CENTER)],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.button("提交"),Ctl.button("清除")],
		[Ctl.link("在线说明","https://www.bilibili.com/")]
	],true,"right")
	
	var tree = Ctl.tree()
	tree.custom_minimum_size.x = 200
	
	var tree2 = Ctl.tree()
	tree2.custom_minimum_size.x = 200
	
	var layout = Ctl.hbox([tree,main,tree2])
	
	
	get_scene().add_child(layout)
	Ctl.set_layout_owner(layout,get_scene())
```
![左中右固定布局](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691334274729-209b8eaa-a21a-42b3-8d99-b9181377a018.png#averageHue=%23b5a48a&clientId=u1712aea1-5ed4-4&from=paste&height=512&id=u4bd3ce74&originHeight=1280&originWidth=2429&originalType=binary&ratio=2.5&rotation=0&showTitle=true&size=234320&status=done&style=none&taskId=ued3bdd3b-41e5-4428-9fb0-0a978683766&title=%E5%B7%A6%E4%B8%AD%E5%8F%B3%E5%9B%BA%E5%AE%9A%E5%B8%83%E5%B1%80&width=971.6 "左中右固定布局")
### vbox(items:Array,name:String) ->  VBoxContainer
```swift
@tool
extends EditorScript

func _run():
	# =================================== 顶部 ===================================
	var top = Ctl.colorRect()
	Ctl.min_height(top,100)
	Ctl.fill_x(top)
	# =================================== 中间 ===================================
	# ===== 左侧
	var tree = Ctl.tree()
	Ctl.min_width(tree,200)
	
	# ===== 中间
	var main = Ctl.layout([
		[Ctl.label("测试",HORIZONTAL_ALIGNMENT_CENTER)],
		[Ctl.label("姓名"),Ctl.text("请输入姓名")],
		[Ctl.label("性别"),Ctl.select(["男","女"])],
		[Ctl.button("提交"),Ctl.button("清除")],
		[Ctl.link("在线说明","https://www.bilibili.com/")]
	],true,"right")
	
	# ===== 右侧
	var tree2 = Ctl.tree()
	Ctl.min_width(tree2,200)
	
	
	var main_center = Ctl.hbox([tree,main,tree2])
	Ctl.fill(main_center)
	# =================================== 底部 ===================================
	var bottom = Ctl.colorRect(Color.GREEN_YELLOW)
	Ctl.min_height(bottom,30)
	Ctl.fill_x(bottom)
	
	
	var layout = Ctl.vbox([top,main_center,bottom]) # 完整布局
	
	
	get_scene().add_child(layout)
	Ctl.set_layout_owner(layout,get_scene())
```
![结合了垂直和水平盒子的布局](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691334703918-37f31228-662d-4d97-ad26-11c16a618d63.png#averageHue=%23b0fa34&clientId=u1712aea1-5ed4-4&from=paste&height=487&id=u03a53e6a&originHeight=1217&originWidth=2362&originalType=binary&ratio=2.5&rotation=0&showTitle=true&size=244061&status=done&style=none&taskId=u6f4649cc-8f3b-4eb6-8b7c-6bbe1a17d38&title=%E7%BB%93%E5%90%88%E4%BA%86%E5%9E%82%E7%9B%B4%E5%92%8C%E6%B0%B4%E5%B9%B3%E7%9B%92%E5%AD%90%E7%9A%84%E5%B8%83%E5%B1%80&width=944.8 "结合了垂直和水平盒子的布局")
### grid(items:Array,name:String) -> GridContainer
```swift
@tool
extends EditorScript

func _run():
	var ctls:Array[Control]
	for i in range(10):
		var btn = Ctl.button("button%d" % i)
		Ctl.fill_x(btn)
		ctls.append(btn)
		
	var ctl = Ctl.grid(ctls,2)
	
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691335637816-c90d22b7-e7a9-4713-b080-3c12a6fafd0b.png#averageHue=%237cd383&clientId=u1712aea1-5ed4-4&from=paste&height=349&id=u0cabf5a0&originHeight=872&originWidth=1227&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=180688&status=done&style=none&taskId=ucc5b22c2-a81c-4caa-89cc-6e54706a213&title=&width=490.8)
### tabs(items:Array,name:String) -> TabContainer
```swift
@tool
extends EditorScript

func _run():
	var ctls:Array[Control]
	for i in range(10):
		var btn = Ctl.button("button%d" % i)
		Ctl.fill_x(btn)
		ctls.append(btn)
		
	var ctl = Ctl.tabs(ctls)
	Ctl.size(ctl,Vector2(200,200))
	
	get_scene().add_child(ctl)
	Ctl.set_layout_owner(ctl,get_scene())
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691335809193-511a213c-5f13-4922-8b66-7fb77ad2e1ef.png#averageHue=%2376c278&clientId=u1712aea1-5ed4-4&from=paste&height=344&id=u6b65f104&originHeight=861&originWidth=1275&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=135007&status=done&style=none&taskId=uf15ba5dc-2ab9-40d0-b5e0-973137772fb&title=&width=510)
### hflow(items:Array,name:String) -> HFlowContainer
这是是描述。
### vflow(items:Array,name:String) -> VFlowContainer
这是是描述。
## 布局设置
### set_layout_owner(layout:Node,owner:Node) -> void
设置`layout`及其所有子结点的`owner` - 用于插件或EditorScript添加时使用。
## 控件layout设置属性
### fill_x(ctl:Control) -> void
将`ctl`控件水平撑开，也就是设置`ctl`的`size_flags_horizontal`等于`SIZE_EXPAND_FILL`。
### fill_y(ctl:Control) -> void
将`ctl`控件垂直撑开，也就是设置`ctl`的`size_flags_vertical`等于`SIZE_EXPAND_FILL`。
### fill(ctl:Control) -> void
将`ctl`控件水平和垂直撑开，也就是设置`ctl`的`size_flags_horizontal`和`size_flags_vertical`都等于`SIZE_EXPAND_FILL`。
### min_width(ctl:Control,val:float) -> void
设定`ctl`控件的最小宽度（`custom_minimum_size.x`）为`val`。
### min_height(ctl:Control,val:float) -> void
设定`ctl`控件的最小宽高度（`custom_minimum_size.y`）为`val`。
### min_size(ctl:Control,val:Vector2) -> void
设定`ctl`控件的最小尺寸（`custom_minimum_size`）为`val`。
### width(ctl:Control,val:float) -> void
设定`ctl`控件的宽度（`size.x`）为`val`。
### height(ctl:Control,val:float) -> void
设定`ctl`控件的高度（`size.y`）为`val`。
### size(ctl:Control,val:Vector2) -> void
设定`ctl`控件的尺寸（`size`）为`val`。
## 源代码
以下是完整源代码：
```swift
# ========================================================
# 名称：Ctl
# 类型：静态函数库
# 简介：控件节点生成函数库
# 作者：巽星石
# Godot版本：4.1-stable (official)
# 创建时间：2023-08-04 01:37:41
# 最后修改时间：2023年8月6日23:37:45
# ========================================================
class_name Ctl

# 选择框
static func select(items:PackedStringArray,name:String = "select") -> OptionButton:
	var ctl = OptionButton.new()
	ctl.name = name
	for item in items:
		if item == "---":
			ctl.add_separator()
		elif item.begins_with("---") and item != "---":
			ctl.add_separator(item.replace("---",""))
		else:
			ctl.add_item(item)
	return ctl

# 数字微调框
static func spin(min:float = 0.0,max:float = 100.0,step:float =1.0,name:String = "spin") -> SpinBox:
	var ctl = SpinBox.new()
	ctl.name = name
	ctl.min_value = min
	ctl.max_value = max
	ctl.step = step
	return ctl

# 单行文本框
static func text(placeholder:String="",text:String = "",name:String = "text") -> LineEdit:
	var ctl = LineEdit.new()
	ctl.name = name
	ctl.text = text
	ctl.placeholder_text = placeholder
	return ctl

# 多行文本框
static func textArea(placeholder:String="",text:String = "",name:String = "textArea") -> TextEdit:
	var ctl = TextEdit.new()
	ctl.name = name
	ctl.text = text
	ctl.placeholder_text = placeholder
	return ctl

# 标签
static func label(text:String,h_align:int = HORIZONTAL_ALIGNMENT_LEFT,v_align:int = VERTICAL_ALIGNMENT_TOP,tool_tip:String="",name:String = "label") -> Label:
	var ctl = Label.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	# 水平和垂直对齐
	ctl.horizontal_alignment = h_align
	ctl.vertical_alignment = v_align
	return ctl

# 标签
static func rich_label(text:String,tool_tip:String="",name:String = "rtxlab") -> RichTextLabel:
	var ctl = RichTextLabel.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	
	return ctl


# 按钮
static func button(text:String,tool_tip:String="",name:String = "button") -> Button:
	var ctl = Button.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	return ctl

# 超链接
static func link(text:String,url:String,tool_tip:String="",name:String = "link") -> LinkButton:
	var ctl = LinkButton.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	ctl.set_meta("url",url)
	return ctl

# 复选框
static func checkBox(text:String,tool_tip:String="",name:String = "ckeck") -> CheckBox:
	var ctl = CheckBox.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	return ctl

# 开关按钮
static func onOff(text:String,tool_tip:String="",name:String = "onoff") -> CheckButton:
	var ctl = CheckButton.new()
	ctl.name = name
	ctl.text = text
	ctl.tooltip_text = tool_tip
	return ctl

# 颜色选择器按钮
static func colorButton(color:Color = Color.WHITE,tool_tip:String="",name:String = "colorBtn") -> ColorPickerButton:
	var ctl = ColorPickerButton.new()
	ctl.name = name
	ctl.color = color
	ctl.tooltip_text = tool_tip
	return ctl

# 纯色块
static func colorRect(color:Color = Color.WHITE,tool_tip:String="",name:String = "colorRect") -> ColorRect:
	var ctl = ColorRect.new()
	ctl.name = name
	ctl.color = color
	ctl.tooltip_text = tool_tip
	return ctl

# 图片
static func picture(src:String,tool_tip:String="",name:String = "pic") -> TextureRect:
	var ctl = TextureRect.new()
	ctl.name = name
	ctl.texture = load(src)
	ctl.tooltip_text = tool_tip
	return ctl

# 树
static func tree(tool_tip:String="",name:String = "tree") -> Tree:
	var ctl = Tree.new()
	ctl.name = name
	ctl.tooltip_text = tool_tip
	return ctl

# 列表
static func itemList(items:PackedStringArray,icon:Texture2D,tool_tip:String="",name:String = "list") -> ItemList:
	var ctl = ItemList.new()
	ctl.name = name
	ctl.tooltip_text = tool_tip
	for item in items:
		if icon:
			ctl.add_item(item,icon)
		else:
			ctl.add_item(item)
	return ctl

# 进度条
static func progress(min:float = 0.0,max:float = 100.0,step:float =1.0,tool_tip:String="",name:String = "progress") -> ProgressBar:
	var ctl = ProgressBar.new()
	ctl.name = name
	ctl.tooltip_text = tool_tip
	ctl.min_value = min
	ctl.max_value = max
	ctl.step = step
	return ctl

# 水平滑动条
static func hslider(min:float = 0.0,max:float = 100.0,step:float =1.0,tool_tip:String="",name:String = "hslider") -> HSlider:
	var ctl = HSlider.new()
	ctl.name = name
	ctl.tooltip_text = tool_tip
	ctl.min_value = min
	ctl.max_value = max
	ctl.step = step
	return ctl

# 垂直滑动条
static func vslider(min:float = 0.0,max:float = 100.0,step:float =1.0,tool_tip:String="",name:String = "vslider") -> VSlider:
	var ctl = VSlider.new()
	ctl.name = name
	ctl.tooltip_text = tool_tip
	ctl.min_value = min
	ctl.max_value = max
	ctl.step = step
	return ctl

# 水平分割线
static func hline(name:String = "hline") -> HSeparator:
	var ctl = HSeparator.new()
	ctl.name = name
	return ctl

# 垂直分割线
static func vline(name:String = "vline") -> VSeparator:
	var ctl = VSeparator.new()
	ctl.name = name
	return ctl


# =================================== 布局 ===================================
# 布局函数
# 默认使用VBox+HBox的形式构造行列布局
# 每行超过一个元素，就使用HBox布局
# 类似于一种简单的表格布局
static func layout(ctls:Array,scroll:bool = false,name:String = "layout"):
	var root_ctl:Control
	if scroll:
		root_ctl = ScrollContainer.new()
		root_ctl.name = "scroll"
		fill(root_ctl)
	# VBox
	var vbox = VBoxContainer.new()
	vbox.name = name
	fill_x(vbox)
	# 加载行
	for row in ctls:
		if row.size() == 1:
			vbox.add_child(row[0],true)
			fill_x(row[0])
		elif row.size() > 1:
			var hbox = HBoxContainer.new()
			fill_x(hbox)
			for ct in row:
				hbox.add_child(ct,true)
				fill_x(ct)
			vbox.add_child(hbox,true)
	if scroll:
		root_ctl.add_child(vbox,true)
	else:
		root_ctl = vbox
	return root_ctl

# 左右可调布局
static func hsplit(left_ctl:Node,right_ctl:Node,name:String = "hsplit") -> HSplitContainer:
	var h_split = HSplitContainer.new()
	h_split.name = name
	fill(h_split)
	h_split.add_child(left_ctl,true)
	h_split.add_child(right_ctl,true)
	return h_split

# 上下可调布局
static func vsplit(up_ctl:Node,bottom_ctl:Node,name:String = "vsplit") -> VSplitContainer:
	var v_split = VSplitContainer.new()
	v_split.name = name
	fill(v_split)
	v_split.add_child(up_ctl,true)
	v_split.add_child(bottom_ctl,true)
	return v_split


# 左右固定布局
static func LRBox(left_ctl:Node,right_ctl:Node,name:String = "LRBox") -> HBoxContainer:
	var ctl = HBoxContainer.new()
	ctl.name = name
	fill(ctl)
	ctl.add_child(left_ctl,true)
	ctl.add_child(right_ctl,true)
	return ctl


# 上下固定布局
static func UDBox(up_ctl:Node,bottom_ctl:Node,name:String = "UDBox") -> VBoxContainer:
	var ctl = VBoxContainer.new()
	ctl.name = name
	fill(ctl)
	ctl.add_child(up_ctl,true)
	ctl.add_child(bottom_ctl,true)
	return ctl


# 水平盒子
static func hbox(items:Array[Control],name:String = "HBox") -> HBoxContainer:
	var ctl = HBoxContainer.new()
	ctl.name = name
	for item in items:
		ctl.add_child(item,true)
	return ctl

# 垂直盒子
static func vbox(items:Array[Control],name:String = "HBox") -> VBoxContainer:
	var ctl = VBoxContainer.new()
	ctl.name = name
	for item in items:
		ctl.add_child(item,true)
	return ctl


# 网格布局
static func grid(items:Array[Control],cols:int =1,name:String = "grid") -> GridContainer:
	var grid = GridContainer.new()
	grid.name = name
	grid.columns = cols
	fill(grid)
	for item in items:
		grid.add_child(item,true)
	return grid

# 选项卡布局
static func tabs(items:Array[Control],name:String = "tabs") -> TabContainer:
	var tab = TabContainer.new()
	tab.name = name
	fill(tab)
	for item in items:
		tab.add_child(item,true)
	return tab


# 水平流式布局
static func hflow(items:Array[Control],name:String = "hflow") -> HFlowContainer:
	var ctl = HFlowContainer.new()
	ctl.name = name
	fill(ctl)
	for item in items:
		ctl.add_child(item,true)
	return ctl

# 垂直流式布局
static func vflow(items:Array[Control],name:String = "vflow") -> VFlowContainer:
	var ctl = VFlowContainer.new()
	ctl.name = name
	fill(ctl)
	for item in items:
		ctl.add_child(item,true)
	return ctl

# 设置layout及其所有子结点的owner - 用于插件或EditorScript添加
static func set_layout_owner(layout:Node,owner:Node) -> void:
	layout.owner = owner
	for child in layout.get_children():
		child.owner = owner
		set_layout_owner(child,owner)

# =================================== 撑开 ===================================
# 水平撑开
static func fill_x(ctl:Control) -> void:
	ctl.size_flags_horizontal = Control.SIZE_EXPAND_FILL

# 垂直撑开
static func fill_y(ctl:Control) -> void:
	ctl.size_flags_vertical = Control.SIZE_EXPAND_FILL

# 水平+垂直撑开
static func fill(ctl:Control) -> void:
	ctl.size_flags_horizontal = Control.SIZE_EXPAND_FILL
	ctl.size_flags_vertical = Control.SIZE_EXPAND_FILL

# =================================== 最小尺寸 ===================================
# 最小宽度
static func min_width(ctl:Control,val:float) -> void:
	ctl.custom_minimum_size.x = val

# 最小高度
static func min_height(ctl:Control,val:float) -> void:
	ctl.custom_minimum_size.y = val

# 最小尺寸
static func min_size(ctl:Control,val:Vector2) -> void:
	ctl.custom_minimum_size = val

# =================================== 尺寸 ===================================
# 宽度
static func width(ctl:Control,val:float) -> void:
	ctl.size.x = val

# 高度
static func height(ctl:Control,val:float) -> void:
	ctl.size.y = val

# 尺寸
static func size(ctl:Control,val:Vector2) -> void:
	ctl.size = val

```

