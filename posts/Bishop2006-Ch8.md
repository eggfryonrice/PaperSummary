# Pattern Recognition and Machine Learning - Graphical Models

Probabilistic graphical models have useful properties
- Provide a simple way to visualize the structure of a probablistic model
- Give insights into properties of the model including conditional independence properties.
- Complex computations requred to perform inference and learning in sophisticated models can be expressed in terms of graphical manipulations.

In probabilistic graphical model, each node of the graph represents a random variable, and links express probablistic relationships between variables. Directed graphs are useful for expressing causal relationships between random variables, whereas undirected graphs are better suited to expressing softe constraints between random variables. It is often convenient to convert both types of graph into factor graph.

## Baysian Networks

For any choice of joint distribution, we can decompose it like below:

$$
p(x_1, \dots, x_K) = p(x_K|x_1, \dots, x_{K-1})\dots p(x2|x1)p(x1)
$$

We may represent right hand side of the above into simple graphical model, by adding directed links to each conditional distribution. This will result in fully connected graph. If there is a link going from node $a$ to node $b$m we say node $a$ is parent of node $b$ and node $b$ is child of node $a$. Note that this can happen with completely general joint distributions. 

For general graph with absense of some links, joint probability distribution is product of set of conditional distribution of each node, each conditioned on their parents. hence,

$$
p(\mathbf{x}) = \prod^K_{k=1} p(x_k|\mathrm{pa}_k)
$$

where $\mathrm{pa}_k$ denotes set of parents of $x_k$. 

The directed graphs we consider have restriction that there must be no directed cycles. Such graphs are called directed acyclic graphs, or DAGs.  This is equivaled to the statement that there exists an ordering of the nodes.

### Example: Polynomial regression
Consider Bayesian polynomial regression model, with polynomial coefficients $\mathbf{w}$, and observed data $\mathbf{t} = (t_1, \dots, t_N)^T$. We also have input data $\mathbf{x} = (x_1, \dots, x_N)^T$, noise variance $\sigma^2$, and $\alpha$ is precision of Gaussian prior over $\mathbf{w}$. Then we have

$$
p(\mathbf{t}, \mathbf{w}) = p(\mathbf{w}) \prod^N_{n=1} p(t_n|\mathbf{w})
$$

So we have graphical model with directed links from $\mathbf{w}$ to $t_i$'s. Instead of drawing all nodes $t_1 \dots t_N$, we may draw single representative node $t_n$ and surround it with a box, called plated, labeled with $N$ indicating that ther eare $N$ nodes of this kind.

When considering parameters,

$$
p(\mathbf{t}, \mathbf{w}|\mathbf{x}, \alpha, \sigma^2) = p(\mathbf{w}|\alpha) \prod^N_{n=1} p(t_n|\mathbf{w}, x_n, \sigma^2)
$$

We can put $\mathbf{x}$, $\alpha$, and $\sigma^2$ explicitly in the graphical representation. 

In machine learning problem $t_n$ is obsered variable, and we represent int by shading the corresponding nodes. Nonobserved variables, in this case $\mathbf{w}$ are called latent variable. 

In general, our ultimate goal is prediction itself, not $\mathbf{w}$. When we have input $\hat{x}$ and $\hat{t}$. then,

$$
p(\hat{t}, \mathbf{t}, \mathbf{w} \mid \hat{x}, \mathbf{x}, \alpha, \sigma^2)
= \left[ \prod_{n=1}^{N} p(t_n \mid x_n, \mathbf{w}, \sigma^2) \right] 
p(\mathbf{w} \mid \alpha) p(\hat{t} \mid \hat{x}, \mathbf{w}, \sigma^2)
$$

Then we can get graphs with $\mathbf{t}, \mathbf{w}, \hat{t}$ as nodes, $\alpha, \sigma^2, \mathbf{x}, \hat{x}$ as parameters. We have 

