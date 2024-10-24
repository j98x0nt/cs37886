java c
MA 575 – LINEAR REGRESSION – FALL 2024
Chapter 3: Multiple Linear RegressionThe introduction of Chapter 2 is worth reading again, and is reproduced here below. After that we first discuss the matrix representation of the multiple linear regression model, and the least squares estimation procedure. In the process, we highlight some geometric interpretation of the least squares. We will then study the distri- butional properties of the model estimators, and the implication for statistical inference. Much of the chapter can be found in Chapter 7 of the first textbook listed in the syllabus (referred to as Textbook 1).
1.  IntroductionIn many projects it often happens that we measure and collect several variables on each statistical unit. We may then be interested in one of the variables, say y, called the response, or outcome, or dependent variable; specifically in the conditional distribution of y given some other variables, say x1 , . . . , xp, and called regres- sors, or predictors, or explanatory variables. Throughout this class, we will assume that the response variable y is a continuous variable, or can be reasonably viewed as a continuous variable. Assuming that we have n statistical units, our dataset is {(yi, xi1,...,xip),  1 ≤ i ≤ n}, where yi  (resp. xij) is the value of variable y (resp. xj) on the i-th unit. As statisticians we shall view the collected data as realizations of random vectors that we shall also write as {(yi, xi1,...,xip),  1 ≤ i ≤ n} (unlike most statistics textbook, we will not bother distinguish between random variables and their realizations). Because we are only interested in the conditional distribution of y given (x1 ,..., xp), it is somewhat equivalent to assume – as we will do – that the variables xij  are non-random, and to focus on y1 , . . . , yn  as random variables that we assume to be independent (but not identically distributed),and whereyi ∼ fy|(x1...,xp)(·|xi1 ,...,xip).                                                     (1)Our aim is to use the collected data to estimate the function fin (1). Statistical models to estimate f are called regression models. The linear regression model is the simplest of all regression models. It is a statistical model for yi that postulates that the expectation of yi is a linear function of xi1 , . . . ,xip:E(yi|xi1,...,xip) = β0 + β1xi1 + ... + βpxip ,                                         (2)
where β0,...,βp  are called regression coefficients. The model express the idea that if xij  changes by one unit, everything else being equal, then on average we expect yi  to change by βj  unit. Hence βj  captures the important of xj in explaining the variations of y. Equivalently, model (2) can be written as
yi = β0 + β1xi1 + ... + βpxip + ϵi ,    where   E(ϵi) = 0.                                 (3)
One particular distribution that we can give to ϵ so that E(ϵ) = 0 isN(0,σ2 ) for some variance σ 2 . If we make that choice, we get the following special case of model (3)
yi = β0 + β1xi1 + ... + βpxip + ϵi ,    where   ϵ ~ N(0,σ2 ).                            (4)
Model (4) is the main model that we will study in this class. The parameter set of model (4) is then (σ2 ,β0,...,βp). It is important to keep in mind that when we write a model such as (4), we are implicitly assuming that there
exists one specific parameter value (σ*(2),β0*,...,βp*) (that we will sometimes called the true value of the
parameter) such that
y = β0*+ β1*x1 + ... + βp*xp + ϵ,    where   ϵ ~ N(0,σ*(2)).
We say that the model is correctly specified. In some cases, one maybe interested for various reasons in fitting a misspecified model. However these are advanced topics that we will not consider in this class.Remark 1. One issue we need to clarify is the meaning of the model. In this class we will say that the variables xj for which the corresponding parameter β*j  are non-zero, we will say that these variables are significant in describing the distribution of y (or in describing the variability of y). We will try to avoid this terminology, but in some other contexts we may say that these variables have a significant effect on y. However we are not able to say whether these effects are causal. In other words, we will not be able to say that these explanatory variables have a cause-effect relationship with y. Additional tools that are beyond the scope of this course are needed to investigate causal effects.Example 1 (The diabete dataset and why we need multiple linear regression models).  Ten baseline variables, age, sex, body mass index, average blood pressure, and six blood serum measurements were obtained for a set of n = 442 diabetes patients. The response variable of interest, y, is a quantitative variable of disease progression that is measured one year after the baseline. The dataset is available on blackboard. We assume independence between the patients. The goal is to find the baseline variables that are significant in describing the distribution of the outcome y. To remove scaling effects in the measurements we will work with the z-scores: we center and re-scale all the variables.We will use this data set throughout the chapter to illustrate the concepts. But for now, we will use it to illustrate why we do need multiple linear models. Indeed, to answer the question asked above, we could choose to fit 10 simple linear regression models (one for each regressor), and select as important all the regressorswith a significant β1 , as we have done in the last chapter. The issue with this approach is spurious correlation. Let’s illustrate first. The code below loads and transforms the data as mentioned above, and fits a simple linear regression model of Y on S2  (the second blood serum measurement).
diabete  =  read .delim(’diabetes .data’,header  =  T) #center  and  rescale  data
data  =  lapply(diabete,  function(x)  scale(x,  center=T,  scale=T))
model_S2  =  lm(Y˜S2,  data=data) summary(model_S2)
Coefficients:



