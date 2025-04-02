---
title: 轮廓系数算法(测试KMeans，java实现)
tags:
  - 轮廓系数
  - java
categories:
  - 学习
description: "\U0001F347 测试 KMeans 算法选择几个簇心最优解"
abbrlink: cef847c9
date: 2022-04-17 13:17:04
cover:
---
# 轮廓系数算法(测试KMeans，java实现)
```java
/**
 * a(i) - b(i) / max(a(i), b(i))
 * a(i) the average of same cluster
 * b(i) the min average of not same cluster
 * @param data
 * @param result
 * @param flag
 * @param center
 * @return
 */
@Data
public class SilhouetteCoefficient {
    private Set<Cluster> clusters;
    private List<Point> points = new ArrayList<>();
    private List<Point> members = new ArrayList<>();

    public SilhouetteCoefficient(Set<Cluster> clusters) {
        this.clusters = clusters;
    }

    public double getEuclideanDis(Point point1, Point point2) {
        float[] novelInfo1 = point1.getNovelInfo();
        float[] novelInfo2 = point2.getNovelInfo();
        double dist_temp = 0;

        for (int i = 0; i < novelInfo1.length; i++) {
            dist_temp += Math.pow(novelInfo1[i] - novelInfo2[i], 2);
        }
        return Math.sqrt(dist_temp);
    }

    public double run() {
        int index = clusters.size();
        Cluster[] clu = new Cluster[index];

        for (Cluster cluster : clusters) {
            int clusterId = cluster.getId();
            members = cluster.getMembers();
            for (Point member : members) {
                points.add(member);
            }
            clu[clusterId] = cluster;
        }

        double sum = 0.0;
        for (int i = 0; i < index; i++) {
            double inDis = 0.0;

            for (int j = 0; j < clu[i].getMembers().size(); j++) {
                inDis = 0;
                double[] other = new double[index];
                for (int y = 0; y < points.size(); y++) {
                    double dis = getEuclideanDis(clu[i].getMembers().get(j), points.get(y));
                    // 认为是本簇的
                    if (i == points.get(y).getClusterId()) {
                        inDis += dis;
                        continue;
                    }

                    // 不是本簇的
                    for (int k = 0; k < index; k++) {
                        if (points.get(y).getClusterId() == k) {
                            other[k] += dis;
                            break;
                        }
                    }
                }
                // 点到其它簇点的距离和，用数组other存储
                for (int v = 0; v < index; v++) {
                    int size = clu[v].getMembers().size();
                    double avg_tmp = other[v] / size;
                    other[v] = avg_tmp;
                }
                double compute_b;
                double temp;
                for (int b = 0; b < index - 1; b++) {//
                    for (int c = 0; c < index - 1 - b; c++) {
                        if (other[c] > other[c + 1]) {
                            temp = other[c];
                            other[c] = other[c + 1];
                            other[c + 1] = temp;
                        }
                    }
                }

                compute_b = other[1];
                int tag = clu[i].getMembers().size();
                if (tag == 1) {
                    tag = 2;
                }
                double compute_a = inDis / ( tag - 1);
                double s = (compute_b - compute_a) / Math.max(compute_a, compute_b);
                sum += s;

            }
        }
        return sum / points.size();
    }
}

```