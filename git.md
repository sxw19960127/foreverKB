# Git

分布式版本控制系统。

## 历史

```
Git创始人 linux
linux一开始采用bitkeeper充当分布式版本控制系统,进行代码的合并。有一天linux社区有人黑了bitkeeper之后便没有了其使用权,于是linux花了10天写出了git。
git 是一个分布式的版本控制系统;
版本控制系统是一个工具,用来保存我们各个阶段的代码;
我们每一个阶段提交的代码就是一个版本,版本控制系统可以随时切换任意一个版本,去读取里面的内容;
好处: 
1.可以让我们更加清晰直观的去发现我们的代码是怎么从无到有去写的;
如果我们把每一个版本的代码提交的都非常好,有利于自己日后阅读或者是方便别人来阅读;
2.可以进行代码的回滚;
当我们满足需求去实现一个功能的时候,测试时没有发现问题,但在上线的时候发现了问题,这个时候我们需要把我们的代码回滚到最近的一个没有问题的版本,让他回到当初没有bug的时候,再在本地进行bug的修复;
版本控制系统: 1.本地版本控制系统;2.集中式版本控制系统;3.分布式版本控制系统;
	本地版本控制系统: 本地存着所有的版本,缺点(没有备份,不利于协作开发与合并);
	集中式版本控制系统(svn): 中央服务器中存着所有的版本,缺点(必须联网才能工作,从服务器拉取数据的时候,需要有网络,以及提交数据的时候也需要网络,如果中央服务器崩溃了,那我们就无法去存取数据了)
	分布式版本控制系统: 完整的版本不仅存在于中央服务器上,还存在于每一个人的电脑上,我们只需要完成我们的操作,在本地完成我们的需求,到最后整合的时候往服务器上去提交,服务器只充当一个中转的作用,充当一个备份的作用,和集中式版本控制系统的中央服务器相比,降低了服务器的重要性,当服务器崩溃的时候,并不会影响我们电脑里面的版本库;还有一个好处是可以自动把我们的代码进行合并;
		git诞生记 linux bitkeeper,linux社区有人黑了bitkeeper,所以bitkeeper收回使用权,然后linux用了10天写出git
```

## 优点

```
版本控制系统是一个工具,用以保存我们每个阶段的代码,可以切换读取各个阶段所提交的代码。好处:
	1.便于自己或者他人去看我们的代码是如何从无到有实现的;
	2.代码回滚;
	(当为项目添加一个新功能,我们完成了需求并且在本地测试是没有问题的,但提交上线时出现了bug。这时我们需要做两步操作:1.修复bug;2.代码回滚。将线上的代码回滚到最近一个没有bug的版本)
```

## 版本控制系统类别

```
版本控制系统:
	1.本地版本控制系统; 
	2.集中式版本控制系统; 
	3.分布式版本控制系统;
本地版本控制系统:
	1.没有备份,所有阶段代码和版本都保存在自己的电脑里;
	2.对协作开发不友好;
集中式版本控制系统:
	1.必须要联网才能进行工作;
	2.如果中央服务器崩溃了,则每个人都是无法工作的;
分布式版本控制系统:
	1.可以自动的进行代码合并;
	2.版本库存在于中央服务器中以及每个人的电脑中;
	3.对协作开发非常友好;
	4.中央服务器只充当中转的作用,只有当最终版本确定以后才会上传到中央服务器;
```

## 起步

```
安装git:
	1.官网 https://git-scm.com
配置git:
	1.用户信息配置: 
		git config --global user.name shuxiaowei //配置用户名
		git config --global user.email 1183476700@qq.com //配置邮箱
检查配置是否成功: 
	1.git config --global user.name //回车查看用户名是否配置成功
	2.git config --global user.email //回车查看邮箱是否配置成功
	3.git config --list //回车查看所有配置项信息
获取Git仓库:
	1.从现有目录中初始化,让当前项目被git仓库所管理: git init
	2.将远程的仓库拉下来,克隆现有的仓库: git clone url
	(复制存在于服务器端的仓库链接地址;打开终端,cd Desktop/;git clone url;实现克隆仓库到本地)
```

