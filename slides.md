---
# 还可以尝试使用“default”启动simple
theme: seriph
# 随机图片 from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  、
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# 在单页（SPA）构建中启用 pdf 下载，也可以指定一个自定义 url
download: true
---


# Git flow工作流

Author： 张全地  
2022.05.31

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    start<carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Git branch分支

### 长期分支  

1. master （主分支）
	* 版本库初始化以后自动建立的
	* 代码库中有且仅有一个主分支，不可删除
	* 不要直接修改master分支代码
	* master分支对外发布，每一次推送应该打标签（tag）做记录，方便追溯
2. develop （开发分支）
	* 基于master分支创建
	* 代码库中有且仅有一个开发分支，不可删除
	* 不要直接修改master分支代码

<br>
<br>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## 短期分支（协助分支）
3. feature （功能分支）
	* 新功能或新特性开发分支
	* 基于develop分支创建
	* 新特性或新功能开发完成，合到develop分支
	* 临时分支，功能完成后可删除
4. release （预发布分支）
	* 基于develop分支创建（feature分支合并到develop分支之后）
	* 临时分支，产品上线后可选删除
	* 发布到内部测试区，供测试人员测试
5. hotfix （补丁分支）
	* 基于master创建
	* 对线上的版本打补丁或bug修复
	* 修复完成后，合并到master分支和develop分支
	* 临时分支，修复上线后可删除

---
layout: two-cols
---

