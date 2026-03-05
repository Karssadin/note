---
tags:
  - Linux
up:
  - "[[编程/归档/八股文/Linux]]"
down:
relation:
---

Shell 是 Linux 的命令行解释器，也是一种脚本语言。常见 Shell：bash（默认）、zsh、sh。

## 脚本执行

```bash
#!/bin/bash                   # shebang 指定解释器
chmod +x script.sh            # 添加执行权限
./script.sh                   # 直接执行
bash script.sh                # 无需执行权限
source script.sh              # 在当前 Shell 环境执行（变量会保留）
```

## 变量

```bash
NAME="hello"                  # 赋值（等号两侧不能有空格）
echo $NAME                    # 引用变量
echo ${NAME}_suffix           # 大括号界定变量名
readonly PI=3.14              # 只读变量
unset NAME                    # 删除变量

# 环境变量
export PATH=$PATH:/usr/local/bin
env                           # 查看所有环境变量
```

## 特殊变量

| 变量 | 含义 |
|------|------|
| `$0` | 脚本名 |
| `$1`~`$9` | 位置参数 |
| `$#` | 参数个数 |
| `$@` | 所有参数（独立字符串） |
| `$*` | 所有参数（单个字符串） |
| `$?` | 上条命令退出码（0=成功） |
| `$$` | 当前进程 PID |

## 条件判断

```bash
if [ -f "file.txt" ]; then
    echo "文件存在"
elif [ -d "dir" ]; then
    echo "目录存在"
else
    echo "都不存在"
fi

# 常用判断
[ -f file ]    # 是否普通文件
[ -d dir ]     # 是否目录
[ -z "$var" ]  # 字符串是否为空
[ "$a" = "$b" ] # 字符串相等
[ $a -eq $b ]  # 数值相等（-ne/-gt/-lt/-ge/-le）
```

## 循环

```bash
# for 循环
for i in {1..10}; do echo $i; done
for file in *.txt; do echo $file; done

# while 循环
while read line; do
    echo "$line"
done < input.txt

# until 循环
until [ $count -ge 10 ]; do
    count=$((count + 1))
done
```

## 函数

```bash
greet() {
    local name=$1              # local 局部变量
    echo "Hello, $name"
    return 0                   # 返回退出码（不是返回值）
}
greet "World"
result=$(greet "World")        # 捕获输出作为"返回值"
```

## 脚本调试

```bash
set -x    # 打印每条执行的命令（调试）
set -e    # 遇到错误立即退出
set -u    # 使用未定义变量时报错
set -o pipefail  # 管道中任意命令失败则整体失败
```

## 数组

```bash
# 定义数组
arr=(apple banana cherry)
arr[3]="date"

# 访问元素
echo ${arr[0]}           # apple
echo ${arr[@]}           # 所有元素：apple banana cherry date
echo ${#arr[@]}          # 元素个数：4

# 遍历
for item in "${arr[@]}"; do
    echo "$item"
done

# 切片
echo ${arr[@]:1:2}       # 从下标1开始取2个：banana cherry

# 追加元素
arr+=("elderberry")

# 关联数组（类似 map，bash 4+）
declare -A map
map["key1"]="value1"
map["key2"]="value2"
echo ${map["key1"]}      # value1
for k in "${!map[@]}"; do echo "$k=${map[$k]}"; done
```

## case 语句

```bash
read -p "输入一个选项: " choice
case "$choice" in
    a|A)
        echo "选择了 A"
        ;;
    b|B)
        echo "选择了 B"
        ;;
    [0-9])
        echo "选择了数字"
        ;;
    *)
        echo "未知选项"
        ;;
esac

# 实用场景：处理命令行参数
case "$1" in
    start)  echo "启动服务" ;;
    stop)   echo "停止服务" ;;
    status) echo "查看状态" ;;
    *)      echo "用法: $0 {start|stop|status}" ; exit 1 ;;
esac
```

## Here Document（heredoc）

```bash
# 基本用法：多行字符串
cat << EOF
第一行
第二行
变量会被展开: $HOME
EOF

# 抑制变量展开（单引号包裹 EOF）
cat << 'EOF'
这里 $HOME 不会被展开
EOF

# 写入文件
cat > config.txt << EOF
HOST=localhost
PORT=8080
DEBUG=true
EOF

# 传递给命令
mysql -u root -p << EOF
USE mydb;
SELECT * FROM users;
EOF

# 缩进（<<- 忽略前导 tab）
if true; then
    cat <<- EOF
        缩进的内容（前导tab被忽略）
    EOF
fi
```

## 常用技巧

```bash
# 管道与重定向
cmd1 | cmd2                    # 管道
cmd > file 2>&1                # stdout 和 stderr 都写入文件
cmd >> file                    # 追加

# 命令替换
files=$(ls)
count=$(wc -l < file.txt)

# 进程替换（避免子 Shell 变量丢失）
while read line; do
    echo "$line"
done < <(ls -la)               # < <(...) 进程替换

# 算术运算
result=$((3 + 4))
result=$(expr 3 + 4)           # 旧式写法
let "result = 3 + 4"

# 字符串操作
${#str}                        # 字符串长度
${str:0:5}                     # 截取前 5 个字符
${str/old/new}                 # 替换第一个
${str//old/new}                # 替换所有
${str#prefix}                  # 删除最短前缀
${str%suffix}                  # 删除最短后缀
${str:-default}                # 如果 str 为空则使用 default

# 判断命令是否存在
if command -v git &>/dev/null; then
    echo "git 已安装"
fi
```
