#### 正则表达式

##### 基础正则

###### 1）^ 以...**开头的行**

```shell	
grep '^mv' oldboy.txt
```

###### 2) $ 以....结尾的行

```shell
grep '48$' oldboy.txt 
grep '48' oldboy.txt #包含48的所有行
```

###### 3）^$ 空行

- 空行这一行中没有任何内容（空格也符合）

  ```shell
   grep '^$' test.txt
   
   grep -n '^$' test.txt
  ```

  ![image-20211004110809223](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211004110809223.png)

- 企业应用案例

  ​	排除文件中的空行

  ```shell
  grep -v '^$' test.txt
  ```

###### 4) . 表示任意一个字符

```sh
grep '.' text.txt
```

![image-20211004111336562](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211004111336562.png)

###### 5) \ 转义字符 

- 匹配出以.结尾的所有行

```sh
grep '\.$' test.txt
```

###### 6) * 前一个字符出现0次或0次以上

 ```sh
 grep '0*' test.txt #0出现一次以上的行匹配出来
 ```

###### 7） .* 所有内容

```sh
grep '^.*t' test.txt # 以t开头的字符
```

![image-20211004114909051](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211004114909051.png)

###### 8）[] [abc] 1次匹配一个字符，匹配任何一个字符（a或b或c）

- [a-z]

- [A-Z]

- [1-9]

- 匹配文件中的大小写字母和数字

  ```sh
  grep '[a-zA-Z0-9]' test.txt
  grep '[a-Z0-9]' test.txt
  grep -i '[a-z0-9]' test.txt #-i 不区分大小写
  ```

  

```sh
grep '[abc]' test.txt

grep -o '[abc]' test.txt #把匹配过程打印出来
```

![image-20211005102944659](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211005102944659.png)

注意：[a-z|A-Z|0-9] 匹配的是大小写字母、数字和|

###### 9）[^]  取反

```sh
grep '[^abc]' test.txt #排除a或b或c之外的内容
```



##### 扩展正则

###### + 前一个字符连续出现1次或者1次以上

```sh
egrep '0+' test.txt
grep -E '0+' test.txt #扩展正则用egrep或者grep -E
```

- 匹配中文件中连续的数字

```sh
egrep '[0-9]+' test.txt
```

- 匹配文件中连续的字母

```sh
egrep '[a-Z]+' test.txt
```

###### | 或者

[] 与 | 区别：

[]：一次匹配一个字符  比如[abc] 匹配a或b或c 

|：一次匹配一个字符或者多个  a|b|c 或 ab|ac   可以匹配单词

###### () 

- 被括起来的内容，表示一个整体

  ```sh
  egrep 'oldb(o|e)y' test.txt
  ```

  ![image-20211005111037807](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211005111037807.png)

- 后向引用，用于sed命令

###### {} 连续出现

o{n,m} 前一个字母o至少出现n次，最多连续出现m次

o{n} 前一个字母o正好出现了n次

o{n,} 前一个字母至少出现n次

o{,m} 前一个字母最多出现m次

###### ？连续出现  前一个字符出现0次或者1次

```sh
egrep 'go?d' test.txt
```

![image-20211005111712349](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211005111712349.png)

#### 文件处理三剑客

##### 1. 三剑客特点及应用场景

| 命令 | 特点                     | 场景                                       |
| ---- | ------------------------ | ------------------------------------------ |
| grep | 过滤                     | 过滤速度是最快的                           |
| sed  | 替换，修改文件命令，取行 | 如果替换，修改文件命令，取某个范围的内容   |
| awk  | 取列，统计计算           | 取列 <br />对比比较 >= <= !=<br />统计计算 |

##### 2. grep	

| 选项 | 含义                                        |
| ---- | ------------------------------------------- |
| -E   | egrep 支持扩展正则                          |
| -A   | 了解，-A5 匹配你要的内容并且显示接下来的5行 |
| -B   | -B5 匹配你要的内容并且显示上面的5行         |
| -C   | context 上下-C5匹配你要的内容上下5行        |
| -c   | 统计出现了多少行                            |
| -v   | 取反                                        |
| -n   | 显示行号                                    |
| -i   | 忽略大小写                                  |
| -w   | 精确匹配                                    |

