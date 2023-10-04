```swift
# 这是一个简单的ES脚本
@tool
extends EditorScript

var myFile = preload("res://addons/Script++/lib/myFile.gd").new()
var base_dir = "C:/Users/Lenovo/Downloads/"
var file_name = "EasyX基础入门"
# 运行函数
func _run():
	var md = myFile.loadString("%s/%s.md" % [base_dir,file_name])
	md = H5Creator.parse(md)
	var fpath = "%s/%s.html" % [base_dir,file_name]
	myFile.saveString(fpath,md)
	myFile.shell_open(fpath)
	pass
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1691779235175-e5490b11-1b4d-49e5-85ff-c85a12b714d6.png#averageHue=%23edeceb&clientId=u381d952a-9ee2-4&from=paste&height=319&id=ue734008b&originHeight=956&originWidth=1141&originalType=binary&ratio=3&rotation=0&showTitle=false&size=117102&status=done&style=stroke&taskId=u67d7547e-16af-4f53-8bac-cdf2852aaed&title=&width=380.3333333333333)
