# Ext-Outer-Functors

This repository contains the code related to the article "[(Non-)vanishing results for extensions between simple functors on free groups](https://arxiv.org/abs/2311.16881)" by Louis Hainaut

## Abstract
In this article we study cohomological properties of the category of *polynomial outer functors* on free groups, which are the functors from the category of finitely generated free groups into the category of rational vector spaces which send all inner automorphisms to the identity morphism, and which satisfy a certain polynomiality property. More precisely, we prove vanishing and non-vanishing results for the Ext groups between simple polynomial outer functors. This work is inspired by an earlier result of Vespa for the category of all polynomial functors from finitely generated free groups to rational vector spaces; it follows in particular from her results that, in this larger category, the Ext groups between simple functors are always concentrated in a specific single degree. Our main results show that, when we pass to the full subcategory of polynomial outer functors, Ext groups between simple functors are sometimes non-trivial outside of this specific degree.

## Code
All the algorithms can be found in the file [Ext-Groups_Outer-Functors.ipynb](https://github.com/louishainaut/Ext-Outer-Functors/blob/main/Ext-Groups_Outer-Functors.ipynb). Alternatively the result of some computations is displayed in the file [Computations_Ext_Outer-Functors.pdf](https://github.com/louishainaut/Ext-Outer-Functors/blob/main/Computations_Ext_Outer-Functors.pdf); however the readability of this PDF is not so great, so you might prefer to do the computation yourself and access only the part of the result that you are interested at.

The algorithms are meant to be used in the following way: The first cell contains all the definitions, you will need to run this cell the first time, but after that you don't need to return to this cell, and it should not be necessary to understand the contents of this cell. The most useful functions provided are the following ones:

`Compute_ExtGroups(bound, Outer = True, CompFact = None, check = True)`:
This function implements the algorithm discussed in the proof of Theorem 4.11, to compute Euler characteristics of the Ext groups $Ext(a^n, a^m)$ from the composition factors of $\omega\beta\mathbb{Q}\mathfrak{S}_n$. It tends to take a rather long time to execute this function, it is recommended to only call it once. This function computes all the Euler characteristics at the same time, it is recommended to use the next function to see the results. This function takes the following parameters:
- `bound`: Positive integer, indicates the range of Ext groups which get computed. If the other parameters are set to their default, then `bound` cannot exceed the value $10$. Even if the boolean parameters are set to other values, be aware that for values of `bound` larger than `10` the computations start to take a rather long time.
- `Outer`: boolean, set to `True` by default, in which case the function outputs the Euler characteristic for Ext groups of outer functors. If set to `False`, then the function computes Ext groups in the category of all functors on free groups, see [Extensions between functors from free groups](https://londmathsoc.onlinelibrary.wiley.com/doi/full/10.1112/blms.12091) for more details about these.
- `CompFact`: Optional parameter, can be used to provide the composition factors needed for the algorithm. By default, they will be computed within the function, but if you want to keep access to these composition factors you can compute them separately and give them to the function. Unless you know what you are doing, you should call the function `All_Composition_Factors` to compute them (and you should be sure that the parameter `Outer` is the same for both functions).
- `check`: boolean, set to `True` by default, in which case it performs a few basic checks on the parameter `CompFact` and prevents the parameter `bound` to exceed $10$ if `Outer` is also set to `True` (since in that case some of the composition factors would be incorrect). Unless you know what you are doing, you should probably not modify this parameter.

`Access_Ext_Group(Ext, source, target)`:
This function is used to display individual Euler characteristics. You need to first compute the first parameter `Ext` using the previous function. Depending on the values of `source` and `target` it will show $\chi(Ext(a^n, a^m))$ or $\chi(Ext(\alpha S_{\nu}, \alpha S_{\lambda}))$. It takes the following parameters:
- `Ext`: Should be computed with the previous function, contains the information about all the Euler characteristics.
- `source`: Can either be a non-negative integer or a partition. In the former case it will display $\chi(Ext(a^n, -))$, and in the latter case it will display $\chi(Ext(\alpha S_{\nu}, -))$.
- `target`: Can either be a non-negative integer or a partition. In the former case it will display $\chi(Ext(-, a^m))$, and in the latter case it will display $\chi(Ext(-, \alpha S_{\lambda}))$.

`Find_NonKoszul(Ext)`:
This function can be used to find the values of the partitions $(\nu, \lambda)$ for which the Euler characteristic of $Ext^*(\alpha S_{\nu}, \alpha S_{\lambda})$ guarantees that the Koszul property is not satisfied. The parameter `Ext` should be computed using `Compute_ExtGroups`.

`Compare_ExtGroups(ExtOut, ExtAll, diff, n)`:
This function implements the formula for $\chi(Ext(a^n, a^{n+diff}))$ of Remark 4.23, and provides as output a pair containing the result of this formula and the missing terms. It takes the following parameters:
- `ExtOut`: All the Euler characteristics of Ext groups in the category of outer functors. Should be computed from `Compute_ExtGroups` with the parameter `bound` having value at least `n+diff`.
- `ExtAll`: All the Euler characteristics of Ext groups in the category of all functors. Should be computed from `Compute_ExtGroups` with the parameter `bound` having value at least `n+diff`, and the parameter `Outer` set to `False`.
- `diff`, `n`: integer parameters, determine the degrees.

`Text_Form(SymFunc)`:
This function can be used to visualize some of the results; it creates a string which is more compact that the default display of Sage. 

`All_Composition_Factors(bound, Outer = True, Partial = False)`:
This function is used to compute the graded pieces $(\omega\beta\mathbb{Q}\mathfrak{S}_n)^{[p]}$, which are needed for the function `Compute_ExtGroups`. The majority of the code for this function comes from [Configuration spaces on a wedge of spheres and Hochschild–Pirashvili homology](https://ahl.centre-mersenne.org/item/AHL_2024__7__841_0/). It takes the following parameters:
- `bound`: non-negative integer, determines which is the largest value of $n$ which gets computed. Note that if the parameter `Outer` is set to `True` then `bound` should not exceed the value $10$, as then the some of the results computed will be incorrect.
- `Outer`: boolen, set to `True` by default, in which case the function computes the graded pieces $(\omega\beta\mathbb{Q}\mathfrak{S}_n)^{[p]}$. If set to `False`, then the function computes $(\beta\mathbb{Q}\mathfrak{S}_n)^{[p]}$ instead.
- `Partial`: boolean, set to `False` by default. Unless you know what you are doing, it is not recommended to set this parameter to `True`.
