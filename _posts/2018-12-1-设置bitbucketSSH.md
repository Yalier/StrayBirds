---

title: bitbucket配置密钥
layout: post
comments: false
category: 技术

---

	
	
	$ ssh-keygen 生成密钥文件并保存到本地
	
	$ ls ~/.ssh 列出保存到文件目录
	
	$ eval `ssh-agent` 设置代理
	
	$ ssh-add -K ~/.ssh/文件名 添加私有密钥名
	
	$ cat ~/.ssh/id_rsa.pub 显示公钥
	
	然后在bitbucket账号里面点击设置，点击ssh密钥，点击添加密钥将公钥复制进去保存就可以了,label可以去一个名字
	


