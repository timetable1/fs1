/* Write a c++ program to read two lists of names and then match the names in the two lists using
cosequential Match based on a single loop. Output the names common to both the lists*/

#include<stdio.h>
#include<stdlib.h>
#include<iostream.h>
#include<fstream.h>
#include<stdlib.h>
#include<conio.h>
#include<string.h>
#include<iomanip.h>

void writelists()
{
 fstream out1,out2;
 int i,m,n;
 char name[20];

 out1.open("file1.txt",ios::out);
 out2.open("file2.txt",ios::out);
 
	if((!out1)||(!out2))
	 {
  	     cout<<"Unable to open one of the list files";
  	     getch();
 	     exit(0);
	 }

 cout<<"Enter the number of names you want to enter in file1";
 cin>>m;
 cout<<"enter the names in ascending order";
 
 for(i=0;i<m;i++)
 {
  cin>>name;
  out1<<name;
  out1<<"\n";
 }
 
 cout<<"Enter the number of names you want to enter in file2 :";
 cin>>n;
 cout<<"enter the names in ascending order :";

 for(i=0;i<n;i++)
 {
  cin>>name;
  out2<<name;
  out2<<"\n";
 }

out1.close();
out2.close();
}


void main()
{
 char list1[100][20],list2[100][20];
 int i,j,m,n,k=0;
 clrscr();

 fstream out1,out2,out3;

 writelists();
 out1.open("file1.txt",ios::in);
 out2.open("file2.txt",ios::in);
 out3.open("file3.txt",ios::out);

	 if((!out1)||(!out2)||(!out3))
 	   {
 	          cout<<"Unable to open one of the file";
 	          getch();
	          exit(0);
	 }
 clrscr();

 m=0;
 n=0;

 cout<<"LIST1 \n";
 
while(!out1.eof())
 {
  out1.getline(list1[m],20,'\n');
  cout<<list1[m]<<"\n";
  m++;
 }

cout<<"LIST2 \n";

while(!out2.eof())
 {
  out2.getline(list2[n],20,'\n');
  cout<<list2[n]<<"\n";
  n++;
 }

 m--;
 n--;

 i=0;
 j=0;

 cout<<"\n Elements common to both files are :";
 	
	while(i<m&&j<n)
 	{
		 if(strcmp(list1[i],list2[j])==0)
 			{
			      out3<<list1[i];
 			      cout<<list1[i]<<"\n";
			       i++;
			       j++;
 			      k++;
			 }
 
		else if(strcmp(list1[i],list2[j])<0)
 			{
			       i++;
 			}
 
		else
 			{
			       j++;
 			}
	 }

	 if(!k)
	     {
 		cout<<"No common names";
	     }
 getch();
}