Estimate
Std .  Error  t  value
Pr(> | t | )
(Intercept)  -9 . 082e-17
4 . 689e-02
0 .000
1 .000000
S2                         1 .741e-01
4 . 695e-02
3 .7080 .000236  ***




The p value is 0.0002. Hence S2  is clearly useful in explaining the variation of Y. Now let’s fit a multiple linear regression that includes all 10 regressors.
mult_model  =  lm(Y  ˜ . ,  data=data); summary(mult_model)
#this  summary代 写MA 575 – LINEAR REGRESSION – FALL 2024 Chapter 3: Multiple Linear RegressionR
代做程序编程语言  shows  that  S2  is  not  important
Coefficients:
Estimate  Std .  Error  t  value  Pr(> | t | ) (Intercept)  -3 .470e-16    3 .341e-02       0 .000  1 .000000
AGE                  -6 . 183e-03    3 . 691e-02    -0 .168  0 .867031
SEX                 -1 .481e-01    3 .782e-02    -3 . 917  0 .000104  ***
BMI                   3 .211e-01    4 . 110e-02       7 .813  4 .30e-14  *** BP                        2 . 004e-01    4 . 041e-02      4 . 958  1 . 02e-06  ***
S1                      -4 . 893e-01    2 .574e-01    -1 . 901  0 .057948   .S2                      2 . 945e-01    2 . 094e-01       1 .406  0 .160390 S3                      6 .241e-02    1 .313e-01       0 .475  0 . 634723 S4                      1 . 094e-01     9 . 974e-02      1 .097  0 .273459
S5
4 . 640e-01
1 . 062e-01
4 .3701 .56e-05  ***
S6
4 . 177e-02
4 . 076e-02
1 .025
0 .305990We will learn later in the chapter how the statistical test in the multiple model is performed, but from the pvalue, we conclude that we cannot reject the hypothesis that the coefficient of S2 is 0. Hence, according to the multiple linear model, the variable S2  is not important in describing the distribution of Y, which contradicts the first model. To understand the issue, let’s look at the correlation plot between all the variables. The issue is that S1  and S2  are highly correlated. The multiple regression model tells us that S2  does not have much effect on Y, but S1  does. However, since S1  and S2  are highly correlated, in the absence of S1 , S2  shows an effect on Y. We call that a spurious correlation. Fitting multiple linear regression models help again spurious correlations.

FIG  1. Correlation plot between variables in the diabetes dataset.
2.  Matrix representation and parameter estimation of multiple linear models
We can write model (4) for all n subjects as

If we set
nd  X =  
\ yn  ,         \ ϵn  ,              \I βp  ,I                        \  1   xn1     . . .   xnp  ,
then, clearly we can write thesen equations together asy = Xβ + ϵ .                                                                   (5)
Equation (5) is the matrix formulation of the multiple linear model. The iid N(0,σ2 ) assumption that we impose on the error terms ϵ 1 , . . . ,ϵn becomes
ϵ ∼ N(0,σ2 In),where In  ∈ Rn×n is then-dimensional identity matrix. Throughout this chapter, and in fact this entire class, we will assume that there is no redundant explanatory variable among the p + 1 explanatory variables that we have:Rank(X) = p + 1,                                                              (6) where Rank(X) denotes the rank of the matrix X. The assumption implies that n ≥ p + 1 (why?). From the distributional assumption on the error terms, it follows that the log-likelihood function of the model is given
by
2.1. Parameter estimationThe maximum likelihood estimation of (σ2 , β) is the value of the parameter set that maximizes ℓ as given in
(7). Equivalently, this corresponds to the value of (σ2 , β) that minimizes

