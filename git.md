#git 命令行

### 克隆现有仓库
    git clone [url]

### 检查当前文件状态
    查看状态（较详细）
    git status
    状态简览
    git statis -s 

### 跟踪新文件
    git add 命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适
    git add <file>

### 提交
    git commit -m ['message']
    git commit -a -m ['message'] 修改后不用 git add

### 移除
    git rm
    git rm -f 强制移除

### 推送
    git push origin master

### 拉取
    git pull
