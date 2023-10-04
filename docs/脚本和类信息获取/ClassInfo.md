文档生成时间：2023-04-18 23:38:11
## 基本信息
| 项目 | 信息 |
| --- | --- |
| 文件名 | ClassInfo.gd |
| Godot版本 | 4.0.2-stable (official) |
| is_tool | false |
| extend | RefCounted |
| class_name | ClassInfo |
| 所在项目名称 | Script++ |
| 文件路径 | res://addons/Script++/lib/ClassInfo.gd |
| 绝对路径 | C:/Users/巽星石/Documents/【Godot4项目】/Script++/addons/Script++/lib/ClassInfo.gd |
| 作者 | 巽星石 |

## 概述
基于`ClassDB`创建的静态函数库。用于获得更加简化的Godot4.0内部类的成员信息。
自定义类可以由`ScriptInfo`函数库提供的静态函数，获取相应的类成员信息。
在函数名称设计上，`ScriptInfo`和`ClassInfo`将保持尽量一致。以使使用者忘记两者之间的差异。
## 方法
### get_signal_lists(className:String) -> PackedStringArray
返回`className`所指定的类型的信号列表。
例如：
```swift
@tool
extends EditorScript

func _run():
	var list = ClassInfo.get_signal_lists("Control")
	print(JSON.stringify(list,"\t"))
```
将会输出：
```swift
[
	"focus_entered()",
	"focus_exited()",
	"gui_input(event)",
	"minimum_size_changed()",
	"mouse_entered()",
	"mouse_exited()",
	"resized()",
	"size_flags_changed()",
	"theme_changed()"
]
```
查看Control类型的信号列表，可以发现一致。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/8438332/1681834314660-6c43a72a-231a-4d36-a523-522c1627e5e8.png#averageHue=%232a2f37&clientId=udd0031ed-251e-4&from=paste&height=207&id=uc41df85d&originHeight=517&originWidth=493&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=69564&status=done&style=none&taskId=u36c90410-e428-4bd8-9896-fa714c915a8&title=&width=197.2)
### get_enum_lists(className:String) -> Array[Dictionary]
返回`className`所指定的类型的枚举字典列表。
例如：
```swift
@tool
extends EditorScript

func _run():
	var list = ClassInfo.get_enum_lists("Control")
	print(JSON.stringify(list,"\t"))
```
将会输出：
```swift
[
	{
		"name": "FocusMode",
		"vals": [
			"FOCUS_NONE",
			"FOCUS_CLICK",
			"FOCUS_ALL"
		]
	},
	{
		"name": "CursorShape",
		"vals": [
			"CURSOR_ARROW",
			"CURSOR_IBEAM",
			"CURSOR_POINTING_HAND",
			"CURSOR_CROSS",
			"CURSOR_WAIT",
			"CURSOR_BUSY",
			"CURSOR_DRAG",
			"CURSOR_CAN_DROP",
			"CURSOR_FORBIDDEN",
			"CURSOR_VSIZE",
			"CURSOR_HSIZE",
			"CURSOR_BDIAGSIZE",
			"CURSOR_FDIAGSIZE",
			"CURSOR_MOVE",
			"CURSOR_VSPLIT",
			"CURSOR_HSPLIT",
			"CURSOR_HELP"
		]
	},
	{
		"name": "LayoutPreset",
		"vals": [
			"PRESET_TOP_LEFT",
			"PRESET_TOP_RIGHT",
			"PRESET_BOTTOM_LEFT",
			"PRESET_BOTTOM_RIGHT",
			"PRESET_CENTER_LEFT",
			"PRESET_CENTER_TOP",
			"PRESET_CENTER_RIGHT",
			"PRESET_CENTER_BOTTOM",
			"PRESET_CENTER",
			"PRESET_LEFT_WIDE",
			"PRESET_TOP_WIDE",
			"PRESET_RIGHT_WIDE",
			"PRESET_BOTTOM_WIDE",
			"PRESET_VCENTER_WIDE",
			"PRESET_HCENTER_WIDE",
			"PRESET_FULL_RECT"
		]
	},
	{
		"name": "LayoutPresetMode",
		"vals": [
			"PRESET_MODE_MINSIZE",
			"PRESET_MODE_KEEP_WIDTH",
			"PRESET_MODE_KEEP_HEIGHT",
			"PRESET_MODE_KEEP_SIZE"
		]
	},
	{
		"name": "SizeFlags",
		"vals": [
			"SIZE_SHRINK_BEGIN",
			"SIZE_FILL",
			"SIZE_EXPAND",
			"SIZE_EXPAND_FILL",
			"SIZE_SHRINK_CENTER",
			"SIZE_SHRINK_END"
		]
	},
	{
		"name": "MouseFilter",
		"vals": [
			"MOUSE_FILTER_STOP",
			"MOUSE_FILTER_PASS",
			"MOUSE_FILTER_IGNORE"
		]
	},
	{
		"name": "GrowDirection",
		"vals": [
			"GROW_DIRECTION_BEGIN",
			"GROW_DIRECTION_END",
			"GROW_DIRECTION_BOTH"
		]
	},
	{
		"name": "Anchor",
		"vals": [
			"ANCHOR_BEGIN",
			"ANCHOR_END"
		]
	},
	{
		"name": "LayoutDirection",
		"vals": [
			"LAYOUT_DIRECTION_INHERITED",
			"LAYOUT_DIRECTION_LOCALE",
			"LAYOUT_DIRECTION_LTR",
			"LAYOUT_DIRECTION_RTL"
		]
	},
	{
		"name": "TextDirection",
		"vals": [
			"TEXT_DIRECTION_INHERITED",
			"TEXT_DIRECTION_AUTO",
			"TEXT_DIRECTION_LTR",
			"TEXT_DIRECTION_RTL"
		]
	}
]
```
### get_property_lists(className:String) -> Array
返回`className`所指定的类型的枚举字典列表。
例如：
```swift
@tool
extends EditorScript

func _run():
	var list = ClassInfo.get_property_lists("Control")
	print(JSON.stringify(list,"\t"))
```
将会输出：
```swift
[
	{
		"name": "Anchor Offsets",
		"type": "null"
	},
	{
		"name": "Anchor Points",
		"type": "null"
	},
	{
		"name": "Container Sizing",
		"type": "null"
	},
	{
		"name": "Focus",
		"type": "null"
	},
	{
		"name": "Grow Direction",
		"type": "null"
	},
	{
		"name": "Input",
		"type": "null"
	},
	{
		"name": "Layout",
		"type": "null"
	},
	{
		"name": "Localization",
		"type": "null"
	},
	{
		"name": "Mouse",
		"type": "null"
	},
	{
		"name": "Theme",
		"type": "null"
	},
	{
		"name": "Tooltip",
		"type": "null"
	},
	{
		"name": "Transform",
		"type": "null"
	},
	{
		"name": "anchor_bottom",
		"type": "float"
	},
	{
		"name": "anchor_left",
		"type": "float"
	},
	{
		"name": "anchor_right",
		"type": "float"
	},
	{
		"name": "anchor_top",
		"type": "float"
	},
	{
		"name": "anchors_preset",
		"type": "int"
	},
	{
		"name": "auto_translate",
		"type": "bool"
	},
	{
		"name": "clip_contents",
		"type": "bool"
	},
	{
		"name": "custom_minimum_size",
		"type": "Vector2"
	},
	{
		"name": "focus_mode",
		"type": "int"
	},
	{
		"name": "focus_neighbor_bottom",
		"type": "NodePath"
	},
	{
		"name": "focus_neighbor_left",
		"type": "NodePath"
	},
	{
		"name": "focus_neighbor_right",
		"type": "NodePath"
	},
	{
		"name": "focus_neighbor_top",
		"type": "NodePath"
	},
	{
		"name": "focus_next",
		"type": "NodePath"
	},
	{
		"name": "focus_previous",
		"type": "NodePath"
	},
	{
		"name": "global_position",
		"type": "Vector2"
	},
	{
		"name": "grow_horizontal",
		"type": "int"
	},
	{
		"name": "grow_vertical",
		"type": "int"
	},
	{
		"name": "layout_direction",
		"type": "int"
	},
	{
		"name": "layout_mode",
		"type": "int"
	},
	{
		"name": "localize_numeral_system",
		"type": "bool"
	},
	{
		"name": "mouse_default_cursor_shape",
		"type": "int"
	},
	{
		"name": "mouse_filter",
		"type": "int"
	},
	{
		"name": "mouse_force_pass_scroll_events",
		"type": "bool"
	},
	{
		"name": "offset_bottom",
		"type": "int"
	},
	{
		"name": "offset_left",
		"type": "int"
	},
	{
		"name": "offset_right",
		"type": "int"
	},
	{
		"name": "offset_top",
		"type": "int"
	},
	{
		"name": "pivot_offset",
		"type": "Vector2"
	},
	{
		"name": "position",
		"type": "Vector2"
	},
	{
		"name": "rotation",
		"type": "float"
	},
	{
		"name": "rotation_degrees",
		"type": "float"
	},
	{
		"name": "scale",
		"type": "Vector2"
	},
	{
		"name": "shortcut_context",
		"type": ""
	},
	{
		"name": "size",
		"type": "Vector2"
	},
	{
		"name": "size_flags_horizontal",
		"type": "int"
	},
	{
		"name": "size_flags_stretch_ratio",
		"type": "float"
	},
	{
		"name": "size_flags_vertical",
		"type": "int"
	},
	{
		"name": "theme",
		"type": "Theme"
	},
	{
		"name": "theme_type_variation",
		"type": "String"
	},
	{
		"name": "tooltip_text",
		"type": "String"
	}
]
```
### get_method_lists(className:String) -> PackedStringArray
返回`className`所指定的类型的方法列表，形式采用`函数名(参数列表) -> 返回类型`字符串。
例如：
```swift
@tool
extends EditorScript

func _run():
	var list = ClassInfo.get_method_lists("Control")
	print(JSON.stringify(list,"\t"))
```
输出：
```swift
[
	"_can_drop_data(at_position:Vector2,data:null) -> bool",
	"_drop_data(at_position:Vector2,data:null) -> void",
	"_get_anchors_layout_preset() -> int",
	"_get_drag_data(at_position:Vector2) -> void",
	"_get_layout_mode() -> int",
	"_get_minimum_size() -> Vector2",
	"_gui_input(event:Object) -> void",
	"_has_point(point:Vector2) -> bool",
	"_make_custom_tooltip(for_text:String) -> Object",
	"_set_anchor(side:int,anchor:float) -> void",
	"_set_anchors_layout_preset(preset:int) -> void",
	"_set_global_position(position:Vector2) -> void",
	"_set_layout_mode(mode:int) -> void",
	"_set_position(position:Vector2) -> void",
	"_set_size(size:Vector2) -> void",
	"_structured_text_parser(args:Array,text:String) -> Array",
	"_update_minimum_size() -> void",
	"accept_event() -> void",
	"add_theme_color_override(name:StringName,color:Color) -> void",
	"add_theme_constant_override(name:StringName,constant:int) -> void",
	"add_theme_font_override(name:StringName,font:Object) -> void",
	"add_theme_font_size_override(name:StringName,font_size:int) -> void",
	"add_theme_icon_override(name:StringName,texture:Object) -> void",
	"add_theme_stylebox_override(name:StringName,stylebox:Object) -> void",
	"begin_bulk_theme_override() -> void",
	"end_bulk_theme_override() -> void",
	"find_next_valid_focus() -> Object",
	"find_prev_valid_focus() -> Object",
	"force_drag(data:null,preview:Object) -> void",
	"get_anchor(side:int) -> float",
	"get_begin() -> Vector2",
	"get_combined_minimum_size() -> Vector2",
	"get_cursor_shape(position:Vector2) -> int",
	"get_custom_minimum_size() -> Vector2",
	"get_default_cursor_shape() -> int",
	"get_end() -> Vector2",
	"get_focus_mode() -> int",
	"get_focus_neighbor(side:int) -> NodePath",
	"get_focus_next() -> NodePath",
	"get_focus_previous() -> NodePath",
	"get_global_position() -> Vector2",
	"get_global_rect() -> Rect2",
	"get_h_grow_direction() -> int",
	"get_h_size_flags() -> int",
	"get_layout_direction() -> int",
	"get_minimum_size() -> Vector2",
	"get_mouse_filter() -> int",
	"get_offset(offset:int) -> float",
	"get_parent_area_size() -> Vector2",
	"get_parent_control() -> Object",
	"get_pivot_offset() -> Vector2",
	"get_position() -> Vector2",
	"get_rect() -> Rect2",
	"get_rotation() -> float",
	"get_rotation_degrees() -> float",
	"get_scale() -> Vector2",
	"get_screen_position() -> Vector2",
	"get_shortcut_context() -> Object",
	"get_size() -> Vector2",
	"get_stretch_ratio() -> float",
	"get_theme() -> Object",
	"get_theme_color(name:StringName,theme_type:StringName) -> Color",
	"get_theme_constant(name:StringName,theme_type:StringName) -> int",
	"get_theme_default_base_scale() -> float",
	"get_theme_default_font() -> Object",
	"get_theme_default_font_size() -> int",
	"get_theme_font(name:StringName,theme_type:StringName) -> Object",
	"get_theme_font_size(name:StringName,theme_type:StringName) -> int",
	"get_theme_icon(name:StringName,theme_type:StringName) -> Object",
	"get_theme_stylebox(name:StringName,theme_type:StringName) -> Object",
	"get_theme_type_variation() -> StringName",
	"get_tooltip(at_position:Vector2) -> String",
	"get_tooltip_text() -> String",
	"get_v_grow_direction() -> int",
	"get_v_size_flags() -> int",
	"grab_click_focus() -> void",
	"grab_focus() -> void",
	"has_focus() -> bool",
	"has_theme_color(name:StringName,theme_type:StringName) -> bool",
	"has_theme_color_override(name:StringName) -> bool",
	"has_theme_constant(name:StringName,theme_type:StringName) -> bool",
	"has_theme_constant_override(name:StringName) -> bool",
	"has_theme_font(name:StringName,theme_type:StringName) -> bool",
	"has_theme_font_override(name:StringName) -> bool",
	"has_theme_font_size(name:StringName,theme_type:StringName) -> bool",
	"has_theme_font_size_override(name:StringName) -> bool",
	"has_theme_icon(name:StringName,theme_type:StringName) -> bool",
	"has_theme_icon_override(name:StringName) -> bool",
	"has_theme_stylebox(name:StringName,theme_type:StringName) -> bool",
	"has_theme_stylebox_override(name:StringName) -> bool",
	"is_auto_translating() -> bool",
	"is_clipping_contents() -> bool",
	"is_drag_successful() -> bool",
	"is_force_pass_scroll_events() -> bool",
	"is_layout_rtl() -> bool",
	"is_localizing_numeral_system() -> bool",
	"release_focus() -> void",
	"remove_theme_color_override(name:StringName) -> void",
	"remove_theme_constant_override(name:StringName) -> void",
	"remove_theme_font_override(name:StringName) -> void",
	"remove_theme_font_size_override(name:StringName) -> void",
	"remove_theme_icon_override(name:StringName) -> void",
	"remove_theme_stylebox_override(name:StringName) -> void",
	"reset_size() -> void",
	"set_anchor(side:int,anchor:float,keep_offset:bool,push_opposite_anchor:bool) -> void",
	"set_anchor_and_offset(side:int,anchor:float,offset:float,push_opposite_anchor:bool) -> void",
	"set_anchors_and_offsets_preset(preset:int,resize_mode:int,margin:int) -> void",
	"set_anchors_preset(preset:int,keep_offsets:bool) -> void",
	"set_auto_translate(enable:bool) -> void",
	"set_begin(position:Vector2) -> void",
	"set_clip_contents(enable:bool) -> void",
	"set_custom_minimum_size(size:Vector2) -> void",
	"set_default_cursor_shape(shape:int) -> void",
	"set_drag_forwarding(drag_func:Callable,can_drop_func:Callable,drop_func:Callable) -> void",
	"set_drag_preview(control:Object) -> void",
	"set_end(position:Vector2) -> void",
	"set_focus_mode(mode:int) -> void",
	"set_focus_neighbor(side:int,neighbor:NodePath) -> void",
	"set_focus_next(next:NodePath) -> void",
	"set_focus_previous(previous:NodePath) -> void",
	"set_force_pass_scroll_events(force_pass_scroll_events:bool) -> void",
	"set_global_position(position:Vector2,keep_offsets:bool) -> void",
	"set_h_grow_direction(direction:int) -> void",
	"set_h_size_flags(flags:int) -> void",
	"set_layout_direction(direction:int) -> void",
	"set_localize_numeral_system(enable:bool) -> void",
	"set_mouse_filter(filter:int) -> void",
	"set_offset(side:int,offset:float) -> void",
	"set_offsets_preset(preset:int,resize_mode:int,margin:int) -> void",
	"set_pivot_offset(pivot_offset:Vector2) -> void",
	"set_position(position:Vector2,keep_offsets:bool) -> void",
	"set_rotation(radians:float) -> void",
	"set_rotation_degrees(degrees:float) -> void",
	"set_scale(scale:Vector2) -> void",
	"set_shortcut_context(node:Object) -> void",
	"set_size(size:Vector2,keep_offsets:bool) -> void",
	"set_stretch_ratio(ratio:float) -> void",
	"set_theme(theme:Object) -> void",
	"set_theme_type_variation(theme_type:StringName) -> void",
	"set_tooltip_text(hint:String) -> void",
	"set_v_grow_direction(direction:int) -> void",
	"set_v_size_flags(flags:int) -> void",
	"update_minimum_size() -> void",
	"warp_mouse(position:Vector2) -> void"
]
```
### get_arg_str(args_arr:Array) -> String
返回函数参数列表字符串。仅用于`get_method_lists()`方法调用。
### get_return_str(return_dic:Dictionary) -> String
返回函数return的类型。仅用于`get_method_lists()`方法调用。
### get_type_name(type:int) -> String
返回对应数据类型的名称。用于辅助其他方法，快速获取数据类型名称。
例如：
```swift
print(ClassInfo.get_type_name(typeof("hahaha")))  # 输出String
print(ClassInfo.get_type_name(typeof(1)))  # 输出Int
	
var arr = []
print(ClassInfo.get_type_name(typeof(arr)))  # 输出Array
# ...以此类推
```
## 源代码
以下是完整源代码：
```swift
# ========================================================
# 名称：ClassInfo
# 类型：静态函数库
# 简介：基于ClassDB，获得更加简化和丰富的内部类信息
# 作者：巽星石
# Godot版本：4.0.2-stable (official)
# 创建时间：2023-04-18 22:26:03
# 最后修改时间：2023年4月19日00:40:45
# ========================================================

class_name ClassInfo

# =================================== 获取内置类成员列表 ===================================
# 获取内置类的信号列表
static func get_signal_lists(className:String) -> PackedStringArray:
	var signal_arr:PackedStringArray = []
	var list = ClassDB.class_get_signal_list(className,true)
	for sig in list:
		var args:PackedStringArray = []
		for arg in sig["args"]:
			args.append(arg["name"])
		signal_arr.append("%s(%s)" % [sig["name"],",".join(args)])
	signal_arr.sort() # 按字母进行升序
	return signal_arr

# 获取内置类的枚举列表
static func get_enum_lists(className:String) -> Array[Dictionary]:
	var enums_arr:Array[Dictionary] = []
	var enum_list = ClassDB.class_get_enum_list(className,true)
	for enm in enum_list:
		var enum_dict = {}
		enum_dict["name"] = enm
		enum_dict["vals"] = ClassDB.class_get_enum_constants(className,enm,true)
		enums_arr.append(enum_dict)
	enums_arr.sort() # 按字母进行升序
	return enums_arr

# 获取内置类的变量列表
static func get_property_lists(className:String) -> Array[Dictionary]:
	var prop_arr:Array[Dictionary] = []
	var list = ClassDB.class_get_property_list(className,true)
	# 按照属性名称字母进行升序排列
	list.sort_custom(func(a,b):
		return a["name"] < b["name"]
	)
	for prop in list:
		var dict = {}
		dict["name"] = prop["name"]
		if prop["type"] != 24: # 不是object类型
			dict["type"] = get_type_name(prop["type"])
		else:
			dict["type"] = prop["class_name"]
		prop_arr.append(dict)
	return prop_arr

# 返回一个Script对象的方法字符串列表
static func get_method_lists(className:String) -> PackedStringArray:
	var list = ClassDB.class_get_method_list(className,true)
	# 按照属性名称字母进行升序排列
	list.sort_custom(func(a,b):
		return a["name"] < b["name"]
	)
	var str_list:PackedStringArray = []
	for mtod in list:
		var aa = "%s(%s) -> %s" % [mtod["name"],get_arg_str(mtod["args"]),get_return_str(mtod["return"])]
		str_list.append(aa)
	return str_list

# +++++++++++++++++ 以下两个函数仅用于get_method_lists()方法调用 +++++++++++++++++
# 返回函数参数列表字符串
static func get_arg_str(args_arr:Array) -> String:
	var str_list:PackedStringArray = []
	if args_arr.size() == 0:
		return ""
	else:
		for arg in args_arr:
			var aa = "%s:%s" % [arg["name"],get_type_name(arg["type"])]
			str_list.append(aa)
		return ",".join(str_list)

# 返回函数return的类型字符串
static func get_return_str(return_dic:Dictionary) -> String:
	var rt_str = "void"
	if return_dic["type"] != 0:
		rt_str = get_type_name(return_dic["type"])
	return rt_str

# 根据类型返回类型名称
static func get_type_name(type:int) ->String:
	var name_str_arr = ["null","bool","int","float","String","Vector2","Vector2i","Rect2","Rect2i","Vector3",
						"Vector3i","Transform2D","Vector4","Vector4i","Plane","Quaternion","AABB","Basis","Transform3D","Projection",
						"Color","StringName","NodePath","RID","Object","Callable","Signal","Dictionary","Array","PackedByteArray",
						"PackedInt32Array","PackedInt64Array","PackedFloat32Array","PackedFloat64Array","PackedStringArray","PackedVector2Array","PackedVector3Array","PackedColorArray"]
	return name_str_arr[type]

```


