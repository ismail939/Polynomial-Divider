#include "stdio.h"
#include "string.h"
#include "math.h"
//we declare a function to divide two terms like 2x^3 and 5x
int TermDivide(double coff1,double coff2,int p1,int p2,double coffR[]);
//multiply function to make an important part of long division where we just divided so we multiply back.
void Multiply(double coffR[],int powerOfR,double coff2[],int order2,double subtracted[]);
//the following function is to print any poly
void PrintPoly(double coff[],int highPower);
//a function to print the remainder
void PrintRemainder(double coff1[],int order1);
//to scan the coff of a polynomial
void ScanCoffeicents(double coff[],int highPower);
//this is our most important function and it returns 0 if the two polynomials aren't divisible
int PolynomialDivider();
int RemainderCheck(double coff1[],int order1);
//main
int main()
{
    PolynomialDivider();
}


int PolynomialDivider()
{
    double coffR[20]={0};//declaring the result array of coff
    double coff1[20]={0};//poly1
    double coff2[20]={0};//poly2
    int order1,order2;
    //input the order 1 and the corresponding coffs
    printf("Enter the highest power of the numerator:");
    scanf("%d",&order1);
    ScanCoffeicents(coff1,order1);
    //input the order 2 and the corresponding coffs
    printf("Enter the highest power of the denominator:");
    scanf("%d",&order2);
    ScanCoffeicents(coff2,order2);
    int counter=0;
    for(int i=0;i<=order2;i++)//in case the entered coefficients of the denominator are all zeros
    {
        if(coff2[i]!=0) counter++;
    }
    if(counter==0)
    {
        printf("It is forbidden to divide by zero!");
        return 0;
    }
    if(order1<order2)//if the input was like this (x+1)/(x^2+2)then the dom isn't a factor of the nom of course
    {
        printf("(");
        PrintPoly(coff1,order1);
        printf(")/");
        printf("(");
        PrintPoly(coff2,order2);
        printf(")");
        return 0;
    }
    for(int m=order1;m>=0;m--)
    {
        int check=TermDivide(coff1[m],coff2[order2],m,order2,coffR);
        //if(check==0)
          //  break;
        double subtracted[20]={0};
        Multiply(coffR,m-order2,coff2,order2,subtracted);
        for(int j=m;j>=0;j--)
        {
            coff1[j]=coff1[j]-subtracted[j];
        }
    }
    PrintPoly(coffR,order1-order2);//printing the result without the remainder
    int check1=RemainderCheck(coff1,order1);
    if(check1==1)
    {
        printf("+(");
        PrintRemainder(coff1,order1);
        printf(")");
        printf("/(");
        PrintPoly(coff2,order2);
        printf(")");
    }



}
void ScanCoffeicents(double coff[],int highPower)
{
    printf("Enter coefficients of poly separated with spaces and from a%d to a0 that a0 is coff of x^0\n",highPower);
    printf("if the coff is 0 write it\n");
    for(int i=highPower;i>=0;i--)
    {
        scanf("%lf",&coff[i]);
    }
}
int TermDivide(double coff1,double coff2,int p1,int p2,double coffR[])
{
    int pT;
    double coffT;
    if(p1>=p2)
        {
         pT=p1-p2;
         coffT=coff1/coff2;
         coffR[pT]=coffT;
         return 1;
        }
    return 0;
}
void Multiply(double coffR[],int powerOfR,double coff2[],int order2,double subtracted[])
{
    int powerT;
    for(int i=order2;i>=0;i--)
    {
        powerT=i+powerOfR;
        subtracted[powerT]=coffR[powerOfR]*coff2[i];
    }
}

void PrintPoly(double coff[],int highPower)
{
    for(int i=highPower;i>=0;i--)
        {
            if(coff[i]!=0)
           {
            if(i>1)
            {
                if(fabs(coff[i])!=1)
                {
                    printf("%0.1lfx^%d",coff[i],i);
                }
                else
                {
                    if(coff[i]==-1.0f)
                        printf("-");
                    printf("x^%d",i);
                }

                if(i>0&&coff[i-1]>0)
                    printf("+");
            }
            else if(i==1)
            {
                if(fabs(coff[i])!=1)
                    printf("%0.1lfx",coff[i]);
                else
                {
                    if(coff[i]==-1.0f)
                        printf("-");
                    printf("x");
                }
                if(i>0&&coff[i-1]>0)
                    printf("+");
            }
            else
            {
                if(coff[i]>0&&coff[i+1]==0)
                    printf("+");
                printf("%0.1lf",coff[i]);
            }
           }
        }
}

void PrintRemainder(double coff1[],int order1)
{
    for(int i=order1;i>=0;i--){
    if(coff1[i]!=0&&coff1[i]*0==0)//printing the remainder in the case it exsists
    {

        if(i>1)
        {
            if(fabs(coff1[i])!=1)
            {
                printf("%0.1lfx^%d",coff1[i],i);
            }
            else
            {
                if(coff1[i]==-1.0)
                    printf("-");
                printf("x^%d",i);
            }

            }
            else if(i==1)
            {
                if(fabs(coff1[i])!=1)
                    printf("%0.1lfx",coff1[i]);
                else
                {
                    if(coff1[i]==-1.0)
                        printf("-");
                    printf("x");
                }
            }
            else
            {
                printf("%0.1lf",coff1[i]);
            }
            if(coff1[i-1]>0)
                printf("+");

}
}
}
int RemainderCheck(double coff1[],int order1)
{
    for(int i=0;i<=order1;i++)
    {
        if(coff1[i]!=0)
            return 1;
    }
    return 0;
}
