```swift
## Node3DCreator 静态函数库
##
## 用于快速创建具有某些特定属性或设置的3D节点实例[br]
## 作者：巽星石[br]
## Godot版本： v4.0.1.stable.official [cacf49999][br]
## 创建时间：2023年4月1日23:29:03[br]
## 最后修改时间：2023年4月1日23:29:07[br]

class_name Node3DCreator

## 创建并返回一个billboard模式的Sprite3D节点，默认加载"res://icon.svg"为纹理，默认节点名称为"Sprite3D"
static func new_billboard_Sprite3D(texture:Texture = load("res://icon.svg"),node_name:String = "Sprite3D") -> Sprite3D:
	var node = Sprite3D.new()
	node.billboard = BaseMaterial3D.BILLBOARD_ENABLED
	node.texture = texture
	return node


## 创建并返回一个具有特定网格类型的MeshInstance3D节点，默认网格类型为"BoxMesh"，默认节点名称为"MeshInstance3D"
static func new_MeshInstance3D(MeshType:String = "BoxMesh",node_name:String = "MeshInstance3D") -> MeshInstance3D:
	var ins = MeshInstance3D.new()
	if MeshType not in ClassDB.get_inheriters_from_class("Mesh"):
		MeshType = "BoxMesh"
	ins.name = MeshType
	var mes = ClassDB.instantiate(MeshType)
	ins.mesh = mes
	return ins
```
