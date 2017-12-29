

## 配置步骤

1. 生成公钥密钥对        ssh-keygen -b 4096 -f git_rsa
2. 拷贝公钥到目标服务器（制定目标的用户，源服务器）   ssh-copy-id -i git_rsa git@git.example.com
3. 配置本地 ssh 配置文件  
          
          ~/.ssh/config file:
          Host git git.example.com
            User git
            IdentityFile /home/thomas/.ssh/git_rsa



## ssh-agent
http://www.unixwiz.net/techtips/ssh-agent-forwarding.html
