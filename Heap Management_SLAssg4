#include<stdio.h>
#include<stdlib.h>
#include<math.h>
#define P 8

 int max_mem_size ;

struct alloc_node
{
    int start_add;
    int end_add;
    int size;
    struct alloc_node *next;
};

struct free_node
{
    int start_add;
    int end_add;
    int size;
    struct free_node *next;
};

struct free_list
{
    int id;
    struct free_node *next;
};


struct alloc_node *lptr;



void initialize(struct free_list fl[]);
int allocate_space(int num);
struct free_node* make_free_node(int size,int start_add,int end_add);
void print_free_lists(struct free_list fl[]);
void insert_free_list(struct free_node *ptr,struct free_list fl[],int level);
struct alloc_node* make_alloc_node(int size,int strt_add,int end_add);
struct alloc_node* insert_alloc_list(int val,int strt_add,int end_add,struct alloc_node *lptr);
void print_alloc_list(struct alloc_node *lptr);
void delete_free_list_node(struct free_list fl[],int level,int strt_add);
void alloc_a_list(int alloc_size,struct free_list fl[]);
void delete_alloc_list_node(int strt_add,struct free_list fl[]);
void dealloc(int strt_add,struct free_list fl[]);

int main()
{ 
    int max_mem_size = pow(2,P);
    int i,done=0,action,size,alloc_size,address;
    struct free_list fl[P];
    struct free_node *ptr;
    lptr=NULL;

    initialize(fl);

     print_free_lists(fl);
     print_alloc_list(lptr);
    
    while(done==0)
    {
        printf("\n");
        printf("--------------------------------------------------------------------------------------------------\n");
        printf("press -1 to quit");
        printf("\n");

       
        printf("SELECT YOUR ACTION\n");
        printf("1.Allocate\n2.Deallocate\n\n");

        scanf("%d",&action);

        if(action==-1)
        {
            done=1;
        }
        else if(action==1)
        {
            printf("ENTER THE SIZE TO ALLOCATE\n");
            scanf("%d",&size);
            alloc_size=allocate_space(size);
            alloc_a_list(alloc_size,fl);
        }
        else
        {
            printf("ENTER THE START ADDRESS TO DEALLOCATE\n");
            scanf("%d",&address);
            dealloc(address,fl);
        }

         print_free_lists(fl);
         print_alloc_list(lptr);
        
    }


}

void initialize(struct free_list fl[])
{
    int i;

    for(i=0;i<P;i++)
    {
        fl[i].id=pow(2,i);
        fl[i].next=NULL;
    }

    struct free_node *ptr;
    ptr=make_free_node(pow(2,P-1),0,pow(2,P-1)-1);

    fl[P-1].next=ptr;


}

int allocate_space(int num)
{ 
  int mem_size,alloc_size;
  mem_size=pow(2,P);
  while(num <= mem_size)
  {   
      alloc_size=mem_size;
      mem_size/=2;
  } 

  max_mem_size -= alloc_size;

  return alloc_size;

  //insert node into free list of size alloc list;


}


struct free_node* make_free_node(int size,int start_add,int end_add)
{
    struct free_node *nptr;
    nptr=(struct free_node*)malloc(sizeof(struct free_node));
    nptr->size=size;
    nptr->start_add=start_add;
    nptr->end_add=end_add;
    nptr->next=NULL;

    return nptr;
}

void print_free_lists(struct free_list fl[])
{   
    int i;
    struct free_node *ptr;

    printf("\nFREE LIST:\n");

    for(i=0;i<P;i++)
    {
        if(fl[i].next==NULL)
        {
            printf("%d----->NULL\n",fl[i].id);
        }
        else
        {   
            ptr=fl[i].next;
            printf("%d------>",fl[i].id);
            while(ptr!=NULL)
            {
                printf("%d------>%d=======%d-------->",ptr->size,ptr->start_add,ptr->end_add);
                ptr=ptr->next;
            }

            printf("\n");
        }
                  
    }

}


void insert_free_list(struct free_node *ptr,struct free_list fl[],int level)
{   
    struct free_node *nptr;
    nptr=fl[level].next;   

    if(nptr==NULL)
    {
       fl[level].next=ptr; 
    }
    else
    {
        while(nptr->next!=NULL)
        {
            nptr=nptr->next;
        }

        nptr->next=ptr;
    }


}

struct alloc_node* insert_alloc_list(int val,int strt_add,int end_add,struct alloc_node *lptr)
{
    struct alloc_node *ptr,*nptr;
    ptr=make_alloc_node(val,strt_add,end_add);
    nptr=lptr;

    if(lptr==NULL)
    {
        lptr=ptr;
    }
    else
    {
        while(nptr->next!=NULL)
        {
            nptr=nptr->next;
        }

        nptr->next=ptr;
    }

    return lptr;

}

struct alloc_node* make_alloc_node(int size,int strt_add,int end_add)
{
    struct alloc_node *ptr;
    ptr = (struct alloc_node*)malloc(sizeof(struct alloc_node));  
    ptr->size=size;
    ptr->start_add=strt_add;
    ptr->end_add=end_add;
    ptr->next=NULL;
    return ptr;

}

void print_alloc_list(struct alloc_node *lptr)
{

    struct alloc_node *ptr;
    ptr=lptr;

    printf("\n\n ALLOC LIST \n\n");

    while(ptr!=NULL)
    {
        printf("%d------>%d======%d------>",ptr->size,ptr->start_add,ptr->end_add);
        ptr=ptr->next;
    }

    printf("\n\n");
}


