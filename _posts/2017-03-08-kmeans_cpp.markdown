---
layout:     post
title:      "Simple implementation of Kmeans in C++ (Kmeans简单应用C++"
subtitle:   ""
date:       2017-03-08 11:10:00
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
tags:
    - Coding

---
I finished it here to help anyone want to get familiar with it.

Brief description of k-means:

1. randomly select K(number of classes) centroids from data points
2. find minimal distance from every data points to each centroids and the points will be labeled according to their distance to each centroids
3. update centroids based on configured clusters
4. repeat 2 & 3 until centroids are not changed or maximum iterations reached.

```cpp

/*
* kmeans.cpp
*
*  Created on: Mar 8, 2017
*      Author: pingge
*/

#include "kmeans.h"
#include <vector>
#include <stdlib.h>
#include <math.h>
#include <limits.h>

using namespace std;

struct points
{
    int x;
    int y;
};
/*
* brief@
* pts cloud points coordinates
* num_pts number of cloud points
* k_classes number of clusters
* iter_num maximum number of iterations
*/
void kmeans(points* pts, int num_pts, int k_classes, int iter_num)
{
    int i = 0, j = 0, k = 0;
    int iter = 0;
    double distance;
    double min_distance;
    bool flag = true;
    points* centroids = new points[k_classes];
    points* new_centroids = new points[k_classes];
    /**randomly get centroids**/
    for (int i = 0; i < k_classes; i++)
    {
        centroids[i] = pts[rand()%num_pts];
    }
    int Label[num_pts];
    /**begin iteration**/
    for (iter = 0; iter < iter_num; iter++)
    {
        /**calculate distance from pts to centroids**/
        for (j = 0; j < num_pts; j++)
        {
            min_distance = INT_MAX;
            for (k = 0; k < k_classes; k++)
            {
                distance = sqrt(pow((pts[j].x - centroids[k].x), 2) + pow((pts[j].y - centroids[k].y), 2));
                if (distance < min_distance)
                {
                    min_distance = distance;
                    Label[j] = k;
                }
            }
        }
        /**calculate new centroids**/
        int pts_nums_in_class = 0;
        double sum_x = 0;
        double sum_y = 0;
        for (i = 0; i < k_classes; i++)
        {
            for (j = 0; j < num_pts; j++)
            {
                if (Label[j] == i)
                {
                    pts_nums_in_class ++;
                    sum_x += pts[j].x;
                    sum_y += pts[j].y;
                }
            }
            new_centroids[i].x = (int)sum_x/pts_nums_in_class;
            new_centroids[i].y = (int)sum_y/pts_nums_in_class;
            pts_nums_in_class = 0;
            sum_x = 0;
            sum_y = 0;
        }
        /**if centroids not changed, break out**/
        for (i = 0; i < k_classes; i++)
        {
            if ((new_centroids[i].x != centroids[i].x) || (new_centroids[i].y != centroids[i].y))
            {
                flag = false;
                break;
            }
        }
        if (flag)
            break;
        else
        {
            /**update centroids**/
            for (i = 0; i < k_classes; i++)
            {
                centroids[i].x = new_centroids[i].x;
                centroids[i].y = new_centroids[i].y;
            }
        }
    }
    delete[] centroids;
    delete[] new_centroids;
}

```

