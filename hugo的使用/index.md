# hugo的使用

参考  <https://www.adamormsby.com/posts/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/>

### 安装hugo

1. github 下载linux安装包

  *注意：下载extend版*


2. 解压后把二进制文件复制到/usr/bin/

### create new site

```
#### hugo new site [SITE_NAME]
hugo new site mysite
```

### github 建新repo  


#### Enter the project folder.
```
cd mysite
```

#### Initialize git locally.
```
git init
```

#### Set our new Github repo as the remote for our local project

```
#git remote add origin https://github.com/[GITHUB_USERNAME]/[SOURCE_REPO_NAME].git
git remote add origin https://github.com/charlesren/mysite.git
```

#### Stage all files for commit.
```
git add .
```
#### Commit files.
```
git commit -m "committing our hugo template"
```
#### Push to the remote master
```
git push -u origin master
```


### 创建  Live Site Data Repository

```
#git submodule add https://github.com/[GITHUB_USERNAME]/[PUBLIC_REPO_NAME].git public
git submodule add https://github.com/charlesren/mysite-public.git public
```

#### 删除原public目录下文件
```
rm -rf ./public/*
```

#### Perform a site build and output to 'public/' directory.

```
 hugo
cd public
git add .
git commit -m "first build"
```

#####  Return to the project root.

```
cd ../
```

#### commit change

```
git add .
git commit -m "first build - update submodule reference"
```

##### Push the source project *and* the public submodule to Github together.

```
git push -u origin master --recurse-submodules=on-demand
```


### Add a Theme to Our Site

> Make a fork of the Hello Friend theme so we have our own copy of it.
>
> 这里fork https://github.com/dillonzq/LoveIt
>
> fork 后地址：  https://github.com/charlesren/LoveIt
>
> Add our forked repo as a submodule in our main project’s themes folder.
>
> Add the theme submodule from the root project folder
>
> my sample URL - https://github.com/aormsby/F-hugo-theme-hello-friend.git
>
> git submodule add https://github.com/[GITHUB_USERNAME]/[FORKED_THEME_REPO_NAME].git themes/hello-friend

```
git submodule add https://github.com/charlesren/LoveIt.git  themes/LoveIt
```

#### 修改config.toml

```
cp themes/LoveIt/exampleSite/config.toml .

sed  -i 's/^themesDir = .*/themesDir = "themes"/' config.toml

# Because we’re making a Github ‘project site’ we want to change thebaseURL to match our project URL.

##baseURL = [GITHUB_USER_NAME].github.io/[PROJECT_NAME]

#实际URL 为 baseURL = https://charlesren.github.io/mysite-public/

sed  -i 's|^baseURL = .*|baseURL = "https://charlesren.github.io/mysite-public/"|' config.toml
sed  -i 's|^title = .*|title = "CharlesRen"|' config.toml
sed  -i 's|Email = "xxxx@xxxx.com".*|Email = "renccn@gmail.com"|g' config.toml
sed  -i 's|Email = "xxxx@xxxx.com".*|Email = "renccn@gmail.com"|g' config.toml

## subtitle = "Focus on Cloud Native,Golang,Speculate in Stocks"
## subtitle = "专注于云原生、Go语言、股票投机"

# Header config
name = "CharlesRen"

##  [author]
  name = "CharlesRen"
  email = "renccn@gmail.com"
  link = "https://charlesren.github.io/mysite-public/"

```



### Test Site and Deploy
```shell
hugo
cd public
git add .
git commit -m "build with theme"
cd ../
git add .
git commit -m "build with theme - update submodule reference"
git push -u origin master --recurse-submodules=on-demand
```




至此站点已完成，但没有内容


### 添加about页

```
hugo new about.md

vi content/about.md
```

注意：生成静态网站时，hugo会忽略所有通过 draft: true 标记为草稿的文件。必须改为 draft: false 才会编译进 HTML 文件。
否则，生成的网站没有文章

### 添加其他posts

```
hugo new posts/fistPage.md
```


注意：生成静态网站时，hugo会忽略所有通过 draft: true 标记为草稿的文件。必须改为 draft: false 才会编译进 HTML 文件。
否则，生成的网站没有文章


### 问题

- 问题一

> Error: Error building site: failed to render pages: render of "page" failed: execute of template failed: template: _default/single.html:18:124: executing "content" at <partial "function/content.html">: error calling partial: "/data/mysite/themes/LoveIt/layouts/partials/function/content.html:4:19": execute of template failed: template: partials/function/content.html:4:19: executing "partials/function/content.html" at <partial "function/ruby.html" $content>: error calling partial: partial that returns a value needs a non-zero argument.

解决方案 ：  https://github.com/dillonzq/LoveIt/pull/519/files

- 问题二  about页面不显示



解决方案:config.toml写绝对地址


- 问题三 文章自动分类到标签、分类、文档

解决方案：文章前加tags\ categories
```yaml
---
title: "Firstpg"
date: 2020-11-26T16:45:15+08:00
draft: false
tags: ["cncf"]
categories: ["cncf","documentation"]
---
```

***如已通过本文建站，在别的设备上只需clone my-site,public,Loveit主题,即可使用***

