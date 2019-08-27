# Program Common

+ 常用 unicode 中文字符 code points 范围：[\u4e00, \u9fa5]。
+ 如果要包含更广泛的中文字符：[\u2E80, \uFE4F]，取自中日韩统一表意文字（CJK Unified Ideographs），外加一些特殊的字符。
+ windows 批量 kill 进程：`taskkill /F /IM xxx.exe`, 可使用 `1>nul` 或 `>nul` 参数屏蔽程序正确输出信息，但不能屏蔽错误输出信息；如果要屏蔽错误输出信息可以使用 `2>nul`，但不会屏蔽正确输出信息。
+ `@<command>` 可以屏蔽命令本身，`echo off` 可以一直屏蔽命令本身，直到你手动执行 `echo on`。这两个命令都不能屏蔽命令的输出信息。