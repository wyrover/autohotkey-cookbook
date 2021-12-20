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

```js
SetSystemTime(Year := 1601, Month := 1, DayOfWeek := 0, Day := 1, Hour := 0, Minute := 0, Second := 0, Milliseconds := 0)
{
    static VarSetCapacity(SYSTEMTIME, 16)
    , NumPut(Year,      SYSTEMTIME,  0, "UShort"), NumPut(Month,        SYSTEMTIME,  2, "UShort")
    , NumPut(DayOfWeek, SYSTEMTIME,  4, "UShort"), NumPut(Day,          SYSTEMTIME,  6, "UShort")
    , NumPut(Hour,      SYSTEMTIME,  8, "UShort"), NumPut(Minute,       SYSTEMTIME, 10, "UShort")
    , NumPut(Second,    SYSTEMTIME, 12, "UShort"), NumPut(Milliseconds, SYSTEMTIME, 14, "UShort")
    if !(DllCall("kernel32.dll\SetSystemTime", "Ptr", &SYSTEMTIME))
        return DllCall("kernel32.dll\GetLastError")
    return 1
}
; ===============================================================================================================================

SetSystemTime(2015, 1, 25, 7, 13, 37, 27, 724)
```


```js
GetSystemTime()
{
    static SYSTEMTIME, init := VarSetCapacity(SYSTEMTIME, 16, 0) && NumPut(16, SYSTEMTIME, "UShort")
    DllCall("kernel32.dll\GetSystemTime", "Ptr", &SYSTEMTIME, "Ptr")
    return { 1 : NumGet(SYSTEMTIME,  0, "UShort"), 2 : NumGet(SYSTEMTIME,  2, "UShort")
           , 3 : NumGet(SYSTEMTIME,  4, "UShort"), 4 : NumGet(SYSTEMTIME,  6, "UShort")
           , 5 : NumGet(SYSTEMTIME,  8, "UShort"), 6 : NumGet(SYSTEMTIME, 10, "UShort")
           , 7 : NumGet(SYSTEMTIME, 12, "UShort"), 8 : NumGet(SYSTEMTIME, 14, "UShort") }
}
; ===============================================================================================================================

GetSystemTime := GetSystemTime()

MsgBox % "GetSystemTime function`n"
       . "SYSTEMTIME structure`n`n"
       . "wYear:`t`t"                GetSystemTime[1]   "`n"
       . "wMonth:`t`t"               GetSystemTime[2]   "`n"
       . "wDayOfWeek:`t"             GetSystemTime[3]   "`n"
       . "wDay:`t`t"                 GetSystemTime[4]   "`n"
       . "wHour:`t`t"                GetSystemTime[5]   "`n"
       . "wMinute:`t`t"              GetSystemTime[6]   "`n"
       . "wSecond:`t`t"              GetSystemTime[7]   "`n"
       . "wMilliseconds:`t"          GetSystemTime[8]

```



## 类型大小
https://www.autohotkey.com/boards/viewtopic.php?t=79797

```
Windows Data Types
https://docs.microsoft.com/en-us/windows/win32/winprog/windows-data-types

