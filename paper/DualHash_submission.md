DualHash: A Stochastic Primal-Dual Algorithm with Theoretical Guarantee for
                              Deep Hashing
                             Luxuan Li∗ , Xiao Wang† , and Chunfeng Cui‡


Abstract. Deep hashing encodes high-dimensional image features into compact binary codes, enabling efficient
          similarity search in large-scale content-based image retrieval, a problem akin to semantic-preserving
          lossy compression under extreme bit-rate constraints. A fundamental challenge in deep hashing stems
          from the discrete nature of quantization in generating the codes. W-type regularizations, such as
          ||z| − 1|, have been proven effective as they encourage variables toward binary values. However,
          existing methods often directly optimize these regularizations without convergence guarantees. While
          proximal gradient methods offer a promising solution, the coupling between W-type regularizers and
          neural network outputs results in composite forms that generally lack closed-form proximal solutions.
          In this paper, we present a stochastic primal-dual hashing algorithm, referred to as DualHash, that
          provides rigorous complexity bounds. Using Fenchel duality, we partially transform the nonconvex
          W-type regularization optimization into the dual space, which results in a proximal operator that
          admits closed-form solutions. We derive two algorithm instances: a momentum-accelerated version
          with O(ε−4 ) complexity and an improved O(ε−3 ) version using variance reduction. Experiments
          on three standard datasets demonstrate superior retrieval accuracy and reduced quantization error,
          validating the practical value of DualHash for image retrieval problems. Our implementation of
          DualHash is available at https://github.com/RailLee/DualHash.

Key words. nonconvex nonsmooth optimization, primal-dual method, deep hashing, image retrieval, stochastic
           optimization, oracle complexity

MSC codes. 90C26, 90C46, 65K05, 68T07, 68P20

    1. Introduction. Hashing is a pivotal technique in large-scale image retrieval, which encodes
image content into compact binary codes (e.g., ±1) for efficient storage and accurate similarity
search. Mathematically, this amounts to a constrained projection onto the binary hypercube
{−1, +1}d that preserves semantic neighborhood structures, a nonconvex discrete optimization
problem akin to lossy image compression, where the fidelity criterion is pairwise similarity rather
than pixel-level reconstruction. Deep hashing leverages neural networks to learn such projections,
achieving compression ratios up to 512× while maintaining competitive retrieval accuracy [41].
The mainstream approaches fall into two categories: sign-based methods and continuous
relaxation. A fundamental challenge, however, is that the sgn function has zero gradient almost
everywhere, rendering standard first-order methods ineffective [8]. Continuous relaxation
mitigates this but inevitably introduces quantization error —the discrepancy between optimized
continuous values and the final binary codes [60]. To mitigate this issue, researchers have
developed specialized regularization techniques [7, 36, 39]. Among them, W-type regularizations,
such as ||z| − 1|, have proven effective. These regularizers impose sharp penalties to encourage
variables towards binary values (e.g., −1 or 1). Mathematically, this approach naturally
   ∗
     School of Mathematical Sciences, Beihang University, Beijing, 100191, China (luxuanli@buaa.edu.cn).
   †
     School of Computer Science and Engineering, Sun Yat-Sen University, Guangzhou, 510006, China
(wangx936@mail.sysu.edu.cn).
   ‡
     School of Mathematical Sciences, Beihang University, Beijing, 100191, China (chunfengcui@buaa.edu.cn).

                                                      1
2                                                              LUXUAN LI, XIAO WANG, AND CHUNFENG CUI




                                                  x                        −1         +1        x

                   (a) h(z) = ∥z∥1                                      (b) h(z) = ∥|z| − 1∥1

                      Figure 1.1: The V-type (left) versus W-type (right) regularizations.

formulates deep hashing as a nonconvex composite optimization problem1 :
                                                   n             n
                                                1X          X
(1.1)                                  min         fi (x) +   h(Di (x)),
                                      x∈Rdx     n
                                                  i=1           i=1

where fi and Di are continuously differentiable and h is a proper and lower semi-continuous
(l.s.c.) regularizer. Classic examples include the ℓ1 norm to introduce sparsity and the ℓ2
norm weight decay to prevent overfitting. In practice, researchers typically apply stochastic
(sub)gradient methods (SGD) directly to optimize the objective (1.1), and deep hashing follows
this paradigm as well. This seemingly effective choice, however, frequently leads to suboptimal
performance. The regularizer is often nonsmooth around some regions; using subgradients may
result in slow convergence and oscillation.
     The proximal operator is a powerful tool for handling nonsmooth regularizers, with wide-
spread applications in signal/image processing and computer vision, including sparse recov-
ery [64], inverse imaging problems [19, 50, 35], low-rank and robust matrix factorization [69, 12],
and structured neural network training [4, 31]. For the special case Di (x) = x, stochastic
proximal gradient methods (SPGD) and their variance-reduced or momentum-based variants
have been extensively studied [38, 62]. However, these methods require regularizers with
tractable proximal operators (e.g., ℓ1 soft-thresholding). Although the W-type regularizer
admits a closed-form proximal operator [2] (Figure 1.1), its composition with neural network
outputs h(Di (x)) renders the proximal subproblem intractable, making SPGD inapplicable
to Problem (1.1). Moreover, unlike standard deep learning tasks, where regularization is
typically imposed on model parameters, deep hashing requires regularization directly on the
network output to enforce binary codes, as illustrated in Figure 1.2. This makes the nonsmooth
composite form h(Di (x)) intrinsic to deep hashing and therefore necessary to handle explicitly.
     For nonsmooth compositional optimization, including Problem (1.1), significant research
has been conducted when h ◦ D is either convex [68, 54] or weakly convex [17, 21, 56, 29].
For instance, h ◦ D is weakly convex when h is convex and D is smooth [17, 21, 56], or when
h is component-wise nondecreasing and D is weakly convex [29]. However, when h itself is
nonconvex, as in our setting, h ◦ D is not necessarily weakly convex, rendering these methods
inapplicable. For the case where h is nonconvex, [5] reformulates the problem as a constrained
    1
        A formal definition of this problem is provided in Section 3.
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                         3




            Figure 1.2: Comparison between deep learning for hashing and classification.

optimization by imposing redundant variables and establishes asymptotic convergence via a
Lagrangian-based approach. When h is Lipschitz continuous but possibly nonconvex, Yao et
al.[63] establish an O(1/(δϵ2 )) iteration complexity for finding a (δ, ϵ)-stationary point. More
recently, Zhang et al.[66] provide a systematic analysis for the cases where h is nonconvex with
or without Lipschitz continuity. Among their results most relevant to ours, they establish an
O(ϵ−2   −1
     1 ϵ2 ) iteration complexity for finding an (ϵ1 , ϵ2 , 0)-stationary point for Lipschitz continuous
h, along with subsequential convergence results. However, all consider only the deterministic
setting, whereas ours addresses the stochastic finite-sum formulation that naturally arises in
large-scale deep hashing; directly extending their results to our setting is nontrivial.
    1.1. Contributions. In this paper, we propose an efficient deep hashing algorithm to address
these challenges posed by nonlinear composition with W-type regularization. Our approach
reformulates the W-type regularized problem using a decoupling scheme and develops a new
primal-dual algorithm based on Fenchel duality. We aim to bridge the theoretical-practical gap
by providing rigorous non-asymptotic convergence analysis and enhanced practical performance
in deep hashing. Our main contributions are as follows:
  • Novel primal-dual hashing algorithms. We reformulate deep hashing models with
     W-type regularization as a finite-sum muti-block optimization problem (P), decoupling
     the challenging composite term h(D(x)) and facilitating sequential convergence analysis.
     Grounded in this formulation, we propose DualHash, a stochastic primal-dual algorithm
     for deep hashing. Using Fenchel duality, we partially transform the nonconvex W-type
     regularization optimization into a convex dual formulation that admits closed-form updates,
     ensuring robustness and stable convergence. We then develop two important instances: a
     momentum-based implementation (DualHash-StoM) and a variance-reduced implemen-
     tation using StoRM (DualHash-StoRM).
4                                                                  LUXUAN LI, XIAO WANG, AND CHUNFENG CUI



       Table 1.1: Comparison of optimization and convergence guarantee of deep hashing methods.

      Type          Method                    Model                Quantization Loss            Optimization   Complexity
                   DDSH [30]           min Ls (b) + Q(u, b)                 ∥ · ∥2                  BQP
     Discrete      DSDH [36]                                                                     sgn update
                                      min Ls (u) + Q(u, b)                  ∥ · ∥2
                   DPSH [37]                                                                        DCC
                                                                                           
                    DCH [7]                                         log 1 + disth (|u|,1)
                                                                                  γ
                   DNNH [33]            min Ls (u) + h(u)               I[−1,1]d (u)               SGDM            —
                    DSH [39]                                            ∥|u| − 1∥1
                                                                                   2
    Continuous
                   DTSH [57]                                       P ∥|u| − 1∥2
                   DHN [71]                                         i log(cosh(|ui | − 1))
                  MDSHC [55]          min Lc (u) + Ls (u) + h(u)         ∥|u| − 1∥1               RMSProp
                  HashNet [8]                                                                      SGDM
                                            min Ls (u)                         -
                 OrthoHash [28]                                                                    Adam
                  DualHash-StoM                                                                           O(ε−4 )
                                     min Ls (u) + Q(u, b) + h(b)    ∥|b| − 1∥1      Sto Primal-Dual
                DualHash-StoRM                                                                            O(ε−3 )
Notes: The Model column shows the core optimization objective, where b denotes binary codes and u denotes continuous
outputs. The Quantization Loss column specifies the loss (Q(·, ·) or h(·)) for quantization error, with ‘-’ indicating
no explicit loss. DCC stands for Discrete Cyclic Coordinate, while BQP denotes Binary Quadratic Programming. The
Complexity column reports the convergence rate, with ‘-’ indicating no such analysis.

    • Comparable complexity guarantee. We provide rigorous non-asymptotic complexity
      analysis for deep hashing optimization under the challenging stochastic, nonconvex, multi-
      block primal-dual framework. Specifically, to attain stochastic critical points of Problem
      (P), we establish an O(ε−4 ) oracle complexity result for DualHash-StoM and, under the
      mean-square smoothness condition, an improved O(ε−3 ) complexity result for DualHash-
      StoRM, matching the best-known lower bound for stochastic smooth optimization [1].
      Table 1.1 summarizes the comparison with existing deep hashing methods.
    • Superior empirical performance. Extensive experiments on three widely-used datasets
      demonstrate the practical effectiveness of DualHash. In particular, the proposed method
      achieves consistently lower quantization errors across different bit lengths compared to
      baselines, validating the advantages of our primal-dual framework for image retrieval in
      both single-label and multi-label settings.
    1.2. Related Work.
    Optimization in Deep Hashing. W-type regularizers such as ∥|z| − 1∥1 have been widely
adopted in deep hashing to bridge the gap between continuous network outputs and discrete
binary codes [39, 55, 71]. Alternative strategies either introduce auxiliary hash codes with a
smoother surrogate ∥ · ∥22 [30, 36, 37], or refine the discretization objective via greedy coding,
central similarity targets, or proxy-based supervision [51, 65, 11]. Nevertheless, even recent
architectures continue to rely on W-type regularization to mitigate quantization error. For
instance, He and Wei [27] employ ∥|z| − 1∥22 as a smooth surrogate for ∥|z| − 1∥1 to ease
gradient-based optimization, highlighting that the core difficulty of developing theoretically
grounded methods for nonsmooth W-type regularization under nonlinear network mappings
has not been resolved. This is precisely the gap this work aims to fill.
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                   5


     Primal-Dual and Stochastic Splitting Methods. Primal-dual methods, such as PDHG [10]
and its unified extensions [22], are foundational tools for composite optimization with broad
applications in imaging sciences. These methods enjoy ergodic O(1/k) convergence and remain
stable under adjoint mismatch [13]. For convex nonlinear saddle-point models, Gao and
Zhang [24] proposed an alternative extrapolation scheme with provable convergence guarantees.
Stochastic counterparts, notably SPDHG [9] and the stochastic majorize-minimize subspace
method [14], handle structured finite-sum objectives with almost-sure and ergodic convergence.
In the nonconvex setting, Clason et al. [15] established global convergence under the Kurdyka–
Łojasiewicz (KL) condition, while Guo et al. [26] further addressed nonconvex stochastic
composite problems. More recently, Shumaylov et al. [50] incorporated weakly convex neural-
network regularizers into a primal-dual framework and proved convergence of PDHGM to
critical points under the KL condition. When the problem admits a block-separable structure,
splitting methods such as ADMM with its stochastic variants [58, 70, 12, 3] and stochastic
alternative minimization [20, 45, 4] provide promising approaches for nonconvex multi-block
optimization.
     Stochastic Nonconvex Constrained Optimization. From a constrained optimization perspective,
Problem (1.1) can be reformulated as a nonlinearly constrained optimization problem of the
form
                               minq f (x) + h(x) s.t. c(x) = 0.
                              x∈R

Several recent works have established oracle complexity results for first-order stochastic
augmented-Lagrangian and penalty methods for problems of this type. For instance, Jin
and Wang [32] proposed a single-loop stochastic primal-dual method based on a linearized
augmented-Lagrangian function, achieving oracle complexities of O(ε−6 ) from an arbitrary
initial point and O(ε−5 ) from a feasible one for locating an ε-KKT point. By incorporating the
recursive momentum estimator of [16], Shi et al. [49] developed a momentum-based linearized
augmented-Lagrangian method, improving these bounds to O(ε−4 ), or the nearly optimal
O(ε−3 ) when initialized at a nearly feasible point. To certify a stronger notion of stationarity
that deterministically enforces constraint violation of at most ε, Lu et al. [40] introduced a
linearized quadratic-penalty method with truncated momentum. Under a regularity condition
on the constraint gradients, recursive momentum yields the nearly optimal complexity O(ε  e −3 ).
Notations. We use a, a, and A to denote scalars, vectors, and matrices, respectively.
Throughout this paper, 1 represents a vector of all ones, and i, j serve as indices. For matrix
variables B, Y , and Λ, we work directly in     n×d , equipped with the Frobenius inner product
                                            pR
⟨·, ·⟩ and induced Frobenius norm ∥ · ∥ := ⟨·, ·⟩. Unless specified otherwise, ∥ · ∥ denotes the
Euclidean norm for vectors and the Frobenius norm for matrices. | · | denotes element-wise
absolute value. For a closed set C ⊆ Rq , the distance from a point x ∈ Rq to C is defined as
dist(x, C) := miny∈C ∥x − y∥.
     2. Preliminary. The extended real-valued function f : Rq → (−∞, +∞] is called proper
if its domain dom(f ) := {x ∈ Rq | f (x) < +∞} is nonempty and inf q f > −∞; it is called
                                                                       x∈R
closed if its epigraph epi(f ) := (x, t) ∈ Rq+1 | f (x) ≤ t is closed. For a proper, closed, and
                                 
6                                                     LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


convex function f , the proximal mapping of f , denoted by proxτ f (y), is defined as
                                                                    
                                                        1          2
                        proxτ f (y) := argmin f (x) + ∥x − y∥ ,
                                       x∈dom(f )       2τ

where τ is a positive scalar parameter. The proximal mapping admits the following properties.
    Proposition 2.1. [47] Let f : Rq → R ∪ {+∞} be proper and closed. Then, the following
holds.
    (i) The set proxτ f (x) is nonempty and compact for any x ∈ Rq and τ > 0.
   (ii) If f is convex, then proxτ f (x) contains exactly one value for any x ∈ Rq and τ > 0.
    We use the StoRM (stochastic recursive momentum) variance-reduction estimator [16]. In
stochastic optimization problems where F (x) = Eξ [F (x; ξ)] with ξ ∈ Ξ, standard momentum
methods use

                     dk = ρk ∇F (xk , ξ k ) + (1 − ρk )dk−1 , where ρk ∈ [0, 1],

yielding a biased estimate of ∇F (xk ). StoRM adds the correction term (1−ρk−1 )(∇F (xk , ξ k )−
∇F (xk−1 , ξ k )), resulting in:

                       ∇F xk , ξ k  ,
                                  
            d =k
                              k  k              k−1           k−1   k
                                                                       if k = 1,
                       ∇F x , ξ + (1 − ρk−1 ) d     − ∇F x , ξ          , if k > 1,