$$
p(\hat{t} \mid \hat{x}, \mathbf{x}, \mathbf{t}, \alpha, \sigma^2)
\propto \int p(\hat{t}, \mathbf{t}, \mathbf{w} \mid \hat{x}, \mathbf{x}, \alpha, \sigma^2) \, \mathrm{d}\mathbf{w}
$$

### Generative models

Consider joint distribution $p(x_1, \dots, x_K)$. Suppose that those variables have been ordered such that there is no links from any node to any lower numbered node. We want to sample $\hat{x_1}, \dots, \hat{x_K}$ from the joint distribution. To do that, we sample $x_n$ from $p(x_n\|\mathrm{pa}_n)$ consecutively for $n = 1\dots K$. 

For practical appliations, higher numbered variables would correspond to observations, while lower numbered nodes corresponding to latent variables. Role of latent variable is to allow complicated distribution over the observed variables to be represented in terms of a model constructed from simpler (typically exponential family) conditional distributions.

The graphical model captures the causal process by which the observed data was generated. Such models are often called generative models. The hidden variables in a probabilitic model need not have any explicit physical interpretation but may be introduced simply to allow a more complex joint distributions to be constructed from simpler components. 

### Discrete variables

The probability distribution $p(x\|\mathbf{\mu})$ for single discrete variable $\mathbf{x}$ having $K$ possible states is given by 

$$
p(\mathbf{x}|\mathbf{\mu}) = \prod^K_{k=1} \mu_k^{x_k}
$$

When we have two discrete variables $\mathbf{x}_2, \mathbf{x}_2$, each with $K$ states. then

$$
p(\mathbf{x}_1,\mathbf{x}_2|\mathbf{\mu}) = \prod^K_{k=1} \mu_{kl}^{x_{1k}x_{2l}}
$$

And here, number of parameters that must be specified for joint distribution over $M$ variables is $K^M - 1$, which grows exponentially with respect to $M$.

When we seperate $p(\mathbf{x}_1,\mathbf{x}_2)$ into $p(\mathbf{x}_2\|\mathbf{x}_1) p(\mathbf{x}_1)$, then we need $K-1$ parameters for $p(\mathbf{x}_1)$, and $K(K-1)$ parameters for $p(\mathbf{x}_2\|\mathbf{x}_1)$, so we have total $K^2-1$ parameters, which is the same number as before. 

If $\mathbf{x}_1,\dots, \mathbf{x}_M$ are independent, total number of parameters would be $M(K-1)$, which grows linearly. From graphical perspective, we have reduced number of parameters by dropping links in the graph, at the expense of having a restricted class of distributions.

If graph is fully connected, we need $K^M-1$ parameters. If there is no link in the graph, we need $M(K-1)$ parameters. Graphs with intermediate levels of connectivity may be more general than fully factorized one but have less parmeters than general joint distribution. For example, with graph with chain of node, so that $\mathbf{x} _i$ is conditioned only on $ \mathbf{x} _{i-1} $, we need $ K-1 + (M-1)K(K-1) $ parameters. 

Alternative way of reducing number of independent parameters is by sharing parameters. For example, if $ p( \mathbf{x} _i \| \mathbf{x} _{i-1} ) $ for $ i=2,\dots,M $ are governed by same $ K(K-1) $ parameters, we need total $ K^2-1 $ parameters to define joint distribution.

Another way of controlling number of parameters is to use parameterized models for conditional distributions instead of complete tables of conditional probability values. In the case when parent binary variables $x_i$ is governed by $\mu_i = p(x_i = 1)$, we nee $2^M$ paremeters to represent $p(y\|x_1, \dots, x_M)$ for binary $y$. But by giving a restriction on conditional distribution, we may represent probability via logistic sigmoid function acting on linear combinations of the parent variables.

$$
p(y=1 \mid x_1, \dots, x_M) = \sigma\left( w_0 + \sum_{i=1}^{M} w_i x_i \right)
$$

### Linear-Gaussian models