# Gitflow工作流
**GitPrime** CEO：  
Vincent Driessen（文森特 · 德里森）2010年  
博文：[成功的 Git 分支模型](https://nvie.com/posts/a-successful-git-branching-model/)

::right::

![在这里插入图片描述](https://img-blog.csdnimg.cn/8acae3c312b74731b71465ef734c1e4c.png)
---
layout: two-cols
---
# 创建项目初**始化仓库**  

* 初始化仓库，自动创建master
* 基于mater创建develop分支

::right::

![在这里插入图片描述](https://img-blog.csdnimg.cn/b62b8236a6a54b33b6683254b9f70457.png)
---
layout: two-cols
---

# 开发**创建功能分支**	
* 基于develope创建feature分支
* 功能分支完成，merge到develop分支

::right::

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e0fb1ae7c084b77a8819729eea32820.png)
---
layout: two-cols
---

# 预发布

* 基于develop创建release分支
* 完成release后merge到master分支，同时merge到develop分支

::right::

![在这里插入图片描述](https://img-blog.csdnimg.cn/92ef7f850ad84577ad539b708a817812.png)
---
layout: two-cols
---

# hotfix修复（线上版本出现问题）

* 基于master创建hotfix分支
* 修复完成后，merge到master分支，同时合并到develop分支

::right::

![在这里插入图片描述](https://img-blog.csdnimg.cn/e46a22772d094b269a7fe8ec4d2dcae3.png)
---

# 完整的gitflow工作流程图

![在这里插入图片描述](https://img-blog.csdnimg.cn/452a5462ba234b91bc8521f30c3195a1.png)

---

# Git flow落地

### 初始化项目
* 创建并切换到Develop分支
```bash
	git checkout -b develop master
```
* 创建feature功能分支
```bash
	git checkout -b feature-x develop
```
---

### 功能开发
* 开发完成后，将功能分支合并到develop分支：
```bash
	#切换到develop
	git checkout develop
	#合并功能分支到develop
	git merge --no-ff feature-x
	#切换到功能分支并删除
	git branch -d feature-x
```
---

### 预发布 => 发布
* 创建release预发布分支
```bash
	git checkout -b release-1.2 develop
```
* 完成后，合并到master分支
```bash
	#切换到master分支
	git checkout master
	#合并release到master
	git merge --no-ff release-1.2
	# 对合并生成的新节点，做一个标签
	git tag -a 1.2
```
* 再合并到develop分支
```bash
	#切换到develop分支
	git checkout develop
	#合并release到develop
	git merge --no-ff release-1.2
```
* 合并完成之后删除release分支
```bash
	git branch -d release-1.2
```
---

### hotfix修复
* 创建hotfix分支

```bash
	git checkout -b fixbug-0.1 master
```
* 完成后，先合并到master分支
```bash
	#切换到master分支
	git checkout master
	#合并hotfix到master
	git merge --no-ff fixbug-0.1
	#打标签
	git tag -a 0.1.1
```
* 再合并到develop分支
```bash
	#切换到develop分支
	git checkout develop
	#合并hotfix到develop
	git merge --no-ff fixbug-0.1
```
* 删除hotfix分支
```bash
	git branch -d fixbug-0.1
```
---

# 落地方式二
安装git-flow工具

---

# 落地方式三

使用Sourcetree可视化工具管理
## 安装Sourcetree工具
最新版本[Sourcetree安装教程](https://blog.csdn.net/zqd_java/article/details/123681302)

---

##  初始化项目
clone示例项目：https://gitee.com/quandizhang/git-flow.git
![在这里插入图片描述](https://img-blog.csdnimg.cn/ab55ab208acb46e0a859d9c66d189eea.png)

---

## 使用git工作流初始化
![在这里插入图片描述](https://img-blog.csdnimg.cn/3acb78be814249b28a808defba3d0d67.png)

---

* 点击确定后，本地会自动创建并切换到develop分支。
![在这里插入图片描述](https://img-blog.csdnimg.cn/cead169ad64a493bbbb633ee3efd98f9.png)

---


* 我们将本地develop推送到云端
![在这里插入图片描述](https://img-blog.csdnimg.cn/a45d1f782ee642b0ae57abd509e299c7.png)

---

## 接下来，我们创建feature功能分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/135ba729fe734b0ea2dfd0b71a24b84f.png)

---

* 基于develop分支创建
填写分支名称
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ab5e4261b51414ca53811df2b6ef214.png)

---

* 创建完成
自动切换到新创建的分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/dc6343ce62e74661be5e9c5238d7078a.png)

---

* 同样，我们需要把新创建的分支推送到云端仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/28e6544e7c174be885c24cf72358629a.png)

---

* 完成feature功能分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/55fb0ae1e00b4665ade2336486505ce8.png)

---

* 合并到develop，并删除功能分支：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8878ea68048242f19114dcfa922f31a7.png)

---

## 创建release预发布分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/a32fe634f9e7471f89ea17fe48b51a56.png)

---

* 基于develop创建
填写预发布版本名称
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0cf20e0f98e40e19fe6acc601c14a35.png)

---

* 创建完成
![在这里插入图片描述](https://img-blog.csdnimg.cn/33c934635bbf439ba3aa8fdcc789e514.png)

---

* 同样需要推送到远程仓库：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0921bc2c1134423e9448e8e43d4cbca1.png)

---

* 完成预发布版本
![在这里插入图片描述](https://img-blog.csdnimg.cn/c85f479d453b4d1f9bda9a256fb821ac.png)

---

* 标签（版本号）
删除release分支
合并到master分支，合并到develop分支![在这里插入图片描述](https://img-blog.csdnimg.cn/07f79f457e6041c6b2261de4ed50413a.png)

---

查看标签：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d54f63768e340bcac8fc2880d18122b.png)

---

## 创建hotfix
![在这里插入图片描述](https://img-blog.csdnimg.cn/3dbb58de2e394f62b11edfe117808f14.png)

---

* 基于master创建
填写补丁名称
![在这里插入图片描述](https://img-blog.csdnimg.cn/9771035da0b940e29928b9c3fa564b71.png)

---

* 同样，需要推送到远程仓库：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a0e2d7284bd24496a8567c46a813ced7.png)

---

* 完成补丁修复
![在这里插入图片描述](https://img-blog.csdnimg.cn/df353a28d5a04b51a68d861594efd7b7.png)

---

* 标签（版本名称）
合并到master，合并到develop
删除hotfix分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ef9d1e834dc4a57972c58ca1f76b444.png)

---

* 这边会生成一个新标签：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8108f72828d24df6b66869bc56c66ac3.png)
（完结）上面就是使用Sourcetree实现Git flow工作流的整个过程。
