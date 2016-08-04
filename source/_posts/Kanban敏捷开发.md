# Kanban敏捷开发

title:  Kanban敏捷开发
date: 2016-02-12 18:00:00
tags:
- Agile

---

<img src="http://i.imgur.com/U6TqWxL.png" style="max-height: 300px;"/>

`Kanban`是敏捷开发（Agile Development）的一种实现模式。

早在1940年，日本丰田公司已借鉴超市库存的管理方法来改善自身的工作流程。超市在管理库存的时候，总希望库存量尽可能与消费者的需求接近，以减少不必要的库存。只要库存量能够及时根据消费者的需求量来调节，超市就能极大地提高仓库的运行效率，从而最终为自身和消费者都创造价值。丰田公司采用了这种及时管理模式（*Just In Time*， *JIT*）。

<!--more-->

在丰田公司内部，当一个组完成现有任务并准备好接受下一个任务时，它会把这个信息通过向其他组递出一张卡片，或者`Kanban`，来传达。虽然现代`Kanban`工作流程有了很大改进，但是它依然保留着*Just In Time*的核心理念。

>简单来说，及时制度（JIT）主要的核心是“让正确的物资，在正确的时间，流动到正确的地方，数量是刚刚好的数量。” *-- Wikipedia*

这种管理模式同样适用于管理软件开发。

----------

## 尽早发现瓶颈

![](http://i.imgur.com/4c1hLhB.png)

假设一个小组的工作流程为一个pipeline。如果里面的testers一周只能测试5个features，那么即使developers一周能实现10个新features，整个小组一周也只能交付5个features。这里，testers就是pipeline中的瓶颈。

假如这个瓶颈没有被发现，那么testers面前的任务就会堆积如山，尽管如此，整个小组的效率并没有得到提高。

![](http://i.imgur.com/EMcKcF8.png)

如果问题得不到解决，developers辛辛苦苦实现出来的新features无法及时投放市场，最终将可能导致错失商机。更有严重的是，如果testers为了提高效率，降低对质量的要求，将有可能导致低质量的代码进入到产品中。 从另外一个角度来说，如果团队及时发现瓶颈所在，就能对此作出调节。比如，他们可以调派更多的testers，或者让部分developers帮助完成部分的test automation工作。

所以，我们回到一开始的主题，看看`Kanban`如何及时地向团队反馈当前的瓶颈！

## 举一个栗子

`Kanban`管理模式简洁而有力。一个简单的`Kanban`系统甚至可以由一张大纸板，和贴在上面的便签组成。

在这个系统中，大纸板上画有多个列表。一个列代表一个的工作步骤，一张便签即代表一个的任务。每个任务都经过这个工作流程从最左边的列流向最右边的列。每个列顶部有一个数字limit，代表该列最大便签容量。

这个容量limit是`Kanban`和管理模式最大的不同。通过限制某步骤的最大容量，`Kanban`能够防止过度生产（overproduction），并动态地揭示流程的瓶颈。

>Limiting work-in-progress reveals the bottlenecks so you can address them.

在下面的栗子中，Test一栏已经达到了它最大的工作容量3，不能够放入新的任务。Analysis和Development因为Test进度的原因，无法把已经完成的任务挪到下一栏，也到达了它们的最大容量（3和5）而不能放入新任务。通过Kanban表格，团队发现Test成为了瓶颈，并开始思考如何帮助testers改进测试环节的效率。

![](http://i.imgur.com/KssgGef.png)

当testers完成了一个任务之后，这个任务便签就被挪到Deploy一栏。

![](http://i.imgur.com/ISwO9uT.png)

由于现在Test一栏终于可以接受新任务了，Test、Development、Analysis从各自上一栏中挪入一个新的任务便签。

![](http://i.imgur.com/HfjazC5.png)

从上面的例子可以看出，`Kanban`能够动态地展示团队工作流程的瓶颈。一旦Project Manager发现某个环节影响到团队进度，ta可以及时调配资源改进这个环节。


## 四大核心原则

`Kanban`从脱胎自丰田公司的工程管理方法以来，在不同领域都有发展出具有领域特色的实现形式。虽然形式多样，但是它们始终遵循着下面一些核心原则。（`Kanban`有四大原则和五大原则两种不同说法，这里我们采用了较为流行的四大原则说法）

为了表述准确，我们这里直接引用了文章 [*LeanKit：What is Kanban？*](http://leankit.com/learn/kanban/what-is-kanban/) 中对这四大原则的阐释。

- Visualize the workflow
- Limit Work in Process
- Focus on Flow
- Continuous Improvement

>**Visualize the workflow**
>By creating a visual model of your work and workflow, you can observe the flow of work moving through your Kanban system. Making the work visible—along with blockers, bottlenecks and queues—instantly leads to increased communication and collaboration.

>**Limit Work in Process**
>By limiting how much unfinished work is in process, you can reduce the time it takes an item to travel through the Kanban system. You can also avoid problems caused by task switching and reduce the need to constantly reprioritize items.

>**Focus on Flow**
>By using work-in-process (WIP) limits and developing team-driven policies, you can optimize your Kanban system to improve the smooth flow of work, collect metrics to analyze flow, and even get leading indicators of future problems by analyzing the flow of work.

>**Continuous Improvement**
>Once your Kanban system is in place, it becomes the cornerstone for a culture of continuous improvement. Teams measure their effectiveness by tracking flow, quality, throughput, lead times and more. Experiments and analysis can change the system to improve the team’s effectiveness.


## Scrum vs. Kanban
`Kanban` 和 `Scrum` 虽然有很多相似的概念，但是它们是两种不同的项目管理方法。这里，我们引用文章 [*Atlassian: A brief introduction to kanban*](https://www.atlassian.com/agile/kanban/) 中的一个表格来比较两者之间的不同。

| Item       |    SCRUM  | KANBAN     |
| :--------: | :--------:| :--------: |
| **Cadence** | Regular fixed length sprints (ie, 2 weeks) |  Continuous flow   |
| **Release methodology** |   At the end of each sprint if approved by the product owner |  Continuous delivery or at the team's discretion  |
| **Roles** |    Product owner, scrum master, development team | No existing roles. Some teams enlist the help of an agile coach.  |
| **Key metrics** |    Velocity | Cycle time  |
| **Change philosophy** |    Teams should strive to not make changes to the sprint forecast during the sprint. Doing so compromises learnings around estimation. | Change can happen at any time  |

有的团队把`Kanban`和`Scrum`两种方法糅合成一种叫做`Scrumban`的方法。这个方法吸取了`Scrum`中固定长度的*Sprint*和各种角色（*Product Owner*、*Sprint Master*等等）概念，同时也吸收了`Kanban`中“Focus on Flow” 和 “Limit Work In Progress”等原则。但是，刚开始采用Agile的团队建议只采取`Scrum`和`Kanban`中的一种，在熟练掌握所选方法后才根据团队特点进行相应的糅合或创新。

## 参考资料
[Kanban敏捷开发](http://hackjutsu.com/2016/02/12/Kanban%E6%95%8F%E6%8D%B7%E5%BC%80%E5%8F%91/#.VsDuYeaSMoI.google_plusone_share)
 [Atlassian: A brief introduction to kanban](https://www.atlassian.com/agile/kanban/)
 [LeanKit：What is Kanban？](http://leankit.com/learn/kanban/what-is-kanban/)
 [Kanban Blog：What is Kanban?](http://kanbanblog.com/explained/)
