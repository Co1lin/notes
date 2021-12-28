# Symbol Table

## 作用

收集标识符的属性信息

进行上下文语义合法性检查

在目标代码生成阶段，符号表是对符号名进行地址分配的依据

符号表需要体现：符号的作用域、与可见性信息

## 常见属性

- 名字

- 类别：常量、变量、函数、类名

- 类型：`int`, `double`, ...

- 存储类别：数据区 or 代码区；静态数据区 or 动态数据区；栈区 or 堆区

    存储分配：数据单元大小；相对基地址的偏移

- 作用域与可见性

## 实现

创建时间：语法分析同时/之后，语义检查之前

## 作用域与可见性

scopes 作用域（要么嵌套，要么不相交；不可能交错）

当前作用域

open scopes 开作用域：当前作用域与包含它的程序单元

- 开作用域内声明的符号才可以访问

close scopes 闭作用域：不属于开作用域

多符号表组织：每个作用域有各自的符号表

单符号表组织：所有嵌套的作用域共用一个全局符号表

### 单符号表组织

- 每个作用域一个作用域号
- 仅记录开作用域中的符号
- 某个作用域成为闭作用域时，从符号表中删除该作用域中所声明的符号

e.g.

```pascal
const a=25;
var x,y;
procedure p;
	var z;
	begin
	end;
procedure r; 
	var x, s; 
	procedure t;
		var v, x, y;
		begin
		end;
	begin /*here*/
	end;
begin
end.
```

<img src="symbol_table.assets/Screen%20Shot%202021-12-28%20at%209.05.53%20PM.png" alt="Screen Shot 2021-12-28 at 9.05.53 PM" style="zoom:50%;" />

- `LEV` 为最外层的层号
- `DX` 为**变量**在分配存储时，该过程存储区的基址； `ADDR` 为给变量分配的地址相对于基址的偏移
- `CX` 为**过程**活动记录中控制单元的数目； `SIZE` 为过程中局部变量的数量 + `CX`

### 多符号表组织

- **作用域栈**：每个开作用域对应栈中的一项，当前的开作用域为该栈的栈顶
- 新的作用域开放时，新符号表将被创建，并将其入栈
- 当前作用域成为闭作用域时，从栈顶弹出相应的作用域

e.g.

```pascal
const a=25;
var x,y;
procedure p;
	var z;
	begin
	end;
procedure r; 
	var x, s; 
	procedure t;
		var v;
		begin
		end;
	begin /*here*/
	end;
begin
end.
```

![Screen Shot 2021-12-28 at 9.13.35 PM](symbol_table.assets/Screen%20Shot%202021-12-28%20at%209.13.35%20PM.png)









