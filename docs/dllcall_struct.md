## 返回字符串

```js
sData: = DllCall("DdeAccessData", "Uint", hData, "Uint", 0, "Str")
csvWindowInfo: = StrGet( & sData, "CP0")
```

## 分配内存

```js
if A_PtrSize = 4 {
    VarSetCapacity(pi, 16, 0)
    sisize: = VarSetCapacity(si, 68, 0)
    NumPut(sisize, si, 0, "UInt")
    NumPut(0x100, si, 44, "UInt")
    NumPut(hStdInRd, si, 56, "Ptr")
    NumPut(hStdOutWr, si, 60, "Ptr")
    NumPut(hStdOutWr, si, 64, "Ptr")
}
else if A_PtrSize = 8 {

    VarSetCapacity(pi, 24, 0)
    sisize: = VarSetCapacity(si, 96, 0)
    NumPut(sisize, si, 0, "UInt")
    NumPut(0x100, si, 60, "UInt")
    NumPut(hStdInRd, si, 80, "Ptr")
    NumPut(hStdOutWr, si, 88, "Ptr")
    NumPut(hStdOutWr, si, 96, "Ptr")

}
```
