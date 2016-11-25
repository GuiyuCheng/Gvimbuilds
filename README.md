# Gvimbuilds

1. 官方提供的默认下载安装版本为32位版本，64位版本需要自编译，当然，有很多非官方渠道提供预编译的32/64位版本，官网有部分链接，个人喜欢的是另外一个网站 https://bintray.com/micbou/generic/vim，实时更新最新版本的预编译32/64位安装版
2. 由于插件很多都是使用Python编写的，比如YCM，所以必须安装对应32/64位版本的python 2.x或Python 3.x，官网下载即可 https://www.python.org/downloads/
3. 如果使用Vundle插件且需要从github下载，则需要Git和Curl的支持

<1>下载安装msysGit或者git for windows（已经集成了Curl），注意安装过程中选择Run git from Windows command prompt选项，当然，如果选择了Use Git Bash Only也没关系，手动添加Git/bin和Git/cmd到系统环境变量Path即可。

<2>新建一个文本文件，添加如下内容，然后保存并改名为curl.cmd，将其复制到Git安装目录下的Git/cmd目录下（注意修改git版本位数对应的PATH路径--curl工具路径）

@rem Do not use "echo off" to not affect any child calls.
@setlocal

@rem Get the abolute path to the parent directory, which is assumed to be the
@rem Git installation root.
@for /F "delims=" %%I in ("%~dp0..") do @set git_install_root=%%~fI
@set PATH=%git_install_root%\bin;%git_install_root%\mingw\bin;%git_install_root%\mingw64\bin;%PATH%
@rem !!!!!!! For 64bit msysgit, replace 'mingw' above with 'mingw64' !!!!!!!

@if not exist "%HOME%" @set HOME=%HOMEDRIVE%%HOMEPATH%
@if not exist "%HOME%" @set HOME=%USERPROFILE%

@curl.exe %*

<3>在命令行输入以下内容对git/curl是否正确安装使用进行验证（正常情况下会输出版本信息）

git --version
curl --version

<4>下载并配置Vundle插件，注意在windows下，vim默认使用的配置文件和目录为_vimrc/vimfiles

首先使用git命令将Vundle下载至 %HOME% 或者 %USERPROFILE% 目录下的vimfiles目录中，该目录一般为C:\Users\${username}\vimfiles

git clone https://github.com/VundleVim/Vundle.vim.git %USERPROFILE%/vimfiles/bundle/Vundle.vim

其次必须在_vimrc文件中添加Vundle配置内容，重点注意以下两条（其他内容与linux下的配置相同）

set rtp+=%HOME%/vimfiles/bundle/Vundle.vim/
call vundle#begin('%HOME%/vimfiles/bundle/')

<5>以上内容请参考Vundle的Github帮助页面 https://github.com/VundleVim/Vundle.vim/wiki/Vundle-for-Windows

5. 对于vim配置而言，如果设定 encoding=UTF-8，则要求vim相关的目录结构中不能包含中文，比如用户名使用中文名称，则系统无法搜索 $HOME 目录下的内容如插件、主题、缓存文件等等

注意：如果不使用 UTF-8，那么系统默认使用 cp936，配置中使用 UTF-8/Unicode 编码的部分内容就会无法识别，启动 vim 时会报错，还有Powerline使用的特殊字符设定那里也会报错

6. 为了便于修改管理，建议仅在 $HOME 目录下添加一个用户配置文件 _vimrc，里面仅包含一句内容

source ${userpath}/_vimrc

然后将 _vimrc/vimfiles 等文件全部放置于自定义路径 ${userpath} 下，并在该 _vimrc 中添加

set rtp+=${userpath}/vimfiles

当然，上述Vundle下载路径也要修改为该目录，同时相应修改配置语句为

set rtp+=${userpath}/vimfiles/bundle/Vundle.vim/
call vundle#begin('${userpath}/vimfiles/bundle/')

注意：如果使用 UTF-8 编码，此步可以解决中文用户名引起的部分问题，但是缓存问题依然存在，可以尝试使用修改 TMP/TEMP 环境变量的路径来解决，但是会影响其他软件的缓存目录

7. YCM的编译需要VS/Cmake/7.z/Python等支持，相当麻烦，故而如果可以下载到预编译版并可以正常使用最好，否则的话，建议还是不要使用了