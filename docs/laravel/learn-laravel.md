#### 学习策略

Laravel 是个功能齐全的全栈框架，学习她相当于你在学习成为全栈工程师。如果你之前没有学习过类似的全栈框架，你会发现你很快会被埋进大量的技术概念和专有名词里。这并不是你不够聪明，而是：
人类短时间内的记忆和信息处理能力都是有限的，当短时间内暴露在大量的信息面前时，你的注意力会被严重分散，带来的是挫折感和烦躁不安。

所以，我们需要一套更加聪明的学习策略。

我将框架知识分类为以下：

- 底层实现知识 —— 如 [服务容器](https://d.laravel-china.org/docs/5.5/container)、[服务提供器](https://d.laravel-china.org/docs/5.5/providers)、[Facades](https://d.laravel-china.org/docs/5.5/facades)、Contracts、Repository 等
- 框架使用知识 —— 如用户注册登录、邮件发送、数据模型的 CRUD、用户数据获取等

每一个分类下都有非常多的概念需要学习，但是很明显，学习框架的使用要比学习底层实现原理要简单有趣多了，并且因为学习的愉悦性高了，我们能记得更牢固。
当你有一定的框架使用经验以后，再去学习底层实现的概念，你能更好地理解这些技术概念的来龙去脉，最终达到会事半功倍的学习效果。并且这时候学习底层实现，也会让你对框架的理解更加深入，你会发现你对框架使用技巧会变得更加灵活。
用比较简单的话来讲，就是在一开始学习的时候，先不管底层实现，利用框架提供的功能，先建造一些可用的项目，等熟悉掌握了这些框架功能的使用以后，再去学习底层实现概念。
即使是做了分类，并且有了先后顺序还不够。因为单单框架使用这部分的知识，涉及的概念也是非常多，很容易陷入信息过载的情况。所以我们需要有一个循序渐进的方案，先学习简单的，常用的概念，然后再慢慢学复杂的，并且在学习的过程中要注意重复学习，这样概念才能记得越牢固。

#### 推荐学习路径

##### 1. 框架的使用知识学习#

基于以上的思想，我创建了 《Laravel 实战课程》，计划中有三本（也有可能更多），分别是：

- 第一本 —— 《Laravel 入门教程 - 从零到部署上线》
- 第二本 —— 《Laravel 进阶课程 - 从零开始构建论坛系统》
- 第三本 —— 《Laravel 高级课程 - 构架 API 服务器》

第一本书教授如何使用 Laravel 一步一步构建一个类似新浪微博的应用，书中很多技术话题会被一带而过，这是有意而为之的，我们希望让读者保持对编码线索的专注，不被篇幅悠长的名词解释分心。通过阅读本教程，你将学到如 HTML、CSS、JavaScript、PHP 和 Laravel 等 Web 开发相关的基础知识。不仅如此，本书还会对这些基础知识点进行延伸扩展，为你讲解一些在 Web 开发中更为专业、实用的技能，如 Git 工作流、Laravel Mix 前端工作流、Bootstrap 框架基本使用等。这些知识将为你未来的编程开发奠定下坚实的基础。

第二本以构建论坛项目 LaraBBS 为线索，展开对 Laravel 框架的全面学习。编码规范遵循 Laravel 项目开发规范 ，应用程序架构思路贴近 Laravel 框架的设计哲学。在论坛系统的构建中，我们将学到多角色用户权限系统、管理员后台、注册验证码、图片上传、图片裁剪，XSS 防御、自定义命令行、自定义中间件、任务调度、队列系统的使用、应用缓存、Redis、模型事件监控、表单验证、消息通知、邮件通知、模型修改器等知识。在本课程的学习中，你不仅能学到使用 Laravel 开发一个论坛项目，还能学到安全优先、高扩展性的大型项目架构经验。

第三本将以构建 API 服务器为目标，来展开。目前本课程正在紧张撰写中，敬请期待。

##### 2. 框架的底层实现学习#

学完了以上三本书，你将拥有一定的项目开发经验，对框架的功能使用也会有一个比较全面的系统性理解。这时候，会是学习『底层实现』的好时机。

底层实现的知识学习，可以从文档开始，打开 Laravel 的文档中心 —— [d.laravel-china.org](https://d.laravel-china.org)  ，找到最新版本的 Laravel 文档，然后仔细阅读 2、3 遍。因为有了上面的项目经验，此时的文档阅读啃起来会轻松多了。
阅读文档后，可以尝试看下 Laravel 底层的源码，看看这些框架的功能都是怎么实现的。

学习过程中可以适当做笔记，例如：

- zhangbao 同学的 [Laravel 文档阅读笔记](https://laravel-china.org/whoops-learning-notes)
- leoyang 同学的 [Laravel 源码分析笔记](https://laravel-china.org/leoyang)

#### 错误的学习方法

一上来就开始啃文档 。
如果你是新手，有太多的新概念你需要学习，你会发现学习起来非常艰难，甚至怀疑文档是不是写的太烂了（社区里经常出现这种抱怨）。
事实上，不是文档写的太烂，而是你把文档用错了。文档的『目的』是快速查阅，一份优秀文档的标准是语言简练，释义，这个 Laravel 的文档做的很棒。但是，文档并不适合做入门学习使用，上面我们已经讲过，原因是信息量太大。

##### 寻找网络上零散的课程进行学习。
如果你想学习单个概念，这些零散的小课程会很方便。但是，如果是想以阅读大量课程来达到系统性学习的目的，你将会很失望。很多时候你会感觉 —— 你好像学了很多，学了很久，以为自己学会了，但是心里还是没底气。
你需要的是通过项目，完整的项目，将所有的知识串起来去记忆。你的作品，清清楚楚摆在面前，看着你一步步构建出来的一套系统，自信心也会有所增加。

一开始就学习高级话题，如 [服务容器](https://d.laravel-china.org/docs/5.5/container)、[服务提供器](https://d.laravel-china.org/docs/5.5/providers)、[Facades](https://d.laravel-china.org/docs/5.5/facades)、Contracts、Repository 等
很多时候你会发现这些话题晦涩难懂，很难学习。并且即使你毅力比较好，死记硬背，很快也会忘记，学习效率非常低下。然后最重要的，学会这些概念，并无法使你掌握构建一个完整项目的能力。

EOF

Laravel 书籍：

- [《Laravel 入门教程 - 从零到部署上线》](https://laravel-china.org/topics/3383)
- [《Laravel 进阶课程 - 从零开始构建论坛系统》](https://laravel-china.org/topics/6592)
