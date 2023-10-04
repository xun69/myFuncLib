```swift
# ======================================================================
# 名称：myPluginCreator
# 描述：用于快速创建插件的函数库
# 类型：静态函数库
# 作者：巽星石
# Godot版本：v4.0.2.stable.official [7a0977ce2]
# 创建时间：2023年4月8日11:11:11
# 最后修改时间：2023年4月8日11:11:14
# ======================================================================



class_name myPluginCreator


enum Plugin_Type{
	DOCK_PLUGIN = 0,         # 基于EditorPlugin的面板插件
	MAINSCREEN_PLUGIN = 1,   # 基于MainScreenPlugin的主屏幕插件
	SCRIPT_PLUGIN = 2,       # 基于ScriptPlugin的脚本插件
	GDEDITOR_DOCK_PLUGIN = 3 # 基于GDEditor的面板插件
}


# 创建主屏幕插件
static func create_mainscreen_plugin():
	pass

# 在当前项目中快速创建一个面板插件的基本框架
static func create_plugin(plugin_name:String,plugin_author:String,plug_type:int,desc:String = "",version = "",script_file_name = "plugin.gd"):
	var plugin_dir = "res://addons/%s/" % plugin_name
	var main_script_file_path = "%s%s" % [plugin_dir,script_file_name]
	var main_tscn_file_path = "%s%s" % [plugin_dir,"main.tscn"]
	
	# 创建插件路径
	if !DirAccess.dir_exists_absolute(plugin_dir):
		DirAccess.make_dir_recursive_absolute(plugin_dir)
		
	# 生成插件的.cfg文件
	create_plugin_cfg(plugin_name,plugin_author,desc,version,script_file_name)
	# 根据插件类型选择模板代码
	var tmp_code = ""
	match plug_type:
		Plugin_Type.DOCK_PLUGIN:
			tmp_code = dock_plugin_tmp_code(plugin_name)
		Plugin_Type.MAINSCREEN_PLUGIN:
			tmp_code = mainscreen_plugin_tmp_code(plugin_name,plugin_name)
	# 生成插件的主脚本文件
	myFile.saveString(main_script_file_path,tmp_code)
	# 生成main.tscn
	var ctr = Control.new()
	ctr.size_flags_horizontal = Control.SIZE_EXPAND_FILL
	ctr.size_flags_vertical = Control.SIZE_EXPAND_FILL
	var scene = PackedScene.new()
	scene.pack(ctr)
	ResourceSaver.save(scene,main_tscn_file_path)
	pass




# 生成插件的.cfg文件
static func create_plugin_cfg(plugin_name:String,plugin_author:String,desc:String = "",version = "",script_file_name = "plugin.gd"):
	myConfig.create_plugin_cfg(plugin_name,plugin_author,desc,version,script_file_name)
	pass

# 将插件的.cfg文件内容解析为字典


# ================================ 各类插件的模板代码 ================================

# dock插件模板代码
static func dock_plugin_tmp_code(plugin_name:String) -> String:
	return """@tool
extends EditorPlugin


var dock = preload("res://addons/[@plugin_name]/main.tscn").instantiate()

func _enter_tree():
	add_control_to_dock(EditorPlugin.DOCK_SLOT_LEFT_BR,dock)
		
func _exit_tree():
	remove_control_from_docks(dock)""".replace("[@plugin_name]",plugin_name)

# 底部面板插件模板代码

# 主屏幕插件模板代码
static func mainscreen_plugin_tmp_code(plugin_name:String,title:String) -> String:
	return """@tool
extends MainScreenPlugin

var ui:Control = preload("res://addons/[@plugin_name]/main.tscn").instantiate()
var icon:Texture = get_editor_interface().get_base_control().get_theme_icon("Node", "EditorIcons")
var title:String = "[@title]"


func _init():
	super._init(ui,icon,title)""".replace("[@title]",title).replace("[@plugin_name]",plugin_name)
	
# 打包依赖

```
使用：
```swift
@tool
extends EditorScript

func _run():
	myPluginCreator.create_plugin("new_plug2","巽星石",myPluginCreator.Plugin_Type.DOCK_PLUGIN)
	pass
```
上面代码运行后，创建如下结构：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1680927243512-64015812-82f6-457f-9e3c-9bb0e53a19b1.png#averageHue=%232a323c&clientId=ud1ff26aa-2fed-4&from=paste&height=271&id=udb56fa1b&originHeight=812&originWidth=528&originalType=binary&ratio=3&rotation=0&showTitle=false&size=52322&status=done&style=none&taskId=uea556deb-ca4f-42c8-87a1-cd90c95e5cd&title=&width=176)
