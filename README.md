# list-shell
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<dirent.h>
char *buff,*t1,*t2,*t3,ch;
int pid;
void list(char t2,char *t3)
{
    DIR *dir;
    struct dirent *entry;
    int cnt=0;
    dir=opendir(t3);
    if (dir==NULL)
    {
        printf("Directory %s not found",t3);
        return;
    }
    
    switch(t2)
    {
        case 'f' :  while((entry=readdir(dir))!=NULL)
                    {
                        printf("%s\n",entry->d_name);
                    }
                    break;
        
        case 'n' :  while((entry=readdir(dir))!=NULL)
                        cnt++;
                        printf("Total No of Entries: %d\n",cnt);
                        break;
        
        case 'i' :  while((entry=readdir(dir))!=NULL)
                    {
                        printf("\n%s\t %d",entry->d_name,entry->d_ino);
                    }
                    break;
        default :   printf("Invalid argument");
    }
    
    closedir(dir);
}
main()
{
    while(1)
    {
        printf("myshell$");
        fflush(stdin);
        t1=(char *)malloc(80);
        t2=(char *)malloc(80);
        t3=(char *)malloc(80);
        buff=(char *)malloc(80);
        fgets(buff,80,stdin);
        sscanf(buff,"%s %s %s",t1,t2,t3);
        
        if(strcmp(t1,"pause")==0)
            exit(0);
        else if(strcmp(t1,"list")==0)
            list(t2[0],t3);
        else
        {
            pid=fork();
            if(pid<0)
                printf("Child process is not created\n");
            else if(pid==0)
            {
                execlp("/bin",NULL);
        
                if(strcmp(t1,"exit")==0)
                    exit(0);
                    system(buff);
            }
            else
            {
                wait(NULL);
                exit(0);
            }
        }
    }
}
