延迟加载是指通过代码声明，在exe开始运行时不加载dll库，需要用到库中函数时再加载dll库。

## 关键函数
**HMODULE LoadLibraryA(
  LPCSTR lpLibFileName
);**

该函数返回一个句柄，表示加载的库。如果不指定绝对路径，会从生成目标文件夹寻找库文件。

**FARPROC GetProcAddress(
    HMODULE   hModule,    // DLL模块句柄
    LPCSTR       lpProcName   // 函数名
);**

该函数使用两个参数：句柄和字符串，并返回一个F
