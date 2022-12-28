Document Clustering
================
2022-12-26

### About the Study and the Data Set

Document clustering is a technique that uses algorithms to group text
documents into categories based on their content. This can be useful for
organizing search results or other large collections of texts, making it
easier to find relevant information. Clustering methods are often used
by search engines to group search results into categories. There are two
main types of algorithms used for document clustering: hierarchical and
k-means. An example of a website that uses document clustering is Daily
Kos, an American political blog that publishes news and opinion articles
from a progressive perspective. The site was founded in 2002 and has a
large daily traffic.

The file “dailykos.csv” contains data on 3,430 news articles or blog
posts published on the website Daily Kos in the year leading up to the
2004 United States Presidential Election. The candidates in this
election were incumbent President George W. Bush of the Republican Party
and John Kerry of the Democratic Party. The invasion of Iraq in 2003 was
a major focus of the election, particularly in terms of foreign policy.
The dataset includes a list of 1,545 words that appeared at least 50
times across the articles in the dataset. These words have been
processed to remove punctuation and common words known as “stop words.”
For each article, the dataset includes the number of times each word
appears in that article.

### Hierarchical Clustering

``` r
dailykos = read.csv("dailykos.csv")
```

The “hclust” function is being used to create a tree-like diagram
(called a dendrogram) that shows the arrangement of the clusters. The
“ward.D” method specifies the algorithm used to calculate the distance
between clusters. Ward’s method is a method of variance minimization,
which seeks to minimize the sum of squared distances between points in
the same cluster.

``` r
kosDist = dist(dailykos, method = "euclidean")
kosHierClust = hclust(kosDist, method = "ward.D")
```

Let’s plot the tree and see what number of cluster seems approperiate.

``` r
plot(kosHierClust)
```

