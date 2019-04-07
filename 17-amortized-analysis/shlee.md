### Amortized Analysis 

- average the time required to perform a sequence of data-structure operations performed.
- used for algorithms where an occasional operation is very slow, but most of the other operations are faster.
- 


#### Aggregate analysis
- for all n, a sequence of n operations takes worst-case time T(n) in total. In the worst case, amortized cost per operation is therefor T(n)/n.
- Note that this amortized cost applied to each operation, even when there are several types of operations in the sequence.

#### The accounting method
- assign differing charges to diffent operations. 

#### The potential method
- need to choose potential function `phi`
- c_i = c_i + phi(D_i) - phi(D_(i-1))
#### Dynamic tables
