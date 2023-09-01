# C++和MPI并行程序设计

## 环境配置（Clion）

[(97条消息) CLoin中配置MPI搭建并行环境_starcpdk微信～的博客-CSDN博客_clion mpi](https://blog.csdn.net/weixin_44735933/article/details/111561179)

CMakeLists文件中应该插入的内容

```cpp
include_directories("F:\\MPI\\Include") \\MPI文件夹中Include文件夹所在位置
link_directories("F:\\MPI\\Lib\\x64") \\MPI文件夹中找到Lib\\x64文件夹
link_libraries(msmpi)\\固定语句
```

## 两个小时入门MPI与并行计算系列

[两小时入门MPI与并行计算系列 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/355652501)



## MPI Tutorial

[Tutorials · MPI Tutorial](https://mpitutorial.com/tutorials/)



## 代码示例

```cpp
#include <mpi.h>
#include <stdio.h>
double f(double);
double f(double x)
{
    return (4.0/(1.0+x*x));
}
int main(int argc,char *argv[])
{
    int myid, numprocs;
    int n, i;
    double mypi, pi;
    double h, sum, x;
    MPI_Init(&argc,&argv);
    MPI_Comm_size(MPI_COMM_WORLD,&numprocs);
    MPI_Comm_rank(MPI_COMM_WORLD,&myid);
    //printf("Process %d of %d.\n", myid, numprocs);
    n = 100;
    h = 1.0 / (double) n;
    sum = 0.0;
    if (myid == 0) {
        printf("The total number of programs is: %d \n", numprocs);
    }
    for (i = myid + 1; i <= n; i += numprocs)
    {
        x = h * ((double)i - 0.5);
        sum +=f(x);
    }
    mypi = h * sum;
    MPI_Reduce(&mypi, &pi, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
    if (myid == 0)
    {
        printf("The result is %.10f.\n",pi);
    }
    MPI_Finalize();
}
```

