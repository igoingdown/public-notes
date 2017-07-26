###### Input：

​    Nth-layer-Headers *Header*;

###### Ouput:

​    *Map*, a map from node to each node's noisy support.

1. *M* $\leftarrow$  $\left \{ \right\}$;
2. *Map* $\leftarrow$  $\left \{ \right\}$;
3. *visitedSet* $\leftarrow$ $\emptyset$;
4. *HeadQueue* $\leftarrow$  *Header*;
5. $\mathbf{while}$ *HeadQueue* is not empty $\mathbf{do}$ 
6. ​    *aNode* $\leftarrow$ top element of *HeadQueue*;
7. ​    pop the top element of *HeadQueue*;
8. ​    $\mathbf{if}$ *aNode* is not visited $\mathbf{then}$ 
9. ​        /\*\*\*\*\*\*$\mathbf{Case}$ $\mathbf 1$:  *aNode* only appears in one lattice\*\*\*\*\*\*/  
10. ​        add *aNode* to *visitedSet*;
11. ​        add noise to the support of *aNode*;
12. ​        add $\left ( {aNode \rightarrow noisySupport} \right )​$ to *M*;
13. ​    $\mathbf{else}​$ $\mathbf{then}​$ 
14. ​        /\*\*\*\*\*\*$\mathbf{Case}$ $\mathbf 2$:  *aNode* appears in multi lattices\*\*\*\*\*\*/
15. ​        update the noisy support of *aNode*;
16. ​        update *M*[*aNode*];
17. ​    $\mathbf{end}$ $\mathbf{if}$
18. ​    $\mathbf{for}$ each node *n* in children of *aNode* $\mathbf{do}$
19. ​        add *n* to *HeadQueue*;
20. ​    $\mathbf{end}$ $\mathbf{for}$
21. $\mathbf{end}$ $\mathbf{while}$
22. $\mathbf{for}$ each node *n* in *M* $\mathbf{do}$
23. ​    $\mathbf{for}$ each node *a* in ancestors of node *n*  $\mathbf{do}$
24. ​        add the noisy support of node *a* to the noisy support of node *n*;
25. ​        add $\left( {n \rightarrow sumNoisySupport} \right)$ to *Map*;
26. ​    $\mathbf{end}$ $\mathbf{for}$
27. $\mathbf{end}$ $\mathbf{for}$
28. return *Map*;


