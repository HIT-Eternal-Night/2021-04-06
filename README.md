# 2021-04-06
类型定义、联合、阿姆斯特朗数

1、自定义数据类型-->typedef
C语言提供了一个叫做typedef的功能来声明一个已有的数据类型的新名字。
比如： typedef int Length;
这就使得Length 成为了 int 的别名。
Length 这个名字就可以代替 int 出现在变量定义和参数声明的地方：
Length a,b,len;
length numbers[10];

·这种做法改善了程序的可读性
typedef long int64_t; //重载已有的类型名字，新名字的含义更清晰具有可移植性
typedef struct ADate {
  int month;
  int day;
  int year;
} Date;       //简化了复杂的名字

2、联合：
union Anelt {
	int i;
	char c;
} elt1,elt2;
elt1.i = 4;		
elt2.c = 'a';	
elt2.i = 0XDEADBEEF;
说明：所有的成员共享一个空间，同一时间只有一个成员是有效的。union的大小是其最大的成员，初始化是对第一个成员做的。

在下述代码的情况下我们用union。
#include<stdio.h>

typedef union {
	int i;
	char ch[sizeof(int)];
} CHI;
 
int main(int argc,char const *argv[])
{
	CHI chi;
	int i;
	chi.i = 1234;
	for (i=0 ; i<sizeof(int) ; i++)
	{
		printf("%02hhX",chi.ch[i]);
	}
	printf("\n");
	
	return 0;
}
输出结果：D2040000

我们可以用这种方式得到一个整数（double、float也可以）内部的各个字节。
当我们要做文件操作的时候，当我们要把一个整数以二进制的形式写到文件里的时候，就可以用union。

3、阿姆斯特朗数
水仙花数（Narcissistic number）也被称为超完全数字不变数（pluperfect digital invariant, PPDI）、
自恋数、自幂数、阿姆斯壮数或阿姆斯特朗数（Armstrong number）

水仙花数是指一个 3 位数，它的每个位上的数字的 3次幂之和等于它本身（例如：1^3 + 5^3+ 3^3 = 153）

水仙花数只是自幂数的一种，严格来说3位数的3次幂数才称为水仙花数。
附：其他位数的自幂数名字
一位自幂数：独身数
两位自幂数：没有
三位自幂数：水仙花数
四位自幂数：四叶玫瑰数
五位自幂数：五角星数
六位自幂数：六合数
七位自幂数：北斗七星数
八位自幂数：八仙数
九位自幂数：九九重阳数
十位自幂数：十全十美数

常见水仙花数：
水仙花数又称阿姆斯特朗数。
三位的水仙花数共有4个：153，370，371，407；
四位的四叶玫瑰数共有3个：1634，8208，9474；
五位的五角星数共有3个：54748，92727，93084；
六位的六合数只有1个：548834；
七位的北斗七星数共有4个：1741725，4210818，9800817，9926315；
八位的八仙数共有3个：24678050，24678051，88593477
……

C语言速度比拼：
本题要点：
一、通过输入的位数计算遍历起点
二、遍历10^n-->10^(n+1)-1
三、获取每一位的值并乘方（先取余后除法没什么好说的）
四、数组存储0-9的乘方简化计算（重点！！！）

成果展示：
#include<stdio.h>
#define LEN 10 //存放0-9的n次幂的数组长度 

int Exponentiation(int x,int n);

int main(int argc,char const *argv[])
{
	int n;
	scanf("%d",&n);
	//计算遍历起点 
	int i;
	int begin = 1;
	for (i=1 ; i<n ; i++)
	{
		begin *= 10;
	}
	//存放0-9的n次幂的数组
	int a[LEN] = {0};
	for (i=0 ; i<LEN ; i++)
	{
		a[i] = Exponentiation(i,n);
	}
	//遍历10^n-->10^(n+1)-1 
	int armstrong;
	for (armstrong = begin ; armstrong < begin*10 ; armstrong++)
	{
		int temp = armstrong;
		int sum  = 0;
		int d; //取末位的变量
		
		do{
			d = temp % 10;
			temp /= 10;
			sum += a[d];
		} while (temp > 0);
		
		if (sum == armstrong)
		{
			printf("%d\n",armstrong);
		}
	}
	
	return 0;
}
//函数功能：计算x的n次幂 
int Exponentiation(int x,int n)
{
	int i;
	int y = x;
	for (i=1 ; i<n ; i++)
	{
		y *= x;	
	} 
	return y;
}