void delete_free_list_node(struct free_list fl[],int level,int strt_add)
{
    struct free_node *ptr,*prev;
    ptr=fl[level].next;
    prev=NULL;

    while(ptr->start_add!=strt_add)
    {   
        prev=ptr;
        ptr=ptr->next;
    }

    if(prev==NULL)
    {
        free(ptr);
        fl[level].next=NULL;
    }
    else
    {
        prev->next=ptr->next;
        free(ptr);
    }


    
}



////////////////////////////////////////////////////alloc logic /////////////////////////////////////////////
void alloc_a_list(int alloc_size,struct free_list fl[])
{

int i,size,strt_add,end_add,strt_add_1,end_add_1,strt_add_2,end_add_2,mid,j,size_1,level=0;
struct free_node *ptr,*nptr;
int flag=0,count=0;

size_1=alloc_size;



for(i=0;i<P;i++)
{
    if(fl[i].next==NULL)
    {
        count++;
    }
}

while(size_1!=1)
    {
        size_1=size_1/2;
        level++;

    }


if(count==P)
{
    printf("\n\n not enough memory to allocate \n\n");
}




for(i=0;i<P;i++)
{
    if(fl[i].next!=NULL && fl[i].next->size==alloc_size )
    {   
        lptr=insert_alloc_list(pow(2,i),fl[i].next->start_add,fl[i].next->end_add,lptr);
        delete_free_list_node(fl,i,fl[i].next->start_add);
        flag=1;

    }

    
}









if(flag==0)
{

i=level;

while(fl[i].next==NULL)
{
    i++;
}   
    j=i;
    ptr=fl[i].next;
    size=ptr->size;
    strt_add=ptr->start_add;
    end_add=ptr->end_add;

    if(size<alloc_size)
    {
        printf("\n\n not enough memory to allocate \n\n");
    }


    
    else if(size == alloc_size)
    {
        delete_free_list_node(fl,i,strt_add);
        //add to alloc list
        lptr=insert_alloc_list(size,strt_add,end_add,lptr);
    }
    else
    {   
        while(size!=alloc_size)
        {
        
         mid=(strt_add + end_add)/2;
         size=size/2;

         
        if(i==j)
        {delete_free_list_node(fl,i,strt_add);}

        

         strt_add_1=strt_add;
         end_add_1=mid;
         strt_add_2=mid+1;
         end_add_2=end_add;

         //insert st_add_2 and end_add_2 into free list of size-->size/2  (i-1)

         nptr=make_free_node(size,strt_add_2,end_add_2);

         insert_free_list(nptr,fl,i-1);
        



         strt_add=strt_add_1;
         end_add=end_add_1;

            i--;
        

        }

        if(size==alloc_size)
        {
            //insert into alloc_list size,strt_add,end_add;

            lptr=insert_alloc_list(size,strt_add,end_add,lptr);
        }

        if(i<=0)
        {
            printf("\n\n not enough memory to allocate \n\n");
}
        }



    }


}





void dealloc(int strt_add,struct free_list fl[])
{
    int buddy_num,buddy_add,i,base_add,size,size_1,level=0,flag=1,done=1,flag1=1;
    struct alloc_node *ptr,*prev;
    struct free_node *nptr,*travel,*insert_node;


    ptr=lptr;
    prev=NULL;

    while(ptr!=NULL && ptr->start_add!=strt_add)
    {
        prev=ptr;
        ptr=ptr->next;

    }

    

    size=ptr->size;
    size_1=size;
    base_add=strt_add;
    buddy_num=base_add/size;

    insert_node=make_free_node(size,ptr->start_add,ptr->end_add);

    if(prev==NULL)
    {   
        lptr=ptr->next;
        free(ptr);
    }
    else
    {
        prev->next=ptr->next;
        free(ptr);
    }

    while(size_1!=1)
    {
        size_1=size_1/2;
        level++;

    }

    if(buddy_num%2==0)
    {
        buddy_add=base_add + size;
    }
    else
    {
        buddy_add=base_add - size;
    }
    
    for(i=level;i<P&&flag1!=0;i++)
    {    
        travel=fl[i].next;

        flag=0;

        //insert directly if list is NULL

        if(travel==NULL)
        {
            insert_free_list(insert_node,fl,i);
            flag1=0;
        }
        else
        {   
            while(travel!=NULL&&flag==0)

            {   
                

                if(travel->start_add==buddy_add)
                {
                    insert_node->size=size*2;

                    if(buddy_num%2==0)
                    {
                        insert_node->end_add=travel->end_add;
                    }
                    else
                    {
                        insert_node->start_add=travel->start_add;
                    }
                    
                    //buddy found so delete
                    delete_free_list_node(fl,i,travel->start_add);
                    //travel=NULL;
                    flag=1;

            
                    
                }

                travel=travel->next;
            }

            if(flag!=1)
            {
                //add to list no buddy found
                insert_free_list(insert_node,fl,i);
                flag1=0;
        
            }
            else
            {
                base_add=insert_node->start_add;
                size=insert_node->size;
                buddy_num=base_add/size;

                if(buddy_num%2==0)
                {
                     buddy_add=base_add + size;
                }
                else
                {
                    buddy_add=base_add - size;
                }

            

            }
            




        }
        



    

    }



}
