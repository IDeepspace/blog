---
title: Web 前端工程化（二） —— CI/CD
author: Deepspace
categories: FrontEnd
date: 2019-11-05
urlname: what-is-ci-cd
tags:
  - 持续集成
  - 持续交付
  - 持续部署
---

工厂的生产中，我们需要思考如何让生产线以快速、自动化和可重复的方式从原材料生产出消费品。在软件开发领域，也同样会思考同样的问题：**如何以快速、自动化和可重复的方式从源代码生成发布版本。**

`CI/CD` 是一种通过在应用开发阶段引入自动化来频繁向客户交付应用的方法。`CI/CD` 的核心概念是持续集成、持续交付和持续部署。

缩略词 `CI/CD` 具有几个不同的含义：

`CI/CD` 中的 `CI` 指持续集成（`Continuous Integration`），`CI/CD` 中的 `CD` 指的是持续交付（`Continuous Delivery`）或持续部署（`Continuous Deployment`）。

<!-- more -->

### 一、持续集成

持续集成（`CI`）是在源代码变更后，**自动检测、拉取、构建和进行单元测试的过程**。持续集成是启动管道的环节。

> 注意：这里的**持续**指的并不是 **“一直在运行”**，而是 **“随时可运行”** 的意思。

有了持续集成，软件在每次修改之后都可以保证是可工作的（假如有足够全面的自动化测试集合的话），即使它被坏了，我们也能很快知道，并且修复，避免了应用程序在相当长的一段时间处于内不可用的状态。

<img src="https://gitee.com/IDeepspace/image-hosting/raw/master/FrontEnd/Contiunous-Integration.png" alt="Contiunous-Integration" style="zoom:50%;" />

<p align="center">(图片来自网络)</p>
#### 持续集成的优势

- 提高开发人员的工作效率

  持续集成可将开发人员从手动任务中解放出来，并且鼓励有助于减少发布到客户环境中的错误和缺陷数量的行为，从而提高团队的工作效率。

- 更快发现并解决缺陷

  通过更频繁的测试，团队可以在一些缺陷稍后变成大问题前就发现，并解决这些缺陷。

- 更快地交付更新

  持续集成有助于团队更快、更频繁地向客户交付更新。

下面我们着重看下持续集成的内容，有些偏理论，但是很有用。

#### 准备工作

1. 版本控制

   在《持续交付》这本书中，对于持续集成中的版本控制，书中指出的好实践是：**与项目相关的所有内容都必须提交到一个版本控制库中，包括产品代码、测试代码、数据库脚本、构建与部署脚本，以及所有用于创建、安装、运行和测试该应用程序的东西。**把所有相关的东西放在一个版本控制库里面，有利于我们严格控制每次代码的变更都必须确保相关的代码检查和测试等工作都通过。

   但是，在我的项目实践当中发现，如果项目比较复杂，测试集合（单元测试、集成测试、UI 测试、`API` 功能和性能测试等）会非常多，再加上一些其他的中间件服务（如使用 `NodeJS` 和 `Python` 写的用作系统间通讯的中间件），如果都放在一个版本控制库里面，代码库就会变得异常庞大，并且每次的代码提交如果触发所有类型的测试，那么一次持续集成过程需要的时间也会非常长（我所在的项目常常需要 `30min`）；并且，如果 `CI` 工具（如 `CIrcleCi`）是按照使用时长来收费，一次也会增加成本。

   如果你的项目是一个遗留系统，那么在改造整个遗留系统的过程中，也会因为某些依赖的环境或者 `Module` 的版本过于老旧且无法升级而造成开发阻碍。比如：遗留系统中依赖的 `NodeJS` 版本还是 `6.x`，并且无法快速升级，`AWS` 的 `Lambda` 服务已经放弃支持 `NodeJS 6.x` 的版本了，如果想要使用一些新特性，最终也只好新起一个版本仓库。

   所以，关于版本控制这一点，我觉得应该也需要看实际的应用场景，对非中大型、不那么复杂的项目而言，把和程序相关的东西放在一个版本控制库里，还是很有必要的。

2. 自动化构建

持续集成的构建过程需要实现自动化。并且，需要满足的条件是：**人和计算机都能通过命令行自动执行应用的构建、测试以及部署过程。**构建脚本应该被与代码库一样同等对待，也需要有测试、重构。

3. 团队共识

   持续集成不是一种工具，而是一种实践。它需要团队开发人员遵守一些准则，达成共识。需要每个人一致认同 “**修复破坏应用程序的任意修改是最高优先级的任务**” 。只有大家都接受这样的准则，才能按照预期通过持续集成提高质量。

   > PS：如我们团队遵循的一个准则：**`Pipline` 变红不能过夜**，过夜就要请大家喝奶茶哦。

