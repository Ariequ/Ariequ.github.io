---
layout: post
title: "Behavior Tree(行为树)的C#实现"
date: 2014-09-23 22:26:46 +0800
comments: true
categories: [BehaviorTree]
---
在做一个战场上士兵自动战斗的功能的时候，查看了很多关于Behavior Tree的相关内容。在Unity中也有很多现成的可视化插件工具，可以很方便的定义和实现Behavior Tree。但是因为种种不符合需求的问题，最终还是选择自己来实现Behavior Tree的机制。  

定义士兵的AI，使用的是XML配置文件，XML的结构与实际生成的Behavior Tree结构非常一致。

Behavior Tree的结点类型包括Composite Node、Decorator Node、Condition Node、Action Node。其中Action Node是AI主体所能执行的具体行为，而通往各个叶子结点（Action Node）的路径就是Behavior Tree根据环境所做的决策过程。

先来看一段士兵的Behavior Tree的配置文件

``` ruby Solider Behavior Tree
<Selector name="Solider">
	<Action name="Battle.DieAction">
		<Condition name="Battle.DieCondition" />
	</Action>
	<Action name="Battle.IdelAction">
		<Condition name="Battle.NoEnemyCondition" />
	</Action>
	<Decorator name="RepeatDecorator" repeatCount="1">
		<Sequence name="Attack_A">
			<Action name="Battle.AttackAAction">
			</Action>
		</Sequence>
	</Decorator>
	<Decorator name="RepeatToSuccessDecorator" repeatCount="1">
		<Selector name="Move">
			<Sequence name="MoveToTracingTarget">
				<Action name="Battle.ChooseNewMoveTarget" />
				<Action name="Battle.MoveToTowoardTargetAttackingPlaceAction" />
			</Sequence>
		</Selector>
	</Decorator>
	<Selector name="Solider_Attacked">
		<Condition name="NOTCondition">
			<Condition name="EqualCondition" property="attackStatus" value="0" />
		</Condition>
		<Action name="Battle.BackAction">
			<Condition name="EqualCondition" property="attackStatus" value="2" />
		</Action>
	</Selector>
</Selector>
```

这段配置定义了士兵的行为，包括死亡、Idle、攻击、移动的相关决策行为和决策条件。通过结点的名称也很容易就能够推断出所要实现的功能。  

Behavior的实现


