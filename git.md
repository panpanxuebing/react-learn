#git 命令行

### 克隆现有仓库
    git clone [url]

### 检查当前文件状态
    查看状态（较详细）
    git status
    状态简览
    git status -s 

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

### 生成 SSH 公钥
    - 生成公钥
    $ ssh.keygen
    - 查看保存公钥的位置
    ``` js
    cd ~/.ssh
    ls
    ```
    - 会看到以下两个文件 id_rsa 保存的是私钥， id_rsa.pub 保存的是公钥
    id_rsa id_rsa.pub
    - 我们只需要公钥
    cd id_rsa.pub
    - 复制公钥，在个人的git账号的设置里，添加新的 SSH keys, 把复制的公钥粘贴进去，保存就好
    