#1751200231 钟泽豪

作业1                      
1.   共阴极：80H   
      共阳极：7FH
2. 静态显示：显示的数据是分开送到每一位LED上的。
                   优点：显示亮度很高。
                   缺点：但口线占用较多。
    
    动态显示：数据是同时送到每一个LED上。
                   优点：口线占用较少。
                   缺点：需要编程进行扫描。

作业2

#include<reg52.h>
#define uint unsigned int
#define uchar unsigned char

sbit  EN=P3^4;
sbit  RS= P3^5;
sbit  RW=P3^6;

void Read_Busy()
{
	uchar busy;
	P0 = 0xff;
	RS = 0;
	RW = 1;
	do
	{
		EN = 1;
		busy = P0;
		EN = 0;
	}while(busy & 0x80);
}

void Write_Cmd(uchar cmd)
{
	Read_Busy();
	RS = 0;
	RW = 0;
	P0 = cmd;
	EN = 1;
	EN = 0;
}

void Write_Dat(uchar dat)
{
	Read_Busy();
	RS = 1;
	RW = 0;
	P0 = dat;
	EN = 1;
	EN = 0;
}

void LCD1602_Dis_OneChar(uchar x, uchar y,uchar dat)
{
	if(y)	x |= 0x40;
	x |= 0x80;
	Write_Cmd(x);
	Write_Dat(dat);		
}

void LCD1602_Dis_Str(uchar x, uchar y, uchar *str)
{
	if(y) x |= 0x40;
	x |= 0x80;
	Write_Cmd(x);
	while(*str != '\0')
	{
		Write_Dat(*str++);
	}
}

void Init_LCD1602()
{
	Write_Cmd(0x38); 
	Write_Cmd(0x0c); 
	Write_Cmd(0x06); 
	Write_Cmd(0x01); 
}


void delay(uint z)
{
	uint x,y;
	for(x=z;x>0;x--)
		for(y=114;y>0;y--);
}

void main()
{
	uchar TestStr[] = {"1751200231"};
	uchar TestStr1[] = {"zhongzehao"}
	Init_LCD1602();
	LCD1602_Dis_Str(0, 0, &TestStr[0]);	
	LCD1602_Dis_Str(0, 1, &TestStr1[0]);	
}