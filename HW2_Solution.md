# HW2 Solution

## Author:金之航

## StuID:2183411101

## Problem

### 1.man ls

![image-20210524174304176](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524174304176.png)

(1) ls -a

-a, --all
              do not ignore entries starting with .

![image-20210524174640978](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524174640978.png)

(2) ls -alh

 -h, --human-readable
              with -l, print sizes in human readable format (e.g., 1K 234M 2G)

![image-20210524174705749](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524174705749.png)

(3) ls -alht

 -t           sort by modification time, newest first

![image-20210524175137787](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524175137787.png)

(4)  ls -alht --color

--color[=WHEN]
              colorize the output; WHEN can be 'never', 'auto', or 'always' (the default); more  info
              below

![image-20210524175339461](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524175339461.png)



### 2.marco.sh:

```shell
#!/bin/bash

macro () {                            
    current="$(pwd)"                  #pwd - print name of current/working directory
    echo "$current saved to cache"    #echo - display a line of text
}

polo () {
    cd "$current"                     #goto directory saved by marco
    echo "jump to $current"           #echo - display a line of text
}
```

![image-20210524223841739](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524223841739.png)



### 3.capture.sh

```shell
#!/bin/bash

count=0
while true; do                                           
    n=$((RANDOM % 100))

    if [[ n -eq 42 ]]; then                               
    #if n = 42 then print "Something went wrong" and "The error was using magic numbers"
        echo "Something went wrong"
        echo >&2 "The error was using magic numbers"
    #print "Program successfully ran $count times\n"
        echo "Program successfully ran $count times\n"
    
    #Exit code 0        Success
    #Exit code 1        General errors, Miscellaneous errors
    #Exit code 2        Misuse of shell builtins
        exit 1
    fi
    
    #each time ran successfully count++ and print "Everything went according to plan"
    ((count += 1))
    echo "Everything went according to plan"
done
```



![image-20210524231344905](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524231344905.png)



### 4.a single command

```shell
find *.html | xargs --delimiter="\n" zip test.zip
#find file end with ".html"
#delimiter = "\n" actually " "
#zip name = test.zip
```

![image-20210524234631935](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210524234631935.png)



### 5.myfile.sh

```shell
#!/bin/bash

head=10                    
#defaul input head is 10

if [ $# -lt 1 ]
then
    echo "No Args Input..."
    exit ;
fi
#if input args is less than 1, then print "No Args Input...""

if [ $# -eq 2 ]
then
    let head=$2
fi
#if input args equal 1, then head = the second input

case $1 in 
"access")
echo last access $head:
find . -type f -print0 | xargs -0 stat --format '%x :%n' | sort -nr | head -n$head
;;
"modify")
echo last modify:
find . -type f -print0 | xargs -0 stat --format '%y :%n' | sort -nr | head -n$head
;;
"change")
echo last change:
find . -type f -print0 | xargs -0 stat --format '%z :%n' | sort -nr | head -n$head
;;
*)
echo "Input args: access|modify|change"
;;
esac
#       %w     time of file birth, human-readable; - if unknown
#       %W     time of file birth, seconds since Epoch; 0 if unknown
#       %x     time of last access, human-readable
#       %X     time of last access, seconds since Epoch
#       %y     time of last modification, human-readable
#       %Y     time of last modification, seconds since Epoch
#       %z     time of last change, human-readable
#       %Z     time of last change, seconds since Epoch
#       %n     file name
#       STEP: 1.find local files  2.using xargs and stat reformat  3.sort -n --numeric-sort compare according to string numerical value,  -r, --reverse reverse the result of comparisons 4.head -n  -n, --lines=[-]K print the first K lines instead of the first 10; with the leading '-',  print  all  but the last K lines of each file
```

![image-20210525204146258](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210525204146258.png)