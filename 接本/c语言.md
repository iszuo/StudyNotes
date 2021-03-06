# 整型变量

## 整型变量分类

1. 基本型
   类型说明符为int，在内存中占4个字节
2. 短整量
   类型说明符为short int或short。所占字节和取值范围均与基本型相同。
3. 长整型
   类型说明符为long int或long。在内存中占4个字节。
4. 无符号型
   类型说明符为unsigned

------



# 实型数据

## 实型常量表示方法

- 十进制数：由0~9组成
  例如：20、30、20.5、123.321

- 指数形式：由十进制数，加阶码标志“e”或“E”以及阶码（只能为整数，可以带符号）组成。

  aEn（a为十进制数，n为十进制整数）

```
如：
	2.1E5(等于2.1*105)
	3.7E-2(等于3.7*10-2)
	0.5E7(等于0.5*197)
	-2.8E-2(等于-2.8*10-2)
不合法：
	345(无小数点)
	E7(阶码标志E之前无数字)
	-5(无阶码标志)
	53.-E3(负号位置不对)
	2.7E(无阶码)
注：标准C允许浮点数使用后缀。后缀为“f”或“F”即表示该数为浮点数。如356f和356.是等价的。
```

## 实行变量的分类

- 实行变量分为：单精度（float型）、双精度（double型）和长双精度（long double型）三类。

------

# 强制类型转换

一般形式为：（类型说明符）（表达式）
把表达式的运算结果强制转换为类型说明符所表示的类型。
例：

```c
(float)a		//把a转换为实型
(int)(x+y)		//把x+y的和转换为整型 
```

------

# 算数运算符和算数表达式



| **基本的运算符** |      |
| :--------------- | ---- |
| 加法运算符       | +    |
| 减法运算符       | -    |
| 乘法运算符       | *    |
| 除法运算符       | /    |

注：除法运算符“/”：参与运管量均为整型时，结果也为整型，舍去小数（退一法，不采用四舍五入）。如果运算量中有一个是实型，结果为双精度实型。



```c
#include <stdio.h>
void main(){
	printf("小数"); 
	printf("%d",5/3);
}
//  输出  1
```

​		%：对结果求余数。

```c
#include <stdio.h>
void main(){
	printf("百分号运算    7/2=%d……%d",7/2,7%2); 
}
//  输出  “百分号运算 7/2=3……1”
```

------

# 自增、自减运算符

| 自增、自减 |                                     |
| ---------- | ----------------------------------- |
| i++   i- - | i先自增或自减后，再参与其他运算     |
| ++i   - -i | i先参与其他运算后，再自增或自减后再 |

```c
#include <stdio.h>
void main(){
	int i = 8;
	printf("%d\n",i++);      // 先输出i=8，运算完后i=9
	printf("%d",--i);		// 先运算9-1，
}
// 输出结果为   8  8
```

# 逗号运算符和逗号表达式

## 一般形式

其一般形式为：表达式1，表达式2
其求值过程是分别求两个表达是的值，并以表达式2的值作为整个逗号表达式的值。

```c
#include <stdio.h>
void main(){
	int a=2,b=4,c=6,x,y;
	y=(x=a+b),(b+c);		//  x=a+b 为表达式2
	printf("y=%d,x=%d",y,x); 
}
//  输出结果   y=6,x=6
// 其中(x=a+b)为表达式2      以表达式2的值分别为表达式1和表达式2的值
```

## 嵌套

1. 逗号表达式一殷形式中的表达式1和表达式2也可以又是逗号表达式。

   ```
   例如：
   		表达式1，(表达式2，表达式3，…表达式n)，表达式n+1
   	形成了嵌套情形。因此可以把逗号表达式扩展为以下形式：
   		表达式1，表达式2，……，表达式n
   	整个逗号表达式的值等于表达式n的值。
   ```

   

2. 程序中使用逗号表达式，通常是要分别求逗号表达式内各表达式的值，并不一定要求整个逗号表达式的值。并不是在所有出现逗号的地方都组成逗号表达式，如在变量说明中，函数参数表中逗号只是用作名变量之间的间隔符。

------

# c基本语句

## 1、表达式语句

- 表达式语句由表达式加上分号 “ ; ” 组成。
- 一般形式为：表达式；

- ```c
  例如：
  	x = y + z；		// 赋值语句
  	y + z；			// 加法运算语句，但计算结果不能保留，无意义；
  	i++；			// 自增自减
  ```




## 2、函数调用语句

- 由函数名、实际参数加上分号“；” 组成；
  一般形式为：函数名（实际参数表）；

- ```c
  例如：
  	printf("C Program");	// 调用库函数，输出字符串。
  ```



## 3、控制语句

1. 条件判断的语句
   if语句、switch语句；
2. 循环执行语句
   do while语句、while语句、for语句；
3. 转向语句
   break语句、goto语句、continue语句、return语句；

## 4、复合语句

- 把多个语向用括号  {}  括起来组成的一个语向称复合语句。

- ```c
  例如：
  	{
  		x = y + z;
  		a = b + c;
  		printf("%d%d", x, a);
  	}
  ```

- 注：复合语句内的各条语句都必须以封号 “;” 结尾，在括号 “}” 外不能加分号。

## 5、空语句

- 只有分号 “;” 组成的语句称为空语句。
  空语句是什么也不执行的语句，在程序中空语句用来作空循环体。

