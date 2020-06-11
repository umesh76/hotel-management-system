#include <iostream.h>
#include <conio.h>
#include <fstream.h>
#include <string.h>
#include<stdio.h>
#include<iomanip.h>
#include<stdlib.h>
fstream record;
class person
{

	char address[100];
	int number;
	int nights;
	int room_no;
	int charges;
	int choice;

   public:
	void del();
	void process();
	char name[100];
	char phone_no[20];
	void input();
	void output();
	void modify();
	void Delete();
	void checkout();
	void modify_acc;
	int search()
	{

	     return room_no;
	}

} obj1;
void person::input()
{       clrscr();
	cout<<"\n\n\n\n";
	cout << "\n  \t\t\tenter your name:";
	gets(name);
	cout << "\n\t\t\tenter your address :";
	gets(address);
	cout << "\n\t\t\tenter mobile no. :";
	gets(phone_no);
	cout <<"\n\t\t\tenter the number of staying nights:";
	cin >> nights;
	clrscr();

   MENu:cout<<"\n\n\n\n";
	cout<<"\n\t\t\t choose your room type:";
	cout<<"\n\t\t\t1). ordinary rooms:-";
	cout<<"\n\t\t\t2). deluxe rooms:-";
	cout<<"\n\t\t\t3). suite:-";
	cout<<"\n\t\t\t4). king size room:-";
	cout<<"\n\n\t\t\t Enter your choice:";
	cin>> choice;
	switch(choice)
	{
		case 1:
		{       randomize();
			room_no=(rand()%10)+1;
			charges=400*nights;
			break;
		}
		case 2:
		{       randomize();
			room_no=(rand()%20)+11;
			charges= 600*nights;
			break;
		}
		case 3:
		{       randomize();
			room_no=(rand()%30)+21;
			charges=800*nights;
			break;
		}
		case 4:
		{       randomize();
			room_no=(rand()%40)+31;
			charges=1000*nights;
			break;

		}
		default:
		{
			 cout<<"wrong choice: \nchoose again:" ;
			 goto MENu;
			 break;
		}
	}


}

void person::output()
{
	cout<<" \n\t"<<room_no<<"\t\t"<<name<<"\t\t"<<phone_no;

 }
 void person::del()
 {

    cout<<"\n--------------------------------------------------------\n";
    cout<<"\n---------------------Your Bill----------------------------\n";
    cout<<"\n\t\t Room Number : " <<room_no;
    cout<<"\n\t\t Name	    : " <<name;
    cout<<"\n\t\t Address     : " <<address;
    cout<<"\n\t\t your charges: " <<charges;
    cout<<"\n--------------------------------------------------------\n";
    cout<<"\n--------------Thankyou For Staying----------------------\n";
    cout<<"\n--------------------------------------------------------\n";
    getch();
    clrscr();

}

void create()
	{
	 fstream record;
	record.open("hotel.dat",ios::binary|ios::out|ios::app);
	obj1.input();
	record.write((char*)&obj1,sizeof(obj1));
getch();
clrscr();

record.close();
}


