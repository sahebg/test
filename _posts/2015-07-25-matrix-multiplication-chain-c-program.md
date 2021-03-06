---
layout: post
seo:
   name: Welcome to Saheb's blog - Random Access Memories.
   type: BlogPosting
title: "Matrix Chain Multiplication , with C Program Example"
modified:   2017-07-08
categories: [ComputerSceince]
tags: [C, Algorithm]
sitemap:
    priority: 0.7
    changefreq: 'monthly'
    lastmod: 2017-08-06T12:49:30-05:00
author: Saheb Ghosh
description: Matrix Chain multiplication and algorithmic solution. C Program example of Matrix Chain multiplication.
image: /images/Algo/algo.jpg
pic:
    feature: /Algo/algo.jpg
date: 2017-08-05T12:49:30-05:00
---
Matrix chain multiplication (or Matrix Chain Ordering Problem, MCOP) is an optimization problem that can be solved using dynamic
programming. Given a sequence of matrices, the goal is to find the most efficient way to multiply these matrices. The problem is 
not actually to perform the multiplications, but merely to decide the sequence of the matrix multiplications involved.

Here are many options because matrix multiplication is associative. In other words, no matter how the product is parenthesized, 
the result obtained will remain the same. For example, for four matrices A, B, C, and D, we would have:

  ((AB)C)D = ((A(BC))D) = (AB)(CD) = A((BC)D) = A(B(CD))
    

![_config.yml]({{ site.baseurl }}/images/Algo/matrix.gif)

Dynamic Programming Solution :


- Take the sequence of matrices and separate it into two subsequences.
- Find the minimum cost of multiplying out each subsequence.
- Add these costs together, and add in the cost of multiplying the two result matrices.
- Do this for each possible position at which the sequence of matrices can be split, and take the minimum over all of them.

Program in C :

{% highlight c lineos %}
#include <stdio.h>
#include<limits.h>
#define INFY 999999999
long int m[20][20];
int s[20][20];
int p[20],i,j,n;

void print_optimal(int i,int j)
{
if (i == j)
printf(" A%d ",i);
else
   {
      printf("( ");
      print_optimal(i, s[i][j]);
      print_optimal(s[i][j] + 1, j);
      printf(" )");
   }
}

void matmultiply(void)
{
long int q;
int k;
for(i=n;i>0;i--)
 {
   for(j=i;j<=n;j++)
    {
     if(i==j)
       m[i][j]=0;
     else
       {
        for(k=i;k<j;k++)
        {
         q=m[i][k]+m[k+1][j]+p[i-1]*p[k]*p[j];
         if(q<m[i][j])
          {
            m[i][j]=q;
            s[i][j]=k;
          }
         }
        }
      }
 }
}

int MatrixChainOrder(int p[], int i, int j)
{
    if(i == j)
        return 0;
    int k;
    int min = INT_MAX;
    int count;
 
    for (k = i; k <j; k++)
    {
        count = MatrixChainOrder(p, i, k) +
                MatrixChainOrder(p, k+1, j) +
                p[i-1]*p[k]*p[j];
 
        if (count < min)
            min = count;
    }
 
    // Return minimum count
    return min;
}

void main()
{
int k;
printf("Enter the no. of elements: ");
scanf("%d",&n);
for(i=1;i<=n;i++)
for(j=i+1;j<=n;j++)
{
 m[i][i]=0;
 m[i][j]=INFY;
 s[i][j]=0;
}
printf("\nEnter the dimensions: \n");
for(k=0;k<=n;k++)
{
 printf("P%d: ",k);
 scanf("%d",&p[k]);
}
matmultiply();
printf("\nCost Matrix M:\n");
for(i=1;i<=n;i++)
 for(j=i;j<=n;j++)
  printf("m[%d][%d]: %ld\n",i,j,m[i][j]);


i=1,j=n;
printf("\nMultiplication Sequence : ");
print_optimal(i,j);
printf("\nMinimum number of multiplications is : %d ",
                          MatrixChainOrder(p, 1, n));

}

{% endhighlight %}


- Output:


![_config.yml]({{ site.baseurl }}/images/Algo/image4.png)



Please comment below in case of any problem found during running the code or any other doubts.