We can solve this minimization problem as we did in Chapter 2. First we find β by minimizing the functionβ }→  ∥y − Xβ∥2 . The vector β(ˆ) ∈ Rp+1  that minimizes the function β }→  ∥y − Xβ∥2  is called the leastsquares estimate of β . And the problem of minimizing the function β  }→  ∥y − Xβ∥2  is called the leastsquares problem. Once we solve the least squares problem and found β(ˆ), we then minimize the expression2 )+  ∥y −Xβ(ˆ)∥2 over σ 2 to get the maximum likelihood estimate of σ 2 . The derivation is the sameas in Chapter 2, and gives ˆ(σ)m(2)le  = ∥y − X β(ˆ)∥2 /n. As in Chapter 2, we will not use this estimate. Instead we
will estimate σ2 by
It remains to solve the least squares problem. The least squares problem is actually a projection problem. It is asking to find the element of the space S = {Xb,  b ∈ Rp+1 } that is the closest to y. But we note that
where Xk  denotes the k-th column of the matrix X (check this identity). This implies that the space S is no other than the column space of X. Let P ∈ Rn×n be the projector on S. We can write y = Py + r, where r is orthogonal to S. Therefore we can write
∥y − Xβ∥2 = ∥Py + r − Xβ∥2  = ∥Py − Xβ∥2 + ∥r∥2 .
Since Py ∈ S, we conclude that β(ˆ) exists, and satisfies
Py = X β(ˆ) .
By definition of the projection the vector y − Py = y − X β(ˆ) is orthogonal to all the columns of X. This can be written as X′(y − X β(ˆ)) = 0 and yields the equation(X′X)β(ˆ) = X′y.                                                                (9)
Under the assumption made in (6) that Rank(X)  = p + 1, the X′X is symmetric positive definite, and therefore invertible (can you show this?). We conclude that β(ˆ) is uniquely defined and given byβ(ˆ) = (X′X)−1X′y.                                                            (10)
In conclusion we have
Proposition 2.1.  In model (5), and under assumption (6) the maximum likelihood estimate of β is the same as the least squares estimate of β and is uniquely given by
β(ˆ) = (X′X)−1X′y.
Furthermore y − X β(ˆ) is orthogonal to all the columns of X: X′(y − X β(ˆ)) = 0, and the estimate of σ 2  is given by

Example 2.  Suppose that we have only one regressor that we write only as x (instead of x1). Then as we have seen in Chapter 2, the model writes

' '

If we set
and   ,
then,thesen equations together is

The least squares estimate of (β0 ,β1 ) is then

Let us show that we recover the same estimates as in Chapter 2.


To recover Chapter 2’s expression for  ˆ0 , we write Σixi(2) = Σi(xi − ¯(x))2 + (Σi xi)2 =n. Hence


We then get
=  =¯(y) − ¯(x) ˆ1 ;
which is the same as in Chapter 2.Back to the general model, the vector y(ˆ) = X β(ˆ) is called the vector of fitted values. ϵ(ˆ) = y −y(ˆ) is called the vector of residuals. And  that
y(ˆ) = X β(ˆ) = X(X′X)−1X′y = Hy;   where   H = X(X′X)−1X′:
The matrix H is sometimes called the H matrix. The diagonal elements of H (that is hi  = Hii) are called leverages. Similarly,
ϵ(ˆ) = y − y(ˆ) = y − Hy = (In − H)y = My;   where   M = In − H: It is easy to check that the matrices H and M satisfies
1.  H′ = H, H2  = H (H is an orthogonal projector).
2.  M′ = M, M2  = M (M is an orthogonal projector).
3.  M + H = In and MH = 0, and MX = 0 (zero matrices).



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