![](Documents%20Clustering_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

According to the dendrogram, it is advisable to select either 2 or 3
clusters, as these options correspond to points on the dendrogram where
there is a significant distance between the horizontal lines. On the
other hand, the options of 5 or 6 clusters may not be suitable, as there
is very little space between the horizontal lines at these points on the
dendrogram. In other words, the dendrogram indicates that 2 or 3
clusters would result in a clearer separation between groups, while 5 or
6 clusters may result in more overlap between groups. However, the
purpose of clustering these news articles or blog posts is to group them
into categories that can be presented to readers to help them choose
which articles to read. Meaning, this application of clustering aims to
make it easier for readers to find articles that are relevant to their
interests. For this reason it appears that having 7 or 8 cluster groups
is better than having just 2 or 3.

``` r
# Splitting the tree
hierGroup = cutree(kosHierClust, k = 7)
#Splitting the Data
HierCluster1 = subset(dailykos, hierGroup == 1)
HierCluster2 = subset(dailykos, hierGroup == 2)
HierCluster3 = subset(dailykos, hierGroup == 3)
HierCluster4 = subset(dailykos, hierGroup == 4)
HierCluster5 = subset(dailykos, hierGroup == 5)
HierCluster6 = subset(dailykos, hierGroup == 6)
HierCluster7 = subset(dailykos, hierGroup == 7)
```

Let’s explore the new sub data sets.

``` r
nrow(HierCluster1)
```

    ## [1] 1266

``` r
nrow(HierCluster2)
```

    ## [1] 321

``` r
nrow(HierCluster3)
```

    ## [1] 374

``` r
nrow(HierCluster4)
```

    ## [1] 139

``` r
nrow(HierCluster5)
```

    ## [1] 407

``` r
nrow(HierCluster6)
```

    ## [1] 714

``` r
nrow(HierCluster7)
```

    ## [1] 209

It appears that the first Cluster have the most observations, 1266 that
is. Further more, the cluster with the least number of observations is
the 7th cluster with 209 observation!.

Let’s see a graphical representation of the cluster groups.

``` r
plot(kosHierClust)
rect.hclust(kosHierClust, k = 7, border = "red")
```

![](Documents%20Clustering_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

As we mentioned earlier, this data set is fromt the year leading up to
the 2004 election in the United State. That’s why we believe that the
cluster groups will have different representaion of different topics,
like the war on Iraq. For instance, we will explore what different
cluster represent by looking at the most frequent word in the cluster,
in terms of avreage value.

``` r
library(knitr)
kable((tail(sort(colMeans(HierCluster1)))))
```

|            |         x |
|:-----------|----------:|
| state      | 0.7575039 |
| republican | 0.7590837 |
| poll       | 0.9036335 |
| democrat   | 0.9194313 |
| kerry      | 1.0624013 |
| bush       | 1.7053712 |

We can see that the most frequent word in the first cluster in terms of
the avreage value is Bush, which corrisponds to the president George W.
Bush.

Let’s see for the second cluster.

``` r
kable((tail(sort(colMeans(HierCluster2)))))
```

|           |         x |
|:----------|----------:|
| bush      |  2.847352 |
| democrat  |  2.850467 |
| challenge |  4.096573 |
| vote      |  4.398754 |
| poll      |  4.847352 |
| november  | 10.339564 |

In this cluster, we can see that the words “November”, “Poll”, and
“Vote” are the best to describe this cluster. Again, the elections. Now,
let’s see what cluster of the remaining ones is the one related to the
war in Iraq.

``` r
kable((tail(sort(colMeans(HierCluster3)))))
```

|            |        x |
|:-----------|---------:|
| elect      | 1.647059 |
| parties    | 1.665775 |
| state      | 2.320856 |
| republican | 2.524064 |
| democrat   | 3.823529 |
| bush       | 4.406417 |

``` r
kable((tail(sort(colMeans(HierCluster4)))))
```

|          |        x |
|:---------|---------:|
| campaign | 1.431655 |
| voter    | 1.539568 |
| presided | 1.625899 |
| poll     | 3.589928 |
| bush     | 7.834532 |
| kerry    | 8.438849 |

``` r
kable((tail(sort(colMeans(HierCluster5)))))
```

|                |        x |
|:---------------|---------:|
| american       | 1.090909 |
| presided       | 1.120393 |
| administration | 1.230958 |
| war            | 1.776413 |
| iraq           | 2.427518 |
| bush           | 3.941032 |

``` r
kable((tail(sort(colMeans(HierCluster6)))))
```

|          |         x |
|:---------|----------:|
| race     | 0.4579832 |
| bush     | 0.4887955 |
| kerry    | 0.5168067 |
| elect    | 0.5350140 |
| democrat | 0.5644258 |
| poll     | 0.5812325 |

``` r
kable((tail(sort(colMeans(HierCluster7)))))
```

|          |        x |
|:---------|---------:|
| democrat | 2.148325 |
| clark    | 2.497608 |
| edward   | 2.607655 |
| poll     | 2.765550 |
| kerry    | 3.952153 |
| dean     | 5.803828 |

It appears that Cluster 5 with the words “bush”, “iraq”, and “war” is
the one about the war in Iraq. We can also see that Cluser 7 appears to
be representing the democratic party since it has “dean”, “kerry”, and
“edward” amongst its most frequent words. “dean” represents “Howard
Dean” who is one of the democratic nominees for presedency, while
“kerry” is related to “John kerry” the one that actually won the
democratic nomination, and “edward” which corrisponds to “John Edwards”
who is the vice president nominee.

### K-means Clustering

``` r
set.seed(123)
kmc = kmeans(dailykos, centers = 7)
```

``` r
KmeansCluster1 = subset(dailykos, kmc$cluster == 1)
KmeansCluster2 = subset(dailykos, kmc$cluster == 2)
KmeansCluster3 = subset(dailykos, kmc$cluster == 3)
KmeansCluster4 = subset(dailykos, kmc$cluster == 4)
KmeansCluster5 = subset(dailykos, kmc$cluster == 5)
KmeansCluster6 = subset(dailykos, kmc$cluster == 6)
KmeansCluster7 = subset(dailykos, kmc$cluster == 7)
```

Let’s have a look at our clustered sub data sets.

``` r
nrow(KmeansCluster1)
```

    ## [1] 153

``` r
nrow(KmeansCluster2)
```

    ## [1] 386

``` r
nrow(KmeansCluster3)
```

    ## [1] 276

``` r
nrow(KmeansCluster4)
```

    ## [1] 355

``` r
nrow(KmeansCluster5)
```

    ## [1] 47

``` r
nrow(KmeansCluster6)
```

    ## [1] 1883

``` r
nrow(KmeansCluster7)
```

    ## [1] 330

it appears that our k-mean 6th cluster has the most number of
observations, while the 5th one has the least number of observation.  
Now let’s have a look on which group each k-mean cluster represents.

``` r
kable((tail(sort(colMeans(KmeansCluster1)))))
```

|           |        x |
|:----------|---------:|
| primaries | 2.228758 |
| democrat  | 2.529412 |
| edward    | 2.895425 |
| clark     | 3.000000 |
| kerry     | 5.326797 |
| dean      | 7.699346 |

``` r
kable((tail(sort(colMeans(KmeansCluster2)))))
```

|            |        x |
|:-----------|---------:|
| parties    | 1.582902 |
| vote       | 1.590674 |
| state      | 1.911917 |
| elect      | 1.940414 |
| republican | 2.722798 |
| democrat   | 2.810881 |

``` r
kable((tail(sort(colMeans(KmeansCluster3)))))
```

|                |        x |
|:---------------|---------:|
| iraqi          | 1.568841 |
| american       | 1.684783 |
| administration | 1.952899 |
| war            | 2.927536 |
| bush           | 3.213768 |
| iraq           | 4.115942 |

``` r
kable((tail(sort(colMeans(KmeansCluster4)))))
```

|          |        x |
|:---------|---------:|
| state    | 1.307042 |
| campaign | 1.326761 |
| presided | 1.814085 |
| poll     | 2.205634 |
| kerry    | 5.011268 |
| bush     | 8.771831 |

``` r
kable((tail(sort(colMeans(KmeansCluster5)))))
```

|            |         x |
|:-----------|----------:|
| senate     |  3.829787 |
| seat       |  4.021277 |
| state      |  4.914894 |
| republican |  5.617021 |
| parties    |  6.404255 |
| democrat   | 14.276596 |

``` r
kable((tail(sort(colMeans(KmeansCluster6)))))
```

|          |         x |
|:---------|----------:|
| elect    | 0.4625597 |
| general  | 0.5082315 |
| democrat | 0.6070101 |
| poll     | 0.7185343 |
| kerry    | 0.8050982 |
| bush     | 1.1773765 |

``` r
kable((tail(sort(colMeans(KmeansCluster7)))))
```

|           |         x |
|:----------|----------:|
| democrat  |  2.866667 |
| bush      |  3.081818 |
| challenge |  4.127273 |
| vote      |  4.439394 |
| poll      |  4.863636 |
| november  | 10.369697 |

We can first see that k-mean cluster 7 correspond to hierarchal cluster
2, as the most frequent words in both clusters are “november”, “poll”,
and “vote”. The 3rd k-mean Cluster corresponds to the war on Iraq, which
as we explored earlier happens to be in the 5th hierarical cluster.
Finally, “dean”, “kerry”, and ” edward” which we spoke about before in
the 7th hierarchal cluster, are represented in the 1st k-means cluster.

We can confirm all these observations by observing the following table:

``` r
kable(table(hierGroup, kmc$cluster))
```

|   1 |   2 |   3 |   4 |   5 |   6 |   7 |
|----:|----:|----:|----:|----:|----:|----:|
|  10 | 190 |  43 |  64 |   0 | 959 |   0 |
|   0 |   0 |   1 |   0 |   0 |   0 | 320 |
|  10 | 146 |  54 |  79 |  46 |  30 |   9 |
|   8 |   4 |   0 | 123 |   0 |   4 |   0 |
|   0 |  30 | 178 |  87 |   0 | 111 |   1 |
|   2 |  15 |   0 |   0 |   0 | 697 |   0 |
| 123 |   1 |   0 |   2 |   1 |  82 |   0 |