## 流程

```
先在工作目录中敲代码;
暂时保存于暂存区域;(暂存区域是一个虚拟的可以暂时保存各个阶段代码的地方)
将完整版的代码提交到git仓库; 
三个工作区域: 工作目录,暂存区域,Git仓库(各个阶段的版本在git仓库里面是以快照的形式来存储的)
快照就是记录各个阶段的代码情况,里面的信息包含了作者,还有一个重要的hash值是一个编号,我们可以根据这个编号进行版本的切换,git仓库里面存的都是各式各样的快照;
```

## 快照

```
git仓库存放各个阶段的代码是以快照的形式储存的;
快照里面包含了提交者的信息,方便定位到是谁提交了这份代码;
可以给快照附带描述语句,关于自己在这个阶段做了什么事情;
快照还有一个编号如:98ca9/34ac2/f30ab,我们可以根据编号进行阶段代码的切换;
```

## Git更新状态命令

```
追踪文件与暂存: git add (从工作目录到暂存区域)
提交更新: git commit (从暂存区域到Git仓库)
查看状态: git status
查看修改: git diff
```

```shell
git init 初始化
git add index.html 把index.html文件保存到暂存区域
git add . 把所有修改过的操作下的文件全部都保存到暂存区域
git commit -m "第一次提交" 将第一阶段的版本提交到git仓库
```

## 例子

```
当我们在工作目录中完成了html,并且需要将他保存到暂存区域的时候: git add index.html
当我们修改了html之后再次进行保存: git add index.html
当我们即修改了html,又修改了css,需要对所有文件进行保存到暂存区域: git add .
一个版本的代码完成了,我们需要对其进行照相: git commit -m "第一次提交"
```

## 文件的状态

```shell
使用git status可以查看当前工作目录中所有文件的状态
1.没有追踪的状态 Untracked;
2.没有修改的状态 Unmodified;
3.修改过的状态 modified;
4.暂存的状态 Staged;
```

```
没有追踪的状态;
没有修改的状态;
修改过的状态;
暂存的状态;
```

## git基本指令

```
git diff有三个不同的操作:
比较工作目录和暂存区域之间有什么不同;
比较工作目录和Git仓库之间有什么不同;
比较暂存区域与Git仓库之间有什么不同;
当我们就输入一个git diff的时候默认比较的是工作目录和暂存区域
git diff --staged 比较的是暂存区域和上一个版本之间有什么不同
git diff HEAD 比较当前工作目录和上一个版本有什么不同
```

```
git status指令,会告诉我们哪些文件是新建了没有被追踪的,哪些文件是被修改了的,只要当前文件与上一个版本有任何的修改都会被git status出来;
git diff HEAD指令,可以比较工作目录与上一个版本git仓库有何不同;
git diff --staged指令,可以比较暂存区域与上一个版本git仓库有何不同;
git diff指令,默认比较工作目录与暂存区域有何不同;
Git查看提交历史:
	git log指令,会显示提交历史信息,方便我们去查看项目是怎么一步步写成的;
Git撤销:
	1.撤销,恢复到暂存时刻的代码: git checkout -- index.html
	2.撤销暂存,恢复到上一个版本的代码: git reset HEAD index.html 
	(怎么撤销回上一个暂存区域/怎么撤销回上一个版本)
```

```
Git分支: "指针"
大部分程序的主分支都是master,即master指向当前最稳定的版本;
HEAD指针就是告诉Git,我想在当前哪一个平行世界里玩;
git commit -a -m "第一次提交";(所有文件都在被追踪的情况下,省略git add . 而直接提交成一个版本);
git branch/git status/git log (查看现在在哪个分支上)
注意: 工作中使用分支,我们不要在主分支上修改代码;
git merge issus1;(在master分支上敲击命令,用以将issus1分支融合到master分支上)
```