Type               32-bit / 64-bit
==================================
APIENTRY                        ; won't compile
ATOM                    2 / 2
BOOL                    4 / 4
BOOLEAN                 1 / 1
BYTE                    1 / 1
CALLBACK                        ; won't compile
CCHAR                   1 / 1
CHAR                    1 / 1
COLORREF                4 / 4
CONST                           ; won't compile
DWORD                   4 / 4
DWORD32                 4 / 4
DWORD64                 8 / 8
DWORDLONG               8 / 8
DWORD_PTR               4 / 8
FLOAT                   4 / 4
HACCEL                  4 / 8
HALF_PTR                2 / 4
HANDLE                  4 / 8
HBITMAP                 4 / 8
HBRUSH                  4 / 8
HCOLORSPACE             4 / 8
HCONV                   4 / 8
HCONVLIST               4 / 8
HCURSOR                 4 / 8
HDC                     4 / 8
HDDEDATA                4 / 8
HDESK                   4 / 8
HDROP                   4 / 8
HDWP                    4 / 8
HENHMETAFILE            4 / 8
HFILE                   4 / 4
HFONT                   4 / 8
HGDIOBJ                 4 / 8
HGLOBAL                 4 / 8
HHOOK                   4 / 8
HICON                   4 / 8
HINSTANCE               4 / 8
HKEY                    4 / 8
HKL                     4 / 8
HLOCAL                  4 / 8
HMENU                   4 / 8
HMETAFILE               4 / 8
HMODULE                 4 / 8
HMONITOR                4 / 8
HPALETTE                4 / 8
HPEN                    4 / 8
HRESULT                 4 / 4
HRGN                    4 / 8
HRSRC                   4 / 8
HSZ                     4 / 8
HWINSTA                 4 / 8
HWND                    4 / 8
INT                     4 / 4
INT16                   2 / 2
INT32                   4 / 4
INT64                   8 / 8
INT8                    1 / 1
INT_PTR                 4 / 8
LANGID                  2 / 2
LCID                    4 / 4
LCTYPE                  4 / 4
LGRPID                  4 / 4
LONG                    4 / 4
LONG32                  4 / 4
LONG64                  8 / 8
LONGLONG                8 / 8
LONG_PTR                4 / 8
LPARAM                  4 / 8
LPBOOL                  4 / 8
LPBYTE                  4 / 8
LPCOLORREF              4 / 8
LPCSTR                  4 / 8
LPCTSTR                 4 / 8
LPCVOID                 4 / 8
LPCWSTR                 4 / 8
LPDWORD                 4 / 8
LPHANDLE                4 / 8
LPINT                   4 / 8
LPLONG                  4 / 8
LPSTR                   4 / 8
LPTSTR                  4 / 8
LPVOID                  4 / 8
LPWORD                  4 / 8
LPWSTR                  4 / 8
LRESULT                 4 / 8
PBOOL                   4 / 8
PBOOLEAN                4 / 8
PBYTE                   4 / 8
PCHAR                   4 / 8
PCSTR                   4 / 8
PCTSTR                  4 / 8
PCWSTR                  4 / 8
PDWORD                  4 / 8
PDWORD32                4 / 8
PDWORD64                4 / 8
PDWORDLONG              4 / 8
PDWORD_PTR              4 / 8
PFLOAT                  4 / 8
PHALF_PTR               4 / 8
PHANDLE                 4 / 8
PHKEY                   4 / 8
PINT                    4 / 8
PINT16                  4 / 8
PINT32                  4 / 8
PINT64                  4 / 8
PINT8                   4 / 8
PINT_PTR                4 / 8
PLCID                   4 / 8
PLONG                   4 / 8
PLONG32                 4 / 8
PLONG64                 4 / 8
PLONGLONG               4 / 8
PLONG_PTR               4 / 8
POINTER_32                      ; won't compile
POINTER_64                      ; won't compile
POINTER_SIGNED                  ; won't compile
POINTER_UNSIGNED                ; won't compile
PSHORT                  4 / 8
PSIZE_T                 4 / 8
PSSIZE_T                4 / 8
PSTR                    4 / 8
PTBYTE                  4 / 8
PTCHAR                  4 / 8
PTSTR                   4 / 8
PUCHAR                  4 / 8
PUHALF_PTR              4 / 8
PUINT                   4 / 8
PUINT16                 4 / 8
PUINT32                 4 / 8
PUINT64                 4 / 8
PUINT8                  4 / 8
PUINT_PTR               4 / 8
PULONG                  4 / 8
PULONG32                4 / 8
PULONG64                4 / 8
PULONGLONG              4 / 8
PULONG_PTR              4 / 8
PUSHORT                 4 / 8
PVOID                   4 / 8
PWCHAR                  4 / 8
PWORD                   4 / 8
PWSTR                   4 / 8
QWORD                           ; won't compile
SC_HANDLE               4 / 8
SC_LOCK                 4 / 8
SERVICE_STATUS_HANDLE   4 / 8
SHORT                   2 / 2
SIZE_T                  4 / 8
SSIZE_T                 4 / 8
TBYTE                   1 / 1
TCHAR                   1 / 1
UCHAR                   1 / 1
UHALF_PTR               2 / 4
UINT                    4 / 4
UINT16                  2 / 2
UINT32                  4 / 4
UINT64                  8 / 8
UINT8                   1 / 1
UINT_PTR                4 / 8
ULONG                   4 / 4
ULONG32                 4 / 4
ULONG64                 8 / 8
ULONGLONG               8 / 8
ULONG_PTR               4 / 8
UNICODE_STRING                  ; won't compile
USHORT                  2 / 2
USN                     8 / 8
VOID                            ; won't compile
WCHAR                   2 / 2
WINAPI                          ; won't compile
WORD                    2 / 2
WPARAM                  4 / 8
```

```cpp
#include <iostream>
#include <windows.h>