This estimator exploits the smoothness of F (·, ξ), leading to efficient variance reduction. When
ρk = 1, dk reduces to vanilla mini-batch SGD. We set ρk ∈ (0, 1) for k ≥ 1 in this paper.
    A function f : Rq → R is said to be Lf -smooth if ∥∇f (x) − ∇f (x′ )∥ ≤ Lf ∥x − x′ ∥ for all
x, x′ ∈ Rq . A mapping M : D → Rl is said to be C-Lipschitz continuous if ∥M(x) − M(x′ )∥ ≤
C∥x − x′ ∥ for all x, x′ ∈ D. For an Lf -smooth function, the descent lemma states that

                                                                 Lf
                       f (x) ≤ f (x′ ) + ⟨∇f (x′ ), x − x′ ⟩ +      ∥x − x′ ∥2 .
                                                                 2
    3. Problem Reformulation.
   3.1. W-type Regularized Deep Hashing Model. Deep hashing aims to learn network
parameters x ∈ Rdx that generate binary codes preserving semantic similarity in the Hamming
space. A direct formulation is
                                         n
                                       1X                  
                                   min     ℓs,i sgn(Ci (x)) ,
                                  xRdx n
                                           i=1

where n is the number of training samples, Ci (x) ∈ Rd is the output of the i-th sample, and
ℓs,i is a similarity-preserving loss (e.g., pairwise cross-entropy [71]). This objective is hard to
optimize by backpropagation because sgn(·) is nondifferentiable and has zero gradient almost
everywhere.
     To address this issue, W-type regularization (e.g., h(z) = λ |z| − 1 ) is introduced to drive
continuous outputs toward {−1, 1}. Specifically, the model is trained to produce the i-th
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                    7


continuous network output Di (x) with W-type regularization h, and binary codes are obtained
post-training via bi = sgn(Di (x)). Consequently, we formulate the core optimization problem
for W-type regularized deep hashing methods as follows
                                             n                  n
                                        1X          X
(3.1)                          min         fi (x) +   h(Di (x)),
                              x∈Rdx     n
                                             i=1            i=1

where fi (x) = ℓs,i (x) + µ2 ∥x∥2 with µ > 0 to ensure coercivity.
    3.2. Structured Reformulation. We then employ variable splitting by introducing auxiliary
