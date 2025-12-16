We come across degrees of freedom in statistics everywhere. However, no particular classes dives deep in this topic. First place we encounter degrees of freedom is when discussing sample variance.  $$\hat{\sigma^2}= \frac{(X-\bar{x})^2}{n-1}$$
Obvious question arises, why are we dividing by $n-1$ instead of $n$ . Usual answers are 
1. Well, dividing by $n$ makes the statistics biased and in order to make in unbiased, we should divide by $n-1$ . 
2. Because the degrees of freedom of this statistics is $n-1$.

But what is degrees of freedom? There are several answers. Let's start with the most common answer. 
1. Number of independent pieces of information that goes into the statistics . 
2. The rank of the Quadratic form of the Statistics. 
3. Difference between the dimension of null hypothesis and alternative hypothesis. 
Definition 2 and 3 boils down to definition 1. However definition 2 and 3 helps to understand the degrees of freedom in context where "independent pieces of information" is not very obvious. 
```python
import numpy as np
x=np.array([1,2,3])
```








