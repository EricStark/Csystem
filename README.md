## Welcome to Eric's GitHub Pages
## Welcome to Eric's GitHub Pages

#include<stdio.h>//宏定义以及函数的输入输出头文件
#include<string.h>//对象的动态申请strlen头文件
#include<windows.h>//系统操作函数头文件
#include<conio.h>//控制台函数头文件
#include<stdlib.h>//malloc,free，system，exit函数头文件
#define N sizeof(struct BOOK)//宏定义
int sum;
typedef struct BOOK        //图书信息
{
	char log_number[10];   //登录号
	char name[10];     //书名
	char author[10];    //作者名
	char book_type[10];      //类型
	char publisher[10];  //出版单位
	char putout_time[8];        //出版时间
	float price;       //价格
	int num;         //数量
	int x;
	struct BOOK *next;//指针域
}book,*pNode;
pNode pnew;
pNode CreateList();//创建链表
void TraverseList(pNode pHead);
void HideCursor();    //隐藏光标
void coordinate(int x, int y);   //将光标移动到X,Y坐标处
void color(int x);     //设置颜色
void out_system();             //退出
void menu();           //菜单
void input_book();     //图书入库
void save_book(pNode);//将图书信息存入文件
void find_book();      //查询
void print_book();    //图书总览
void del_book();     //删除图书
void amend_book();    //修改信息
void find_name_book();  //按书名查询
void find_author_book(); //按作者查询
void find_lognumber_book();  //按登录号查询
void find_publisher_book();  //按出版社查询

int main()   //简洁明了的主函数
{

    pNode pHead = NULL;
    pNode CreateList();
    //printf("成功");
	menu();//菜单
}

void TraverseList(pNode pHead)
{
    int i=7;
    int sum=0;
    pNode p = CreateList();
    while(p->next!= NULL)
    {
        p = p->next;
        coordinate(0, i);
        printf("  %5s    %5s     %5s       %5s      %5s         %5.2f %5d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
        sum+=p->num;
        i++;
    }
    coordinate(2, i+2);
    printf("图书总量为：%d",sum);
}
pNode CreateList()
{
    pNode pHead = (pNode)malloc(sizeof(book));
    pNode pTail = pHead;
    pTail->next = NULL;
    FILE*fp = fopen("book.txt","rb");
    while(!feof(fp))
    {
        pNode pNew = (pNode)malloc(sizeof(book));
        fscanf(fp,"%s %s %s %s %s %f %d",pNew->log_number,pNew->name,pNew->author,pNew->publisher,pNew->putout_time,&pNew->price,&pNew->num);
        pTail->next = pNew;
        pNew->next = NULL;
        pTail = pNew;
    }
    return pHead;

}

void HideCursor()     //隐藏光标
{
 CONSOLE_CURSOR_INFO cursor_info = {1, 0};
 SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
}

void color(int x)
{
	//if(x>=0&&x<=15)
	//{
		//SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),x);
	//}
	//else
	//{
		SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),x);
	//}
}

void coordinate(int x, int y)      //将光标移动到X,Y坐标处
{
COORD pos = { x , y };
HANDLE Out = GetStdHandle(STD_OUTPUT_HANDLE);
SetConsoleCursorPosition(Out, pos);
}

