---
title: Fzu 1515 Balloons in a Box 盒子里的气球 解题报告
layout: post
tags: Fzu
categories: OJ解题报告
---
# 题目来源

* [Fzu-1515](http://acm.fzu.edu.cn/problem.php?pid=1515)

# 思路

* 算法艺术与信息学竞赛（俗称黑书）的第一道题（P8）
* N<=6,因此规模较小可以采用枚举
* 要注意可能出现点没有被使用的情况，因此要做好判断
* 此题计算球体覆盖之后剩余的长方体的体积
* 注意精度要用fabs
* PI取3.1415926536
* 最好四舍五入的采用printf的0.lf

# 代码
~~~
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const double PI = 3.1415926536;

struct Point
{
	int x;
	int y;
	int z;
};

int main()
{
	//freopen("input.txt", "r", stdin);
	
	int n; //n<=6
	Point arr_p[6];
	int order_idx[6];
	double arr_r[6];
	
	while(scanf("%d", &n) != EOF)
	{
		//输入矩阵两点 
		Point p1;
		scanf("%d%d%d", &p1.x, &p1.y, &p1.z);
		
		Point p2;
		scanf("%d%d%d", &p2.x, &p2.y, &p2.z);
		
	 	for(int i = 0; i < n; ++i)
	 	{
	 		Point p;
	 		scanf("%d%d%d", &p.x, &p.y, &p.z);
	 		arr_p[i] = p;
		}
		
//		for(int i = 0; i < n; ++i)
//		{
//			Point p = arr_p[i];
//			cout << p.x << " " << p.y << " " << p.z << endl;
//		}

		//构造全排列
		double max_sum = 0;
		for(int i = 0; i < n; ++i)
		{
			order_idx[i] = i;
		}
		do{
//			for(int i = 0; i < n; ++i)
//			{
//				cout << order_idx[i] << " "; 
//			}
//			cout << endl;
			
			double sum = 0;
			for(int i = 0; i < n; ++i)
			{
				Point p = arr_p[order_idx[i]];
				
				double min = fabs(p.x - p1.x);
				if(min > fabs(p.x - p2.x)) min = fabs(p.x - p2.x);
				if(min > fabs(p.y - p1.y)) min = fabs(p.y - p1.y);
				if(min > fabs(p.y - p2.y)) min = fabs(p.y - p2.y);
				if(min > fabs(p.z - p1.z)) min = fabs(p.z - p1.z);
				if(min > fabs(p.z - p2.z)) min = fabs(p.z - p2.z);
				
				for(int j = 0; j < i; ++j)
				{
					if(arr_r[order_idx[j]] != 0) //被使用才计算 
					{			
						Point pp = arr_p[order_idx[j]];
						double R = sqrt((p.x - pp.x) * (p.x - pp.x) + (p.y - pp.y) * (p.y - pp.y) + (p.z - pp.z) * (p.z - pp.z)) - arr_r[order_idx[j]];
						if(min > R) min = R;
					}
				}
				
				if(min < 0) min = 0; //为负数则表示被包含了
				arr_r[order_idx[i]] = min; 
				//printf("%lf ", 4.0 / 3.0 * PI * arr_r[order_idx[i]] * arr_r[order_idx[i]] * arr_r[order_idx[i]]);
				sum += 4.0 / 3.0 * PI * arr_r[order_idx[i]] * arr_r[order_idx[i]] * arr_r[order_idx[i]];
			}
			//printf("%lf ", sum);
			if(max_sum < sum) max_sum = sum;
		}while(next_permutation(order_idx, order_idx + n));
		
		double res = fabs(p1.x - p2.x) * fabs(p1.y - p2.y) * fabs(p1.z - p2.z) - max_sum;
		printf("%0.lf\n", res);
	}
	
    //fclose(stdin);
    
	return 0;
} 
~~~

# 结果

![Fzu-1515-RES](http://pam39teno.bkt.clouddn.com/JekyllWriter/Fzu-1515-RES.png)