void show()
{
	fstream record;
	record.open("hotel.dat",ios::binary|ios::in|ios::app);

		if(!record)
		{
		cout<<"no record";
		}
		else
		{
		clrscr();
		cout<<"\t\t\tGuest Details:";
		cout<<"\n--------------------------------------------------------\n";
		cout<<"| Room Number	| 	Name	|	Phone Number	|";
		cout<<"\n--------------------------------------------------------\n";
		while(record.read((char*)&obj1,sizeof(obj1)))
		    {
		     obj1.output();
		     cout<<endl;

		    }
		 }

record.close();
getch();
clrscr();
}
void person::modify()
{       cout << "\nenter your name:";
	cin >> name;
	cout << "\nenter your address :";
	cin >> address;
	cout << "\nenter mobile no. :";
	cin >> phone_no;
	cout << "\nenter the number of staying nights:";
	cin >> nights;
}
void modify_rec()
{       clrscr();
	int check;
	int count=0;
	fstream record;
	record.open("hotel.dat",ios::binary|ios::in|ios::out);
	if(!record)
	{
		cout<<"no data";
	}
	else
	{
		cout<<"\n\n\n\n\n\n\t\t\tenter room number to be modified:" ;
		cin>>check;
		while(!record.eof())
		{
			record.read((char*)&obj1,sizeof(obj1));
			if(obj1.search()==check)
			{
			  count++;
			  clrscr();
			  cout<<"\n\n\n\t\trecord found.!!!!!";
			  cout<<"\n\t\t\tGuest Details:";
			  cout<<"\n--------------------------------------------------------\n";
			  cout<<"| Room Number	| 	Name	|	Phone Number	|";
			  cout<<"\n--------------------------------------------------------\n";
			  obj1.output();
			  getch();
			  clrscr();
			  cout<<"\n\n\n\t\t\tenter new details\n";
			  obj1.modify();
			  int pos=(-1)*sizeof(obj1);
			  record.seekp(pos,ios::cur);
			  record.write((char*)&obj1,sizeof(obj1));

			  break;
			  }

		}

	}

		if(count==0)
		{
		cout<<"record not found.!!!";
		}

		record.close();
		getch();
		clrscr();


}
void checkout()
{
	clrscr();
	int check;
	int count=0;
	fstream record;
	fstream temp;
	record.open("hotel.dat",ios::binary|ios::in|ios::out);
	if(!record)
	{
		cout<<"file does not exits:";
	}
	else
	{
	cout<<"\n\n\n\n\n\t\t  enter your room number to checkout:";
	cin>>check;
	temp.open("temp.dat",ios::binary|ios::in|ios::out);
	record.seekg(0,ios::beg);
	while(record.read((char*)&obj1,sizeof(obj1)))
	{
	if(check==obj1.search())
	{
	clrscr();
	obj1.del();
	count=1;
	getch();
	clrscr();
	}
	if(check!=obj1.search())
	{
	temp.write((char*)&obj1,sizeof(obj1));
	}
	}
	record.close();
	temp.close();
	remove("hotel.dat");
	rename("temp.dat","hotel.dat");
	if(count==0)
	{
	cout<<"room is not occupied";
	getch();
	clrscr();
	}
	}
}
void search()
{         clrscr();
	  int Room;
	  int check=0;
	cout<<"\n\n\n\n\n\n\n\t\tenter room_no that should be searched:";
	cin>>Room;
	record.open("hotel.dat",ios::binary|ios::in|ios::out);
	while(!record.eof())
	{
	record.read((char*)&obj1,sizeof(obj1));
	if(obj1.search()==Room)
	{
	clrscr();
	cout<<"\t\t\tGuest Details:";
	cout<<"\n--------------------------------------------------------\n";
	cout<<"| Room Number	| 	Name	|	Phone Number	|";
	cout<<"\n--------------------------------------------------------\n";
	obj1.output();
	check++;
	break;
	}
	}
	if(check==0)
	{
	cout<<"\n record not present";
	}
	record.close();
	getch();
	clrscr();
}
int main()
{       clrscr();
	fstream hotel;
	int n = 1;
	int y = 2;
	int i;
	long phone;

   menu:cout<<"\n\n\n\n\n\t\t\t\t 1.Check in:"<<"\n";
	cout<<"\n\t\t\t2.View the details of guests: "<<"\n";
	cout<<"\n\t\t\t     3.Search a record:"<<"\n";
	cout<<"\n\t\t\t\t4.Check out:"<<"\n";
	cout<<"\n\t\t\t     5.Modify a record:"<<"\n";
	cout<<"\n\t\t\t\t   6.Exit:";
	cout<<"\n\t\t\t     Enter your choice:";
	cin>>i;
	switch(i)
      {
	case 1:
	{
		create();
		goto menu;
		break;

	}
	case 2:
	{
		show();
		goto menu;
		break;
	}
	case 3:
	{
		search();
		goto menu;
		break;
	}
	case 4:
	{
		checkout();
		goto menu;
		break;
	}

	case 5:
	{
		modify_rec();
		goto menu;
		break;
	}
	case 6:
	{

		break;
	}
	default:
	{
		cout<<"invalid:\n";
		goto menu;
		break;
	}
    }
    getch();
}