```
Git查看提交历史
git log
Git撤销
撤销到暂存 git checkout -- index.html
撤销暂存 git reset HEAD <file>
```

## 流程

```shell
git init //初始化
git add .
git commit -m "稳定版本" //提交了master稳定版本
git branch lunbotu //创建新分支,用以开发轮播图功能
git checkout lunbotu //切换到轮播图分支
git add .
git commit -m "写一半的轮播图" 
git checkout master //master主分支上发现紧急bug
git branch issus1 //创建另外分支issus1用以解决紧急bug
git chekout issus1 //切换到分支issus1
git add .
git commit -m "修复bug"
git checkout master //回到主分支master,准备融合
git merge issus1 //将issus1融合进主分支master分支,这是会报融合错误,需解决
git checkout lunbotu //分支切换回轮播图
git add .
git commit -m "轮播图提交了"
git checkout master
git merge lunbotu //将写好的轮播图融合进去
git add .
git commit -m "最稳定版本"
```

```shell
Git分支: 指针
默认分支master,指向当前最新的一个版本
创建分支 git branch testing
HEAD指针
有每一期的不同版本,有两个分支(平行世界,针对版本的两个处理操作)分别是master和testing,HEAD指针表示想要在哪个分支(平行世界)去继续我的操作,默认情况下是HEAD指向master,因为我们默认的主分支是master
分支切换 git checkout<名字>
git branch -d index.html 这个指令好像是删除文件的指令
git init 创建空的项目
git add .把所有修改过的操作下的文件全部都保存到暂存区域
git commit -m "第一次" 将第一阶段的版本提交到git仓库
git add .把所有修改过的操作下的文件全部都保存到暂存区域
git commit -m "第二次提交" 将第二阶段的版本提交到git仓库
git commit -a -m "第三次提交" //直接把当前工作目录存储到一个版本,在确认已经修改到最终提交版本的时候可以使用这个指令,不需要git add .,注意在使用这个命令的时候,需要是所有文件都处于被追踪状态,当有未追踪的文件的时候必须先git add.再进行后续的操作
git log //显示我们一共进行了哪些操作
git status //查看状态
git branch dev //创建一个新的分支,但是当我们使用git status查看指针指向的时候发现依旧是指向master分支
git checkout dev //切换分支,改变指针指向
查看此时处于哪个分支上可以使用 git status / git branch指令
git add .
git commit -m "dev分支提交"
git checkout master //此时从dev分支切换到master分支上以后,在dev分支上建立的东西就消失了
工作中使用分支,不要在主分支上修改代码
git merge issus1 //当我们目前处于mster分支上的时候,使用前面的命令即可将我们修复完成的issus1版本融入到稳定版本master上面去
```

```
主题思路:目前有一个稳定版本,我们新建一个lunbotu分支进行我们轮播图的编写,然后发现稳定版本有一个bug,此时我们需要在创建一个分支名为issus1,相当于将稳定版本拿过来进行修复bug,然后使用命令git mergr issus1,将修复完成的issus1分支于稳定版本master进行整合成最新的一个无bug的稳定版本,完事之后再使用命令git checkout lunbotu,回到我们轮播图的界面继续完成我们的后续操作;
master主分支上开辟一个分支去做轮播图,此时发现主分支上有bug,我们回到主分支,再创建一个分支去解决bug,再回到主分支master,使用命令将解决完成的分支融入到稳定版本上,再回到轮播图的分支继续进行工作,最后回到msater主分支,将轮播图分支融入进来,只不过这个时候会出现相互融合办错的情况,我们按需处理一下就行了
远程仓库的使用
```

## GitHub的创建与使用

