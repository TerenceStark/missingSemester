# jHW5 Solution

## Author:金之航

## StuID:2183411101



## Problem

### 1.Job Control

<img src="C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710211039212.png" alt="image-20210710211039212" style="zoom: 80%;" />

-Terminal Multiplexes
-Dot files
-Remote Machines

```shell
sleep 2000
nohup sleep 2000 &
jobs
bg %1 #fg?
kill -STOP %
jobs
kill -HUP %1
kill -HUP %2
kill -KILL %2 #note
```

```shell
sleep 2000
bg sleep
pgrep sleep
```

![image-20210710222623776](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710222623776.png)



```shell
pkill  -ef sleep

-a  Include process ancestors in the match list.  By default, the current pgrep or pkill process and all of its ancestors are excluded (unless -v is used). #默认就是-a所以不用再写
-f  Match against full argument lists. The default is to match against process names.
```

![image-20210710222844132](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710222844132.png)

```shell
sleep 60 | wait && ls
```

![image-20210710223242851](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710223242851.png)

```shell
sleep 6 & pidwait $(pgrep -n sleep)

#pidwait:
#!/bin/bash
while kill -0 $1 
do
echo "$1 is still running"
sleep 1
done
echo "$1 finished"
ls
```

![image-20210710233150988](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710233150988.png)





### 2.Mux

-Sessions
-Windows
-Panes

```shell
tmux new -t foobar
<C-b> N Go to the N th window. Note they are numbered
<C-b> p Goes to the previous window
<C-b> n Goes to the next window
<C-b> , Rename the current window
<C-b> w List current windows
<C-b> " Split the current pane horizontally
<C-b> % Split the current pane vertically
<C-b> <direction> Move to the pane in the specified direction. Direction here means arrow keys.
<C-b> z Toggle zoom for the current pane
<C-b> [ Start scrollback. You can then press <space> to start a selection and <enter> to copy that selection.
<C-b> <space> Cycle through pane arrangements.
```

![image-20210710221334679](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710221334679.png)



### 3.Aliases

wrong typing dc or ls

```shell
alias cd=dc
alias cd="rm -rf"
alias ls=sl
```

![image-20210710234425912](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710234425912.png)

history sort

```shell
bin history | awk '{$1="";print substr($0,1)}' | sort | uniq -c | sort -nr | head -n 10
别名过程省略了，具体就是alias a=b, 有空格的需要用()
```

![image-20210710235258201](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210710235258201.png)



### 4.Dotfiles

Some other examples of tools that can be configured through dotfiles are:

```shell
bash - ~/.bashrc, ~/.bash_profile
zsh - ~/.zshrc, ~/.bash_profile
git - ~/.gitconfig
vim - ~/.vimrc and ~/.vimrc and the ~/.vim folder
ssh - ~/.ssh/config
tmux - ~/.tmux.conf
```



### 5.Portability

I have done ssh(secure shell) already.

```shell
#typing follow cmd on hadoop101
ssh hadoop102
ssh hadoop101
```

![image-20210711002110691](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210711002110691.png)

```shell
python -m SimpleHTTPServer 8888
curl localhost:9999
```

![image-20210711002358894](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210711002358894.png)



### ssh config

```
cat known hosts #查看已经受信任的主机，以及他们的密钥
```

![image-20210711003543231](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210711003543231.png)

可以进行文件的分发：

```shell
#!/bin/bash
#1 获取输入参数个数，如果没有参数，直接退出
pcount=$#
if [ $pcount -lt 1 ]
then
    echo Not Enough Arguement!
    exit;
fi

#2. 遍历集群所有机器
# 也可以采用：
# for host in hadoop{101..103};
for host in hadoop101 hadoop102 hadoop103
do
    echo ====================    $host    ====================
    #3. 遍历所有目录，挨个发送
    for file in $@
    do
        #4 判断文件是否存在
        if [ -e $file ]
        then
            #5. 获取父目录
            pdir=$(cd -P $(dirname $file); pwd)
            echo pdir=$pdir
            
            #6. 获取当前文件的名称
            fname=$(basename $file)
            echo fname=$fname
            
            #7. 通过ssh执行命令：在$host主机上递归创建文件夹（如果存在该文件夹）
            ssh $host "mkdir -p $pdir"
            
			#8. 远程同步文件至$host主机的$USER用户的$pdir文件夹下
            rsync -av $pdir/$fname $USER@$host:$pdir
        else
            echo $file does not exists!
        fi
    done
done

xsync install.sh
```

![image-20210711003136156](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210711003136156.png)



### 6.Mosh

```shell
Mosh表示移动Shell(Mobile Shell)，是一个用于从客户端跨互联网连接远程服务器的命令行工具。它能用于SSH连接，但是比Secure Shell功能更多。
yum install mosh
```



### 7.SSh -N -f

```shell
-N      Do not execute a remote command.  This is useful for just forwarding ports.
-f      Requests ssh to go to background just before command execution.  This is useful if ssh is going to ask for passwords or passphrases, but the user wants it in the background.  This implies -n.  The recommended way to start X11 programs at a remote site is with something like ssh -f host xterm.

If the ExitOnForwardFailure configuration option is set to ``yes'', then a client started with -f will wait for all remote port forwards to be successfully established before placing itself in the background.

ssh -fN -L 9999:localhost:8888 hadoop102
```

