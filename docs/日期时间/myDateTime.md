```swift
# ======================================================================
# 名称：myDateTime
# 描述：关于如期和时间的静态函数库(根据Godot 4.0API重写)
# 说明：日期字符串必须传入 YYYY-MM-DD HH:MM:SS 或 YYYY-MM-DD 或 HH:MM:SS 形式
#      之一，否则判定为无效
# 类型：静态函数库
# 作者：巽星石
# Godot版本：v4.0.2.stable.official [7a0977ce2]
# 创建时间：2022年10月26日18:08:26
# 最后修改时间：2023年4月8日12:31:18
# ======================================================================

class_name myDateTime

# 判断一个字符串的每个字符是不是0-9
static func str_is_int(string:String) -> bool:
	var bol = true
	for i in string.length():
		if not string[i].to_utf8_buffer()[0] in range(48,58): # 48-57为0-9的UTF编码
			bol = false
	return bol



# 判断一个字符串是否是有效的日期时间字符串
# 必须传入 YYYY-MM-DD HH:MM:SS 形式
static func is_valid_DateTimeString(string:String) -> bool:
	var bol = false
	if string.length() == 19: 
		if "-" in string and ":" in string and " " in string:
			# 去除" "、":"和"-"
			var rp_str:String = string.replace(" ","")
			rp_str = rp_str.replace(":","")
			rp_str = rp_str.replace("-","")
			if rp_str.length() == 14:
				# 判断剩余的字符是否全部是0-9
				if str_is_int(rp_str):
					bol = true
	return bol

# 判断一个字符串是否是有效的日期字符串
# 必须传入 YYYY-MM-DD 形式
static func is_valid_DateString(string:String) -> bool:
	var bol = false
	if string.length() == 10: # YYYY-MM-DD
		if "-" in string:
			var rp_str = string.replace("-","")
			if rp_str.length() == 8:
				# 判断剩余的字符是否全部是0-9
				if str_is_int(rp_str):
					bol = true
	return bol

# 判断一个字符串是否是有效的时间字符串
# 必须传入 HH:MM:SS 形式
static func is_valid_TimeString(string:String) -> bool:
	var bol = false
	if string.length() == 8:  # HH:MM:SS
		if ":" in string:
			var rp_str = string.replace(":","")
			if rp_str.length() == 6:
				# 判断剩余的字符是否全部是0-9
				if str_is_int(rp_str):
					bol = true
	return bol

# 返回当前日期时间字符串
# 形如： YYYY-MM-DD HH:MM:SS
static func Now() -> String:
	return Time.call("get_datetime_string_from_system(false,true)").replace("T"," ")

# 返回当前日期字符串
# 形如： YYYY-MM-DD
static func Date() -> String:
	return Now().get_slice(" ",0)

# 返回当前时间字符串
# 形如： HH:MM:SS
static func Time() -> String:
	return Now().get_slice(" ",1)

# 返回一个日期时间字符串的年份
static func Year(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["year"]
	return ele

# 返回一个日期时间字符串的月份
static func Month(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["month"]
	return ele

# 返回一个日期时间字符串的日
static func Day(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["day"]
	return ele

# 返回一个日期时间字符串的小时
static func Hour(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["hour"]
	return ele

# 返回一个日期时间字符串的分钟
static func Minute(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["minute"]
	return ele

# 返回一个日期时间字符串的秒
static func Second(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		ele = dic["second"]
	return ele

# 返回一个日期时间字符串所对应日期是星期几（数字）
# 注意：周一到周六对应1-6,周日对应0
static func Weekday(datetime:String = "") -> int:
	var ele
	if get_datetime_dic(datetime):
		var dic = get_datetime_dic(datetime)
		if dic.has("weekday"):
			ele = dic["weekday"]
	return ele

# 返回一个日期时间字符串所对应日期是星期几（文字）
static func WeekdayName(datetime:String = "") -> String:
	var wd_names = ["日","一","二","三","四","五","六"]
	var wd = Weekday(datetime)
	var wd_name = "星期%s" % wd_names[wd]
	return wd_name

# 返回日期时间数据字典，形式如下：
# {day:00, hour:0, minute:0, month:0, second:0, weekday:0, year:0000}
static func get_datetime_dic(datetime:String = "") -> Dictionary:
	var dic
	if datetime == "":
		datetime = Now()
	var data_string:String = ""
	if is_valid_DateTimeString(datetime): # YYYY-MM-DD HH:MM:SS
		data_string = datetime.replace(" ","T")
	else:
		if is_valid_DateString(datetime): # YYYY-MM-DD
			data_string = datetime
		else: 
			if is_valid_TimeString(datetime): # HH:MM:SS
				dic = {}
				dic["hour"] = int(datetime.get_slice(":",0))
				dic["minute"] = int(datetime.get_slice(":",1))
				dic["second"] = int(datetime.get_slice(":",2))
				return dic
			else: # 非规定格式字符串
				push_error(datetime + "并不是一个有效的日期时间字符串。\n\t请传入YYYY-MM-DD HH:MM:SS或YYYY-MM-DD形式的字符串。")
	if data_string != "":
		dic = Time.call("get_datetime_dict_from_datetime_string(data_string,true)")
	return dic

# 简单的格式化字符串，仅用于日期和时间字串中位数不足时前面补0
static func FormatString(num:int,length:int = 2):
	var num_str
	if str(num).length()<length: # 不足位数 - 前面补0
		num_str = "0".repeat(length- str(num).length()) + str(num)
	else:
		num_str = str(num)
	return num_str

# 返回一个标准的日期时间字符串（非严谨）
static func DateTimeString(year:int,month:int,day:int,hour:int = 0,munite:int = 0,second:int = 0):
	# 验证传入的数据有效性
	if not year in range(10000):
		push_error("%s不是有效的year，请传入0到9999之间的整数" % year)
		return null
	if not month in range(1,13):
		push_error("%s不是有效的month，请传入1到12之间的正整数" % month)
		return null
	if not day in range(1,get_month_day_count(year,month)):
		push_error("%s不是有效的day，%s年%s月有%s天，请传入1到%s之间的正整数" % [day,year,month,get_month_day_count(year,month),get_month_day_count(year,month)])
		return null
	if not hour in range(24):
		push_error("%s不是有效的hour，请传入0到23之间的整数" % hour)
		return null
	if not munite in range(60):
		push_error("%s不是有效的munite，请传入0到59之间的整数" % munite)
		return null
	if not second in range(60):
		push_error("%s不是有效的second，请传入0到59之间的整数" % second)
		return null
	var arr = [year,month,day,hour,munite,second]
	for i in arr.size():
		if i == 0:
			arr[0] = FormatString(arr[0],4)
		else:
			arr[i] = FormatString(arr[i])
	return "%s-%s-%s %s:%s:%s" % arr


# 返回距离一个日期多少天的新日期
static func AfterDays(datetimeFrom:String,days:int) -> String:
	var date
	if is_valid_DateString(datetimeFrom) or is_valid_DateTimeString(datetimeFrom):
		var year = Year(datetimeFrom)
		var month = Month(datetimeFrom)
		var day = Day(datetimeFrom)
		# 时间不用动
		var hour = Hour(datetimeFrom)
		var munite = Minute(datetimeFrom)
		var second = Second(datetimeFrom)
		
		# 计算逻辑
		if days>=0: # 正数
			var month_days = get_month_day_count(year,month) # 本月天数
			if month_days - day >0: # 本月剩余天数>0
				var already_has_days = month_days - day # 本月剩余天数
				if days>already_has_days:      # days大于本月剩余天数
					# 从days里减去本月剩余天数
					days -= already_has_days
					# 月份+1
					if month + 1 <= 12:
						month += 1
					else: # 月份>12
						year += 1  # 年+1
						month = 1  # 月=1
					# 日归零
					day = 0
					# 计算下月
					var is_ok = false
					while !is_ok:
						month_days = get_month_day_count(year,month) # 下月天数
						if days <= month_days:
							day += days
							is_ok = true
							date = DateTimeString(year,month,day,hour,munite,second)
						else:
							days -= month_days
							# 月份+1
							if month + 1 <= 12:
								month += 1
							else: # 月份>12
								year += 1  # 年+1
								month = 1  # 月=1
					
				else:                          # days小于等于本月剩余天数
					day += days
					# 直接返回生成的字符串
					date = DateTimeString(year,month,day,hour,munite,second)
		else: # 负数
			days = -days # 取正
			var is_ok = false
			while !is_ok:
				if days < day:
					day -= days
					is_ok = true
					date = DateTimeString(year,month,day,hour,munite,second)		
				else:
					days -= day
					if month - 1 >0:
						month -= 1
					else:
						year -= 1
						month = 12
					day = get_month_day_count(year,month)
					if days == 0:
						is_ok = true
						date = DateTimeString(year,month,day,hour,munite,second)
	return date


# 判断是否是闰年（leap year）
static func is_leap_year(year:int):
	var bol = false
	if posmod(year,4) == 0:
		bol = true
		if posmod(year,100) == 0: # 年份是100的倍数
			if posmod(year,400) == 0: # 必须是400的整数倍
				bol = true
			else:
				bol = false
	return bol			

# 返回某年的某月共有多少天	
static func get_month_day_count(year:int,month:int) -> int:
	var day_count = 0
	match month:
		2: # 2月
			if is_leap_year(year):# 闰年
				day_count = 29
			else:
				day_count = 28
		1,3,5,7,8,10,12: # 大月
			day_count = 31
		4,6,9,11: # 小月
			day_count = 30
	return day_count

# 返回某年共有多少天	
static func get_year_day_count(year:int) -> int:
	var day_count = 0
	for i in range(12):
		day_count += get_month_day_count(year,i+1)
	return day_count

```







