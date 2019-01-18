本体开发101：创建您第一个本体的指南

Natalya F. Noy 和 Deborah L. McGuinness

# 1  为什么要开发一个本体？

本体(ontology)是指领域概念体系（包括领域术语定义和术语间关系）的形式化规范定义(Gruber 1993)。 近年来，本体开发的工作逐步从人工智能专家的专属领域转移到领域专家的手中。本体在万维网上颇为常见，例如雅虎(yahoo.com)的网站分类体系和亚马逊(Amazon.com)的产品分类与描述体系。万维网联盟（W3C）开发的RDF语言，也即是资源描述框架（Brickley and Guha 1999），支持开发者把网页中的知识表示为机器可解析的格式，从而可以被智能机器人理解并处理。美国国防高级研究计划署（DARPA）与W3C一起，正在通过扩展RDF来开发DARPA智能机器人标记语言（DARPA Agent Markup Lanuguage, DAML。后来升级为W3C的万维网本体语言，Web Ontology  Language，OWL），以更有针对性的构架来促进智能机器人在互联网上的交互（Hendler和McGuinness 2000）。许多学科现在开发了标准化本体以支持领域专家分享和注释领域信息。医学领域出现了大量标准化、结构化的词汇集，例如SNOMED（Price and Spackman 2000）和UMLS（Humphreys and Lindberg 1993）。同时也出现了众多通用本体，例如，联合国开发计划署和美国美国邓白氏公司（Dun＆Bradstreet）共同制定了为产品和服务提供术语的UNSPSC本体（www.unspsc.org）。

为了帮助研究人员在领域中共享信息，本体论定义了常用词汇集，包括机器可理解的领域基本概念及其关系的定义。 那为什么要开发一个本体呢？大致有如下原因：
* 达成信息结构的共识
* 复用领域知识
* 领域规则的显性表示
* 区别领域知识与操作知识
* 分析领域知识


【达成信息结构的共识】是本体开发的常见目标（Musen 1992; Gruber 1993）。例如，当几个不同的网站都包含医疗信息或提供医疗电子商务服务时，如果它们使用相同的底层本体，则只能机器人可以从这些网站中提取并融合信息，进而使用聚合的结果来回答用户查询或作为其他应用程序服务。

【复用领域知识】是本体研究近来激增的动力之一。例如，许多不同领域的模型需要用到时间概念，包括时间区间，时间点，时间的相关度量等。如果有一组研究人员详细研究并定义了这样一个时间本体，那么就可以被其他人直接复用。如果需要构建一个大规模的本体，我们既可以综合若干相关领域的现有本体，也可以从通用本体出发有针对性地扩展领域本体。

【领域规则的显性表示】便于专家有效应对领域知识的变化。领域知识不但包括事实，也包括规则，例如，“鹦鹉是鸟”为事实，“鸟会飞“为规则，那么领域规则的应用就可以推导出”鹦鹉会飞“。如果我们简单地把领域规则用编程的方式表示出来，不但全程依赖程序员支持维护，而且这样的表示亦不利于专家理解与更新。领域规则的显性化表示也便于新用户完整理解领域术语的完整意义。

【区别领域知识与操作知识】是本体的另一个常见用途。正如数据和数据处理流程可以分离，领域应用中不但需要领域知识，同时可以描述如何配置与处理这些领域知识。例如，我们可以定义一个通用的知识图谱问答系统，不但可以使用音乐知识图谱作为数据源，亦可以使用影视知识图谱为数据源。

【分析领域知识】通畅依赖显性的表示领域知识。形式化的术语描述有助与精准地理解领域概念的定义，进而支持知识复用（McGuinness等人，2000）。

构建本体并非领域建模的终点。开发一个本体只是定义了一组数据及其结构。后续的程序（例如推理系统，领域无关的应用程序，或智能机器人）都可以使用本体和基于本体构建的知识库作为输入数据。例如，本文开发了葡萄酒和食物的本体，以及葡萄酒与餐点的合理搭配组合。这个本体可以作为一套餐馆管理工具中应用程序的基础：一个应用程序可以依据当天的菜单创建推荐的葡萄酒或回答服务员和客户的查询。另一个应用程序可以分析一个酒窖的库存列表，并建议今后扩展哪些葡萄酒类别，或是为即将推出的菜单订购搭配的葡萄酒。


## 关于本指南

本文采用Protege-2000（Protege 2000），Ontolingua （Ontolingua 1997），Chimaera（Chimaera 2000）作为本体编辑环境。本指南使用Protege-2000展示本体实例。

本指南中使用的葡萄酒和食品示例源自CLASSIC，一个基于描述逻辑（description logic)的知识表示系统，的论文（Brachman 等人. 1991）以及短教程（McGuinness等人，1994）。 Protege-2000和其他基于框架的系统（frame-based systems) 被用于编辑例子中提到的本体，通过文本和可视化显式说明了分类树体系以及实体所属的具体分类。

本指南中的部分本体设计思想源自于面向对象设计的文献（Rumbaugh et et al。 1991; Booch等人，1997），但是本体开发与面向对象编程中的类和关系的设计有所不同。面向对象的编程主要围绕类的方法，也就是说程序员根据类的操作层面的属性进行设计；而本体设计师根据类的数据结构层面的属性进行设计。

本指南不能解决本体开发人员可能需要处理的所有问题，而是试图提供一些起步性的指导意见，帮助一个新的本体设计师开发新的本体。关于领域建模中的更为复杂数据结构与设计原则，本文点到为止并给出了一些参考资料。

本体设计本身的方法论并无定规，本文的思路主要来源于作者在本体开发中积累的经验。在本指南的结尾也列举了一些替代方法的参考文献.
