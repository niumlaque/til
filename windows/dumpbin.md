Windows で exe ファイルの依存を表示する。  
(Linux の ldd 的な)

```sh
dumpbin.exe /DEPENDENTS <executable>
```

`dumpbin.exe` は大抵
```
...\Program Files (x86)\Microsoft Visual Studio\<VERSION>\BuildTools\VC\Tools\MSVC\<BUILD VERSION>\bin\Hostx64\x64
```
あたりにいる。
