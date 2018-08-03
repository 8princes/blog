# Hexo 写作

标签（空格分隔）： hexo

---

[TOC]

## 安装

```
# https://hexo.io/zh-cn/docs/setup
npm install -g hexo-cli

# 创建博客文件夹 <git clone 8princes 请忽略>
hexo init <folder>
cd <folder>
# 安装 hexo 依赖
npm install
# 运行 server，默认：http://localhost:4000/
hexo server
```

## 写作

https://hexo.io/zh-cn/docs/writing

```
# 约定 <title> = [language] + [type] + [bug] + [any]
hexo new post <title>


```

## 发布

```
# 安装发布工具
# git 需要自己配置 ssh 公钥
npm install hexo-deployer-git --save

# 编辑配置文件：_config.yml

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/8princes/8princes.github.io.git
  branch: master

# 生成静态页面
hexo generate
# 发布静态页面
hexo deploy
```

