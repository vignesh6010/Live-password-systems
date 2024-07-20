#include<pic.h>
__CONFIG(0X3F72);
#define _XTAL_FREQ 400000

#define RS RC0
#define RW RC1
#define EN RC2

#define C1 RB0 //OUTPUT
#define C2 RB1
#define C3 RB2

#define R1 RB4
#define R2 RB5  //INPUT    0XF0;
#define R3 RB6
#define R4 RB7

#define RED RC6
#define GREEN RC7

char pass[] = {'1','6','5','6'};
char Getpass[] ={'0'};
unsigned int n = 0;

void command(unsigned int y)
{
PORTD=y;
RS=0;
RW=0;
EN=1;
__delay_ms(20);
EN=0;
}
void LCD_DATA(const char z)
{
PORTD=z;
RS=1;
RW=0;
EN=1;
__delay_ms(20);
EN=0;
}

void initial()
{
command(0x38);
command(0x06);
command(0x0c);
command(0x01);
}

void LCD_String(const char str[])
{
unsigned int A_Index =0;
for (A_Index=0;A_Index[str]!=0;A_Index++)
{
LCD_DATA(str[A_Index]);
}
}

void keypad()
{
 C1=1;C2=0;C3=0;
 if(R1==1)
{
 while(R1==1);
 LCD_DATA('1');
 Getpass[n] ='1';
 n = n+1;
}
 if(R2==1)
{
 while(R2==1);
 LCD_DATA('4');
 Getpass[n] ='4';
 n = n+1;
}
 if(R3==1)
{
 while(R3==1);
 LCD_DATA('7');
 Getpass[n] ='7';
 n = n+1;
}
 if(R4==1)
{
 while(R4==1);
 LCD_DATA('*');
 Getpass[n] ='*';
 n = n+1;
}
C1=0;C2=1;C3=0;
 if(R1==1)
{
 while(R1==1);
 LCD_DATA('2');
 Getpass[n] ='2';
 n = n+1;
}
 if(R2==1)
{
 while(R2==1);
 LCD_DATA('5');
 Getpass[n] ='5';
 n = n+1;
}
 if(R3==1)
{
 while(R3==1);
 LCD_DATA('8');
 Getpass[n] ='8';
 n = n+1;
}
 if(R4==1)
{
 while(R4==1);
 LCD_DATA('0');
 Getpass[n] ='0';
 n = n+1;
}
C1=0;C2=0;C3=1;
 if(R1==1)
{
 while(R1==1);
 LCD_DATA('3');
 Getpass[n] ='3';
 n = n+1;
}
 if(R2==1)
{
 while(R2==1);
 LCD_DATA('6');
 Getpass[n] ='6';
 n = n+1;
}
 if(R3==1)
{
 while(R3==1);
 LCD_DATA('9');
 Getpass[n] ='9';
 n = n+1;
}
 if(R4==1)
{
 while(R4==1);
 LCD_DATA('#');
 Getpass[n] ='#';
 n = n+1;
}


if(n>3)
{
n=0;
if(pass[0]==Getpass[0] && pass[1]==Getpass[1] && pass[2]==Getpass[2] && pass[3]==Getpass[3])
{
GREEN = 1;
command(0X01);
command(0X80);
LCD_String("Password Match");
__delay_ms(5000);
command(0X01);
command(0X80);
LCD_String("Press the Button...");
command(0XC3);
GREEN =0;
}
else
{
RED =1;
command(0X01);
command(0X80);
LCD_String("Not Match");
__delay_ms(5000);
command(0X01);
command(0X80);
LCD_String("Press the Button...");
command(0XC3);
RED =1;
}
}
}
main()
{
TRISC=0X00;
PORTC=0X00;
TRISD=0X00;
PORTD=0X00;
TRISB=0XF0;
PORTB=0X00;
initial();
__delay_ms(300);
command(0x80);
LCD_String("Welcome");
command(0xc3);
LCD_String("HELLO");
__delay_ms(3000);
command(0X01);
command(0X80);
LCD_String("Press the Button...");
command(0XC3);
while(1)
{
keypad();
}
}
