# Mac系统安装Homebrew
!> 原文参考知乎 https://zhuanlan.zhihu.com/p/111014448

## 1. 使用系统自带的Terminal复制并粘贴下面的脚本 <!-- {docsify-ignore} -->
完整版：（推荐）
```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```
精简版：
```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)" speed
```
**注：** 不要使用root用户执行上面的脚本，不然可能安装过程中会出现异常

## 2.  根据脚本内容提示做出选择，安装成功以后，需要重启终端或者运行 `source /Users/你的用户名/.zprofile`   否则可能无法使用<!-- {docsify-ignore} -->
?> source sourcefilename 作用是在当前bash环境下读取并执行Filename中的命令

## 3. 验证brew是否安装成功<!-- {docsify-ignore} -->
```bash
brew -v
```
出现以下内容表示安装成功
```bash
Homebrew 3.6.20

```