- ```c
  例如：
  		while(getchar() != '\n')
  	{
  		;
  	}
  // 本语句的功能是，只要从键盘输入的字符不是回车则重新输入。
  // 这里的循环体为空语句。
  ```

------

# putchar ()与getchar()

- putchar()   输出一个字符

- getchar()   获取键盘上输入的一个字符

- ```c
  例：
  #include <stdio.h>
  void main(){
  	char c;
  	printf("输入一个字符\n");		// 输入  a	
  	c = getchar();				// 变量c = a
  	printf("输出一个字符\n");
  	putchar(c);					// 将变量c的值输出   c
  } 
  ```

------

# printf( )的格式字符

## 1、%d格式符　

- 用来输出十进制整数
- %d 按整型数据的实际长度输出
- %md 使输出长度为m，如果数据长度小于m，则左补空格，如果大于m，则输出实际长度
- %ld 输出长整型数据

##  2、%o格式符　

- 以八进制形式输出整数

## 3、%x、X格式符　

- 以十六进制形式输出无符号整数

## 4、%u格式符　

- 用来输出unsigned型数据，以十进制形式输出

## 5、%c格式符　

- 用来输出一个字符

## 6、%s格式符　

- 输出一个字符串
- %s输出实际长度字符串
- %ms 输出的串占m列，如果串长度小于m，左补空格，如果大于m，实际输出
- %-ms输出的串占m列，如果串长度小于m，右补空格，
- %m.ns 输出占m列，但只取字符串中左端n个字符并靠右对齐
- %-m.ns m、n含义同上，靠左对齐，如果n>m，则m自动取n值

## 7、%f格式符　

- 以小数形式输出实数
- %f 整数部分全部输出，小数部分输出6位
- %m.nf 输出数据共占m列，其中有n位小数。如果数值长度小于m，左补空格
- %-m.nf 同上，右补空格

## 8、%e、E格式符　

- 以指数形式输出单、双精度实数
- %e 系统指定６位小数，5位指数(e+002 )

## 9、%g、G格式符　

- 以%f或%e中较短的输出宽度输出单、双精度实数
- 输出实数，根据数值大小，自动选f格式或e格式

```c
例
#include <stdio.h>
void main(){
	int a = 15;
	float b = 123.1234567;
	double c = 123456789.1234567;
	char d = 'p';
	printf("a=%d,%5d,%o,%x\n",a,a,a,a);
	printf("b=%f,%lf,%5.4lf,%e\n",b,b,b,b);
	printf("c=%lf,%f,%8.4lf\n",c,c,c);
	printf("d=%c,%8\n",d,d); 
} 
// a=15,   15,17,f
// b=123.123459,123.123459,123.1235,1.231235e+002
// c=123456789.123457,123456789.123457,123456789.1235
// d=p，
```

------

# 格式输入与输出

## 1、“ * ” 符号

- 用以表示该输入项，读入后不赋予相应的变量，即跳过该输入值。

- ```c
  如
  scanf_s("%d %*d %d",&a,&b);
  // 当输入为: 123时，把1赋予a，2被跳过，3赋予b。
  ```

## 2、宽度

- 用十进制整数指定输入的宽度（即字符数）。

- ```c
  如
  	scanf_s("%5d",&a);			// 输入12345678
  	// 只输出12345赋予变量a，其余部分被截取。
  	
  	scanf_s("%4d%4d",&a,&b);	// 输入12345678
  	// 将1234赋予a   	而把5678赋予b
  	
  ```

## 3、长度

- 长度格式符为l和h，I表示输入长整型数据(如%ld)和双精度浮数(如%If)。
  h表示输入短整型数据。

## 4、注意事项

1. 在输入多个数值数据时，若格式控制串中没有非格式字符作输入数据之间的间隔则可用空格、TAB或回车作间隔。C编译在碰到空格、TAB、回车或非法数据(如对“%d”输入“12a”时，a即为非法数据）时即认为该数据结束。

2. 在输入字符数据时，若格式控制串中无非格式字符，认为所有输入的字符均为有效字符。

   ```c
   如：
   scanf_s("%c%c%c",&a,&b,&c);			
   // 输入 d e f
   // 则把'd'赋予a,' 空格符 '赋予b.'e'赋予c。
   ```

   只有当输入为:def时，才能把'd'赋于a，'e'嘘予b，'f'赋予c。
   如果在格式控制中加入空格作为间隔，

   ```c
   如：
   scanf_s("%c %c %c",&a,&b,&c);
   // 则输入时各数据之间可加空格。
   ```


------

# 关系表达式

在c语言中的关系表达式的“真”和“假”，用“1”和“0”表示
（注：用非0的数字表示真）

```c
例：
i = (5 > 0);
// i = 1;
```

------

# 逻辑运算符

| 符号 | 含义   |
| ---- | ------ |
| &&   | 与运算 |
| \|\| | 或运算 |
| ！   | 非运算 |

```c
// 在c语言中用非0的数字表示真
int i;
i == 5;		// i=5此时i为真
!i;			// 非i，取反i变为假
i = 0;		// c语言中用0表示假
```

------

# if语句

1. 第一种
   if（表达式1）{
   	语句1
   }

2. 第二种
   if（表达式1）{

   ​	语句1
   }else{

   ​	语句2
   }

3. 第三种

   if（表达式1）{

   ​	语句1
   }else if（表达式2）{

   ​	语句2
   }……if（表达式n）{

   ​	语句n

   }else{

   ​	语句n+1
   }

