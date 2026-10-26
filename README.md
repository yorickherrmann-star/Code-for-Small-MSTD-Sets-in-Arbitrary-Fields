# Code for "A Classification of Small MSTD Sets in Arbitrary Fields"

This repository contains the code accompanying the paper *A Classification of Small MSTD Sets in Arbitrary Fields* by Yorick Herrmann (arXiv:2608.22189). 

A finite set *A* in an additive abelian group is called an MSTD set if *|A + A| > |A − A|*. The paper proves that no MSTD set of size five exists in any additive abelian group. The paper also uses computer programs to classify all MSTD sets of sizes six through nine in arbitrary fields, and determine the smallest MSTD multiplicative subgroups of *Z/pZ*.

## Notebooks

### `MSTDConfigurationAnalyzer.ipynb`
This notebook contains the main program used to prove Theorems 1.2–1.5. It starts from a configuration of linear expressions in free variables and branches over every possible collision between differences, eliminating one variable at each step. The program reports every MSTD set it finds, together with a certificate *φ* guaranteeing that the search is complete in characteristic zero and every characteristic larger than *φ*. The remaining small characteristics are handled with `MSTDSearchinZpZ.ipynb`. Additionally, the notebook includes code that groups the resulting configurations up to affine equivalence.

By default, the program works in characteristic zero. Setting `multivar_prime_filter=p` instead makes it work in a field of characteristic *p*: every coefficient during the collision branching is reduced modulo *p*, and a set is tested for being MSTD only in that characteristic.

For example, the following runs the search for sets of size six, starting from the configuration *{0, a, b, c, d, e}* (note that we can always assume 0 is in *A*):

```python
A = Analyzer(['0', 'a', 'b', 'c', 'd', 'e'])
results = A.run()
```

The following runs a search for sets of size eight in characteristic 3:

```python
A = Analyzer(['0', 'a', 'b', 'c', 'd', 'e', 'd-e', 'f'], multivar_prime_filter=3)
results = A.run()
```
For this configuration, we started from the base configuration of 8 elements and 7 variables,  *{0, a, b, c, d, e,f,g}*, and we considered the case where *g=d-e*, which allowed us to input a configuration of 6 variables instead of 7, thereby reducing the program runtime.

### `MSTDSearchinZpZ.ipynb`
This notebook lists every MSTD set *A* of a given size in *Z/pZ*, up to the affine maps *x ↦ ax + b* with *a ≠ 0*. It is used to check the small primes not covered by the certificate *φ* output by the previous notebook. The function returns one representative of each affine class. Setting `verbose=True` also prints the number of classes found and lists each representative.

For example, the following finds all MSTD sets of size eight in *Z/37Z*:

```python
mstd_sets_mod_p(37, 8, verbose=True)
```

### `MultiplicativeSubgroupCode.ipynb`
This notebook searches for multiplicative subgroups *A* of *Z/pZ* that are MSTD, and is used to prove Theorem 1.6 and Corollary 4.3. For each order *n*, this notebook uses resultants of the *n*-th cyclotomic polynomial to find the only primes *p* for which the subgroup of order *n* could be MSTD. It then tests each of these primes directly.

The function `run(n)` carries out this process for a single order *n* and prints its progress. For example, the following finds every prime *p* for which the subgroup of order 9 could be MSTD, and tests each one:

```python
r = run(9)
```

The function `mstd_subgroups(p)` works in the other direction: for a fixed prime *p*, it checks every order *n* dividing *p-1* and returns the orders whose subgroup is MSTD. For example, the following shows that the subgroup of order 141 is MSTD in *Z/4231Z*:

```python
mstd_subgroups(4231)   # returns [141]
```

Finally, `count_affine_copies(A, p)` counts the distinct affine copies of a set *A* in *Z/pZ* and checks whether there are *p(p − 1)* of them, which is used together with Lemma 4.2 to prove Corollary 4.3.

## Requirements

The code requires Python 3 (tested on version 3.12) and Jupyter. The package `sympy` is needed only for `MultiplicativeSubgroupCode.ipynb`.

## License

This code is released under the MIT License (see `LICENSE`).
