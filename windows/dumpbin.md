`dumpbin.exe` は大抵
```
...\Program Files (x86)\Microsoft Visual Studio\<VERSION>\BuildTools\VC\Tools\MSVC\<BUILD VERSION>\bin\Hostx64\x64
```
あたりにいる。


## exe ファイルの依存を表示する。  
(Linux の ldd 的な)

```sh
dumpbin.exe /DEPENDENTS <executable>
```

## exe や dll が 何 bit か表示する。

```sh
dumpbin.exe /headers <executable|dll> | findstr machine
```