```shell
git remote //用以查看当前本地仓库有于任一远程仓库有关联吗
git remote add origin http:...	//将本地仓库与远程仓库相连接并且给远程仓库取了小名origin
git remote	//用以检验关联是否成功
本地仓库和远程仓库可以是一对多的关系,我们可以将本地仓库与github,码云关联在一起都是可以的;
git remotw -v	//用以查看当前本地仓库与哪些远程仓库建立了链接关系;
git push -u origin master	//推送到远程仓库的主分支master上,-u表示我最近一段时间都是往这个远程仓库推送的,下面再推送的话,直接git push就行了,如果要更换远程仓库,git push github master;
与远程仓库建立链接到底做了一件什么事情,git branch可以查看本地的分支,git branch -va会把所有的分支都给列出来,会发现远程仓库的一个镜像被保存在了本地;
```

```shell
git clone http://... //克隆远程仓库的地址到本地
git log //用以查看版本
git fetch origin //当我们向远程仓库提交代码的时候,如果当前本地的版本落后于远程仓库的版本是无法进行提交的,报错,所以我们需要先将远程仓库的代码全部拉取到本地;
git branch -va	//检查一下是否有添加新的分支到本地仓库;
git merge origin/master	//将本地仓库与远程仓库进行合并;
git add .
git commit -m "融合了轮播图和导航栏"
git push
```

```shell
:q 推出
在多人开发中我们都是先拉取代码再进行推送代码,将最新版本的远程仓库上的代码拉取下来与自己要提交的版本进行融合,再向远程仓库进行push操作;
git remote
git remote add github https://...
git remote
```

```shell
以本地仓库为基准
以远程仓库为基准
指令git remote用以查看本地仓库与远程仓库之间的联系
git remote add origin http://当前远程仓库的URL地址 (表示将当前本地仓库与远程仓库建立一个联系,因为远程仓库名字太长了,我们给他取了一个小名origin)
git remote -v 查看所有的远程仓库都列举出来
有两个操作:
1.fetch 从远程仓库进行拉取代码
2.push 向远程仓库提交代码
git push origin master (向小名为origin的仓库里面的默认分支master进行推送)
git push -u origin master (表示最近一段时间我都是往这个origin里面去推送提交),下次我们再推送的时候直接使用 git push就可以了
git branch -va 会列举出所有的分支
以本地仓库为基准
在远程创建空仓库
添加远程连接 git remote add <name> url
推送 git push <name> <branch>
多人协作:
拉取 git fetch<name>
合并 git merge<branch>
拉取并合并 git pull <name><branch>
推送 git push <name><branch>
拉取远程仓库内容:
git clone http://....
拉取最新版本的远程仓库,因为在协作开发中大家同时去拉取稳定版本的文件之后,有人已经向master去提交过自己需要完成的代码了,所以在这之后你去提交的时候会报错,原因就是当前你进行操作的版本与服务器的稳定版本不同步,所以此刻需要进行一步融合操作 指令 git fetch origion
将本地的master与远程拉取下来的master进行merge最终合并 指令 git merge origin/master
使用SSH密钥来解决每次拉取上传都要密码的问题,有教程,先不学
git工作流
集中式工作流:每个工作人员直接去操作远程仓库的master分支
功能分支工作流
Gitflow工作流
Forking工作流
```