Multivariate Guassian can be expressed as a directed graph corresponding to a linear-Gaussian model over the component variables. We can impose structure of distribution with general Gaussian and diagonal covariance Guassian representing opposite extremes. 

Consider arbitrary directed acyclic graph over $D$ variables where each node $i$ represent single random variable $x_i$ having Gaussian distribution. Mean of distribution is taken to be linear combination of the staes of its parent nodes.

$$
p(x_i \mid \mathrm{pa}_i) = \mathcal{N}\left( x_i \,\middle|\, \sum_{j \in \mathrm{pa}_i} w_{ij} x_j + b_i, v_i \right)
$$

where $w_{ij}$ and $b_i$ are parameters governing the mean, and $v_i$ is variance of the conditional distribution for $x_i$.

$$
\ln p(x) = \mathrm{const} - \sum_{i=1}^D \frac{1}{2v_i} \left(x_i - \sum_{j \in \mathrm{pa}_i} w_{ij}x_j - b_i\right)^2
$$

We have

$$
\mathbb{E}[x_i] = \sum_{j\in\mathrm{pa}_i} w_{ij} \mathbb{E}[x_j] + b_i
$$

$$
\mathrm{cov}[x_i, x_j] = \sum_{k\in\mathrm{pa}_j} w_{jk}\mathrm{cov}[x_i, x_k] + I_{ij}v_j
$$

So expectation and covariance can be evaluated recursively starting from the lowest numbered node.

If there is no links in the graph, thre will be $2D$ parameters needed to represent distribution. If graph is fully connected, which means each node has all lower numbered nodes as parents, then matrix $w_{ij}$ is lower triangular matrix. This matrix needs $D(D-1)/2$ parameters, so $D(D+1)/2$ parameters are needed in total.


## Conditional Independence

We say $a$ is conditionally independent of $b$ given $c$ if 

$$
p(a|b,c) = p(a|c)
$$

then, 

$$
p(a,b|c) = p(a|b,c)p(b|c) = p(a|c)p(a|b)
$$

We sometimes use shorthand notation

$$
a \perp\!\!\!\perp b \mid c
$$

From graphical model, we can read the independence properties of the joint distribution directly from the graph.

### Example of conditional independence

In the case when 

$$
p(a,b,c) = p(a|c)p(b|c)p(c)
$$

$a$ and $b$ are conditionally independent on $c$ though $a$ and $b$ are not independent. We can represent it by graph with node $c$ pointing node $a$ and $b$. When node $c$ is determined, the conditioned node blocks the path from $a$ to $b$ on graph. 

In the case when 

$$
p(a,b,c) = p(a)p(c|a)p(b|c)
$$

$a$ and $b$ are not indepdendent, but we can checked that $a$ and $b$ are independent conditioned on $c$. We can represent this by graph where $a$ is pointing $c$, $c$ is pointing $b$. When node $c$ is conditioned, the path from $a$ to $b$ is blocked.

In the case when

$$
p(a,b,c) = p(a)p(b)p(c|a,b)
$$

Then marginalizing both side over $c$ will give us $p(a,b)=p(a)p(b)$, which means independence. But $a$ and $b$ are not independent conditioned on $c$. We can represent this by graph where $a$ and $b$ are pointing $c$. When node $c$ is not conditioned, path from $a$ to $b$ is blocked, but conditioning on c unblocks the path, which is opposite behavior.

In summary, tail-to-tail node or head-to-tail node is unblocked unless it is observed, head-to-head node blocks unless it is observed. 


### D-separation

Consider general directed graph, and take $A,B,C$ be arbitrary nonintersecting set of nodes. To check if $A$ and $B$ are independent conditioned on $C$, we check all possible paths from node in $A$ to node in $B$, about whether it is blocked. Path is said to be blocked if one of the below condition holds:
- arrows on the path meet either head-to-tail or tail-to-tail at the node, and node is in the set $C$
- arrows meet head-to-head at the node, and neither the node,nor any of its descendants is in the set $C$.
If all paths are blocked, $A$ is said to be d-separated from $B$ by $C$, and it means $A$ and $B$ are independent conditioned on $C$. 