##### 3. sed

- 格式

  | 命令 | 选项 | sed功能              | 参数     |
  | ---- | ---- | -------------------- | -------- |
  | sed  | -r   | ’s#oldboy#oldgirl#g‘ | test.txt |

- sed 命令核心功能，增删改查

  | 功能 |           |
  | ---- | --------- |
  | s    | 替换      |
  | p    | 显示      |
  | d    | 删除      |
  | cai  | 增加c/a/i |

  ###### sed核心应用

  1）sed 查找p  通常和-n搭配

  | 查找格式           |                                                  |
  | ------------------ | ------------------------------------------------ |
  | '1p' '2p'          | 指定行号查找                                     |
  | ’1,5p‘             | 指定行号范围查找                                 |
  | ’/lisi/p‘          | 类似于grep过滤，//里面可以写正则                 |
  | ’/10.30/,/11.30/p‘ | 表示范围的过滤，如果结尾找不到就会显示到文件结尾 |
  | ’4,$p'             | 第4行到最后一行                                  |
  | 1,/oldboy/         | 从第一行到oldboy的行                             |

  2）过滤

  ```sh
  sed -n '/10/p' test.txt #将包含10的行显示出来
  sed -n '/[0-9]/p' test.txt #包含0-9数字的行
  sed -nr '/[0-9]{3}/' test.txt #包含0-9数字出现了3次的行
  ```

  

  **删除** d

  ```sh
  sed '1d' test.txt #删除第三行	
  sed '2,3d' test.txt #删除第二、三行
  ```

  删除文件中的空行和包含#号的行

  ```sh
  egrep -v '^$|#' test.txt
  sed -r '/^$|#/d' test.txt
  
  # !的作用
  ! 表示取反
  sed -nr '/^$|#/!p' test.txt #遇到空行和#的行不删除
  ```

  **增加** cai

  | 命令 |                                                |
  | ---- | ---------------------------------------------- |
  | c    | replace 替代这行的内容                         |
  | a    | append追加，向指定的行或每一行追加内容(行后面) |
  | i    | insert，向指定的行或每一行插入内容(行前面)     |

  ```sh
  sed '3a lisi996' test.txt #在第三行的后面加上内容
  ```

  ![image-20211005212418581](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211005212418581.png)

```sh
sed '3i lisi996' test.txt ##在第三行的前面加上内容
```

![image-20211005212534990](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211005212534990.png)

**替换** s

- s -->sub substitute

- g -->global 全局替换，sed替换每行所有匹配的内容，不写的话默认替换每行第一个匹配的内容

  | 替换格式    |
  | ----------- |
  | s#aaa#bbb#g |
  | s/aaa/bbb/g |

**反向引用**

```sh
echo 123546 | sed -r 's#(.*)#</1>#g' #括号是把前面123456给当成一个整体，\1表示第一个整体，注意是反斜杠，需要加-r，因为小括号是扩展正则
```

![image-20211006111148982](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211006111148982.png)

```sh
echo zhangsan_lisi | sed -r 's#(^.*)_(.*$)#\2_\1#g' #把两个名字调换，(^.*)表示下划线前的名字，(.*$)表示下划线后面的名字，\2表示第二组,\1表示第一组
```

```sh
ip a s eth0 | sed -n '3p' | sed -r 's#^.*t (.*)/.*#\1#g'  #获取ip地址，^.*t 这个t后面有个空格，是为了匹配intet 这个，(.*)表示ip地址，/.*表示/后面所有的字符

# 精简的写法
ip a s eth0 | sed -rn '3s#^.*t (.*)/.*#\1#gp'
```

![image-20211006112750761](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211006112750761.png)



##### awk

过滤、统计、计算

###### awk里的行与列

| 名词 | awk中的叫法     | 说明                   |
| ---- | --------------- | ---------------------- |
| 行   | 记录record      | 每一行默认通过回车分割 |
| 列   | 字段、域、field | 每一列默认通过空格分割 |

