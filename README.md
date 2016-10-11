#常用git命令

mkdir   创建一个目录

pwd  显示当前目录

git init  把目录变成git可以管理的仓库

git add  将文件存放在暂存区

git commit -m  把文件提交到仓库   -m  ‘后面输入的是本次提交的说明’

git status  查看仓库的当前状态

git diff  查看文件修改的内容

git log   显示从最近到最远的提交日志（如果觉得信息太多就加上 --pretty=oneline 参数）


git cat readme.txt  查看内容

git remote  查看远程库的信息 

git remote -v显示更详细的信息


#版本回退

	HEAD 表示当前版本， 上一个版本就是HEAD^，上上一个版本就是HEAD^^ ， HEAD~100 往上100个版本

	git reset -hard HEAD^ 表示往上回退一个版本

	git reset -hard HEAD^^ 表示往上回退两个版本

	....依次类推

	

	#回到新版本
	
	git reset --hard commitId（只需要前7位）

	git reflog  查看记录的每一次命令	



	git diff HEAD -- readme.txt 查看工作区和版本库里面最新版本的区别


#撤销修改

	git checkout -- readme.txt  撤销修改内容，这里有两种情况：

	一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

	一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

	git reset HEAD file 可以把暂存区的修改撤销掉，重新放回工作区


	场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

	场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，
	第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。


#删除文件

	git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
	
	git rm 用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，
	你会丢失最近一次提交后你修改的内容。


#远程操作


	由于本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

	第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，
	如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

	$ ssh-keygen -t rsa -C "betrs@sina.com"
	你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

	如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，
	id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

	点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容即可


	$ git remote add origin git@github.com:betrs/gitStudy.git  本地仓库与github仓库关联

	git push  把本地库的内容推送到github

	要关联一个远程库，使用命令git remote add origin git@github.com:betrs/gitStudy.git；

	关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

	此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

	git push -fu origin master 强制覆盖已有文件



#远程克隆

	$ git clone git@github.com:betrs/gitStudy.git  从github上克隆一个项目到本地

	要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

	Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。


#创建与合并分支

	查看本地分支：git branch

	查看远程分支：git branch -a

	创建分支：git branch <name>

	切换分支：git checkout <name>

	创建+切换分支：git checkout -b <name>

	合并某分支到当前分支：git merge <name>

	合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
	如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
	合并分支，请注意--no-ff参数，表示禁用Fast forward
	

	删除本地分支：git branch -d <name>

	删除远程分支：git push origin :branch-name

	git log --graph命令可以看到分支合并图。


#BUG修复

	git stash  把工作区域储藏起来，等以后恢复现场后继续工作。

	git stash list  查看储藏的工作区

	修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

	当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。