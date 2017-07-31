---
title: 优雅地写bash脚本
date: 2017-07-31 21:35:18
tags:
---

## 内建变量
变量 | 意义
---|---
`$0` | 脚本文件名
`$1/$2` | 第N(>1)个参数
`$#` | 参数个数
`$@` | 所有参数(数组)
`$*` | 所有参数(字符串)
`$-` | 当前脚本执行时的附加参数，比如`-x`
`$_` | 最近的参数（或者当前脚本执行的目录）
`$IFS` | 输入字段分隔符，一般是空格
`$!` | 最近的后台执行的命令
`$$` | 当前脚本的pid
`$?` | 脚本执行后的返回值

## 传参
用`case`判断参数，支持短选项和长选项
```sh
while [[ $# -gt 0 ]]
do
    KEY=$1
    case $KEY in
        -d|--delay)
            DELAY=$2
            shift
            ;;
        --debug)
            DEBUG=true
            ;;
        *)
            ;;
    esac
    shift
done
```

## 数组
不需要声明，直接以数组方式赋值。几种赋值方式：
```sh
array=(var1 var2)
array=([0]=var1 [1]=var2)
array[0]=var1
```
计算数组元素个数：
`${#array[@]}` or `${#array[*]}`

引用数组：
`${array[@]}`

遍历整个数组：
```
for var in ${array[@]}; do
    echo $var
done
for ((i = 0; i < ${#array[@]}; i++)); do
    echo ${array[$i]}
done
```

## 死循环、sleep、递增
```
COUNT=0
while :
do
    echo $COUNT
    COUNT=`expr $COUNT + 1`
    sleep 1
done
```

## 比较
### 整数比较
运算符 | 意义
---|---
-eq | 等于
-ne | 不等于
-lt | 小于
-le | 小于或等于
-gt | 大于
-ge | 大于或等于

### 字符串比较
表达式 | 意义
---|---
-z "$string" | 空串
-n "$string" | 非空串
"$string1" != "$string2" | 不同
"$string1" == "$string2" | 相同
"$string1" < "$string2" | 小于（字典序）
"$string1" > "$string2" | 大于（字典序）

### 文件比较
运算符 | 意义
---|---
-a | 存在
-e | 存在
-b | 是块文件
-c | 是字符文件
-d | 是目录
-f | 是常规文件
-g | 是set-group-id
-h | 是符号链接
-L | 是符号链接
-k | 设置了sticky位
-p | 有名管道(FIFO)
-r | 可读文件
-s | 大小不为0
-u | 是set-user-id
-w | 可写文件
-x | 可执行文件