void menu()    //菜单
{

		system("cls");  //清屏
		HideCursor();  //隐藏光标
		color(10);    //设置一个好看的颜色
		char t,m;
		coordinate(20,5);//将光标移动到（50，5）坐标处
		printf(" ~欢迎来到优质图书查询管理系统~");
		coordinate(30,8);
		printf("1.-学生入口-");
		coordinate(30,12);
		printf("2.-管理员入口-");
		coordinate(23,16);
		printf("请按照操作编号进行操作选择：");
		printf("\n");
		m = getch();
		switch(m){
        case'1':{
            system("cls");  //清屏
		    HideCursor();  //隐藏光标
		    color(1);
		    coordinate(23,6);
		    printf("|     1.-图书查询-      |");
		    coordinate(23,10);
		    printf("|     2.-图书总览-      |");
		    coordinate(23,14);
		    printf("|     3.-退出软件-      |");
		    coordinate(27,20);
		    printf("请输入你的操作编号:");
		    m = getch();
		    switch(m){
		    case '1':
			    find_book();
                break;
			case '2':
			    print_book();
                break;
			case '3':
			    out_system();
                break;
			default :
                printf("请输入正确指令:1 2 3");
                break;
		    }
            break;
        }

        case'2':{
            system("cls");  //清屏
		    HideCursor();  //隐藏光标
		    color(3);
           coordinate(23,4);
		   printf("|     1.-图书入库-      |");
		   coordinate(23,8);
		   printf("|     2.-修改信息-      |");
		   coordinate(23,12);
		   printf("|     3.-删除信息-      |");
		   coordinate(23,16);
		   printf("|     4.-退出软件-      |");
		   coordinate(27,18);
		   printf("请输入你的操作编号：");
		   t=getch();    //不回显函数，输入字符时自动读取，不会显示键入字符，哈哈HA~~~
		switch(t)
		{
			case '1':
			    input_book();
                break;
			case '2':
			    amend_book();
                break;
			case '3':
			    del_book();
                break;
            case '4':
			    out_system();
                break;

            default:{
                coordinate(20,18);
                printf("请输入正确的指令:1 或2 或3 或4");
                break;}
		   }
           break;

        default:
            break;

		}
    }
}

 void input_book()    //图书录入
{
    while(1){
		system("cls");
		color(10);
		char t;
        pNode p = CreateList();
		//while(p->next!=NULL){
           // p = p->next;
		//}
		pNode pnew=(pNode)malloc(sizeof(book));//申请空间

        p->next = pnew;
        pnew->next = NULL;
        p = pnew;
		//输入图书信息
		coordinate(1,1);
		printf("请输入图书登录号(小于10位数)：");
		scanf("%s",&pnew->log_number);
		getchar();
		coordinate(1,3);
		printf("请输入书名(小于10位数)：");
		scanf("%s",&pnew->name);
		getchar();
		coordinate(1,5);
		printf("请输入作者名(小于10位数)：");
		scanf("%s",&pnew->author);
		getchar();
		coordinate(1,7);
		printf("请输入图书出版单位(小于10位数)：");
		scanf("%s",&pnew->publisher);
		getchar();
		coordinate(1,9);
		printf("请输入图书出版时间(小于8位数)：");
		scanf("%s",&pnew->putout_time);
		getchar();
		coordinate(1,11);
		printf("请输入图书价格：");
		scanf("%f",&pnew->price);
		getchar();
		coordinate(1,13);
		printf("请输入图书数量：");
		scanf("%d",&pnew->num);
		save_book(pnew);
		coordinate(1,15);
		printf("正在快速保存中....");
		Sleep(2000);   //暂停1秒
		system("cls");
		coordinate(23,5);
		printf("-------------------------");
		coordinate(23,6);
		printf("|                       |");
		coordinate(23,7);
		printf("| 保存成功！是否继续？  |");
		coordinate(23,9);
		printf("| 1.是           2.否   |");
		coordinate(23,10);
		printf("|                       |");
		coordinate(23,11);
		printf("-------------------------");
while(1){
        t=getch();
			if(t=='1')
			{
				break;
			 }
			 else if(t=='2')
			 {
			 	menu();
			 }
        }
    }
}