#### 持续集成包含的任务

持续集成一般执行的任务有：

1. 代码静态扫描：通过静态扫描确定代码的一些潜在 `bug`，如未被使用的变量等。
2. 代码样式检查：团队一致定义出需要遵循的编码规范，并通过一些插件对代码进行样式合规性检查，防止不规范的代码进入版本库。比如方法名首字母小写、类的大字母大写、`if` 关键字后面需要加空格等问题都可以纳入到样式检查中。
3. 测试：通过运行自动化的单元测试、集成测试、系统测试可以有效的保证代码的质量和程序功能的正确性。一旦有测试失败，开发人员就需要快速反应进行修复。
4. 测试覆盖率检查：一般项目会设置一个测试覆盖率指标，如果代码达不到这样的测试覆盖率，就不会允许代码迁入。这样可以保证开发人员在新增功能时也要为新加入的功能编写自动化测试。
5. 编译打包：确保没有任何语法错误，生成构建产出物。
6. 发布。

这些任务都必须是能通过命令行自动完成的，不同类型的项目任务略有不同。

#### 持续集成的原则

1. 经常提交代码
2. 不要提交无法构建的代码
3. 构建失败之后不要提交新代码
4. 编写自动化测试
5. 等提交测试通过后再继续工作
6. 回家之前，构建必须处于成功状态
7. 时刻准备回滚到前一个版本
8. 在回滚之前要规定一个修复时间
9. 不要将失败的测试注释掉
10. 测试驱动开发

#### 持续集成的工具

市场上有很多产品可以提供针对自动化构建和测试过程的基础设施。本质上，持续集成的软件包括两个部分：

- 一个一直运行的进程，轮询版本控制系统，查看是否有新的版本提交。

  > 当然，你也可以做一些限制，不让持续集成软件自动运行构建脚本和测试，如过滤 `master` 分支之外的其他分支。

- 提供展现这个流程运行结果的视图，通知构建的成功与否，让开发人员可以找到测试报告，通过 `log` 查看构建过程和构建失败的原因。

常用的构建工具有：

- [Jenkins](http://jenkins-ci.org/)
- [Travis](https://travis-ci.com/)
- [CircleCI](https://circleci.com/)
- [GoCD](https://www.gocd.org/)
- [Gitlab CI](https://about.gitlab.com/product/continuous-integration/)

### 二、持续交付

持续交付（`Continuous Delivery`）是一种软件工程的方法论。持续交付指的是频繁地将软件的新版本，交付给质量团队或者用户（或者是 `Product Owner`）。这种方式可以减少软件开发的成本与时间，减少风险。

#### 类生产环境

在持续集成的基础上，我们将集成后的代码部署到更贴近真实运行环境的 **“类生产环境”** 中。通常至少需要一个类生产环境。

我所在的项目中，创建了两个 “类生产环境”：`DEV` 环境（预发布环境）和 `Staging` 环境。

- `DEV` 环境：用作开发自己线上测试，可以随意部署任意分支代码。

- `Staging` 环境：和 `Production` 环境的配置基本保持一致，用于部署比较稳定的代码版本，有时候用于测试应用程序的某一功能是否稳定。同时，`Staging` 环境也会和其他上下游的系统做集成测试，其他上下游系统的 `Staging` 环境的数据和功能也依赖于我们的 `Staging` 环境。所以，需要保证我们的 `Staging` 环境的应用程序也总是正常工作的。

- `Production` 环境：生产（产品）环境，直接面向于用户。应用程序在 `Staging` 环境中运行正常，没有 `bug`，可以手动部署到生产环境中。

<img src="https://gitee.com/IDeepspace/image-hosting/raw/master/FrontEnd/Continuous-Delivery.png" alt="Continuous-Delivery" style="zoom: 50%;" />

<p align="center">(图片来自网络)</p>
### 三、持续部署

持续部署（`Continuous Deployment`）是持续交付的下一步，是指能够自动提供持续交付管道中的应用程序版本给最终用户使用。

持续部署的前提是能自动化完成测试、构建、部署等步骤。

<img src="https://gitee.com/IDeepspace/image-hosting/raw/master/FrontEnd/Continuous-Deploy.png" alt="Continuous-Deploy" style="zoom:50%;" />

<p align="center">(图片来自网络)</p>
#### 和持续交付的关系

有时候，持续交付也与持续部署混淆。持续部署意味着所有的变更都会被自动部署到生产环境中。持续交付意味着所有的变更都可以被部署到生产环境中，但是出于业务考虑，可以选择不部署。如果要实施持续部署，必须先实施持续交付。
