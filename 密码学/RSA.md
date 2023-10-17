
#rsa #密码学
## 一、RSA算法缘起



## 二、基本过程

### 1.获取公私钥

选取两个足够大且不相等的指数p,q
计算pq乘积n ```n=pq```
计算 #欧拉函数 `φ(n)=(p-1)(q-1)`
选取一个与φ(n) #互质 的整数e `1<e<φ(n)
计算出e对于φ(n)的 #模反元素 d  `de mod φ(n)=1`
获取公钥 `KU=(e,n)`
获取私钥 `KR=(d,n)`

> #欧拉函数 ：指小于n的正整数中与n互质的数的数目。
>  #互质   ：指公约数为1的两个整数，互为互质整数。
>例如 求φ(6)，小于6，有5、4、3、2、1，其中互质的只有5和1，即φ(6)=2，而质数由于只能由本身和1相乘获得，以7为例，其互质的整数有6个，即φ(7)=6=(7-1)
>
> #模反元素： 如果两个正整数e和φ(n) #互质 ，那么一定可以找到一个整数d,使得(ed -1)被φ(n)整除，或者ed除以φ(n)余数为1，此时d叫做e的 #模反元素 模范元素不止一个
> ed-1=kφ(n)
> ed mod φ(n)=1
> ed ≡ 1( mod φ(n) )
> d=d+kφ(n)
> 
> 例如 e=3 φ(n)=11,求3的模反函数d
> 3 11 
> 3* d-1=11* k 求D，欧几里得算法
>  d=(11k+1)/3 = 4(+-)11k//代表加减的意思

### 2.加密
密文m转化为对应的Unic码M
C=M^e mod n

### 3.解密
M =C^d mod n

## 三、代码实现

```c

#include<stdio.h>
#include<string.h>
#include <stdlib.h>

const int max=2e4;
int size;
int miwen[20000];//为加密后的数字密文
char mingwen[20000]; 
 
//判断两个数是否互为素数  eg:p和q e和 t 
int gcd(int p,int q)
{
	int m,n;
	if(q<p)
	{
		m=p;  p=q;  q=m;  //将p换成p和q之间那个小的数 
		m=q%p;  n=q/p;  //辗转相除法求两个数的最大公因数 
	}
	while(m!=0)
	{
		q=p; p=m;  //将p换成p和q之间那个小的数
		m=q%p;  n=q/p;		
	} 
	if(m==0&&n==q)
	{
		printf("符合条件！\n");
			return 1;
	}
	else{
		printf("不符合条件！请重新输入：\n");
	    	return 0;
	}
} 
//判断输入的p和q是不是素数 
int sushu(int s){
    int i;
	for(i=2;i<s;i++){
		if(s%i==0) 
		return 0;
	}
	return 1;
}
//求私钥d
int siyao(int e,int t)  //t:欧拉函数 
{
	int d;
	for(d=0;d<t;d++)
	    if(e * d % t==1)
	       return d;
}
//随机生成与 t互质的数e
int getrand(int p,int q)
{
	int t=(p-1)*(q-1);
	while(1)
	{
		int e=rand() % t;
		if(gcd(e,t)==1)
		return e;
	//	if(e<=2)
	//	e=3;
	}
}
void jiami(int e,int n) 
{
	//先将符号明文转换成字母所对应的ascii码。 
	char mingwen[100];    //符号明文 
	printf("请输入明文：\n");
	scanf("%s",mingwen);
	size=strlen(mingwen);
	int ming[strlen(mingwen)]; 
	int i; //定义符号明文 
	for(i=0;i<strlen(mingwen);i++)
	{
	   ming[i]=mingwen[i];        //将字母转换成对应的ascii码。 
	//printf("%d",mingwen[i]);  //将字母转换成对应的ascii码。可以不输出 
	} 
	int flag=1;
	int k=0;    //miwen为加密后的数字密文 
	for(k=0;k<strlen(mingwen);k++)
	{int j;
	    for(j=0;j<e;j++)
		{
		    flag=flag*ming[k]%n; 
	    }
	    miwen[k]=flag; 
	    flag=1;
	} 
	printf("加密密文为：\n");
	int m=0;
	for( m=0;m<strlen(mingwen);m++) 
	printf("%d",miwen[m]); 
}
void jiemi(int d,int n)
{
	int de_mingwen[size],flag=1;//解密后得到的数字明文（即ascii码） 
	char de_ming[size];//解密后得到的字符串明文 
	int i=0;
	for(;i<size;i++)
	{int j=0;
	   for(;j<d;j++)
	   {
	   	  flag=flag*miwen[i]%n;
	   }
	   de_mingwen[i]=flag; 
	   flag=1;
	} 
	printf("解密后的明文为：\n");
	int t=0;
	for(;t<size;t++)
	{
		de_ming[t]=de_mingwen[t];
		printf("%c",de_ming[t]);
	}
}
int main()
{
	int p,q,e,d,n,t,tep;
	while(1)
	{
		printf("请输入p:",p);
		scanf("%d",&p);
		tep=sushu(p);
		if(tep==0)
		{
			printf("p不是素数，请重新输入p！\n");
		    continue;
		} 
		printf("请输入q:",q);
		scanf("%d",&q);
	    tep=sushu(q);
	    if(tep==0)
		{
		printf("q不是素数，请重新输入q！\n");
		printf("请输入q:",q);
		scanf("%d",&q);
		tep=sushu(q);
		}
		int n=p*q;
		int t=(p-1)*(q-1);
		tep=gcd(p,q);
		if(tep==0)   continue;
		printf("t=(q-1)*(p-1)=%d\n",t);
		e=getrand(p,q);
		printf("公钥(e=%d n=%d)\n",e,n);
		tep=(e,t);
		d=siyao(e,t);
		printf("私钥d=%d",d);
		int a=0;
		while(a!=3)
		{
			printf("\n-------------------------\n");
			printf("1、加密\n");
	        printf("2、解密\n");
	        printf("3、退出");
	        printf("\n-------------------------\n");
	        scanf("%d",&a);
	        getchar();
	        if(a==1)
	        {
	        	jiami(e,n);
	        }
	        else if(a==2)
	        {
	        	printf("请输入密钥：");
		        scanf("%d",&d);
		        jiemi(d,n);
	        }
	        else 
			     return 0;
		}		
    }
    return 0;

}

```