int main() {
    // std::cout << "APIENTRY = " << sizeof(APIENTRY) << std::endl; // won't work
    std::cout << "ATOM = " << sizeof(ATOM) << std::endl;
    std::cout << "BOOL = " << sizeof(BOOL) << std::endl;
    std::cout << "BOOLEAN = " << sizeof(BOOLEAN) << std::endl;
    std::cout << "BYTE = " << sizeof(BYTE) << std::endl;
    // std::cout << "CALLBACK = " << sizeof(CALLBACK) << std::endl; // won't work
    std::cout << "CCHAR = " << sizeof(CCHAR) << std::endl;
    std::cout << "CHAR = " << sizeof(CHAR) << std::endl;
    std::cout << "COLORREF = " << sizeof(COLORREF) << std::endl;
    // std::cout << "CONST = " << sizeof(CONST) << std::endl; // won't work
    std::cout << "DWORD = " << sizeof(DWORD) << std::endl;
    std::cout << "DWORDLONG = " << sizeof(DWORDLONG) << std::endl;
    std::cout << "DWORD_PTR = " << sizeof(DWORD_PTR) << std::endl;
    std::cout << "DWORD32 = " << sizeof(DWORD32) << std::endl;
    std::cout << "DWORD64 = " << sizeof(DWORD64) << std::endl;
    std::cout << "FLOAT = " << sizeof(FLOAT) << std::endl;
    std::cout << "HACCEL = " << sizeof(HACCEL) << std::endl;
    std::cout << "HALF_PTR = " << sizeof(HALF_PTR) << std::endl;
    std::cout << "HANDLE = " << sizeof(HANDLE) << std::endl;
    std::cout << "HBITMAP = " << sizeof(HBITMAP) << std::endl;
    std::cout << "HBRUSH = " << sizeof(HBRUSH) << std::endl;
    std::cout << "HCOLORSPACE = " << sizeof(HCOLORSPACE) << std::endl;
    std::cout << "HCONV = " << sizeof(HCONV) << std::endl;
    std::cout << "HCONVLIST = " << sizeof(HCONVLIST) << std::endl;
    std::cout << "HCURSOR = " << sizeof(HCURSOR) << std::endl;
    std::cout << "HDC = " << sizeof(HDC) << std::endl;
    std::cout << "HDDEDATA = " << sizeof(HDDEDATA) << std::endl;
    std::cout << "HDESK = " << sizeof(HDESK) << std::endl;
    std::cout << "HDROP = " << sizeof(HDROP) << std::endl;
    std::cout << "HDWP = " << sizeof(HDWP) << std::endl;
    std::cout << "HENHMETAFILE = " << sizeof(HENHMETAFILE) << std::endl;
    std::cout << "HFILE = " << sizeof(HFILE) << std::endl;
    std::cout << "HFONT = " << sizeof(HFONT) << std::endl;
    std::cout << "HGDIOBJ = " << sizeof(HGDIOBJ) << std::endl;
    std::cout << "HGLOBAL = " << sizeof(HGLOBAL) << std::endl;
    std::cout << "HHOOK = " << sizeof(HHOOK) << std::endl;
    std::cout << "HICON = " << sizeof(HICON) << std::endl;
    std::cout << "HINSTANCE = " << sizeof(HINSTANCE) << std::endl;
    std::cout << "HKEY = " << sizeof(HKEY) << std::endl;
    std::cout << "HKL = " << sizeof(HKL) << std::endl;
    std::cout << "HLOCAL = " << sizeof(HLOCAL) << std::endl;
    std::cout << "HMENU = " << sizeof(HMENU) << std::endl;
    std::cout << "HMETAFILE = " << sizeof(HMETAFILE) << std::endl;
    std::cout << "HMODULE = " << sizeof(HMODULE) << std::endl;
    std::cout << "HMONITOR = " << sizeof(HMONITOR) << std::endl;
    std::cout << "HPALETTE = " << sizeof(HPALETTE) << std::endl;
    std::cout << "HPEN = " << sizeof(HPEN) << std::endl;
    std::cout << "HRESULT = " << sizeof(HRESULT) << std::endl;
    std::cout << "HRGN = " << sizeof(HRGN) << std::endl;
    std::cout << "HRSRC = " << sizeof(HRSRC) << std::endl;
    std::cout << "HSZ = " << sizeof(HSZ) << std::endl;
    std::cout << "HWINSTA = " << sizeof(HWINSTA) << std::endl;
    std::cout << "HWND = " << sizeof(HWND) << std::endl;
    std::cout << "INT = " << sizeof(INT) << std::endl;
    std::cout << "INT_PTR = " << sizeof(INT_PTR) << std::endl;
    std::cout << "INT8 = " << sizeof(INT8) << std::endl;
    std::cout << "INT16 = " << sizeof(INT16) << std::endl;
    std::cout << "INT32 = " << sizeof(INT32) << std::endl;
    std::cout << "INT64 = " << sizeof(INT64) << std::endl;
    std::cout << "LANGID = " << sizeof(LANGID) << std::endl;
    std::cout << "LCID = " << sizeof(LCID) << std::endl;
    std::cout << "LCTYPE = " << sizeof(LCTYPE) << std::endl;
    std::cout << "LGRPID = " << sizeof(LGRPID) << std::endl;
    std::cout << "LONG = " << sizeof(LONG) << std::endl;
    std::cout << "LONGLONG = " << sizeof(LONGLONG) << std::endl;
    std::cout << "LONG_PTR = " << sizeof(LONG_PTR) << std::endl;
    std::cout << "LONG32 = " << sizeof(LONG32) << std::endl;
    std::cout << "LONG64 = " << sizeof(LONG64) << std::endl;
    std::cout << "LPARAM = " << sizeof(LPARAM) << std::endl;
    std::cout << "LPBOOL = " << sizeof(LPBOOL) << std::endl;
    std::cout << "LPBYTE = " << sizeof(LPBYTE) << std::endl;
    std::cout << "LPCOLORREF = " << sizeof(LPCOLORREF) << std::endl;
    std::cout << "LPCSTR = " << sizeof(LPCSTR) << std::endl;
    std::cout << "LPCTSTR = " << sizeof(LPCTSTR) << std::endl;
    std::cout << "LPCVOID = " << sizeof(LPCVOID) << std::endl;
    std::cout << "LPCWSTR = " << sizeof(LPCWSTR) << std::endl;
    std::cout << "LPDWORD = " << sizeof(LPDWORD) << std::endl;
    std::cout << "LPHANDLE = " << sizeof(LPHANDLE) << std::endl;
    std::cout << "LPINT = " << sizeof(LPINT) << std::endl;
    std::cout << "LPLONG = " << sizeof(LPLONG) << std::endl;
    std::cout << "LPSTR = " << sizeof(LPSTR) << std::endl;
    std::cout << "LPTSTR = " << sizeof(LPTSTR) << std::endl;
    std::cout << "LPVOID = " << sizeof(LPVOID) << std::endl;
    std::cout << "LPWORD = " << sizeof(LPWORD) << std::endl;
    std::cout << "LPWSTR = " << sizeof(LPWSTR) << std::endl;
    std::cout << "LRESULT = " << sizeof(LRESULT) << std::endl;
    std::cout << "PBOOL = " << sizeof(PBOOL) << std::endl;
    std::cout << "PBOOLEAN = " << sizeof(PBOOLEAN) << std::endl;
    std::cout << "PBYTE = " << sizeof(PBYTE) << std::endl;
    std::cout << "PCHAR = " << sizeof(PCHAR) << std::endl;
    std::cout << "PCSTR = " << sizeof(PCSTR) << std::endl;
    std::cout << "PCTSTR = " << sizeof(PCTSTR) << std::endl;
    std::cout << "PCWSTR = " << sizeof(PCWSTR) << std::endl;
    std::cout << "PDWORD = " << sizeof(PDWORD) << std::endl;
    std::cout << "PDWORDLONG = " << sizeof(PDWORDLONG) << std::endl;
    std::cout << "PDWORD_PTR = " << sizeof(PDWORD_PTR) << std::endl;
    std::cout << "PDWORD32 = " << sizeof(PDWORD32) << std::endl;
    std::cout << "PDWORD64 = " << sizeof(PDWORD64) << std::endl;
    std::cout << "PFLOAT = " << sizeof(PFLOAT) << std::endl;
    std::cout << "PHALF_PTR = " << sizeof(PHALF_PTR) << std::endl;
    std::cout << "PHANDLE = " << sizeof(PHANDLE) << std::endl;
    std::cout << "PHKEY = " << sizeof(PHKEY) << std::endl;
    std::cout << "PINT = " << sizeof(PINT) << std::endl;
    std::cout << "PINT_PTR = " << sizeof(PINT_PTR) << std::endl;
    std::cout << "PINT8 = " << sizeof(PINT8) << std::endl;
    std::cout << "PINT16 = " << sizeof(PINT16) << std::endl;
    std::cout << "PINT32 = " << sizeof(PINT32) << std::endl;
    std::cout << "PINT64 = " << sizeof(PINT64) << std::endl;
    std::cout << "PLCID = " << sizeof(PLCID) << std::endl;
    std::cout << "PLONG = " << sizeof(PLONG) << std::endl;
    std::cout << "PLONGLONG = " << sizeof(PLONGLONG) << std::endl;
    std::cout << "PLONG_PTR = " << sizeof(PLONG_PTR) << std::endl;
    std::cout << "PLONG32 = " << sizeof(PLONG32) << std::endl;
    std::cout << "PLONG64 = " << sizeof(PLONG64) << std::endl;
    // std::cout << "POINTER_32 = " << sizeof(POINTER_32) << std::endl; // won't work
    // std::cout << "POINTER_64 = " << sizeof(POINTER_64) << std::endl; // won't work
    // std::cout << "POINTER_SIGNED = " << sizeof(POINTER_SIGNED) << std::endl; // won't work
    // std::cout << "POINTER_UNSIGNED = " << sizeof(POINTER_UNSIGNED) << std::endl; // won't work
    std::cout << "PSHORT = " << sizeof(PSHORT) << std::endl;
    std::cout << "PSIZE_T = " << sizeof(PSIZE_T) << std::endl;
    std::cout << "PSSIZE_T = " << sizeof(PSSIZE_T) << std::endl;
    std::cout << "PSTR = " << sizeof(PSTR) << std::endl;
    std::cout << "PTBYTE = " << sizeof(PTBYTE) << std::endl;
    std::cout << "PTCHAR = " << sizeof(PTCHAR) << std::endl;
    std::cout << "PTSTR = " << sizeof(PTSTR) << std::endl;
    std::cout << "PUCHAR = " << sizeof(PUCHAR) << std::endl;
    std::cout << "PUHALF_PTR = " << sizeof(PUHALF_PTR) << std::endl;
    std::cout << "PUINT = " << sizeof(PUINT) << std::endl;
    std::cout << "PUINT_PTR = " << sizeof(PUINT_PTR) << std::endl;
    std::cout << "PUINT8 = " << sizeof(PUINT8) << std::endl;
    std::cout << "PUINT16 = " << sizeof(PUINT16) << std::endl;
    std::cout << "PUINT32 = " << sizeof(PUINT32) << std::endl;
    std::cout << "PUINT64 = " << sizeof(PUINT64) << std::endl;
    std::cout << "PULONG = " << sizeof(PULONG) << std::endl;
    std::cout << "PULONGLONG = " << sizeof(PULONGLONG) << std::endl;
    std::cout << "PULONG_PTR = " << sizeof(PULONG_PTR) << std::endl;
    std::cout << "PULONG32 = " << sizeof(PULONG32) << std::endl;
    std::cout << "PULONG64 = " << sizeof(PULONG64) << std::endl;
    std::cout << "PUSHORT = " << sizeof(PUSHORT) << std::endl;
    std::cout << "PVOID = " << sizeof(PVOID) << std::endl;
    std::cout << "PWCHAR = " << sizeof(PWCHAR) << std::endl;
    std::cout << "PWORD = " << sizeof(PWORD) << std::endl;
    std::cout << "PWSTR = " << sizeof(PWSTR) << std::endl;
    // std::cout << "QWORD = " << sizeof(QWORD) << std::endl; // won't work
    std::cout << "SC_HANDLE = " << sizeof(SC_HANDLE) << std::endl;
    std::cout << "SC_LOCK = " << sizeof(SC_LOCK) << std::endl;
    std::cout << "SERVICE_STATUS_HANDLE = " << sizeof(SERVICE_STATUS_HANDLE) << std::endl;
    std::cout << "SHORT = " << sizeof(SHORT) << std::endl;
    std::cout << "SIZE_T = " << sizeof(SIZE_T) << std::endl;
    std::cout << "SSIZE_T = " << sizeof(SSIZE_T) << std::endl;
    std::cout << "TBYTE = " << sizeof(TBYTE) << std::endl;
    std::cout << "TCHAR = " << sizeof(TCHAR) << std::endl;
    std::cout << "UCHAR = " << sizeof(UCHAR) << std::endl;
    std::cout << "UHALF_PTR = " << sizeof(UHALF_PTR) << std::endl;
    std::cout << "UINT = " << sizeof(UINT) << std::endl;
    std::cout << "UINT_PTR = " << sizeof(UINT_PTR) << std::endl;
    std::cout << "UINT8 = " << sizeof(UINT8) << std::endl;
    std::cout << "UINT16 = " << sizeof(UINT16) << std::endl;
    std::cout << "UINT32 = " << sizeof(UINT32) << std::endl;
    std::cout << "UINT64 = " << sizeof(UINT64) << std::endl;
    std::cout << "ULONG = " << sizeof(ULONG) << std::endl;
    std::cout << "ULONGLONG = " << sizeof(ULONGLONG) << std::endl;
    std::cout << "ULONG_PTR = " << sizeof(ULONG_PTR) << std::endl;
    std::cout << "ULONG32 = " << sizeof(ULONG32) << std::endl;
    std::cout << "ULONG64 = " << sizeof(ULONG64) << std::endl;
    // std::cout << "UNICODE_STRING = " << sizeof(UNICODE_STRING) << std::endl; // won't work
    std::cout << "USHORT = " << sizeof(USHORT) << std::endl;
    std::cout << "USN = " << sizeof(USN) << std::endl;
    // std::cout << "VOID = " << sizeof(VOID) << std::endl; // won't work
    std::cout << "WCHAR = " << sizeof(WCHAR) << std::endl;
    // std::cout << "WINAPI = " << sizeof(WINAPI) << std::endl; // won't work
    std::cout << "WORD = " << sizeof(WORD) << std::endl;
    std::cout << "WPARAM = " << sizeof(WPARAM) << std::endl;

    return 0;
}
```
