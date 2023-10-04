```swift
# ======================================================================
# 名称：MMCreator
# 描述：创建FreeMind格式文档的工具类
# 类型：类
# 作者：巽星石
# Godot版本：v4.0.2.stable.official [7a0977ce2]
# 创建时间：2022年10月22日15:43:03
# 最后修改时间：2023年4月8日12:36:06
# ======================================================================
@tool
class_name MMCreator

# 根据字典生成freemind文档格式代码
func freemind_doc_code(dic:Dictionary) -> String:
	var code_str = double_tag("map",freemind_node_code(dic),"version=\"1.0.1\"")
	return code_str

# 根据字典生成freemind文档节点代码
func freemind_node_code(dic:Dictionary) -> String:
	var code_str = ""
	if dic["child"].size() >0:
		var ctn = ""
		for child_dic in dic["child"]:
			ctn += freemind_node_code(child_dic)
		code_str += double_tag("node",ctn,"TEXT=\"%s\"" % utf8_to_utf16BE(dic["text"]))
	else:
		code_str += single_tag("node","TEXT=\"%s\"" % utf8_to_utf16BE(dic["text"]))
	return code_str

# 闭合标签 - 返回一对闭合标签 - <tag[attrs]>string</tag>
static func double_tag(tag:String,string:String = "",attrs:String = "") -> String:
	var htm_str = "<%s%s>\n%s</%s>\n" % [tag,(" " + attrs if attrs!="" else ""),string,tag]
	return htm_str

# 单标签 - 返回一个自闭合标签 - <tag[attrs] />
static func single_tag(tag:String,attrs:String = "") -> String:
	var htm_str = "<%s%s />\n" % [tag,(" " + attrs if attrs!="" else "")]
	return htm_str

# ======================================================================
# 核心函数
# 将UTF-8编码转为UTF-16BE编码（汉字适用，其他的不保证）
func utf8_to_utf16BE(string:String):
	var utf_16be = ""
	for i in string.length():
		if string[i].to_utf8_buffer().size() == 3: # 汉字或相应标点
			utf_16be += utf8_char_to_utf_16_hex(string[i])
		else: # 英文、数字等保留原样
			utf_16be += string[i]
	utf_16be = utf_16be.replace("\"","&quot;")
	utf_16be = utf_16be.replace("<","&lt;")
	utf_16be = utf_16be.replace(">","&gt;")
	return utf_16be
# ======================================================================

func utf8_char_to_utf_16_hex(a_char:String):
	# 获得单个字符的utf8三位二进制数
	var utf8_two = to_utf8_two(a_char)
	var sttr = ""
	sttr += utf8_two[0].trim_prefix("1110")
	sttr += utf8_two[1].trim_prefix("10")
	sttr += utf8_two[2].trim_prefix("10")
	# 转为utf_16的两位十进制
	var arr = []
	arr.append(two_to_ten(int(sttr.left(8))))
	arr.append(two_to_ten(int(sttr.right(8))))
	# 转16进制
	arr = "&#x%s;" % String(PackedByteArray(arr).hex_encode())
	return arr

# 获得单个字符的utf8三位二进制数
func to_utf8_two(a_char:String) -> PackedStringArray:
	var arr = a_char.to_utf8_buffer()
	var new_arr = []
	new_arr.append(ten_to_two(arr[0]))
	new_arr.append(ten_to_two(arr[1]))
	new_arr.append(ten_to_two(arr[2]))
	return new_arr

# 简单的十进制转二进制
func ten_to_two(num:int) -> String:
	var two = ""
	if num <2:
		two = num
	else:
		while num/2 >= 1:
			var md = posmod(num,2)
			two = str(md) + two
			num = num/2
		if num/2 < 1:
			var md = posmod(num,2)
			two = str(md) + two
	return two


# 简单的二进制转十进制
func two_to_ten(num:int) -> int:
	var num_str = str(num)
	var new_num = 0
	var sttr = ""
	for n in range(num_str.length()):
		var i = int(num_str[n])
		new_num += i * pow(2,num_str.length()-n-1)
	return new_num

```
