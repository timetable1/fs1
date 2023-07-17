/*6. Write a program to implement index on secondary key, the name, for a file of
student objects. Implement add ( ), search ( ), delete ( ) using the secondary index.*/

#include<stdio.h>
#include<conio.h>
#include<fstream.h>
#include<string.h>
#include<process.h>

 void insert();
 void pack();
 void unpack();
 void help();
 void del();
 void search();

 class student
 {
 public:
	char off[25],name[25],age[25],sem[25],branch[25],usn[25],star[2];
 };

 student s;
 char buffer[55],temp[55];
 int off=0;
 fstream fp;
 fstream gp;
 fstream hp;

 void main()
 {
	int c;
	clrscr();
 while(1)
	{
		cout<<"\t*****Enter your choice*****\n";
		cout<<"1.Pack  2.unpack 3.Delete  4.search 5.exit"<<endl;
		cin>>c;

		switch(c)
		{
			case 1:	hp.open("sec_ind.txt",ios::out);
				hp.close();
				pack();
			break;
		
			case 2:	unpack();
			break;
		
			case 3:	del();
			break;
		
			case 4:	search();
			break;
		
			case 5:	exit(0);
		
			default:	cout<<"Invalid choice\n";
			break;
		}
	}
}

 void pack()
 {
	int flag;
	fstream rp;
	char usn[20],name[20],buffer1[20],off1[20],star[2]={"*"};

	fp.open("sec.txt",ios::app);             //to create file off.txt
	fp.close();
	fp.open("sec.txt",ios::in);
	rp.open("sec1.txt",ios::app);
	hp.open("sec_ind.txt",ios::app);

	off=0;

	insert();

	if(!fp)
	{
		cout<<"Error";
		return;
	}

	fp.getline(buffer1,60,'\n');

while(!fp.eof())
  {
	char star1[2]={"0"};
	sscanf(buffer1,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",off1,s.usn,s.name,s.sem,s.age,s.branch,star1);
	
	if(strcmp(star,star1)==0 && flag)
		{
			flag=0;
			off+=strlen(buffer);
			rp<<off1<<"|"<<buffer<<endl;
			sscanf(buffer,"%[^|]|%[^|]|",usn,name);
			hp<<usn<<"|"<<name<<"|"<<off1<<endl;
		}
	else
		{
			rp<<off<<"|"<<s.usn<<"|"<<s.name<<"|"<<s.sem<<"|"<<s.age<<"|"<<s.branch<<"|";
			if(strcmp(star1,star)==0)
			rp<<star1<<"|"<<endl;
			else
			rp<<endl;
			hp<<s.usn<<"|"<<s.name<<"|"<<off<<endl;
			off+=strlen(buffer1);
		}
	fp.getline(buffer1,60,'\n');
  }

	if(flag)
	{
	sscanf(buffer,"%[^|]|%[^|]|",usn,name);
	
	rp<<off<<"|"<<buffer<<endl;
	hp<<usn<<"|"<<name<<"|"<<off<<endl;
	}
hp.close();
fp.close();
rp.close();

char new1[]={"sec1.txt"};
char old[]={"sec.txt"};

remove("sec.txt");
rename(new1,old);
clrscr();
}

 void insert()
 {
	cout<<"USN";
	cin>>temp;
	strcpy(buffer,temp);
	strcat(buffer,"|");
	cout<<"name";help();
	cout<<"sem";help();
	cout<<"age";help();
      	 cout<<"Branch";help();
 }

void help()
{
	 cin>>temp;
	strcat(buffer,temp);
	strcat(buffer,"|");
}

void unpack()
{
	char star2[2]={"*"};
	fp.open("sec.txt",ios::in);
	fp.getline(buffer,60,'\n');
	cout<<"\t*****Unpacked record*****"<<endl;
	cout<<"OFF\tUSN\tName\tSEM\tage\tBranch"<<endl;
 
while(!fp.eof())
 {
	  char star1[2]={"0"};
	  sscanf(buffer,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",s.off,s.usn,s.name,s.sem,s.age,s.branch,star1);
	  if(strcmp(star1,star2)!=0)
		{
		cout<<s.off<<"\t"<<s.usn<<"\t"<<s.name<<"\t"<<s.sem<<"\t"<<s.age<<"\t"<<s.branch<<"\n";
		}
		
	fp.getline(buffer,60,'\n');
 }
   fp.close();
   getch();
   clrscr();
}

void del()
{
	char key1[25],key2[20],usn[25],buffer1[55],offset[25],name[20];
	int Notfound=1;
	
	cout<<"enter the primary key of the record to delete (usn)\n";
	cin>>key1;
	cout<<"Enter the secondary key of the record to delete (Name)\n";
	cin>>key2;

	fp.open("sec.txt",ios::in);
	gp.open("sec1.txt",ios::app);
	hp.open("sec_ind.txt",ios::in);

while(!hp.eof())
 {
     hp.getline(buffer1,60,'\n');
     sscanf(buffer1,"%[^|]|%[^|]|%[^|]|",usn,name,offset);
	if(strcmp(usn,key1)==0 && strcmp(name,key2)==0)
	{
		Notfound=0;
		break;
	}
 }
hp.close();
   
    if(Notfound)
     {
	cout<<"Invalid key\n";
	fp.close();
	gp.close();
	return;
     }

fp.getline(buffer,60,'\n');

while(!fp.eof())
  {
	sscanf(buffer,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",s.off,s.usn,s.name,s.sem,s.age,s.branch,s.star);
	if(strcmp(offset,s.off)==0)
		{
			gp<<buffer<<"*|"<<endl;
		}
	else
    	    gp<<buffer<<endl;
    	    fp.getline(buffer,60,'\n');
  }
cout<<"Deletion successful\n";

fp.close();
gp.close();

char new1[]={"sec1.txt"};
char old[]={"sec.txt"};

remove("sec.txt");
rename(new1,old);
}

void search()
{
fstream hp;
char key1[20],key2[20],name[20],usn[25],modify[55],offset[25],in_off[25],star1[2]={"*"};
int Notfound=1;

	cout<<"enter the primary key of the record to delete (usn)\n";
	cin>>key1;
	cout<<"Enter the secondary key of the record to delete (Name)\n";
	cin>>key2;

fp.open("sec.txt",ios::in);
hp.open("sec_ind.txt",ios::in);

hp.getline(buffer,60,'\n');

while(!hp.eof())
{
	sscanf(buffer,"%[^|]|%[^|]|%[^|]|",usn,name,in_off);
	
	if(strcmp(usn,key1)==0 && strcmp(name,key2)==0)
	  {
		Notfound=0;
		strcpy(offset,in_off);
	  }

	  hp.getline(buffer,60,'\n');
}
	if(Notfound)
		{
		cout<<"\t*****Record not found*****"<<endl;
		 hp.close();
		 fp.close();
		return;
		}
		hp.close();
		
	Notfound=1;

fp.getline(buffer,60,'\n');

while(!fp.eof())
  {      char star2[2]={"0"};
	 sscanf(buffer,"%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|%[^|]|",in_off,s.usn,s.name,s.sem,s.age,s.branch,star2);
	 
	if(strcmp(offset,in_off)==0 && strcmp(star1,star2)!=0)
		{
			Notfound=0;
			cout<<"Record Found\n";
			cout<<"OFFSET\tUSN\tName\tsem\tage\tbranch\n";
			cout<<in_off<<"\t"<<s.usn<<"\t"<<s.name<<"\t"<<s.sem<<"\t"<<s.age<<"\t"<<s.branch<<endl;
			fp.close();
			getch();
			return;
		}
fp.getline(buffer,60,'\n');
  }

  if(Notfound)
  {
  cout<<"******Record not found******\n";
  fp.close();
  getch();
  clrscr();
  }
}
