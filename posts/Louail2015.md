
# Uncovering the spatial structure of mobility networks

## Introduction 

Origin-destination (OD) matrices encapsulates the complete information about the flow of individuals in a city, at a given spatial scale and for specific prupose. It is difficult to extract high-level, synthetic information fro large networks. Methods such as community detection and stochastic block modeling were recently proposed, but they are hard to be applied to commuting networks in cities, where edge represent flows of individuals that travel daily from their residential neighborhood to their main activity area. Those links can be distinguished into severl types, (major residential area to employment centres, smaller residential aras to important employment centres, major residential neighbourhoods to smaller activity areas ...) and spatial properties of these commuting flows are fundamental in cities.

In this paper, they propose simple and versatile method to compare structure of large, weighted and directed networks. Then they apply the mdthod to commuting OD matrices of 31 cities extracted from large mobile phone data set, and discuss patterns the their method reveals. Then, they use method to determine categories of networks with respect to theri structure.

## ICDR values

For a given city, we have $n \times n$ matrix $F_{ij}$ where $n$ is the number of spatial units the compose the city at the spatial aggregation level considered. $F_{ij}$ represents the number of individuals living in location $i$ and commuting to location $j$. When computing number of inhabitants and workers in each cell, we do not consider the diagonal of the OD matrix, i.e. person who live and work at the same cell.

They extract residential and work locations with a large density. Then, they reorder rows and columns of OD matrix to separate hotspotsfrom non-hotspots, by putting $m$ residential hotspots on top lines, and $p$ work hotspots on left columns. Then the OD matrix becomes four quadrant matrix. Then by normalizing the quadrant with the total number of commuters in the OD matrix, we can reduce the OD matrix to $2\times 2$ matrix $\Lambda$

$$
\Lambda = \begin{pmatrix}
I & D \\
C & R
\end{pmatrix}
$$

where

$$
I = \left(\sum_{i \in 1..m,\, j \in 1..p} F_{ij}\right) / \left(\sum_{i,j \in 1..n} F_{ij}\right)
$$
$$
C = \left(\sum_{i \in m+1..n,\, j \in 1..p} F_{ij}\right) / \left(\sum_{i,j \in 1..n} F_{ij}\right)
$$
$$
D = \left(\sum_{i \in 1..m,\, j \in p+1..n} F_{ij}\right) / \left(\sum_{i,j \in 1..n} F_{ij}\right)
$$
$$
R = \left(\sum_{i \in m_1..n,\, j \in p+1..n} F_{ij}\right) / \left(\sum_{i,j \in 1..n} F_{ij}\right)
$$

then $I$ is proportion of integrated flows from residential hotspots to work hotspots, $C$ is proportion of convergent flows  from random residential places to work hotspots, $D$ is proportion of divergent flows from residential hotspots to activity places. $R$ is proportion of random flows that occur at random in the city. We have $I+C+D+R = 1$.


## Application of ICDR to Spanish cities

They infer most likely locations of one's home and workplace by using geolocated activity of one's mobile phone. By aggregating them, they constructed OD matrices. There can be variety of data collection protocols, depending on nature of data source (survey of geolocated data) and spatial sale of OD matrix (division in wards, countries, municiplaties, ...). But it is known that various source os pervasive data provide very similar mobility information when compared with OD matrices built from surveys. 

They applied ICDR method to OD matrices extracted from 31 Spanish urban areas during 5-week period. To determine hotspots, they used parameter-free method based on Lorenz curve of the densities. Then, for each city, numbers of residential employment hostposts scale sublinearly with the population size. Number of work hotspots grow significanly slower than number of resideential hotspots, showing that residential areas are more dispersed in the city and are more numerous than activity centres. They plotted $I,C,D,R$ values and compared with population size of these cities. They observed that proportion $I$ decrease, $R$ increase, and $C$ and $D$ remains constant, as population size increases.

They used several more methods to analyze ICDR values.

### Null model
They used null model of commuting flows to evaluate to waht extent the ICDR signatures of cities are charactersistic of their commuting structure. For each city, they generated random OD matrices of the same size with random flows of individuals preserving in and out degree of each node. They observed Z-scores of ICDR values of each city compared to values by null model. They observed that Z-scores of $I$ and $R$ are positive and increase as cities grow, while $C$ and $D$ are negative and decreases as cities grow. The result demonstrates that the larget a city, the less random the values are, which is in contrast with naive expectation that larget city would have more disordered structure of individual's mobility.

### Robustness of ICDR values

It is needed to check if ICDR values are robust under choice of hotspots. To identify hotspot, we should choose threshold of density $ \rho^* $ so that cell $i$ is hotpoit iff $ \rho(i) > \rho^* $. They measured sensitivity of the result to $ \rho^* $.  Whatever density threshold $ \rho^* $ chosen, they observed the same qualitative behavior.

### Distance of each type of flows

They computed average distance travelled by individuals per tyep of flows in ICDR. They observed that average distance for all categories of flows increases with population size, which seems natural. They observed that average distance of C increases faster than other types of flows. This means commuters from small residential aras to important activity centres travel on average longer distance than all other individuals. 

High impact of city size on $C$ may be because residential areas have expanded while activity centres remained at their location.

When we compare the average distance from average distance obtained by null model, for all types of flows, distance measured in the empirical data are shorter than that of null model. Average distance of C increases the fastest even with null model. 

Distance ratio, $\overline{D_{null}} / \overline{D_{Data}}$ was highest for $R$ than $D,C,D$, suggesting spatial structure of $R$ flows. 

When considering fraction of total commuting distance by type of flow, they observed that respective fraction is constant and independent of city size. 

### Classification of cities

They cluster cities by euclidean distance of their ICDR values. They ovserved that largest cities are clustered together, characterized by larget proportion of $R$. This can be interpreted as increased facility in bigger urban areas to commute from any part of the city to any other part. They also checked that clustering is robust by comparing the clustered result after giving reasonable amount of noise.

## Discussion

By above methodology, they found out that
- Proportion of integrated flows ($I$) decreases with city size, while proportion of random flows ($R$) increases.
- Individuals living in residential hot spot who work ine employment main hub ($I$ flows) tavel shorter distances than the others.
- As city size increases, largest impact is on convergent flows ($C$), individuals living in smaller residential areas and commuting to important employment centres.
- Classification of cities based on ICDR values leads to gropus with consistent population size.
- Comparison with null model suggests that mobility gets more specific and be far from random organization as city grows.
