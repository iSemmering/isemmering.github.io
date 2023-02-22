# Mac安装nvm
!> 本教程主要通过brew的方式安装nvm，如果你的系统还未安装brew，请参考 [Mac系统安装Homebrew](/other/install-homebrew)
## 1. 使用系统自带的Terminal复制并粘贴下面的脚本<!-- {docsify-ignore} -->
```bash
brew install nvm
```
?>如果出现以下提示，请按照操作进行处理
```bash
Please note that upstream has asked us to make explicit managing
nvm via Homebrew is unsupported by them and you should check any
problems against the standard nvm install method prior to reporting.

You should create NVM's working directory if it doesn't exist:

  mkdir ~/.nvm

Add the following to ~/.zshrc or your desired shell
configuration file:

  export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

You can set $NVM_DIR to any location, but leaving it unchanged from
/opt/homebrew/opt/nvm will destroy any nvm-installed Node installations
upon upgrade/reinstall.

Type `nvm help` for further information.
```
### 1.1 如果当前目录没有缺少.nvm目录，请自行创建：`mkdir ~/.nvm`<!-- {docsify-ignore} -->
### 1.2 编辑.zshrc文件`vim ~/.zshrc`，插入控制台输出的信息内容<!-- {docsify-ignore} -->
```bash
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```
### 1.3 保存成功以后，重新执行 `source ~/.zshrc`，然后重新执行`brew install nvm`<!-- {docsify-ignore} -->

## 2. 验证nvm是否安装成功：`nvm -v` <!-- {docsify-ignore} -->