void amend_book()    //修改图书信息
{

		system("cls");
		color(10);
		int i=8,j=0,x;

		char ch,t;
		FILE *fp; //文件指针
		char C_name[10];
		char log_number[10];   //登录号
		char name[10];     //书名
		char author[10];    //作者名
		char publisher[10];  //出版单位
		char putout_time[8];        //出版时间
		float price;       //价格
		int num;         //数量
		pNode p = CreateList();
		pNode head = p;
		coordinate(1,1);
		printf("请输入你要修改的图书的书名：");
		gets(C_name);
		while(p->next!=NULL)    //初始化p->x为0
		{
			p->x=0;
			p=p->next;
		}
		p=head;//让p重新指向表头
		coordinate(1,3);
		printf("******************************图书信息***********************************");
		coordinate(1,5);
		printf("-------------------------------------------------------------------------");
		coordinate(1,6);
		printf("登录号     书名      作者名      出版单位      出版时间     价格    数量");
		coordinate(1,7);
		printf("--------------------------------------------------------------------------");
		p=p->next;
		while(p!=NULL)
		{
			if(strcmp(p->name,C_name)==0)
			{
				coordinate(1,i);
				++j;
				printf("%d   %s       %s          %s         %5s         %4s       %.2f        %d",j,p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
				p->x=j;    //给符合查询标准的结点标号
				i++;
			}
			p=p->next;
		}
		if(j==0){
            coordinate(1,i);
			printf("没有找到相应的图书信息！(按0返回，按1重新搜索)");

                ch=getch();
				switch(ch)
				{
				    case'0':
					    menu();
					case'1':
					    amend_book();
					default:
                        break;
				}
		}

         while(1)              //利用死循环重复操作。
		       {
			coordinate(1,i+2);
			printf("请输入您要修改的图书的编号：");
			scanf("%d",&x);
			getchar();
			if(x>j||x==0)
			{
				coordinate(1,++i);
				printf("输入错误，请重新输入!");
				Sleep(2000);
			}
			else
			{
				break;
			}
		}


        p=head;//让p重新指向表头
        p=p->next;
		while(p!=NULL&&p->x!=x)   //遍历链表查询符合条件的结点
		{
			p=p->next;
		}
		if(p)    //如果p不为空
		{
			system("cls");
			//输入要修改的信息
			coordinate(1,1);
			printf("请输入图书登录号(小于10位数)：");
			scanf("%s",log_number);
			getchar();
			strcpy(p->log_number,log_number);
			coordinate(1,3);
			printf("请输入书名(小于10位数)：");
			scanf("%s",name);
			getchar();
			strcpy(p->name,name);
			coordinate(1,5);
			printf("请输入作者名(小于10位数)：");
			scanf("%s",author);
			getchar();
			strcpy(p->author,author);
			coordinate(1,7);
			printf("请输入图书出版单位(小于10位数)：");
			scanf("%s",publisher);
			getchar();
			strcpy(p->publisher,publisher);
			coordinate(1,9);
			printf("请输入图书出版时间(小于8位数)：");
			scanf("%s",putout_time);
			getchar();
			strcpy(p->putout_time,putout_time);
			coordinate(1,11);
			printf("请输入图书价格：");
			scanf("%f",&price);
			getchar();
			p->price=price;
			coordinate(1,13);
			printf("请输入图书数量：");
			scanf("%d",&num);
			getchar();
			p->num=num;
		}
		color(7);
		coordinate(46,8);
		printf("-------------------------");
		coordinate(46,9);
		printf("|                       |");
		coordinate(46,10);
		printf("|     是否确认修改？    |");
		coordinate(46,12);
		printf("| 1.是             2.否 |");
		coordinate(46,13);
		printf("|                       |");
		coordinate(46,14);
		printf("-------------------------");
		while(1)   //利用死循环防止其他按键干扰
		{
			t=getch();
			if(t=='1')
			{
				break;
			}
			else if(t=='2')
			{
				menu();
			}
		}
		system("cls");
		coordinate(23,10);
		printf("正在修改，请稍后....");
		Sleep(2000);
		fp=fopen("book.txt","wb");   //!以只写的方式打开名为mybook的二进制文件，打开的同时清空文件中的内容--实际上是多余的
		if(fp==NULL)
		{
			printf("不能打开文件！");
		}
		else  //将head写入fp所指向的文件中
		{
		coordinate(1,14);
		printf("这是你选择修改后书的内容:");
		    printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
		    getch();
		}
		fclose(fp);//关闭文件
		p = head;
		p=p->next;
		if(p!=NULL)
		{
			//p=p->next;     //让p指向第二个结点
			fp=fopen("book.txt","ab");   //以追加的方式打开文件
			if(fp==NULL)
			{
				printf("不能打开文件！");
			}
			while(p!=NULL)
			{
			   // printf("%d %s %s %s %s %s %.2f %d",j,p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
				fprintf(fp,"%s %s %s %s %s %.2f %d\r\n",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
				p=p->next;
			}
			fclose(fp);  //关闭文件
		}
		Sleep(2000);   //暂停2秒
		system("cls");
		coordinate(2,5);
		printf("修改成功！即将自动返回主菜单....");
		Sleep(2000);
		menu();
}

void del_book()   //删除信息
{
	while(1)
	{
		system("cls");
		color(9);
		FILE *fp;
		pNode pre,pn;
		int j=0,x,i=11;
		char name[10];
		char t,c,ch;
		pNode p = CreateList();
		pNode head = p;
		pre = p;
		coordinate(1,1);
		printf("请输入你要删除的图书的书名：");
		scanf("%s",name);
		p = p->next;
		while(p!=NULL){
            p->x=0;
			p=p->next;
		}

		p=head;
		coordinate(1,3);
		printf("*******************************图书信息************************************");
		coordinate(1,5);
		printf("---------------------------------------------------------------------------");
		coordinate(1,7);
		printf("登录号     书名     作者名     出版单位      出版时间      价格       数量");
		coordinate(1,9);
		printf("----------------------------------------------------------------------------");
		p= p->next;
		while(p!=NULL)
		{
			if(strcmp(p->name,name)==0)
			{
				coordinate(1,i);
				j++;
				printf("%d   %s       %s          %s         %5s         %4s       %.2f        %d",j,p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
				p->x=j;
				i++;
			}
			p=p->next;
		}
		if(j==0)                   //如果j=0，即没有进入前面的搜索循环，也就是没有找到相应的信息
		{
			coordinate(1,i+1);
			printf("没有找到相应的图书信息！(按0返回，按1重新搜索)");

                ch=getch();
				switch(ch)
				{
				    case'0':
					    menu();
					case'1':
					    del_book();
					default:
                        break;
				}
	}

		while(1){
			coordinate(1,i+3);
			printf("请输入您要删除的图书的编号：");
			scanf("%d",&x);
			getchar();
			if(x>j||x==0)
			{
				coordinate(1,++i);
				printf("输入错误，请重新输入!");
				Sleep(2000);
			}
			else
			{
				break;
			}
		}
		color(7);
		coordinate(46,10);
		printf("-------------------------");
		coordinate(46,11);
        printf("|     是否确认修改？    |");
		coordinate(46,13);
		printf("| 1.是             2.否 |");
		coordinate(46,14);
		printf("-------------------------");
		while(1)
		{
			t=getch();
			if(t=='1')
			{
				break;
			}
			else if(t=='2')
			{
				menu();
			}
		}
		p=head;
		p = p->next;
		while(p!=NULL&&p->x!=x)//找
		{
            pre=p;
			p=p->next;
		}

		if(p->next!=NULL)//!删除的实际操作
		{
			pre->next= p->next;
			free(p);
		}
        else
			{
				pre->next = NULL;
				free(p);
			}

		fp=fopen("book.txt","wb");
		if(fp==NULL)
		{
			printf("不能打开文件夹");
		}
		else
		{
		    system("cls");
		    printf("请等待....");
		    //printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
		    Sleep(2000);
		}
		fclose(fp);



		pn = head;
		pn = pn->next;
		if(pn!=NULL)
		{
			//p=p->next;
			fp=fopen("book.txt","ab");
			if(fp==NULL)
			{
				printf("不能打开文件！");
			}
			while(pn!=NULL)
			{
				//printf("%d %s %s %s %s %s %.2f %d",j,p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);

				fprintf(fp,"%s %s %s %s %s %.2f %d\r\n",pn->log_number,pn->name,pn->author,pn->publisher,pn->putout_time,pn->price,pn->num);
                pn = pn->next;
			}
			fclose(fp);
		}




		system("cls");
		coordinate(23,8);
		printf("正在删除，请稍后....");
		Sleep(2000);
		system("cls");
		coordinate(20,8);
		printf("-------------------------");
		coordinate(20,9);
		printf("|                       |");
		coordinate(20,10);
		printf("|  删除成功，是否继续？ |");
		coordinate(20,12);
		printf("| 1.是             2.否 |");
		coordinate(20,13);
		printf("|                       |");
		coordinate(20,14);
		printf("-------------------------");
		while(1)
		{
			c=getch();
			if(c=='1')
			{
				break;
			}
			else if(c=='2')
			{
				menu();
			}
		}

	}
}

void print_book()   //图书总览
{
	system("cls");
	color(6);
	int i=11;
	pNode p;
    p = CreateList();
	coordinate(14,2);
	printf("********************图书总览************************");
	coordinate(1,4);
	printf("---------------------------------------------------------------------");
	coordinate(5,5);
	printf("登录号    书名    作者名     出版单位     出版时间      价格    数量");
	coordinate(1,6);
	printf("---------------------------------------------------------------------");
	if(p->next==NULL)
	{
		coordinate(4,11);
		printf("书库暂时没有书哦~赶快去添加几本吧~(按任意键返回)");
		getch();
		menu();
	}
	else
	{
		TraverseList(p);
		//printf("成功");
	}
	coordinate(2,12);
	coordinate(2,i+3);
	printf("按任意键返回");
	getch();//不回显函数
	menu();
}

void find_book()  //查询图书
{
	do
	{
		system("cls");  //清屏
		color(8);
		char t;
		coordinate(29,2);
		printf(" 图书查询");
		coordinate(22,6);
		printf("|     1.书名  查询      |");
		coordinate(22,8);
		printf("|     2.作者  查询      |");
		coordinate(22,10);
		printf("|     3.登录号查询      |");
		coordinate(22,12);
		printf("|     4.出版社查询      |");
		coordinate(27,16);
		printf("按0返回主菜单");
		t=getch();
		switch(t)
		{
			case '0':
            menu();
			break;
			case '1':
            find_name_book();
			break;
			case '2':
            find_author_book();
			break;
			case '3':
            find_lognumber_book();
			break;
			case '4':
            find_publisher_book();
			break;
			default :
            break;
		 }
	}while(1);
}

void find_name_book()  //按名字查询
{
	system("cls");
	color(8);
	int i=11;
	pNode p=CreateList();
	char name[10];
	coordinate(2,4);
	printf("请输入您要查询图书的书名：");
	gets(name);
	coordinate(4,6);
	printf("正在查询....");
	Sleep(2000);
	coordinate(2,7);
	printf("******************************图书总览*************************************");
	coordinate(2,8);
	printf("----------------------------------------------------------------------------");
	coordinate(2,9);
	printf("登录号     书名     作者名      出版单位      出版时间      价格      数量");
	coordinate(2,10);
	printf("-----------------------------------------------------------------------------");
	p=p->next;
	while(p!=NULL)
	{
		if(strcmp(p->name,name)==0)
		{
			coordinate(2,i);
			printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
			i++;
		}
		p=p->next;
	}
	coordinate(2,i+2);
	printf("按任意键返回！");
	getch();
	find_book();
}

void find_author_book()   //按作者名查询
{
	system("cls");
	color(8);
	int i=11;
	char author[10];
	pNode p=CreateList();
	coordinate(2,4);
	printf("请输入您要查询图书的作者名：");
	gets(author);
	coordinate(4,6);
	printf("正在查询....");
	Sleep(2000);
	coordinate(2,7);
	printf("*****************************图书总览**************************************");
	coordinate(2,8);
	printf("----------------------------------------------------------------------------");
	coordinate(2,9);
	printf("登录号     书名      作者名      出版单位     出版时间      价格     数量");
	coordinate(2,10);
	printf("-----------------------------------------------------------------------------");
	p=p->next;
	while(p!=NULL)
	{
		if(strcmp(p->author,author)==0)
		{
			coordinate(2,i);
			printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
			i++;
		}
		p=p->next;
	}
	coordinate(2,i+2);
	printf("按任意键返回！");
	getch();
	find_book();
}

void find_lognumber_book()   //按图书编号查询
{
	system("cls");
	color(8);
	int i=11;
	pNode p=CreateList();
	char number[10];
	coordinate(2,4);
	printf("请输入您要查询图书的登录号：");
	gets(number);
	coordinate(2,6);
	printf("正在查询....");
	Sleep(2000);
	coordinate(2,7);
	printf("********************************图书总览**********************************");
	coordinate(2,8);
	printf("--------------------------------------------------------------------------");
	coordinate(2,9);
	printf("登录号    书名     作者名     出版单位    出版时间      价格    数量");
	coordinate(2,10);
	printf("--------------------------------------------------------------------------");
	p=p->next;
	while(p!=NULL)
	{
		if(strcmp(p->log_number,number)==0)//!此方法的核心
		{
			coordinate(2,i);
			printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
			i++;
		}
		p=p->next;
	}
	coordinate(2,i+2);
	printf("按任意键返回！");
	getch();
	find_book();
}

void find_publisher_book()   //按出版商查询
{
	system("cls");
	color(8);
	int i=11;
	pNode p=CreateList();
	char publish[10];
	coordinate(2,4);
	printf("请输入您要查询图书的出版社：");
	gets(publish);
	coordinate(2,6);
	printf("正在查询....");
	Sleep(2000);
	coordinate(2,7);
	printf("*****************************图书总览*************************************");
	coordinate(2,8);
	printf("-----------------------------------------------------------------------------");
	coordinate(2,9);
	printf("登录号     书名     作者名      出版单位      出版时间       价格      数量");
	coordinate(2,10);
	printf("-----------------------------------------------------------------------------");
	p=p->next;
	while(p!=NULL)
	{
		if(strcmp(p->publisher,publish)==0)
		{
			coordinate(2,i);
			printf("%s %s %s %s %s %.2f %d",p->log_number,p->name,p->author,p->publisher,p->putout_time,p->price,p->num);
			i++;
		}
		p=p->next;
	}
	coordinate(2,i+2);
	printf("按任意键返回！");
	getch();
	find_book();
}

void save_book(pNode pnew)   //将p中内容写入文件
{

	FILE *fp;    //文件指针
	fp=fopen("book.txt","ab");   //以追加的方式打开名字为mybook的二进制文件
	if(fp==NULL)
	{
		printf("不能打开文件！");
	}
	else   //将p所指向的一段大小为N的内容存入fp所指向的文件中
	{
	    fprintf(fp,"%s %s %s %s %s %.2f %d\r\n",pnew->log_number,pnew->name,pnew->author,pnew->publisher,pnew->putout_time,pnew->price,pnew->num);

	}
	fclose(fp);    //关闭文件
 }

 void out_system(){
	char t;
	coordinate(48,16);
	printf("-----------------------");
	coordinate(48,18);
	printf("|   您确定要退出吗？  |");
	coordinate(48,20);
	printf("| 1.确定     2.取消   |");
	coordinate(48,22);
	printf("-----------------------");
		t=getch();         //输入t
		switch(t)
		{
			case '1':
			system("cls");
		    color(6);
			coordinate(23,10);
			printf("正在安全退出....");
			Sleep(2000);     //暂停1秒
			system("cls");
			color(14);
			coordinate(2,2);
			printf("已安全退出软件");
			coordinate(23,8);
			printf("谢谢您对本软件的支持！--------------------------------");
			coordinate(23,10);
			printf("拜拜，欢迎下次使用！------------------------------------");
			exit(0);
            break; //终止程序
			case '2':
			menu();
            break;   //调用函数，进入菜单
			default :
            break;
		}

}