For purpose of d-separation, paremeters behave in the same was as observed nodes.  Since parameter nodes doesn't have parents, paths trhough those nodes are always tail-tail, hence they play no role in d-separation

Consider problem of finding the posterior distribution for mean of univariate Gaussian distribution. It is represented by directed graph where each $x_1, \dots x_N$ are conditioned on $\mu$, which follows prior $p(\mu)$. Then every path from $x_i$ to $x_j$ is blocked, so $x_i$'s are independent conditioned on $\mu$. However, observations are no longer independent if we integrate over $\mu$.

Consider Bayesian polynomail regression, with nodes $t_n, \mathbf{w}, \hat{t}$. Then we have $\hat{t}$ is independent with $t_n$ conditioned on $\mathbf{w}$. Thus, after we get posterior distribution of $\mathbf{w}$, we don't have to make predictions of $\hat{t}$ for new input oveservations $\hat{x}$.

Consider naive Bayes model, which we use conditional independence assumptions to simplify model structure. Suppose we observed $\mathbf{x}=(x_1,\dots,x_D)$, and we want to assigned $\mathbf{x}$ to one of $K$ classes. We represent those classes by $K$-dimensional binary vector $\mathbf{z}$, and define generative model by introducing multinomial prior $p(\mathbf{z}\|\mathbf{\mu})$ over class labels. Here, conditioned on $\mathbf{z}$, $x_1, \dots, x_D$ are independent. But marginalizing over $\mathbf{z}$, which can be interpreted as $\mathbf{z}$ is unobserved, $x_1, \dots, x_D$ are not independent anymore. For example, if probability density within each class id Gaussian, convariancce matrix for each Gaussian is diagonal. This simplification can be useful when dimension $D$ is high.

Consider a filter that allow distribution $p(\mathbf{x})$ to pass iff it can be expressed in terms of factorization implaied by the graph. We will call set of distribution that can be filtered as $\mathcal{DF}$. Alternatively, if we make a set of distributions, by putting all the distribution satisfying conditional independence for all graph, it will be $\mathcal{DF}$. This is due to d-separation theorem. Note that the graph doesn't contain distribution of each variables, so a particular graph describes family of probability distribution.

Consider joint distribution $p(\mathbf{x} _1, \dots, \mathbf{x} _D)$ represented by directed graph. Consider conditional distribution of of a particular node $\mathbf{x} _i$. Then

$$
p(\mathbf{x}_i \mid \mathbf{x}_{\{j \neq i\}}) =
\frac{p(\mathbf{x}_1, \ldots, \mathbf{x}_D)}{\int p(\mathbf{x}_1, \ldots, \mathbf{x}_D) \, \mathrm{d}\mathbf{x}_i}
=
\frac{\prod_k p(\mathbf{x}_k \mid \mathrm{pa}_k)}{\int \prod_k p(\mathbf{x}_k \mid \mathrm{pa}_k) \, \mathrm{d}\mathbf{x}_i}
$$

Here, $p(x_k\|\mathrm{pa}_k)$ does not have any functional dependence of $x_i$ if $i\neq k$, so it can be taken outside of integral. Then  factors remaining will be $p(\mathbf{x} _i \| \mathrm{pa}_i)$ and conditional distribution of $\mathbf{x} _k$ conditioned on $\mathbf{x} _i$. $p(\mathbf{x} _i \| \mathrm{pa}_i)$ depends on parent nodes of $\mathbf{x} _i$, whereas $p(\mathbf{x} _k \| \mathrm{pa}_k)$ depends on children of  $\mathbf{x} _i$ as well as on the co-parents, which means parents of $\mathbf{x} _k$ other than $\mathbf{x} _i$. The set of nodes including parents, children, and co-parents is called Markov blanket, and can be interpreted as minimal set of nodes that isolates $\mathbf{x} _i$ from rest of the graph.

## Markov random fields.

