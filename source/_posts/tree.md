---
title: 二叉树的非递归遍历实现
date: 2017-07-09 18:01:59
tags: 二叉树
---
遍历一颗二叉树有多中方法，假如用D、L、R分别代表二叉树的根节点、左子树、右子树，那么要遍历一颗二叉树就有6中遍历顺序：DLR、DRL、LDR、LRD、RDL、RLD。一般在遍历时遵循先左后右的原则，因此常用的便利方法有三种：DLR(先序遍历)、LDR(中序遍历)、LRD(后序遍历)。树的定义具有递归性，因此，通常的遍历方法均采用递归方法，递归只需少量代码，便可描述出求解过程所需要的多次重复运算，大大减少了程序代码量，有效优化了代码结构。但一般不提倡使用递归，因为递归执行效率低，并且在每一层递归调用中，系统都会为局部变量和返回点开辟栈用于存储数据，递归次数过多容易造成栈溢出。所以，除非一个问题在其求解分析过程中呈现明显的递归规律，使用递归相较于栈更符合逻辑思维，否则无需追求递归。通过学习，本文下面就二叉树的三种遍历方式给出如下的非递归遍历方法。
1. 首先简单构建一个二叉树
``` bash
var tree={
	value:1,
	left:{
		value:4,
		left:{
			value:6
		}
	},
	right:{
		value:2,
		left:{
			value:9,
			left:{
				value:7
			},
			right:{
				value:8
			}
		},
		right:{
			value:3
		}
	}
```
<!--more-->
2. 前序遍历
操作思想：1.根节点入栈；2.判断栈是否为空，不为空时，弹出栈顶元素作为根节点，并输出；如果根节点的左、右子树不为空，则先后将其右子节点和左子节点入栈；3.重复操作2
代码实现：
``` bash
var preOrder=function(node){
	if(!node){
		throw new Error('Empty Tree'); 
	}
	var stack=[];
	stack.push(node);
	while(stack.length!==0){
		node=stack.pop(); 
		console.log(node.value);
		if(node.right){
			stack.push(node.right);
		}
		if(node.left){
			stack.push(node.left);
		}
	}
}
```
3. 中序遍历
操作思想：当根节点存在或栈不为空成立时，如果根节点存在，则将根节点入栈，并将该根节点的左子节点作为新的根节点；否则弹栈栈顶元素为根节点，输出根节点并将根节点的右子节点作为新的根节点
代码实现：
``` bash
var inOrder=function(node){
	if(!node){  
		throw new Error('Empty Tree');
	}
	var stack=[];
	while(stack.length!== 0||node){
		if(node){
			stack.push(node);
			node=node.left;
		}else{
			node=stack.pop();
			console.log(node.value);
			node=node.right;
		}
	}
}
4. 后序遍历
操作思想: 1.将根节点入栈；2。判断栈是否为空，如果栈为空，则遍历结束；如果栈不为空，弹出栈顶元素作为临时元素；并进行如下判断：a.如果临时元素的左子节点存在且其左右子节点均不与根节点相同，则将临时节点的左子节点入栈；b.如果临时节点的右子节点存在且不与根节点相同，则将临时节点的右子节点入栈；c.否则，弹出栈顶元素，并且将临时元素作为新的根节点。3，重复2
代码实现：
``` bash
var posOrder=function(node){
	if(!node){
		throw new Error('Empty Tree');
	}
	var stack=[];
	stack.push(node);
	var tmp=null;
	while(stack.length!=0){
		tmp=stack[stack.length-1];
		if(tmp.left&&node!==tmp.left&&node!==tmp.right){
			stack.push(tmp.left);
		}else if(tmp.right&&node!==tmp.right){
			stack.push(tmp.right);
		}else{
			console.log(stack.pop().value);
			node=tmp;
		}
	}
}
```