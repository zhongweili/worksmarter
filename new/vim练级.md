# vim
https://coolshell.cn/articles/5426.html

vim [cheatsheet](https://github.com/skywind3000/awesome-cheatsheets/blob/master/editors/vim.txt)
## 基础
:q    关闭
:w    保存
i    插入模式
h j k l    左上下右移动
x    删除当前字符
dd    删除当前行
u    撤销上一次操作
/pattern   查找

## 常用
0    跳到行首
$    跳到行尾
gg    跳到开头
g(n)    跳到第n行 
G    跳到最后一行
{    移动到上一段
}    移动到下一段
a    光标后进入插入模式
gi    跳到上一次插入模式位置
o    在下一行插入新行
CRTL-W    删除一个单词
CRTL-[    退出插入模式
SHIFT-Right    向右移动一个单词(同 w)
v    开始标记
y    复制标记内容
yy    复制当前行
p    粘贴到光标后
P    粘贴到光标前
:w <filename>    按文件名保存
:set number     显示行号