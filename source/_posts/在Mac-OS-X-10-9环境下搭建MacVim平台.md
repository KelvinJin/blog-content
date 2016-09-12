---
title: 在Mac OS X 10.9环境下搭建MacVim平台
permalink: untitled
id: 5
updated: '2015-01-27 18:56:26'
date: 2015-01-27 18:52:07
tags:
---

最近想练练写代码，但是用的是Mac系统，由于一些原因不打算装Windows，而且之前由于好奇又提前安装了OS X 10.9的mavericks development preview版本，所以只好想办法在这样尴尬的环境下搭建一个顺手的programming平台。

之前倒是用过xcode，很强大的一款IDE，不过对C和C++的支持似乎有限。另外，由于之前在Linux上尝试过Vim，可惜能力有限未能配置好，所以一直有遗憾。正好现在想在Mac上试试搭建编程环境，不如重拾Vim。在Mac上自带有比较新版本的Vim，不过据说用MacVim的人也挺多，因为后者对Mac系统进行了一些优化，比如一些蛋疼的快捷键。基于这些原因，我打算尝试在Mac系统下搭建一个用着顺手的MacVim。

+ 下载最新版的MacVim：
[下载MacVim](https://code.google.com/p/macvim/)

我的是7.4b BETA版，然后解压后里面有三个文件：一个是典型的Mac App程序，这个拖到应用程序里面去就行了；一个是readme好好看看；还有一个是命令行程序，把它放到/usr/local/bin/文件夹下面（可以在Finder程序中选择“前往”-“前往文件夹”，或者按command+shift+G），这样以后在命令行就可以快捷打开macvim（比如你想编辑一个文件，就cd到那个文件所在目录然后macvim “文件名”就可以打开编辑了）。

+ 安装Vundle插件：

这个插件十分强大，用于管理Vim的插件安装，启用或删除。在下载之前我们先建一些有用的目录，在终端里面敲以下命令：
```shell
$ mkdir -p ~/.vim/bundle/bundle
$ mkdir ~/.vim/autoload/
```
然后执行命令下载Vundle：
```
$ git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
```
下载成功后我们先将vundle自己的一些必要设置写到MacVim的配置文件中。执行以下命令：
```
$ mvim ~/.vimrc
```
这是Vim的配置文件，在其中写入下列代码：
```conf ~/.vimrc
set nocompatible               " be iMproved
filetype off                   " required!

set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" let Vundle manage Vundle
" required!
Bundle 'gmarik/vundle'

" My Bundles here:
"
" original repos on github
Bundle 'tpope/vim-fugitive'
Bundle 'Lokaltog/vim-easymotion'
Bundle 'rstacruz/sparkup', {'rtp': 'vim/'}
Bundle 'tpope/vim-rails.git'
" vim-scripts repos
Bundle 'L9'
Bundle 'FuzzyFinder'
" non github repos
Bundle 'git://git.wincent.com/command-t.git'
" git repos on your local machine (ie. when working on your own plugin)
Bundle 'file:///Users/gmarik/path/to/plugin'
" ...

filetype plugin indent on     " required!
"
" Brief help
" :BundleList          - list configured bundles
" :BundleInstall(!)    - install(update) bundles
" :BundleSearch(!) foo - search(or refresh cache first) for foo
" :BundleClean(!)      - confirm(or auto-approve) removal of unused bundles
"
" see :h vundle for more details or wiki for FAQ
" NOTE: comments after Bundle command are not allowed..
```
记得保存。

+ 安装YouCompleteMe插件

这里我用安装YCM插件为例来说明怎么为MacVim安装插件。当然，顺便也分享一下在OS X 10.9中（有些特殊），应该怎么安装这个碉堡了的插件。

我还是忍不住想说，这个插件真是牛逼。它集成了很多自动补全相关的Vim插件，而且看样子似乎是专为MacVim设计的，只是后来扩展到了其他平台。而且该插件不仅对C、C++的支持很好，就连Java，Python什么的也有插件辅助。最重要的是，它脱离了传统的Ctags的那种上下文索引补全，免去了更新文件后必须再次执行Ctags检索命令的复杂过程（虽然也有方法能使得很方便）。YCM采用实时检索的Clang工具，对当前文件进行编译，链接相关的文件（这里的文件可以提前设置好，不一定是程序中实际用到的，也就是说所有的库文件都能检索），并且仅在打开文件进行编辑时执行一次，后面就用缓存检索，大大加快了补全速度。当然，缺点就是当你的项目特别大的时候，“通过编译”似乎是一件不太容易的事情。。。

好吧，说了这么多废话，赶紧来说怎么安装，哦差点忘了说，我这个方法是使得YCM支持C、C++的补全。

1.由于OS X 10.9的特殊性，/usr/include目录已经不见了= =。我们必须先下载Xcode 5.0，然后执行下列链接命令:
```shell
ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX10.9.sdk/usr/include/ /usr/include 
```
2.将YCM下载到本地，和Vundle一样，执行下列命令：
```shell
$ git clone https://github.com/Valloric/YouCompleteMe ~/.vim/bundle/
```
3.安装过程中需要用到CMake，去官网下个独立包来安装（我选的dmg的方便）：
[CMake下载](http://www.cmake.org/cmake/resources/software.html)
4.安装完CMake后，由于此时YCM还不支持OS X 10.9，所以如果直接按照作者介绍的那样快捷安装是会出错的。我们需要采用完整方法。先手动下载LLVM/Clang的最新版：
[Clang下载](http://llvm.org/releases/download.html)
选Mac系统的Binary就行了。下完后解压，不要动。继续打开终端，执行如下命令：
```shell
$ mkdir ~/ycm_build
$ mkdir -p ~/ycm_temp/llvm_root_dir
```
5.然后把刚才解压的LLVM的一堆bin、doc、include等文件夹放到这里面去。接着就是最关键的一步了，执行如下命令：
```shell
$ cd ~/ycm_build
$ cmake -G "Unix Makefiles" -DPATH_TO_LLVM_ROOT=~/ycm_temp/llvm_root_dir -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 . ~/.vim/bundle/YouCompleteMe/cpp
```
OK，理论上讲应该是安装成功了，如果不出错的话，我记得最后一步可能会出错，会说无法创建"Unix Makefiles"，解决方法麻烦自己google下咯，似乎不太困难。其实最关键的两步，一步是Xcode的库文件链接到usr/include，还有就是最后一步的参数配置。安装完成后，还需要一个.ycm_extra_conf.py文件来支持检索，这个文件可以自己配置。局部的配置方法是在代码同一目录建立这样一个文件：
```
$ mvim .ycm_extra_conf.py
```
然后写入以下内容（这是我的配置，具体说明请参考作者的github）：
```python .ycm_extra_conf.py
import os
import ycm_core

'-x',
'c++',
'-I',
'/usr/include/c++/4.2.1/',
'-I',
'.'
]
compilation_database_folder = ''

if compilation_database_folder:
  database = ycm_core.CompilationDatabase( compilation_database_folder )
else:
  database = None

def DirectoryOfThisScript():
  return os.path.dirname( os.path.abspath( __file__ ) )

def MakeRelativePathsInFlagsAbsolute( flags, working_directory ):
  if not working_directory:
    return list( flags )
  new_flags = []
  make_next_absolute = False
  path_flags = [ '-isystem', '-I', '-iquote', '--sysroot=' ]
  for flag in flags:
    new_flag = flag

    if make_next_absolute:
      make_next_absolute = False
      if not flag.startswith( '/' ):
        new_flag = os.path.join( working_directory, flag )

    for path_flag in path_flags:
      if flag == path_flag:
        make_next_absolute = True
        break

      if flag.startswith( path_flag ):
        path = flag[ len( path_flag ): ]
        new_flag = path_flag + os.path.join( working_directory, path )
        break

    if new_flag:
      new_flags.append( new_flag )
  return new_flags

def FlagsForFile( filename ):
  if database:
    # Bear in mind that compilation_info.compiler_flags_ does NOT return a
    # python list, but a "list-like" StringVec object
    compilation_info = database.GetCompilationInfoForFile( filename )
    final_flags = MakeRelativePathsInFlagsAbsolute(
      compilation_info.compiler_flags_,
      compilation_info.compiler_working_dir_ )

    # NOTE: This is just for YouCompleteMe; it's highly likely that your project
    # does NOT need to remove the stdlib flag. DO NOT USE THIS IN YOUR
    # ycm_extra_conf IF YOU'RE NOT 100% YOU NEED IT.
  else:
    relative_to = DirectoryOfThisScript()
    final_flags = MakeRelativePathsInFlagsAbsolute( flags, relative_to )

  return {
    'flags': final_flags,
    'do_cache': True
  }
```

OK，最后附上我自己的.vimrc，里面已经有一些设置了，也包含一些插件的设置：
```conf .vimrc
" An example for a vimrc file.
"
"
" Maintainer:	Bram Moolenaar &lt;Bram@vim.org&gt;
" Last change:	2011 Apr 15
"
" To use it, copy it to
"     for Unix and OS/2:  ~/.vimrc
"	      for Amiga:  s:.vimrc
"  for MS-DOS and Win32:  $VIM_vimrc
"	    for OpenVMS:  sys$login:.vimrc

" When started as "evim", evim.vim will already have done these settings.
if v:progname =~? "evim"
  finish
endif

" Use Vim settings, rather than Vi settings (much better!).
" This must be first, because it changes other options as a side effect.
set nocompatible

" 显示行号
set number

" 语法高亮
syntax on

" 按照C语法自动缩进
set cindent

" 设置缩进长度
set tabstop=4 
set shiftwidth=4 
set softtabstop=4
set noexpandtab
set smarttab

" 设置tab键跟行尾符显示出来
set list lcs=tab:&gt;-,trail:-

" 设置自动换行
set wrap

"-------------------------------------
"		搜索设置
"-------------------------------------
"打开搜索高亮
set hlsearch

" 忽略大小写
set ignorecase

" 在查找过程中就高亮
set incsearch

" 显示括号对应
set showmatch

" 编码设置
set encoding=utf-8
set fenc=utf-8    
set fileencodings=ucs-bom,utf-8,cp936,gb2312,gb18030,big5 

" 设置背景色
set bg=dark

" 设置颜色主题
colorscheme koehler

" 设置透明度
set transparency=20

"-----------------------
"	插件设置
"-----------------------

filetype on			" required!

set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" let Vundle manage Vundle 
" " required!
" 这是Vundle本身的设置
Bundle 'gmarik/vundle'

" 这是我的插件设置
" original repos on github

" vim-scripts repos

Bundle 'Valloric/YouCompleteMe'
Bundle 'winmanager'
Bundle 'taglist.vim'
Bundle 'bufexplorer.zip'

" non github repos
Bundle 'https://github.com/scrooloose/nerdtree.git'
Bundle 'https://github.com/scrooloose/nerdcommenter.git'
Bundle 'https://github.com/scrooloose/syntastic.git'

let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/cpp/ycm/.ycm_extra_conf.py'

 filetype plugin indent on     " required!  
 "  
 " Brief help  
 " :BundleList          - list configured bundles  
 " :BundleInstall(!)    - install(update) bundles  
 " :BundleSearch(!) foo - search(or refresh cache first) for foo  
 " :BundleClean(!)      - confirm(or auto-approve) removal of unused bundles  
 "  
 " see :h vundle for more details or wiki for FAQ  
 " NOTE: comments after Bundle command are not allowed.. 
 "
func! CompileGcc()
    exec "w"
    let compilecmd="!gcc "
    let compileflag="-o %&lt; "
    if search("mpi/.h") != 0
        let compilecmd = "!mpicc "
    endif
    if search("glut/.h") != 0
        let compileflag .= " -lglut -lGLU -lGL "
    endif
    if search("cv/.h") != 0
        let compileflag .= " -lcv -lhighgui -lcvaux "
    endif
    if search("omp/.h") != 0
        let compileflag .= " -fopenmp "
    endif
    if search("math/.h") != 0
        let compileflag .= " -lm "
    endif
    exec compilecmd." % ".compileflag
endfunc
func! CompileGpp()
    exec "w"
    let compilecmd="!g++ "
    let compileflag="-o %&lt; "
    if search("mpi/.h") != 0
        let compilecmd = "!mpic++ "
    endif
    if search("glut/.h") != 0
        let compileflag .= " -lglut -lGLU -lGL "
    endif
    if search("cv/.h") != 0
        let compileflag .= " -lcv -lhighgui -lcvaux "
    endif
    if search("omp/.h") != 0
        let compileflag .= " -fopenmp "
    endif
    if search("math/.h") != 0
        let compileflag .= " -lm "
    endif
    exec compilecmd." % ".compileflag
endfunc

func! RunPython()
        exec "!python %"
endfunc
func! CompileJava()
    exec "!javac %"
endfunc

func! CompileCode()
        exec "w"
        if &amp;filetype == "cpp"
                exec "call CompileGpp()"
        elseif &amp;filetype == "c"
                exec "call CompileGcc()"
        elseif &amp;filetype == "python"
                exec "call RunPython()"
        elseif &amp;filetype == "java"
                exec "call CompileJava()"
        endif
endfunc

func! RunResult()
        exec "w"
        if search("mpi/.h") != 0
            exec "!mpirun -np 4 ./%&lt;"
        elseif &amp;filetype == "cpp"
            exec "! ./%&lt;"
        elseif &amp;filetype == "c"
            exec "! ./%&lt;"
        elseif &amp;filetype == "python"
            exec "call RunPython"
        elseif &amp;filetype == "java"
            exec "!java %&lt;"
        endif
endfunc

map  :call CompileCode()
imap  :call CompileCode()
vmap  :call CompileCode()

map  :call RunResult()
```