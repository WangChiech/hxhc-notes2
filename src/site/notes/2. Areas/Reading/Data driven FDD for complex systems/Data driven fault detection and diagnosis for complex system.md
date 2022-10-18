---
{"date":"2022-09-30","time":"10:48","tags":[],"cssclass":null,"dg-publish":true,"dg-class":"algorithm","dg-show-local-graph":true,"permalink":"/2-areas/reading/data-driven-fdd-for-complex-systems/data-driven-fault-detection-and-diagnosis-for-complex-system/","dgHomeLink":true,"dgPassFrontmatter":true,"dgShowLocalGraph":true,"dgShowBacklinks":true,"dgShowInlineTitle":true}
---


Fault detection and diagnosis (FDD) is an important task in industry manufacturing. Typically, there are 3 ways to realize FDD: knowledge-based, data driven and hybrid. Nowadays, with the access to large volume of data, data driven FDD becomes more and more promising. Below are some aspects and discussions of data driven FDD.

## Problems

Multimode, multiscale, multistage
Nonlinear, correlation, nonstationary, non-Gaussianity

## Variable grouping

It is very common that a process fault is caused by or has a significant effect on one or only a few variables. For such faults, a fault detection index that contains all of the variables is not optimal.

> There are two reasons[^1]:
>
> 1. Amplification effect: Using more variables usually leads to a larger control limit, thus reducing the sensitivity of the fault detection index. Small faults may be hard to be detected.
> 2. Masking effect: If only a few variables contribute to the fault, the effect of faulty variables on the fault detection index may be weakened by the fault free variables, and thus, the fault detection index may not exceed its control region.

The amplification effect can be reduced by  decreasing the number of variables involved in the fault detection index. The masking effect can be reduced by excluding fault-free variables from the fault detection index. But it is difficult to determine how many or which variables should be included before FDD.

Some data driven FDD methods have the ability to reduce the amplification and masking effect, such as the FDD methods based on dimension reduction, sparse modeling, and multiblock modeling. Among these methods, multiblock modeling is a promising and especially useful method for large scale systems.  The key to multiblock modeling is how to divide a system into blocks. Typically, a system is divided according to expert knowledge. For example, each unit or subsection can be considered as a block. But such a division may not accurately characterize the variable correlation. Luo et al. proposed a novel variable grouping method based on variable correlation[^1].

### Intragroup FDD

There are 3 ways to use the intragroup variable correlations for FDD.

1. Group based $T^2$
2. Dominative latent variable
3. Intragroup regression

### Intergroup FDD

If FDD is only performed by intragroup methods, variable correlations among different groups will be ignored. To retain such information between groups, an intergroup $T^2$ statistic is defined for the use of variable correlations among K groups.

$T_{bg,j}^2=T^2_{j,o}-\sum^K_{k=1}T^2_{j,k}$

where  $T^2_{j,o}=x_j^T\Sigma^{-1}x_j$, $T^2_{j,k}=x^T_{j,k}\Sigma_k^{-1}x_{j,k}$

[^1]: Luo, L., Wang, J., Tong, C., & Zhu, J. (2020). Multivariate fault detection and diagnosis based on variable grouping. *Industrial & Engineering Chemistry Research*, 59(16), 7693-7705.
