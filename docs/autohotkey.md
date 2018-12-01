## 获取控件信息

- ControlGetText   Edit 控件信息
- ControlClick     客户区坐标
- TreeView 控件可用 Acc 访问，并发送 ControlSend 相关的快捷键来移动控件, 比点击树控件坐标好使 

## 切换 tab

``` js	
SendMessage, 0x1330, 2,, SysTabControl322	 ; 0x1330 is TCM_SETCURFOCUS
sleep, 0
SendMessage, 0x130C, 2,, SysTabControl322  ; 0x130C is TCM_SETCURSEL			
```

如不行，就用 ControlClick 点击客户区


## 文华函数导出

```js
StringRight(ByRef InputVar, Count) 
{
    StringRight, v, InputVar, %Count%
    Return, v
}

StringTrimRight(ByRef InputVar, Count) 
{
    StringTrimRight, v, InputVar, %Count%
    Return, v
}


FileDelete(FilePattern)
{
    FileDelete, %FilePattern%
    if (!ErrorLevel)
        Return False    
    Return True
}

ParentDir(fileOrFolder)
{
    fileOrFolder := RegExReplace(fileOrFolder, "(\\|/)", "\")
    if (StringRight(fileOrFolder, 1) == "\")
        fileOrFolder:= StringTrimRight(fileOrFolder, 1)
    RegexMatch(fileOrFolder, "^.*\\", returned)
    return returned
}

EnsureDirExists(path)
{
   ;if path is a file, this ensures that the parent dir exists
   dir := ParentDir(path)

   ;figure out if it is a file or dir
   ;split off filename if applicable
   FileCreateDir, %dir%
}

FileAppend(Filename, Content)
{
    EnsureDirExists(Filename)
    FileAppend, %Content%, %Filename%
}





;get window by title
ControlGet, vText, List, , SysListView321, 函数列表
;~ MsgBox, % vText

nav_list := StrSplit(vText, "`n")
;MsgBox, % nav_list.MaxIndex() 

filename_all := "wenhua_all_functions.md"
FileDelete(filename_all)
WinActivate, 函数列表
ControlSend, SysListView322, {Home}, 函数列表	
ControlSend, SysListView321, {Home}, 函数列表	
Y := 115
Loop % nav_list.MaxIndex() {
	Y := Y + 19
	MouseClick, left, 72, Y 

	;ControlSend, SysListView321, {Down 1}, 函数列表	
	Sleep, 1000
	
	FileAppend(filename_all, "# " . nav_list[A_Index+1] . "`r`n")    
	filename := nav_list[A_Index+1] . ".md"    
    FileDelete(filename)
	
	ControlSend, SysListView322, {Home}, 函数列表	
	ControlGet, vText2, List, , SysListView322, 函数列表
	function_list := StrSplit(vText2, "`n")	
	Loop % function_list.MaxIndex() {
		titleArray := StrSplit(function_list[A_Index], "`t")
		FileAppend(filename, "## " . titleArray[1] . "`r`n")          
		FileAppend(filename, "`r`n" . titleArray[2] . "`r`n")    
		FileAppend(filename, "`r`n")   
		
		
		FileAppend(filename_all, "## " . titleArray[1] . "`r`n")          
		FileAppend(filename_all, "`r`n" . titleArray[2] . "`r`n")    
		FileAppend(filename_all, "`r`n")   
		
		
		
		ControlGetText, OutputVar, Edit1, 函数列表
		FileAppend(filename, "`````` cpp `r`n")   
		FileAppend(filename, OutputVar)  
		FileAppend(filename, "`r`n```````")   	
		FileAppend(filename, "`r`n`r`n")   
		
		
		FileAppend(filename_all, "`````` cpp `r`n")   
		FileAppend(filename_all, OutputVar)  
		FileAppend(filename_all, "`r`n```````")   	
		FileAppend(filename_all, "`r`n`r`n")   
		ControlSend, SysListView322, {Down 1}, 函数列表	
		
		
	}
}

;~ ControlGet, vText, List, , SysListView322, 函数列表
;~ MsgBox, % vText

;~ ControlGetText, OutputVar, Edit1, 函数列表
;~ MsgBox, % OutputVar
```

## 文华公式导出

```js
ComObjError(false)

#include Acc.ahk

StringRight(ByRef InputVar, Count) 
{
    StringRight, v, InputVar, %Count%
    Return, v
}

StringTrimRight(ByRef InputVar, Count) 
{
    StringTrimRight, v, InputVar, %Count%
    Return, v
}


