---
layout: post
title: "简单Lua教程"
date: 2014-04-15 20:38:16 +0800
comments: true
author: 最萌小汐
categories: YDWE
---

#**简单Lua教程**

本文旨在让有jass基础的用户快速上手Lua,因此需要一定的jass基础.当然如果你有其他语言的基础那更好

##1.准备工作

最新版本的[YDWE](http://www.ydwe.net/download.html)

建议在配置中关闭预处理器,因为它和Lua的一个符号有冲突,当然你也可以选择回避使用这个符号.

##2.入口

使用```call Cheat("exec-lua: filename")```来运行地图中名称为"filename"的lua脚本

你可以选择在外部编写该脚本后导入地图,也可以通过YDWE在编辑器内编写并自动导入地图

##3.在YDWE内写Lua脚本

在YDWE的任何自定义代码区域输入以下内容

```lua
<?import("filename")[[

	script
		
]]?>
```

这段代码等价于你在地图内导入了一个名字为"filename",内容为"script"的脚本

你可能迫不及待想要尝试写Lua了,不过在这里我要建议你在运行自己的Lua脚本之前,先**单独**运行一次以下代码(通过```call Cheat("exec-lua: MU_console.lua")```)

```lua
<?import("MU_console.lua")[[

	require("jass.runtime").console = true
	
]]?>
```

这段代码的效果是打开一个控制台窗口,这样如果你的其他Lua代码出现了错误,控制台窗口中将会显示出错误原因和错误位置,非常便利.此外通过Lua的print函数,你也可以将各种内容显示在这里

##4.hello world

没错,第一步是hello world

```lua
	print("hello world")
```

控制台窗口中将会显示"hello world"

##5.Lua的基本介绍

lua是一个简单,高效,强大的脚本语言,其语法和jass有很多相似之处,因此有jass基础的话可以很快掌握lua

lua和jass最大的不同在于lua不需要定义变量类型,例:

```lua
	local a = 1
	if a == 1 then
		a = "等于1"
	end
```

在这个例子中,a的类型分别为数字(number)与字符串(string)

lua的注释符为 ```--```

你也可以进行多行注释

```lua
	--[[
		我已经被注释掉了
	--]]
```

####在lua中有以下几个类型:

* nil   空值,任何变量在赋值前都是nil,给变量赋值nil相当于摧毁该变量,类似于jass中的null

* boolean   布尔值,与jass一样包含true与false

* number   数字,lua中的数字没有整数或实数之分,因此在lua中5/2 == 2.5

* string   字符串,与jass中的字符串基本相同

* table   表,lua中最强大的类型,他可以简单的当做数组或哈希表使用,更是lua构成复杂的高级功能的基础,这将在之后重点学习

* function   函数,与jass中的函数不同,lua中的函数也被视为一个值,你可以随时将它赋值给一个变量,或在其他函数中作为参数或返回值传递

* userdata   自定义数据,你可以将其理解为jass中的handle,lua无法直接对其进行修改

####lua有以下几个保留字:

```lua
	and break do else elseif
	end false for function if
	in local nil not or
	repeat return then true until
	while
```

与jass很像不是吗?

##6.值之间的操作

####运算
```lua
	1 + 2 == 3
	3 * 4 == 12
	2 - 6 == -4
	7 / 2 == 3.5
	4 ^ 2 == 16
	5 % 1.5 == 0.5
	1 .. 1 == "11"
	"6" + "7" == 13
```
lua在必要的时候会自动转换类型,因此需要特别留意字符串是通过 .. 来连接的

此外lua还自带了 幂(^) 与 取模(%)

####逻辑
```lua
	if x == true then --1
		a = 1
	elseif x then --2
		a = 2
	elseif not x then --3
		a = 3
	elseif x and y then --4
		a = 4
	elseif x or y then --5
		a = 5
	end
```
lua的if结构与jass相似,只要注意endif要改成end

第2个条件中,只有当x的值为nil或false才不成立,其他情况包括0或""都是成立的


##7.变量

lua对变量进行操作时不需要 set 关键字

lua使用全局变量无须事先声明,任何时候```a = 10```都是有效的.在a被赋值前,如果去获取a的值将返回"nil"

局部变量的声明方式类似于jass:

```lua
	local a = 10
```

局部变量可以在任何位置声明,影响范围由声明的位置而定,例如:
```lua
	local a = 10
	if a then
		local a = 20
		print(a)
	end
	print(a)
```
将依次显示20与10

记住,lua不需要写变量类型哦

##8.字符串

lua的字符串有3种符号,例:
```lua
	text1 = "I'm hungry"
	text2 = '"That loli seems delicacies!", I said'
	text3 = [[
"呜喵"\n"呜喵"\n"呜喵"
	]]
```

其中用[[]]表示的字符串将忠实的记录你输入的文字,忽略其中的任何转义符.如果你```print(text3)```,控制台将显示
```
"呜喵"\n"呜喵"\n"呜喵"
```

需要注意的是,[[]]形式的字符串可能会与你的脚本产生一些冲突,例如
```lua
	text = [[
		a = t[v[5]]
	]]
```
你可以改写成如下的形式
```lua
	text = [===[
		a = t[v[5]]
	]===]
```
这样lua只会把拥有相同等号数量的括号认为是一组

同样的,你可以将YDWE内写lua的代码改成如下代码:
```lua
<?import(filename)[=====[

	script
	
]=====]?>
```
以回避一些冲突