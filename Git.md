## Git

#### 1、初始化本地库

**git 	init：初始化本地库,在当前目录生成一个.git文件，该文件存放的是本地库相关的目录和文件（不要去轻易修改与删除）。**

#### 2、Git目录说明

工作目录：任意目录下，我们开发代码的目录

暂存区域：.git目录下，作用：有个后悔（返回撤销）的余地

本地仓库：.git目录下，Git存储项目的仓库

#### 3、签名

设置签名的作用：区分不同开发人员的身份

​	注意：为Git设置签名与远程库（代码托管中心）的账号密码没有任何关系	

​	设置签名命令： 

​	本地库级别设置签名方式：

------

​	**git config user.name zs**

​	**git config user.email zs@bjpowernode.com**

​	信息保存位置：**./.git/config** 文件

------

​	系统用户级别设置签名方式：

​	**git config --global user.name zs**

​	**git config --global user.email zs@bjpowernode.com**

​	**~/.gitconfig** 文件

​	优先级按照就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别

​	的签名

#### 4、git指令说明

![image-20210104114352480](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104114352480.png)

**git status:查看工作区/暂存区状态**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104120102236.png" alt="image-20210104120102236" style="zoom: 69%;" />

**git add xx：将文件放入暂存区**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104120243559.png" alt="image-20210104120243559" style="zoom: 80%;" />

**git rm  --cached  xx：将加入暂存区的文件撤回到工作区**

**gitcommit	[-m"提交信息" xx不用进入vim填写信息。]	xx：将文件提交到本地库（使用完后进入vim编辑器，填写提交的信息）；**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104121336817.png" alt="image-20210104121336817" style="zoom: 74%;" />

**如果修改本地库的文件则需要添加到缓存区，在提交才能完成一次修改，每次提交会生成一个提交历史版本。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104121742499.png" alt="image-20210104121742499" style="zoom: 80%;" />

#### 5、产看git提交历史

 **Git log：查看提交历史的详细信息。--pretty=oneline使用一行显示；--oneline只显示hash值的一部分；--graph以图表显示**

**commit:xxxxxx当前版本号（hash值）**

**HEAD指向当前版本的指针**

**之后是作者、提交时间与提交我们写的描述**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104122048328.png" alt="image-20210104122048328" style="zoom: 74%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104122436488.png" alt="image-20210104122436488" style="zoom: 84%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104122658695.png" alt="image-20210104122658695" style="zoom:84%;" />

**git reflog更加简洁的显示提交历史**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104123154226.png" alt="image-20210104123154226" style="zoom:88%;" />

#### 6、历史提交记录的回退与前进

（**推荐）基于索引：git reset --hard  索引值。**

**--hard：重置缓存区与工作区与本地库。**

**--mixed：重置本地库与暂存区。**

**--soft：仅仅只是本地库移动HEAD指针。**



**2.基于^（只能后退）：get reset --hard HEAD^(后退一步)。**

**3.基于~同2，只是~n后退了n个历史阶段，如get reset --hard HEAD~2（后退2步）。**

#### 7、找回删除文件

**前提是创建的文件已经提交到本地库！**

步骤：

1. 删除文件

2. 添加删除操作到暂存区

3. 如果不想删除，**get reset --hard HEAD**

   <img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104130231709.png" alt="image-20210104130231709" style="zoom: 70%;" />

4. 如果已经提交操作到本地库

5. 查看提交历史从之前的历史中找回删除的文件。

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104130400346.png" alt="image-20210104130400346" style="zoom: 80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104130455866.png" alt="image-20210104130455866" style="zoom: 83%;" />

#### 8、比较文件的内容

git diff xx：查看文件的变化，先修改在diff查看

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104130938735.png" alt="image-20210104130938735" style="zoom:87%;" />

git diff HEAD^ xx：比较上一个版本的文件变化。

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104130824252.png" alt="image-20210104130824252" style="zoom:80%;" />

#### 9、分支管理

**查看所有分支与当前所在的分支：git -branch -v。**

**创建一个分支：git branch 分支名。**

**切换分支：git checkout 分支名。**

------

**分支的合并。**

**修改分支中文件的内容。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104134451166.png" alt="image-20210104134451166" style="zoom:80%;" />

**合并分支。**

​	切换到主分支上

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104134634828.png" alt="image-20210104134634828" style="zoom:82%;" />

​	进行合并

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104135010137.png" alt="image-20210104135010137" style="zoom:82%;" />

**解决冲突**

主分支、次分支都修改同一个文件同一行后提交到本地库，并且主分支向合并次分支，会出现冲突。

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104140230705.png" alt="image-20210104140230705" style="zoom:83%;" />