FileDelete(FilePattern)
{
    FileDelete, %FilePattern%
    if (!ErrorLevel)
        Return False    
    Return True
}

ParentDir(fileOrFolder)
{
    fileOrFolder := RegExReplace(fileOrFolder, "(\\|/)", "\")
    if (StringRight(fileOrFolder, 1) == "\")
        fileOrFolder:= StringTrimRight(fileOrFolder, 1)
    RegexMatch(fileOrFolder, "^.*\\", returned)
    return returned
}

EnsureDirExists(path)
{
   ;if path is a file, this ensures that the parent dir exists
   dir := ParentDir(path)

   ;figure out if it is a file or dir
   ;split off filename if applicable
   FileCreateDir, %dir%
}

FileAppend(Filename, Content)
{
    EnsureDirExists(Filename)
    FileAppend, %Content%, %Filename%
}

GetAccLocation(AccObj, Child=0, byref x="", byref y="", byref w="", byref h="") {
    AccObj.accLocation(ComObj(0x4003,&x:=0), ComObj(0x4003,&y:=0), ComObj(0x4003,&w:=0), ComObj(0x4003,&h:=0), Child)
    return  "x" (x:=NumGet(x,0,"int")) "  "
    .   "y" (y:=NumGet(y,0,"int")) "  "
    .   "w" (w:=NumGet(w,0,"int")) "  "
    .   "h" (h:=NumGet(h,0,"int"))
}


filename_all := "wenhua_all_formular.md"
FileDelete(filename_all)

ControlGet hwnd, hwnd, , SysTreeView321, My Language(麦语言)趋势跟踪模型编写平台
client := Acc_ObjectFromWindow(hwnd)
tree := Acc_Children(client)[4]

For wach, child in Acc_Children(tree)
{
	
	if Not IsObject(child) {
		name := tree.accName(child)
		;MsgBox, % name
		
		if (name == "摆动分析" or name == "量仓分析" or name == "形态选股" or name == "指标选股" or name == "走势选股" or name == "其他" or name == "趋势模型编写示范" or name == "公式条件单编写示范" or name == "模型开发案例") {
			
			ControlClick, SysTreeView321, My Language(麦语言)趋势跟踪模型编写平台
			Send {WheelDown 6}
		}
		
		
		
		if (name == "K线形态" or name == "趋势分析"  or name == "摆动分析" or name == "量仓分析" or name == "形态选股" or name == "指标选股" or name == "走势选股" or name == "其他" or name == "趋势模型编写示范" or name == "公式条件单编写示范" or name == "模型开发案例" or name == "自编") {
			FileAppend(filename_all, "# " . name . "`r`n`r`n") 			
			continue
		} else {
			FileAppend(filename_all, "## " . name . "`r`n") 
		}
		
		
		
		
		
		 GetAccLocation(tree, child, x, y, w, h)
		 ;MsgBox, % x
		 ;MsgBox, % y
		 
		try {
            Acc.accDoDefaultAction(child)
        } catch e {

        }
		
		x += w / 2
        y += h / 2
        SetControlDelay -1
        ControlClick, x%x% y%y%, My Language(麦语言)趋势跟踪模型编写平台
		
		ControlClick, Scintilla1, My Language(麦语言)趋势跟踪模型编写平台
		
		Send ^a
		Send ^c
		ClipWait
		;MsgBox, %clipboard%
		
		ControlGet, vText2, List, , SysListView321, My Language(麦语言)趋势跟踪模型编写平台
		arg_list := StrSplit(vText2, "`n")	
		if (arg_list[1] != "1		0	0	0") {
			FileAppend(filename_all, "`r`n`````` cpp`r`n") 
			Loop % arg_list.MaxIndex() {			
				FileAppend(filename_all, arg_list[A_Index] . "`r`n") 
			}
			FileAppend(filename_all, "`r`n```````r`n") 
		}
		
		FileAppend(filename_all, "`r`n`````` cpp`r`n") 
		FileAppend(filename_all, clipboard) 
		FileAppend(filename_all, "`r`n```````r`n") 
		FileAppend(filename_all, "`r`n") 
		
		
		
		Sleep, 500
		
		
		
	}
}


```

## 通达信函数导出

```js
StringRight(ByRef InputVar, Count) 
{
    StringRight, v, InputVar, %Count%
    Return, v
}

StringTrimRight(ByRef InputVar, Count) 
{
    StringTrimRight, v, InputVar, %Count%
    Return, v
}


FileDelete(FilePattern)
{
    FileDelete, %FilePattern%
    if (!ErrorLevel)
        Return False    
    Return True
}

ParentDir(fileOrFolder)
{
    fileOrFolder := RegExReplace(fileOrFolder, "(\\|/)", "\")
    if (StringRight(fileOrFolder, 1) == "\")
        fileOrFolder:= StringTrimRight(fileOrFolder, 1)
    RegexMatch(fileOrFolder, "^.*\\", returned)
    return returned
}

EnsureDirExists(path)
{
   ;if path is a file, this ensures that the parent dir exists
   dir := ParentDir(path)

   ;figure out if it is a file or dir
   ;split off filename if applicable
   FileCreateDir, %dir%
}

FileAppend(Filename, Content)
{
    EnsureDirExists(Filename)
    FileAppend, %Content%, %Filename%
}





;get window by title
ControlGet, vText, List, , SysListView321, 插入函数
;~ MsgBox, % vText

nav_list := StrSplit(vText, "`n")
;MsgBox, % nav_list.MaxIndex() 

filename_all := "tdx_functions.md"
FileDelete(filename_all)
WinActivate, 插入函数
ControlSend, SysListView322, {Home}, 插入函数	
ControlSend, SysListView321, {Home}, 插入函数	
;MouseClick, left, 60, 60
Y := 40
Loop % nav_list.MaxIndex() {
	Y := Y + 22
	;~ MouseClick, left, 60, Y 

	ControlSend, SysListView321, {Down 1}, 插入函数
	Sleep, 1000
	
	FileAppend(filename_all, "# " . nav_list[A_Index+1] . "`r`n")    

	ControlSend, SysListView322, {Home}, 插入函数	
	ControlGet, vText2, List, , SysListView322, 插入函数
	function_list := StrSplit(vText2, "`n")	
	Loop % function_list.MaxIndex() {
		titleArray := StrSplit(function_list[A_Index], "`t")
		
		
		
		FileAppend(filename_all, "## " . titleArray[1] . "`r`n")          
		FileAppend(filename_all, "`r`n" . titleArray[2] . "`r`n")    
		FileAppend(filename_all, "`r`n")   
		
		
		
		ControlGetText, OutputVar, RICHEDIT1, 插入函数
		
		;~ MsgBox, % OutputVar
		
		
		
		FileAppend(filename_all, "`````` cpp `r`n")   
		FileAppend(filename_all, OutputVar)  
		FileAppend(filename_all, "`r`n```````")   	
		FileAppend(filename_all, "`r`n`r`n")   
		ControlSend, SysListView322, {Down 1}, 插入函数
		Sleep, 500
		
		
	}
}

;~ ControlGet, vText, List, , SysListView322, 函数列表
;~ MsgBox, % vText

;~ ControlGetText, OutputVar, Edit1, 函数列表
;~ MsgBox, % OutputVar
```


## 通达信公式导出


```js
ComObjError(false)
SetTitleMatchMode RegEx



#include Acc.ahk

StringRight(ByRef InputVar, Count) 
{
    StringRight, v, InputVar, %Count%
    Return, v
}

StringTrimRight(ByRef InputVar, Count) 
{
    StringTrimRight, v, InputVar, %Count%
    Return, v
}

;------------------------------------------------
; StrStartsWith
StrStartsWith(string, start)
{
    return InStr(string, start) = 1
}


FileDelete(FilePattern)
{
    FileDelete, %FilePattern%
    if (!ErrorLevel)
        Return False    
    Return True
}

ParentDir(fileOrFolder)
{
    fileOrFolder := RegExReplace(fileOrFolder, "(\\|/)", "\")
    if (StringRight(fileOrFolder, 1) == "\")
        fileOrFolder:= StringTrimRight(fileOrFolder, 1)
    RegexMatch(fileOrFolder, "^.*\\", returned)
    return returned
}

EnsureDirExists(path)
{
   ;if path is a file, this ensures that the parent dir exists
   dir := ParentDir(path)

   ;figure out if it is a file or dir
   ;split off filename if applicable
   FileCreateDir, %dir%
}

FileAppend(Filename, Content)
{
    EnsureDirExists(Filename)
    FileAppend, %Content%, %Filename%
}

GetAccLocation(AccObj, Child=0, byref x="", byref y="", byref w="", byref h="") {
    AccObj.accLocation(ComObj(0x4003,&x:=0), ComObj(0x4003,&y:=0), ComObj(0x4003,&w:=0), ComObj(0x4003,&h:=0), Child)
    return  "x" (x:=NumGet(x,0,"int")) "  "
    .   "y" (y:=NumGet(y,0,"int")) "  "
    .   "w" (w:=NumGet(w,0,"int")) "  "
    .   "h" (h:=NumGet(h,0,"int"))
}

WinActivate, 公式管理器
;~ WinSet, Style, -0xC00000, A
;~ WinSet, Style, -0x40000 , A
;~ WinMove, A, , -1, 0, A_ScreenWidth, A_ScreenHeight-1
filename_all := "tdx_all_formular.md"
FileDelete(filename_all)
;~ ControlMove, SysTreeView321, 0, 30, 2000, 1050, 公式管理器
ControlSend, SysTreeView321, {Home}, 公式管理器
ControlGet hwnd, hwnd, , SysTreeView321, 公式管理器
client := Acc_ObjectFromWindow(hwnd)
tree := Acc_Children(client)[4]

count := 0


;~ x := 88
;~ y := 78
For wach, child in Acc_Children(tree)
{
	
	if Not IsObject(child) {
		name := tree.accName(child)
		
		
        ControlSend, SysTreeView321, {Enter}, 公式管理器
		Sleep, 200
		if WinExist("密码保护") {
			WinClose, 密码保护
			ControlFocus, SysTreeView321, 公式管理器										
		} 
		
		params = 
		content = 
		description = 
		clipboard = 		
		WinActivate, 公式编辑器	
		IfWinActive, 公式编辑器 
		{
			
			ControlGetText, Param1, Edit5, 公式编辑器 			
			ControlGetText, Param1Min, Edit6, 公式编辑器 
			ControlGetText, Param1Max, Edit7, 公式编辑器 
			ControlGetText, Param1Value, Edit8, 公式编辑器 		
			
			if (Param1 <> "") {				
				params := params . Param1 . "`t" . Param1Min . "`t" . Param1Max . "`t" . Param1Value				
			}
			
			ControlGetText, Param2, Edit10, 公式编辑器 			
			ControlGetText, Param2Min, Edit11, 公式编辑器 
			ControlGetText, Param2Max, Edit12, 公式编辑器 
			ControlGetText, Param2Value, Edit13, 公式编辑器 	
			
			if (Param2 <> "") {
				params := params . "`r`n"
				params := params . Param2 . "`t" . Param2Min . "`t" . Param2Max . "`t" . Param2Value				
			}


			ControlGetText, Param3, Edit15, 公式编辑器 			
			ControlGetText, Param3Min, Edit16, 公式编辑器 
			ControlGetText, Param3Max, Edit17, 公式编辑器 
			ControlGetText, Param3Value, Edit18, 公式编辑器 	

			if (Param3 <> "") {
				params := params . "`r`n"
				params := params . Param3 . "`t" . Param3Min . "`t" . Param3Max . "`t" . Param3Value				
			}
			
			
			ControlGetText, Param4, Edit20, 公式编辑器 			
			ControlGetText, Param4Min, Edit21, 公式编辑器 
			ControlGetText, Param4Max, Edit22, 公式编辑器 
			ControlGetText, Param4Value, Edit23, 公式编辑器 		
			
			
			if (Param4 <> "") {
				params := params . "`r`n"
				params := params . Param4 . "`t" . Param4Min . "`t" . Param4Max . "`t" . Param4Value				
			}
			
			SendMessage, 0x1330, 1,, SysTabControl322	 ; 0x1330 is TCM_SETCURFOCUS
			sleep, 0
			SendMessage, 0x130C, 1,, SysTabControl322  ; 0x130C is TCM_SETCURSEL
			
			
			ControlGetText, Param5, Edit5, 公式编辑器 			
			ControlGetText, Param5Min, Edit6, 公式编辑器 
			ControlGetText, Param5Max, Edit7, 公式编辑器 
			ControlGetText, Param5Value, Edit8, 公式编辑器 		
			
			if (Param5 <> "") {
				params := params . "`r`n"
				params := params . Param5 . "`t" . Param5Min . "`t" . Param5Max . "`t" . Param5Value				
			}
			
			ControlGetText, Param6, Edit10, 公式编辑器 			
			ControlGetText, Param6Min, Edit11, 公式编辑器 
			ControlGetText, Param6Max, Edit12, 公式编辑器 
			ControlGetText, Param6Value, Edit13, 公式编辑器 	
			
			if (Param6 <> "") {
				params := params . "`r`n"
				params := params . Param6 . "`t" . Param6Min . "`t" . Param6Max . "`t" . Param6Value				
			}


			ControlGetText, Param7, Edit15, 公式编辑器 			
			ControlGetText, Param7Min, Edit16, 公式编辑器 
			ControlGetText, Param7Max, Edit17, 公式编辑器 
			ControlGetText, Param7Value, Edit18, 公式编辑器 	

			if (Param7 <> "") {
				params := params . "`r`n"
				params := params . Param7 . "`t" . Param7Min . "`t" . Param7Max . "`t" . Param7Value				
			}
			
			
			ControlGetText, Param8, Edit20, 公式编辑器 			
			ControlGetText, Param8Min, Edit21, 公式编辑器 
			ControlGetText, Param8Max, Edit22, 公式编辑器 
			ControlGetText, Param8Value, Edit23, 公式编辑器 		
			
			
			if (Param8 <> "") {
				params := params . "`r`n"
				params := params . Param8 . "`t" . Param8Min . "`t" . Param8Max . "`t" . Param8Value				
			}
			
			SendMessage, 0x1330, 2,, SysTabControl322	 ; 0x1330 is TCM_SETCURFOCUS
			sleep, 0
			SendMessage, 0x130C, 2,, SysTabControl322  ; 0x130C is TCM_SETCURSEL
			
			ControlGetText, Param9, Edit5, 公式编辑器 			
			ControlGetText, Param9Min, Edit6, 公式编辑器 
			ControlGetText, Param9Max, Edit7, 公式编辑器 
			ControlGetText, Param9Value, Edit8, 公式编辑器 		
			
			if (Param9 <> "") {
				params := params . "`r`n"
				params := params . Param9 . "`t" . Param9Min . "`t" . Param9Max . "`t" . Param9Value				
			}
			
			ControlGetText, Param10, Edit10, 公式编辑器 			
			ControlGetText, Param10Min, Edit11, 公式编辑器 
			ControlGetText, Param10Max, Edit12, 公式编辑器 
			ControlGetText, Param10Value, Edit13, 公式编辑器 	
			
			if (Param10 <> "") {
				params := params . "`r`n"
				params := params . Param10 . "`t" . Param10Min . "`t" . Param10Max . "`t" . Param10Value				
			}


			ControlGetText, Param11, Edit15, 公式编辑器 			
			ControlGetText, Param11Min, Edit16, 公式编辑器 
			ControlGetText, Param11Max, Edit17, 公式编辑器 
			ControlGetText, Param11Value, Edit18, 公式编辑器 	

			if (Param11 <> "") {
				params := params . "`r`n"
				params := params . Param11 . "`t" . Param11Min . "`t" . Param11Max . "`t" . Param11Value				
			}
			
			
			ControlGetText, Param12, Edit20, 公式编辑器 			
			ControlGetText, Param12Min, Edit21, 公式编辑器 
			ControlGetText, Param12Max, Edit22, 公式编辑器 
			ControlGetText, Param12Value, Edit23, 公式编辑器 		
			
			
			if (Param12 <> "") {
				params := params . "`r`n"
				params := params . Param12 . "`t" . Param12Min . "`t" . Param12Max . "`t" . Param12Value				
			}
			
			SendMessage, 0x1330, 3,, SysTabControl322	 ; 0x1330 is TCM_SETCURFOCUS
			sleep, 0
			SendMessage, 0x130C, 3,, SysTabControl322  ; 0x130C is TCM_SETCURSEL
			
			ControlGetText, Param13, Edit5, 公式编辑器 			
			ControlGetText, Param13Min, Edit6, 公式编辑器 
			ControlGetText, Param13Max, Edit7, 公式编辑器 
			ControlGetText, Param13Value, Edit8, 公式编辑器 		
			
			if (Param13 <> "") {
				params := params . "`r`n"
				params := params . Param13 . "`t" . Param13Min . "`t" . Param13Max . "`t" . Param13Value				
			}
			
			ControlGetText, Param14, Edit10, 公式编辑器 			
			ControlGetText, Param14Min, Edit11, 公式编辑器 
			ControlGetText, Param14Max, Edit12, 公式编辑器 
			ControlGetText, Param14Value, Edit13, 公式编辑器 	
			
			if (Param14 <> "") {
				params := params . "`r`n"
				params := params . Param14 . "`t" . Param14Min . "`t" . Param14Max . "`t" . Param14Value				
			}


			ControlGetText, Param15, Edit15, 公式编辑器 			
			ControlGetText, Param15Min, Edit16, 公式编辑器 
			ControlGetText, Param15Max, Edit17, 公式编辑器 
			ControlGetText, Param15Value, Edit18, 公式编辑器 	

			if (Param15 <> "") {
				params := params . "`r`n"
				params := params . Param15 . "`t" . Param15Min . "`t" . Param15Max . "`t" . Param15Value				
			}
			
			
			ControlGetText, Param16, Edit20, 公式编辑器 			
			ControlGetText, Param16Min, Edit21, 公式编辑器 
			ControlGetText, Param16Max, Edit22, 公式编辑器 
			ControlGetText, Param16Value, Edit23, 公式编辑器 		
			
			
			if (Param12 <> "") {
				params := params . "`r`n"
				params := params . Param16 . "`t" . Param16Min . "`t" . Param16Max . "`t" . Param16Value				
			}			
			
			

			Send ^a
			Sleep, 500
			Send ^c
			ClipWait
			content = %clipboard%	
			
			
			
			;~ SendMessage, 0x1330, 3,, SysTabControl321, 公式编辑器 		 ; 0x1330 is TCM_SETCURFOCUS
			;~ sleep, 0
			;~ SendMessage, 0x130C, 3,, SysTabControl321, 公式编辑器 	  ; 0x130C is TCM_SETCURSEL
			
			ControlClick, x719 y575, 公式编辑器  
			ControlGetText, OutputVar, Edit30, 公式编辑器	
			;ControlGet, OutputVar, Line, 1, Edit30, 公式管理器	

			description := OutputVar
			;MsgBox, %OutputVar%
			
			;MsgBox, %clipboard%
			Sleep, 200			
			WinClose, 公式编辑器			
			
		}
		
		if (content <> "")
			FileAppend(filename_all, "### " . name . "`r`n") 	
		else
			FileAppend(filename_all, "## " . name . "`r`n")
		
		if (params <> "") {
			FileAppend(filename_all, "`r`n`````` pascal`r`n") 
			FileAppend(filename_all, params) 
			FileAppend(filename_all, "`r`n```````r`n") 
		}
		
		if (content <> "") {
			FileAppend(filename_all, "`r`n`````` pascal`r`n") 
			FileAppend(filename_all, content) 
			FileAppend(filename_all, "`r`n```````r`n") 
		}
		
		if (description <> "") {
			FileAppend(filename_all, "`r`n`````` pascal`r`n") 
			FileAppend(filename_all, description) 
			FileAppend(filename_all, "`r`n```````r`n") 
		}
		FileAppend(filename_all, "`r`n") 
			
		ControlFocus, SysTreeView321, 公式管理器
		ControlSend, SysTreeView321, {Down 1}, 公式管理器
		
		
		
		
		Sleep, 200
		
		
		
		
		
	}
}


```


## Acc.ahk


```js
;------------------------------------------------------------------------------
; Acc.ahk Standard Library
; by Sean
; Updated by jethrow:
; 	Modified ComObjEnwrap params from (9,pacc) --> (9,pacc,1)
; 	Changed ComObjUnwrap to ComObjValue in order to avoid AddRef (thanks fincs)
; 	Added Acc_GetRoleText & Acc_GetStateText
; 	Added additional functions - commented below
; 	Removed original Acc_Children function
;	Added Acc_Error, Acc_ChildrenByRole, & Acc_Get functions
; last updated 10/25/2012
;------------------------------------------------------------------------------

Acc_Init()
{
	Static	h
	If Not	h
		h:=DllCall("LoadLibrary","Str","oleacc","Ptr")
}
Acc_ObjectFromEvent(ByRef _idChild_, hWnd, idObject, idChild)
{
	Acc_Init()
	If	DllCall("oleacc\AccessibleObjectFromEvent", "Ptr", hWnd, "UInt", idObject, "UInt", idChild, "Ptr*", pacc, "Ptr", VarSetCapacity(varChild,8+2*A_PtrSize,0)*0+&varChild)=0
	Return	ComObjEnwrap(9,pacc,1), _idChild_:=NumGet(varChild,8,"UInt")
}

Acc_ObjectFromPoint(ByRef _idChild_ = "", x = "", y = "")
{
	Acc_Init()
	If	DllCall("oleacc\AccessibleObjectFromPoint", "Int64", x==""||y==""?0*DllCall("GetCursorPos","Int64*",pt)+pt:x&0xFFFFFFFF|y<<32, "Ptr*", pacc, "Ptr", VarSetCapacity(varChild,8+2*A_PtrSize,0)*0+&varChild)=0
	Return	ComObjEnwrap(9,pacc,1), _idChild_:=NumGet(varChild,8,"UInt")
}

Acc_ObjectFromWindow(hWnd, idObject = 0)
{
	Acc_Init()
	If	DllCall("oleacc\AccessibleObjectFromWindow", "Ptr", hWnd, "UInt", idObject&=0xFFFFFFFF, "Ptr", -VarSetCapacity(IID,16)+NumPut(idObject==0xFFFFFFF0?0x46000000000000C0:0x719B3800AA000C81,NumPut(idObject==0xFFFFFFF0?0x0000000000020400:0x11CF3C3D618736E0,IID,"Int64"),"Int64"), "Ptr*", pacc)=0
	Return	ComObjEnwrap(9,pacc,1)
}

Acc_WindowFromObject(pacc)
{
	If	DllCall("oleacc\WindowFromAccessibleObject", "Ptr", IsObject(pacc)?ComObjValue(pacc):pacc, "Ptr*", hWnd)=0
	Return	hWnd
}

Acc_GetRoleText(nRole)
{
	nSize := DllCall("oleacc\GetRoleText", "Uint", nRole, "Ptr", 0, "Uint", 0)
	VarSetCapacity(sRole, (A_IsUnicode?2:1)*nSize)
	DllCall("oleacc\GetRoleText", "Uint", nRole, "str", sRole, "Uint", nSize+1)
	Return	sRole
}

Acc_GetStateText(nState)
{
	nSize := DllCall("oleacc\GetStateText", "Uint", nState, "Ptr", 0, "Uint", 0)
	VarSetCapacity(sState, (A_IsUnicode?2:1)*nSize)
	DllCall("oleacc\GetStateText", "Uint", nState, "str", sState, "Uint", nSize+1)
	Return	sState
}

Acc_SetWinEventHook(eventMin, eventMax, pCallback)
{
	Return	DllCall("SetWinEventHook", "Uint", eventMin, "Uint", eventMax, "Uint", 0, "Ptr", pCallback, "Uint", 0, "Uint", 0, "Uint", 0)
}

Acc_UnhookWinEvent(hHook)
{
	Return	DllCall("UnhookWinEvent", "Ptr", hHook)
}
/*	Win Events:
	pCallback := RegisterCallback("WinEventProc")
	WinEventProc(hHook, event, hWnd, idObject, idChild, eventThread, eventTime)
	{
		Critical
		Acc := Acc_ObjectFromEvent(_idChild_, hWnd, idObject, idChild)
		; Code Here:
	}
*/

; Written by jethrow
Acc_Role(Acc, ChildId=0) {
	try return ComObjType(Acc,"Name")="IAccessible"?Acc_GetRoleText(Acc.accRole(ChildId)):"invalid object"
}
Acc_State(Acc, ChildId=0) {
	try return ComObjType(Acc,"Name")="IAccessible"?Acc_GetStateText(Acc.accState(ChildId)):"invalid object"
}
Acc_Location(Acc, ChildId=0, byref Position="") { ; adapted from Sean's code
	try Acc.accLocation(ComObj(0x4003,&x:=0), ComObj(0x4003,&y:=0), ComObj(0x4003,&w:=0), ComObj(0x4003,&h:=0), ChildId)
	catch
		return
	Position := "x" NumGet(x,0,"int") " y" NumGet(y,0,"int") " w" NumGet(w,0,"int") " h" NumGet(h,0,"int")
	return	{x:NumGet(x,0,"int"), y:NumGet(y,0,"int"), w:NumGet(w,0,"int"), h:NumGet(h,0,"int")}
}
Acc_Parent(Acc) { 
	try parent:=Acc.accParent
	return parent?Acc_Query(parent):
}
Acc_Child(Acc, ChildId=0) {
	try child:=Acc.accChild(ChildId)
	return child?Acc_Query(child):
}
Acc_Query(Acc) { ; thanks Lexikos - www.autohotkey.com/forum/viewtopic.php?t=81731&p=509530#509530
	try return ComObj(9, ComObjQuery(Acc,"{618736e0-3c3d-11cf-810c-00aa00389b71}"), 1)
}
Acc_Error(p="") {
	static setting:=0
	return p=""?setting:setting:=p
}
Acc_Children(Acc) {
	if ComObjType(Acc,"Name") != "IAccessible"
		ErrorLevel := "Invalid IAccessible Object"
	else {
		Acc_Init(), cChildren:=Acc.accChildCount, Children:=[]
		if DllCall("oleacc\AccessibleChildren", "Ptr",ComObjValue(Acc), "Int",0, "Int",cChildren, "Ptr",VarSetCapacity(varChildren,cChildren*(8+2*A_PtrSize),0)*0+&varChildren, "Int*",cChildren)=0 {
			Loop %cChildren%
				i:=(A_Index-1)*(A_PtrSize*2+8)+8, child:=NumGet(varChildren,i), Children.Insert(NumGet(varChildren,i-8)=9?Acc_Query(child):child), NumGet(varChildren,i-8)=9?ObjRelease(child):
			return Children.MaxIndex()?Children:
		} else
			ErrorLevel := "AccessibleChildren DllCall Failed"
	}
	if Acc_Error()
		throw Exception(ErrorLevel,-1)
}
Acc_ChildrenByRole(Acc, Role) {
	if ComObjType(Acc,"Name")!="IAccessible"
		ErrorLevel := "Invalid IAccessible Object"
	else {
		Acc_Init(), cChildren:=Acc.accChildCount, Children:=[]
		if DllCall("oleacc\AccessibleChildren", "Ptr",ComObjValue(Acc), "Int",0, "Int",cChildren, "Ptr",VarSetCapacity(varChildren,cChildren*(8+2*A_PtrSize),0)*0+&varChildren, "Int*",cChildren)=0 {
			Loop %cChildren% {
				i:=(A_Index-1)*(A_PtrSize*2+8)+8, child:=NumGet(varChildren,i)
				if NumGet(varChildren,i-8)=9
					AccChild:=Acc_Query(child), ObjRelease(child), Acc_Role(AccChild)=Role?Children.Insert(AccChild):
				else
					Acc_Role(Acc, child)=Role?Children.Insert(child):
			}
			return Children.MaxIndex()?Children:, ErrorLevel:=0
		} else
			ErrorLevel := "AccessibleChildren DllCall Failed"
	}
	if Acc_Error()
		throw Exception(ErrorLevel,-1)
}
Acc_Get(Cmd, ChildPath="", ChildID=0, WinTitle="", WinText="", ExcludeTitle="", ExcludeText="") {
	static properties := {Action:"DefaultAction", DoAction:"DoDefaultAction", Keyboard:"KeyboardShortcut"}
	AccObj :=   IsObject(WinTitle)? WinTitle
			:   Acc_ObjectFromWindow( WinExist(WinTitle, WinText, ExcludeTitle, ExcludeText), 0 )
	if ComObjType(AccObj, "Name") != "IAccessible"
		ErrorLevel := "Could not access an IAccessible Object"
	else {
		StringReplace, ChildPath, ChildPath, _, %A_Space%, All
		AccError:=Acc_Error(), Acc_Error(true)
		Loop Parse, ChildPath, ., %A_Space%
			try {
				if A_LoopField is digit
					Children:=Acc_Children(AccObj), m2:=A_LoopField ; mimic "m2" output in else-statement
				else
					RegExMatch(A_LoopField, "(\D*)(\d*)", m), Children:=Acc_ChildrenByRole(AccObj, m1), m2:=(m2?m2:1)
				if Not Children.HasKey(m2)
					throw
				AccObj := Children[m2]
			} catch {
				ErrorLevel:="Cannot access ChildPath Item #" A_Index " -> " A_LoopField, Acc_Error(AccError)
				if Acc_Error()
					throw Exception("Cannot access ChildPath Item", -1, "Item #" A_Index " -> " A_LoopField)
				return
			}
		Acc_Error(AccError)
		StringReplace, Cmd, Cmd, %A_Space%, , All
		properties.HasKey(Cmd)? Cmd:=properties[Cmd]:
		try {
			if (Cmd = "Location")
				AccObj.accLocation(ComObj(0x4003,&x:=0), ComObj(0x4003,&y:=0), ComObj(0x4003,&w:=0), ComObj(0x4003,&h:=0), ChildId)
			  , ret_val := "x" NumGet(x,0,"int") " y" NumGet(y,0,"int") " w" NumGet(w,0,"int") " h" NumGet(h,0,"int")
			else if (Cmd = "Object")
				ret_val := AccObj
			else if Cmd in Role,State
				ret_val := Acc_%Cmd%(AccObj, ChildID+0)
			else if Cmd in ChildCount,Selection,Focus
				ret_val := AccObj["acc" Cmd]
			else
				ret_val := AccObj["acc" Cmd](ChildID+0)
		} catch {
			ErrorLevel := """" Cmd """ Cmd Not Implemented"
			if Acc_Error()
				throw Exception("Cmd Not Implemented", -1, Cmd)
			return
		}
		return ret_val, ErrorLevel:=0
	}
	if Acc_Error()
		throw Exception(ErrorLevel,-1)
}
```