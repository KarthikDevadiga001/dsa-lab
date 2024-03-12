# dsa-lab
9. Binary Tree
#include <stdio.h>
#include <stdlib.h>
struct node
{
 int data;
 struct node *left;
 struct node *right;
};
typedef struct node *NODE;
NODE create_node(int item)
{
 NODE temp;
 temp=(NODE)malloc(sizeof(struct node));
 temp->data=item;
 temp->left=NULL;
 temp->right=NULL;
 return temp;
}
NODE insertleft(NODE root,int item)
{
 root->left=create_node(item);
 return root->left;
}
NODE insertright(NODE root,int item)
{
 root->right=create_node(item);
 return root->right;
}
void display(NODE root)
{
 if(root!=NULL)
 {
 display(root->left);
 printf("%d\t",root->data);
 display(root->right);
 }
}
int count_nodes(NODE root)
{
 if (root == NULL)
 return 0;
 else
 return (count_nodes(root->left) + count_nodes(root->right) + 1);
}
int height(NODE root)
{
 int leftht,rightht;
 if(root == NULL)
 return -1;
 else
 {
 leftht = height(root->left);
 rightht = height(root->right);
 if(leftht > rightht)
 return leftht + 1;
 else
 return rightht + 1;
 }
}
int leaf_nodes(NODE root)
{
 if(root==NULL)
 return 0;
 else if(root->left == NULL && root->right == NULL)
 return 1;
 else
 return leaf_nodes(root->left) + leaf_nodes(root->right);
}
int nonleaf_nodes(NODE root)
{
 if(root==NULL || (root->left == NULL && root->right == NULL))
 return 0;
 else
 return nonleaf_nodes(root->left) + nonleaf_nodes(root-
>right) + 1;
}
int main()
{
 NODE root=NULL;
 root=create_node(45);
 insertleft(root,39);
 insertright(root,78);
 insertleft(root->right,54);
 insertright(root->right,79);
 insertright(root->right->left,55);
 insertright(root->right->right,80);
 printf("\n The tree(inorder) is\n");
 display(root);
 printf("\n");
 printf("\n The total number of nodes is 
%d\n",count_nodes(root));
 printf("\n The height of the tree is %d\n",height(root));
 printf("\n The total number of leaf nodes is 
%d\n",leaf_nodes(root));
 printf("\n The total number of non-leaf nodes is 
%d\n",nonleaf_nodes(root));
 return 0;
}



10. Binary Search Tree
#include <stdio.h>
#include <stdlib.h>
struct node
{
 int data;
 struct node *left;
 struct node *right;
};
typedef struct node *NODE;
NODE create_node(int item)
{
 NODE temp;
 temp=(NODE)malloc(sizeof(struct node));
 temp->data=item;
 temp->left=NULL;
 temp->right=NULL;
 return temp;
}
NODE Insertbst(NODE root,int item)
{
 NODE temp;
 temp=create_node(item);
 if(root==NULL)
 return temp;
 else
 {
 if(item < root->data)
 root->left=Insertbst(root->left,item);
 else
 root->right=Insertbst(root->right,item);
 }
 return root;
}
void preorder(NODE root)
{
 if(root!=NULL)
 {
 printf("%d\t",root->data);
 preorder(root->left);
 preorder(root->right);
 }
}
void inorder(NODE root)
{
 if(root!=NULL)
 {
 inorder(root->left);
 printf("%d\t",root->data);
 inorder(root->right);
 }
}
void postorder(NODE root)
{
 if(root!=NULL)
 {
 postorder(root->left);
 postorder(root->right);
 printf("%d\t",root->data);
 }
}
NODE inordersuccessor(NODE root)
{
 NODE cur=root;
 while(cur->left != NULL)
 cur = cur->left;
 return cur;
}
NODE deletenode(NODE root,int key)
{
 NODE temp;
 if(root == NULL)
 return NULL;
 if(key<root->data)
 root->left = deletenode(root->left,key);
 else if(key > root->data)
 root->right = deletenode(root->right,key);
 else
 {
 if(root->left == NULL)
 {
 temp=root->right;
 free(root);
 return temp;
 }
 if(root->right == NULL)
 {
 temp=root->left;
 free(root);
 return temp;
 }
 temp=inordersuccessor(root->right);
 root->data=temp->data;
 root->right=deletenode(root->right,temp->data);
 }
 return root;
}
int main()
{
 NODE root = NULL;
 int ch,item,key;
 for(;;)
 {
 printf("\n 1. Insert");
 printf("\n 2. Preorder");
 printf("\n 3. Inorder");
 printf("\n 4. Postorder");
 printf("\n 5. Delete");
 printf("\n 6. Exit");
 printf("\n Read ur choice:");
 scanf("%d",&ch);
 switch(ch)
 {
 case 1:printf("\n Read element to be inserted :");
 scanf("%d",&item);
 root=Insertbst(root,item);
 break;
 case 2:printf("\n The Preorder traversal is\n");
 preorder(root);
break;
 case 3:printf("\n The Inorder traversal is\n");
 inorder(root);
break;
 case 4:printf("\n The Postorder traversal is\n");
 postorder(root);
break;
 case 5:printf("\n Read node to be deleted : ");
 scanf("%d",&key);
 root=deletenode(root,key);
break;
 default :exit(0);
 }
 }
 return 0;
}



1. Infix to Postfix
#include <stdlib.h>
#include <ctype.h>
#define SIZE 20
struct stack
{
 int top;
 char data[SIZE];
};
typedef struct stack STACK;
void push(STACK *s,char item)
{
 s->data[++(s->top)]=item;
}
char pop(STACK *s)
{
 return s->data[(s->top)--];
}
int preced(char symbol)
{
 switch(symbol)
 {
 case '^':return 5;
 case '*':
 case '/':return 3;
 case '+':
 case '-':return 1;
 }
}
void infixtopostfix(STACK *s,char infix[SIZE])
{
 int i,j=0;
 char postfix[SIZE],temp,symbol;
 for(i=0;infix[i]!='\0';i++)
 {
 symbol=infix[i];
 if(isalnum(symbol))
 postfix[j++]=symbol;
 else
 {
 switch(symbol)
 {
 case '(':push(s,symbol);
 break;
 case ')':temp=pop(s);
 while(temp!='(')
{
 postfix[j++]=temp;
temp=pop(s);
 }
 break;
 case '+':
 case '-':
 case '*':
 case '/':
 case '^': if (s->top ==-1 || s->data[s->top]=='(')
 push(s,symbol);
 else
{
 while(preced(s->data[s->top])>= 
preced(symbol) && s->top!=-1 &&s->data[s->top]!='(')
 postfix[j++]=pop(s);
 push(s,symbol);
 }
break;
 default :printf("\n Invalid!!!!!");
 exit(0);
 }
 }
 }
 while(s->top!=-1)
 postfix[j++]=pop(s);
 postfix[j]='\0';
 printf("\n The postfix expression is %s\n",postfix);
}
int main()
{
 STACK s;
 s.top=-1;
 char infix[SIZE];
 printf("\n Read Infix expression\n");
 scanf("%s",infix);
 infixtopostfix(&s,infix);
 return 0;
}
2. Evaluation of Prefix 
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <math.h>
#include <string.h>
#define SIZE 20
struct stack
{
 int top;
 float data[SIZE];
};
typedef struct stack STACK;
void push(STACK *s,float item)
{
 s->data[++(s->top)]=item;
}
float pop(STACK *s)
{
 return s->data[(s->top)--];
}
float operate(float op1,float op2,char symbol)
{
 switch(symbol)
 {
 case '+':return op1+op2;
 case '-':return op1-op2;
 case '*':return op1*op2;
 case '/':return op1/op2;
 case '^':return pow(op1,op2);
 }
}
float eval(STACK *s,char prefix[SIZE])
{
 int i;
 char symbol;
 float res,op1,op2;
 for(i=strlen(prefix)-1;i>=0;i--)
 {
 symbol=prefix[i];
 if(isdigit(symbol))
 push(s,symbol-'0');
 else
 {
 op1=pop(s);
 op2=pop(s);
 res=operate(op1,op2,symbol);
 push(s,res);
 }
 }
 return pop(s);
}
int main()
{
 char prefix[SIZE];
 STACK s;
 float ans;
 s.top=-1;
 printf("\n Read prefix expr\n");
 scanf("%s",prefix);
 ans=eval(&s,prefix);
 printf("\n The final answer is %f\n",ans);
 return 0;
}



5. Queue of Integers using Circular List


#include <stdio.h>
#include <stdlib.h>
#define SIZE 5
int count;
struct node
{
 int data;
 struct node *addr;
};
typedef struct node *NODE;
NODE insertend(NODE last,int item)
{
 NODE temp;
 if(count>=SIZE)
 {
 printf("\ Queue full");
 return last;
 }
 count=count+1;
 temp=(NODE)malloc(sizeof(struct node));
 temp->data=item;
 if(last==NULL)
 {
 temp->addr=temp;
 return temp;
 }
 else
 {
 temp->addr=last->addr;
 last->addr=temp;
 return temp;
 }
}
NODE deletebegin(NODE last)
{
 NODE temp;
 if(last==NULL)
 {
 printf("\n Queue empty");
 return NULL;
 }
 if(last->addr==last)
 {
 printf("\n Element deleted is %d\n",last->data);
 free(last);
 return NULL;
 }
 else
 {
 temp=last->addr;
 last->addr=temp->addr;
 printf("\n Element deleted is %d\n",temp->data);
 free(temp);
 return last;
 }
}
void display(NODE last)
{
 NODE temp;
 if(last==NULL)
 printf("\n Queue is empty");
 else
 {
 printf("\n Queue Content are\n");
 temp=last->addr;
 while(temp!=last)
 {
 printf("%d\t",temp->data);
 temp=temp->addr;
 }
 printf("%d\t",temp->data);
 }
}
int main()
{
 NODE last=NULL;
 int item,ch;
 for(;;)
 {
 printf("\n1.Insert\n2.Delete\n3.Display\n4.Exit");
 printf("\nRead Choice :");
 scanf("%d",&ch);
 switch(ch)
 {
 case 1:printf("\n Read data to be inserted:");
 scanf("%d",&item);
last=insertend(last,item);
break;
 case 2:last=deletebegin(last);
 break;
 case 3:display(last);
 break;
 default:exit(0);
 }
 }
 return 0;
}

