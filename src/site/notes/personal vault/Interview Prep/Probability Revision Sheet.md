---
{"dg-publish":true,"permalink":"/personal-vault/interview-prep/probability-revision-sheet/"}
---




Probability Core Ideas.

1.  Permutations :  $$P( n , k) = \frac{n!}{ (n-k)!}$$
2. Combinations :  $$C(n,k) = \frac{n!}{ (n-r)! \space r!}$$
3. Pigeonhole Principle : If we have more pigeons than pigeonholes then at least one hole contains more than 1 pigeon.
4. Axioms of Probability : 
	1. 0 <= P(E) <=1
	2. P(S) = 1  ( then S is sample space)
	3. For mutually exclusive events , E1 , E2 .... : $P \cup E_{i} = \Sigma P (E_i)$ 
	4. $P \cup B = P(A) + P(B) - P(A \cap B  )$  ( inclusion-exclusion principle)
	5. $P(A^{c}) = 1 - P(A)$
5. Conditional Probability : $$P(A|B) = \frac{P(A \cap B)}{ P(B)}$$ assuming P(B) > 0
6. Bayes theorem :  $$P(A|B) = \frac{P(B|A) P(A)}{ P(B)}$$ Important for updating beliefs.
7. Independence : $$P(A \cap B) = P(A) P(B)$$ this implies $$ P(A|B) = P(A) \space and \space P(B|A) = P(B)$$
8. Random Variables : Discrete 
	1.  PMF $$P(a) = P(X=a)$$
	2. CDF : $$F(a) = P( X <= a)$$
	3. Expected Value : $$E[X] = \Sigma \space x P(x)$$
	4. $$Var(X) = E[(X - E[X]^{2})] = E[X^{2}]- (E[X])^{2} $$
	5. Standard Deviation $$ \sqrt(Var)
$$

9. Continuous Random Variable : 
		1. $$ PDF = P( a <= X <= b ) = \int_{a}^{b}f(x)dx$$
		2. $$CDF = F(a) = P(X <= a) = \int_{-\infty}^{a}f(x)dx$$
		3.  $$ \text{Var}(X) = \int_{-\infty}^{\infty} (x - E[X])^2 f(x)dx $$
	7. Interview focus : Calc exp , variances.
10. Distributions -refer pdf 
11. Joint Distributions , Covariance and Correlation 
	-  Joint PMF/PDF: $P(X=x,Y=y)$ or $f(x,y)$. 
	- Marginal PMF/PDF: Obtained by summing/integrating over the other variable(s). 
	- Conditional PMF/PDF: $p(y|x) = \frac{p(x,y)}{p_X(x)}$ or $f(y|x) = \frac{f(x,y)}{f_X(x)}$. 
	- Independence of Random Variables: $f(x,y) = f_X(x)f_Y(y)$ or $P(X=x,Y=y) = P(X=x)P(Y=y)$. - Expectation of functions of random variables: $E[g(X,Y)]$. 
	- Covariance: $Cov(X,Y) = E[(X - E[X])(Y - E[Y])] = E[XY] - E[X]E[Y]$. Measures linear relationship. - Correlation: $\rho(X,Y) = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}$. Normalized covariance, $-1 \le \rho \le 1$. 
	- Properties of Expectation and Variance: 
		- $E[aX+bY] = aE[X]+bE[Y]$ (Linearity of Expectation - always holds). 
		- $Var(X+Y) = Var(X)+Var(Y)+2Cov(X,Y)$. 
		- If $X$ and $Y$ are independent, $Cov(X,Y)=0$ and $Var(X+Y)=Var(X)+Var(Y)$.



- Permutations - nPr
- Combinations - nCr
- Multinomial Coefficients - dividing  n into r groups  $$ \frac{n!}{ r1! r2! r3! ....}$$ where r1 + r2 + r3...rn = n 
-  No of integer solutions of equations 
- ![Pasted image 20250602175117.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250602175117.png)
- ![[Pasted image 20250602175253.png \| 500]]
- Matching Problem : Derangement 
![derangements.jpg](/img/user/personal%20vault/Attachments/derangements.jpg)

![capture_temp (0).jpg](/img/user/personal%20vault/Attachments/capture_temp%20(0).jpg)


- Circular Problems
![circular_perm.jpg\| 500](/img/user/personal%20vault/Attachments/circular_perm.jpg)


![[circular_perm_1.jpg \| 500]]




