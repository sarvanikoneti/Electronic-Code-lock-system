# Electronic-Code-lock-system
#include<reg52.h>
#include<string.h>
#define lcdport P1
sbit col1=P0^0;
sbit col2=P0^1;
sbit col3=P0^2;
sbit col4=P0^3;
sbit row1=P0^4;
sbit row2=P0^5;
sbit row3=P0^6;
sbit row4=P0^7;
sbit rs=P1^0;
sbit en=P1^2;
sbit buzzer=P2^6;
char pass[4],i=0;
void delay(int itime)
{
    int i,j;
    for(i=0;i<itime;i++)
    for(j=0;j<1275;j++);
}
void daten()
{
    rs=1;
    en=1;
    delay(5);
    en=0;
}
void lcddata(unsigned char ch)
{
    lcdport=ch & 0xf0;
    daten();
    lcdport=(ch<<4) & 0xf0;
    daten();
}
void cmden(void)
{
    rs=0;
    en=1;
    delay(5);
    en=0;
}
void lcdcmd(unsigned char ch)
{
    lcdport=ch & 0xf0;
    cmden();
    lcdport=(ch<<4) & 0xf0;
    cmden();
}
void lcdstring(char *str)
{
    while(*str)
    {
        lcddata(*str);
        str++;
    }
}
void lcd_init(void)
{
    lcdcmd(0x02);
    lcdcmd(0x28);
    lcdcmd(0x0e);
    lcdcmd(0x01);
}
void keypad()
{
        int cursor=192,flag=0;
    lcdcmd(1);
    lcdstring("Enter Ur Passkey");
    lcdcmd(0xc0);
    i=0;
    while(i<4)
    {
    flag=cursor;
		row1=row2=row3=row4=1;	
     col1=0;
     col2=col3=col4=1;
     if(!row1)
     {
        lcddata('1');
        pass[i++]='1';
        cursor++;
        while(!row1);
          }
      else if(!row2)
     {
        lcddata('2');
        pass[i++]='2';
        cursor++;
        while(!row2);
        }
      else if(!row3)
     {
        lcddata('3');
        pass[i++]='3';
        cursor++;
        while(!row3);
        }
      else if(!row4)
     {
        lcddata('A');
        pass[i++]='A';
        cursor++;
        while(!row4);
        }
     col2=0;
     col1=col3=col4=1;
     if(!row1)
     {
        lcddata('4');
        pass[i++]='4';
        cursor++;
        while(!row1);
        }
      else if(!row2)
     {
        lcddata('5');
        pass[i++]='5';
        cursor++;
        while(!row2);
        }
      else if(!row3)
     {
        lcddata('6');
        pass[i++]='6';
        cursor++;
        while(!row3);
        }
      else if(!row4)
     {
        lcddata('B');
        pass[i++]='B';
        cursor++;
        while(!row4);
        }
     col3=0;
     col1=col2=col4=1;
     if(!row1)
     {
        lcddata('7');
        pass[i++]='7';
        cursor++;
        while(!row1);
        }
      else if(!row2)
     {
        lcddata('8');
        pass[i++]='8';
        cursor++;
        while(!row2);
        }
      else if(!row3)
     {
        lcddata('9');
        pass[i++]='9';
        cursor++;
        while(!row3);
        }    
      else if(!row4)
     {
        lcddata('C');
        pass[i++]='C';
        cursor++;
        while(!row4);
        }
      col4=0;
     col1=col3=col2=1;
     if(!row1)
     {
        lcddata('*');
        pass[i++]='*';
        cursor++;
        while(!row1);
        }   
      else if(!row2)
     {
        lcddata('0');
        pass[i++]='0';
        cursor++;
        while(!row2);
        }
      else if(!row3)
     {
        lcddata('#');
        pass[i++]='#';
        cursor++;
        while(!row3);
        }
      else if(!row4)
     {
        lcddata('D');
        pass[i++]='D';
        cursor++;
        while(!row4);
          }
     if(i>0)
     {
         if(flag!=cursor)
         delay(100);
         lcdcmd(cursor-1);
         lcddata('*');
     } 
 }      
}
void accept()
{
     lcdcmd(1);
     lcdstring("WELCOME");
     lcdcmd(192);
     lcdstring("PASSWORD ACCEPTED");
     delay(200);
}
void wrong()
{
    lcdcmd(1);
    lcdstring("WRONG PASSKEY");
    lcdcmd(192);
    lcdstring("PLZ TRY AGAIN");
    delay(200);
}
void main()
{
    lcd_init();
    lcdstring("Electronic Code");
    lcdcmd(0xc0);
    lcdstring("  Lock System  ");
    delay(400);
    lcdcmd(1); 
    lcdstring("DONE BY");
	  lcdcmd(0xc0);
	  lcdstring("92 , 93, 79");
    delay(400);
    while(1)
    {
            i=0;
            keypad();
            if(strncmp(pass,"2705",4)==0)
            {
            accept();
            lcdcmd(1);
            lcdstring("ACCESS GRANTED");
            delay(300);
                }     
         else 
        {
           lcdcmd(1);
           lcdstring("ACCESS DENIED");
           wrong();
           delay(300);
        }
    }
}
