# GGG1235

#include<stdio.h>
#include<string.h>

FILE *fp;				/*宣告档案*/

struct member			/*宣告结构*/
{
    int  num;
	int ID;
	long int password;
	long int money; 
} ;

int q,n;				/*q:数据库内数据数,n:执行程序中第几笔数据*/
int readfile(struct member guide[]);		/*读档*/
int writefile(struct member guide[]);		/*写档*/
struct member guide[100];
check_id(int ID);				/*寻找数据库是否有此账号ID子程序*/
check_PW(long int PW);			/*确认ID密码是否正确子程序*/
void withdraw();

void main()
{
	long int PW=0;
	int a,i;					/*a:确认数据库账号和密码传回之值*/
	int id;
	int l;

	fp=fopen("member2.txt","r");
	if((fp=fopen("member2.txt","r"))==NULL)		/*看是否能开启档案*/
	{
		printf("can not open the file");	/*不行的话就显示"can not open the file" */
	} 
	q=readfile(guide);							/*呼叫读文件子程序*/
	
	fclose(fp);						/*关档*/

	printf("**********************************\n");
	printf("      欢迎光临此提款机\n");
	printf("**********************************\n");
	for(n=1;n<=3;n++)
	{
		printf("请输入提款卡ID:");
		scanf("%d",&id);
		a=check_id(id);				/*确认数据库是否有此ID子程序*/
		if(a==1)
		break;
		else 
		printf("ID输入错误\n");
	}

    for(i=1;i<=3;i++)
	{
		printf("请输入密码:");
		scanf("%d",&PW);
		a=check_PW(PW);				/*确认账号密码子程序*/
		if(a==1)
		break;
		else
		printf("密码输入错误\n");
		if(i==3)
		{	
			printf("你的卡已经被怪兽吃掉啦!!\n");/*输入密码错误三次*/
			printf("请带100万元向OSAKADO赎回");
			goto end;				/*中断程序*/
		}
	}
	
	withdraw();						/*选择服务项目*/

	writefile(guide);				/*呼叫写文件子程序*/
	
	end:
			printf("\n");
	scanf("%d",&p);
}

int readfile(struct member guide[])
{
	int i=0;
    
	int fileend=0;						/*fileend  宣告侦测是否已到档案结尾*/
	if((fp=fopen("member2.txt","r"))==NULL)		/*看是否能开启档案*/
	{
		printf("can not open the file");	//不行的话就显示
											//"can not open the file" 
	} 
	while(fileend==feof(fp))			 /*feof函数检查是否已到档案结尾，*/
									/*若不是则传回0，若是则传回非0的值*/
	{
		i=i+1;
		fscanf(fp,"%d",&guide[i].num); /*读取档案数据的num*/
        fscanf(fp,"%d",&guide[i].ID);/*读取档案数据的ID*/
		fscanf(fp,"%ld",&guide[i].password);/*读取档案数据的password*/
		fscanf(fp,"%ld",&guide[i].money);/*读取档案数据的money*/

	}
	return i;
}
check_id(int id)	/*子程序:确认数据库内账号是否存在*/
{
	for(n=1;n<=q-1;n++)
	{
		if(id==guide[n].ID)  /*账号ID存在传回值1*/
			return 1;
	}
	return 0;				 /*账号ID不存在传回值0*/
}

check_PW(long int PW)/*子程序;确认ID密码*/
{

	 
		if(PW==guide[n].password)
			return 1;			/*密码正确传回值1*/
	
	return 0;					/*密码不正确传回值0*/
}

void withdraw()					/*服务项目子程序*/
{
	long int x;	/*x表示提款的金额数目*/
	int T;		/*T表示提款类别*/

	printf("\n1.存簿提款\n");
	printf("2.跨行提款\n");
	printf("3.查询结余\n");

	printf("请输入欲使用的服务类别(1~3):");
	scanf("%d",&T);

	if(T>=1&&T<=3)				/*选择项目*/
	{
		switch(T)
		{
		case 1:
			printf("请输入欲提款金额:");
			scanf("%ld",&x);
			if(x<=guide[n].money)
			{
				guide[n].money=guide[n].money-x;
			}
			else
				printf("输入金额错误,余额不足\n");
			break;

		case 2:
			printf("请输入欲提款金额:");
			scanf("%ld",&x);
			if(x<=guide[n].money-6)
			{
				guide[n].money=guide[n].money-x-6;	/*跨行手续费6元*/
			}
			else
				printf("输入金额错误,余额不足\n");
			break;
		case 3:
			x=0;
			break;
		}
	}

	printf("本次提款金额:%d元整\n",x);
	printf("您的余额还有%ld元整\n",guide[n].money);
	printf("谢谢您的使用，欢迎再度利用本局提款。");
}
int writefile(struct member guide[])				/*写文件子程序*/
{
	int i;
	 fp=fopen("member2.txt","w");                   /*开启member2.txt檔*/
	 for(i=1;i<=q-1;i++)
	 {
			
		fprintf(fp,"%d\t",guide[i].num);    			/*将矩阵的num写到档案*/
        fprintf(fp,"%d\t",guide[i].ID); 				/*将矩阵的ID写到档案*/
		fprintf(fp,"%ld\t",guide[i].password);		/*将矩阵的password写到档案*/
        fprintf(fp,"%ld\t",guide[i].money);		/*将矩阵的money写到档案*/
	 fclose(fp);									/*关闭档案*/
}