**解决方法就是修改文件**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104140705543.png" alt="image-20210104140705543" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104140850395.png" alt="image-20210104140850395" style="zoom:74%;" />

**分支管理的本质：指针的移动**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104141435651.png" alt="image-20210104141435651" style="zoom:58%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104141549066.png" alt="image-20210104141549066" style="zoom:58%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104141712016.png" alt="image-20210104141712016" style="zoom:58%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104141823117.png" alt="image-20210104141823117" style="zoom:58%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104141933909.png" alt="image-20210104141933909" style="zoom:58%;" />

#### 10、Git&GitHub

**向远程仓库推送东西**

远程仓库先建一个Repositories

之后进入先建的仓库，有一个https

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104142425451.png" alt="image-20210104142425451"  />

在本地设置远程库地址别名：

**查看别名：git remote -v.**

**设置别名：git remote add 别名 https地址。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104142920514.png" alt="image-20210104142920514" style="zoom: 80%;" />

然后向远程仓库push xx

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104143407083.png" alt="image-20210104143407083" style="zoom: 88%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104143556353.png" alt="image-20210104143556353" style="zoom:50%;" />

推送完成



**使用SSH免密推送（推荐），SSH下克隆有说。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104150153675.png" alt="image-20210104150153675" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104150210342.png" alt="image-20210104150210342" style="zoom:80%;" />

------

**克隆远程库：**

**先建目录**

**git clone HTTPS地址**

**克隆完成的三件事：完整的下载本地库；创建远程地址的别名；初始化本地库；**

------

使用SSH克隆

**git clone SSH地址**

**获取SSH的key与查看是否连接成功（不用记，以后公司会给）**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104145035633.png" alt="image-20210104145035633" style="zoom: 80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104145205037.png" alt="image-20210104145205037" style="zoom: 52%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104145639991.png" alt="image-20210104145639991" style="zoom:80%;" />

**拉取仓库与多人协同解决冲突**

**git pull = git fetch +merge**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104155919771.png" alt="image-20210104155919771" style="zoom: 68%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104160030753.png" alt="image-20210104160030753" style="zoom:78%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104160227960.png" alt="image-20210104160227960" style="zoom:75%;" />

**在远程仓库已经修改了文件的内容，我们再次push会出现冲突。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104152307822.png" alt="image-20210104152307822" style="zoom: 80%;" />

**这时候我们需要将远程库的代码拉取在进行修改（谁push冲突谁修改）**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104152916553.png" alt="image-20210104152916553" style="zoom:86%;" />

**解决方式与上分支管理解决一样**

#### 11、拉别人进入开发

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104153628417.png" alt="image-20210104153628417" style="zoom: 50%;" />

之后收到邮件，点击进去确认就行了

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104153907736.png" alt="image-20210104153907736" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104154030223.png" alt="image-20210104154030223" style="zoom:80%;" />

#### 12、打标签

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104154710560.png" alt="image-20210104154710560" style="zoom: 67%;" />

#### 13、idea集成git

##### idea集成git

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104161042381.png" alt="image-20210104161042381" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104162014029.png" alt="image-20210104162014029" style="zoom: 58%;" />

##### idea中克隆

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104162610991.png" alt="image-20210104162610991" style="zoom:78%;" />

##### idea中查看git log与diff

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104163253447.png" alt="image-20210104163253447" style="zoom:43%;" />

##### idea使用git的常用命令

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104163559599.png" alt="image-20210104163559599" style="zoom: 90%;" />

##### idea项目中右键git选项也有常用命令

##### idea这边也有常用命令

![](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104163759735.png)

![image-20210104163846185](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104163846185.png)

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104164118853.png" alt="image-20210104164118853" style="zoom: 54%;" />

##### idea项目推送到远程仓库

##### 让我们的项目被git管理

之前通过命令初始化git的本地仓库，现在使用idea

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104190106763.png" alt="image-20210104190106763" style="zoom:80%;" />

**初始化本地库后，项目修改过的文件会变成红色，将项目添加到暂存区变为绿色，之后提交到本地库（与git命令行同理）。这里提交的话在idea中可以省略添加到暂存区，直接提交~~~**

##### **推送项目**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104191036948.png" alt="image-20210104191036948" style="zoom: 71%;" />

##### 分支操作

![image-20210104192244061](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104192244061.png)

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104192549519.png" alt="image-20210104192549519" style="zoom: 50%;" />

**在当前分支push之后会在远程仓库先建一个与本地库分支同名的分支。**

![image-20210104193024629](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104193024629.png)

**反之远程到本地，先建一个与远程一样的分支，pull=fetch+merge**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210104193425960.png" alt="image-20210104193425960" style="zoom:50%;" />

