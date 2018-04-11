#include<stdio.h>
struct process
{
    char pname;
    int arvl_tm,brst_tm,ct,wtng_tm,trnarnd_time,priority,brst_tm1;
}p[10],p1[10];
void main()
{
	struct process temp;
    int i,time=0,t1,t2,bu_t=0,largest,num,count=0,k,pf2=0,limit2,n,pos,j,flag=0,y;
    float wait_time=0,turnaround_time= 0;
    printf("\nEnter Number of Processes:\n");
    scanf("%d",&num);
    n=num;
    for(i=0;i<num;i++)
    {
    	printf("\nEnter process name:-");
    	fflush(stdin);
        scanf("%c",&p[i].pname);
        printf("Enter Arrival Time:-");
        scanf("%d",&p[i].arvl_tm);
        printf("Enter Burst Time:-");
        scanf("%d",&p[i].brst_tm);
        p[i].brst_tm1=p[i].brst_tm;
        printf("Enter Priority:\t");
        scanf("%d",&p[i].priority);
    }
    printf("\nTime Quantum for premptive priority queue:-");
    scanf("%d",&t1);
    printf("\nTime Quantum for Round Robin queue:-");
    scanf("%d",&t2);
    if((t1%2==0)&&(t2%2==0))
    {
    	printf("\nTRe enter values of time quantum ");
    	printf("\nTime Quantum for premptive priority queue:-");
    scanf("%d",&t1);
    printf("\nTime Quantum for Round Robin queue:-");
    scanf("%d",&t2);
	}
    printf("\n\nprocess\t|Turnaround Time|Waiting Time\n\n");
    for(i=0;i<num;i++)
    {
        pos=i;
        for(j=i+1;j<num;j++)
        {
            if(p[j].arvl_tm<p[pos].arvl_tm)
                pos=j;
        }
        temp=p[i];
        p[i]=p[pos];
        p[pos]=temp;
    }
    time=p[0].arvl_tm;
    for(i=0;num!=0;i++)
    {
    	while(count!=t1)
    	{
    		count++;
    		if(p[i].arvl_tm<=time)
    		{
    			for(j=i+1;j<num;j++)
    			{
    				if(p[j].arvl_tm==time&&p[j].priority<p[i].priority)
    				{
    					p1[pf2]=p[i];
						pf2++;
    					for(k=i;k<num-1;k++)
    						p[k]=p[k+1];
    					num--;
						count=0;
    					i=j-1;
    					j--;
					}
				}
			}
			time++;
			p[i].brst_tm--;
			if(p[i].brst_tm==0)
			{
				p[i].trnarnd_time=time-p[i].arvl_tm;
				p[i].wtng_tm=p[i].trnarnd_time-p[i].brst_tm1;
				printf("%c\t|\t%d\t|\t%d\n",p[i].pname,p[i].trnarnd_time,p[i].wtng_tm);
				wait_time+=time-p[i].arvl_tm-p[i].brst_tm1; 
    			turnaround_time+=time-p[i].arvl_tm;
    			for(k=i;k<num-1;k++)
    				p[k]=p[k+1];i--;
    			num--;
				count=t1;break;
			}
		}
		count=0;
		if(p[i].brst_tm!=0)
		{
			p1[pf2]=p[i];
			pf2++;
			for(k=i;k<num-1;k++)
    			p[k]=p[k+1];
    		num--;
		}
			if(i==num-1)
				i=-1;
	}
	
	limit2=pf2;
	for(count=0;limit2!=0;) 
	{ 
		if(p1[count].brst_tm<=t2&&p1[count].brst_tm>0) 
    	{ 
    		time+=p1[count].brst_tm; 
    		p1[count].brst_tm=0; 
    		flag=1; 
    	} 
    	else if(p1[count].brst_tm>0) 
    	{ 
    		p1[count].brst_tm-=t2; 
    		time+=t2; 
    	} 
    	if(p1[count].brst_tm==0&&flag==1) 
    	{ 
    		limit2--; 
    		p1[count].trnarnd_time=time-p1[count].arvl_tm;
			p1[count].wtng_tm=p1[count].trnarnd_time-p1[count].brst_tm1; 
			printf("%c\t|\t%d\t|\t%d\n",p1[count].pname,p1[count].trnarnd_time,p1[count].wtng_tm); 
    		turnaround_time+=time-p1[count].arvl_tm; 
    		wait_time+=time-p1[count].arvl_tm-p1[count].brst_tm1;
    		for(k=count;k<limit2;k++)
    			p1[k]=p1[k+1];count--;
    		flag=0; 
    	} 

    	if(count==limit2-1) 
      		count=0; 
    	else 
    		count++; 
    }
    printf("\nAverage Waiting Time= %f\n",wait_time*1.0/n); 
    printf("Average Turnaround Time = %f",turnaround_time*1.0/n);   
}
