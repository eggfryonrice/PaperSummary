# Uncovering structural diversity in commuting networks: Global and local entropy.

## Introduction

Diverse mobility patterns makes space for adaptation and innovation to maintain the functionaing of the system across different conditions and change. Entropy is one of th emost common ways of quantifying diversity. A common way to measure entropy in graphs in based on the degree distribution, measuring the probability of having a node witha  certain number of links. More recent studies extends information thoery concepts to network to understand entropy in weighted and directed graphs. In commuting networks, entropy is commonly used as a relative measure of the distribution of commuters amongst employmnet locations. Prior studies calculated entropy for individual trajectories or specific nodes within the network, i.e. at a local level. This paper test different measures of entropy at both global and local sale, considering the probability of distribution of flows in both nodes and links. 

## Methods

In this paper, entropy is measured on uncertanty of origin and destination. They used different toy models of commuting networks to compare different measures (with different forms of normalisation) on both local and global scale applied to the links and nodes of the network. These measures consider commuting flows as directed and weighted graphs $G$, represented by set of $n$ vertices V(G) and $m$ edges E(G), and weight $w_{ij}$ is assigned to each edge to represent total flow from origin $i$ to destination $j$.

## Global diversity 

### Spatial distributiono flabour supply and demand

Labour supply and demand are not evely distributed in the geographic space, giving rise to complex patterns of spatial interactions. Entropy measure can explore whether flows in the city tend to be concentraed in dominant areas (monocentric pattern), or evenly dispersed from many origins to many destintaions (polycentric patterns). 

With $p_{ij} = \frac{w_{ij}}{\sum_i \sum_j w_{ij}}$ representing probability of trip from node $i$ to node $j$, we can obtain the diversity of locations from/to which workers go/arrive to work by following global entropy measures:
- Global out-flow entropy at node level:

$$
H^{out}_{GN} = - \sum_{\forall i} \left(\sum_{\forall j} p_{ij}\right) log\left(\sum_{\forall j} p_{ij}\right)
$$

- Global in-flow entropy at node level:

$$
H^{in}_{GN} = - \sum_{\forall j} \left(\sum_{\forall i} p_{ij}\right) log\left(\sum_{\forall i} p_{ij}\right)
$$

In general, commuting destinations tend to be more highly concentrated than the commuting origins, since employment opportunities tends to cluster in few locations. But this does not always holds.

To normalize the result, we get the maximum possible value for each system, which is $H_{Tot} = \log n$. We can get normalized entropies by dividing entropies by $H_{Tot}$.


### Commuter trips distribution

The system will be more diverse if there are many combinations of origin-destination pairs, and amout of those pairs is evenly distributed. Global entropy at link level can be defined as:

$$
H_{GL} = - \sum_{\forall i} \sum_{\forall j} p_{ij} log p_{ij}
$$

Its maximum possible value is $H_{Tot} = \log m$, so we can get normalized entropy by dividing entropy by $H_{Tot}$. More sutable normalization can be obtained by considering maximum possible number o flinks in the graph, which is $n(n-1)$. In this case, we may divide $H_{max}$ instead of $H_{Tot}$. 

## Local diversity

We might also be interested in role of individual locations. They introduced local entropy at in and out commuting scenarios at node $i$:
- Local in-flow entropy:

$$
H_L^{in} = - \sum_{\forall j} \frac{w_{ji}}{\sum_k w_{ki}} log \frac{w_{ji}}{\sum_k w_{ki}}
$$

- Local out-flow entropy:

$$
H_L^{in} = - \sum_{\forall j} \frac{w_{ij}}{\sum_k w_{ik}} log \frac{w_{ij}}{\sum_k w_{ik}} 
$$

Those measure give infomration about the node diversity in therms of the flows that are sent or received by its direct neighbors. Normalisation can be achieved by dividing maximum entropy given by in or out degree of the node, which are $\log(deg_{in}(v_i))$ or $\log(deg_{out}(v_i))$. Or maximum possible value, $\log (n-1)$ may be used.

They also described conditional entropy among all nodes. 

- Average local in-flow entropy:

$$
H^{in}_{L\mu} = \sum_{\forall i} p_i (H_L^{in})_i
$$

- Average local out-flow entropy:

$$
H^{out}_{L\mu} = \sum_{\forall i} p_i (H_L^{out})_i
$$
