---
title: "在Daocloud安装自定义的Ghost Docker"
date: 2016-03-19T10:12:35+08:00
categories: 技术
tags: [ghost,docker]
---

Daocloud默认赠送2x的容器，正好运行一个Ghost博客，但是Daocloud官方提供的Ghost的安装不能自定义模版和配置。所以我们必须在Docker里做一些更改。

本人对Docker也是小白一枚，所以过程是比较复杂的，但结果很简单。

首先准备一个Git仓库，最好是私密的，因为会储存个人配置和修改过的模版。

首先是修改配置，这里不用Daocloud的持久化存储(因为会占用容器)，数据库使用Daocloud提供的MySQL，附件使用七牛云，邮件系统使用Mailgun，`production`部分配置如下：<!--more-->

```json
production: {
	url: process.env.GHOST_URL,
	mail: {
	    transport: 'SMTP',
	    options: {
		service: 'Mailgun',
		auth: {
		    user: '', // mailgun username
		    pass: ''  // mailgun password
		}
	    }
	},
	database: {
		client: 'mysql',
		connection: {
			host     : process.env.MYSQL_PORT_3306_TCP_ADDR,
			port     : process.env.MYSQL_PORT_3306_TCP_PORT,
			user     : process.env.MYSQL_USERNAME,
			password : process.env.MYSQL_PASSWORD,
			database : process.env.MYSQL_INSTANCE_NAME,
			charset  : 'utf8'
		}
	},
	storage: {
		active: 'qn-store',
		'qn-store': {
			accessKey: 'xxxxxxxxxx',
			secretKey: 'xxxxxxxxxx',
			bucket: '',
			domain: '',
			// timeout: '3600000', // default rpc timeout: one hour, optional
			// if your app outside of China, please set `uploadURL` to `http://up.qiniug.com/`
			uploadURL: 'http://up.qiniug.com/',
			filePath: 'YYYY/MM/' // default `YYYY/MM/`, will be formated by moment.js, using `[]` to escape
		}
	},
	server: {
	    host: '127.0.0.1',
	    port: '2368'
	}
},
```

[七牛云存储插件](https://github.com/minwe/qn-store)请使用git方式安装，这样更改Docker的时候更方便一些。

然后是Dockerfile，这里是从[官方](https://github.com/docker-library/ghost)继承的镜像(heartnn试过从ubuntu环境开始构建，实在不是我等新手能搞定的)：

```dockerfile
FROM ghost:0.7.8

# Add in better default config 
ADD config.example.js $GHOST_SOURCE/config.example.js
ADD storage.tar.gz $GHOST_SOURCE/content/
ADD casper.tar.gz $GHOST_SOURCE/content/themes/

# Fix ownership in src
RUN chown -R user $GHOST_SOURCE/content

# Default environment variables
ENV GHOST_URL http://heartnn.com
ENV PROD_FORCE_ADMIN_SSL false
ENV NODE_ENV production
```

修改GHOST_URL为自己的博客地址即可，Git仓库里对应的还有：

- config.example.js
- storage.tar.gz #七牛云存储对应目录
- casper.tar.gz #自定义模版

Windows下可以用7-Zip压缩tar.gz。全部完成后`git push`。

最后到Daocloud后台进行代码构建，成功后从镜像仓库中部署即可，Daocloud绑定域名时提供的CNAME是香港的ucloud，速度也是非常快的。
