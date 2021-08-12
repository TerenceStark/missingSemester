# HW4Solution

## Author:金之航

## StuID:2183411101



## Problem

### 1.Regex One

### ![image-20210705203034832](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210705203034832.png)

### regex101 test:

![image-20210705224112911](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210705224112911.png)



### 2.wordcount

![image-20210706003427119](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210706003427119.png)

```shell
cat /usr/share/dict/words | tr "[:upper:]" "[:lower:]" | grep -E "(^[^aeiou.\-\s]*[aeiou]{1}[^aeiou.\-\s]*$)" | sed -E "s/.*([a-z]{1})$/\1/" | sort | uniq -c | sort -r | grep -E "[aeiou]{1}$" | head -5

cat /usr/share/dict/words | tr "[:upper:]" "[:lower:]" | grep -E "(^[^aeiou.\-\s]*[aeiou]{1}[^aeiou.\-\s]*$)" | sed -E "s/.*([a-z]{1})$/\1/" | sort | uniq -c | sort -n | grep -E "[aeiou]{1}$" | head -5

tr "[:upper:]" "[:lower:]" #忽略大小写
grep -E "(^[^aeiou.\-\s]*[aeiou]{1}[^aeiou.\-\s]*$)" #获取想要的数据，仅含一个元音字母的单词
sed -E "s/.*([a-z]{1})$/\1/" #利用脚本来处理文本文件，只留下元音字母
sort | uniq -c #用uniq -c统计元音字符出现次数
sort -n | grep -E "[aeiou]{1}$" | head -5 #按大小排列
```

output:

![image-20210705223844854](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210705223844854.png)



### 3.sed

```shell
sed s/REGEX/SUBSTITUTION/ input.txt > input.txt 表达式中后一个 input.txt会首先被清空，而且是发生在前的。所以前面一个input.txt在还没有被 sed 处理时已经为空了。在使用正则处理文件前最好是首先备份文件。

sed -i.bak s/REGEX/SUBSTITUTION/ input.txt > input.txt
可以自动创建一个后缀为 .bak 的备份文件。
```



### 4.curl  http://dean.xjtu.edu.cn

```shell
curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n

curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | awk 'BEGIN { row = 0 } $2 ~ /^<d.*v>$/ { row +=1 } END { print row }' #随便写的
```

![image-20210705235548410](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210705235548410.png)

```shell
curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | awk 'BEGIN { row = 0 } { print $1 } END { print row } ' | paste -sd+ | bc -l #求行数，可用wc代替

curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | paste -sd\ |  awk '{print $8}' #求中位数

curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | awk 'BEGIN { row = 0 } { print $1 } END { print row } ' | paste -sd\ | awk '{print ($7+$8)/2}'  #求平均
#...

sudo yum -y install R  #安装R语言
curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | awk '{ print $1 }' | R --slave -e 'x<-scan(file="stdin", quiet=TRUE);summary (x)'
#or
curl http://dean.xjtu.edu.cn | grep -Eo "(<[a-z]+)( ?.*?)*?>" | sed -E "s/(<\w+?)( ?.*?)*?>/\1>/" | sort | uniq -c | sort -n | awk '{ print $1 }' | R --slave -e 'x<-scan(file="stdin", quiet=TRUE);min(x);max(x);mean(x);median(x);'
```

![image-20210706105943351](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210706105943351.png)

![image-20210706121812692](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210706121812692.png)