###### 1）取行

awk的内置变量

| 内置变量 | 含义                                                         |      |
| -------- | ------------------------------------------------------------ | ---- |
| NR       | 记录行号                                                     |      |
| NF       | 每行有多少列  $NF表示最后一列                                |      |
| FS       | -F: 相当于 -v FS=:  #Field Separator 字段分隔符，每个字段结束标记 |      |
| OFS      | Output Field Separator 输出字段分隔符（awk显示每一列的时候，每一列用什么分割，默认是空格） |      |

| awk          |                    |      |
| ------------ | ------------------ | ---- |
| NR==1        | 取某一行           |      |
| NR>=1&&NR<=5 | 取出一到五行       |      |
| /zhangsan/   | 取包含zhangsan的行 |      |
| /101/,/105/  |                    |      |
| 符号         | > < >= <= == !=    |      |

```sh
awk 'NR==1' test.txt
awk 'NR>=1 && NR<=5' test.txt
```

###### 2)取列

- -F 指定分隔符，指定每一列结束标记（默认是空格，连续的空格 ，tab键）

- $数字  $1表示第一列 

  ```sh
  awk '{print$1}' test.txt #取第一列
  awk 'NR==2{print$2}' test.txt #第二行第2列
  ```

- $0 表示整行的内容

```sh
head -3 /etc/passwd | awk -F: '{print $1,$NF}' |column -t #-F: 表示用：作为分隔符；$1,$NF:第一列和最后一列；column -t显示的好看一点
```

![image-20211006171510621](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211006171510621.png)

```sh
head -3 /etc/passwd | awk -F: '{print $1"@@"$NF}' |column -t #中间用@@分割
```

![image-20211006171716895](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211006171716895.png)



获取ip地址

```sh
 ip a s eth0 | awk -F"[ /]+" 'NR==3{print $3}' #“[ /]+” 是以空格和/进行分割，对于第三行进行分隔，取分隔后的第三列
```

![image-20211007104604423](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007104604423.png)

###### awk 的模式匹配

| awk  | -F"[ /]+" | 'NR==3{print $3}' |
| ---- | --------- | ----------------- |
| 命令 | 选项      | ‘条件(动作)’      |
|      |           |                   |

谁可以作为awk的条件

- 比较符号
- 正则
- 范围表达式//，//
- 特殊条件：BEGIN和END

1）比较符号，参考取行

2）正则：

- 支持扩展正则
- awk可以精确到某一列，某一列中包含/不包含....的内容   ~包含  !~ 不包含

| 正则                 | awk正则                                     |
| -------------------- | ------------------------------------------- |
| ^ 表示以...开头的行  | 某一列的开头 $3~/^lisi/ #第三列是否包含lisi |
| $ 表示以....结尾的列 | 某一列的结尾   $3~/lisi$/                   |
| ^$ 表示空行          | 某一列是空的                                |

找出etc/passwd里第3列以2开头的行，并显示第一列、第三列和最后一列

```sh
head -5 /etc/passwd | awk -F: '$3~/^1/{print $1,$3,$NF}'
```

![image-20211007111746785](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007111746785.png)

找出etc/passwd里第3列以1或者2开头的行，并显示第一列、第三列和最后一列

```sh
head -5 /etc/passwd | awk -F: '$3~/^[12]/{print $1,$3,$NF}'
```

![image-20211007111941991](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007111941991.png)

3） 表示范围

- //，//   #常用
- NR==1,NR==5 #从第一行开始到第五行结束

4）特殊模式 BEGIN{} 和 END{}

| 模式    | 含义                              | 应用场景                                                     |
| ------- | --------------------------------- | ------------------------------------------------------------ |
| BEGIN{} | 里面的内容会在awk读取文件之前执行 | 1）进行简单统计，计算，不涉及读取文件（常见）<br />2）用来处理文件之前，添加表头<br />3）用来定义awk变量 |
| END{}   | 里面的内容会在awk读取文件之后执行 | 1）awk进行统计，先进行计算，最后END里面输出结果<br />2）awk使用数组，用来输出数组结果 |

