# ssh
ssh-keygen -t rsa -C "397063810@qq.com"

# 初始化git仓库
echo "# SourceCode" >> README.md  
git init  
git add README.md  
git commit -m "first commit"  
git remote add origin git@github.com:chenchuang93/SourceCode.git  
git push -u origin master  

# 从远程仓库克隆
git clone git@github.com:CyC2018/CS-Notes.git

# 提交代码
git commit -m "first commit" // git commit -a 
git push [remote-name] [branch-name]

# 同步代码
git pull

# 查看url
git remote -v

# 查看本地状态
git status