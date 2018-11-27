## 获取控件信息

- ControlGetText   Edit 控件信息
- ControlClick     客户区坐标
- TreeView 控件可用 Acc 访问，并发送 ControlSend 相关的快捷键来移动控件, 比点击树控件坐标好使 

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
