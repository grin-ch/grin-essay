| 指令                      | 功能                       | 备注                |
| ----------------------- | ------------------------ | ----------------- |
| **删除/复制/粘贴**            | **d和y一个是删除一个是复制,用法相同**   |                   |
| dd/yy                   | 删除/复制当前行                 |                   |
| ndd/yy                  | 删除/复制当前往后n行              |                   |
| d/y0                    | 删除/复制游标处到该行第一个字符         | 数字`0`             |
| d/y$                    | 删除/复制游标处到该行最后一个字符        |                   |
| d/yG                    | 删除/复制光标到文件最后一行的所有字符      |                   |
| p                       | 向下一行粘贴                   |                   |
| P                       | 向上一行粘贴                   |                   |
| **文字选择**                | **选择多个字符**               | **配合d/y/p**       |
| v                       | 字符选择                     | 小写`v`             |
| V                       | 行选择                      | 大写`V`             |
| ctrl + v                | 区块选择,矩形                  |                   |
| **回退操作**                |                          |                   |
| u                       | 复原前一个动作                  | ctrl + z          |
| ctrl + r                | 重复前一个动作                  | ctrl + shift + z  |
| .                       | 重复前一个动作                  |                   |
| **编辑模式**                |                          |                   |
| i,I                     | i:光标所在处,I:当前行第一个非空格字符处   | 进入插入模式            |
| a,A                     | a:光标下一个字符处,A:最后一个字符处     | 进入插入模式            |
| o,O                     | o:下一行新起一行,O:上一行新起一行      | 进入插入模式            |
| r,R                     | r:取代光标所在字符一次,R:一直取代      | 进入取代模式            |
| ESC                     | 推出到一般指令模式                |                   |
| **常用快捷键**               | **一般指令模式**               |                   |
| ctrl + f                | 向下翻一页                    | 相当于Page Down      |
| ctrl + b                | 向上翻一页                    | 相当于Page Up        |
| ctrl + d                | 向下翻半页                    |                   |
| ctrl + u                | 向上翻半页                    |                   |
| 0 或 Home                | 移动到这一行最前的字符处             | 数字`0`             |
| $ 或 End                 | 移动到这一行最后的字符处             |                   |
| H                       | 移动到这页的最开头字符              |                   |
| G                       | 移动到文件的最后一行               |                   |
| nG                      | 移动到文件的第n行                | n是数字              |
| gg                      | 动到文件的第1行                 | 相当于1g             |
| n Enter                 | 向下移动n行                   |                   |
| /[word]                 | 向下查询 word                | word 是输入的字符       |
| ?[word]                 | 向上查询word                 |                   |
| n                       | 重复前一个**搜索**动作            |                   |
| N                       | 反向执行前一个**搜索**动作          |                   |
| :n1,n2s/w1/w2/g         | 在n1和n2之间搜索w1并且替换为w2      | 比如`:3,5s/a/b/g`   |
| :1,$s/w1/w2/g           | 全文范围搜索w1并且替换为w2          |                   |
| :1,$s/w1/w2/gc          | 全文范围搜索w1并且替换为w2          | 取代前会逐个提示          |
| nx                      | 向后删除n格                   | x为一格,x=1x         |
| X                       | 向前删除一格                   |                   |
| **指令**                  | **指令列模式**                |                   |
| :q                      | 离开                       |                   |
| :q!                     | 不保存就强制离开                 |                   |
| :wq                     | 保持后离开                    |                   |
| ZZ                      | 修改过文件则自动保存,离开            | 大写`Z`             |
| :w [filename]           | 保持到 filename,            | `:w`则为保存          |
| :r [filename]           | 将filename里的内容写入到当前光标所在列后 |                   |
| :n1,n2 w [filename]     | 将n1到n2的内容存到filename      |                   |
| :! [command]            | 暂时离开vim,查看command的结果     | 如`:! ls`          |
| :set nu                 | 显示行号                     |                   |
| :set nonu               | 不显示行号                    |                   |
| :set fileformats=[type] | 设置换行符为type的格式            | type : [unix,dos] |
| **多文件操作**               |                          |                   |
| :n                      | 下一个文件                    |                   |
| :N                      | 上一个文件                    |                   |
| :files                  | 查看文件列表                   |                   |
| **多窗口**                 |                          |                   |
| :sp {filename}          | 开启一个新的并列窗口               | filename 可以指定文件   |
| ctrl + w + j            | 下一个窗口                    | 先ctrl + w, 再按j    |
| ctrl + w + k            | 上一个窗口                    |                   |
| ctrl + w + w            | 切换窗口                     |                   |
| ctrl + w + q            | 关闭窗口                     |                   |
| **补齐**                  |                          |                   |
| ctrl + x -> ctrl + n    | 根据当前文件内容补齐               |                   |
| ctrl + x -> ctrl + f    | 根据当前目录文件名补齐              |                   |
| ctrl + x -> ctrl + o    | 以扩展名为语法和vim内置关键字补齐       |                   |

##### 配置文件在家目录下 ~/.vimrc

```sh
" 设置编码
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8

" 显示
set nu				" 行号显示
set bg=dark			" 背景色
set hlsearch		" 高亮反白
set ruler			" 显示光标
set showmode		" 左下角状态显示
set cursorline		" 突出显示当前行
set cursorcolumn	" 突出显示当前列

" 缩进
set expandtab		" 设置空格为缩进
set shiftwidth=4	" 设置每一级缩进为4个空格
set backspace=2		" 随时退格键删除
set autoindent		" 自动缩进
set ts=4			" 设置Tab长度为4空格

" 智能提示
syntax on			" 语法检查 高亮
set showmatch		" 显示括号匹配
set incsearch		" 实时搜索
set ignorecase		" 搜索时大小写不敏感

" 符号配对
inoremap { {}<Esc>i<CR><Esc>koi<Esc>j<C-S-v><S-%>=j<S-$>xa
inoremap ' ''<Esc>i
inoremap " ""<Esc>i
inoremap ` ``<Esc>i
inoremap [ []<Esc>i
inoremap ( ()<Esc>i

filetype plugin indent on	" 启用自动补全
"au InsertLeave *.go,*.sh,*.yml write	"退出插入模式指定类型的文件自动保存

" 新建 .sh 文件，自动插入文件头
autocmd BufNewFile *.sh exec ":call SetTitle()"
""定义函数SetTitle，自动插入文件头
func SetTitle()
    if &filetype == 'sh'
        call setline(1,"\#########################################################################")
        call setline(2, "\# Author: grin")
        call setline(3, "\# Created Time: ".strftime("%c"))
        call setline(4, "\#########################################################################")
        call setline(5, "\#!/bin/bash")
        call setline(6, "")
    endif
    " 自动定位到文件末尾
    normal G
endfunc
```



