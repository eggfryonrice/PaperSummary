# Reconciling modern machine learning practice and the bias-variance trade-oﬀ

I am reading this paper to see how bias-variance tradeoff I learned in CS376 can be applied to DNN, which has different properties compared to classical models.

## Introduction 
Machine learning on the problem of prediction does the following:
given training examples from $\mathbb{R}^d \times \mathbb{R}$, we learn predictor $h_n: \mathbb{R}^d \rightarrow \mathbb{R}$ by choosing from some function class $\mathcal{H}$, which is neural network with certain architecture, by minimizing empirical risk $\frac{1}{n} \sum_{i=1}^{n} \ell(h(x_i), y_i)$

The challenge here is mismatch between minimzaing empirical risk and minimizing true (or test) risk $\mathbb{E}_{(x,y)~P}[l(h(x), y)]$. Conventional wisdom in mathince learning controls the capacity of $\mathcal{H}$ based on bias-variance tradeoff by finding the "sweet spot" between underfitting and overfitting. Control of function class can be explicit by choice of $\mathcal{H}$, or be implicit using regularization. The performance of the model shows U-shaped risk curve.

But in modern mathince learning, large neural networks with hight function class capacity and near-perfect fit to training data often give very accurate predictions on new data. Recent empirical evidence indicates taht neural networks and kernel mathinces trained to interpolate the training data obtain near optimal test results even when the training data are corrupted with high levels of noise.

The main finding of this paper is a pattern, "double descent" risk curve, which is pattern of how performance on unseen data depends on model capacity, and the meahcanism underlying its emergence. When funtion class capacity is below interpolation threshold, learend predictors exhibit the classical U-shaped curve. But increasing the function class capacity beyond interpolation threshold leads to decreasing risk, typically going below the risk achieved at the sweet point in classical regime. Learned predictors on the right of the interpolation threshold fit the training data perfectly and have zero empirical risk, but those frome richer function classes tends to smaller norm and are thus simpler. 


## Neural networks
### Random Fourier features 
The paper first consider popular class of nonlinear parametric models called Random Fourier Features (RFF). It can be vieewd as a class of two layer neural networks with fixed weights in the first layer. RFF model family $\mathcal{H}_N$ with $N$ parameters consists of fucntion $h: \mathbb{R}^2 -> \mathbb{C}$ of the form

$$
h(x) = \sum_{k=1}^{N} a_k \, \phi(x; v_k) \quad \text{where} \quad \phi(x; v) := e^{\sqrt{-1} \langle v, x \rangle},
$$

and $v_1, \dots , v_N$ are sampled independently from normal distribution. We consider $\mathcal{H}_N$ as a class of real-valued functions with $2N$ real-valued parameters (real and imaginary). 

As $N \rightarrow \infty$, model family becomes a close approximation to the Reproducing Kernel Hilbert Space (RKHS), corresponding to the Gaussian kernel, denoted by $\mathcal{H}_{\infty} $.

Given $n$ datas from $\mathbb{R}^d \times \mathbb{R}$, they find $h_{n, N} \in \mathcal{H}_N$ by ERM with squared loss. But minimizer is not unique for $N > n$. In that case, they choose minimizer with the minimum $L_2$ loss coefficients. 

When $n < N$, we can observe classical U-shaped curve that can be predicted by bias variance tradeoff. for $n>N$, where all function classes are rich enough to achieve zero training risk, increasing N progressively construct better approximation, showing the second descent segment of the curve. 

There are more experiments with other dataset and models structure in appendix.

### Neural newtorks and backpropagation
In general multilayer neural networks, we use stochastic gradient descent (SGD) to fit the trainin data. They observed that increasing the number of parameters in fully connected two layer neural networks lead to a risk curve qualitatively similar to that observed with RFF models. 

The compuational complexity of ERM with neural networks make double descent risk curve difficult to observe. In under parametrized regime, the result is highly sensitive to initialization due to nonconvexity of the ERM optimization. This variability in training and test risks masks the double descent curve. 

To have this effect for datasets as large as ImageNet, model size should have extensive amout of parameters. The amount of parameters needed is larget than many neural network models for ImageNet. In such cases, classical regime of the U-shaped risk curve is more appropriate to understand generalization. For smaller datasets, simply training to obtain zero training risk often results in good test performance.

## Decision trees and ensemble methods

They experimented with random forest. To further enlarge the function class, they also considered ensembles of several interpolating trees. As a result, double descent curve appear just as with neural networks.

## Concluding thoughts

For random Fourier or random ReLU features, solutions are constructed explicitly by minimum norm linear regression in the feature space. For neural networks, we used SGD. SGD initialized at zero converges to minimum norm solution. While SGD for more general case is not fully understood, there is some evidence that similar minimum norm inductive bias is present. Ensembling in decision tree leads to interpolating solution with higher degree of smoothness, and those averaged solution performed better than any individual tree. So all three methods lead to minimum norm solution. 

In this paper, modern models usually outperform the classical model on the test set. But beside that, there is a growing understanding that larger models are easy to optimize as local methods, such as SGD, converge to global minima of the training risk in over parametrized regimes. Thus, large interpolting models have low test risk and easy to optimize at the same time. It is likely that the models to the left of the interpolation peak have optimzation properties qualitatively different from those to the right.

The understanding of model performance developed in this work delineates the limits of classical analyses and opens new lines of enquiry to study and compare computational, statistical, and mathematical properties of the classical and modern regimes in machine learning.

