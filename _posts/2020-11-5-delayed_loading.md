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

该函数使用两个参数：句柄和字符串，并返回一个FARPROC类型的函数指针。我们需要根据所要引用的函数定义自己的函数指针类型，并对该函数返回值做类型转换。

示例: ![_config.yml]({{ site.baseurl }}/images/delay_loading.PNG)

## TIPS
由于C++有多态机制，编译器在编译时会为同一个函数名附加一些标识符来标记具体是哪个函数，在delay_loading中直接使用函数名是找不到的，这时有两种处理方式，一是使用dll查看器查看实际生成的函数名、二是将不需要重载的函数声明为extern "C", C语言风格的代码没有多态因此不会在函数名中加入标识符。
