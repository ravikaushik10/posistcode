#include<bits/stdc++.h>
#include<iostream>
#include<string.h>

using namespace std;


struct Owner
{

        string add;
	int ownerNo;
	string mobile;
	string name;
	string pwd;
	float value;


};

vector<Owner*> owners;
Owner *currOwner;

struct Node
{
	int nodeId;
	time_t timestamp;
	Owner *owner;
	int nodeNumber;
	vector<int> childNodeId;
	Node *refernceNodeId;
	vector<Node*> refChildNodeId;

};

Node *getNode(int n,Owner *owner)
{
	Node *pointnode=new Node;
	pointnode->timestamp=time(0);
	pointnode->owner=owner;
	pointnode->nodeNumber=n;
	pointnode->nodeId=(n);
	vector<Node*> v;
	pointnode->refChildNodeId=v;
	pointnode->refernceNodeId=NULL;
	vector<int> s;
	pointnode->childNodeId=s;
	return pointnode;
}

void task3(Node *children,int iddata,vector<Node*> &vec)
{
	for(int i=0;i<vec.size();i++)
	{
		if(vec[i]->nodeId==iddata)
		{
			vec[i]->refChildNodeId.push_back(children);
			vec[i]->childNodeId.push_back(children->nodeId);
			children->refernceNodeId=vec[i];
			break;
		}
	}
}

/* Encryption of data is done here */

string encryption(string str,int n)
{
	string sol=str;
	for(int i=0;i<sol.length();i++)
	{
		sol[i]+=n+10;
	}

	return sol;
}

/*decryption of data is done here*/

string decryption(string data,int n)
{
	string sol=data;
	for(int i=0;i<sol.length();i++)
	{
		sol[i]+=n-10;
	}
	return sol;
}


Owner *getOwner(int n,string name, string add, string mobile, string phn, float num,string pass)
{

	Owner *own=new Owner;
	own->ownerNo=n;


	own->pwd=encryption(pass,n);
	own->add=encryption(add,n);
	own->name=encryption(name,n);
	own->mobile=encryption(mobile,n);
	own->value=num;

	return own;
}
// verifying the owner
bool task4(vector<Owner*> &owners,Owner *currOwner,vector<Node*> &v)
{
	cout<<"Enter NodeId: ";
	int id;
	cin>>id;
	bool f=false;
	for(int i=0;i<v.size();i++)
	{
		if(v[i]->nodeId==id)
		{
			if(currOwner==v[i]->owner)
			{
				f=true;
			}
			break;
		}
	}

	return f;
}

int main(){

St:
    int num=0,nOwners=0;
    vector<vector<Node*> > set;
    vector<Node*> v;

    vector<Owner*> owners;
    int q;

    Owner *currOwner;
 abc:   bool f=false;

    while(!f)
    {
    	cout<<"Enter 1 to login ";
    	cout<<"\nEnter 2 to signup"<<endl;
    	cin>>q;
    	if(q==1)
	{

		cout<<"Enter key and password";
	    	string pass;
	    	int p, q, r;
	    	int key;
	    	cin>>key;
	    	cin>>pass;

	    	for(int i=0;i<owners.size();i++)
		{
	    		if(owners[i]->ownerNo==key)
			{
	    			if(pass==decryption(owners[i]->pwd,owners[i]->ownerNo))
				{
	    				f=true;
	    				currOwner=owners[i];
	    				cout<<"login success\n";
	    				break;
				}
			}
		}
		if(!f)
            cout<<"Unable to login"<<endl;

	}
	else
	{
		cout<<"Enter name, address, mobile, value, password:"<<endl;
		string name,add,mobile,phone,pass;
		float value;
		cin>>name;
		cin>>add;
		cin>>mobile;
		cin>>value;
		cin>>pass;


		currOwner=getOwner(nOwners++,name, add, mobile, phone, value,pass);
		owners.push_back(currOwner);
		cout<<"Key=  "<<currOwner->ownerNo<<endl;
		f=true;
	}
    }


    while(1)
    {
    	cout<<"Select any of below options: \n";
    	cout<<"1. Create Genesis Node\n";
    	cout<<"2. Create child nodes of a particular node\n";
    	cout<<"3. create child node that originates from a particular node\n";
    	cout<<"4. Encryption and decryption of data inside the node\n";
    	cout<<"5. verify the owner of the node\n";
    	cout<<"6. signout\n";

	cin>>q;
	if(q==1)
	{

		vector<Node*> v;
		v.push_back(getNode(num++,currOwner));
		set.push_back(v);
	}
	else if(q==2)
	{

		int n=0;
		cout<<"Enter number of nodes: ";
		cin>>n;
		vector<Node*> v(1);
		for(int i=0;i<n;i++)
		{
			v[0]=(getNode(num++,currOwner));
			set.push_back(v);
		}
		cout<<"Pushing nodes... "<<endl;
	}
	else if(q==3)
	{
		//Create child nodes
		Node *np=getNode(num++,currOwner);
		v.push_back(np);
		cout<<"Enter  set number: ";
		int setNo;
		cin>>setNo;
		set[setNo].push_back(np);

	}
	else if(q==4)
	{
		//encryption and decryption:
		//str is data to encrpt
		//str1 is encrypted data
		//str2 is decrypted data
		string str,str1,str2;
		str=owners[currOwner->ownerNo]->add;
		int n=currOwner->ownerNo;
		str1=encryption(str,n);
		str2=decryption(str1,n);


	}
	else if(q==5)
	{
		//Owner verification
		bool f=false;
		for(int i=0;i<set.size();i++)
		{
			f=task4(owners,currOwner,set[i]);
			if(f)
			{
				cout<<"You are the authorised owner!"<<endl;
			}
		}
		if(!f)
		{
			cout<<"You are NOT the authorised owner!"<<endl;
		}
	}
	else if(q==6)
        {
                cout<<"signed out!\n";
                goto abc;
        }
	else
	{
		break;
	}
   }
    return 0;
}
