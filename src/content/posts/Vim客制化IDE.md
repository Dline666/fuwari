---
title: Vim客制化IDE
published: 2025-11-12
description: '工欲善其事，必先利其器'
image: 'background/vimIDE.png'
tags: [Vim]
category: ''
draft: false 
lang: ''
---

---

# 🧠 在 CentOS 7 上从零打造一个像 VSCode 一样强大的 Vim C++ IDE

> <mark>*工欲善其事，必先利其器*</mark>
> 
> 本文记录了我在 **CentOS 7 虚拟机** 上，将传统的 Vim 打造成一个支持语法高亮、智能补全、文件树浏览、状态栏美化的 **轻量级 C++ IDE** 的完整过程。 
> 初步搭建借助chatgpt搭建耗时两天，以下为搭建过程

---

## 🪜 一、准备工作

系统环境：

```
CentOS 7 (x86_64)
```

目标：

- 让 Vim 支持现代语法高亮；

- 拥有 C++ 智能补全功能；

- 增加文件树浏览、状态栏等插件；

- 打造出接近 VSCode 的开发体验。

---

## 🧩 二、升级 Vim 至 9.x 版本

CentOS 自带的 Vim 版本通常较低（7.4），不支持许多现代插件，因此第一步是**编译安装最新版 Vim**。

> 🤔编译安装是指从GitHub上直接下载vim的源码到本地来编译，用yum源仓库来获取的版本无法达到最新版

### 1. 移除旧版本 Vim

> ⚠️ 注意：`vim-minimal` 被系统依赖（如 sudo）所需，请谨慎。

```bash
yum remove vim-enhanced vim-common vim-filesystem -y
```

### 2. 安装依赖库

```bash
yum install -y git gcc make ncurses-devel python3 python3-devel
```

### 3. 获取 Vim 源码并编译

```bash
cd /usr/local/src
git clone https://github.com/vim/vim.git
cd vim
make distclean  # 如果曾经编译过
./configure --with-features=huge \
            --enable-multibyte \
            --enable-python3interp=yes \
            --with-python3-config-dir=$(python3-config --configdir) \
            --enable-gui=no \
            --enable-cscope \
            --prefix=/usr/local
make -j$(nproc)
make install
```

### 4. 替换系统默认 Vim

```bash
ln -sf /usr/local/bin/vim /usr/bin/vim
```

### 5. 验证版本

```bash
vim --version | head -n 1
```

输出示例：

```
VIM - Vi IMproved 9.1 (2024 Jan 02)
```

---

## ⚙️ 三、安装插件管理器 vim-plug

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

验证是否成功：

```bash
ls ~/.vim/autoload/plug.vim
```

存在则表示安装成功。

---

## 📝 四、配置 .vimrc

创建或覆盖 `~/.vimrc`：

```bash
cat > ~/.vimrc << 'EOF'
"========================
"   基础设置
"========================
set nocompatible       
filetype off
set number
set cursorline
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent
set smartindent
set hlsearch
set incsearch
set ignorecase
set smartcase
set backspace=2
set encoding=utf-8
set fileencodings=utf-8,gbk,ucs-bom,cp936
syntax on

"========================
"   键位映射
"========================
imap jk <ESC>
imap kj <ESC>
nnoremap <F2> :bp<CR>
nnoremap <F3> :bn<CR>
nnoremap <F4> :wa<CR>
nnoremap <F5> :wa<CR>:make<CR>
nnoremap <F8> :sh<CR>

"========================
"   插件管理
"========================
call plug#begin('~/.vim/plugged')

Plug 'preservim/nerdtree'              " 文件树浏览器
Plug 'octol/vim-cpp-enhanced-highlight' " C++ 语法增强
Plug 'Rip-Rip/clang_complete'           " C/C++ 智能补全
Plug 'vim-airline/vim-airline'          " 状态栏增强

call plug#end()

"========================
"   clang_complete 设置
"========================
let g:clang_use_library = 1
let g:clang_complete_auto = 1
let g:clang_complete_copen = 1
let g:clang_close_preview = 1
let g:clang_library_path = '/usr/lib64/llvm/libclang.so'

"========================
"   快捷键
"========================
map <F9> :NERDTreeToggle<CR>
EOF
```

---

## 🚀 五、安装插件

打开 Vim：

```bash
vim
```

在命令模式中输入：

```vim
:PlugInstall
```

等待所有插件安装完成后，退出 Vim。

---

## 🧠 六、验证功能

### 1️⃣ NERDTree 文件树

