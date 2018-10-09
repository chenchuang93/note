# ssh
ssh-keygen -t rsa -C "397063810@qq.com"

# 初始化git仓库
echo "# SourceCode" >> README.md  
git init  
git add README.md  
git commit -m "first commit"  
git remote add origin git@github.com:chenchuang93/SourceCode.git  
git push -u origin master  