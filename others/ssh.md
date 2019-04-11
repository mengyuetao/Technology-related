

## 配置步骤

1. 生成公钥密钥对        ssh-keygen -b 4096 -f git_rsa
2. 拷贝公钥到目标服务器（制定目标的用户，源服务器）   ssh-copy-id -i git_rsa git@git.example.com
3. 配置本地 ssh 配置文件  

          ~/.ssh/config file:
          Host git git.example.com
            User git
            IdentityFile /home/thomas/.ssh/git_rsa


## 异常日志
sshd
/var/log/secure

## ssh-agent
An Illustrated Guide to SSH Agent Forwarding
http://www.unixwiz.net/techtips/ssh-agent-forwarding.html


## 反向代理
利用ssh反向代理以及autossh实现从外网连接内网服务器

https://www.cnblogs.com/kwongtai/p/6903420.html