按 **F9** 键或执行：

```
:NERDTreeToggle
```

左侧应出现文件树浏览器。


> 🤔
> 
> 通过 o（open）打开文件
> 通过 ctrl + w (窗口操作)  ---> h(向左) 返回文件树

### 2️⃣ 语法高亮

打开 `.cpp` 文件，应有彩色语法提示。

### 3️⃣ 状态栏

底部应显示美观的状态信息。

### 4️⃣ 自动补全

输入：

```cpp
std::
```

应自动弹出 `cout`、`cin` 等提示。  
若未自动弹出，可按：

```
Ctrl + X + O
```

---

## 🎨 七、可选增强

| 功能    | 插件                       | 说明               |
| ----- | ------------------------ | ---------------- |
| 代码格式化 | `rhysd/vim-clang-format` | 一键自动对齐缩进         |
| 智能诊断  | `neoclide/coc.nvim`      | 基于 LSP 的语义补全（较重） |
| 主题美化  | `morhetz/gruvbox`        | 柔和配色方案           |

---

## ✅ 八、最终效果

> Vim 此时已经是一个 **轻量级 C++ IDE**：
> 
> - 打开即自动保存；
> 
> - 自动高亮、自动补全；
> 
> - 一键格式化代码；
> 
> - 一键编译运行；
> 
> - 文件树管理与状态栏美化；
> 
> - 插件结构清晰、易维护。

---

## 📦 总结

通过手动编译 Vim 9.x 并结合 `vim-plug` 插件管理，我们让一款轻量编辑器焕发新生：

- 灵活；

- 快速；

- 干净；

- 可无限扩展。

在 CentOS 7 环境下，这个方案是**高效、简洁又稳定**的 C++ 开发选择。

---

### Tips

> windows下按`win+.`：可以在markdown编辑器中插入emoji表情

<mark>最后贴上我的.vimrc文件</mark>

```vim
"========================
"   基础设置
"========================
set nocompatible
filetype off

set number              " 显示行号
set cursorline          " 高亮当前行
set tabstop=4           " Tab 显示为 4 个空格
set shiftwidth=4        " 自动缩进宽度 4
set expandtab           " Tab 转为空格
set autoindent
set smartindent
set hlsearch            " 高亮搜索
set incsearch          " 搜索时逐步匹配
set ignorecase          " 搜索时忽略大小写
set smartcase           " 若包含大写则区分大小写
set backspace=2         " 允许退格删除缩进、换行
set encoding=utf-8
set fileencodings=utf-8,gbk,ucs-bom,cp936
set termencoding=utf-8
syntax on

"========================
"   键位映射
"========================
imap jk <ESC>
imap kj <ESC>

nnoremap <F2> :bp<CR>
nnoremap <F3> :bn<CR>
nnoremap <F4> :wa<CR>
nnoremap <F5> :wa<CR>:make<CR>
nnoremap <F8> :sh<CR>

"========================
"   插件管理（vim-plug）
"========================
call plug#begin('~/.vim/plugged')
" 文件树浏览器
Plug 'preservim/nerdtree'
" C/C++ 语法高亮增强
Plug 'octol/vim-cpp-enhanced-highlight'
" 自动补全（基于 clang）
Plug 'Rip-Rip/clang_complete'
" 状态栏增强
Plug 'vim-airline/vim-airline'
" 括号补全
Plug 'jiangmiao/auto-pairs'
"自动保存
Plug '907th/vim-auto-save'
"自动格式化代码
Plug 'rhysd/vim-clang-format'
"实时代码错误检查(在保存时显示编译错误，警告)
Plug 'dense-analysis/ale'
"语义补全加LSP支持，暂未实现
"Plug 'neoclide/coc.nvim'
call plug#end()

"========================
"   clang_complete 配置
"========================
let g:clang_use_library = 1
let g:clang_complete_auto = 1
let g:clang_complete_copen = 1
let g:clang_close_preview = 1

" 指定 libclang.so 的路径（根据系统可能不同）
let g:clang_library_path = '/usr/lib64/llvm/libclang.so'

"========================
"   NERDTree 快捷键
"========================
map <F9> :NERDTreeToggle<CR>
"       ========================
"   格式化代码vim-clang-format 快捷键
"       =======================
map <F7> :ClangFormat<CR>
"配置风格（Google/LLVM/Mozilla/Chromium）
let g:clang_format#style_option = 'Google'

"=============================
"   vim-auto-save 配置
"============================
let g:auto_save = 1
```
