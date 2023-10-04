```swift
# ======================================================================
# 名称：MDPage
# 描述：从原先的MDCreator中分离出，为GDScript脚本生成Markdown格式的API文档框架
#      可用于快速的创建脚本的API文档
# 类型：普通函数库
# 依赖：    
#        - MDCreator.gd
#        - ScriptInfo.gd
# 作者：巽星石
# Godot版本：v4.0.2.stable.official [7a0977ce2]
# 创建时间：2022年10月21日22:28:37
# 最后修改时间：2023年4月8日12:42:34
# ======================================================================

class_name MDPage

# ========================= 依赖 =========================
var gd = GDEditor.new()
var md = MDCreator.new()
var mFile = myFile.new()
var mDateTime = myDateTime.new()
# =============== 为GDScript脚本生成Markdown =====================

# 传入路径
# 生成一个GDscript的基础结构的Markdown文档
func make_GDscript_MD_by_path(script_path:String,md_save_path:String,is_part:bool = false):
	var page_str = GDScript_API_Page(script_path,is_part)
	mFile.saveString(md_save_path,page_str) # 保存Markdown文件
	var tree = gd.face.get_tree()
	var face = EditorPlugin.new().get_editor_interface()
	await face.get_tree().create_timer(2.0).timeout
	if is_part:
		# .assets/路径
		var assets_path = "%s.assets/" % md_save_path.trim_suffix(".md")
		mFile.dir_exists_or_create(assets_path)  # 创建.assets/路径
		# 获取场景面板节点列表截图并保存
		var scene_tree_path = "%s%s.png" % [assets_path,"scene_tree_dock"] 
		gd.save_scene_tree_dock_screen_short(scene_tree_path)
		# 获取节点图标并保存
		var nodes_deep_arr = gd.get_current_scene_nodes_deep_dic_arr()
		for node in nodes_deep_arr:
			var script_name = script_path.get_file()
			var clas = node["node"].get_class()
			var icon:Texture = gd.get_icon(clas)
			var img = icon.get_data()
			# 图标尺寸缩小为20×20
			img.resize(20,20,Image.INTERPOLATE_NEAREST)
			var save_path = md_save_path
			img.save_png("%s%s.png" % [assets_path,clas])

# 传入路径
# 生成一个新的marp幻灯片格式，可以直接导出为PDF、PPT等
func make_new_Marp_by_path(md_save_path:String):
	var file_name = md_save_path.get_file().replace(".md","")
	var page_str = new_marp_Page(file_name,"巽星石")
	mFile.saveString(md_save_path,page_str) # 保存Markdown文件
	var tree = gd.face.get_tree()
	var face = EditorPlugin.new().get_editor_interface()
	await face.get_tree().create_timer(2.0).timeout
	# .assets/路径
	var assets_path = "%s.assets/" % md_save_path.trim_suffix(".md")
	mFile.dir_exists_or_create(assets_path)  # 创建.assets/路径
	# 复制默认的封面图和logo
	var face_from_path = "res://addons/myTreeAdd/imgs/face.jpg"
	var face_to_path = "%s%s" % [assets_path,"face.jpg"] 
	var icon_from_path = "res://addons/myTreeAdd/imgs/godot.png"
	var icon_to_path = "%s%s" % [assets_path,"godot.png"] 
	mFile.copy(face_from_path,face_to_path)
	mFile.copy(icon_from_path,icon_to_path)


# 传入路径
# 生成一个GDscript的基础结构的Markdown文档 - marp幻灯片格式，可以直接导出为PDF、PPT等
func make_GDscript_Marp_by_path(script_path:String,md_save_path:String,is_part:bool = false):
	var page_str = GDScript_marp_Page(script_path,is_part)
	mFile.saveString(md_save_path,page_str) # 保存Markdown文件
	var tree = gd.face.get_tree()
	var face = EditorPlugin.new().get_editor_interface()
	await face.get_tree().create_timer(2.0).timeout
	# .assets/路径
	var assets_path = "%s.assets/" % md_save_path.trim_suffix(".md")
	mFile.dir_exists_or_create(assets_path)  # 创建.assets/路径
	# 复制默认的封面图和logo
	var face_from_path = "res://addons/myTreeAdd/imgs/face.jpg"
	var face_to_path = "%s%s" % [assets_path,"face.jpg"] 
	var icon_from_path = "res://addons/myTreeAdd/imgs/godot.png"
	var icon_to_path = "%s%s" % [assets_path,"godot.png"] 
	mFile.copy(face_from_path,face_to_path)
	mFile.copy(icon_from_path,icon_to_path)
		


# 返回 场景+代码的名称或路径
func tscn_and_gd_path(path:String) -> String:
	var f_path = path.replace(".gd","") # 没有后缀的路径
	return "%s<br>%s" % [f_path+".tscn",f_path+".gd"]


# 返回API页面的Markdown字符串
func GDScript_API_Page(script_path:String,is_part:bool = false) -> String:
	var info = ScriptInfo.new(script_path)
	var md_str = ""
	md_str += md.H(1,"%s" % info.file_name() if !is_part else info.file_name().replace(".gd",""))
	md_str += md.H(2,"基本信息")
	# 脚本信息
	md_str += md.H(3,"脚本信息")
	md_str += md.TH(["项目","信息"])
	md_str += md.TR(["文件名",info.file_name() if !is_part else tscn_and_gd_path(info.file_name())])
	md_str += md.TR(["is_tool",("是" if info.is_tool() else "否")])
	md_str += md.TR(["extend",(info.get_extends_type() if info.get_extends_type() else "-")])
	md_str += md.TR(["class_name",(info.get_class_name() if info.get_class_name() else "-")])
	# 文件信息
	md_str += md.H(3,"文件信息")
	md_str += md.TH(["项目","信息"])
	md_str += md.TR(["项目名称",info.project_name()])
	md_str += md.TR(["文件路径",info.file_path() if !is_part else tscn_and_gd_path(info.file_path())])
	md_str += md.TR(["绝对路径",info.global_path()if !is_part else tscn_and_gd_path(info.global_path())])
	# 其他信息
	md_str += md.H(3,"其他信息")
	md_str += md.TH(["项目","信息"])
	md_str += md.TR(["作者",""])
	md_str += md.TR(["备注",""])
	# 文档信息
	md_str += md.H(3,"文档信息")
	md_str += md.TH(["项目","信息"])
	md_str += md.TR(["文档创建时间",mDateTime.Now()])
	md_str += md.H(2,"概述")
	md_str += md.P("这里写概述")
	if is_part: # 如果是组件
		md_str += md.TH(["参数面板","示例"])
		md_str += md.TR(["",""])
		md_str += md.H(2,"场景")
		md_str += md.H(3,"场景组成")
		md_str += md.H(4,"场景组成截图")
		md_str += md.TH(["场景面板","场景预览"])
		var ss = md.img("%s.assets/%s.png" %[script_path.get_file(),"scene_tree_dock"]).replace("\n","")
		md_str += md.TR([ss,""])
		md_str += md.H(4,"场景节点清单")
		md_str += md.TH(["节点","类型","描述"])
		# 创建节点清单
		var nodes_deep_arr = gd.get_current_scene_nodes_deep_dic_arr()
		for node in nodes_deep_arr:
			var deep = node["deep"]
			var script_name = script_path.get_file()
			var clas = node["node"].get_class()
			var icon_str = md.img("%s.assets/%s.png" %[script_name,clas]).replace("\n","")
			md_str += md.TR([
				"&nbsp;".repeat(deep*8) + icon_str + node["node"].name,
				node["node"].get_class(),
				""
				])
		md_str += md.H(4,"场景文件(.tscn)源码")
		# 场景代码
		var path = gd.get_current_scene_path()
		var tscn_code = mFile.loadString(path)
		md_str += md.Code_Block(tscn_code,"ini")
	# 信号
	var signal_names = info.get_signals()
	if signal_names.size() > 0:
		md_str += md.H(2,"信号")
		md_str += md.TH(["信号","描述"])
		for signel in signal_names:
			md_str += md.TR([signel,""])
			
	# 枚举
	# enum_names返回值形式如下：
	# [[name,vals],[name,vals],[name,vals]...]
	# 其中name是枚举的名称，vals是值列表
	var enum_names = info.get_enums() # 所有脚本内部方法名
	if enum_names.size() > 0:
		md_str += md.H(2,"枚举")
		for enu in enum_names:
			md_str += md.H(3,enu[0])
			md_str += md.TH(["可选值","描述"])
			var vals = enu[1].split(",")
			for val in vals:
				md_str += md.TR([val,""])
	
	# 变量
	# 返回形式如下：
	# [[导出变量数组],[onready变量数组],[普通全局变量数组]]
	var var_names = info.get_propertys()
	var show_propertys = false
	for vars in var_names:
		if var_names[vars].size()>0:
			show_propertys = true
	if show_propertys:
		md_str += md.H(2,"属性")
		# 导出变量
		if var_names["export_vars"].size()>0:
			md_str += md.H(3,"导出变量")
			md_str += md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["export_vars"]:
				if ver.begins_with("export"):
					md_str += md.TR([ver,"","",""])
		# 全局变量
		if var_names["global_vars"].size()>0:
			md_str += md.H(3,"普通全局变量")
			md_str += md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["global_vars"]:
				if ver.begins_with("var"):
					md_str += md.TR([ver,"","",""])
		# onready变量
		if var_names["onready_vars"].size()>0:
			md_str += md.H(3,"onready变量")
			md_str += md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["onready_vars"]:
				if ver.begins_with("onready"):
					md_str += md.TR([ver,"","",""])
	# 常量
	var const_names = info.get_consts()
	if const_names.size() > 0:
		md_str += md.H(2,"常量")
		md_str += md.TH(["常量","值类型","值","描述"])
		for cst in const_names:
			md_str += md.TR([cst,"","",""])
	
	# 方法
	var func_names = info.get_methods()
	md_str += md.H(2,"方法")
	for fuc in func_names:
		md_str += md.H(3,fuc)
		md_str += md.P("这里是方法的描述。")
	md_str += md.H(2,"源代码")
	md_str += md.P("这里是完整源代码：")
	md_str += md.Code_Block(info.get_source_code(),"swift")
	return md_str

# ========================= marp MarkDown幻灯片生成 =========================
# yaml head -- 幻灯片基础设置
func marp_head(icon_path:String) -> String:
	var head:String = """---
# 开启
marp: true
# 主题
theme: base-theme
style: |
	section {
		background-color: #eee;
	}
	section H1{
		font-size:36px;
		color:#blue;
	}
	section H2{
		font-size:36px;
		color:blue;
		border-left:5px solid blue;
		padding-left:10px;
	}
# 尺寸
size: 16:9
# 是否显示页码
paginate: true
---

	"""
	head = head.replace("【页眉图标】",icon_path)
	head = head.replace("【项目名】",ProjectSettings.get_setting("application/config/name"))
	return head
	

# 封面页
func marp_face_page(bg_img:String,type:String,script_name:String,author:String) -> String:
	var face_page = """
![bg left:40%](【背景图】)
[【类型】]【脚本名称】
===
这里是简要描述
<br>
<br>

【文档创建时间】 **@【作者】** 

---

"""
	face_page = face_page.replace("【背景图】",bg_img)
	face_page = face_page.replace("【类型】",type)
	face_page = face_page.replace("【脚本名称】",script_name)
	face_page = face_page.replace("【文档创建时间】",mDateTime.Now())
	face_page = face_page.replace("【作者】",author)
	return face_page

# 章节页
func marp_h1_page(icon_path:String,title:String,script_name:String,content:String = "") -> String:
	var h1_page = """
<!--
header: '![w:50](【页眉图标】) 【项目名】 /【脚本名称】 /**【章节名】**'
footer: 文档创建时间：【文档创建时间】
-->
# 【章节名】
【内容】

---

"""
	h1_page = h1_page.replace("【页眉图标】",icon_path)
	h1_page = h1_page.replace("【项目名】",ProjectSettings.get_setting("application/config/name"))
	h1_page = h1_page.replace("【脚本名称】",script_name)
	h1_page = h1_page.replace("【章节名】",title)
	h1_page = h1_page.replace("【内容】",content)
	h1_page = h1_page.replace("【文档创建时间】",mDateTime.Now())
	return h1_page

# 2级标题页
func marp_h2_page(title:String,content:String = "") -> String:
	
	var h2_page = """
## 【2级标题】
【内容】

---

"""
	h2_page = h2_page.replace("【2级标题】",title)
	h2_page = h2_page.replace("【内容】",content)
	return h2_page
	
# 3级标题页
func marp_h3_page(title:String,content:String = "") -> String:
	
	var h3_page = """
### 【3级标题】
【内容】

---

"""
	h3_page = h3_page.replace("【3级标题】",title)
	h3_page = h3_page.replace("【内容】",content)
	return h3_page

# 返回Godot的版本
func get_godot_version_string() -> String:
	var dic = Engine.get_version_info()
	return "%s.%s.%s.%s" % [dic["major"],dic["minor"],dic["patch"],dic["status"]]

# 最后一页
func end_page(bg_img:String,author:String) -> String:
	var end_page_str = """

![bg opacity:0.2 blur:10px](【背景图片】)
# 完结

感谢您的观看。
<br>
<br>

**@【作者】** 为您提供

"""
	end_page_str = end_page_str.replace("【背景图片】",bg_img)
	end_page_str = end_page_str.replace("【作者】",author)
	return end_page_str

# 返回可用于生成PPT的marpit Markdown字符串 -- 新文档（万用）
func new_marp_Page(title:String,author:String) -> String:
	var md_str = ""
	
	var icon_path = "%s.assets/godot.png" % title # 图标的相对路径
	var bg_img = "%s.assets/face.jpg" % title   # 封面页背景图
	
	# 头部 - 设置
	md_str += marp_head(icon_path)
	# 封面
	md_str += marp_face_page(bg_img,"",title,author)
	# 概述
	md_str += marp_h1_page(icon_path,"一级标题",title,"这里是内容")
	md_str += end_page(bg_img,author)
	return md_str

# 返回可用于生成PPT的marpit Markdown字符串 -- 脚本API文档
func GDScript_marp_Page(script_path:String,is_part:bool = false) -> String:
	var info = ScriptInfo.new(script_path)
	var md_str = ""
	
	var script_name = script_path.get_file() # 脚本名称
	var icon_path = "%s.assets/godot.png" % script_name # 图标的相对路径
	var bg_img = "%s.assets/face.jpg" % script_name   # 封面页背景图
	var type = "组件" if is_part else "函数库" # 脚本的类型：函数库、组件
	script_name = script_name.replace(".gd","") # 脚本名称
	var author = "巽星石" # 作者
	
	# 头部 - 设置
	md_str += marp_head(icon_path)
	# 封面
	md_str += marp_face_page(bg_img,type,script_name,author)
	# 概述
	md_str += marp_h1_page(icon_path,"概述",script_name,"这里是概述") 
	# 目录
	md_str += marp_h1_page(icon_path,"目录",script_name,"- 这里是简单目录") 
	# 基本信息
	md_str += marp_h1_page(icon_path,"基本信息",script_name) 
	# 脚本信息
	var ul1 = md.UL([
		"文件名：%s" % info.file_name() if !is_part else tscn_and_gd_path(info.file_name()),
		"tool脚本：%s" % ("是" if info.is_tool() else "否"),
		"继承自：%s" % (info.get_extends_type() if info.get_extends_type() else "-"),
		"类名：%s" % (info.get_class_name() if info.get_class_name() else "-"),
	])
	md_str += marp_h2_page("脚本信息",ul1)
	# 文件信息
	var ul2 = md.UL([
		"项目名称：%s" % info.project_name(),
		"文件路径：%s" % info.file_path() if !is_part else tscn_and_gd_path(info.file_path()),
		"绝对路径：%s" % info.global_path()if !is_part else tscn_and_gd_path(info.global_path()),
	])
	md_str += marp_h2_page("文件信息",ul2)
	# 其他信息
	var ul3 = md.UL([
		"作者：%s" % info.project_name(),
		"Godot版本：%s" % get_godot_version_string() ,
		"备注：%s" % "",
	])
	md_str += marp_h2_page("其他信息",ul3)
	
	# 成员
	md_str += marp_h1_page(icon_path,"成员",script_name) 
	# 信号
	var signal_names = info.get_signals()
	if signal_names.size() > 0:
		var table1 = md.TH(["信号","描述"])
		for signel in signal_names:
			table1 += md.TR([signel,""])
		md_str += marp_h2_page("信号",table1) 
	
			
	# 枚举
	# enum_names返回值形式如下：
	# [[name,vals],[name,vals],[name,vals]...]
	# 其中name是枚举的名称，vals是值列表
	var enum_names = info.get_enums() # 所有脚本内部方法名
	if enum_names.size() > 0:
		var table2 = ""
		for enu in enum_names:
			table2 += md.H(3,enu[0])
			table2 += md.TH(["可选值","描述"])
			var vals = enu[1].split(",")
			for val in vals:
				table2 += md.TR([val,""])
		md_str += marp_h2_page("枚举",table2) 
	
	# 变量
	# 返回形式如下：
	# [[导出变量数组],[onready变量数组],[普通全局变量数组]]
	var var_names = info.get_propertys()
	var show_propertys = false
	for vars in var_names:
		if var_names[vars].size()>0:
			show_propertys = true
	if show_propertys:
		# 导出变量
		if var_names["export_vars"].size()>0:
			var table3 = md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["export_vars"]:
				if ver.begins_with("export"):
					table3 += md.TR([ver,"","",""])
			md_str += marp_h2_page("导出变量",table3) 
		# 全局变量
		if var_names["global_vars"].size()>0:
			var table4 = md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["global_vars"]:
				if ver.begins_with("var"):
					table4 += md.TR([ver,"","",""])
			md_str += marp_h2_page("普通全局变量",table4) 
		# onready变量
		if var_names["onready_vars"].size()>0:
			var table5 = md.TH(["变量","值类型","默认值","描述"])
			for ver in var_names["onready_vars"]:
				if ver.begins_with("onready"):
					table5 += md.TR([ver,"","",""])
			md_str += marp_h2_page("onready变量",table5) 
	# 常量
	var const_names = info.get_consts()
	if const_names.size() > 0:
		var table6 = md.TH(["常量","值类型","值","描述"])
		for cst in const_names:
			table6 += md.TR([cst,"","",""])
		md_str += marp_h2_page("常量",table6) 
	
	
	# 方法
	md_str += marp_h1_page(icon_path,"方法",script_name) 
	var func_names = info.get_methods()
	var table7 = ""
	for fuc in func_names:
		table7 += marp_h2_page(fuc,"这里是方法的描述。")
	md_str += table7
	# 源代码
	md_str += marp_h1_page(icon_path,"源代码",script_name,"后面是完整源代码") 
	# 按每10行划分代码
	var code_split_10 = code_split(info.get_source_code())
	var code_pages = ""
	for i in code_split_10.size():
		var code_block = md.Code_Block(code_split_10[i],"swift")
		if i == 0:
			code_pages += marp_h2_page("源代码",code_block)
		else:
			code_pages += marp_h2_page("源代码 - %s" % (i+1),code_block)
	md_str += code_pages
	md_str += end_page(bg_img,author)
	return md_str

# 按指定的行数划分代码，返回数组
func code_split(code:String,lines:int = 15) -> PackedStringArray:
	var _lines:PackedStringArray = []
	var code_lines = code.split("\n")
	var lines_str = ""
	for i in code_lines.size():
		if posmod(i+1,lines) != 0:
			lines_str += code_lines[i] + "\n"
		elif i == code_lines.size():
			lines_str += code_lines[i] + "\n"
			_lines.append(lines_str)
			lines_str = ""
		else:
			lines_str += code_lines[i] + "\n"
			_lines.append(lines_str)
			lines_str = ""
	return _lines

```