```shell
集中式工作流
克隆中央仓库
敲代码
git pull --rebase
解决冲突
git add .
git rebase --continue
git push
集中式工作流程各阶段步骤:
cd Desktop/
git clone http://...
git add .
git commit -m "修改了文件"
git fetch origin //为什么要执行这步关键的指令,因为在我们提交版本之前可能有人已经提交给远程仓库代码了,所以就会导致我们现在的代码与远程服务器端的代码不一致,导致报错,所以我们拉取一下代码验证一下看看是不是服务器端代码与我们自己这边代码不一致
git merge origin/master //将我们的代码与服务器端的代码进行融合操作
这一步会出现融合报错,因为在服务器端口拉取并且上传代码的时候会存在在一个html文件中有多个人进行了修改,所以在融合的时候就会报错问你是需要保留哪部分的代码
解决完冲突之后
git add .
git commit -m "解决冲突"
git push
//注意: 在我们merge解决冲突的时候,也会提交给远程服务器一个版本,这是merge属性一个不好的地方,解决的方法就是将 git rebase origin/master 这个命令代替 git merge origin/master ,完事之后如果没有冲突的话直接git push,这种解决办法就叫做变机,实现原理是将我们本地的master直接移到origin master后面,如果有冲突就解决冲突
使用pull代替 fetch和merge这两步操作,其中git pull --rebase这个指令就代表了两步操作,分别是fetch和没有融合操作显示到提交情况列表的merge ,这个时候如果有融合解决就会显示出来,保留上方操作,解决完成融合报错问题之后,继续执行程序 先git add . 保存我们之间的融合操作,再 git rebase --continue ,最后再 git push
注意: 先pull 再push,其中pull有两种方式,一种是merge , 一种是变机,(merge会多一个版本,因为它会保存一个融合的版本),(变机则不会,他直接将我们的版本提交到远程版本的下一个了,所以不会多一个新的版本), 我们git pull的时候可以加一个 --rebase 就是变机
```

```shell
功能分支工作流
新建分支开发
开发完上传到远程仓库分支
pull request
接受或者修改
功能分支工作流各阶段流程:
git add .
git commit -m "修改了"
git push //会报错,因为功能分支工作流的理念就是将服务器端的一个master进行保护,不允许开发者随意地去提交,需求通过验证才能够提交,所以我们需要去创建一个分支branch
git branch lunbotu
git checkout lunbotu
git log //查看提交历史
git push origin lunbotu //因为远程的master分支是受保护的,不能够直接去提交,所以我们可以先提交到远程的一个lunbotu分支
在远程的轮播图里面有一个活跃分支，有提交合并请求的按钮,进去填写一下基本信息,当然了在所有人那边都是可以看到这个合并请求的
```

```shell
gitflow
master分支只存储写好的版本
develop分支相当于以前的master
功能分支
replease分支
```

```shell
Forking工作流
fork别人仓库
添加上游仓库的地址
往自己的仓库传代码
Pull request
```

## 如何使用git命令工具解决你和你的小伙伴代码版本不一致的问题？

```
思想：首先拉取别人线上远程仓库，此时会有冲突，我们就在vscode里面一步步解决冲突，当所有的冲突都解决完成之后再提交到自己的线上仓库中，然后你再自己线上仓库中发MR(Merge Requests)给指定的人A，指定人A再线上解决完冲突之后会给你合作小伙伴说你可以拉取线上代码了，然后你的合作小伙伴再执行和你一样的操作，最后你和你的小伙伴都再拉取一下别人线上远程仓库，就可以实现你们两个的代码版本一致了(别人线上仓库叫upstream,分支叫dev,自己的线上仓库叫origion,分支名叫dev)
a.git pull --rebase upstream dev
b.git rebase --continue
解决冲突:当所有的冲突都解决完成之后再进行下面的操作(如何查看自己的冲突是否解决完成?利用vscode的源代码管理,快捷键 ctrl + shift + G)
c.git add .
然后重复执行b和c的操作,直到vscode工具下面的源代码管理中不再出现合并冲突,或者使用git rebase --continue
发现下面没有任务了，此时我们的冲突就解决完成了，然后就可以把我们本地的代码上传到自己线上的仓库上去,
d.git push origion dev
e.再自己线上仓库发MR给指定人A，等待指定人A冲突的合并
f.等待你的小伙伴执行相同操作,当你们的冲突都解决完成之后,最后再拉取一下别人线上的仓库即可
git pull --rebase upstream dev
```

```
git pull --rebase upstream dev和git pull upstream dev的区别:
当执行git pull --rebase upstream dev和git push origion dev之后提示你当前版本落后于线上版本时,可以使用git pull upstream dev来进行解决。因为提示你当前版本落后于线上版本时,是因为git pull --rebase upstream dev会打乱当前的时间顺序。
```