Markov random field, also known as markov network or an undirected graphical model, has set of nodes each of which corresponds to a varaible or gropu of variables, as well as set of undirected links each of which connects a pair of nodes. 

### Conditional independence properties

Removing the directionality from the links of the graph removes asymmetry between parent and child nodes. To check the conditional independence property of undirected graph, we consider all possible path connected one node to other node, and check if all such path pass through one or more nodes in condition set. If th4ere is at least one such path that is not blocked, conditional independence does not necessarily hold.

Markov blanket for undirected graph is all neighbouring nodes.

### Factorization properties

cliques is suabset of the nodes in a graph that are fully connected. Maximal clique is clique that is not possible to further include any other nodes. 

Considr clique $C$ and set of variables in that clique $\mathbf{x}_C$. Now we write joint distribution as product of potential functions over maximal cliques $\psi_C(\mathbf{x}_C)$. Then

$$
p(\mathbf{x}) = \frac{1}{Z} \prod_{C} \psi_C(\mathbf{x}_C).
$$

where $Z$ is normalization constant to make $p(\mathbf{x})$ to be sum to 1. 


### Image denoising

Let the observed noisy image be array of binary pixel values $y_i \in \{ -1, +1 \} $, where index $i = 1, \dots, D$ runs over all pixels. We suppose image is obtained from unknown noise-free images, described by $x_i \in \{ -1, +1 \} $. Let's consider the corrupted image is obtained by flipping the signe of the pixel with probability 10% for example.

Here, there is strong correlation between $x_i$ and $y_i$, and there are strong correlation between neighboring pixels $x_i$ and $x_j$. So when we draw a graph, graph has two types of cliques.

Cliques of a form $\{ x_i, y_i \}$ have energy function that expresses correlation between these variables. We may choose simple potential function $\mathrm{exp}(\eta x_i y_i)$ for positive constant $\eta$. We assign similar potential function to cliques of the form $\{x_i, x_j\}$. We give $\mathrm{exp}(\beta x_i x_j)$ where $\beta$ is positive constant. 

We may also add extra term $\mathrm{exp}(h x_i)$ for each pixel of noise free image, to give bias to the model towards pixel values of one particular sign. 

Complete energy cuntion takes the form 

$$
E(\mathbf{x}, \mathbf{y}) = h \sum_i x_i - \beta \sum_{\{i,j\}} x_i x_j - \eta \sum_i x_i y_i
$$

and resulting joint distribution is

$$
p(\mathbf{x}, \mathbf{y}) = \frac{1}{Z} \exp\{-E(\mathbf{x}, \mathbf{y})\}
$$

fixing elements of $\mathbf{y}$ to observed values defines conditional distribution $p(x\|y)$. To find $\mathbf{x}$ with high probability, we use iterative technique called iterated conditional models, or ICM, which is application of coordinate-wise gradient ascent. We start by $x_i = y_i$ for all $i$, then take one $x_j$, and evaluate total energy when $x_j$ is changed, fixing all others, and set $x_j$ to state with lower energy.  We repeate until suitable stopping criterion is satisfied.

There are some more effective alrogithm, such as graph cuts.

### Relation to directed graphs

For simple example, for directed graph 

$$
p(\mathbf{x}) = p(x_1) p(x_2 \mid x_1) p(x_3 \mid x_2) \cdots p(x_N \mid x_{N-1})
$$

This can be converted into undirected graph representation

$$
p(\mathbf{x}) = \frac{1}{Z} \psi_{1,2}(x_1, x_2) \psi_{2,3}(x_2, x_3) \cdots \psi_{N-1,N}(x_{N-1}, x_N)
$$

Conversion can be done by identifying $\psi_{1,2}(x_1, x_2) = p(x_1)p(x_2\|x_1)$, $\psi_{i,i+1}(x_i, x_{i+1}) = p(x_{i+1} \| x_i)$

Acrtually, we can convert any distribution specified by factorzation over directed graph into one specified by factorization over undirected graph. 