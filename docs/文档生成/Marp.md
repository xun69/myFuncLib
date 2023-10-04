```swift
# ========================================================
# 名称：Marp
# 类型：函数库
# 简介：生成Marp代码，用于制作PPT
# 作者：巽星石
# Godot版本：4.0.2-stable (official)
# 创建时间：2023-04-12 02:00:56
# 最后修改时间：2023-04-12 02:00:56
# ========================================================


# 返回godot4PPT统一模板的代码
func get_godot4_ppt_str() -> String:
	# === 文档模板 === 
	var tmp_path = "res://addons/Script++/lib/Marp_tmps/Godot4PPT.md"
	var tmp = FileAccess.get_file_as_string(tmp_path)
	return tmp

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1681236163123-e0c62fc7-01fe-4316-824f-8c5d4fd76221.png#averageHue=%23272e37&clientId=u7364ac69-3b70-4&from=paste&height=360&id=ud0fca5e6&originHeight=899&originWidth=840&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=110628&status=done&style=none&taskId=u4fdef84f-0022-411e-9514-2779f7493fa&title=&width=336)
