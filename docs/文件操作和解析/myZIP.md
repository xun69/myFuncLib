## 概述
Godot提供了`ZIPPacker`和`ZIPReader`两个类，用于创建和读取ZIP压缩包。
本静态函数库基于Godot提供的这两个类，实现了一些简易的辅助函数，可以让你更轻松的创建和读取ZIP压缩包。
## 源代码
```swift
# ===========================
# myZIP
# 基于ZIPPacker和ZIPReader封装的ZIP压缩包创建和读取辅助函数库
# Godot 4.0.3.stable.official [5222a99f5]
# 巽星石
# 2023年6月15日21:54:26
# 2023年6月15日21:54:53
# ===========================
class_name myZIP
# =========================== 读取ZIP ===========================
# 读取ZIP
static func read_zip(zip_path:String,call_back:Callable):
	var zip_readder := ZIPReader.new()
	var err := zip_readder.open(zip_path)
	if err!= OK:
		return err
	call_back.call(zip_readder)
	zip_readder.close()

# 从ZIP压缩包中读取并返回JPG图片
static func get_zip_image(zip_reader:ZIPReader,file_name:String) -> ImageTexture:
	var img_bytes =zip_reader.read_file(file_name)
	var img = Image.new()
	var ext = file_name.get_extension()
	if ext in ["jpg","png","bmp","tga","webp"]:
		img.call("load_%s_from_buffer" % ext,img_bytes)
	else:
		return
	return ImageTexture.create_from_image(img)



# 返回ZIP压缩包中指定名称的纯文本文件的文本内容
static func get_file_txt(zip_reader:ZIPReader,file_name:String) -> String:
	var txt_bytes = zip_reader.read_file(file_name)
	return txt_bytes.get_string_from_utf8()

# 返回ZIP压缩包中指定名称的二进制文件的字节数组内容
static func get_file_bytes(zip_reader:ZIPReader,file_name:String) -> PackedByteArray:
	var txt_bytes = zip_reader.read_file(file_name)
	return txt_bytes

# =========================== 读取ZIP ===========================
# 创建ZIP
static func create_zip(zip_path:String,call_back:Callable):
	var zip_packer := ZIPPacker.new()
	var err := zip_packer.open(zip_path,ZIPPacker.APPEND_CREATE)
	if err != OK:
		return err
	call_back.call(zip_packer)
	zip_packer.close_file()
	zip_packer.close()
	return OK

# 在zip中添加已经存在的纯文本文件
static func put_txt_file(zip_packer:ZIPPacker,file_name:String,file_path:String):
	zip_packer.start_file(file_name)
	var file_content = get_txt_file_string(file_path)
	zip_packer.write_file(file_content.to_utf8_buffer())

# 在zip中添加已经存在的二进制文件
static func put_binary_file(zip_packer:ZIPPacker,file_name:String,file_path:String):
	zip_packer.start_file(file_name)
	var file_content = get_binary_file_bytes(file_path)
	zip_packer.write_file(file_content)

# 在zip中写入纯文本文件
static func write_txt_file(zip_packer:ZIPPacker,file_name:String,file_content:String):
	zip_packer.start_file(file_name)
	zip_packer.write_file(file_content.to_utf8_buffer())

# 在zip中写入二进制文件
static func write_binary_file(zip_packer:ZIPPacker,file_name:String,file_content:PackedByteArray):
	zip_packer.start_file(file_name)
	zip_packer.write_file(file_content)

# =========================== 辅助函数 ===========================
# 返回文件的二进制字节数组形式
static func get_binary_file_bytes(file_path:String):
	return 	FileAccess.get_file_as_bytes(file_path)

# 返回纯文件的字符串内容
static func get_txt_file_string(file_path:String):
	return 	FileAccess.get_file_as_string(file_path)

```
## 用法
### 创建ZIP压缩包
使用`create_zip()`方法，可以创建一个ZIP压缩包。
第一个参数指定创建zip压缩包的名称和路径，第二个参数是一个回调函数，参数是一个`ZIPPacker`类型。
在回调函数体内我们可以处理压缩包。
```swift
myZIP.create_zip("res://1.zip",func(zip_packer:ZIPPacker):
    # 创建后的操作
)
```
### 写入文件
通过`put_txt_file()`方法，可以在ZIP压缩包的指定路径下，添加指定的纯文本文件。
```swift
myZIP.create_zip("res://1.zip",func(zip_packer:ZIPPacker):
    myZIP.put_txt_file(zip_packer,"aaa/1.txt","res://1.txt")
)
```
通过`put_binary_file()`方法，可以在ZIP压缩包的指定路径下，添加指定的二进制文件，这些文件包括但不限于图片、音频以及可执行程序等。
```swift
myZIP.create_zip("res://1.zip",func(zip_packer:ZIPPacker):
	myZIP.put_binary_file(zip_packer,"imgs/1.jpg","res://1.jpg")
)
```
> **注意**
> 目前的缺陷是，Godot4.0的ZIPPacker，如果使用中文文件名会出现名称乱码情况，所以只能使用英文。

除了`put_txt_file()`和`put_binary_file()`方法外，还提供了`write_txt_file()`和`write_binary_file()`方法，可以直接从Godot内写入纯文本和二进制内容。比如：
直接在ZIP压缩包内创建纯文本文件，并写入内容：
```swift
myZIP.create_zip("res://1.zip",func(zip_packer:ZIPPacker):
    myZIP.write_txt_file(zip_packer,"aaa/1.txt","你好，这是一个ZIP文件")
)
```
直接在ZIP压缩包内创建二进制本文件，并写入字节数据：
```swift
myZIP.create_zip("res://1.zip",func(zip_packer:ZIPPacker):
    var img_data = texture_rect.texture.get_image().get_data()
	myZIP.write_binary_file(zip_packer,"imgs/1.jpg",img_data)
)
```
### 读取压缩包内容
和`create_zip()`方法类似。使用回调函数来处理压缩包的读取。
第一个参数指定读取的zip压缩包的名称和路径，第二个参数是一个回调函数，参数是一个`ZIPReader`类型。
在回调函数体内我们可以处理压缩包的读取操作。
```swift
myZIP.read_zip("res://1.zip",func(zip_reader:ZIPReader):
	# 读取后的操作
)
```
### 获取ZIP文件列表
通过`ZIPReader`自身的`get_files()`方法，可以获取ZIP压缩包内的文件列表。
```swift
myZIP.read_zip("res://1.zip",func(zip_reader:ZIPReader):
    var file_list = zip_reader.get_files()
    print(file_list) # ["imgs/1.jpg"]
)
```
### 直接从ZIP压缩包中获取图片
通过`get_zip_image()`方法可以直接获取ZIP压缩包中的图片资源，返回的类型是`ImageTexture`，因此可以直接赋值给Godot内部的节点的Texture或Icon等属性。
```swift
myZIP.read_zip("res://1.zip",func(zip_reader:ZIPReader):
	texture_rect.texture  = myZIP.get_zip_image(zip_reader,"imgs/1.jpg")
)
```
## 辅助函数
通过`get_binary_file_bytes()`和`get_txt_file_string()`，可以获取非压缩包内文件的字节或纯文本数据。