- END{}统计计算

- 统计方法：

  | 统计方法            | 简写形式  | 应用场景                         |
  | ------------------- | --------- | -------------------------------- |
  | i=i+1               | i++       | 计数                             |
  | sum = sum+？        | sum+=？   | 求和                             |
  | array[] = array[]+1 | array[]++ | 数组分类计数，要统计什么，[]里面 |

  统计/etc/services 有多少空行

  ```sh
   awk '/^$/{i++}END{print i}' /etc/services # /^$/ 表示空行；{i++}统计 END{...} 将变量打印
  ```

  ![image-20211007114259143](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007114259143.png)

  从1+到100

  ```sh
  seq 100 | awk '{sum+=$0}END{print sum}'
  #如果想把加的过程打印出来
  seq 100 | awk '{sum+=$0;print sum}END{print sum}'
  ```

  ![image-20211007114634357](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007114634357.png)

  ![image-20211007114757166](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007114757166.png)

###### awk数组 

- 统计日志
- 统计次数
- 累加求和

|                    | shell数组                              | awk数组                          |
| ------------------ | -------------------------------------- | -------------------------------- |
| 形式               | array[0]=oldboy array[1]=lidao         | array[0]=oldboy array[1]=lidao   |
| 使用               | echo $(array[0]) $(array[0])           | print array[0] array[1]          |
| 批量输出数组的内容 | for i in $(array[*])<br />do<br />done | for(i in array)<br />    print i |



```sh
 awk 'BEGIN{a[0]=123;a[1]="lidao";print a[0],a[1]} ' #注意都写在BEGIN{}内部,字符串需要加双引号进行判断
```

![image-20211007172552047](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007172552047.png)

```sh
awk 'BEGIN{a[0]=123;a[1]="lidao";for (i in a) print i} ' #打印的是下标
 awk 'BEGIN{a[0]=123;a[1]="lidao";for (i in a) print a[i]} ' #打印的是数组内容
```

![image-20211007173919509](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211007173919509.png)

###### for循环

| shell for循环                                            | awk for循环                      |                             |
| -------------------------------------------------------- | -------------------------------- | --------------------------- |
| for ((i=1;i<=10;i++))<br />do<br />    echo $i<br />done | for (i=1;i<=10;i++)<br />print i | awk for循环用来遍历每个字段 |
|                                                          |                                  |                             |
|                                                          |                                  |                             |
|                                                          |                                  |                             |
|                                                          |                                  |                             |

```sh
awk 'BEGIN{
for(i=1;i<=100;i++)
sum+=i;
print sum}' #注意sum+=i后面是有个分号
```

![image-20211017102244481](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211017102244481.png)

###### if判断

| shell if判断                                                 | awk if判断                            |      |
| ------------------------------------------------------------ | ------------------------------------- | ---- |
| if ["lisi" -eq 18] then <br />echo ' xxx'<br />fi            | if(条件)<br />print xxx               |      |
| if ["lisi" -eq 18] then <br />echo 'xxx'<br />else<br />    echo 'xxx' | if ()<br />print<br />else<br />print |      |

```sh
df -h | awk -F "[ %]+" 'NR>1{if($5>=50) print "disk not enough"} ' #按分隔符分割后，磁盘空间大于50的打印出来，需要将第一行去掉，所以加上NR>1
```

![image-20211017105257463](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211017105257463.png)

```sh
#统计这段语句中，字符数小于6的单词
echo i am oldboy teacher welcome to oldboy training class
#先打印出每个单词
 echo i am oldboy teacher welcome to oldboy training class | awk 
'{for(i=1;i<=NF;i++)print $i}'
#再统计
echo i am oldboy teacher welcome to oldboy training class | awk '{for(i=1;i<=NF;i++) if(length($i)<6)print $i}'#使用length进行统计长度

```

 ![image-20211017105705026](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211017105705026.png)

![image-20211017105845284](C:\Users\15929\AppData\Roaming\Typora\typora-user-images\image-20211017105845284.png)