variables bi = Di (x) for all i = 1, . . . , n, and then apply the quadratic penalty method to
obtain the following two-block finite-sum optimization problem:
                                                            1   Pn
                                                   F (x,B)= n       i=1 Fi (x,bi )
                                    z                 }|                {
                                       n                n                   n
                                    1 X             γ  X                    X
(P)             min       J(x, B) =       fi (x) +        ∥Di (x) − bi ∥2 +    h(bi ) .
            x∈Rdx ,B∈Rn×d           n              2n
                                    | i=1{z    } | i=1 {z               } |i=1 {z }
                                         f (x)                          P (x,B)      h(B)

where we define B = [b1 , b2 , . . . , bn ] and γ > 0 as the penalty parameter. This reformulation
decomposes the original problem (3.1) into simpler structural blocks, enabling us to exploit the
problem structure better. Building upon Problem (P), we develop a stochastic primal-dual
algorithm that addresses the lack of tractable proximal mappings in the nonsmooth composite
form and the absence of convergence guarantees in existing deep hashing methods.
      4. Stochastic Primal-Dual Hashing Algorithms.
   4.1. Algorithmic Framework. Let us consider the equivalent constrained formulation of
Problem (P):

                                       min                 F (x, B) + h(Y )
                            x∈Rdx ,B∈Rn×d ,Y ∈Rn×d

                                                    s.t.   Y = B,

whose corresponding dual problem is

(D)               max            min                   F (x, B) + h(Y ) + ⟨Λ, B − Y ⟩.
                 Λ∈Rn×d x∈Rdx ,B∈Rn×d ,Y ∈Rn×d

Recalling the definition of the conjugate function [47]:

(4.1)                       h∗ (x) := sup {⟨x, y⟩ − h(y)},                  x ∈ Rn
                                       y∈Rn

and observing that the minimization with respect to Y in Problem (D) can be performed
independently, we then simplify the Lagrangian function in the problem (D) to

(4.2)                      L(x, B, Λ) := F (x, B) + ⟨B, Λ⟩ − h∗ (Λ).
8                                                               LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


                                                                                                    τ
                                                                                     −(τ + λ)
                                      z                                   x                                            x
            −λ          λ                          −λ          λ                               −λ          λ (τ + λ)
                                                                                                 −τ

    (a) h(z) = λ ∥|z| − 1∥1 , λ = 1               (b) h∗ (x), λ = 1                       (c) proxh∗ (x), λ = τ = 1

         Figure 4.1: A W-type function, its conjugate function, and the proximal mapping of h∗ .

Through this transformation, we replace h(Y ) with −h∗ (Λ) and optimize the h∗ in the dual
space. The convexity of h∗ makes proxh∗ (·) often much easier to compute (potentially having
a closed-form solution) than proxh (·). For the W-type regularization on the matrix variable
B ∈ Rn×d ,
                                                               n X
                                                               X d
                                      h(B) = λ∥|B| − 1∥1 =               λ |bij | − 1 ,
                                                               i=1 j=1

its conjugate is separable element-wise:
                               n X
                                 d
                                                              (
                    ∗
                               X
                                          ∗                ∗   |Λij |, if Λij ∈ [−λ, λ],
                   h (Λ) =         h (Λij ), where h (Λij ) =
                           i=1 j=1
                                                               +∞, otherwise,

whose proximal operator in the piecewise solution (see Figure 4.1 for details) is
                                       
                                       
                                       λ,         Yij > λ + τ,
                                       
                                       Yij − τ, τ < Yij ≤ λ + τ,
                                       
                                       
                                       
(4.3)               [proxτ h∗ (Y )]ij = 0,         −τ ≤ Yij ≤ τ,
                                       
                                        Yij + τ, −λ − τ ≤ Yij < −τ,
                                       
                                       
                                       
                                       
                                       
                                        −λ,        Yij < −λ − τ.
                                       

This motivates us to perform a proximal step to update Λ.
    We now outline the DualHash framework, which employs an alternating update scheme
for x, B, Λ.
    First, we adopt a stochastic first-order method for the x-update since computing the full
gradient ∇x F (x, B) is computationally expensive when the number of samples n is large, as is
typical in deep learning. Specifically, at each iteration k, we update x by utilizing a stochastic
gradient descent step
                                  xk+1 = xk − ηk ∇ ˜ x F (xk , B k ),

where ∇ ˜ x F (xk , B k ) is a stochastic estimator of ∇x F (xk , B k ).
     Second, the auxiliary variable B is updated via a vanilla gradient descent step (4.4); this
suffices because the objective (4.2) with respect to B is smooth with an inexpensive gradient,
                                                                                    
(4.4)        B k+1 = B k − τ ∇B L(xk+1 , B k , Λk ) = B k − τ ∇B F (xk+1 , B k ) + Λk .
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                   9


Algorithm 4.1 DualHash-StoM
 1: Initialize:   x1 = z 1 = y 1 ∈ Rdx , B 1 ∈ Rn×d , Λ1 ∈ Rn×d , parameters {ηk }, τ , bk ,
    {αk , βk ∈ (0, 1)}
 2: for k = 1, 2, . . . do
 3:   Compute Gk by (4.6)
 4:   Update xk+1 by (4.7)
 5:   Update B k+1 by (4.4)
 6:   Update Λk+1 by (4.5)
 7: end for
 8: Return: xR where R is uniformly sampled from {2, . . . , T + 1}



    Finally, the dual variable Λ is updated via proximal gradient ascent on h∗ , using an
extrapolation step introduced in [10]. The update solves the following optimization problem:
                                                       
               Λk+1 = prox(1/τ h∗ ) Λk + 2B k+1 − B k
                                   n                              τ          o
(4.5)               = arg min        h∗ (Λ) − ⟨Λ, 2B k+1 − B k ⟩ + ∥Λ − Λk ∥2 .
                          Λ∈Rn×d                                  2
                                                   ˜ x F (xk , B k ), enabling the incorporation
    Our framework accommodates flexible choices of ∇
of advanced momentum techniques to enhance both theoretical complexity and practical
performance.
    4.2. Implementations.
    4.2.1. DualHash with Momentum Acceleration. We first propose a stochastic first-
order algorithm with momentum acceleration for solving Problem (P). At each iteration
k, we uniformly sample a mini-batch Jk ⊆ {1, · · · , n} with bk = |Jk | ≤ n, and compute
˜ x F (xk , B k ) := ∇x F (xk , B k ; Jk ) evaluated at the extrapolated point z k :
∇

(4.6)             Gk = ∇x F (z k , B k ; Jk ),   where z k = xk + βk (xk − xk−1 ).

The update of xk is then:

(4.7)                xk+1 = y k − ηk Gk , where y k = xk + αk (xk − xk−1 ).

The overall algorithm, DualHash-StoM, is detailed in Algorithm 4.1.
    Remark 4.1. The momentum acceleration framework in (4.7) covers these two accelerations:
Nesterov acceleration [43] corresponds to αk = βk ̸= 0; the heavy-ball method [46] is obtained
with βk = 0; vanilla SGD is recovered when αk = βk = 0. In this paper, we set αk , βk ∈ (0, 1)
for all k ≥ 1.

    4.2.2. DualHash with Recursive Momentum. Alternatively, we consider the StoRM
estimator [16] for the x-update, which achieves variance reduction without requiring full-
gradient or large-batch gradient computations. Given its practical effectiveness in deep learning
10                                                     LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


Algorithm 4.2 DualHash-StoRM
 1: Initialize: x1 ∈ Rdx , B 1 ∈ Rn×d , Λ1 ∈ Rn×d , parameters {ηk }, τ , bk , {ρk ∈ (0, 1)}
 2: for k = 1, 2, . . . do
 3:   Compute D k by (4.8)
 4:   Update xk+1 by (4.9).
 5:   Update B k+1 by (4.4).
 6:   Update Λk+1 by (4.5).
 7: end for
 8: Return: xR where R is uniformly sampled from {2, . . . , T + 1}


[34], we construct a recursive momentum estimator D k for ˜     ∇x F (xk , B k ) as follows:
             (
         k     ∇x F (x1 , B 1 ; J1 ),                                                     if k = 1,
(4.8) D =                       k−1           k−1  k−1
                                                                          k     k
               (1 − ρk−1 ) D         − ∇x F (x , B     ; Jk−1 ) + ∇x F (x , B ; Jk ), if k > 1.

The update of x is then

(4.9)                                   xk+1 = xk − ηk D k .

The overall algorithm, DualHash-StoRM, is detailed in Algorithm 4.2.
    5. Complexity analysis. We now analyze the complexity of the proposed algorithms in
terms of the number of stochastic first-order oracle (SFO) calls required to find an ε-approximate
stochastic critical point of the Lagrangian function (4.2). Specifically, we seek a point satisfying

(5.1)                            E[dist2 (0, ∂L(x∗ , B ∗ , Λ∗ ))] ≤ ε2

for a given ε > 0. We assume in this paper the set critL := {(x∗ , B ∗ , Λ∗ ) ∈ Rdx ×Rn×d ×Rn×d :
0 ∈ ∂L(x∗ , B ∗ , Λ∗ )} is nonempty. This focus is standard in non-convex stochastic first-order
optimization analysis, as finding a global or even local minimizer for non-convex optimization
problems is generally NP-hard. We limit the maximum number of iterations to a fixed integer,
denoted by T with a little abuse of notation.
    In the remainder of this paper, we impose the following assumptions.
     Assumption 5.1. (i) Assume that the objective of Problem (P) is lower bounded by J ∗ ;
 (ii) Assume that F (x, B) is LF = O(1)-smooth with respect to (x, B) at x ∈ Rdx and
      B ∈ Rn×d ;
(iii) Suppose index i ∼ Uniform{1, . . . , n}. For any B ∈ Rn×d , there exists a constant σ 2 ≥ 0
      such that Ei [∇x Fi (x, bi )] = ∇x F (x, B) and Ei [∥∇x Fi (x, bi ) − ∇x F (x, B)∥2 ] ≤ σ 2 .
    Remark 5.1. The smoothness constant LF remains O(1), as the penalty formulation cancels
                √
the potential O( n) scaling. A rigorous analysis is in Appendix B.
We first prove that the iterates generated by our algorithms are bounded, which is crucial for
the subsequent convergence analysis.
    Lemma 5.1. Let {(xk , B k , Λk )}k∈N be generated by Algorithm 4.1 or 4.2. Under Assump-
tion 5.1(i), the sequence {(xk , B k , Λk )}k∈N is bounded.
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                     11


    Proof. For Λk , by the closed-form proximal mapping in (4.3), Λk+1 ∈ [−λ, λ]n×d . Hence
{Λk } is bounded.
   For (xk , B k ), since {Λk } is bounded and J is coercive in (x, B) with J(xk , B k ) ≥ J ∗ by
Assumption 5.1(i), the sublevel set {(x, B) : J(x, B) ≤ c} is bounded for any c < +∞, hence
{(xk , B k )} is bounded.
   We then prove that any primal-dual optimal solution (x∗ , B ∗ , Λ∗ ) satisfies the first-order
necessary optimality condition of the Lagrangian (4.2), i.e., it is a critical point of L.
    Lemma 5.2. Under Assumption 5.1(i), suppose that(x∗ , B ∗ , Λ∗ ) are the optimal solutions
of Problem (P) and Problem (D), respectively. Then, it holds that (x∗ , B ∗ , Λ∗ ) ∈ critL, i.e.,
0 ∈ ∂L(x∗ , B ∗ , Λ∗ ).
    Proof. First, due to the coercivity of L(x, B, Λ) , we have min L(x, B, Λ) > −∞. According
                                                                x,B
to [6, Theorem 2.158], (x∗ , B ∗ , Λ∗ ) are optimal solutions to the primal problem (P) and the
dual problem (D) if and only if:
                        
                        (x∗ , B ∗ ) ∈       argmin     L(x∗ , B ∗ , Λ∗ ),
(5.2)                                     x∈Rdx ,B∈Rn×d
                           0 = h(B ∗ ) + h∗ (Λ∗ ) − ⟨Λ∗ , B ∗ ⟩.
                        

    The first condition in (5.2) implies that 0 belongs to the subdifferential of L with respect
to (x, B) at (x∗ , B ∗ , Λ∗ ). Since L is continuously differentiable in (x, B), we have
                                  (
                                    0 = ∇f (x∗ ) + ∇x G(x∗ , B ∗ ),
(5.3)
                                    0 = ∇B G(x∗ , B ∗ ) + Λ∗ .
    For the second condition in (5.2), by the definition of the conjugate function, if this equality
holds, then Λ∗ is the optimal solution of sup{⟨b∗ , λ⟩ − h∗ (λ)}, i.e.,
(5.4)                                     B ∗ ∈ ∂h∗ (Λ∗ ).
Combining (5.3) and (5.4), we obtain 0 ∈ ∂L(x∗ , B ∗ , Λ∗ ), completing the proof.
    Our analysis aims to bound E[∥∂L∥2 ]. The presence of dual variables Λ renders the
Lagrangian (4.2) non-descent, precluding the direct use of standard methods [62, 67]. To this
end, we construct suitable Lyapunov functions (see (5.12) and (5.21)) to obtain the approximate
descent property. We first analyze the dual dynamics by establishing bounds on ∥Λk+1 − Λk ∥2
in terms of the primal residuals, enabling the Lyapunov construction.
    Lemma 5.3. Under Assumption 5.1, for any k ≥ 1, it holds that
            ∥Λk+1 − Λk ∥2
(5.5)     ≤ 3τ −2 ∥B k+2 − B k+1 ∥2 + 3(τ −1 + LF )2 ∥B k+1 − B k ∥2 + 3L2F ∥xk+2 − xk+1 ∥2 .
    Proof. Using the update of B k in (4.4) twice yields that
        ∥Λk+1 − Λk ∥
    = τ −1 (B k+1 − B k+2 ) + τ −1 (B k+1 − B k ) + ∇B F (xk+1 , B k ) − ∇B F (xk+2 , B k+1 )
    ≤ τ −1 ∥(B k+1 − B k+2 )∥ + (τ −1 + LF )∥B k+1 − B k ∥ + LF ∥xk+2 − xk+1 ∥,
12                                                         LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


where the second inequality uses the smoothness of F (x, B) in Assumption 5.1. Therefore,
using the fact (a + b + c)2 ≤ 3(a2 + b2 + c2 ), one has
              ∥Λk+1 − Λk ∥2
(5.6)       ≤ 3τ −2 ∥B k+2 − B k+1 ∥2 + 3(τ −1 + LF )2 ∥B k+1 − B k ∥2 + 3L2F ∥xk+2 − xk+1 ∥2 ,
and completes the proof.
   Next, we analyze how the Lagrangian function (4.2) changes during the updates of primal
and dual variables in one iteration. The total change can be decomposed as:
             L(xk+1 , B k+1 , Λk+1 ) − L(xk , B k , Λk )
(5.7)      = L(xk+1 , B k+1 , Λk+1 ) − L(xk+1 , B k , Λk ) + L(xk+1 , B k , Λk ) − L(xk , B k , Λk ) .
             |                   {z                     } |                    {z                 }
                     one-iteration progress on B and Λ                  one-iteration progress on x

Since the updates of B and Λ are common to Algorithms 4.1 and 4.2, the following lemma
bounds their one-step change. For notational simplicity, we define
                                   ∥xk − xk−1 ∥2                 ∥B k − B k−1 ∥2F
                           ∆kx =                 ,       ∆kB =                    .
                                        2                               2

     Lemma 5.4 (One-iteration progress on B and Λ). Consider the sequence {xk , B k , Λk }k∈N
generated by Algorithm 4.1 or Algorithm 4.2. Under Assumption 5.1(ii), for any k ≥ 1, it holds
that
                          L(xk+1 , B k+1 , Λk+1 ) − L(xk+1 , B k , Λk )
(5.8)                   ≤K1 ∆k+2     k+1     k       k+2
                             B − K2 ∆B + K3 ∆B + K4 ∆x   + K3 ∆k+1
                                                               x ,
                                                                           τ L2F
where K1 = 3δτ −1 , K2 = 2τ −1 − LF − 3δτ (τ −1 + LF )2 , K3 =                                 2
                                                                             δ , and K4 = 3δτ LF .
     Proof. Using the smoothness of F with respect to B, we conclude that
     L(xk+1 , B k+1 , Λk+1 ) − L(xk+1 , B k , Λk )
(5.9)
          
     −1 LF                    D                                                    E
 ≤− τ −      ∥B k+1 − B k ∥2 + Λk+1 − Λk , τ ∇B F (xk , B k−1 ) − ∇B F (xk+1 , B k ) .
         2
For the inner product term in (5.9), we apply the block-wise separability of (4.2) with respect
to B = {bi }ni=1 and Λ = {λi }ni=1 and obtain
                   ⟨Λk+1 − Λk , τ (∇B F (xk , B k−1 ) − ∇B F (xk+1 , B k ))⟩
                    δτ                   τ
                  ≤ ∥Λk+1 − Λk ∥2 + ∥∇B F (xk+1 , B k ) − ∇B F (xk , B k+1 )∥2
                     2                  2δ
                    δτ                  τ L2F  k+1                             
(5.10)            ≤ ∥Λk+1 − Λk ∥2 +            ∥x      − xk ∥2 + ∥B k − B k−1 ∥2 ,
                     2                    2δ
where the first inequality follows from the fact that ⟨a, b⟩ ≤ 2δ ∥a∥2 + 2δ
                                                                          1
                                                                            ∥b∥2 for any δ > 0
and the second inequality follows from the smoothness of F with respect to B again. Then,
substituting (5.10) and (5.6) into (5.9) and rearranging the inequality yields (5.8).
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                       13


    We now analyze the stochastic x-updates, which introduces algorithm-specific challenges:
as (5.5) and (5.10) show, randomness in the x-updates propagates to the B- and Λ-updates,
necessitating careful parameter tuning and the construction of Lyapunov functions. We present
the key lemmas and complexity results of both algorithms below2
    5.1. Oracle complexity of DualHash-StoM. Let us denote the gradient error in the k-th
iteration as
                                ek = Gk − ∇x F (z k , B k ),
where Gk is defined in (4.6). Under Assumption 5.1(iii), one has E[∥ek ∥2 ] ≤ σ 2 /bk . The
one-iteration progress on x is estimated as follows, with the proof provided in Appendix C.1.
    Lemma 5.5 (One-iteration progress on x). Consider the sequence {xk , B k , Λk }k∈N generated
by Algorithm 4.1. Under Assumption 5.1(ii) and the parameters in (5.18), for any k ≥ 1, it
holds that
                                                                                   1
(5.11)       L(xk+1 , B k , Λk ) − L(xk , B k , Λk ) ≤ −K5 ∆k+1
                                                            x   + K6 ∆kx +           ∥ek ∥2 ,
                                                                                  LF

where K5 = 1−α                αk        2
            ηk − 2LF and K6 = ηk + 2LF βk .
              k


    To handle the oscillation of (4.2), we introduce the following Lyapunov function and sequence
to establish an in-expectation approximate sufficient descent property for Algorithm 4.1:

(5.12)           Ψm (Qk ) = L(xk , B k , Λk ) − C1 ∆k+1     k       k+1
                                                    B + C2 ∆B − C3 ∆x   + C4 ∆kx ,
                      Qk := {xk , B k , Λk , B k+1 , B k−1 , xk+1 , xk−1 },   {Qk }k∈N ,

where the positive coefficients C1 , C2 , C3 , C4 (independent of k) are chosen as:
                     
                     
                     
                      C1 = K 1 ,
                     C2 = 1 (K2 − K1 + K3 ),
                     
                     
                                2
(5.13)
                     
                      C3 = K4 ,
                                     2L β 2
                     
                     C = L̄F + Fα , where L̄ = 2L + K + K .
                     
                     
                         4        1−2α−ν                F      F      3    4

We also assume the parameters {ηk }, {αk , βk }, τ in Algorithm 4.1 are set as follows:
                                                                p
                              η                                   δ̃
(5.14)                  ηk =    , αk = α, βk = β, τ ≤                ,
                             LF                                 LF

where η = L̄(1−2α−ν)/2
             F  (1+ν)β 2
                         , α ∈ (0, 1−ν                        2
                                    2 ), β ∈ (0, 1), δ̃ = cδ δ are given constants independent of
            LF
                 + (1−α)α
T with some ν, δ, cδ > 0. The construction yields the following in-expectation approximate
sufficient descent property of Ψm (Qk ). The proof is provided in Appendix C.2, and the positivity
of the coefficients in (5.13) and the descent constants in (5.16) is established in Lemma C.2.
   2
   Detailed proofs of all lemmas and theorems, along with complete parameter derivations, are given in
Appendices C and D.
14                                                        LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


    Lemma 5.6. Under Assumption 5.1 and the parameter conditions (5.14), for any k ≥ 1, it
holds that
                                                                                       σ2
(5.15)    E[Ψm (Qk+1 )] − E[Ψm (Qk )] ≤ −CB E[∆k+1  k           k+1
                                               B + ∆B ] − Cx E[∆x   + ∆kx ] +               ,
                                                                                      bk LF
where the descent coefficients are given by
                             (
                               CB = C2 − K3 = K2 −K23 −K1 > 0,
(5.16)
                               Cx = νC4 > 0.

    We now present the convergence rate of Algorithm 4.1 under the parameter conditions
(5.14), which guides the selection of the batch size bk .
    Lemma 5.7 (Convergence rate of Algorithm 4.1). Under the conditions of Lemma 5.6, for
any T ≥ 1, there exists R uniformly selected from {2, . . . , T + 1} such that (xR , B R , ΛR )
satisfies (5.1) with

                                                             L∆ ∆1     σ2
(5.17)                  E[dist2 (0, ∂L(xR , B R , ΛR ))] ≤         + Cσ ,
                                                              T        bk
                                                          1 ,S2 }+max{S3 ,S4 }
where S1 , S2 , S3 , S4 are defined in (C.10), L∆ := max{Smin{C  B ,Cx }
                                                                               , Cσ := L
                                                                                       LF + 3, and
                                                                                        ∆


∆1 := Ψ1m − Ψ∗m denotes the initial objective gap with Ψ1m = Ψm (Q1 ) and the lower bound Ψ∗m .
Here L∆ and Cσ are constants independent of T .
From (5.17), to achieve the O(T −1/2 ) convergence rate, we need to set bk = Θ(T ). Therefore,
combining (5.14) with bk = b = cb T , the complete parameter setting in Algorithm 4.1 is
                                                        p
                        η                                 δ̃
(5.18)            ηk =     , αk = α, βk = β, τ ≤             , bk = b = cb T,
                       LF                               LF

where η, α, β, τ , δ̃, cb are given constants independent of T , and ν, δ, cδ > 0 are positive
constants. The complexity result is established in Theorem 5.2, with the proof in Appendix C.4.
    Theorem 5.2. Under Assumption 5.1, let {xk , B k , Λk }k∈N be the iterate sequence from
Algorithm 4.1 with the parameters satisfying (5.18). Then there exists R uniformly selected
from {2, . . . , T + 1} such that (xR , B R , ΛR ) satisfies (5.1) provided that T satisfies
                                                             2
                                               L∆ ∆1 + Cσ σcb
                                           &                     '
(5.19)                               T =                             .
                                                     ε2

Moreover, the total number of SFO calls is of order O(ε−4 ).
    5.2. Oracle complexity of DualHash-StoRM. Before proceeding, we introduce an addi-
tional assumption, also known as the mean-squared smoothness assumption, widely used in
variance reduction methods [1, 23, 52, 20].
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                         15


    Assumption 5.2. There exists LF > 0 such that for any index i selected uniformly from
{1, 2, . . . , n}, x, x′ ∈ Rdx , and B, B ′ ∈ Rn×d with bi , b′i ∈ Rd denoting the i-th rows of B, B ′ ,
it holds that
                  Ei ∥∇x Fi (x, bi ) − ∇x Fi (x′ , b′i )∥2 ≤ L2F ∥x − x′ ∥2 + ∥B − B ′ ∥2 .
                                                                                      

This assumption is slightly stronger than the LF -smoothness of the finite-sum function F
in Assumption 5.1(ii). In particular, this assumption implies Assumption 5.1(ii) by Jensen’s
inequality, but not conversely.
    Let us denote the gradient error
                                      ek = D k − ∇x F (xk , B k ),
where D k is defined in (4.8). Similarly, the following lemma estimates the one-iteration progress
of the Lagrangian function (4.2) via the x-update (4.9).
    Lemma 5.8 (One-iteration progress on x [62]). Consider the sequence {xk , B k , Λk }k∈N
generated by Algorithm 4.2. Under Assumption 5.1(i)-(ii), for any k ≥ 1, it holds that
                                                            ηk k 2
(5.20)          L(xk+1 , B k , Λk ) − L(xk , B k , Λk ) ≤     ∥e ∥ − (ηk−1 − LF )∆k+1
                                                                                  x .
                                                            2
We introduce the following Lyapunov function of Algorithm 4.2:
(5.21)               Ψr (Qk ) = L(xk , B k , Λk ) − C1 ∆k+1      k        k+1
                                                        B + C2 ∆ B − C3 ∆ x ,

where C1 , C2 , C3 , independent of k, are chosen as in (5.13). In the next lemma, we establish
an approximate sufficient descent property of Ψr , with the proof provided in Appendix D.1.
    Lemma 5.9. Under Assumption 5.1(i)-(ii) and Assumption 5.2, with τ satisfying (5.14),
for any k ≥ 1, it holds that
                                                               ηk
(5.22)       Ψr (Qk+1 ) − Ψr (Qk ) ≤ −CB ∆k+1
                                          B   + ∆ k     k k+1
                                                  B − Cx ∆ x   + ∥ek ∥2 ,
                                                                  2
where the descent coefficients are given by:
                      (
                         CB = C2 − K3 = K2 −K23 −K1 > 0,
                         Cxk = ηk−1 − L̃F , where L̃F = LF + K3 + K4 .
Furthermore, the following inequalities hold:
                  (                   
                     CB ∆k+1         k        k          k+1 ) + ηk ∥ek ∥2 ,
(5.23)                      B   + ∆  B ≤ Ψr (Q ) − Ψr (Q          2
                       k  k+1          k      k+1     ηk   k  2
                     Cx ∆x ≤ Ψr (Q ) − Ψr (Q ) + 2 ∥e ∥ .
    We now analysis the convergence rate of Algorithm 4.2 and we use the following parameter
setting,
                                             p
                   η           8ρη 2           δ̃
(5.24)    ηk =        1 , ρk =    2 ,   τ≤        , b1 = cb T 1/3 , bk = b (k ≥ 2),
                L̃F T 3         T3           LF

where η, ρ, δ̃, cb , and b are positive constants independent of T , and δ̃ = cδ δ 2 with δ, cδ > 0.
16                                                     LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


    Lemma 5.10 (Convergence rate of Algorithm 4.2). Under Assumption 5.1 and parameter
conditions (5.24), for any T ≥ 1, there exists R uniformly selected from {2, . . . , T + 1} such
that (xR , B R , ΛR ) satisfies (5.1) with
                                                   ˜1
                                               L̃∆ ∆
                                                          
                                                              σ2         32ρη 2 σ 2     1
                                                                                            
(5.25)       E[dist2 (0, ∂L(xR , B R , ΛR ))] ≤ 2/3 + C̃σ              +            ·         ,
                                                T           4ρη 2 b1 T      b         T 2/3
                                                                                 2
                                                                                    
where S̃4 := max{S1 , S2 } with S1 , S2 defined in (C.10), L̃∆ := CS̃B4 1 + 8η L̃
                                                                                      + 2(Cδ +4)
                                                                                              η
                                                                                                L̃F
                                                                                                    +
                                                                                 F

(7 + 3Cδ ), C̃σ := 6 + Cδ + 2CS̃4 ηL̃ , and ∆
                                            ˜ 1 := Ψ1r − Ψ∗r denotes the initial objective gap with
                                      B   F
Ψ1r = Ψr (Q1 ) and the lower bound Ψ∗r . Here L̃∆ and C̃σ are constants independent of T .
From (5.25), to acieve the O(T −2/3 ) convergence rate, we require b1 = Θ(T 1/3 ) and bk = O(1)
for k > 1. With this parameter choice, we proceed to analyze the oracle complexity of
Algorithm 4.2.
    Theorem 5.3. Under Assumption 5.1(i), (iii) and Assumption 5.2, let {xk , B k , Λk }k∈N be
the iterate sequence from Algorithm 4.2 with the parameters satisfying (5.24). Then there exists
R uniformly selected from {2, . . . , T + 1} such that (xR , B R , ΛR ) satisfies (5.1) provided that
T satisfies
                                               2                   3 
                                        ˜          σ      32ρη 2 σ 2    2
                               L̃∆ ∆1 + C̃σ 4ρη2 cb +        b
(5.26)                    T =                                            ,
                                                                          
                                                  ε3                     
                                                                         

Moreover, the total number of SFO calls is of order O(ε−3 ).
    Remark 5.4. (i) The results in Theorems 5.2 and 5.3 are novel for stochastic, non-convex,
     multi-block primal-dual optimization, a challenging area where existing work has largely
     focused on one-block [62, 49, 53] or deterministic settings [70].
(ii) Algorithm 4.1 with constant parameters from (5.18) attains both oracle and sample
     O(ε−4 ) complexities, which matches that of stochastic first-order methods (SFOMs) for
     one-block stochastic optimization [25]. Meanwhile, Algorithm 4.2 with the refined constant
     parameters from (5.24) attains both oracle and sample O(ε−3 ) complexities. This matches
     the best-known lower bound [1] and the complexity of variance-reduced SFOMs for single-
     block problems [49, 62]. Notably, despite tackling a multi-block problem, our primal-dual
     framework preserves this efficiency without degrading the dependence on ε.
    6. Numerical experiments. We conduct extensive experiments to evaluate the effectiveness
of DualHash against several state-of-the-art hashing methods on three standard image retrieval
datasets. All experiments use PyTorch 1.12.1 with CUDA 11.3 on an NVIDIA V100 GPU
platform.
    6.1. Experimental Setup.
    Datasets. CIFAR-103 is a single-label dataset comprising 60,000 32 × 32 color images
from 10 classes. In our experiment, we randomly sample 1,000 images per class (10,000 total)
     3
         https://www.cs.toronto.edu/~kriz/cifar.html
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                              17


for training, 500 per class (5,000 total) for validation, and 500 per class (5,000 total) for testing.
NUS-WIDE4 is a multi-label dataset containing approximately 270,000 web images, each
annotated with one or more labels from 81 categories. In our experiment, we use a subset
of 195,834 images from the 21 most frequent concepts (each with at least 5,000 images) and
randomly sample 700 images per class (14,700 total) for training, 300 per class (6,300 total)
for validation, and 300 per class (6,300 total) for testing. ImageNet5 is a large-scale image
dataset from the Large Scale Visual Recognition Challenge (ILSVRC 2015). It contains over
1.2 million images in the training set and 50K images in the validation set, where each image is
single-labeled by one of the 1,000 categories. Following [8], in our experiment, we randomly
select 100 categories to create ImageNet-100. We use 100 images per category (10,000 total)
for training, 30 per category (3,000 total) for validation, and 50 per category (5,000 total) for
testing. The training, validation, and testing sets are mutually exclusive with no data overlap.
    Training Setup. It is worth noting that our method is compatible with different similarity
measurement paradigms, including pointwise, pairwise and tripletwise methods. In our experi-
ments, we focus on deep supervised pairwise hashing using a pairwise cross-entropy loss f and
the nonsmooth W-type regularization term h(z) = λ∥|z| − 1∥1 .
    Baselines. We compare against eight baselines: SDH [48]; four deep hashing methods with
W-type regularizations (DHN [71], DSH [39], DTSH [57], MDSHC [55]); and three state-of-
the-art methods addressing quantization error (HashNet [8], DSDH [36], OrthoHash [28]).
Following [39], we report the mean Average Precision (mAP), precision curves (P@topK), and
precision within Hamming radius 2 (P@r=2). Detailed definitions and a full introduction of
the baselines are provided in the Supplementary Materials A.
    Architecture. For fair comparison, all deep learning-based methods employ AlexNet and
ResNet-506 as the backbone network and use ELU activation to leverage its 1-Lipschitz
continuous gradient property for satisfying the smoothness assumption (Assumption 5.1(ii)).
All images are resized to 256 × 256 pixels and center-cropped to 224 × 224. We fine-tune
the pre-trained convolutional layers (conv1–conv5) and fully connected layers (fc6–fc7), while
initializing any newly added layers with Kaiming initialization7 . For the non-deep method
(SDH [48]), we use the following input features: 512-dimensional GIST vectors for CIFAR-10,
500-dimensional bag-of-words features for NUS-WIDE, and 1024-dimensional CNN features
for ImageNet-100. During training, we monitor validation performance in real-time to observe
convergence behavior and adjust hyperparameters accordingly. For evaluation, we use the
training set as the retrieval database and the test set as queries.
    Remark 6.1. For standard ReLU-activated backbones, existing nonsmooth stochastic opti-
mization literature establishes convergence guarantees for such networks under weaker tame
or weakly convex frameworks [18, 42], which are fundamentally distinct from our smooth
primal-dual analysis. Extending our framework to this nonsmooth setting is an important
direction for future work.
   4
      http://lms.comp.nus.edu.sg/research/NUS-WIDE.htm
   5
      https://www.image-net.org/
    6
      AlexNet is the default backbone unless specified.
    7
      Although DSDH [36] uses the CNN-F architecture, it shares a similar structure with AlexNet—both consist
of five convolutional layers followed by two fully connected layers.
18                                                       LUXUAN LI, XIAO WANG, AND CHUNFENG CUI



Table 6.1: The mAP results of DualHash and baselines with different numbers of bits on CIFAR-10
and NUS-WIDE datasets.

                                   CIFAR-10 (mAP@All)                   NUS-WIDE (mAP@5000)
        Methods
                         16 bits     32 bits   48 bits   64 bits   16 bits   32 bits   48 bits   64 bits
        SDH [48]          0.4254     0.4575    0.4751    0.4855    0.4756*   0.5545*   0.5786*   0.5812*
       SDH-CNN            0.6665     0.6501    0.6484    0.6260    0.4872    0.6529    0.6451    0.5239
        DSH [39]          0.6610     0.7530    0.7817    0.8010    0.5195    0.6629    0.6617    0.6634
       DTSH [57]          0.7661     0.7501    0.7651    0.7744    0.6590    0.6845    0.7130    0.7371
       DHN [71]           0.6568     0.7608    0.7838    0.8051    0.5562    0.6427    0.6808    0.7042
      HashNet [8]         0.3354     0.6511    0.7732    0.8396    0.5879    0.6769    0.7221    0.7249
       DSDH [36]          0.2732     0.3648    0.4222    0.4941    0.5153    0.5953    0.6338    0.6619
     OrthoHash [28]       0.8070     0.8059    0.8387    0.8355    0.6653    0.6912    0.7083    0.7176
      MDSHC [55]          0.8142     0.8238    0.8350    0.8262    0.5902    0.6452    0.6081    0.6493
    DualHash-StoRM       0.8037      0.8051    0.8168    0.8345    0.6485    0.6802    0.6951    0.6982
    DualHash-StoM        0.8215      0.8481    0.8534    0.8539    0.6339    0.7002    0.7248    0.7448
*
 SDH results on NUS-WIDE are cited from HashNet and reported for reference under a similar protocol. Bold
values indicate the best results.

    Hyperparameter Settings. Unless the original implementation imposes a method-specific
component, all deep baselines are trained under the same data splits, evaluation protocol,
backbone family, image preprocessing, and early-stopping rule as DualHash. All models are
trained for a maximum of 300 epochs with early stopping based on validation mAP. We
use a fixed mini-batch size of 256 for CIFAR-10 and 128 for NUS-WIDE and ImageNet-100.
For DualHash-StoM, the learning rate ηk is selected from {10−4 , 10−3 , 10−2 , 10−1 } with step
decay, approximating ηk ∼ O(L−1       F ) without computing LF . For DualHash-StoRM, ηk is
chosen from {5 × 10 , 5 × 10 , 5 × 10−1 } following O(T −1/3 ) scaling, and ρk is selected
                       −3          −2

from {0.01, 0.05, 0.1, 0.2, 0.7} via cross-validation. For DualHash-StoM, we set the momentum
parameters αk = βk = 0.905. Baselines follow their official optimizer settings. The proximal
operator for h∗ (·) requires τ < λ to ensure well-defined updates. We tune λ ∈ {10−3 , 10−2 , 5 ×
10−2 , 10−1 } and set τ accordingly. For all datasets, we set τ = 10−2 , λ = 5 × 10−2 , and γ = 3.
In our practical implementation, we use a dual-step-size variant for multipliers Λ, i.e., the
Λ-update uses step size 0.1τ , while the theoretical analysis is stated for the unified τ -scheme in
Sections 4 and 5.
    6.2. Convergence Validation. We provide empirical evidence that is consistent with
our theoretical results (Theorems 5.2 and 5.3) and compare with HashNet, OrthoHash, and
MDSHC with 64-bit codes on CIFAR-10 and NUS-WIDE. Figure 6.1 shows the training loss
and validation mAP curves over epochs. As shown in Figure 6.1 (a)-(b), DualHash-StoRM
achieves the fastest convergence and lowest loss values on CIFAR-10 and NUS-WIDE, requiring
only approximately 25 epochs on CIFAR-10. This aligns with our theoretical outcome: the
variance reduction mechanism accelerates convergence. Though slower than DualHash-StoRM,
DualHash-StoM still outperforms other baselines (approximately 75 epochs on CIFAR-10).
Meanwhile, Figure 6.1 (c)-(d) show that although DualHash-StoRM converges faster during
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                                                                                                                                                    19


                                                                              DualHash-StoM                      DualHash-StoRM                       HashNet         OrthoHash     MDSHC


                    1.00                                                             1.00
                                                                                                                                            0.8                                          0.7

                    0.75                                                             0.75


    Training Loss                                                    Training Loss
                                                                                                                                            0.6
                                                                                                                                      mAP                                          mAP
                                                                                                                                                                                         0.6
                    0.50                                                             0.50
                                                                                                                                            0.4
                    0.25                                                             0.25
                                                                                                                                                                                         0.5

                    0.00                                                             0.00                                                   0.2
                           0       50     100      150         200                          0        50     100      150        200               0             100        200                 0      50    100    150    200
                                         Epoch                                                             Epoch                                                  Epoch                                    Epoch
                               (a) Training loss on CIFAR-10                                    (b) Training loss on NUS-WIDE                          (c) mAP@All on CIFAR-10                     (d) mAP@5000 on NUS-WIDE




Figure 6.1: Training loss and the mAP results of DualHash and baselines for 64-bit codes on CIFAR-10
and NUS-WIDE datasets.




Figure 6.2: Retrieval performance of DualHash and baselines on CIFAR-10 and NUS-WIDE datasets.
(a), (c): P@topK for 64-bit codes; (b), (d): P@r=2 with different numbers of bits.


Table 6.2: The mAP results of DualHash and baselines with different numbers of bits on CIFAR-10
and NUS-WIDE datasets with the ResNet-50 backbone.

                                                                                        CIFAR-10 (mAP@All)                                                                NUS-WIDE (mAP@5000)
  Method (ResNet-50)                                       16 bits                                 32 bits          48 bits                  64 bits              16 bits         32 bits              48 bits           64 bits
        DSH [39]                                           0.7403                                  0.8116            0.8217                  0.8239               0.5559          0.6952                0.6981           0.7165
     OrthoHash [28]                                        0.9032                                  0.9182            0.9202                  0.9224               0.7448          0.7600                0.7569           0.7863
      MDSHC [55]                                           0.8980                                  0.9007            0.9063                  0.9227               0.6428          0.7002                0.7248           0.7758
    DualHash-StoM                                              0.8559                             0.9202            0.9392                  0.9416                 0.6538         0.7783               0.7721            0.8016
Bold values indicate the best results.




training, DualHash-StoM ultimately exhibits superior retrieval performance compared to other
baselines.
   6.3. Comparison of Retrieval Performance. We report retrieval performance on CIFAR-10
and NUS-WIDE datasets, as shown in Tables 6.1 and 6.2. In terms of mAP, DualHash methods
20                                                   LUXUAN LI, XIAO WANG, AND CHUNFENG CUI



Table 6.3: The mAP results of DualHash and baselines with different numbers of bits on the ImageNet-
100 dataset with the ResNet-50 backbone.

                                               ImageNet-100 (mAP@1000)
                      Method (ResNet-50)   16 bits   32 bits   48 bits   64 bits
                      DSH [39]             0.7179    0.7448    0.7335    0.7585
                      OrthoHash [28]       0.8040    0.8292    0.8510    0.8636
                      MDSHC [55]           0.8047    0.8131    0.8561    0.8657
                      DualHash-StoM        0.7402    0.7705    0.8662    0.8714
Bold values indicate the best results.



consistently outperform most baselines, including recent advanced methods like OrthoHash and
MDSHC. On CIFAR-10, DualHash-StoM achieves 0.8539 mAP with 64-bit codes, outperforming
OrthoHash (0.8355). On the NUS-WIDE dataset, it reaches 0.7448 mAP, significantly superior
to MDSHC (0.6493). Although slightly less accurate than the StoM variant, DualHash-StoRM
remains comparable to other baselines, providing a trade-off between accuracy and efficiency.
    To further demonstrate the scalability of our approach, we evaluate our method and three
baselines using a ResNet-50 backbone. As shown in Table 6.2, this modern architecture
brings substantial performance gains in terms of mAP for all methods across CIFAR-10 and NUS-
WIDE. Our DualHash-StoM, for example, achieves mAP improvements of 10.3% on CIFAR-10
(0.9416 vs 0.8539) and 7.6% on NUS-WIDE (0.8016 vs 0.7448) at 64 bits. Nevertheless, our
method consistently maintains peak performance at 32 bits or more, outperforming all baseline
methods. Moreover, on the more complex ImageNet-100 dataset, our method continues to
demonstrate superior performance. As reported in Table 6.3, DualHash-StoM reaches 86.62%
and 87.14% mAP at 48 and 64 bits, outperforming all baselines. These results confirm that our
method maintains robust performance and strong generalization capability as data complexity
increases.
    For P@topK (Figure 6.2 (a),(c)), DualHash maintains 0.9 precision at lower top-k values
on CIFAR-10, significantly outperforming other methods. This indicates that our method
is suitable for precision-oriented image retrieval systems. For P@r=2 (Figure 6.2 (b),(d)),
DualHash-StoM consistently maintains peak performance at 32 bits and above, with 32 bits
being optimal across CIFAR-10 and NUS-WIDE. These results align with the fact that as
bit length increases, the Hamming space becomes sparse and few data points fall within the
Hamming ball with radius 2 [44].
    6.4. Quantization Error Analysis. To elucidate the mechanisms behind the performance
of DualHash, following [28], we examine the quantization error (the discrepancy between
continuous outputs and binary codes, Figure 6.3) and the Hamming distance distributions of
the resulting hash codes (Figures 6.4 and 6.5) across different bit lengths K on the NUS-WIDE
dataset. We compare DualHash-StoM against four deep hashing methods: HashNet [8] and
OrthoHash [28] (employing distinct quantization strategies), and DTSH [57] and MDSHC [55]
(both utilizing W -type regularization).
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                                                                                              21




                                                           (b) Separability versus Quantization error (16-bit)   (c) mAP@5000 values versus Quantization error (16-bit)




    (a) Quantization error across different bits numbers



                                                           (d) Separability versus Quantization error (64-bit)   (e) mAP@5000 values versus Quantization error (64-bit)



Figure 6.3: Quantization error and its impact on retrieval performance across different numbers of bits
on NUS-WIDE.




Figure 6.4: Histogram of intra-class and inter-class Hamming distances of DualHash and baselines
for 64-bit codes on the NUS-WIDE dataset. The arrow annotation is the separability in Hamming
distances, E[Dinter ] − E[Dintra ]. We normalized the frequency so that the sum over all bins equals 1.




Figure 6.5: Histogram of intra-class and inter-class Hamming distances of DualHash and baselines with
16-bit codes on NUS-WIDE.


(1) Quantization Error Correlates with Intra- and Inter-class Distances. We find
that methods achieving smaller quantization errors produce hash codes with reduced intra-class
distances and enhanced inter-class separability, as shown in Figure 6.3(a) and Figures 6.4
and 6.5. In the 64-bit setting (Figure 6.4), both HashNet and DualHash exhibit similar
inter-class distributions (close to Hamming distance K/2 = 32). However, DualHash’s lower
quantization error (0.0906 vs. 0.1390) enables significantly superior separability, i.e., the
difference in the mean of intra-class distance (the blue dotted line) with the mean of inter-class
distance (the orange dotted line). In contrast, MDSHC’s high quantization error (0.8023) leads
22                                                    LUXUAN LI, XIAO WANG, AND CHUNFENG CUI




Figure 6.6: The mAP results of DualHash-StoM with different settings of η and λ for 64-bit and 16-bit
codes on CIFAR-10 and NUS-WIDE.


to substantially larger intra-class distances and weaker inter-class separation, yielding a 5×
inferior separability ratio (24.19 vs. 4.47). This demonstrates that reducing quantization error
enhances code quality primarily by compressing intra-class distributions.
(2) DualHash Achieves Lower Quantization Error via Dual-Space Reformulation.
As shown in Figure 6.3(a), DualHash attains the smallest quantization error across different bit
lengths among all evaluated methods. This performance advantage stems from a fundamental
methodological innovation: DualHash employs a novel dual-space reformulation of the W-
regularization. The convexity property inherent in our dual formulation’s conjugate function
yields significant optimization advantages compared to directly optimizing the original nonconvex
regularization or activation functions.
(3) Bit-Dependent Performance and the Separability-Semantics Trade-off. While
lower quantization error generally improves separability (Figure 6.3(b) and (d)), its impact on
retrieval performance varies significantly across bit settings, revealing a fundamental trade-off
between class separability and semantic preservation.
▷ High-bit regime (K = 64): Lower quantization error helps produce high mAP. At K = 64
(Figures 6.3(d) and (e)), lower quantization error consistently correlates with higher mAP by
enhancing separability and preserving both semantic discriminability and visual feature quality.
Methods achieving low quantization loss (e.g., DualHash, HashNet, and DTSH) demonstrate
superior retrieval accuracy, with DualHash achieving peak mAP performance.
▷ Low-bit regime (K = 16): Low quantization error may produce low performance. At K = 16
(Figure 6.3(b) and (c)), we observe a contrasting pattern: both extremely low and extremely high
quantization error methods yield suboptimal performance. Notably, DTSH, which maintains
intermediate quantization error, achieves superior mAP. Methods with very low quantization
error (e.g., DualHash and HashNet) exhibit relatively lower mAP, particularly on multi-label
datasets (e.g., NUS-WIDE). Specifically, we observe that DualHash generates hash codes with
low intra-class distances and high inter-class separation. This creates relatively clean class
separation, but the actual visual relationships between images may get lost. Consequently,
DualHash excels on simple datasets like CIFAR-10 but shows relatively lower performance on
complex datasets like NUS-WIDE (multi-label) and ImageNet-100 (large-scale single-label).
   6.5. Parameter Sensitivity Analysis. To explore the sensitivity of key hyperparameters
η and λ in DualHash, we conduct a grid search with η ∈ {10−4 , 10−3 , 10−2 , 10−1 } and λ ∈
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                   23


{10−3 , 10−2 , 5 × 10−2 , 10−1 }. Unlike many deep hashing methods that exhibit significant
hyperparameter sensitivity, DualHash-StoM demonstrates robust performance across a wide
range of hyperparameter combinations (see Figure 6.6). Specifically, 64-bit codes achieve
excellent stability with mAP variations below 3% on CIFAR-10 and 8% on NUS-WIDE. While
16-bit codes show greater sensitivity (up to 18% on CIFAR-10), this behavior is consistent
with the intrinsic optimization challenges of extremely compact hash codes. Overall, the
method demonstrates strong stability for standard hash lengths (≥64 bits), which simplifies
hyperparameter tuning in practical high-precision retrieval applications.
    7. Conclusion and Discussion. This paper proposed DualHash, a stochastic primal-dual
algorithm for deep hashing with rigorous non-asymptotic convergence guarantees. Our approach
effectively addressed the fundamental optimization challenge of W-type regularization with
nonlinear composition through a novel “decoupling-dual-structure” strategy: transforming
the nonconvex regularizer into a convex dual space with closed-form proximal updates. We
provided a distinct theoretical framework beyond conventional primal-dual analysis, establishing
complexity bounds of O(ε−4 ) for DualHash-StoM and improved O(ε−3 ) for DualHash-StoRM.
Extensive experiments validated that DualHash achieved superior retrieval performance across
all metrics, including the highest mAP at 32 bits or more, consistently lower quantization errors,
and strong robustness on all three datasets (CIFAR-10, NUS-WIDE, and ImageNet-100).
    Beyond deep hashing, our core decoupling-dual-structure optimization principle offered
a promising pathway for tackling nonconvex regularization challenges in related domains,
including image reconsrturction [19], plug-and-play proximal methods [59] and, binary neural
networks [2].
    References.
 [1] Y. Arjevani, Y. Carmon, J. C. Duchi, D. J. Foster, N. Srebro, and B. Wood-
     worth, Lower bounds for non-convex stochastic optimization, Mathematical Programming,
     199 (2023), pp. 165–214.
 [2] Y. Bai, Y.-X. Wang, and E. Liberty, ProxQuant: Quantized neural networks via
     proximal operators, in International Conference on Learning Representations (ICLR), 2019.
 [3] F. Bian, J. Liang, and X. Zhang, A stochastic alternating direction method of multipliers
     for non-smooth and non-convex optimization, Inverse Problems, 37 (2021).
 [4] F. Bian, R. Liu, and X. Zhang, A stochastic three-block alternating minimization
     algorithm and its application to quantized deep neural networks, CSIAM Transactions on
     Applied Mathematics, (2026).
 [5] J. Bolte, S. Sabach, and M. Teboulle, Nonconvex lagrangian-based optimization:
     Monitoring schemes and global convergence, Mathematics of Operations Research, 43
     (2018), pp. 1210–1232.
 [6] J. F. Bonnans and A. Shapiro, Perturbation Analysis of Optimization Problems,
     Springer Science & Business Media, 2013.
 [7] Y. Cao, M. Long, B. Liu, and J. Wang, Deep cauchy hashing for hamming space
     retrieval, in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern
     Recognition (CVPR), 2018, pp. 1229–1237.
 [8] Z. Cao, M. Long, J. Wang, and P. S. Yu, HashNet: Deep learning to hash by
     continuation, in Proceedings of the IEEE International Conference on Computer Vision
24                                                  LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


     (ICCV), 2017, pp. 5608–5617.
 [9] A. Chambolle, M. J. Ehrhardt, P. Richtárik, and C.-B. Schönlieb, Stochastic
     primal-dual hybrid gradient algorithm with arbitrary sampling and imaging applications,
     SIAM Journal on Optimization, 28 (2018), pp. 2783–2808.
[10] A. Chambolle and T. Pock, A first-order primal-dual algorithm for convex problems
     with applications to imaging, Journal of Mathematical Imaging and Vision, 40 (2011),
     pp. 120–145.
[11] L. Chen, R. Liu, Y. Zhou, X. Ma, Y. Chen, and D. Zhang, Deep hashing with
     semantic hash centers for image retrieval, ACM Transactions on Information Systems, 43
     (2025), pp. 160:1–160:38.
[12] X. Chen, C. Cui, and D. Han, Convergence of three-block ADMM for weakly convex
     optimization problems, SIAM Journal on Imaging Sciences, 18 (2025), pp. 449–493.
[13] E. Chouzenoux, A. Contreras, J.-C. Pesquet, and M. Savanier, Convergence
     results for primal-dual algorithms in the presence of adjoint mismatch, SIAM Journal on
     Imaging Sciences, 16 (2023), pp. 1–34.
[14] E. Chouzenoux and J.-C. Pesquet, A stochastic majorize-minimize subspace algorithm
     for online penalized least squares estimation, IEEE Transactions on Signal Processing, 65
     (2017), pp. 4770–4783.
[15] C. Clason, S. Mazurenko, and T. Valkonen, Acceleration and Global Convergence of
     a First-Order Primal-Dual Method for Nonconvex Problems, SIAM Journal on Optimization,
     29 (2019), pp. 933–963.
[16] A. Cutkosky and F. Orabona, Momentum-based variance reduction in non-convex
     SGD, in Advances in Neural Information Processing Systems (NeurIPS), 2019, pp. 15236–
     15245.
[17] D. Davis and D. Drusvyatskiy, Stochastic model-based minimization of weakly convex
     functions, SIAM Journal on Optimization, 29 (2019), pp. 207–239.
[18] D. Davis, D. Drusvyatskiy, S. Kakade, and J. D. Lee, Stochastic subgradient
     method converges on tame functions, Foundations of Computational Mathematics, 20
     (2020), pp. 119–154.
[19] Q. Ding, G. Chen, X. Zhang, Q. Huang, H. Ji, and H. Gao, Low-dose ct with deep
     learning regularization via proximal forward-backward splitting, Physics in Medicine and
     Biology, 65 (2020), p. 125009.
[20] D. Driggs, J. Tang, J. Liang, M. Davies, and C.-B. Schönlieb, A stochastic
     proximal alternating minimization for nonsmooth and nonconvex optimization, SIAM
     Journal on Imaging Sciences, 14 (2021), pp. 1932–1970.
[21] D. Drusvyatskiy and C. Paquette, Efficiency of minimizing compositions of convex
     functions and smooth maps, Mathematical Programming, 178 (2019), pp. 503–558.
[22] E. Esser, X. Zhang, and T. F. Chan, A general framework for a class of first order
     primal-dual algorithms for convex optimization in imaging science, SIAM Journal on
     Imaging Sciences, 3 (2010), pp. 1015–1046.
[23] C. Fang, C. J. Li, Z. Lin, and T. Zhang, SPIDER: Near-optimal non-convex optimiza-
     tion via stochastic path-integrated differential estimator, in Advances in Neural Information
     Processing Systems, vol. 31, 2018, pp. 687–697.
[24] Y. Gao and W. Zhang, An alternative extrapolation scheme of PDHGM for saddle
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                 25


     point problem with nonlinear function, Computational Optimization and Applications, 85
     (2023), pp. 263–291.
[25] S. Ghadimi, G. Lan, and H. Zhang, Mini-batch stochastic approximation methods for
     nonconvex stochastic composite optimization, Mathematical Programming, 155 (2013),
     pp. 267 – 305.
[26] J. Guo, Preconditioned primal-dual gradient methods for nonconvex composite and finite-
     sum optimization, AIMS Mathematics, 11 (2026), pp. 2188–2226.
[27] C. He and H. Wei, Mambahash: Visual state space deep hashing model for large-scale
     image retrieval, in Proceedings of the International Conference on Multimedia Retrieval
     (ICMR), 2025, pp. 442–450.
[28] I. T. Hoe, K. W. Ng, T. Zhang, C. S. Chan, Y.-Z. Song, and T. Xiang, One loss
     for all: Deep hashing with a single cosine similarity based learning objective, in Advances
     in Neural Information Processing Systems (NeurIPS), 2021, pp. 24286–24298.
[29] Q. Hu, D. Zhu, and T. Yang, Non-smooth weakly-convex finite-sum coupled compo-
     sitional optimization, in Advances in Neural Information Processing Systems (NeurIPS),
     A. Oh, T. Naumann, A. Globerson, K. Saenko, M. Hardt, and S. Levine, eds., vol. 36,
     Curran Associates, Inc., 2023, pp. 5348–5403.
[30] Q.-Y. Jiang, X. Cui, and W.-J. Li, Deep discrete supervised hashing, IEEE Transactions
     on Image Processing, 27 (2018), pp. 5996–6009.
[31] L. Jin, J. Ma, Z. Liu, A. Gromov, A. Defazio, and L. Xiao, Parq: Piecewise-affine
     regularized quantization, in International Conference on Machine Learning (ICML), 2025,
     pp. 28044–28062.
[32] L. Jin and X. Wang, A stochastic primal-dual method for a class of nonconvex constrained
     optimization, Computational Optimization and Applications, 83 (2022), pp. 143–180.
[33] H. Lai, Y. Pan, Y. Liu, and S. Yan, Simultaneous feature learning and hash coding
     with deep neural networks, in Proceedings of the IEEE Conference on Computer Vision
     and Pattern Recognition (CVPR), 2015, pp. 3270–3278.
[34] K. Levy, A. Kavis, and V. Cevher, STORM+: Fully adaptive SGD with recursive
     momentum for nonconvex optimization, in Advances in Neural Information Processing
     Systems (NeurIPS), 2021, pp. 20571–20582.
[35] J. Li, C. Huang, R. Chan, H. Feng, M. K. Ng, and T. Zeng, Spherical image
     inpainting with frame transformation and data-driven prior deep networks, SIAM Journal
     on Imaging Sciences, 16 (2023), pp. 1177–1194.
[36] Q. Li, Z. Sun, R. He, and T. Tan, Deep supervised discrete hashing, in Advances in
     neural information processing systems (NeurIPS), 2017, pp. 2479–2488.
[37] W.-J. Li, S. Wang, and W.-C. Kang, Feature learning based deep supervised hashing
     with pairwise labels, in International Joint Conference on Artificial Intelligence (IJCAI),
     2016, pp. 1711–1717.
[38] Z. Li and J. Li, Simple and optimal stochastic gradient methods for nonsmooth nonconvex
     optimization, Journal of Machine Learning Research, 23 (2022), pp. 1–61.
[39] H. Liu, R. Wang, S. Shan, and X. Chen, Deep supervised hashing for fast image
     retrieval, in Proceedings of the IEEE Conference on Computer Vision and Pattern Recog-
     nition (CVPR), 2016, pp. 2064–2072.
[40] Z. Lu, S. Mei, and Y. Xiao, Variance-reduced first-order methods for deterministically
26                                                 LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


     constrained stochastic nonconvex optimization with strong convergence guarantees, SIAM
     Journal on Optimization, 36 (2026), pp. 1–31.
[41] X. Luo, H. Wang, D. Wu, C. Chen, M. Deng, J. Huang, and X.-S. Hua, A
     survey on deep hashing methods, ACM Transactions on Knowledge Discovery from Data,
     17 (2023).
[42] V. Mai and M. Johansson, Convergence of a Stochastic Gradient Method with Momen-
     tum for Non-Smooth Non-Convex Optimization, in International Conference on Machine
     Learning (ICML), vol. 119, 2020, pp. 6630–6639.
[43] Y. Nesterov, A method for solving the convex programming problem with convergence
     rate O(1/k^2), Proceedings of the USSR Academy of Sciences, 269 (1983), pp. 543–547.
[44] M. Norouzi, A. Punjani, and D. J. Fleet, Fast search in Hamming space with
     multi-index hashing, in Proceedings of the IEEE Conference on Computer Vision and
     Pattern Recognition (CVPR), 2012, pp. 3108–3115.
[45] T. Pock and S. Sabach, Inertial proximal alternating linearized minimization (iPALM)
     for nonconvex and nonsmooth problems, SIAM Journal on Imaging Sciences, 9 (2016),
     pp. 1756–1787.
[46] B. T. Polyak, Some methods of speeding up the convergence of iteration methods, USSR
     Computational Mathematics and Mathematical Physics, (1964).
[47] T. R. Rockafellar and R. J.-B. Wets, Variational Analysis, Grundlehren der
     mathematischen Wissenschaften, Springer-Verlag, 1998.
[48] F. Shen, C. Shen, W. Liu, and H. T. Shen, Supervised discrete hashing, in Proceedings
     of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2015,
     pp. 37–45.
[49] Q. Shi, X. Wang, and H. Wang, A momentum-based linearized augmented lagrangian
     method for nonconvex constrained stochastic optimization, Mathematics of Operations
     Research, 51 (2025), pp. 92–133.
[50] Z. Shumaylov, J. Budd, S. Mukherjee, and C.-B. Schönlieb, Weakly convex regu-
     larisers for inverse problems: Convergence of critical points and primal-dual optimisation,
     in International Conference on Machine Learning (ICML), 2024.
[51] S. Su, C. Zhang, K. Han, and Y. Tian, Greedy hash: Towards fast optimization
     for accurate hash coding in cnn, in Advances in Neural Information Processing Systems
     (NeurIPS), vol. 31, 2018.
[52] Q. Tran Dinh, D. Liu, and L. Nguyen, Hybrid variance-reduced SGD algorithms
     for minimax problems with nonconvex-linear function, in Advances in Neural Information
     Processing Systems (NIPS), vol. 33, Curran Associates, Inc., 2020, pp. 11096–11107.
[53] Q. Tran-Dinh, N. H. Pham, D. T. Phan, and L. M. Nguyen, A hybrid stochastic
     optimization framework for composite nonconvex optimization, Mathematical Programming,
     191 (2022), pp. 1005–1071.
[54] B. Wang and T. Yang, A near-optimal single-loop stochastic algorithm for convex finite-
     sum coupled compositional optimization, in International Conference on Machine Learning,
     vol. 267 of Proceedings of Machine Learning Research, PMLR, 2025, pp. 65091–65121.
[55] L. Wang, Y. Pan, C. Liu, H. Lai, J. Yin, and Y. Liu, Deep hashing with minimal-
     distance-separated hash centers, in Proceedings of the IEEE/CVF Conference on Computer
     Vision and Pattern Recognition (CVPR), 2023, pp. 23455–23464.
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                               27


[56] X. Wang and X. Chen, Complexity of finite-sum optimization with nonsmooth composite
     functions and non-lipschitz regularization, SIAM Journal on Optimization, 34 (2024),
     pp. 2472–2502.
[57] X. Wang, Y. Shi, and K. M. Kitani, Deep supervised hashing with triplet labels, in
     Asian Conference on Computer Vision (ACCV), 2016, pp. 70–84.
[58] Y. Wang, W. Yin, and J. Zeng, Global convergence of ADMM in nonconvex nonsmooth
     optimization, Journal of Scientific Computing, 78 (2019), p. 29–63.
[59] K. Wei, A. Aviles-Rivero, J. Liang, Y. Fu, H. Huang, and C.-B. Schönlieb,
     TFPnP: Tuning-free plug-and-play proximal algorithms with applications to inverse imaging
     problems, Journal of Machine Learning Research, 23 (2022), pp. 1–48.
[60] Y. Weiss, A. Torralba, and R. Fergus, Spectral hashing, in Advances in Neural
     Information Processing Systems (NeurIPS), 2008, pp. 1753–1760.
[61] R. Xia, Y. Pan, H. Lai, C. Liu, and S. Yan, Supervised hashing for image retrieval
     via image representation learning, in AAAI conference on artificial intelligence (AAAI),
     2014, pp. 2156–2162.
[62] Y. Xu and Y. Xu, Momentum-based variance-reduced proximal stochastic gradient method
     for composite nonconvex stochastic optimization, Journal of Optimization Theory and
     Applications, 196 (2023), pp. 266–297.
[63] Y. Yao, Q. Lin, and T. Yang, A note on complexity for two classes of structured non-
     smooth non-convex compositional optimization, 2024, https://arxiv.org/abs/2411.14342.
[64] P. Yin, Y. Lou, Q. He, and J. Xin, Minimization of 1-2 for compressed sensing, SIAM
     Journal on Scientific Computing, 37 (2015), pp. A536–A563.
[65] L. Yuan, T. Wang, X. Zhang, F. E. Tay, Z. Jie, W. Liu, and J. Feng, Central
     Similarity Quantization for Efficient Image and Video Retrieval, in 2020 IEEE/CVF
     Conference on Computer Vision and Pattern Recognition (CVPR), Seattle, WA, USA,
     jun 2020, IEEE, pp. 3080–3089.
[66] H. Zhang, N. Marumo, T. K. Pong, and A. Takeda, Complexity and convergence
     analysis of a single-loop sdcam for lipschitz composite optimization and beyond, 2025,
     https://arxiv.org/abs/2512.24059.
[67] J. Zhang and L. Xiao, Stochastic variance-reduced prox-linear algorithms for nonconvex
     composite optimization, Mathematical Programming, (2022), pp. 1–43.
[68] Z. Zhang and G. Lan, Optimal methods for convex nested stochastic composite opti-
     mization, Mathematical Programming, 212 (2025), pp. 1–48.
[69] X. Zhao, M. Bai, and M. K. Ng, Nonconvex optimization for robust tensor completion
     from grossly sparse observations, Journal of Scientific Computing, 85 (2020), p. 46.
[70] D. Zhu, L. Zhao, and S. Zhang, A first-order primal-dual method for nonconvex
     constrained optimization based on the augmented lagrangian, Mathematics of Operations
     Research, 49 (2024), pp. 125–150.
[71] H. Zhu, M. Long, J. Wang, and Y. Cao, Deep hashing network for efficient similarity
     retrieval, in AAAI Conference on Artificial Intelligence (AAAI), 2016, pp. 2415–2421.
28                                                           LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


    Theoretical Appendix. This supplementary material provides useful definitions and
propositions, necessary lemmas with detailed proofs, and the complete derivations for the main
theoretical results and parameters presented in the primary work.
     Appendix A. Preliminaries. We introduce the following definitions related to subdifferen-
tials.
      Definition A.1 (Subdifferentials [47]). Let f : Rq → (−∞, +∞] be a proper closed function.
                                                                                 ˆ (x), is the set
      (i) For a given x ∈ dom f , the Fréchet subdifferential of f at x, written ∂f
                              q
          of all vectors u ∈ R that satisfy
                                                  f (y) − f (x) − ⟨u, y − x⟩
                                        lim inf                              ≥ 0.
                                       y̸=x,y→x            ∥y − x∥

     (ii) The limiting-subdifferential, or simply the subdifferential, of f at x, written ∂f (x), is
          defined through the following closure process

                 ∂f (x) := {u ∈ Rq : ∃xk → x, f (xk ) → f (x), uk ∈ ˆ
                                                                    ∂f (xk ) → u as k → ∞}.
             ˆ (x) = ∂f (x) = ∅ for all x ∈
Notionally, ∂f                              / dom(f ). From Definition A.1 it follows that
ˆ                                              ˆ (x) is closed convex while ∂f (x) is merely
∂f (x) ⊂ ∂f (x) for all x ∈ dom(f ). Moreover, ∂f
closed. In particular, if f is convex, then
                   ˆ (x) = ∂f (x) = {d ∈ Rq | f (y) ≥ f (x) + ⟨d, y − x⟩, ∀y ∈ Rq }.
                   ∂f

The following proposition lists several useful properties of subgradients.
    Proposition A.2 (Properties of subdifferential[47]). Let f : Rq1 → (−∞, ∞] and g : Rq2 →
(−∞, ∞] be proper and lower semicontinuous, and let h : Rq1 → R be continuously differentiable.
Then the following results hold:
    (i) For any x ∈ Rq1 , we have ˆ  ∂f (x) ⊆ ∂f (x). If, in addition, f is convex, we have
        ˆ
        ∂f (x) = ∂f (x).
   (ii) For x ∈ Rq1 with f (x) < ∞, it holds

                                        ∂(f + h)(x) = ˆ
                                        ˆ             ∂f (x) + ∇h(x)           and
                                        ∂(f + h)(x) = ∂f (x) + ∇h(x).

     (iii) If σ(x1 , x2 ) = f1 (x1 ) + f2 (x2 ), then for any x̄1 , x̄2

                           ∂ˆx1 f1 (x̄1 )
                                                                              
                                              ˆ                     ∂x1 f1 (x̄1 )
                                            ⊆ ∂σ(x̄1 , x̄2 ) and                   ⊆ ∂σ(x̄1 , x̄2 ).
                           ∂ˆx2 f2 (x̄2 )                           ∂x2 f2 (x̄2 )

For Algorithm 4.1, the key relationships among iterates via the momentum acceleration
framework in (4.7) are established as follows.
    Proposition A.3 (Lemma 5.2 in [45]). Let {xk }k∈N , {y k }k∈N , and {z k }k∈N be sequences
generated by (4.7) and (4.6). For all k ∈ N, we have:
    (i) ∥xk − y k ∥2 = αk2 ∥xk − xk−1 ∥2 ,
   (ii) ∥xk − z k ∥2 = βk2 ∥xk − xk−1 ∥2 ,
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                  29


  (iii) ∥xk+1 − y k ∥2 ≥ (1 − αk )∥xk+1 − xk ∥2 + αk (αk − 1)∥xk − xk−1 ∥2 .
Proposition A.3(iii) decomposes the extrapolation error ∥xk+1 − y k ∥2 into a weighted combi-
nation of consecutive squared displacements, which, for αk ∈ [0, 1), enables the telescoping
descent structure essential to the Lyapunov-based convergence analysis.
     Appendix B. Discussions on Assumption 5.1(ii) in Section 5. This section verifies that
the smoothness constant LF in Assumption 5.1(ii) is independent of the sample size n, which
is essential for our convergence analysis.
     For the smooth properties of mappings in our problem, we consider the nonlinear mapping
D(x) = (D1 (x), . . . , Dn (x)) from X ⊆ Rdx to Rn×d , and make the following assumption:
    Assumption B.1 (Properties of D). Suppose there exist constants LD , L∇D > 0 such that
for any x, x′ ∈ X : ∥D(x) − D(x′ )∥ ≤ LD ∥x − x′ ∥ and ∥∇D(x) − ∇D(x′ )∥ ≤ L∇D ∥x − x′ ∥.
   Combining Assumption 5.1(i) and Assumption B.1, we can establish that there exist
constants CD , GD , CDB > 0 such that:

                       ∥D(x)∥ ≤ CD , ∥∇D(x)∥ ≤ GD , and ∥D(x) − B∥ ≤ CDB .

Based on these assumptions, we can establish the smoothness properties of the composite
function F . First, we observe that P (x, B) is nγ -smooth with respect to B and nγ (L∇D (CD +
CB ) + GD LD )-smooth with respect to x. Combining these properties, P (x, B) has an overall
smoothness constant:
                       γ
                         q
                 Ls =     2 max{L2∇D CDB2 + (2G2 + 1)L2 , 2G2 + 1} = O(1).
                                                    D     D     D
                       n
Consequently, F (x, B) is LF -smooth with respect to (x, B), where LF = O(1). An important
                                                                 √
observation is that, despite CD , GD and CDB potentially being O( n), the overall smoothness
constant LF remains independent of the sample size n, which is crucial for our convergence
analysis.
   Appendix C. Complexity analysis of DualHash-StoM (Algorithm 4.1). We adopt the
notation and intermediate results from Section 5. Let us introduce the notation that will be
used throughout our analysis:

             ξ k = {ξjk , j ∈ Jk },                            (mini-batch at iteration k)
                [k]         1   2        k
            ξ         = {ξ , ξ , . . . , ξ },   (history of mini-batches up to iteration k)
        Eξ[k] [·] = E[·|ξ [k] ].                   (conditional expectation given history)

   For dk ∈ ∂L(xk , B k , Λk ), let us denote dk = (dkx , dkB , dkΛ ) and
                                 
                                       k           k    k
                                  dx = ∇x F (x , B ),
                                 
                                      dkB = ∇B F (xk , B k ) + Λk ,
                                 
                                      dkΛ = B k − GΛ
                                                   k,
                                 

where GΛ k ∈ ∂h∗ (Λk ). We first present a lemma concerning a property of a continuously

differentiable function with a Lipschitz continuous gradient, followed by a proposition that
30                                                        LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


details the interrelations among the sequences {xk }k∈N , {y k }k∈N , {z k }k∈N generated by the
algorithm.
    Lemma C.1. Let f (x) : Rq → R be a continuously differentiable function with an L-Lipschitz
continuous gradient. Then, for any x, y, z ∈ Rq and any x+ ∈ Rq defined by
                                       x+ = y − η∇f (z), η > 0,
it holds that
(C.1)
                                                      L +          1          1
  f (x+ ) ≤ f (x) + ⟨∇f (x) − ∇f (z), x+ − x⟩ +         ∥x − x∥2 + ∥x − y∥2 − ∥x+ − y∥2 .
                                                      2           2η         2η
     Proof. By the definition of x+ , we obtain that
                                                                  1
                           x+ = argmin{⟨∇f (z), u − y⟩ +            ∥u − y∥2 },
                                   u∈Rd                          2η
and hence, in particular, by taking u = x, we obtain
                                             1         1
                    0 ≤ ⟨∇f (z), x − x+ ⟩ + ∥x − y∥2 − ∥x+ − y∥2 .
                                            2η        2η
Invoking the descent lemma for a smooth function yields the result.
     C.1. Proof of Lemma 5.5.
     Proof. Recalling the equivalent relation:
(C.2)            L(xk+1 , B k , Λk ) − L(xk , B k , Λk ) = F (xk+1 , B k ) − F (xk , B k ).
Then, applying the smoothness of F with respect to x and (C.1) yields
            F (xk+1 , B k ) ≤F (xk , B k ) + ⟨∇x F (xk , B k ) − Gk , xk+1 − xk ⟩
                              LF k+1                 1                    1
(C.3)                       +    ∥x       − xk ∥2 +      ∥xk − y k ∥2 −      ∥xk+1 − y k ∥2 .
                               2                    2ηk                  2ηk
For the inner product term in (C.3), using the facts that ⟨a, b⟩ ≤ 2s ∥a∥2 + 2s
                                                                             1
                                                                                ∥b∥2 for any s > 0
            2           2            2
and ∥a − c∥ ≤ 2∥a − b∥ + 2∥b − c∥ yields
          ⟨∇x F (xk , B k ) − Gk , xk+1 − xk ⟩
           s                   1                                   1
        ≤ ∥xk+1 − xk ∥2 + ∥∇x F (xk , B k ) − ∇x F (z k , B k )∥2 + ∥∇x F (z k , B k ) − Gk ∥2
           2                   s                                   s
                               L 2
           s k+1                               1
(C.4) ≤ ∥x        − xk ∥2 + F ∥xk − z k ∥2 + ∥∇x F (z k , B k ) − Gk ∥2 .
           2                    s              s
 Setting s = LF in (C.4) and substituting into (C.3), then applying Proposition A.3, we obtain
                                           1 − αk          ∥xk+1 − xk ∥2
           F (xk+1 , B k ) ≤ F (xk , B k ) − (     − 2LF )
                                             ηk                  2
                             αk               k
                                            ∥x − x ∥k−1 2     1
(C.5)                     +(    + 2LF βk2 )               +     ∥∇x F (z k , B k ) − Gk ∥2 .
                             ηk                   2          LF
Substituting (C.5) into (C.2) yields (5.11) and completes the proof.
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                           31


    C.2. Proof of Lemma 5.6.
    Proof. Substituting one-iteration progress (5.11) and (5.8) into (5.7) , we obtain

             L(xk+1 , B k+1 , Λk+1 ) − L(xk , B k , Λk )
                                                                                          1
(C.6)     ≤ K1 ∆k+2     k+1     k       k+2
                B − K2 ∆B + K3 ∆B + K4 ∆x   − (K5 − K3 )∆k+1
                                                         x   + K6 ∆kx +                     ∥ek ∥2 .
                                                                                         LF
Applying the definition of Ψm from (5.12) to (C.6), rearranging, and taking the conditional
expectation on both sides, we obtain
        Eξ[k+1] Ψm (Qk+1 ) − Eξ[k] Ψm (Qk )
                                         

    ≤ − (C1 − K1 )Eξ[k+1] [∆k+2                            k+1                     k
                            B ] − (K2 − C1 − C2 )Eξ[k+1] [∆B ] − (C2 − K3 )Eξ[k] [∆B ]
                                                                                                 σ2
(C.7) − (C3 − K4 )Eξ[k+1] [∆k+2                                 k+1                     k
                            x ] − (K5 − K3 − C3 − C4 )Eξ[k+1] [∆x ] − (C4 − K6 )Eξ[k] [∆x ] +         .
                                                                                                bk LF

Then substituting the values of coefficients in (5.13) and parameter conditions (5.14) into (C.7),
we derive
             h           i         h          i
      Eξ[k+1] Ψm (Qk+1 ) − Eξ[k] Ψm (Qk )
                    h      i           h    i                 h      i          h i    σ2
    ≤ − CB Eξ[k+1] ∆k+1  B    +  Eξ [k]  ∆ k
                                           B    − C x   Eξ [k+1]  ∆ k+1
                                                                    x     + Eξ [k]  ∆k
                                                                                     x  +       .
                                                                                          bk LF
Taking the full expectation of both sides leads to (5.16), thereby completing the proof.
The technical proof that the coefficients of Ψm in (5.13) and the descent constants CB , Cx are
indeed positive is given as follows.
    Lemma C.2 (Positivity of Lyapunov related coefficients). If parameter conditions (5.14) hold,
then the following inequalities are satisfied:
                                
                                
                                
                                 K5 − K3 − C3 − C4 ≥ νC4 ,
                                
                                
                                
                                
                                 C4 − K6 ≥ νC4 ,
                                
                                C > 0,
                                    4
                                
                                
                                 C B > 0,
                                
                                
                                
                                
                                 C2 , C1 > 0,
                                
                                C > 0.
                                    x

   Proof. For K5 − K3 − C3 − C4 and C4 − K6 in (C.7), we obtain by straightforward
computations by choosing some ν > 0,
                                           1 − αk
                  K5 − K3 − C3 − C4 =              − L̄F − (1 + ν)C4 + νC4
                                              ηk
                                                                           
                                                       1    L̄F + (1 + ν)C4
                                         = (1 − αk )      −                   + νC4
                                                       ηk        1 − αk
                                         ≥ νC4 .
32                                                     LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


 where the last inequality follows from αk = α and η1k = L̄F +(1+ν)C
                                                              1−α
                                                                    4
                                                                      . Then, one has

                            α
          C4 − K6 = C4 −       − 2LF βk2
                            ηk
                       1
                            (1 − α)C4 − αL̄F − (1 + ν)C4 α − 2LF β 2 (1 − α)
                                                                             
                   =
                     1−α
                                                       2LF β 2               2LF β 2 α
                                                             
                       1
                   ≥         (1 − 2α − ν)C4 − α L̄F +             + νC4 +
                     1−α                                  α                    1−α
                   ≥ νC4 ,

                                                                           2L     β2
                                                                    L̄F + F
where the last inequality follows from α ∈ (0, 1−ν
                                                2 ) and C4 =
                                                                          α
                                                                      1−2α−ν           > 0. We rearrange η1k
                                                                 (1−2α−ν)/2
using the definition of C4 and obtain ηk = LηF where η = L̄F           (1+ν)β 2
                                                                                  .
                                                                 LF
                                                                      + (1−α)α
    Next, we verify that the coefficients of Ψm satisfying (5.13) and τ satisfying (5.14) ensure
that CB , Cx > 0. Let us denote t = τ LF . Choosing δ > 0, we have
                                                                 
                             LF 2 − 6δ             1
                      CB =                 − 3δ +       t − (6δ + 1) ≥ 0.
                              2       t            δ

If we choose δ = 16 > 0, then
                                 2       2 13
                                    CB = − t − 2 ≥ 0.
                               LF        t     2
                     √           i
By selecting τ ∈ 0, 6( 30−2    1
                        13 )δ LF , it is sufficient to ensures CB > 0, and consequently,
C2 , C1 > 0. Therefore, for some δ > 0, setting δ̃ = cδ δ 2 and ensuring τ 2 L2F ≤ δ̃ guarantee
positive coefficients for Ψm in (5.13) and CB , Cx > 0.
We then derive an upper error bound on dist2 (0, ∂L(xk , B k , Λk )) based on

(C.8)            dist2 (0, ∂L(xk , B k , Λk )) ≤ ∥dk ∥2 = ∥dkx ∥2 + ∥dkB ∥2 + ∥dkΛ ∥2 .

     Lemma C.3 (Stationarity error bound). Under Assumption 5.1, for any k ≥ 1, it holds that

                                                                                                      3σ 2
(C.9) E[dist2 (0, ∂L(xk , B k , Λk ))] ≤ S1 E[∆k+1         k      k    k+1    k    k
                                               B ] + S2 E[∆B ] + S3 E[∆x ] + S4 E[∆x ] +                   ,
                                                                                                     bk LF
where the coefficients are given by
                               
                               
                               
                                S1     = 4τ −2 + 12,
                                        = 4(1 + 3(1 + τ LF )2 ),
                               
                               S
                                   2
(C.10)
                               
                               
                                S k
                                   3    = 4(3ηk−2 + L2F + 3(τ LF )2 ),
                               S k     = 6(L2F βk2 + 6ηk−2 αk2 ).
                               
                                  4
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                                33


    Proof. For estimating ∥dkx ∥2 , using the optimality condition of xk , we obtain

          ∥dkx ∥2
         = ∥∇x F (xk , B k )∥2
                                          2
         = ηk−2 ηk ∇x F (xk , B k ) + 0
                                                            2
         = ηk−2 ηk ∇x F (xk , B k ) − Gk + y k − xk+1
                                         

                                                                                                      2
(C.11)   = ηk−2 ηk ∇x F (xk , B k ) − ∇x F (z k , B k ) + ∇x F (z k , B k ) − Gk + y k − xk + xk − xk+1

(C.12)   ≤ 3∥∇x F (xk , B k ) − ∇x F (z k , B k )∥2 + 3ηk−2 ∥xk+1 − xk + xk − y k ∥2 + 3∥ek ∥2
         ≤ 3L2F ∥xk − z k ∥2 + 6ηk−2 ∥xk+1 − xk ∥2 + ∥xk − y k ∥2 + 3∥ek ∥2 ,
                                                                     
(C.13)

where (C.11) is deduced by using the fact that (a + b + c)2 ≤ 3(a2 + b2 + c2 ) and (C.12) is
deduced by the smoothness of F (x, B) and (a+b)2 ≤ 2(a2 +b2 ). Then applying Proposition A.3
to (C.13) concludes

(C.14)                ∥dkx ∥2 ≤ 6(L2F βk2 + 2ηk−2 αk2 )∆kx + 12ηk−2 ∆k+1
                                                                     x   + 3∥ek ∥2 .

For estimating ∥dkB ∥2 , using the B-update (4.4) and the smoothness of F , we obtain

             ∥dkB ∥2 = ∥∇B F (xk+1 , B k ) + Λk + ∇B F (xk , B k ) − ∇B F (xk+1 , B k )∥2
(C.15)               ≤ 4τ −2 ∆k+1   2 k+1
                              B + 4LF ∆x ,

where the last inequality follows from the fact (a + b)2 ≤ 2(a2 + b2 ).
   For estimating ∥dkΛ ∥2 , using the Λ-update (4.5), we obtain

                           ∥dkΛ ∥2 ≤ 2τ 2 ∥Λk − Λk−1 ∥2 + 2∥B k − B k−1 ∥2 .

    Then, substituting (5.6) into the above inequality yields that

(C.16)              ∥dkΛ ∥2 ≤ 12∆k+1                   2  k            2 k+1
                                 B + 4(1 + 3(1 + τ LF ) )∆B + 12(τ LF ) ∆x .

   Finally, substituting (C.14), (C.15), and (C.16) into (C.8), and taking the full expectation
on both sides, yields (C.9), completing the proof.
    C.3. Proof of Lemma 5.7.
    Proof. From (C.9), we obtain

(C.17)
                                                                                                     3σ 2
  E[dist2 (0, ∂L(xk , B k , Λk ))] ≤ (max{S1 , S2 } + max{S3 , S4 })E[∆B
                                                                       k+1
                                                                           + ∆kB + ∆k+1
                                                                                    x   + ∆kx ] +         .
                                                                                                    bk LF

 Similarly, from (5.15), we obtain

                                                                                                  σ2
     E[Ψm (Qk+1 )] − E[Ψm (Qk )] ≤ − min{CB , Cx }E[∆k+1  k    k+1
                                                     B + ∆B + ∆x   + ∆kx ] +                           ,
                                                                                                 bk LF
34                                                       LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


which yields that
                                                                                     σ2
                                                                                         
                                              1
      E[∆k+1  k    k+1
         B + ∆B + ∆x   + ∆kx ] ≤                                 k            k+1
                                                          E[Ψm (Q )] − E[Ψm (Q )] +
                                         min{CB , Cx }                              bk LF
Substituting this inequality into (C.17), summing over k = 1, · · · , T , and dividing by T , we
obtain
       T                                       T
     1X                                     L∆ X                                        σ2
         E[dist2 (0, ∂L(xk , B k , Λk ))] ≤        E[Ψm (Qk )] − E[Ψm (Qk+1 )] + (3 + L∆ ) ,
     T                                      T                                             bk
       k=1                                       k=1

which yields the conclusion. Denoting ak = E[dist2 (0, ∂L(xk , B k , Λk ))], we have
                                    T
                                 1X      L∆ ∆1        L∆ σ 2
                                    ak ≤       + (3 +   ) .
                                 T        T           LF bk
                                   k=1

By uniformly selecting R′ ∈ {1, . . . , T } and letting R = R′ + 1, we obtain
                                                           L∆ ∆1        L∆ σ 2
                      E[dist2 (0, ∂L(xR , B R , ΛR ))] ≤         + (3 +   ) ,
                                                            T           LF bk
and complete the proof.
      C.4. Proof of Theorem 5.2.
      Proof. When bk = b = cb T , we have
                                                                         σ2
                                                                           
                            2            R   R    R  1
                       E[dist (0, ∂L(x , B , Λ ))] ≤          L∆ ∆1 + Cσ      .
                                                     T                   cb

With the conditions on T in (5.19), this gives E[dist2 (0, ∂L(xR , B R , ΛR ))] ≤ ε2 . Then, the
oracle complexity of SFO is
                          Complexity = (T − 1)bk = O(T 2 ) = O(ε−4 ).

   Appendix D. Complexity analysis of DualHash-StoRM (Algorithm 4.2). We adopt the
same notation as in Appendix C.
      D.1. Proof of Lemma 5.9.
      Proof. Substituting the one-iteration progress (5.20) and (5.9) into (5.7), we obtain

       L(xk+1 , B k+1 , Λk+1 ) − L(xk , B k , Λk )
       s            1
      ≤ ∥ek ∥2 − (− + 2ηk−1 − LF − K3 )∆k+1     x  + K1 ∆k+2     k+2
                                                         B + K4 ∆x   − K2 ∆k+1     k
                                                                           B + K3 ∆B ,
       2            s
where setting s = ηk yields

              L(xk+1 , B k+1 , Λk+1 ) − L(xk , B k , Λk )
              ηk
(D.1)        ≤ ∥ek ∥2 − (ηk−1 − LF − K3 )∆k+1  x     + K1 ∆k+2      k+2
                                                           B + K 4 ∆x   − K2 ∆k+1     k
                                                                              B + K3 ∆B .
               2
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                      35


Applying the definition of Ψr from (5.21) into (D.1), we rearrange the above inequality

            Ψr (Qk+1 ) − Ψr (Qk )
          ≤ − (C1 − K1 )∆k+2                    k+1             k           k+2
                          B − (K2 − C1 − C2 )∆B − (C2 − K3 )∆B − (C3 − K4 )∆x
                                                          ηk
(D.2)       − (ηk−1 − LF − K3 − C3 )∆k+1
                                     x   − (C4 − K6 )∆kx + ∥ek ∥2 .
                                                          2
By substituting the Lyapunov function coefficients (5.13) into (D.2), we obtain (5.22). With τ
satisfying (5.14), Lemma C.2 guarantees that CB > 0.
    We provide a recursive bound on the gradient error vector {ek } with analysis comparable
to those in [62, Lemma 2] and [49, Lemma 8].
     Lemma D.1 (Recursive bound on gradient error). Under Assumption 5.1, for any k ≥ 1, it
holds that
                                                          2ρ2 σ 2   4(1 − ρk )2 L2F        h     i
        Eξ[k+1] [∥ek+1 ∥2 ] ≤ (1 − ρk )2 Eξ[k] [∥ek ∥2 ] + k      +                 Eξ[k+1] ∆k+1
                                                                                              x    .
                                                          |Jk+1 |       |Jk+1 |

We now establish a weighted error accumulation bound T1 Tk=1 ρk Eξ[k] ∥ek ∥2 for the variance-
                                                                  P                     

reduced gradient estimator in Algorithm 4.2. For this analysis, we require the parameters ηk ,
ρk , τ and the batch size bk to satisfy:
                                                                           p              (
                                   1                                          δ̃            b1 k = 1,
(D.3) ηk+1 ≤ ηk , ηk L̃F ≤ , 0 < 8η1 ηk L2F ≤ ρk ≤ 1, τ ≤                        , bk =
                                   2                                       LF               b k ≥ 2.


    Lemma D.2 (Weighted error accumulation bound). Under Assumption 5.1 and parameter
conditions (D.3), for any T ≥ 1, the following weighted cumulative error bound holds:
                     T                i 16η L2 ˜             T
                   1X          h
                                           1 F ∆1   2σ 2   1 X 4ρ2k σ 2
(D.4)                  ρk Eξ[k] ∥ek ∥2 ≤          +      +              ,
                   T                        T       T b1 T        b
                      k=1                                             k=2

      ˜ 1 = Ψ1r − Ψ∗r , Ψ1r = Ψr (Q1 ) and Ψ∗r is the lower bound of Ψr .
where ∆
    Proof. Under (D.3), we have a lower bound for Cxk with
                                                   1
(D.5)                                     Cxk ≥       > 0.
                                                  2η1
Substituting this inequality into the second inequality of (5.23) and taking the conditional
expectation on both sides yields
                 h          i       h        i     1         h     i η
                                                                         k
                                                                                 h        i
(D.6)     Eξ[k+1] Ψr (Qk+1 ) − Eξ[k] Ψr (Qk ) ≤ −     Eξ[k+1] ∆k+1
                                                                x    +     E [k]   ∥e k 2
                                                                                       ∥    .
                                                  2η1                   2 ξ
From Lemma D.1, we obtain
               h        i                 h      i 2ρ2 σ 2                        h     i
        Eξ[k+1] ∥ek+1 ∥2 ≤ (1 − ρk ) Eξ[k] ∥ek ∥2 + k      + 4(1 − ρk )L2F Eξ[k+1] ∆k+1
                                                                                    x     ,
                                                   |Jk+1 |
36                                                             LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


where the inequality follows from 0 < ρk < 1 and |Jk+1 | ≥ 1, and this implies
                   h      i
         − Eξ[k+1] ∆k+1x

               1
                               h      i           h          i           h       i 2ρ2 σ 2 
                                   k 2                  k+1 2                 k 2
       ≤                 E ξ[k]  ∥e ∥    − Eξ[k+1]   ∥e    ∥    − ρ E
                                                                   k ξ[k]   ∥e ∥   + k        .
         4(1 − ρk )L2F                                                              |Jk+1 |
Substituting this inequality into (D.6), we obtain
    Eξ[k+1] Ψr (Qk+1 ) − Eξ[k] Ψr (Qk )
                                    

                                                             k 2  2ρ2k σ 2
                                                                            
           1                 k 2          k+1 2                             ηk
                                                                               + Eξ[k] ∥ek ∥2
                                                                                             
  ≤                    Eξ[k] ∥e ∥ − Eξ[k+1] ∥e  ∥ − ρk Eξ[k] ∥e ∥ +
    8(1 − ρk )η1 L2F                                                |Jk+1 |     2
(D.7)
                                                  2ρ2 σ 2
                                                                                       
      1                                                                       ρk     ηk
                  Eξ[k] ∥ek ∥2 − Eξ[k+1] ∥ek+1 ∥2 + k                                         Eξ[k] ∥ek ∥2 ,
                                                                                                      
  ≤                                                                 −              −
    8η1 L2F                                        |Jk+1 |                       2
                                                                            8η1 LF   2
where the second inequality follows from 1 − ρk ≤ 1. From (D.3), it follows that 8ηρkL2 − η2k ≥
                                                                                                               1   F
  ρk
16η1 L2F
         . Applying this inequality to (D.7) yields the following expected descent property of the
Lyapunov function Ψr
                                                                                     
                                             1                              1
           Eξ[k+1] Ψr (Qk+1 ) +                      k+1 2           k
                                                   ∥e ∥ − Eξ[k] Ψr (Q ) +           k 2
                                                                                  ∥e ∥
                                           8η1 L2F                        8η1 L2F
                        ρk              k 2         ρ2k σ 2
(D.8)         ≤−              E  [k] [∥e ∥  ] +                 .
                      16η1 L2F ξ                4η1 L2F |Jk+1 |
Finally, summing up (D.8) from k = 1 to T and dividing by T yields
                      T                             ˜ 1 2Eξ[1] [∥e1 ∥2 ]     T
                    1X              k 2    16η1 L2F ∆                      1 X 4ρ2k σ 2
                        ρk Eξ[k] [∥e ∥ ] ≤             +                 +              ,
                    T                          T              T            T   |Jk+1 |
                      k=1                                                                     k=2
yielding (D.4) and completes the proof.
       We derive an upper stationarity error bound for the iterates generated by Algorithm 4.2.
       Lemma D.3 (Stationarity error bound). Under Assumption 5.1 (ii), for any k ≥ 1, it holds
that
(D.9)               dist2 (0, ∂L(xk , B k , Λk )) ≤ S1 ∆k+1     k    k k+1
                                                        B + S2 ∆B + S3 ∆x  + 2∥ek ∥2 ,
where the coefficients are defined as (C.10).
     Proof. This proof follows a similar approach to that of Lemma C.3 in Appendix C. The key
difference lies in the estimation of ∥dkx ∥2 . To estimate ∥dkx ∥2 , we use the optimality condition
of xk+1 from (4.9) to obtain
                                                                             2
                             ∥dkx ∥2 = ηk−2 ηk ∇x F (xk , B k ) + 0
                                                                             2
                                     = ηk−2 ηk ∇x F (xk , B k ) − D k + ηk D k
                                                                            2
                                     = ηk−2 ηk ek + (xk − xk+1 )
(D.10)                               ≤ 4ηk−2 ∆k+1
                                              x   + 2∥ek ∥2 .
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                                          37


Then, substituting (D.10), (C.15), and (C.16) into (C.8) yields (D.9) and completes the proof.
    Selecting appropriate parameters is crucial as the ultimate oracle complexities depend on
these choices.
   D.2. Proof of averaged stationarity error bound for STORM. We now establish the
convergence rate of Algorithm 4.2 by taking constant parameters, i.e., ηk ≡ η1 and ρk ≡ ρ1 for
k ≥ 1. The first result is to estimate the averaged expected stationarity error.
    Lemma D.4. Under the conditions of Lemma D.2 , it holds that for any T ≥ 1,
                                                                                                   
       T                                     ˜                1          (6 + C 1 + S̃4 η1 )16η L2
    1 X                                      ∆1  S̃4   2(Cδ + 4)               δ   2CB        1 F
          E[dist2 (0, ∂L(xk , B k , Λk ))] ≤          +              +                              
   T                                         T    CB          η1                    ρ1
      k=1
                                                                  !
                                                                      2σ 2     4ρ1 σ 2 (T − 1)
                                                                                                
                                                      1  S̃ 4 η 1
(D.11)                                       + 6 + Cδ +                      +                    ,
                                                         2CB         b1 ρ1 T          bT

where S̃4 = max{S1 , S2 } and Cδ1 = 1 + L̃32δ̃ .
                                                      F

    Proof. Applying ηk ≤ η1 from (D.3) to (D.9) in Lemma D.3 yields
                                       S4                                         S4 η1
(D.12) dist2 (0, ∂L(xk , B k , Λk )) ≤      Ψr (Qk ) − Ψr (Qk+1 ) + S3k ∆k+1
                                                                         x   + (2 +       )∥ek ∥2 .
                                       CB                                           2CB
For the second term in (D.12), by recalling the second inequality in (5.23), we obtain
                                1                           ηk k 2 
                      ∆k+1
                        x   ≤       Ψ r (Qk
                                            ) − Ψr (Qk+1
                                                         ) +   ∥e ∥ ,
                               Cxk                           2

where we use the positive value of Cxk from (D.5). Therefore, substituting the inequality above
and the definition of S3k from (C.10), into S3k ∆k+1
                                                 x , one has

           S3k ∆k+1
                x
            S k                          ηk       
          ≤ 3k Ψr (Qk ) − Ψr (Qk+1 ) + ∥ek ∥2
            Cx                             2
                           2       2
            4[1 + (1 + 3τ )(ηk LF ) ]                         ηk      
          ≤                             Ψr (Qk ) − Ψr (Qk+1 ) + ∥ek ∥2
                  ηk (1 − ηk L̃F )                             2
              4[1 + (1 + L32δ̃ )(ηk LF )2 ]                                  ηk k 2 
          ≤                    F
                                                  Ψr (Qk ) − Ψr (Qk+1 ) +       ∥e ∥
                    ηk (1 − ηk L̃F )                                          2
              4[1 + (1 + L̃32δ̃ )(ηk L̃F )2 ]                              2[1 + (1 + L̃32δ̃ )(ηk L̃F )2 ]
                                                          k         k+1
          ≤                    F
                                                  Ψr (Q ) − Ψr (Q         ) +              F
                                                                                                             ∥ek ∥2
                   η1 (1 − ηk L̃F )                                                 1 − ηk L̃F
              4[1 + C (η L̃ )2 ] 
                       δ   k   F
                                                       2[1 + C (η L̃ )2 ]
                                                                  δ k F
          ≤                      Ψr (Qk ) − Ψr (Qk+1 ) +                   ∥ek ∥2
              η1 (1 − ηk L̃F )                              1 − ηk L̃F
            2(Cδ + 4)                        
(D.13)    ≤              Ψr (Qk ) − Ψr (Qk+1 ) + (Cδ + 4)∥ek ∥2 ,
                η1
38                                                       LUXUAN LI, XIAO WANG, AND CHUNFENG CUI


where the third inequality follows from τ 2 ≤ δ̃/L2F , the fourth inequality is derived from
L̃F ≥ LF and ηk ≡ η1 , and the last inequality holds because (1 + cx2 )/(1 − x) ≤ 2 + 2c for
0 < x ≤ 1/2 and constant c > 1 under parameter conditions (D.3).
    On the other hand, by taking the conditional expectation on both sides of (D.12) and
averaging over T , we obtain
                  T
                1X       h                             i
                    Eξ[k] dist2 (0, ∂L(xk , B k , Λk ))
                T
                  k=1
                       T                                           T
                S4 X           h
                                        k
                                          i           h
                                                         k+1
                                                             i 1 X            h     i
             ≤             Eξ[k] Ψr (Q ) − Eξ[k+1] Ψr (Q ) +          S3k Eξ[k] ∆k+1
                                                                                  x
               CB T                                             T
                      k=1                                         k=1
                      T             
                  1X           S4 η1        h       i
               +          2+           Eξ[k] ∥ek ∥2
                 T             2CB
                    k=1
               S4 ˜         T                                   T
               CB ∆ 1
                                                  i         
                          1X k            h
                                              k+1       S4 η1 1 X      h        i
(D.14)       ≤         +        S3 Eξ[k+1] ∆x      + 2+           Eξ[k] ∥ek ∥2 .
                 T       T                              2CB T
                              k=1                                           k=1

     Then substituting (D.13) into (D.14) yields
                        T
                    1X
                           Eξ[k] [dist2 (0, ∂L(xk , B k , Λk ))]
                    T
                       k=1
                                     
                      S4    2(Cδ +4) ˜                              T
                      CB +    S4 η1      ∆1         S4 η1
                                                                  X
                                                                  1
(D.15)            ≤                          + 6+           + Cδ      Eξ[k] [∥ek ∥2 ].
                              T                      2CB          T
                                                                      k=1

     Since ρk ≡ ρ1 for k ≥ 1, then it follows from (D.4) that
                       T                            ˜1
                     1X                    16η1 L2F ∆    2σ 2   4ρ1 σ 2 (T − 1)
(D.16)                   Eξ[k] [∥ek ∥2 ] ≤             +      +
                     T                        ρ1 T       b1 T          bT
                        k=1

Then, substituting (D.16) into (D.15) and taking the full expectation on both sides yields
(D.11).
     D.3. Proof of Lemma 5.10.
    Proof. Substituting the parameter conditions (5.24) into (D.11) from Lemma D.4, and
                                 2
applying the bound ρk ≡ T8ρη   2/3 , b1 = cb T
                                               1/3 , b = b for k ≥ 2, we obtain (5.25) by selecting R
                                                      k
uniformly from {2, . . . , T + 1}.
With this parameter choice, we proceed to analyze the oracle complexity of Algorithm 4.2.
     D.4. Proof of Theorem 5.3.
     Proof. When b1 = cb T 1/3 , we have

                                                                       σ2       32ρη 2 σ 2
                                                                                         
                                                1        ˜
           E[dist2 (0, ∂L(xR , B R , ΛR )] ≤          L̃ ∆
                                                        ∆ 1 + C̃ σ            +               .
                                               T 2/3                 4ρη 2 cb      b
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                       39


With the conditions on T in (5.26), this gives E[dist2 (0, ∂L(xR , B R , ΛR ))] ≤ ε2 . Then, the
oracle complexity is
                                                               1
               Complexity = b1 + (T − 1)b = O(T 3 ) + O(T ) = O(T ) = O(ε−3 ),

which completes the proof.
    Experimental Appendix.
    This supplementary material provides detailed experimental settings and additional results.
    Appendix E. Experiment Details.
      E.1. Deep Pairwise Supervised Hashing Formulation. We consider deep supervised
pairwise hashing in our experiments, which aims to learn binary codes that minimize Hamming
distances for similar pairs and maximize them for dissimilar pairs. Let A = {ai ∈ Rda : i =
1, . . . , n} be the training set with labels Y = {yi ∈ {0, 1}C } (one-hot for single-label, multi-hot
for multi-label). Key notations are in Table E.1.
      Network Architecture. A CNN extracts features from ai , followed by a fully connected hash
layer producing d-dimensional outputs C(x; ai ), which are passed through tanh to yield ui .
      Loss Function. For hash codes in {−1, 1}d , the Hamming distance is dhij = 12 (d − ⟨hi , hj ⟩).
Following [61, 33], similarity soij = 1 if samples share labels, else 0. The likelihood is:
                                                           o                       o
                      P (soij | hi , hj ) = σ(⟨hi , hj ⟩)sij (1 − σ(⟨hi , hj ⟩))1−sij ,

where σ(x) = (1 + exp(−αx))−1 , α < 1. Using continuous codes ui and shij = 12 ⟨ui , uj ⟩, the
loss becomes:
                                    1 X                         
(E.1)               min LS (x) = −         log 1 + exp −αsoij shij .
                     x             |S| o
                                                sij ∈S


    We use the W-type regularization:
                                       n
                                       X
(E.2)                        h(Z) =          h(zi ),     h(z) = λ∥ |z| − 1 ∥1 ,
                                       i=1

where λ > 0 controls regularization strength.
   Combining (E.1) and (E.2) into (P) gives the final loss of DualHash in our experiments.
                                          1 X                        
                          min        −         log 1 + exp −αsoij shij
                      x∈X ,B∈Rn×d        |S| o
                                              sij ∈S
                                               n                    n
                                          γ X                X
                                     +        ∥bi − ui ∥22 +   λ∥ |bi | − 1 ∥1 .
                                         2n
                                              i=1                  i=1

    Out-of-sample Extension. For an unseen query aq ∈
                                                    / A, its binary code is predicted as:

                                          bq = sgn(C(x; aq )).
 40                                                    LUXUAN LI, XIAO WANG, AND CHUNFENG CUI



                             Table E.1: Summary of symbols and notations.

                      Symbol                  Description
                      ai , yi                 Input feature and label vector for
                                              i-th sample
                      soij , shij             Similarity in input space and
                                              Hamming space
                      doij , dhij             Distance in input space and
                                              Hamming space
                      C(x; ai )               Network output before activation
                      bi ∈ {−1, 1}d           Binary hash code for i-th sample
                      ui = tanh(C(x; ai ))    Continuous code after activation
                      n, d                    Number of samples and hash
                                              code length
                      x                       Network parameters
                      da , dx                 Dimension of input feature and
                                              parameters


       E.2. Implementation Details.
       Baseline methods. We compare our method with eight baseline approaches, including one
  traditional discrete hashing method and seven deep supervised hashing methods:
    (i) SDH [48]: A pointwise supervised hashing method using hand-crafted features. It employs
        discrete cyclic coordinate descent to directly optimize binary codes.
   (ii) DHN [71]: A Bayesian framework thatP     minimizes pairwise cross-entropy loss and quanti-
        zation error using tanh relaxation and i log(cosh(|zi | − 1)) regularization.
  (iii) DSH [39]: Uses max-min pairwise loss with ∥|z| − 1∥1 regularization to minimize quanti-
        zation error.
  (iv) DTSH [57]: Learns hash codes by maximizing triplet likelihood with negative log-triplet
        loss, using ∥|z| − 1∥22 quantization regularization and a positive margin α to accelerate
        training and normalize Hamming distance gaps.
   (v) HashNet [8]: Progressively increases β in tanh(βx) to approximate the sgn function,
        addressing nonconvex optimization challenges.
  (vi) DSDH [36]: Integrates pairwise labels and classification information using alternating
        minimization to learn features and binary codes with the sgn function.
 (vii) OrthoHash [28]: Uses a single cosine similarity loss to maximize alignment between
        continuous codes and binary orthogonal codes, ensuring discriminativeness with minimal
        quantization error.
(viii) MDSHC [55]: A two-stage method that first generates hash centers with minimal
        Hamming distance constraints, then uses three-part losses with ∥|z| − 1∥1 regularization
        to reduce quantization error.
       Evaluation Protocols. Following [39], we adopt four evaluation metrics:
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                             41


 (i) Mean Average Precision (mAP): For a query xq , the Average Precision is computed as:
                                                 PN
                                                     k=1 P (k) · Irel (k)
                                     AP(xq ) =                              ,
                                                      min(N, Rq )

      where P (k) is precision at rank k, Irel (k) indicates whether the k-th result is relevant, Rq
      is the total relevant samples, and N is the number of retrieved items. mAP averages AP
      over all queries. For CIFAR-10, we use all returned neighbors; for NUS-WIDE, we consider
      top-5000 results.
 (ii) Precision at Top K (P@topK): The proportion of true neighbors among top-K retrieved
      results, averaged over all queries.
(iii) Precision within Hamming Radius 2 (P@r=2): Measures retrieval accuracy for items within
      Hamming distance 2 from the query. This metric is particularly efficient as Hamming
      radius search has O(1) time complexity per query.
(iv) Precision-recall curves of Hamming ranking: PR curves illustrate precision at various
      levels of recall, offering insights into the trade-off between precision and recall at different
      thresholds. These curves are widely recognized as effective indicators of the robustness
      and overall performance of different algorithms.
    Appendix F. Additional Experimental Results.
    F.1. Comparison of Optimization Algorithms with Different W-Type Regularizations.
To validate the effectiveness of our primal-dual framework, we compare DualHash-StoM
against optimization algorithms with different W-type regularizations. Table F.1 summarizes
the configurations of different regularization functions and their corresponding optimization
approaches. For StoMHash-WCR, we employ the regularization λ∥z 2 − 1∥1 following the
iSPALM framework [45]. The proximal operator is computed with parameter ρ > 0 under the
condition ηρ > 21 , which ensures the subproblem is strongly convex with a unique solution.


Table F.1: Configurations of different W-type regularizations and optimization algorithms for DualHash-
StoM and three baselines.

    Method                          h(z)                 Convexity              Algorithm   Complexity
    DSH [39]                    ∥|z| − 1∥1             Nonconvex                 SGDM           –
                                   2
    StoMHash-WCR           P ∥z − 1∥1                 Weakly convex              SPGD           –
    DHN [71]                i log(cosh(|zi | − 1))     Nonconvex                 SGDM           –
    DualHash-StoM               ∥|z| − 1∥1             Nonconvex                  PD            ✓
   Notes: Algorithm describes the optimization strategy: SGDM (stochastic subgradient descent with
   momentum), SPGD (stochastic proximal gradient descent), PD (primal-dual). Complexity reports
   oracle complexity; ‘–’ indicates no analysis available.

    Training performance analysis. Figures F.1 and F.2 present training dynamics across different
optimization approaches, comparing training loss, P@top1000, and P@r=2 metrics for 64-
bit codes on CIFAR-10 and NUS-WIDE. On CIFAR-10, DualHash-StoM demonstrates
strong optimization efficiency across all metrics. Our method achieves fast convergence in
training loss while consistently maintaining high P@r=2 and P@top1000 values throughout
42                                                  LUXUAN LI, XIAO WANG, AND CHUNFENG CUI




Figure F.1: Training performance comparison of DualHash-StoM and three baselines with 64-bit codes
on the CIFAR-10 dataset.




Figure F.2: Training performance comparison of DualHash-StoM and three baselines with 64-bit codes
on the NUS-WIDE dataset.



the optimization process. While StoMHash with weakly convex regularization (StoMHash-
WCR) shows competitive performance, DualHash-StoM achieves faster convergence and higher
retrieval performance, demonstrating the effectiveness of our primal-dual transformation for
handling W-type regularizations. On the more challenging NUS-WIDE dataset with complex
multi-label scenarios, DualHash-StoM maintains competitive retrieval performance despite
slightly slower initial convergence. The experimental results on diverse datasets indicate that
utilizing nonconvex nonsmooth regularization with specialized primal-dual optimization can
improve hash code learning and retrieval performance in practical applications.
    Retrieval performance in Hamming space. We report mAP, P@r=2, and the average time per
training epoch on CIFAR-10 and NUS-WIDE, all with 64-bit codes, for different methods in
Table F.2. The results highlight the superior retrieval accuracy of DualHash-StoM compared
to the baseline methods.
    Specifically, on the CIFAR-10 dataset, DualHash-StoM achieves 9.9% and 27.2% im-
DUALHASH: A STOCHASTIC PRIMAL-DUAL ALGORITHM                                                           43



Table F.2: Retrieval performance comparison of DualHash-StoM and three baselines with 64-bit codes
on CIFAR-10 and NUS-WIDE datasets.

                                           CIFAR-10                                NUS-WIDE
       Methods
                               mAP@All         P@r=2       Time*       mAP@5000       P@r=2    Time*
       DSH [39]                  0.8010        0.4401       7.38         0.6634       0.4004   21.14
       DHN [71]                  0.8051        0.5272       6.74         0.7042       0.5432   16.47
       StoMHash-WCR              0.7773        0.5402       7.04         0.7225       0.5857   16.69
       DualHash-StoM             0.8539        0.6872       6.86         0.7448       0.6548   16.83
   *
       Training time per epoch (seconds). Bold values indicate the best results.


provements over StoMHash-WCR in mAP@All and P@r=2, respectively. Similarly, on the
NUS-WIDE dataset, DualHash-StoM outperforms StoMHash-WCR with a 3.1% improvement
in mAP@5000 and an 11.8% improvement in P@r=2. Computationally, DualHash-StoM shows
comparable efficiency to baselines such as DHN (e.g., 6.86s vs. 6.74s on CIFAR-10 and 16.83s
vs. 16.47s on NUS-WIDE). This marginal time increase is justified by the substantial perfor-
mance gains. Furthermore, Figure F.3 displays P@topK results and PR curves with 64-bit
codes on CIFAR-10 and NUS-WIDE. DualHash-StoM consistently outperforms all comparison
methods by large margins in P@topK. For PR curves, DualHash-StoM demonstrates superior
performance at lower recall levels, making it especially well-suited for precision-critical image
retrieval systems.




Figure F.3: Retrieval performance comparison of DualHash-StoM and three baselines with 64-bit codes
on CIFAR-10 and NUS-WIDE datasets. (a), (c): P@topK results; (b), (d): PR curves.
