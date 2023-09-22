# C++: switch case语句

> 好消息！好消息！python3.10要引入switch-case了！

类似 `if-else` 语句，`switch-case` 语句用于处理复杂的条件判断和分支操作，但相较前者有更好的可读性，在代码中出现冗长的 `if-else` 阶梯代码时，`switch-case` 语句可作为一个不错的替代方案。

## 基础结构

一个 **switch** 语句可以包含任意数量的 **case** 标签，每个 **case** 标签中可执行若干条语句，通常以 **break** 语句结束。**default** 标签为可选项，至多包含一个，用于处理 **case** 标签未列举的值。

```cpp
switch (expression)
{
	case constant_expression_1 :
		// statement_1
		break;
	case constant_expression_2 :
		// statement_2
		break;
		/* ... */
	
	default:
		// statement_default
		break;
}

```

Q:**为什么必须加break?**

## switch-case 流程图

它求值表达式或变量的值（基于`switch`括号内给出的内容），然后根据结果执行相应的情况。

![](https://img.geek-docs.com/cpp/cpp-tutorial/4f4a0032c3c6f26d1bc5a76c8a08546f.jpg)