---
title: kmeans算法（java实现）
tags:
  - kmeans
  - java
categories:
  - 学习
description: "\U0001F361 对聚类算法一次尝试，使用 java 算法实现，目的在于对小说数据的分类"
abbrlink: b6ce25ff
date: 2022-04-16 11:24:37
cover:
---
# kmeans算法（java实现）
```java
package com.redbean.noveldata;

import lombok.Data;

import java.util.*;

/**
 * @author redbean
 * @create 2022/4/15 - 16:39
 */
public class kMeansTest {

    public static void main(String[] args) {
        ArrayList<float[]> dataSet = new ArrayList<float[]>();
        dataSet.add(new float[] {1000025, 434464, 24143});
        dataSet.add(new float[] {1000060, 557784, 62171});
        dataSet.add(new float[] { 1, 4, 4});
        dataSet.add(new float[] { 2, 6, 5});
        dataSet.add(new float[] { 3, 9, 6});
        dataSet.add(new float[] { 4, 5, 4});
        dataSet.add(new float[] { 5, 4, 2});
        dataSet.add(new float[] { 6, 9, 7});
        dataSet.add(new float[] { 7, 9, 8});
        dataSet.add(new float[] { 8, 2, 10});
        dataSet.add(new float[] { 9, 9, 12});
        dataSet.add(new float[] { 10, 8, 112});
        dataSet.add(new float[] { 11, 8, 4});
        KMeans kMeans = new KMeans(3, dataSet);
        Set<Cluster> run = kMeans.run();
        System.out.println("迭代次数：" + kMeans.getIterRunTimes());
        for (Cluster cluster : run) {
            System.out.println(cluster.getId());
            System.out.println(cluster.getCenter());
            System.out.println(cluster.getMembers());
            System.out.println("====================");
        }
    }
}

@Data// 点的格式
class Point {
    private float[] novelInfo;
    private float Id;
    private int clusterId;  // 簇id
    private float dist;     // 与中心簇的欧几里得距离

    public Point(float Id, float[] novelInfo) {
        this.novelInfo = novelInfo;
        this.Id = Id;
    }

    public Point(float[] novelInfo) {
        this.Id = -1;       // 表示不属于任何一个类
        this.novelInfo = novelInfo;
    }
}

@Data
        // 簇群
class Cluster {
    private int id; // 簇id
    private Point center; // 中心簇点
    private List<Point> members = new ArrayList<>(); // 成员

    public Cluster(int id, Point center) {
        this.id = id;
        this.center = center;
    }

    public Cluster(int id, Point center, List<Point> members) {
        this.id = id;
        this.center = center;
        this.members = members;
    }

    public void addPoint(Point newPoint) {
        if (!members.contains(newPoint)) {
            members.add(newPoint);
        } else {
            System.out.println("<<<<<<<<<<<<<<<<<< 样本数据{" + newPoint + "} 已经存在>>>>>>>>>>>>>>>>>>>>>>>");
        }
    }
}

// 计算欧氏距离
class DistanceCompute {
    public double getEuclideanDis(Point point1, Point point2) {
        float[] novelInfo1 = point1.getNovelInfo();
        float[] novelInfo2 = point2.getNovelInfo();
        double dist_temp = 0;

        for (int i = 0; i < novelInfo1.length; i++) {
            dist_temp += Math.pow(novelInfo1[i] - novelInfo2[i], 2);
        }
        return Math.sqrt(dist_temp);
    }
}

// 计算类，输入：k，原始数据
class KMeans {
    private int kNum;                                    // 簇的个数
    private final int ITER_SUM = 10;                     // 迭代次数

    private final int ITER_MAX_TIMES = 1000000000;       // 单次迭代的最大运行次数
    private int iterRunTimes = 0;                        // 单次迭代运行的实际次数
    private final float ITER_STOP = (float) 0.01;        // 单次迭代终止条件，类的距离差

    private List<float[]> novelData = null;                // 原始数据
    private static List<Point> pointList = null;          // 原始数据构成的点集合
    private DistanceCompute distanceCompute = new DistanceCompute();    // 创建欧氏距离计算
    private int len = 0;                                  // 每个数据点的维度

    public KMeans(int kNum, List<float[]> novelData) {
        this.kNum = kNum;
        this.novelData = novelData;
        // 初始化点集
        init();
    }

    // 将每个原始数据转换为点类
    private void init() {
        pointList = new ArrayList<Point>();
        len = novelData.get(0).length - 1;
        // 将novelid赋值点id，后面的数据赋值给点novelinfo
        int tmp_len = novelData.get(0).length - 1;
        for (int i = 0, j = novelData.size(); i < j; i++) {
            float[] tmp = new float[tmp_len];;
            for (int f = 0; f < tmp_len; f++) {
                tmp[f] = novelData.get(i)[f + 1];
            }
            pointList.add(new Point(novelData.get(i)[0], tmp));
        }
    }

    // 第一次，随机选择中心点，构成中心簇点（这个时候一个簇里只有一个点，别的都没有）
    public Set<Cluster> chooseCenterCluster() {
        HashSet<Cluster> clusterHashSet = new HashSet<Cluster>();
        Random random = new Random();
        // 创建k个簇
        for (int id = 0; id < kNum; ) {
            // 中心簇点
            Point point = pointList.get(random.nextInt(pointList.size()));
            // 标记是否已经选择该数据
            boolean flag = true;
            for (Cluster cluster : clusterHashSet) {
                if (cluster.getCenter().equals(point)) {
                    flag = false;
                }
            }
            if (flag) {
                Cluster cluster = new Cluster(id, point);
                clusterHashSet.add(cluster);
                id++;
            }
        }
        return clusterHashSet;
    }

    // 对每个点分配一个簇
    // 传入簇
    public void cluster(Set<Cluster> clusterSet) {
        // 计算每个点到每个中心簇点的欧式距离，并为每个点标记簇号
        for (Point point : pointList) {
            float min_dis = Integer.MAX_VALUE;
            for (Cluster cluster : clusterSet) {
                // 跟每个簇心进行欧式距离比较，如果后一个簇心的距离比前一个簇心的距离要小的话，这个点归为后一个簇类中
                // 如果到后一个簇心的距离比到前一个簇心大的话，保持前一个簇心的距离和类别
                float tmp_dis = (float) Math.min(distanceCompute.getEuclideanDis(point, cluster.getCenter()), min_dis);
                if (tmp_dis != min_dis) {
                    min_dis = tmp_dis;
                    point.setClusterId(cluster.getId());
                    point.setDist(min_dis);
                }
            }
        }
        // 以上操作只是对点进行标记，并没有对簇进行改动
        // 下面清空簇的成员，按照点的簇id重新加载簇
        for (Cluster cluster : clusterSet) {
            cluster.getMembers().clear();
            for (Point point : pointList) {
                if (point.getClusterId() == cluster.getId()) {
                    cluster.addPoint(point);
                }
            }
        }
    }

    // 计算每个类的中心位置
    public boolean getUpdate(Set<Cluster> hashSet) {
        boolean isNeedIter = false;
        for (Cluster cluster : hashSet) {
            List<Point> members = cluster.getMembers();
            float[] sumAll = new float[len];
            // 各维度求和，（按簇类的列求和）
            for (int i = 0; i < len; i++) {
                for (int j = 0; j < members.size(); j++) {
                    sumAll[i] += members.get(j).getNovelInfo()[i];
                }
            }
            // 计算平均值
            for (int i = 0; i < len; i++) {
                sumAll[i] = (float) sumAll[i] / members.size();
            }

            // 计算新旧簇心的距离，如果距离大于设定的距离ITER_STOP则继续迭代
            if (distanceCompute.getEuclideanDis(cluster.getCenter(), new Point(sumAll)) > ITER_STOP) {
                isNeedIter = true;
            }
            // 设置簇的中心位置
            cluster.setCenter(new Point(sumAll));
        }
        return isNeedIter;
    }

    // 运行kmeans
    public Set<Cluster> run() {
        Set<Cluster> clusters = chooseCenterCluster();
        boolean ifNeedIter = true;
        while (ifNeedIter) {
            cluster(clusters);
            ifNeedIter = getUpdate(clusters);
            iterRunTimes ++;
        }
        return clusters;
    }

    // 返回真正的运行次数
    public int getIterRunTimes() {
        return iterRunTimes;
    }
}


```