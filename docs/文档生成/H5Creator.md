```swift
# ======================================================================
# 名称：H5Creator Godot4.0版本
# 描述：HTML快速生成（主要用于文档和报表生成，与游戏无关）
# 说明：本函数库基于HTML5所支持的语法进行创建
#      H5页面基础框架以VScode生成的框架为基础
# 类型：函数库
# 作者：@巽星石
# Godot版本：
# 创建时间：2022年9月25日10:57:49
# 最后修改时间：2023年8月12日02:34:45
# ======================================================================

class_name H5Creator
# =================================== Markdown解析 ===================================

static func css():
	return """
<style>
pre{margin: 0px;padding: 0px;}
.inline_code{
	padding:0px 5px;margin:0px 2px;
	border:1px solid #ccc;
	background-color: #eee;
	border-radius: 5px;
	font-size: smaller;
}
.code_block{
	padding:5px;margin: 5px;
	border:1px solid #ccc;
	background-color: #eee;
	border-radius: 5px;
	font-size: smaller;
	overflow-x: auto;
}
img{display: block;margin: 5px auto;max-width: 700px;border-radius: 5px;border:1px solid #ccc;}
table,tr,td{border:1px solid #ccc;border-spacing: 0px;border-collapse: collapse;padding: 5px;font-size: smaller;}
table{margin:5px 0px;}
tr:nth-child(1) {background-color:#e5ffff;border-radius: 5px;text-align: center;font-weight: bold;}
</style>
"""

# Markdown --> HTML
static func parse(md:String):
	# Markdown:H1~H6 --> HTML:H1~H6
	for i in range(6,0,-1):
		md = parse_h(i,md)
	
	# Markdown:**text** --> HTML:<b>text</b>
	md = parse_bold(md)
	# Markdown:图片 --> HTML:图片
	md = parse_img(md)
	# Markdown:超链接 --> HTML:超链接
	md = parse_link(md)
	# Markdown:table --> HTML:table
	md = parse_table(md)
	md = md.replace("\n\n<tr><td>","<table><tr><td>")
	md = md.replace("</tr>\n\n","</table>")
		
	# Markdown:代码块 --> HTML:span.code_block
	md = parse_code_block(md)
	# Markdown:行内代码 --> HTML:span.inline_code
	md = parse_inline_code(md)
	# Markdown:ul --> HTML:ul
	md = parse_ul(md)
	
	
	
	md = md.replace("\n\n","</p>")
	md = md.replace("___","<hr>")
	return css() + md

# Markdown:H1~H6 --> HTML:H1~H6
static func parse_h(level:int,md:String):
	
	var reg = RegEx.new()
	reg.compile("%s (.*)" % "#".repeat(level))
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_title = mat.get_string()
			var title = mat.get_string(1)
			md = md.replace(md_title,H(level,title))
	return md

# Markdown:ul --> HTML:ul
static func parse_ul(md:String):
	
	var reg = RegEx.new()
	reg.compile("- (.*)")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_ul = mat.get_string()
			var title = mat.get_string(1)
			md = md.replace(md_ul,double_tag("li",title))
	return md

# Markdown:table --> HTML:table
static func parse_table(md:String):

	var reg = RegEx.new()
	reg.compile("|([^-\\n]*)|".replace("|","\\|"))

	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_td = mat.get_string()
			var text = mat.get_string(1)
			var tr = "<tr><td>%s</tr>" % text
			tr = tr.replace("|","<td>")
			md = md.replace(md_td,tr)
	# 去除 | --- |形式
	reg.compile("|([-]*.*)|\n".replace("|","\\|"))
	result = reg.search_all(md)
	if result:
		for mat in result:
			var md_line = mat.get_string()
			md = md.replace(md_line,"")
	return md


# Markdown:**text** --> HTML:<b>text</b>
static func parse_bold(md:String):
	
	var reg = RegEx.new()
	reg.compile("[\\*]{2}(.+)[\\*]{2}")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_bold = mat.get_string()
			var content = mat.get_string(1)
			md = md.replace(md_bold,double_tag("b",content))
	return md



# Markdown:行内代码 --> HTML:span.inline_code
static func parse_inline_code(md:String):
	
	var reg = RegEx.new()
	reg.compile("`([^`]+)`")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_inline = mat.get_string()
			var code = mat.get_string(1)
			var span = "<span class=\"inline_code\">%s</span>" % code
			md = md.replace(md_inline,span)
	return md

# Markdown:代码 --> HTML:span.code_block
static func parse_code_block(md:String):
	
	var reg = RegEx.new()
	reg.compile("```\n([^`]+)```")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_code = mat.get_string()
			var code = mat.get_string(1)
			var pre = double_tag("pre",code)
			var div = double_tag("div",pre,"class=\"code_block\"")
			
			md = md.replace(md_code,div)
	return md


# Markdown:超链接 --> HTML:超链接
static func parse_link(md:String,target:String = "_blank"):
	
	var reg = RegEx.new()
	reg.compile("\\[(.*)\\]\\((.*)\\)")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_url = mat.get_string()
			var title = mat.get_string(1)
			var url = mat.get_string(2)
			md = md.replace(md_url,A(url,title,"target=\"%s\"" % target))
	return md

# Markdown:图片 --> HTML:图片
static func parse_img(md:String,target:String = "_blank"):
	
	var reg = RegEx.new()
	reg.compile("!\\[(.*)\\]\\((.*)\\)")
	
	var result:Array[RegExMatch] = reg.search_all(md)
	if result:
		for mat in result:
			var md_img = mat.get_string()
			var desc = mat.get_string(1)
			var src = mat.get_string(2)
			md = md.replace(md_img,img(src,"alt=\"%s\"" % desc))
	return md

# ========================= 标签函数 =========================
# H1-H6
static func H(level:int,title:String) -> String:
	level = clamp(level,1,6)
	return double_tag("h%s" % level,title)

# 段落 - <p>content</p>
static func P(content:String) -> String:
	return double_tag("p",content)


# 超链接 - <a href=[url]>title</a> | <a href=[url]>[url]</a>
static func A(url:String = "",title:String = "",attrs:String = "") -> String:
	var ctn = title if title != "" else url
	var attr = ("href='%s' %s" % [url,attrs]) if (url !="") else ""
	return double_tag("a",ctn,attr)

# 图片 - <img src='[url]' />
static func img(url:String,attrs:String = "") -> String:
	var attr = ("src='%s' %s" % [url,attrs]) if (url !="") else ""
	return single_tag("img",attr)

# OL - 有序列表
static func OL(str_list:PackedStringArray) -> String:
	var return_str = ""
	var ol_str = "<ol>\n%s</ol>\n"
	var li_str = "<li>%s</li>\n"
	for i in str_list.size():
		return_str += li_str % str_list[i]
	return_str = ol_str % return_str
	return  return_str

# UL - 无序列表
static func UL(str_list:PackedStringArray) -> String:
	var return_str = ""
	var ul_str = "<ul>\n%s</ul>\n"
	var li_str = "<li>%s</li>\n"
	for i in str_list.size():
		return_str += li_str % str_list[i]
	return_str = ul_str % return_str
	return  return_str

# HR - 水平分隔线
static func HR() -> String:
	return "<hr />"

# BR - 强制换行
static func BR() -> String:
	return "
"


# table - 表格
static func table(fields:PackedStringArray,rows:Array) -> String:
	var th = TH(fields)
	var trs = ""
	for row in rows:
		trs += TR(row)
	return double_tag("table",th + trs,"border=0")


# 表格 - 表头
static func TH(fields:PackedStringArray) -> String:
	var tr = double_tag("tr","%s")
	var ths = ""
	for fd in fields:
		ths +=  double_tag("th","%s") % fd
	tr = tr % ths
	return tr
	
# 表格 - 行
static func TR(vals:PackedStringArray) -> String:
	var tr = double_tag("tr","%s")
	var tds = ""
	for val in vals:
		tds +=  double_tag("td","%s") % val
	tr = tr % tds
	return tr

# === 表单元素 === 
# input[text] - <input type='text'[name][size][maxlength] />
static func input_text(name:String = "",size:int=0,maxlength:int=0) -> String:
	var ipt_text:String = single_tag("input","type='text'[name][size][maxlength]")
	ipt_text = ipt_text.replace("[name]"," name='%s'" % name if name != "" else "")
	ipt_text = ipt_text.replace("[size]"," size='%s'" % size if size != 0 else "")
	ipt_text = ipt_text.replace("[maxlength]"," maxlength='%s'" % maxlength if maxlength != 0 else "")
	return ipt_text

# input[password] - <input type='password'[name] />
static func input_password(name:String = "") -> String:
	var ipt_text:String = single_tag("input","type='password'[name]")
	ipt_text = ipt_text.replace("[name]"," name='%s'" % name if name != "" else "")
	return ipt_text

# input[checkbox] - <input type='checkbox'[name][checked] />
static func input_checkbox(name:String = "",checked:bool = false) -> String:
	var ipt_text:String = single_tag("input","type='heckbox'[name][checked]")
	ipt_text = ipt_text.replace("[name]"," name='%s'" % name if name != "" else "")
	ipt_text = ipt_text.replace("[checked]"," checked='checked'" if checked else "")
	return ipt_text

# input[radio] - <input type='radio'[name][checked] />
static func input_radio(name:String = "",checked:bool = false) -> String:
	var ipt_text:String = single_tag("input","type='radio'[name][checked]")
	ipt_text = ipt_text.replace("[name]"," name='%s'" % name if name != "" else "")
	ipt_text = ipt_text.replace("[checked]"," checked='checked'" if checked else "")
	return ipt_text
	
# input[submit] - <input type='submit' value='[value]' />
static func input_submit(value:String = "确定") -> String:
	return single_tag("input","type='submit' value='%s'" % value)

# input[reset] - <input type='reset' value='[value]' />
static func input_reset(value:String = "清空") -> String:
	return single_tag("input","type='reset' value='%s'" % value)

# input[hidden] - <input type='hidden' value='[value]' />
static func input_hidden(value:String = "") -> String:
	return single_tag("input","type='hidden' value='%s'" % value)

# select>option - <option[selected]>string</option>
static func input_option(string:String = "",selected:bool = false) -> String:
	var opt_text:String = double_tag("option","%s","[selected]")
	opt_text = opt_text % string
	opt_text = opt_text.replace("[selected]"," selected='selected'" if selected else "")
	return opt_text

# select
# - <select[name]>
# - [options]
# - </select>
static func input_select(name:String = "",options:PackedStringArray = []) -> String:
	var sel_text:String = double_tag("select","%s","[name]")
	sel_text = sel_text.replace("[name]","name='%s'" % name if name != "" else "")
	var options_str = ""
	if options.size() >0:
		for opt in options:
			options_str += input_option(opt)
	sel_text = sel_text % options_str
	return sel_text


# textarea - <textarea[name][rows][cols]>string</textarea>
static func input_textarea(name:String = "",string:String = "",rows:int = 0,cols:int =0) -> String:
	var txt_text:String = double_tag("textarea","\n%s\n" % string,"[name][rows][cols]")
	txt_text = txt_text.replace("[name]","name='%s'" % name if name != "" else "")
	txt_text = txt_text.replace("[rows]"," rows='%s'" % rows if rows != 0 else "")
	txt_text = txt_text.replace("[cols]"," cols='%s'" % cols if cols != 0 else "")
	return txt_text

# form - <form[action][method]>\n[inputs_str]\n</form>
static func form(inputs_str:String = "",action:String = "",method:String = "") -> String:
	var form_text:String = double_tag("form","\n%s\n" % inputs_str,"[action][method]")
	form_text = form_text.replace("[action]","action='%s'" % action if action != "" else "")
	form_text = form_text.replace("[method]"," method='%s'" % method if method != "" else "")
	return form_text

# ========================= 底层 =========================

# 闭合标签 - 返回一对闭合标签 - <tag[attrs]>string</tag>
static func double_tag(tag:String,string:String = "",attrs:String = "") -> String:
	var htm_str = "<%s%s>%s</%s>\n" % [tag,(" " + attrs if attrs!="" else ""),string,tag]
	return htm_str

# 单标签 - 返回一个自闭合标签 - <tag[attrs] />
static func single_tag(tag:String,attrs:String = "") -> String:
	var htm_str = "<%s%s />\n" % [tag,(" " + attrs if attrs!="" else "")]
	return htm_str
# ========================= 额外需要 =========================
# 返回当前日期时间字符串
# 形如： YYYY-MM-DD HH:MM:SS
static func Now() -> String:
	return Time.get_datetime_string_from_system(false,true).replace("T"," ")

```
