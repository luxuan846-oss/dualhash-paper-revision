# Smoothing Meets Perturbation: Unified and Tight Analysis for Nonconvex-Concave Minimax Optimization

- **Authors:** Jiajin Li; Mahesh Nagarajan; Siyu Pan; Nanxi Zhang
- **Date:** April 30, 2026
- **arXiv:** [2602.14185](https://arxiv.org/abs/2602.14185)
- **DOI:** [10.48550/arXiv.2602.14185](https://doi.org/10.48550/arXiv.2602.14185)
- **Zotero item key:** 8MZKMW3B
- **Zotero attachment key:** ND299347

## Abstract

This paper studies smooth nonconvex-concave minimax optimization and two acceleration mechanisms for single-loop first-order methods: dual perturbation and smoothing. Although both techniques improve convergence guarantees, their relative advantages remain unclear due to the distinction between game stationarity (GS) and optimization stationarity (OS). We provide a tight characterization of their iteration complexities under both notions. We show that smoothing accelerates convergence to both GS and OS, whereas dual perturbation improves the rate only for GS and does not accelerate OS. Matching lower bounds based on hard instances establish the tightness of these rates. Motivated by this separation, we propose Perturbed Smoothed GDA, a single-loop method combining both techniques. It improves the complexity for GS over existing single-loop methods while preserving the state-of-the-art rate for OS, and further admits asymptotic convergence to 0-GS, which is not available for vanilla Smoothed GDA.

## Full text

Smoothing Meets Perturbation: Unified and Tight Analysis for
Nonconvex-Concave Minimax Optimization
Jiajin Li,∗ Mahesh Nagarajan,† Siyu Pan,‡ Nanxi Zhang§¶
April 30, 2026
Abstract
This paper studies smooth nonconvex-concave minimax optimization and two acceleration mechanisms for single-loop first-order methods: dual perturbation and smoothing. Although both techniques improve convergence guarantees, their relative advantages remain unclear due to the distinction between game stationarity (GS) and optimization stationarity (OS). We provide a tight characterization of their iteration complexities under both notions. We show that smoothing accelerates convergence to both GS and OS, whereas dual perturbation improves the rate only for GS and does not accelerate OS. Matching lower bounds based on hard instances establish the tightness of these rates. Motivated by this separation, we propose Perturbed Smoothed GDA, a single-loop method combining both techniques. It improves the complexity for GS over existing single-loop methods while preserving the state-of-the-art rate for OS, and further admits asymptotic convergence to 0-GS, which is not available for vanilla Smoothed GDA.
1 Introduction
In this paper, we consider the smooth nonconvex–concave (NC-C) minimax problem
min
x∈X max
y∈Y f (x, y), (1.1)
where f : Rn × Rd → R is l-smooth with respect to both x and y, X ⊆ Rn is a nonempty, closed convex set, and Y ⊆ Rd is a nonempty, compact, convex set. Problem (1.1) has received increasing attention due to its extensive applications in machine learning and operations research, including training generative adversarial networks (Arjovsky et al., 2017; Goodfellow et al., 2020), robust training of deep neural networks (Sinha et al., 2018), multi-agent reinforcement learning (Omidshafiei et al., 2017; Dai et al., 2018) and game theory (e.g., finding first-order Nash equilibria (FNE), see (Nouiehed et al., 2019; Ostrovskii et al., 2021)). Motivated by the computational efficiency and the needs of large-scale learning, we primarily focus on single-loop first-order methods. The simplest algorithm of this class is gradient descent–ascent (GDA). In GDA, each iteration consists of a gradient-descent step on the minimization variable x and a gradient-ascent step on the maximization variable y. Although vanilla GDA may fail to converge in the general smooth NC-C setting, Lin et al. (2020a) show that a two-timescale variant (TS-GDA)—with a substantially larger
∗Sauder School of Business, University of British Columbia, Vancouver BC, Canada. jiajin.li@sauder.ubc.ca †Sauder School of Business, University of British Columbia, Vancouver BC, Canada. mahesh.nagarajan@sauder.ubc.ca ‡Sauder School of Business, University of British Columbia, Vancouver BC, Canada. siyu.pan@sauder.ubc.ca §Ivey Business School, University of Western Ontario, London ON, Canada. nzhang@ivey.ca ¶Authors are listed in alphabetical order.
1
arXiv:2602.14185v2 [math.OC] 29 Apr 2026

step-size for ascent than for descent—restores convergence guarantees. However, this guarantee comes with a slow convergence rate: TS-GDA requires O(ε−6) iterations to reach both ε-game stationary (ε-GS) and ε-optimization stationary (ε-OS) points (cf. (Lin et al., 2020b, Definition A.5), (Li et al., 2025, Definition 3.1)). The slow convergence of TS-GDA stems from the fact that it essentially behaves as an inexact subgradient method applied to the nonsmooth value function Φ(x) := maxy∈Y f (x, y) + ιX (x), where ιX (·) is an indicator function that takes value 0 if x ∈ X and +∞ otherwise. To improve the convergence rate, two acceleration techniques have recently been proposed to overcome this nonsmooth barrier. The first is Smoothing via the Moreau envelope: The Moreau envelope of Φ provides a smooth surrogate with Lipschitz-continuous gradients (Davis and Drusvyatskiy, 2019). Building on this idea, Zhang et al. (2020) propose the Smoothed GDA method, which replaces the original objective with a regularized surrogate f (x, y) + r
2 ∥x − z∥2 (r > l), where z serves as an approximate proximal point of x with respect to the value function Φ. Rather than solving the proximal subproblem exactly, Zhang et al. (2020) perform a single GDA update on (x, y, z) in each iteration, leading to an improved iteration complexity of O(ε−4) for finding an ε-GS point (Zhang et al., 2020). A recent work (Li et al., 2025) further shows that Smoothed GDA also attains an ε-OS point in O(ε−4) iterations. The second acceleration technique is dual perturbation. This approach adds a small quadratic regularizer to the dual objective, thereby making it strongly concave. Such a perturbation is closely related to the regularization technique of (Nesterov, 2013). By the envelope theorem, this modification also yields a smooth value function maxy∈Y f (·, y) − ε
2 ∥y∥2. Moreover, due to the compactness of Y, any ε-GS of the perturbed problem is also a Θ(ε)-GS point of the original problem. Applying GDA to this perturbed formulation yields the Perturbed GDA algorithm, which attains an iteration complexity of O(ε−4) for finding an ε-GS solution (Xu et al., 2023; Lu et al., 2020; Xu et al., 2026). However, in the existing literature, direct comparisons of iteration complexities between dual perturbation and smoothing can be misleading, as the literature often fails to clearly distinguish between two distinct notions of stationarity: game stationarity and optimization stationarity. This oversight has fostered the inaccurate impression that both smoothing and dual perturbation yield the same O(ε−4) rate for smooth problems under single-loop first-order methods. In reality, Li et al. (2025) clarifies that Smoothed GDA achieves both ε-GS and ε-OS within O(ε−4) iterations, and the dual-perturbation schemes developed in (Xu et al., 2023; Lu et al., 2020) establish an O(ε−4) rate only for ε-GS. To the best of our knowledge, the iteration complexity of Perturbed GDA for attaining an ε-OS point has not been characterized in the existing literature.
Our contributions. This distinction between GS and OS is more than a notational nuance; GS and OS impose intrinsically different complexity requirements, as already evidenced by their different complexity in the multi-loop setting. Lin et al. (2020b) show that multi-loop first-order methods attain an ε-GS solution with iteration complexity O(ε−2.5 log2( 1
ε )), whereas attaining an ε-OS solution requires O(ε−3 log2( 1
ε )) iterations. This disparity in complexity shows that OS and GS are distinct for smooth NC-C minimax problems, which raises the following question for single-loop first-order methods:
(Q1) For single-loop first-order algorithms, is there a complexity gap between the iteration complexity of finding an ε-GS and ε-OS?
In this paper, we provide a precise characterization addressing (Q1). We first show that for Perturbed GDA, although it achieves an iteration complexity of O(ε−4) for computing an ε-GS, its complexity degrades to O(ε−6) for attaining an ε-OS. We further prove that these bounds are tight by constructing matching hard instances on which the algorithm indeed requires this many iterations. This establishes a tight separation between GS and OS for Perturbed GDA. In contrast, Smoothed GDA achieves the same iteration complexity under the two stationarity notions, attaining both ε-GS and ε-OS within O(ε−4) iterations. We also establish
2

matching lower bounds, certifying that these rates are tight. These results lead to the following conceptual picture of dual perturbation and smoothing: Dual perturbation achieves an accelerated rate under the GS notion, but its complexity deteriorates under the OS notion. In contrast, smoothing attains the same accelerated rate for both GS and OS stationarity. This contrast implies that the two techniques accelerate the game stationarity through distinct mechanisms, and raises the possibility of a synergy effect of the two techniques. This motivates the second question we tackle in this paper.
(Q2) Can combining dual perturbation and smoothing improve the iteration complexity beyond either technique alone?
To answer (Q2), we introduce Perturbed Smoothed GDA, a single-loop algorithm that uses both smoothing and dual perturbation techniques. Our analysis shows that this algorithm achieves an ε-OS in O(ε−4) iterations, and this rate is tight under our assumptions. This result confirms the earlier insight that dual perturbation does not improve the iteration complexity to ε-OS as the iteration complexity remains identical to that of Smoothed GDA. On the other hand, our Perturbed Smoothed GDA achieves an ε-GS in O(ε−3) iterations, a strict improvement over both Smoothed GDA and Perturbed GDA, which each requires O(ε−4) iterations. This result gives a positive answer to (Q2) and shows that smoothing and perturbation can be synergistic in accelerating convergence to game stationarity.
Single Loop Algorithms Optimization Stationarity Game Stationarity
Upper Bound Lower Bound Upper Bound Lower Bound
TS-GDA
(Lin et al., 2020a; Boţ and Böhm, 2023; Mahdavinia et al., 2022) O l3L2D2
Y ∆Φ
ε6 Ω l3L2D2
Y ∆Φ
ε6 O l3L2D2
Y ∆Φ
ε6 Ω l3L2D2
Y ∆Φ ε6
Smoothed GDA
(Zhang et al., 2020; Li et al., 2025) O l3D2
Y ∆Ψ2
ε4 Ω l3D2
Y ∆Ψ2
ε4 O l3D2
Y ∆Ψ2
ε4 Ω l3D2
Y ∆Ψ2 ε4
Perturbed GDA
(Xu et al., 2023; Lu et al., 2020) O l5D4
Y ∆Ψ1
ε6 Ω l5D4
Y ∆Ψ1
ε6 O l3D2
Y ∆Ψ1
ε4 Ω l3D2
Y ∆Ψ1 ε4
Perturbed Smoothed GDA
(this work) O l3D2
Y ∆Ψ2
ε4 Ω l3D2
Y ∆Ψ2
ε4 O l2DY ∆Ψ2
ε3 Ω l2DY ∆Ψ2
ε3
Table 1: Gray-shaded entries denote results newly developed in this paper. Here, L is the Lipschitz constant of f ; DY stands for the diameter of Y; ∆Φ, ∆Ψ1, and ∆Ψ2 measure the initialization of the algorithm, and their formal definitions are given in Definition 3.1.
This iteration complexity difference can be explained by the structural relation between ε-GS and ε-OS. It is known that ε-OS implies Θ(ε)-GS, but not conversely, see, e.g., (Li et al., 2025, Theorem 7.1); thus, OS is a strictly stronger criterion for convergence. Correspondingly, smoothing and dual perturbation act on different components of the problem. Smoothing modifies the value function and directly targets OS, which automatically secures GS guarantees. In contrast, dual perturbation regularizes the inner maximization objective and primarily facilitates the convergence to GS, but the lack of a reverse implication (ε-GS ̸⇒ε-OS) prevents this benefit from extending to OS. Therefore, the two techniques operate through distinct yet complementary mechanisms: Smoothing promotes convergence to OS (and by extension GS), whereas dual perturbation mainly accelerates convergence to GS by rendering the inner maximization (dual) objective strongly concave. When combined, these effects yield a strictly better complexity for finding an ε-GS point than either technique alone.
3

Beyond the improvement in iteration complexity, the perturbation component in Perturbed Smoothed GDA also enables us to establish an asymptotic convergence result, i.e., we show that Perturbed Smoothed GDA converges to 0-GS when its iteration number goes to infinity. Such an asymptotic convergence result is not available for Smoothed GDA under the current analysis. Importantly, the smoothing-perturbation synergy is structural and is not restricted to single-loop methods. Building on the same principle, we propose a double-loop first-order algorithm, Perturbed Smoothed FOAM, that combines dual perturbation and smoothing within a multi-loop framework. This combination transforms the original NC-C problem into a sequence of strongly convex-strongly concave (SC-SC) subproblems, thereby enabling the use of optimal first-order methods within each iteration. In particular, we employ FOAM (Kovalev and Gasnikov, 2022) as an efficient inner solver, although the improvement does not rely on a specific choice of solver. Our algorithm achieves iteration complexities O(ε−2.5 log( 1
ε )) for ε-GS and O(ε−3 log( 1
ε )) for ε-OS. In comparison, most existing multi-loop methods either attain weaker guarantees or rely on more involved triple-loop constructions to obtain their rates (Nouiehed et al., 2019; Thekumparampil et al., 2019; Lin et al., 2020b; Kong and Monteiro, 2021; Zhao, 2024). A recent double-loop method proposed in (Lu and Mei, 2026) achieves the rate O(ε−2.5 log( 1
ε )) for ε-GS, but does not provide a corresponding guarantee for ε-OS. Thus, Perturbed Smoothed FOAM matches the best-known complexity for ε-GS while improving the state-of-the-art complexity for ε-OS among first-order methods (Zhang and Hu, 2025). Finally, on the technical side, we develop a unified Lyapunov-function-based framework that subsumes the analysis of existing single-loop methods, including Smoothed GDA and Perturbed GDA, as well as our proposed Perturbed Smoothed GDA. The analysis for Perturbed Smoothed GDA avoids the cumbersome two-stage arguments for Smoothed GDA used in prior works such as (Zhang et al., 2020; Li et al., 2025). As a result, we are able to prove an asymptotic convergence result for Perturbed Smoothed GDA. Furthermore, the framework extends naturally to the double-loop setting and applies to our proposed Perturbed Smoothed FOAM. Overall, this yields a streamlined proof structure that not only simplifies the analysis but explicitly disentangles the distinct roles of smoothing and perturbation in the convergence dynamics.
Notation. We use bold lowercase letters (e.g., x, y) to denote vectors, and calligraphic uppercase letters (e.g., X , Y) to denote sets. For a closed and convex set X ⊆ Rn, the indicator function ιX : Rn → {0, +∞} is defined by ιX (x) = 0 if x ∈ X and +∞ otherwise. We write DS := supx,y∈S ∥x − y∥ for the diameter of S, and projX (z) for the projection of a point z onto X . Given r > 0, the proximal operator of a function (if
well-defined) f : Rn → R at a point z is defined as proxr·f (z) := argminx∈X f (x) + 1
2r ∥x − z∥2. We use dist(x, S) := infz∈S ∥x − z∥ to denote the distance from x to set S. For a positive integer T , we use the shorthand notation [T ] := {1, . . . , T } to denote the index set of the first T positive integers. Throughout this paper, ε ∈ (0, 1) denotes a sufficiently small positive number. We write ak ↘ 0 to denote that ak is a non-increasing sequence converging to 0.
Structure of the paper. The paper is organized as follows. In Section 2, we introduce the preliminaries, including assumptions, stationarity notions, and the two acceleration tools: dual perturbation and smoothing. The main body of the paper focuses on single-loop methods: In Section 3, we present the main results, including the proposed Perturbed Smoothed GDA together with all iteration-complexity bounds and their tight analyses. In Section 4, we develop a unified convergence analysis framework for these single-loop algorithms. In Section 5, we provide a tight analysis that sharpens these iteration complexities. In Section 6, we further show that the two acceleration techniques and the unified analysis from Section 4 also apply to a double-loop setting, resulting in a new algorithm, Perturbed Smoothed FOAM, improving the best known iteration complexities. Finally, Section 7 concludes the paper.
4

2 Preliminaries
In this section, we collect the main assumptions and definitions used in the analysis. We begin with a smoothness assumption on the objective function f and then describe the properties required of the dual function. These assumptions are imposed throughout the paper.
Assumption 2.1 (l-smooth). The function f is continuously differentiable and there exists a positive constant l > 0 such that for all x, x′ ∈ X and y, y′ ∈ Y, we have
∥∇xf (x, y) − ∇xf (x′, y′)∥ ≤ l(∥x − x′∥ + ∥y − y′∥), and
∥∇yf (x, y) − ∇yf (x′, y′)∥ ≤ l(∥x − x′∥ + ∥y − y′∥).
Assumption 2.2. For each x ∈ X , the dual function f (x, ·) is concave. The dual domain Y ⊆ Rd is convex and bounded with diameter DY > 0. Without loss of generality (WLOG), we assume that 0 ∈ Y.
Our goal is to find approximate stationary points of problem (1.1). We introduce two standard stationarity measures (cf. (Li et al., 2025, Definition 3.1), (Lin et al., 2020b, Definition A.5)) that are used throughout the rest of the paper.
Definition 2.1 (Stationarity points). Let ε > 0 be given.
(i) The point x ∈ X is an ε-optimization-stationary point (ε-OS) of problem (1.1) if
prox 1
2l Φ(x) − x ≤ ε
2l ,
where Φ : Rn → R ∪ {+∞} is given by Φ(x) := maxy′∈Y f (x, y′) + ιX (x).
(ii) The pair (x, y) ∈ X × Y is an ε-game-stationary point (ε-GS) of problem (1.1) if
dist(0, ∇xf (x, y) + ∂ιX (x)) ≤ ε and dist(0, −∇yf (x, y) + ∂ιY (y)) ≤ ε.
Next, we introduce two acceleration techniques for TS-GDA developed in (Lin et al., 2025).
2.1 Dual Perturbation
The idea of dual perturbation is to introduce a small quadratic regularizer of the dual variable to the objective function. This yields the following perturbed, nonconvex-strongly-concave (NC-SC) minimax problem
min
x∈X max
y∈Y ft(x, y), where ft(x, y) = f (x, y) − rty
2 ∥y∥2. (2.1)
Throughout this paper, we let rty > 0 be non-increasing in t. This perturbation renders the objective strongly concave in y at each iteration, thereby enabling the use of standard techniques for NC-SC minimax problems. Intuitively, if we choose rty = O(ε) for all t ≥ 0, the perturbation term alters the dual gradient by at most O(ε). As a result, any ε-GS point of (1.1) remains an O(ε)-GS point of (2.1), so that convergence guarantees established for the perturbed problem (2.1) translate to the original problem (1.1) up to an O(ε) error. Moreover, if the perturbation parameter rty gradually diminishes to zero, the iterates are expected to asymptotically recover stationarity points for the original problem (1.1). Motivated by these observations, the algorithms in (Xu et al., 2023; Lu et al., 2020) essentially apply vanilla GDA to (2.1), which we formalize below as the Perturbed GDA algorithm.
5

Algorithm 1: Perturbed GDA
Data: Initial x0, y0, and step sizes ct, αt > 0 1 for t = 0, . . . , T do
2 xt+1 = projX (xt − ct∇xft(xt, yt));
3 yt+1 = projY (yt + αt∇yft(xt+1, yt)). 4 end
Since the perturbed objective ft is strongly concave in y at each iteration, one may expect Perturbed GDA to improve upon the O(ε−6) iteration complexity of vanilla TS-GDA established in (Lin et al., 2020a; Boţ and Böhm, 2023). This is indeed the case: Xu et al. (2023); Lu et al. (2020) show that Perturbed GDA finds an ε-GS of (1.1) in O(ε−4) iterations, demonstrating a clear improvement in terms of GS. However, we note that such an improvement is only for ε-GS, and the convergence rate of Perturbed GDA with respect to ε-OS remains unexplored in the literature.
2.2 Smoothing
Unlike dual perturbation, the smoothing technique modifies the optimization landscape through a fundamentally different mechanism. It is based on the Moreau envelope. For a given function Φ and any r < 1
l , its Moreau envelope Φr is defined as
Φr(x) := min
z∈Rn
n
Φ(z) + 1
2r ∥x − z∥2o
.
The Moreau envelope Φr(x) is not merely a smooth approximation of Φ, but also a surrogate of the original problem: Its minimizers coincide with those of Φ, and its gradient provides a natural measure of stationarity for Φ (Davis and Drusvyatskiy, 2019). The smoothness of Φr(x) further makes it a convenient and well-structured surrogate for algorithm design. Using this idea, Zhang et al. (2020) introduce the smoothed surrogate function
F (x, y, z) := f (x, y) + rx
2 ∥x − z∥2,
where rx > 0 is a smoothing parameter. In their algorithm, they update (x, z) by gradient descent and y by gradient ascent with respect to F . This algorithm is formally presented in the following Smoothed GDA.
Algorithm 2: Smoothed GDA
Data: Initial point (x0, y0, z0), and parameters rx > l, ct > 0, αt > 0, βt ∈ (0, 1) 1 for t = 0, . . . , T do
2 xt+1 = projX (xt − ct∇xF (xt, yt, zt));
3 yt+1 = projY (yt + αt∇yF (xt+1, yt, zt));
4 zt+1 = zt + βt(xt+1 − zt); 5 end
Recently, Li et al. (2025) further show that Smoothed GDA achieves both ε-GS and ε-OS within O(ε−4) iterations. The intuition for such an improvement is as follows: Note that the definition of Moreau envelope dovetails naturally with the definition of ε-OS in Definition 2.1. This structure enables Smoothed GDA to converge to an ε-OS faster. In other words, the acceleration due to smoothing for ε-OS is “direct". On the other hand, because any ε-OS can be converted into an Θ(ε)-GS in O(ε−2) iterations (cf. (Lin et al., 2020a, Proposition 4.13)), the acceleration induced by smoothing on ε-OS also carries over to ε-GS. In other words, smoothing improves the convergence rate to an ε-GS in an “indirect" manner.
6

3 Main Results
This section presents our main algorithmic and theoretical results. We first propose a new method, Perturbed Smoothed GDA, which combines dual perturbation and smoothing in a single-loop first-order algorithm. We then establish iteration complexity bounds for computing ε-GS and ε-OS solutions across several representative single-loop methods, and provide matching lower bounds showing that these rates are tight. Our analysis makes explicit how the effect of smoothing and perturbation interact and explains why their combination leads to an improved convergence guarantee to ε-GS for NC–C minimax optimization. Finally, we establish asymptotic convergence to 0-GS for both Perturbed GDA and Perturbed Smoothed GDA, a guarantee that is not available for vanilla Smoothed GDA under existing analyses.
3.1 Perturbed Smoothed GDA
Similar to previous methods, our algorithm also starts with a surrogate function. Specifically, at each iteration t, we combine dual perturbation and smoothing and construct the following surrogate function:
Ft(x, y, z) := f (x, y) + rx
2 ∥x − z∥2
| {z }
Smoothing
− rty
2 ∥y∥2
| {z }
Dual perturbation
. (3.1)
We apply gradient descent on variables x and z, and gradient ascent on variable y in each iteration. This update rule leads to the following Perturbed Smoothed GDA algorithm.
Algorithm 3: Perturbed Smoothed GDA
Data: Initial point (x0, y0, z0), and parameters rx > l, rty > 0, ct > 0, αt > 0, βt ∈ (0, 1)
1 for t = 0, . . . , T do
2 xt+1 = projX (xt − ct∇xFt(xt, yt, zt));
3 yt+1 = projY (yt + αt∇yFt(xt+1, yt, zt));
4 zt+1 = zt + βt(xt+1 − zt); 5 end
3.2 Convergence Results and their Tight Analysis
Before presenting our main iteration-complexity theorems, we introduce two preliminaries that facilitate our subsequent analysis. First, in Definition 3.1 we formalize the initial gap for each algorithmic family, namely, Perturbed GDA, Smoothed GDA and Perturbed Smoothed GDA. Second, we specify the step-size conditions which ensure basic descent estimates (See Section 4 for details). These technical bounds are provided for completeness and can be safely skipped in a first reading.
Definition 3.1 (Initial gaps). We define two initial gaps, i.e., ∆Ψ1 and ∆Ψ2 as below:
(i) The first gap is defined as
∆Ψ1 := Ψ0
1(x0, y0) − min
x∈X max
y∈Y f0(x, y),
where Ψ01(x, y) := 2 maxy∈Y f0(x, y) − f0(x, y), and f0(·, ·) is defined in (2.1).
7

(ii) The second gap is defined as
∆Ψ2 := Ψ0
2(x0, y0, z0) − min
x∈X max
y∈Y f0(x, y),
where Ψ02(x, y, z) := F0(x, y, z) − 2 minx∈X F0(x, y, z) + 2 minx∈X maxy∈Y F0(x, y, z), and F0(·, ·, ·) is defined in (3.1).
Remark 3.1. By construction, ∆Ψ1 measures the initial gap associated with Perturbed GDA, and ∆Ψ2 measures the initial gap for both Smoothed GDA and Perturbed Smoothed GDA. For Smoothed GDA, ∆Ψ2 is defined by setting r0y = 0 in F0.
Next, we summarize the step-size conditions for different algorithms:
Condition 3.1 (Step-size conditions for Perturbed GDA). Let rty > 0 for all t ≥ 0. The step sizes {αt}t≥0
and {ct}t≥0 are chosen such that, for all t, we have
αt = Θ 1
l and 0 < αt ≤ 1
4(l + rty) ;
ct = Θ (rty)2
l3
!
and 0 < ct ≤ min
( (rty)2αt
16l2 , rty
l(3rty + 2l) , 3rty
128l2
)
.
Condition 3.2 (Step-size conditions for (Perturbed) Smoothed GDA). Let rx = Θ(l) with rx > 3l. The step sizes {αt}t≥0, {ct}t≥0, and {βt}t≥0 are chosen such that, for all t ≥ 0,
αt = Θ 1
l and 0 < αt ≤ min
(
1
11(l + rty) , (ct)2(rx − l − rty)2
4(l + rty)(1 + ct(rx − l − rty))2
)
;
ct = Θ 1
l and 0 < ct < 1
rx + l + rty
;
0 < βt ≤ min
(
1
36 , (rx − l − rty)2
384rx(rx + l + rty)2
)
.
Moreover, the parameter βt and rty are chosen differently for Smoothed GDA and Perturbed Smoothed GDA:
(i) Smoothed GDA: For all t ≥ 0,
rt
y = 0 and βt = Θ ε2
l2D2
Y
!
.
(ii) Perturbed Smoothed GDA: Let rty > 0 for all t ≥ 0, and define
ωt := 1
q
rty
· (rx − l − rty) + αt(l + rty)(3rx − 2l − 2rty)
αt(rx − l − rty)3/2 .
Then
βt = Θ rty
l
!
, and 0 < βt ≤ 1
384 rxαt(ωt)2 .
8

With these step-size conditions in place, we present our main iteration-complexity results under a constant choice rty. We choose this setting for two reasons: First, our lower bounds (Theorems 3.2 and 3.4) are
established under a constant rty. Stating the upper bounds under the same setting yields a direct tightness guarantee. Second, the core ideas of our analytical framework for iteration complexity analysis are most cleanly shown when rty is constant.
Theorem 3.1 (Iteration complexity of Perturbed GDA). Let the sequence {(xt, yt)}t≥0 be generated by Perturbed GDA with step sizes satisfying Condition 3.1. Given any ε > 0, we have
(i) (Game Stationarity) Let ry := Θ ε
DY and set rty = ry for all t ≥ 0, then there exists an index
t ≤ O l3D2
Y ∆Ψ1
ε4 such that (xt+1, yt+1) is an ε-GS of problem (1.1).
(ii) (Optimization Stationarity) Let ry := Θ ε2
lD2
Y
and set rty = ry for all t ≥ 0, then there exists an
index t ≤ O l5D4
Y ∆Ψ1
ε6 such that xt+1 is an ε-OS of problem (1.1).
Theorem 3.2 (Tightness analysis of Perturbed GDA). Consider Perturbed GDA run with the step-size conditions in Theorem 3.1. Let (xT , yT ) be the output after T iterations. Then there exist an l-smooth function and an initialization (x0, y0) for which the following hold:
(i) (Game Stationarity) Perturbed GDA requires at least T = Ω l3D2
Y ∆Ψ1
ε4 iterations to guarantee that
the point xT , yT is an ε-GS of problem (1.1).
(ii) (Optimization Stationarity) Perturbed GDA requires at least T = Ω l5D4
Y ∆Ψ1
ε6 iterations to guarantee
that the point xT is an ε-OS of problem (1.1).
Remark 3.2. (i) A result analogous to Theorem 3.1 (i) has been obtained in (Lu et al., 2020; Xu et al., 2023). However, our proof relies on a different analytical framework (see Section 4); (ii) As noted in the introduction, prior works do not provide iteration complexity guarantees for ε-OS. In Theorem 3.1 (ii), we establish the first iteration complexity bound in terms of ε-OS, which matches the iteration complexity of vanilla TS-GDA. Together with our tight analysis in Theorem 3.2 (ii), these results show that while dual perturbation accelerates the convergence to ε-GS, it does not improve the convergence to ε-OS. In other words, Theorems 3.1 and 3.2 show that the convergence performance of Perturbed GDA on ε-GS and ε-OS is totally different. This observation clarifies that the benefits of dual perturbation are limited to GS and do not extend to OS in single-loop first-order methods for NC–C minimax optimization.
Theorem 3.3 (Iteration complexity of Perturbed Smoothed GDA). Let the sequence {(xt, yt, zt)}t≥0 be generated by Perturbed Smoothed GDA. Given any ε > 0, we let step sizes satisfy Condition 3.2. Then, we have
(i) (Game Stationarity) Let ry := Θ ε
DY and set rty = ry for all t ≥ 0, then there exists an index
t ≤ O l2DY ∆Ψ2
ε3 such that (xt, yt) is an ε-GS of problem (1.1).
(ii) (Optimization Stationarity) Let ry := Θ ε2
lD2
Y
and set rty = ry for all t ≥ 0, then there exists an
index t ≤ O l3D2
Y ∆Ψ2
ε4 such that xt is an ε-OS of problem (1.1).
9

Remark 3.3. (i) When rty = 0 for all t, Perturbed Smoothed GDA reduces to Smoothed GDA. For completeness, we state the iteration complexity of Smoothed GDA in Theorem B.1. For the OS guarantee, our proof differs from that of (Li et al., 2025). Moreover, due to the additional quadratic perturbation, the analysis of Perturbed Smoothed GDA is substantially different from that of Smoothed GDA. In particular, by exploiting the condition rty > 0, we avoid the two-stage analysis used in (Zhang et al., 2020; Li et al., 2025); (ii) In terms of ε-OS, Smoothed GDA and Perturbed Smoothed GDA achieve the same iteration complexity; (iii) In terms of ε-GS,
the two methods differ in the magnitude of the step size βt. For Smoothed GDA, one needs βt = Θ ε2
lD2
Y
,
whereas for Perturbed Smoothed GDA, it suffices to take βt = Θ ε
lDY . This more relaxed requirement
on βt enables Perturbed Smoothed GDA to achieve a faster convergence to game stationarity compared with Smoothed GDA.
Theorem 3.4 (Tightness analysis of Perturbed Smoothed GDA). Consider the Perturbed Smoothed GDA with step-size conditions in Theorem 3.3. Let (xT , yT , zT ) denote the output after T iterations. Then there exists an l-smooth function and an initialization (x0, y0, z0) such that
(i) (Game Stationarity) Perturbed Smoothed GDA requires at least T = Ω l2DY∆Ψ2
ε3 iterations to
guarantee that the point (xT , yT ) is an ε-GS of problem (1.1).
(ii) (Optimization Stationarity) Perturbed Smoothed GDA requires at least T = Ω l3D2
Y ∆Ψ2
ε4 iterations to
guarantee that the point xT is an ε-OS of problem (1.1).
Using similar instances as in Theorem 3.4, we can also establish the following tight complexity result for Smoothed GDA.
Corollary 3.1 (Tightness analysis of Smoothed GDA). Consider the Smoothed GDA with the step-size conditions from Condition 3.2, and denote by (xT , yT , zT ) the output after T iterations. Then, there exists an l-smooth function and an initialization (x0, y0, z0) such that
(i) (Game Stationarity) Smoothed GDA requires at least T = Ω l3D2
Y ∆Ψ2
ε4 iterations to guarantee that
the point (xT , yT ) is an ε-GS of problem (1.1).
(ii) (Optimization Stationarity) Smoothed GDA requires at least T = Ω l3D2
Y ∆Ψ2
ε4 iterations to guarantee
that the point xT is an ε-OS of problem (1.1).
These results clearly delineate the roles of the two acceleration techniques: Smoothing alone improves convergence to both optimization stationarity and game stationarity, whereas dual perturbation alone accelerates only convergence to game stationarity. By combining the two techniques, Perturbed Smoothed GDA attains the best-known rate for game stationarity while preserving the state-of-the-art rate for optimization stationarity among single-loop first-order methods. In other words, smoothing suffices to achieve the state-of-the-art rate for optimization stationarity, while dual perturbation contributes only to further accelerating game stationarity when used alongside smoothing. Moreover, this cooperative speed-up for game stationarity suggests that smoothing and dual perturbation enhance convergence to ε-GS through distinct, yet complementary, mechanisms. While the above results focus on a constant (non-adaptive) choice of rty for finite-time guarantees, we
next consider a diminishing sequence rty ↘ 0 for both Perturbed GDA and Perturbed Smoothed GDA. We show that the vanishing perturbation further ensures asymptotic convergence to a stationary point of (1.1).
10

Theorem 3.5 (Asymptotic Convergence of Perturbed GDA). Let {(xt, yt)}t≥0 be generated by Perturbed GDA with step sizes satisfying Condition 3.1. Define
I⋆(T ) =
T
[
t=1
argmin
k∈{⌊t/2⌋,...,t}
1
4αk ∥yk+1 − yk∥2 + 1
16ck ∥xk+1 − xk∥2 .
When we choose rty ↘ 0 and t · (rty)2 → ∞, any limit point of {(xt, yt)}t∈I⋆(∞) is a 0-GS of (1.1).
Theorem 3.6 (Asymptotic Convergence of Perturbed Smoothed GDA). Let the sequence {(xt, yt, zt)}t≥0 be generated by Perturbed Smoothed GDA. Define
I⋆(T ) =
T
[
t=1
argmin
k∈{⌊t/2⌋,...,t}
1
8ck ∥xk+1 − xk∥2 + 1
16αk∥yk − yk
+(zk)∥2 + rxβk
8 ∥zk − xk+1∥2 ,
where yk+(z) := projY yk + αk∇yFk(xk(yk, z), yk, z) and xk(y, z) := argmin
x∈X
Fk(x, y, z). When we
choose rty ↘ 0 and t · rty → ∞, any limit point of {(xt, yt)}t∈I⋆(∞) is a 0-GS of (1.1).
Remark 3.4. (i) An interesting point is that the original smoothed GDA does not admit an asymptotic convergence guarantee under the current analysis. As can be seen from the proof in Zhang et al. (2020), the two-stage argument introduces a fundamental difficulty: we cannot rule out the possibility that the iterates enter the unfavorable case infinitely often. Consequently, the Lyapunov function may fail to satisfy a sufficient descent property not only along the whole sequence, but also along any subsequence extracted from it. (ii) Although our non-asymptotic convergence results are stated for a non-adaptive choice of rty, the analysis in
Section 4 extends directly to adaptive choices of rty. In particular, the same finite-time convergence rates continue to hold, provided that the adaptive sequence satisfies the parameter conditions required in the proof.
4 A Unified Convergence Analysis
In this section, we establish iteration complexity guarantees for both Perturbed GDA (see Theorem 3.1) and Perturbed Smoothed GDA (see Theorem 3.3) under two stationarity notions: optimization stationarity and game stationarity. To this end, we develop a unified analytical framework that encompasses both algorithms and serves as the foundation for the subsequent convergence analyses. Our framework contains the following two key components:
(i) The construction of a Lyapunov function that captures both the primal descent and dual ascent dynamics.
(ii) Derive a primal-dual balancing inequality that characterizes the interaction between the primal and dual updates, which ensures a descent property of the Lyapunov function.
Together, these two components establish the iteration complexity guarantees for single-loop algorithms. Furthermore, this analytical framework can apply to the double-loop algorithm (see Section 6) as well. Before proceeding to the formal proof, we summarize in Table 2 the main notations used in this section and throughout the paper.
4.1 Convergence Analysis of Perturbed GDA
In this subsection, we prove Theorems 3.1 and 3.5. Although rty is held constant in Theorem 3.1, we still keep the superscript t. This choice of presentation is to provide the most general form of our analytical framework.
11

Table 2: Notation
Notation Definition Notes
dt(y, z) min
x∈X Ft(x, y, z) dual function
pt(z) min
x∈X max
y∈Y Ft(x, y, z) proximal function
xt(y, z) argmin
x∈X
Ft(x, y, z) 
xt⋆(z) argmin
x∈X
max
y∈Y Ft(x, y, z) 
yt⋆(x) argmax
y∈Y
ft(x, y) 
yt(z) argmax
y∈Y
dt(y, z) 
yt+(z) projY yt + αt∇yFt(xt(yt, z), yt, z) one-step projected gradient ascent on the dual function
Our proof is based on the following Lyapunov function at each iteration t:
Ψt
1(x, y) := 2Φt(x) − ft(x, y) = Φt(x)
| {z }
Primal descent
+ Φt(x) − ft(x, y)
| {z }
Dual ascent
, (4.1)
where Φt(x) := maxy∈Y ft(x, y) + ιX (x).
Note that Ψt1(x, y) consists of two parts: Φt(x) measures how well x is minimizing the objective function and Φt(x) − ft(x, y) measures how far y is from the maximizer of the inner maximization problem of (2.1) given x. Therefore, if the algorithm proceeds properly, the value of the function Ψt1(x, y) should steadily decrease. The crux of our proof lies in establishing the descent of this Lyapunov function. In the following lemma, we give a basic descent estimate on the Lyapunov function in (4.1). For simplicity, we write Ψt1 := Ψt1(xt, yt).
Lemma 4.1 (Basic descent estimate). Suppose Condition 3.1 holds, and let {(xt, yt)}t≥0 be the sequence generated by Perturbed GDA. Then, for any t ≥ 0, we have
Ψt+1
1 − Ψt
1
≤ 2l∥y⋆
t (xt) − yt∥·∥xt+1 − xt∥− 1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 + rt
y − rt+1
y D2
Y.
Proof of Lemma 4.1. First of all, we quantify the basic descent of Φt as follows:
Φt+1(xt+1) − Φt(xt) = Φt+1(xt+1) − Φt(xt+1) + Φt(xt+1) − Φt(xt) . (4.2)
For the first term, observe that
ft+1(x, y) = ft(x, y) + rty − rty+1
2 ∥y∥2.
Hence,
Φt+1(xt+1) = max
y∈Y ft+1(xt+1, y)
12

≤ max
y∈Y ft(xt+1, y) + max
y∈Y
rty − rty+1
2 ∥y∥2
= Φt(xt+1) + rty − rty+1
2 D2
Y . (4.3)
For the second term in (4.2), we have
Φt(xt+1) − Φt(xt)
≤ ∇Φt(xt), xt+1 − xt + l
2 + l2
2rty
∥xt+1 − xt∥2 (4.4)
≤ ∇Φt(xt)−∇xft(xt, yt), xt+1 −xt + ∇xft(xt, yt), xt+1 −xt + l
2 + l2
2rty
∥xt+1 −xt∥2
≤ l∥y⋆
t (xt) − yt∥ · ∥xt+1 − xt∥ − 1
ct ∥xt+1 − xt∥2 + l
2 + l2
2rty
∥xt+1 − xt∥2
≤ l∥y⋆
t (xt) − yt∥ · ∥xt+1 − xt∥ − 1
ct − l
2 − l2
2rty
∥xt+1 − xt∥2, (4.5)
where the first inequality is due to the (l + l2
rty )-smoothness property of Φt (Lin et al., 2020a, Lemma 4.3), and
the third inequality follows from the l-Lipschitz continuity of ∇xft(·, y), the Cauchy-Schwarz inequality, and the characterization of projections onto a closed convex set, i.e., ⟨xt −xt+1, xt −ct∇xft(xt, yt)−xt+1⟩ ≤ 0. Now we focus on the dual ascent part. First note that
Φt+1(xt+1) − ft+1(xt+1, yt+1) − Φt(xt) − ft(xt, yt)
= Φt+1(xt+1)−Φt(xt) + ft(xt+1, yt+1)−ft+1(xt+1, yt+1) + ft(xt+1, yt)−ft(xt+1, yt+1)
+ ft(xt, yt) − ft(xt+1, yt) .
Because the decrease in Φt has been quantified, we study the other three terms ft(xt+1, yt+1)−ft+1(xt+1, yt+1), ft(xt+1, yt) − ft(xt+1, yt+1) and ft(xt, yt) − ft(xt+1, yt) in the following. Since rty is non-increasing, we have
ft(xt+1, yt+1) − ft+1(xt+1, yt+1) = rty+1 − rty
2 ∥yt+1∥2 ≤ 0. (4.6)
Moreover, we have
ft(xt+1, yt) − ft(xt+1, yt+1)
≤ ∇yft(xt+1, yt), yt − yt+1 + l + rty
2 ∥yt+1 − yt∥2
≤− 1
αt ∥yt+1 − yt∥2 + l + rty
2 ∥yt+1 − yt∥2 ≤ − 1
2αt ∥yt+1 − yt∥2, (4.7)
where the first inequality stems from the (l + rty)-Lipschitz continuity of ∇yft(x, ·), the second inequality is due to the characterization of projections onto a closed convex set Y, and the last one arises from Condition 3.1, i.e., 0 < αt < 1
4(l+rty) . Similarly, we obtain
ft(xt, yt) − ft(xt+1, yt) ≤ ∇xft(xt, yt), xt − xt+1 + l
2 ∥xt+1 − xt∥2, (4.8)
13

where the inequality is by the l-Lipschitz continuity of ∇xft(·, y). By summing (4.3), (4.4), (4.6), (4.7), and (4.8), we obtain a bound that controls the dual-ascent component in the Lyapunov function (4.1):
Φt+1(xt+1) − ft+1(xt+1, yt+1) − Φt(xt) − ft(xt, yt)
≤ ∇Φt(xt) − ∇xft(xt, yt), xt+1 − xt + l + l2
2rty
∥xt+1 − xt∥2 − 1
2αt ∥yt+1 − yt∥2
+ rty − rty+1
2 D2
Y
≤ l∥y⋆
t (xt)−yt∥·∥xt+1 −xt∥+ l+ l2
2rty
∥xt+1 −xt∥2 − 1
2αt ∥yt+1 −yt∥2 + rty −rty+1
2 D2
Y , (4.9)
where the second inequality follows from the Cauchy-Schwarz inequality and the l-Lipschitz continuity of ∇xft(x, ·).
Putting (4.5) and (4.9) together yields
Ψt+1
1 − Ψt
1
≤ 2l∥y⋆
t (xt) − yt∥ · ∥xt+1 − xt∥ − 1
2αt ∥yt+1 − yt∥2 − 1
ct − 3l
2 − l2
rty
∥xt+1 − xt∥2
+ (rt
y − rt+1
y )D2
Y
≤ 2l∥y⋆
t (xt) − yt∥ · ∥xt+1 − xt∥ − 1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 + (rt
y − rt+1
y )D2
Y,
where the final inequality is due to Condition 3.1, i.e., 0 < ct ≤ rty
l(3rty+2l) . This completes the proof.
■
In Lemma 4.1, the bound includes a positive term 2l∥yt⋆(xt) − yt∥ · ∥xt+1 − xt∥, which measures the
error introduced by yt not being optimal for a given xt. For the Lyapunov function to decrease, this term needs to be controlled. We leverage the strong concavity of ft(x, ·) to apply a dual error bound in (Pang, 1987, Theorem 3.1), which upper bounds the dual gap ∥yt⋆(xt+1) − yt∥ by the iterate difference ∥yt+1 − yt∥. The resulting bound is stated below.
Lemma 4.2 (Dual error bound; cf. (Pang, 1987) Theorem 3.1). Suppose that the sequence {(xt, yt)}t≥1 is generated by Perturbed GDA. Then, for any t ≥ 0, we have
∥y⋆
t (xt+1) − yt∥ ≤ 1 + αt(l + rty)
αtrty
∥yt+1 − yt∥.
With Lemmas 4.1 and 4.2, we now outline the main idea of the proof of Theorem 3.1. We will use Lemma 4.1 to bound the decrease of the Lyapunov function and a key step is to plug the dual gap bound in Lemma 4.2 to the descent inequality in Lemma 4.1. It yields a refined descent estimate for the Lyapunov function Ψt1. Together with the step-size conditions in Condition 3.1, the claimed ε-GS and ε-OS guarantees can be directly derived from this descent estimate.
Proof of Theorem 3.1. First, by combining Lemmas 4.1 and 4.2, we obtain a descent property for Ψ1. For t ≥ 0,
Ψt+1
1 − Ψt
1
14

≤ 2l∥y⋆
t (xt) − yt∥·∥xt+1 − xt∥− 1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 +(rt
y − rt+1
y )D2
Y
≤ 2l∥y⋆
t (xt+1) − yt∥ · ∥xt+1 − xt∥ + 2l∥y⋆
t (xt+1) − y⋆
t (xt)∥ · ∥xt+1 − xt∥
−1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 + (rt
y − rt+1
y )D2
Y
≤ 2l(1 + αt(l + rty))
αtrty
∥yt+1 − yt∥ · ∥xt+1 − xt∥ + 2l2
rty
∥xt+1 − xt∥2
−1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 + (rt
y − rt+1
y )D2
Y
≤1
4αt ∥yt+1 − yt∥2 + 4l2(1 + αt(l + rty))2
αt(rty)2 ∥xt+1 − xt∥2 + 2l2
rty
∥xt+1 − xt∥2
−1
2αt ∥yt+1 − yt∥2 − 1
2ct ∥xt+1 − xt∥2 + (rt
y − rt+1
y )D2
Y
≤− 1
4αt ∥yt+1 − yt∥2 − 1
16ct ∥xt+1 − xt∥2 + (rt
y − rt+1
y )D2
Y , (4.10)
where the first inequality is by Lemma 4.1 and the third inequality is due to Lemma 4.2 and (Lin et al., 2020a, Lemma 4.3), the fourth one follows because ab ≤ 1
2 (a2 + b2) for any a, b ∈ R, and the last one is due to
Condition 3.1, i.e., 1 + α(l + rty) ≤ 5
4 and ct ≤ min{ 3rty
128l2 , (rty)2αt
16l2 }. Summing (4.10) for t = 0, · · · , T − 1, we have
ΨT
1 − Ψ0
1≤−
T −1
X
t=0
1
4αt ∥yt+1 − yt∥2 + 1
16ct ∥xt+1 − xt∥2 + (r0
y − rT
y )D2
Y.
Thus, because we choose rty = r0y for all t, then there exists a t ∈ [T ] satisfying t ≥ ⌊ T
2 ⌋ such that
∥yt+1 − yt∥ = O
r
αt∆Ψ1
T
!
and ∥xt+1 − xt∥ = O
r
ct∆Ψ1
T
!
.
This conclusion follows from the fact that Ψt1(xt, yt) ≥ Φt(xt) ≥ minx∈X Φt(x).
(i) For the GS case, we choose rty of order Θ 4
r
∆Ψ1 l3
D2
YT . The optimality condition of y-update yields that
0 ∈ rt
yyt + 1
αt (yt+1 − yt) − ∇yf (xt+1, yt) + ∂ιY (yt+1)
∈ − ∇yf (xt+1, yt+1) + ∂ιY (yt+1) + rt
yyt + 1
αt (yt+1 − yt)
+ (∇yf (xt+1, yt+1) − ∇yf (xt+1, yt)).
Then, we have
dist(0, −∇yf (xt+1, yt+1) + ∂ιY (yt+1))
≤ ∥rt
yyt∥ + 1
αt ∥yt+1 − yt∥ + ∥∇yf (xt+1, yt+1) − ∇yf (xt+1, yt)∥
≤ rt
yDY + 1
αt + l ∥yt+1 − yt∥ (4.11)
= O max rt
yDY ,
r l∆Ψ1
T
!!
(4.12)
15

=O


4
s
l3D2
Y ∆Ψ1
T

,
where the second inequality follows from the l-Lipschitz continuity of ∇yf (xt+1, ·) and the first equality is due to Condition 3.1, i.e., αt = Θ( 1
l ).
Next, we turn to the primal part. Similarly, the optimality condition of the x-update yields that
0∈ 1
ct (xt+1 − xt) + ∇xf (xt, yt) + ∂ιX (xt+1)
∈ ∇xf (xt+1, yt+1) + ∂ιX (xt+1) + 1
ct (xt+1 − xt) + (∇xf (xt, yt) − ∇xf (xt+1, yt+1)).
It follows that
dist(0, ∇xf (xt+1, yt+1) + ∂ιX (xt+1))
≤1
ct ∥xt+1 − xt∥ + ∥∇xf (xt+1, yt+1) − ∇xf (xt, yt)∥
≤1
ct + l ∥xt+1 − xt∥ + l∥yt+1 − yt∥
= O max
r ∆Ψ1
ctT ,
r l∆Ψ1
T
!!
(4.13)
=O


4
s
l3D2
Y ∆Ψ1
T

,
where the second inequality follows from the l-smoothness of the function f , and the last equality is a
consequence of Condition 3.1, i.e. ct = Θ (rty)2
l3 = Θ
r
∆Ψ1
l3 D2
YT .
For the GS case, our analysis gives ε ≤ O 4
q l3D2
Y ∆Ψ1
T , which is equivalent to T ≤ O l3D2
Y ∆Ψ1
ε4 .
Thus O l3D2
Y ∆Ψ1
ε4 iterations suffice to reach an ε-GS, and the corresponding choice of rty is rty = Θ( ε
DY ).
(ii) We now turn to the OS case. We choose rty as Θ 3
r
l2 ∆Ψ1
D2
YT . To proceed, we rely on the following
lemma, which establishes a connection between ε-GS and ε-OS for general smooth NC-C minimax problems. The proof of Lemma 4.3 is provided in Appendix A.1.
Lemma 4.3. Suppose that Condition 3.1 holds. We have
prox 1
2l Φ(x) − x
2
≤ 2DY
l dist(0, −∇yf (x, y) + ∂ιY (y)) + 1
l2 dist2(0, ∇xf (x, y) + ∂ιX (x)).
Now, we apply Lemma 4.3 to connect GS and OS:
prox 1
2l Φ(xt+1) − xt+1 2
16

≤ 2DY
l dist(0,−∇yf (xt+1,yt+1)+∂ιY (yt+1))+ 1
l2 dist2(0,∇xf (xt+1,yt+1)+∂ιX (xt+1))
=O


DY
l · max
r l∆Ψ1
T , rt
y DY
!
+1
l2 max
r ∆Ψ1
ctT ,
r l∆Ψ1
T
!2

=O


3
s
D4
Y ∆Ψ1
lT

,
where the first equality comes from (4.12) and (4.13), and the last equality is due to Condition 3.1, i.e.,
ct = Θ (rty)2
l3 = Θ 3
r
∆2
Ψ1
l5 D4
YT2
!
.
Putting everything together yields
2l prox 1
2l Φ(xt+1) − xt+1 = O


6
s
l5D4
Y ∆Ψ1
T

.
Following the similar argument, we need at least T = O l5D4
Y ∆Ψ1
ε6 to reach an ε-OS if we choose
rty = Θ ε2
lD2
Y
. This completes the proof. ■
With Theorem 3.1 proved, we next prove Theorem 3.5. We show that the proof of Theorem 3.5 follows directly from the preceding analysis, even though it uses a diminishing sequence of rty.
Proof of Theorem 3.5. It holds that |I⋆(∞)| = +∞, since for each T , the set
argmin
k∈{⌊T /2⌋,...,T }
1
4αk ∥yk+1 − yk∥2 + 1
16ck ∥xk+1 − xk∥2
is nonempty and contained in I⋆(T ). Hence, maxk∈I⋆(T ) k ≥
jT 2
k
for every T , which implies that I⋆(∞)
is infinite. Let (x∞, y∞) be any limit point of the sequence {(xt, yt)}t∈I⋆(∞). Then there exists a subsequence {(xik , yik )}k≥1 such that
kli→m∞(xik , yik ) = (x∞, y∞). (4.14)
Note that (4.12) and (4.13) holds for every t = ik. In particular, (4.12) implies
kli→m∞ dist(0, −∇yf (xik+1, yik+1) + ∂ιY (yik+1))
=O

kli→m∞ max


s
l ∆Ψ1 + r0yD2
Y
ik
, riyk DY



 = 0, (4.15)
where the last equality follows from riyk ↘ 0. Moreover, since ∥xik+1 − xik ∥ → 0 and ∥yik+1 − yik ∥ → 0, (4.14) yields (xik+1, yik+1) → (x∞, y∞). Because Y is nonempty, closed, and convex, the operator ∂ιY is
17

maximal monotone (Rockafellar, 1970, Theorem A), and hence its graph is closed (Rockafellar and Wets, 2009, Exercise 12.8). Therefore, combining the above with (4.15), we obtain
0 ∈ −∇yf (x∞, y∞) + ∂ιY (y∞). (4.16)
Similarly, for the primal part, (4.13) implies
kli→m∞ dist(0, ∇xf (xik+1, yik+1) + ∂ιX (xik+1))
=O

kli→m∞ max


s
∆Ψ1 + r0yD2
Y
cik ik
,
s
l ∆Ψ1 + r0yD2
Y
ik



 = 0,
where the last equality follows from ik(riyk )2 → ∞ and Condition 3.1, i.e., cik = Θ((riyk )2). By the same argument as above, we obtain
0 ∈ ∇xf (x∞, y∞) + ∂ιX (x∞). (4.17)
Therefore, combining (4.16) and (4.17) yields that (x∞, y∞) is a 0-GS of problem (1.1). This completes the proof. ■
4.2 Convergence Analysis of Perturbed Smoothed GDA
In this subsection, we establish the iteration complexity of Perturbed Smoothed GDA stated in Theorem 3.3. Similar to Section 3.1, though Theorem 3.3 is stated for constant choice of rty, we prove a stronger result
where rty is adaptive to provide the most general form of our analytical framework. Following our unified analysis framework, we begin by introducing the Lyapunov function:
Ψt
2(x, y, z) := Ft(x, y, z) − dt(y, z)
| {z }
Primal descent
+ (pt(z) − dt(y, z))
| {z }
Dual ascent
+ pt(z)
| {z }
Proximal descent
. (4.18)
Similar as before, we first establish a basic descent property of Ψt2(x, y, z), which will be used to prove Theorem 3.3. The proof is essentially the same as that of (Zhang et al., 2020, Proposition 4.1), except that an additional error term (rty − rty+1)D2
Y appears. To deal with the additional error term, the argument follows a
similar technical route as Lemma 4.1. For simplicity, we write Ψt2 := Ψt2(xt, yt, zt).
Lemma 4.4 (Basic descent estimate). Suppose Condition 3.2 holds, and let {(xt, yt, zt)}t≥0 be the sequence generated by Perturbed Smoothed GDA. Then, for any t ≥ 0, we have
Ψt+1
2 − Ψt
2 ≤− 1
8ct ∥xt+1 − xt∥2 − 1
8αt yt − yt
+(zt) 2 − rxβt
8 ∥zt − xt+1∥2
+ 24rxβt x⋆
t (zt) − xt(yt
+(zt), zt) 2 + 3(rty − rty+1)
2 D2
Y.
To establish the descent property of Ψt2, it suffices to control the term ∥xt⋆(zt) − xt(yt+(zt), zt)∥2. By construction, applying Perturbed Smoothed GDA on an NC-C minimax problem can be viewed as applying the vanilla Smoothed GDA to an equivalent NC-SC minimax problem, where the strong concavity on the dual side is induced by the perturbation parameter rty; hence, using the homogeneous dual error bound from (Li
et al., 2025, Corollary 5.1), we bound it by the positive term ∥yt − yt+(zt)∥2 in the basic descent estimate. We formalize this homogeneous dual error bound in the following lemma and its proof is in Appendix A.2.
18

Lemma 4.5 (Dual error bound for NC-SC). For any t ≥ 0, any y ∈ Y and z ∈ Rn, we have
x⋆
t (zt) − xt(yt
+(zt), zt) ≤ ωt yt − yt
+(zt) ,
where ωt := √1rty
· (rx−l−rty)+αt(l+rty)(3rx−2l−2rty)
αt(rx−l−rty)3/2 .
With Lemma 4.4 and Lemma 4.5, we are now ready to present the detailed proof of Theorem 3.3. Similar to the previous subsection, the key of the proof is still using Lemmas 4.4 and 4.5, and Condition 3.2 to establish the decreasing property of the Lyapunov function.
Proof of Theorem 3.3. First, under the step-size condition in Condition 3.2 that βt < 1
384 rxαt(ωt)2 , we obtain
Ψt
2 − Ψt+1
2
≥1
8ct∥xt+1 −xt∥2 + 1
8αt −24rxβt(ωt)2 ∥yt −yt
+(zt)∥2 + rxβt
8 ∥zt −xt+1∥2 + 3(rty−rty+1)
2 D2
Y
≥1
8ct ∥xt+1 − xt∥2 + 1
16αt ∥yt − yt
+(zt)∥2 + rxβt
8 ∥zt − xt+1∥2 + 3(rty − rty+1)
2 D2
Y.
By (Zhang et al., 2020, Lemma B.1), under the definition of ∆Ψ2 and the parameter choice rty = r0y for all t,
for any integer T > 0, there exists an index t ∈ [T ] such that t ≥ ⌊ T
2 ⌋ such that
∥xt+1 − xt∥ ≤
r
8ct∆Ψ2
T , ∥yt − yt
+(zt)∥ ≤
r
16αt∆Ψ2
T , and ∥zt − xt+1∥ ≤
s
8∆Ψ2
rxβtT .
Therefore, substituting the parameter choices from Condition 3.2 into the above estimate yields
max
n
∥xt+1 − xt∥, ∥yt − yt
+(zt)∥
o
=O
r ∆Ψ2 lT
!
, ∥zt − xt+1∥ = O
s
∆Ψ2
rty T
!
. (4.19)
(i) For the GS case, we choose rty as Θ 3
r
l2 ∆Ψ2
D2
YT .
From (4.11), we have
dist(0, −∇yf (xt+1, yt+1) + ∂ιY (yt+1))
≤1
αt + l ∥yt+1 − yt∥ + rt
y DY
≤1
αt + l ∥yt − yt
+(zt)∥ + αtlσt
3∥xt+1 − xt∥ + rt
yDY (4.20)
= O max
r l∆Ψ2
T , rt
y DY
!!
(4.21)
=O 3
r
l2DY ∆Ψ2
T
!
,
19

where the second inequality follows from the triangle inequality and (Zhang et al., 2020, Lemma B.9), which
states that ∥yt+1 − yt+(zt)∥ ≤ αtlσt3∥xt+1 − xt∥, where σt3 := 1+ct(rx−l)
ct(rx−l) = Θ(1), and the first equality
comes from (4.19), the choice of rty and Condition 3.2, i.e., αt = Θ( 1
l ).
Next, we examine the primal part. The optimality condition of the x–update gives
0 ∈ ∇xf (xt, yt) + ∂ιX (xt+1) + 1
ct (xt+1 − xt) + rx(xt+1 − zt)
∈ ∇xf (xt+1, yt+1) + ∂ιX (xt+1) + (∇xf (xt, yt) − ∇xf (xt+1, yt+1))
+1
ct (xt+1 − xt) + rx(xt+1 − zt).
It follows that
dist(0, ∇xf (xt+1, yt+1) + ∂ιX (xt+1))
≤ ∥∇xf (xt, yt) − ∇xf (xt+1, yt+1)∥ + 1
ct ∥xt+1 − xt∥ + rx∥xt+1 − zt∥
≤1
ct + l ∥xt+1 − xt∥ + l∥yt+1 − yt∥ + rx∥xt+1 − zt∥
≤1
ct + l ∥xt+1 − xt∥ + l ∥yt − yt
+(zt)∥ + αtlσt
3∥xt+1 − xt∥ + rx∥xt+1 − zt∥ (4.22)
= O max
r l∆Ψ2
T,
s
l2∆Ψ2
rty T
!!
(4.23)
=O 3
r
l2DY ∆Ψ2
T
!
,
where the second inequality follows from the l-Lipschitz continuity of ∇xf (·, ·), the third inequality follow from (Zhang et al., 2020, Lemma B.9), and the first equality is due to (4.19) and Condition 3.2, i.e., ct, αt = Θ( 1
l ) and rx = Θ(l).
Recall that for the GS case, we choose rty = Θ 3
r
l2 ∆Ψ2
D2
YT so that the two terms in (4.21) and (4.23)
are of the same order. With this choice, the resulting bound scales as O 3
q l2DY ∆Ψ2
T . Thus, to reach an
ε-GS it suffices to take T = O l2DY ∆Ψ2
ε3 , and substituting this T back into the above choice yields the
equivalent parameterization rty = Θ ε
DY .
(ii) For the OS case, we choose rty as Θ
r
l∆Ψ2
D2
YT . Then we obtain
prox 1
2l Φ(xt+1) − xt+1 2
≤ 2DY
l dist(0,−∇yf (xt+1,yt+1)+∂ιY (yt+1))+ 1
l2 dist2(0,∇xf (xt+1,yt+1)+∂ιX (xt+1))
= O DY
l · max
r l∆Ψ2
T , rt
y DY
!
+1
l2 · max
r l∆Ψ2
T ,3
s
l2∆Ψ2
rty T
!!
20

=O


s
D2
Y ∆Ψ2
lT

,
where the first inequality arises from Lemma 4.3, and the first equality follows from (4.21) and (4.23). Putting everything together yields
2l prox 1
2l Φ(xt+1) − xt+1 = O


4
s
l3D2
Y ∆Ψ2
T

.
Following the similar argument as the case of GS, we need at least T = O l3D2
Y ∆Ψ2
ε4 iterations to reach an
ε-OS when we choose rty = Θ ε2
lD2
Y
. This completes the proof. ■
The proof of Theorem 3.6 follows the same steps as the proof of Theorem 3.5, and is therefore omitted.
Remark 4.1. We prove Theorem 3.3 (i) within our unified framework, illustrating its versatility and generality. We note, however, that the same iteration complexity bound can also be recovered through a simple reduction. Indeed, by (Yang et al., 2022, Corollary 4.1), the iteration complexity of Smoothed GDA for the NC-SC minimax problem (2.1) is O(κl∆ε−2). If we choose rty = ry = Θ(ε/DY ) for all t ≥ 0, then κ = l/ry = Θ(lDY /ε). Substituting this relation into the above bound gives
O(κl∆ε−2) = O l2DY ∆Ψ2
ε3 .
A similar observation also appears in (Aybat et al., 2025, Remark 11). Nevertheless, such a reduction only recovers the same finite-time rate for a constant choice of rty. Since a constant perturbation parameter does not vanish, this reduction does not lead to an asymptotic convergence guarantee for the original NC-C problem. Our unified framework goes beyond this reduction by accommodating adaptive or diminishing perturbation sequences; this flexibility is essential for establishing the asymptotic convergence of Perturbed Smoothed GDA; see Theorem 3.6.
5 Tight Analysis
In Section 4, we established the iteration complexity of Perturbed GDA and Perturbed Smoothed GDA for both game stationarity and optimization stationarity. In this section, we show that these bounds are tight. Specifically, we construct hard instances and derive lower bounds on the number of iterations required by each algorithm to reach a stationary point. Comparing these lower bounds with the corresponding upper bounds from Section 4 establishes the tightness claims. For lower bound analysis in this section, we consider algorithms with constant choice of rty for simplicity. Thus, we drop iteration superscripts and write rty = ry,
ct = c, αt = α, and βt = β, as these parameters are fixed across all iterations.
5.1 Game Stationarity
In this subsection, we prove Theorem 3.2 (i) and Theorem 3.4 (i), establishing the tightness of the iteration complexities stated in Theorem 3.1 (i) and Theorem 3.3 (i) for ε-GS. To this end, we construct a hard instance and show that the Perturbed GDA and Perturbed Smoothed GDA require at least the corresponding number of iterations to reach an ε-GS on this instance. The hard instance is given below.
21

Example 5.1. Let the function f : R × [0, DY ] → R and be defined by
f (x, y) =

 
 
0, if x < 0,
−1
2 lx2 + bxy, if 0 ≤ x ≤ ryDY
b,
− lry2D2
Y
2b2 + ryDY y, if x > ryDY
b,
(5.1)
where b = p3lry. It can be verified that f (x, y) is concave in y, and l-smooth.
Remark 5.1. The piecewise-smooth construction in Example 5.1 is introduced to adapt the hard instance to the bounded-dual-domain setting. In particular, unlike standard constructions that rely on strong concavity and quadratic structure, our example only assumes concavity in the dual variable and requires a piecewise modification to ensure the normalization
min
x∈R max
y∈[0,DY ]
f0(x, y) = min
x∈R max
y∈[0,DY ]
n
f (x, y) − ry
2 y2o
= 0.
This normalization ensures that the initial gaps ∆Ψ1 and ∆Ψ2 are well defined and explicitly computable, which is essential for matching the upper bounds in Theorem 3.1 (i) and Theorem 3.3 (i).
5.1.1 Perturbed GDA
In this subsubsection, we prove Theorem 3.2 (i) by showing that Perturbed GDA needs at least the number of iterations stated in Theorem 3.1 (i) to reach an ε-GS on Example 5.1. Specifically, for any given ε, we
choose step sizes α and c to satisfy Condition 3.1, and set ry = Θ ε
DY . We show that if an iterate (xT , yT )
generated by Perturbed GDA is an ε-GS, then necessarily T = Ω l3D2
Y ∆Ψ1
ε4 .
To establish this lower bound, we analyze the dynamics of Perturbed GDA on Example 5.1. In the regime
xt ∈ 0, ryDY
b , the update rule can be written in the following linear vector form:


xt+1
yt+1

 = (I + M1)


xt
yt

 , where M1 :=


lc −cb
αb(1 + lc) −αry − cb2α

.
The convergence behavior of Perturbed GDA is determined by the spectral properties of I + M1. To quantify its contraction factor, we study the eigenvalues of M1 (equivalently, those of I + M1). In particular, a direct calculation shows that M1 has an eigenvalue
λ1 = 1
2 A1 +
q
A21 − 4B1 ,
where A1 := lc − αry − αb2c and B1 := αc (b2 − lry). Using b2 = 3lry, we have A1 = lc − αry(1 + 3lc) and B1 = 2lcαry.
Next, we show that λ1 < 0 and characterize its magnitude. Because λ1 < 0 and |λ1| is sufficiently small, the recursion contracts with factor (1 + λ1) along the corresponding eigendirection. This contraction factor determines the convergence speed along that direction. The proof of Lemma 5.1 is deferred to Appendix A.3.
Lemma 5.1. The eigenvalue λ1 satisfies λ1 < 0 and |λ1| = Θ ε2
l2 D2
Y
.
With the sign and magnitude of λ1 known, we choose (x0, y0) so that its direction is aligned with an eigenvector of I + M1, which reduces the two-dimensional recursion to a one-dimensional multiplicative dynamics along the eigendirection and yields an explicit expression for the iterates.
22

Lemma 5.2. Let Perturbed GDA be initialized at x0 = 2ε
l and y0 = 2b(1+lc)α
√A2
1−4B1+2lc−A1
· x0. Then, for all
t ∈ N, the iterates satisfy xt ∈ [0, ryDY
b ] and yt ∈ [0, DY ]. Moreover, we have
xt = (1 + λ1)tx0, and yt = (1 + λ1)ty0. (5.2)
The proof of Lemma 5.2 is in Appendix A.4. With Lemmas 5.1 and 5.2, we are now ready to prove Theorem 3.2 (i).
Proof of Theorem 3.2 (i). We initialize Perturbed GDA at x0 = 2ε
l ∈ R and y0 = 2b(1+lc)α
√A2
1−4B1+2lc−A1
· 2ε
l∈
[0, DY ]. If (xT , yT ) is an ε-GS, then by definition,
ε ≥ |∇xf (xT , yT )| = |byT − lxT |
= 2b2(1 + lc)α
pA21 − 4B1 + 2lc − A1
−l
!
xT
≥ l|xT | = l(1 + λ1)T |x0| = 2ε(1 + λ1)T ,
where the second equality uses (5.2) and the definition of y0, and the second inequality follows from
2b2(1 + lc)α
pA21 − 4B1 + 2lc − A1
> 2b2(1 + lc)α
2(lc − A1) = 3 + 3lc
1 + 3lc · l > 2l,
using the fact that 3lc = Θ ε2
l2 D2
Y
< 1.
Therefore, we have
(1 + λ1)T ≤ 1
2 =⇒ T = Ω 1
|λ1| = Ω l2D2
Y ε2
!
,
where the last equality follows from Lemma 5.1. Finally, Lemma A.1 gives ∆Ψ1 = O ε2
l under our
initialization, and thus
T = Ω l2D2
Y ε2
!
= Ω l2D2
Y
ε2 · ∆Ψ1
ε2/l
!
= Ω l3D2
Y ∆Ψ1 ε4
!
.
This completes the proof. ■
5.1.2 Perturbed Smoothed GDA
In this subsection, we prove Theorem 3.4 (i) with a similar approach as that for Perturbed GDA. We analyze the performance of Perturbed Smoothed GDA on the hard instance Example 5.1 and derive a lower bound on the number of iterations it needs to reach an ε-GS. The main technical difficulty here is that Perturbed Smoothed GDA introduces an additional auxiliary variable z (via smoothing), so the middle-branch (when
0 ≤ x ≤ ryDY
b ) updates of Example 5.1 are governed by a three-dimensional linear recursion. Consequently, unlike Section 5.1.1 where one only needs to control the spectrum of a 2 × 2 matrix, we must analyze the eigenvalues of a 3 × 3 matrix that couples (xt, yt, zt). Analyzing the contraction factor of this 3 × 3 matrix is the crux of the proof.
23

In the following, we choose the step sizes α, c, β to satisfy Condition 3.2 and set ry = Θ( ε
DY ). We
show that any iterate (xT , yT , zT ) generated by Perturbed Smoothed GDA that is an ε-GS must satisfy
T = Ω l2DY ∆Ψ2
ε3 .
We analyze the dynamics of Perturbed Smoothed GDA on Example 5.1 in the middle branch xt ∈ 0, ryDY
b, where the update takes the linear form:

   
xt+1
yt+1
zt+1

   
= (I + M2)

   
xt
yt
zt

   
, where M2 =

   
cl − crx −cb crx
αb(1 + cl − crx) −ryα − cαb2 crxαb
β(1 + cl − crx) −cbβ −β + βcrx

   
.
Similar to the previous subsection, we study the eigenvalues of M2 (equivalently, those of I + M2) to quantify the contraction rate along a specific eigendirection. In particular, one eigenvalue admits the following closed form,1
λ2 = 1
3 B2 + 1 + i√3 B22 + 3C2
3 · 22
3D
1 3 2
+1
6 · 21
3
(1 − i√3)D
1 3
2 , (5.3)
where
A2 := − 2c3l3 + 6c3l2rx − 6c3lr2
x + 2c3r3
x + 9b2c2lα
+6b2c3l2α − 9b2c2rxα + 12b2c3lrxα − 6b2c3r2
xα
−9b4c2α2 − 6b4c3lα2 + 6b4c3rxα2 + 2b6c3α3 − 3c2l2β
−3c2lrxβ − 6c3l2rxβ + 6c2r2
xβ + 12c3lr2
xβ − 6c3r3
xβ
+18b2cαβ + 6b2c2lαβ + 12b2c2rxαβ + 12b2c3lrxαβ
−12b2c3r2
xαβ − 3b4c2α2β − 6b4c3rxα2β + 3clβ2
+6crxβ2 + 3c2lrxβ2 − 12c2r2
xβ2 − 6c3lr2
xβ2 + 6c3r3
xβ2
−3b2cαβ2 − 3b2c2rxαβ2 + 6b2c3r2
xαβ2 + 2β3
−6crxβ3 + 6c2r2
xβ3 − 2c3r3
xβ3 − 3c2l2αry
+6c2lrxαry − 3c2r2
xαry − 9b2cα2ry − 3b2c2lα2ry
+3b2c2rxα2ry + 6b4c2α3ry − 12clαβry − 6crxαβry
−6c2lrxαβry + 6c2r2
xαβry − 6b2cα2βry − 3b2c2rxα2βry
−3αβ2ry + 6crxαβ2ry − 3c2r2
xαβ2ry
+3clα2r2
y − 3crxα2r2
y + 6b2cα3r2
y − 3α2βr2
y
+3crxα2βr2
y + 2α3r3
y,
B2 := cl − crx − b2cα − β + crxβ − αry,
1The expression and the subsequent algebraic simplifications were obtained using symbolic computations in Mathematica. The corresponding code is available at https://github.com/Smoothing-Meets-Perturbation/ A-Unified-and-Tight-Analysis-for-NC-C.git.
24

C2 := − b2cα + clβ − b2cαβ + clαry − crxαry − αβry + crxαβry, and
D2 := A2 +
q
A22 − 4(B22 + 3C2)3.
Similar to Section 5.1.1, we summarize the sign and order of the eigenvalue λ2 in the next lemma and its proof is deferred to Appendix A.6.
Lemma 5.3. The eigenvalue λ2 satisfies that λ2 < 0 and |λ2| = Θ ε
lDY .
Remark 5.2. In the analysis for Perturbed GDA, we have |λ1| = Θ ε2
l2 D2
Y
(cf. Lemma 5.1), whereas for
Perturbed Smoothed GDA, we obtain |λ2| = Θ ε
lDY (cf. Lemma 5.3). Note that the contraction factor is
1 + λi = 1 − Θ(|λi|) (i = 1, 2), and a larger |λ2| leads to faster contraction. Thus these two algorithms have different dependence on ε.
Next, to explicitly characterize the iterates, we choose (x0, y0, z0) so that its direction aligns with an eigenvector of I + M2 corresponding to the eigenvalue λ2.
Lemma 5.4. Suppose that Perturbed Smoothed GDA is initialized at
x0 = 2(β + λ2)ε
rxλ2
, y0 = clβ + clλ2 − crxλ2 − βλ2 + crxβλ2 − λ22
bc(β + λ2) · x0, z0 = β(1 + λ2)
β + λ2
· x0. (5.4)
Then, for all t ∈ N, the iterates satisfy xt ∈ [0, ryDY
b ] and yt ∈ [0, DY ]. Moreover, we have
xt = (1 + λ2)tx0, yt = (1 + λ2)ty0, and zt = (1 + λ2)tz0.
The proof of Lemma 5.4 is given in Appendix A.7. With Lemmas 5.3 and 5.4, we can prove Theorem 3.4 (i). Because the argument is almost the same as the proof of Theorem 3.2 (i), we defer the details to Appendix A.9.
5.2 Optimization Stationarity
With the tightness results for game stationarity established, we now turn to optimization stationarity. In this subsection, we prove Theorem 3.2 (ii) and Theorem 3.4 (ii), showing that the iteration complexities for finding an ε-OS in Theorem 3.1 (ii) and Theorem 3.3 (ii) are tight. We first present a hard instance, and then show that Perturbed GDA and Perturbed Smoothed GDA require at least the stated numbers of iterations to reach an ε-OS on this instance.
Example 5.2. Let X = R and Y = [0, DY ]. Define f : X × Y → R by
f (x, y) = h(x) y,
where
h(x) =

  
  
l
2(DY +1) x2, if |x| ≤ 1,
l
DY +1 − l
2(DY +1) (|x| − 2)2, if 1 < |x| < 2,
l
DY +1 , if |x| ≥ 2.
(5.5)
Remark 5.3. The construction in Example 5.2 is adapted from the hard instance in (Mahdavinia et al., 2022, Appendix B.3.1). We rescale the coefficients so that the resulting function satisfies the l-smoothness condition. The smoothness verification is provided in Lemma A.3.
25

5.2.1 Perturbed GDA
We prove Theorem 3.2 (ii) in this subsection. Based on Example 5.2, we show that Perturbed GDA requires at least the stated number of iterations in Theorem 3.1 (ii) to find an ε-OS. Our analysis relies on the fact established in Lemma 5.5: Under a suitable initialization and step-size choice, the iterates satisfy |xt| ≤ 1 and yt ≥ 0 for all t. This is useful because, by (5.5), h is convex on |x| ≤ 1, and therefore f (x, yt) = yth(x) is convex in x when yt ≥ 0.
Lemma 5.5. Let the Perturbed GDA be initialized at (x0, y0) with |x0| ≤ 1 and y0 ≥ 0. We choose the step sizes to satisfy Condition 3.1. Then, when the Perturbed GDA is applied on Example 5.2, for all t ≥ 0,
(i) |xt| ≤ 1 and yt ≥ 0;
(ii) |xt| ≥ 1 − lcDY
DY +1
t
|x0|.
Proof of Lemma 5.5. We prove the two claims by induction using the update rule of Perturbed GDA on Example 5.2. The claims hold for t = 0 by the hypothesis. Suppose for t ≥ 0, we have |xt| ≤ 1. Then by (5.5) we have h′(xt) = l
DY +1 xt, and therefore
xt+1 = xt − c∇xf0(xt, yt) = xt − c h′(xt) yt = 1 − lc
DY + 1 yt xt. (5.6)
(i) First of all, we have yt ≥ 0 for all t since Y = [0, DY ]. We next prove |xt| ≤ 1 for all t by induction.
Assume |xt| ≤ 1 for some t ≥ 0. Using 0 ≤ yt ≤ DY and the step-size condition 0 < c ≤ ry2α
16l2 with
α≤ 1
4(l+ry) , we have
0 ≤ lc
DY + 1 yt ≤ lc
DY + 1 DY ≤ lc ≤ ry2α
16l ≤ ry2
64l(l + ry) < 1,
where the last inequality follows from the choice of ry = Θ( ε2
lD2
Y
). Then, it implies 0 ≤ 1 − lc
DY +1 yt ≤ 1.
Combining this with (5.6) yields |xt+1| ≤ |xt| ≤ 1. This completes the induction. (ii) Since 0 ≤ yt ≤ DY , from (5.6) we have
|xt+1| = 1 − lc
DY + 1 yt |xt| ≥ 1 − lcDY
DY + 1 |xt|.
Iterating the above inequality for t steps gives (ii). This completes the proof. ■
With Lemma 5.5, we are ready to prove Theorem 3.2 (ii).
Proof of Theorem 3.2 (ii). By Definition 2.1 (i) , we have that 2l|prox 1
2l Φ(x) − x| ≤ ε when x is an ε-OS.
Moreover, by (Davis and Drusvyatskiy, 2019, Lemma 2.2), we have
2l prox 1
2l Φ(x) − x = ∇Φ 1
2l
(x) .
When |x| ≤ 1, a direct computation yields that
Φ1
2l
(x) = min
z∈R Φ(z) + l(z − x)2 = min
z∈R
lDY
2(DY + 1) z2 + l(z − x)2 = lDY
3DY + 2 x2. (5.7)
26

We fix ε > 0 small enough so that x0 := 3DY+2
lDY ε ≤ 1, and run Perturbed GDA on Example 5.2 initialized at
(x0, y0) with y0 = DY . If xT is an ε-OS, then
ε ≥ ∇Φ 1
2l
(xT ) (5.=7) 2lDY
3DY + 2 |xT | ≥ 2lDY
3DY + 2 1 − lcDY
DY + 1
T
|x0| = 2ε 1 − lcDY
DY + 1
T
,
where the second inequality uses Lemma 5.5 (ii). Therefore, we have
1 − lcDY
DY + 1
T
≤1
2 =⇒ T = Ω DY + 1
lcDY
, (5.8)
In Lemma A.5, we show that under the initialization (x0, y0) = ( 3DY+2
lDY ε, DY ), the initial gap satisfies
∆Ψ1 = O DY +1
lDY ε2 . Substituting this bound into (5.8) yields
T = Ω DY + 1
lcDY
= Ω DY + 1
lcDY
· lDY ∆Ψ1
(DY + 1)ε2 = Ω l5D4
Y ∆Ψ1 ε6
!
,
where the last equality follows from c = Θ ry2
l3 = Θ ε4
l5 D4
Y
. This completes the proof. ■
5.2.2 Perturbed Smoothed GDA
In this subsection, we use Example 5.2 to prove Theorem 3.4 (ii), showing that the iteration complexity bound for Perturbed Smoothed GDA in Theorem 3.3 (ii) is tight. The key idea here differs from that in Section 5.1.2. A direct analysis of the coupled three-variable dynamics (xt, yt, zt) and their spectral properties is technically cumbersome. To address this issue, we restrict attention to trajectories for which xt remains in the region |xt| ≤ 1. Moreover, we introduce an auxiliary (comparison) sequence in which the dual variable is frozen on the boundary, i.e., yt ≡ DY for all t. This modification yields a simplified update rule that is no slower than Perturbed Smoothed GDA in terms of reaching an ε-OS solution. Consequently, a lower bound on the iteration complexity of this auxiliary process also applies to Perturbed Smoothed GDA.
When |xt| ≤ 1, substituting h′(xt) = l
DY+1 xt into the Perturbed Smoothed GDA update yields the equivalent recursion:
xt+1 = 1 − crx − lc
DY + 1 yt xt + crxzt,
yt+1 = proj[0,DY ] yt + α(h(xt+1) − ryyt) , and
zt+1 = β 1 − crx − lc
DY + 1 yt xt + (1 − β + βcrx)zt.
To formalize the comparison argument, we introduce an auxiliary sequence {x ̄t, y ̄t, z ̄t)}t≥0 that follows the same recursion as Perturbed Smoothed GDA except that the dual iterate is frozen on the boundary. Specifically, we initialize (x ̄0, y ̄0, z ̄0) = (x0, y0, z0) and for all t ≥ 0, we define
x ̄t+1 = 1 − crx − lc
DY + 1 DY x ̄t + crxz ̄t,
y ̄t+1 = DY ,
z ̄t+1 = β 1 − crx − lc
DY + 1 DY x ̄t + (1 − β + βcrx) z ̄t.
(5.9)
27

The following lemma shows the relationship between the auxiliary sequence {(x ̄t, y ̄t, z ̄t)}t≥0 and the original iterates {(xt, yt, zt)}t≥0.
Lemma 5.6. Suppose that the initialization (x0, y0, z0) satisfies 0 < x0, z0 < 1 and y0 = DY . Then, for all t ≥ 0, it holds that
(i) 0 < x ̄t ≤ xt ≤ 1,
(ii) 0 ≤ yt ≤ y ̄t = DY ,
(iii) 0 < z ̄t ≤ zt ≤ 1.
Proof of Lemma 5.6. We prove the claim by induction. For the base case t = 0, the claim holds by construction (the auxiliary sequence shares the same initialization as Perturbed Smoothed GDA). Assume that the claim holds for some t ≥ 0. We next show that it also holds for t + 1. (i) We first show that x ̄t+1 > 0. By the update rule of x ̄t+1, it suffices to verify that (1−crx− lc
DY +1 DY ) > 0.
Recall from Condition 3.2 that c < 1
rx+l+ry . Therefore,
1 − crx − lc
DY + 1 DY ≥ 1 − c(rx + l) > 0,
which proves x ̄t+1 > 0. Next, we prove that x ̄t+1 ≤ xt+1. Using the update rule of xt+1, we have
xt+1 = 1 − crx − lc
DY + 1 yt xt + crxzt
≥ 1 − crx − lc
DY + 1 DY x ̄t + crxz ̄t = x ̄t+1,
where the inequality follows from yt ≤ DY , and the induction hypothesis x ̄t ≤ xt and z ̄t ≤ zt. We now show that xt+1 ≤ 1. Since 0 < xt, zt ≤ 1 and yt ≥ 0, it holds that
xt+1 = 1 − crx − lc
DY + 1 yt xt + crxzt
≤ 1 − crx − lc
DY + 1 yt · 1 + crx · 1 = 1 − lc
DY + 1 yt ≤ 1.
(ii) Since y ̄t = DY for all t, the bound 0 ≤ yt ≤ y ̄t = DY holds by the projection in the y-update rule. (iii) The proof of 0 < z ̄t ≤ zt ≤ 1 follows the same argument as in part (i), and is therefore omitted. This completes the proof. ■
Next, we analyze the auxiliary update rule and prove a lower bound on its iteration complexity, which in turn implies a lower bound on the iteration complexity of Perturbed Smoothed GDA on Example 5.2. Note that y ̄t remains constant under the auxiliary update rule (5.9). Hence, it suffices to study the evolution of the pair (x ̄t, z ̄t). Define γ := DY
DY+1 and introduce the matrix
M3 :=


−lcγ − crx crx
β(1 − lcγ − crx) −β + βcrx

.
28

Then the recursion in (5.9) can be written as


x ̄t+1
z ̄t+1

 = (I + M3)


x ̄t
z ̄t

.
To lower bound the iteration complexity of the updates rule in (5.9), we study the eigenvalues of M3, which governs the growth rate of the iterates. One can verify that M3 admits an eigenvalue
λ3 = − A3 + B3β
2 +1
2
p(A3 + B3β)2 − 4lcβγ, (5.10)
where
A3 := lcγ + crx, and B3 := 1 − crx.
The following Lemmas 5.7 and 5.8 summarize the magnitude of λ3 and the structure of its associated eigenvector. Their proofs are deferred to Appendices A.12 and A.13, respectively.
Lemma 5.7. The eigenvalue λ3 satisfies that λ3 < 0 and |λ3| = Θ γε2
l2 D2
Y
.
Lemma 5.8. Let v = (v1, 1)⊤ be an eigenvector of M3 associated with the eigenvalue λ3. Then
v1 = rx
rx + γl + O(β). (5.11)
Equipped with these lemmas, we are now ready to prove Theorem 3.4 (ii).
Proof of Theorem 3.4 (ii). We consider Perturbed Smoothed GDA initialized at (x0, y0, z0), where the pair (x0, z0) is chosen as an eigenvector associated with the eigenvalue λ3, and y0 = DY . In the following analysis,
only the x-component of this eigenvector is needed. Specifically, x0 = 3DY+2
lDY ε. Let T denote the first
iteration such that xT is an ε-OS point. By definition of an ε-OS point, we have ε ≥ |∇Φ 1
2l
(xT )|. Therefore,
ε ≥ ∇Φ 1
2l
(xT )
= 2lDY
3DY + 2 |xT | ≥ 2lDY
3DY + 2 |x ̄T | ≥ 2lDY
3DY + 2 (1 + λ3)T |x ̄0| = 2ε (1 + λ3)T ,
where the first equality follows from (5.7), the second inequality follows from Lemma 5.6, and the third inequality follows from the fact that (x ̄0, z ̄0) is an eigenvector corresponding to λ3. It implies that
(1 + λ3)T ≤ 1
2 =⇒ T = Ω 1
|λ3| ,
Moreover, by Lemma A.6, under the initialization y0 = DY and (x0, z0) chosen as an eigenvector associated
with λ3 with x0 = 3DY +2
lDY ε, we have ∆Ψ2 = O ε2
lγ . Combining this estimate with Lemma 5.7, we further
obtain
T =Ω 1
|λ3| = Ω l2D2
Y
γε2 · lDY
(DY + 1)ε2 · ∆Ψ2
!
= Ω l3D2
Y ∆Ψ2 ε4
!
.
This completes the proof. ■
29

6 Double-loop Algorithm: Perturbed Smoothed FOAM
Our analysis for the iteration complexity of Perturbed Smoothed GDA on ε-GS shows the synergistic effect between smoothing and perturbation. In this section, we show that this synergy between smoothing and perturbation is actually structural to the NC-C minimax problem and can extend naturally to a double-loop algorithmic framework as well. Specifically, we develop a double-loop Perturbed Smoothed FOAM algorithm that uses both smoothing and perturbation techniques. We prove that its iteration complexity of finding an ε-GS and an ε-OS point is O ε−2.5 log( 1
ε ) and O ε−3 log( 1
ε ) , respectively. To the best of our knowledge, these rates are state-of-the-art among all first-order methods for NC-C minimax problems. The idea of Perturbed Smoothed FOAM is as follows. We employ the same surrogate function Ft defined in (3.1) as in Perturbed Smoothed GDA. Importantly, the combination of smoothing and dual perturbation transforms the original NC-C problem into a sequence of strongly convex-strongly concave (SC-SC) subproblems. In particular, for any fixed z, the function Ft(·, ·, z) is strongly convex in x and strongly concave in y. Unlike Perturbed Smoothed GDA, which performs gradient updates on (xt, yt, zt) in each iteration, Perturbed Smoothed FOAM adopts a double-loop scheme. Specifically, at each iteration t, we first fix zt and compute (xt+1, yt+1) as an approximate solution to the SC-SC subproblem
pt(zt) = min
x∈X max
y∈Y Ft(x, y, zt), (6.1)
and then perform a gradient step on zt. To solve the inner SC-SC problem on x and y, we use the single-loop algorithm FOAM proposed by (Kovalev and Gasnikov, 2022). Specifically, we let FOAM(h, x0, y0, μx, μy, δ) denote the output of FOAM algorithm applied to a SC-SC function h. Here, (x0, y0) is the initial point, and μx and μy denote the strong convexity modulus of h(x, y) in x and the strong concavity modulus of h(x, y) in y, respectively. FOAM returns a δ-accurate solution (cf. (Kovalev and Gasnikov, 2022, Definition 1)) in
O √κxκy log(δ−1) iterations, where κx and κy are the condition numbers of h with respect to x and y, respectively. We formally present Perturbed Smoothed FOAM in Algorithm 4.
Algorithm 4: Perturbed Smoothed FOAM
Data: Initial point (x0, y0, z0), and parameters rx > l, rty > 0, βt ∈ (0, 1), and δt > 0
1 for t = 0, . . . , T do
2 define gt(x, y) := Ft(x, y, zt) + ιX (x) − ιY (y); 3 (xt+1, yt+1) := FOAM gt, xt, yt, rx − l, rty, δt ;
4 zt+1 = zt + βt(xt+1 − zt); 5 end
Next, we establish the iteration complexity of Perturbed Smoothed FOAM. We begin by specifying the step-size conditions for the Perturbed Smoothed FOAM algorithm in Condition 6.1.
Condition 6.1 (Parameter conditions for Perturbed Smoothed FOAM). Let rty > 0 for all t ≥ 0. The step sizes are chosen to satisfy
δt = O (rty)4D2
Y l4
!
, rx = Θ(l) > 3l, βt = Θ(1), and 1 − βt = Θ(1).
We prove the iteration complexity of Algorithm 4 using the unified analysis framework developed in Section 4. In this setting, we use pt(z) defined in (6.1) as the Lyapunov function and establish its basic descent property in the following lemma.
30

Lemma 6.1 (Basic descent estimate). Suppose Condition 6.1 holds, and let {(xt, yt, zt)}t≥0 be the sequence generated by Perturbed Smoothed FOAM. Then, for any t ≥ 0, we have
pt+1(zt+1) − pt(zt) ≤ − βt(1 − βt)
rx
∥∇pt(zt)∥2 + βt∥∇pt(zt)∥ · xt+1 − x⋆
t (zt)
+ rx(βt)2 xt+1 − x⋆
t (zt) 2 + rty − rty+1
2 D2
Y.
Proof of Lemma 6.1. We bound the descent of pt as follows:
pt+1(zt+1) − pt(zt) = (pt+1(zt+1) − pt(zt+1)) + (pt(zt+1) − pt(zt)). (6.2)
We first bound the first term on the right-hand side (RHS). Since
Ft+1(x, y, z) = Ft(x, y, z) + rty − rty+1
2 ∥y∥2,
and rty ≥ rty+1, for any fixed z we have
pt+1(z) = min
x∈X max
y∈Y
(
Ft(x, y, z) + rty − rty+1
2 ∥y∥2
)
≤ min
x∈X
(
max
y∈Y Ft(x, y, z) + rty − rty+1
2 D2
Y
)
= pt(z) + rty − rty+1
2 D2
Y.
Applying this bound with z = zt+1 gives
pt+1(zt+1) − pt(zt+1) ≤ rty − rty+1
2 D2
Y.
For the second term on the RHS of (6.2), by (Bauschke and Combettes, 2011, Proposition 12.29), the function pt is rx-smooth. Consequently,
pt(zt+1) ≤ pt(zt) + ⟨∇pt(zt), zt+1 − zt⟩
| {z }
1
+ rx
2 ∥zt+1 − zt∥2
| {z }
2
.
We next bound the two terms 1 and 2 separately. For term 1, we use the update rule of zt+1,
⟨∇pt(zt), zt+1 − zt⟩ = βt⟨∇pt(zt), xt+1 − zt⟩
= βt ∇pt(zt), x⋆
t (zt) − zt + βt ∇pt(zt), xt+1 − x⋆
t (zt)
≤ − βt
rx
∥∇pt(zt)∥2 + βt∥∇pt(zt)∥ · xt+1 − x⋆
t (zt) ,
where the inequality follows the fact that ∇pt(z) = rx(z − xt⋆(z)) and the Cauchy-Schwarz inequality. We now bound term 2,
rx
2 ∥zt+1 − zt∥2 = rx(βt)2
2 ∥zt − xt+1∥2
31

≤ rx(βt)2 xt+1 − x⋆
t (zt) 2 + x⋆
t (zt) − zt 2
≤ rx(βt)2 xt+1 − x⋆
t (zt) 2 + (βt)2
rx
∥∇pt(zt)∥2,
where the first equality follows from the z-update rule, the first inequality follows from the elementary bound (a + b)2 ≤ 2(a2 + b2) for any a, b ∈ R, and the last inequality follows from the identity ∇pt(z) = rx(z − xt⋆(z)).
Putting everything together yields
pt+1(zt+1) − pt(zt) ≤ − βt(1 − βt)
rx
∥∇pt(zt)∥2 + βt∥∇pt(zt)∥ · xt+1 − x⋆
t (zt)
+ rx(βt)2 xt+1 − x⋆
t (zt) 2 + rty − rty+1
2 D2
Y.
This completes the proof.
■
Following the analysis framework in Section 4, after establishing the basic descent estimate in Lemma 6.1, it remains to control the positive term involving ∥xt+1 − xt⋆(zt)∥ in Lemma 6.1. In single-loop algorithms, such error terms are typically controlled indirectly via primal–dual error bounds, e.g., see Lemma 4.2 and Lemma 4.5. In contrast, for the double-loop scheme, the quantity ∥xt+1 − xt⋆(zt)∥ directly corresponds to the inaccuracy in solving the inner SC-SC subproblem. In other words, by the definition of δ-accurate
solution, we directly have ∥xt+1 − xt⋆(zt)∥ ≤ √δt. In the next theorem, we derive the iteration complexity of Perturbed Smoothed FOAM.
Theorem 6.1 (Iteration complexity of Perturbed Smoothed FOAM). Let {(xt, yt, zt)}t≥0 be the sequence generated by Perturbed Smoothed FOAM. For any ε > 0, if the parameters satisfy Condition 6.1 and ∆p0 := p0(z0) − minx∈X maxy∈Y f0(x, y) denote the initial gap. Then
(i) (Game Stationarity) Let ry = Θ ε
DY and set rty = ry for all t ≥ 0, then we can obtain an ε-GS of
problem (1.1) after O l1.5D0.5
Y ∆p0
ε2.5 log 1
ε gradient steps.
(ii) (Optimization Stationarity) Let ry = Θ ε2
lD2
Y
and set rty = ry for all t ≥ 0, then we can obtain an
ε-OS of problem (1.1) after O l2DY ∆p0
ε3 log 1
ε gradient steps.
Proof of Theorem 6.1. By Algorithm 4, for each iteration t ≥ 0, the pair (xt+1, yt+1) is a δt-accurate solution of Ft(·, ·, zt). By definition, this implies that
xt+1 − x⋆
t (zt) 2 + ∥yt+1 − yt(zt)∥2 ≤ δt. (6.3)
Together with Lemma 6.1, it yields that
pt+1(zt+1) − pt(zt)
≤ − βt(1 − βt)
rx
∥∇pt(zt)∥2 + βt√
δt∥∇pt(zt)∥ + rx(βt)2δt + rty − rty+1
2 D2
Y
≤ − βt(1 − βt)
2rx
∥∇pt(zt)∥2 + βt
2(1 − βt) + (βt)2 rxδt + rty − rty+1
2 D2
Y , (6.4)
32

where the second inequality follows from ab ≤ 1
2 (a2 + b2) for all a, b ∈ R. Under Condition 6.1, we choose
βt = Θ(1) and 1 − βt = Θ(1), and set δt = O (rty)4D2
Y
l4 .
If we further choose rty = O( √1T ), then the second term in (6.4) is of order O(rxδt) = O( 1
T 2 ), and
hence its cumulative contribution over T iterations is O( 1
T ).
Using the facts that rty = r0y for all t ≥ 0 and p0(zt) = minx∈X maxy∈Y F0(x, y, zt) ≥ minx∈X maxy∈Y f0(x, y), we sum (6.4) over t = 0, . . . , T − 1 to obtain
min
t∈{⌊ T
2 ⌋,··· ,T −1}
∥∇pt(zt)∥ = O
s
rx∆p0
βt(1 − βt)T
!
=O
r l∆p0
T
!
. (6.5)
Let t⋆ ∈ arg mint∈{⌊ T
2 ⌋,...,T −1} ∥∇pt(zt)∥. Then (6.5) implies that
∥xt⋆+1 − zt⋆ ∥ ≤ xt⋆+1 − x⋆
t⋆ (zt⋆ ) + x⋆
t⋆ (zt⋆ ) − zt⋆
≤
√
δt⋆ + ∥∇pt⋆ (zt⋆ )∥
rx
=O
r ∆p0 lT
!
, (6.6)
where the second inequality follows from (6.3), and the last equality is due to (6.5) and Condition 6.1.
Then, we consider one projected gradient update starting from (xt⋆+1, yt⋆+1, zt⋆), defined by
xˆ := projX xt⋆+1 − α∇xFt⋆ (xt⋆+1, yt⋆+1, zt⋆ ) , (6.7)
yˆ := projY yt⋆+1 + c∇yFt⋆ (xt⋆+1, yt⋆+1, zt⋆ ) , (6.8)
where α = Θ( 1
l ), c = Θ( 1
l ) with 0 < α ≤ 1
rx+l , 0 < c ≤ 1
rt⋆
y +l . Using Lemma A.7, we have that (xˆ, yˆ) is
close to (xt⋆+1, yt⋆+1), namely,
∥xt⋆+1 − xˆ∥ = O
√
δt⋆ , and ∥yt⋆+1 − yˆ∥ = O l
rt⋆
y
√
δt⋆ . (6.9)
We therefore analyze GS and OS stationarity at (xˆ, yˆ).
(i) For the GS case, we choose rty = Θ
r
l∆p0
D2
YT . For the primal stationarity measure, we have
dist(0, ∇xf (xˆ, yˆ) + ∂ιX (xˆ))
= dist 0, ∇xFt⋆(xˆ, yˆ, zt⋆) − rx(xˆ − zt⋆) + ∂ιX (xˆ)
≤ dist 0, ∇xFt⋆(xˆ, yˆ, zt⋆) + ∂ιX (xˆ) + rx∥xˆ − zt⋆∥
≤ dist 0, ∇xFt⋆ (xt⋆+1, yt⋆+1, zt⋆ ) + ∂ιX (xˆ) + rx∥xˆ − zt⋆ ∥
+ ∇xFt⋆ (xˆ, yˆ, zt⋆ ) − ∇xFt⋆ (xt⋆+1, yt⋆+1, zt⋆ )
≤1
α ∥xt⋆+1 −xˆ∥+rx∥xt⋆+1 −xˆ∥+rx∥xt⋆+1 −zt⋆ ∥+(rx + l)(∥xt⋆+1 − xˆ∥+∥yt⋆+1 − yˆ∥)
= O max
r l∆p0
T , l2
rt⋆
y
√
δt⋆
!!
=O
r l∆p0
T
!
, (6.10)
33

where the third inequality is due to (6.7) and (rx +l)-Lipschitz continuity of ∇xFt⋆(·, ·, z), the second equality
follows from (6.6) and (6.9), and the last equality is owing to the choice of rt⋆
y and δt⋆ = O (rt⋆
y )4D2
Y
l4 .
We now turn to the dual part. By a similar argument, we have
dist(0, −∇yf (xˆ, yˆ) + ∂ιY (yˆ))
= dist 0, −∇yFt⋆(xˆ, yˆ, zt⋆) − rt⋆
y yˆ + ∂ιY (yˆ)
+ ∇yFt⋆ (xˆ, yˆ, zt⋆ ) − ∇yFt⋆ (xt⋆+1, yt⋆+1, zt⋆ )
≤1
c ∥yt⋆+1 − yˆ∥ + rt⋆
y ∥yˆ∥ + (rt⋆
y + l)(∥xt⋆+1 − xˆ∥ + ∥yt⋆+1 − yˆ∥)
= O max l2
rt⋆
y
√
δt⋆ , rt⋆
y DY = O
r l∆p0
T
!
. (6.11)
Thus, to reach an ε-GS solution, it suffices to take T = O l∆p0
ε2 outer iterations. Substituting this choice
of T into the parameter selection in Condition 6.1 yields the equivalent scaling rty = Θ ε
DY . Moreover,
the number of inner iterations required is O
q l2
(rx−l)rt⋆
y
log 1
δt⋆ = O
q lDY
ε log 1
ε . Consequently, the
overall oracle complexity is
T = O l∆p0
ε2 ·
r lDY
ε log 1
ε
!
= O l1.5D0.5
Y ∆p0
ε2.5 log 1
ε
!
.
(ii) For the OS case, we choose rty = O ∆p0
D2
YT . Due to Lemma 4.3, (6.10) and (6.11), we have
∥prox 1
2l Φ(xˆ) − xˆ∥2
≤ 2DY
l dist(0, −∇yf (xˆ, yˆ) + ∂ιY (yˆ)) + 1
l2 dist2(0, ∇xf (xˆ, yˆ) + ∂ιX (xˆ))
= O DY
l · rt⋆
y DY + lDY
rt⋆
y
√
δt⋆ + 1
l2
l∆p0
T = O ∆p0
lT .
Therefore, to achieve an ε-OS solution, it suffices to take T = O l∆p0
ε2 outer iterations. Substituting this
choice of T into the parameter selection in Condition 6.1 yields the equivalent scaling rty = Θ ε2
lD2
Y
. Addi
tionally, the number of inner iterations required is O
q l2
(rx−l)rt⋆
y
log 1
δt⋆ = O lDY
ε log 1
ε . Consequently,
the overall oracle complexity is
T = O l∆p0
ε2 · lDY
ε log 1
ε = O l2DY ∆p0
ε3 log 1
ε.
This completes the proof.
■
34

7 Closing Remarks
This paper provides a unified framework to analyze smoothing and perturbation in NC-C minimax optimization. Rather than viewing these techniques as interchangeable acceleration tools, our framework clarifies their distinct algorithmic roles and explains when and how they interact to yield sharper convergence guarantees. This separation leads to a more transparent analysis of existing first-order methods and provides deeper understanding to these acceleration tools, which enables us to unify several seemingly disparate convergence results in the literature. An interesting direction for future research is to investigate whether the structural insights developed here—particularly the decoupling of smoothing and perturbation and the associated primal–dual viewpoint—can be exploited to obtain stronger or more general convergence guarantees. Recent works such as (Zheng et al., 2024, 2025) and (Lu and Wang, 2025) establish convergence results under broad global or local Kurdyka–Łojasiewicz–type assumptions. It would be of interest to explore whether our framework can be used to streamline these analyses or to clarify the algorithmic roles of smoothing and perturbation under such general conditions.
References
Arjovsky, M., Chintala, S., and Bottou, L. (2017). Wasserstein generative adversarial networks. In Proceedings of the 34th International Conference on Machine Learning (ICML 2017), pages 214–223. PMLR.
Aybat, N. S., Hu, J., and Deng, Z. (2025). A retraction-free method for nonsmooth minimax optimization over a compact manifold. arXiv preprint arXiv:2510.22065.
Bauschke, H. and Combettes, P. (2011). Convex Analysis and Monotone Operator Theory in Hilbert Spaces. CMS Books in Mathematics. Springer New York.
Boţ, R. I. and Böhm, A. (2023). Alternating proximal-gradient steps for (stochastic) nonconvex-concave minimax problems. SIAM Journal on Optimization, 33(3):1884–1913.
Dai, B., Shaw, A., Li, L., Xiao, L., He, N., Liu, Z., Chen, J., and Song, L. (2018). Sbeed: Convergent reinforcement learning with nonlinear function approximation. In Proceedings of the 35th International Conference on Machine Learning (ICML 2018), pages 1125–1134. PMLR.
Davis, D. and Drusvyatskiy, D. (2019). Stochastic model-based minimization of weakly convex functions. SIAM Journal on Optimization, 29(1):207–239.
Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A., and Bengio, Y. (2020). Generative adversarial networks. Communications of the ACM, 63(11):139–144.
Kong, W. and Monteiro, R. D. (2021). An accelerated inexact proximal point method for solving nonconvexconcave min-max problems. SIAM Journal on Optimization, 31(4):2558–2585.
Kovalev, D. and Gasnikov, A. (2022). The first optimal algorithm for smooth and strongly-convex-stronglyconcave minimax optimization. Advances in Neural Information Processing Systems, 35.
Li, J., Zhu, L., and So, A. M.-C. (2025). Nonsmooth nonconvex–nonconcave minimax optimization: Primal–dual balancing and iteration complexity analysis. Mathematical Programming, pages 1–51.
35

Lin, T., Jin, C., and Jordan, M. (2020a). On gradient descent ascent for nonconvex-concave minimax problems. In International Conference on Machine Learning, pages 6083–6093. PMLR.
Lin, T., Jin, C., and Jordan, M. I. (2020b). Near-optimal algorithms for minimax optimization. In Conference on Learning Theory, pages 2738–2779. PMLR.
Lin, T., Jin, C., and Jordan, M. I. (2025). Two-timescale gradient descent ascent algorithms for nonconvex minimax optimization. Journal of Machine Learning Research, 26(11):1–45.
Lu, S., Tsaknakis, I., Hong, M., and Chen, Y. (2020). Hybrid block successive approximation for one-sided non-convex min-max problems: algorithms and applications. IEEE Transactions on Signal Processing, 68:3676–3691.
Lu, Z. and Mei, S. (2026). Solving bilevel optimization via sequential minimax optimization. Mathematics of Operations Research.
Lu, Z. and Wang, X. (2025). A first-order method for nonconvex-nonconcave minimax problems under a local kurdyka-lojasiewicz condition. arXiv preprint arXiv:2507.01932.
Mahdavinia, P., Deng, Y., Li, H., and Mahdavi, M. (2022). Tight analysis of extra-gradient and optimistic gradient methods for nonconvex minimax problems. Advances in Neural Information Processing Systems, 35:31213–31225.
Nesterov, Y. (2013). Gradient methods for minimizing composite functions. Mathematical programming, 140(1):125–161.
Nouiehed, M., Sanjabi, M., Huang, T., Lee, J. D., and Razaviyayn, M. (2019). Solving a class of non-convex min-max games using iterative first order methods. Advances in Neural Information Processing Systems, 32.
Omidshafiei, S., Pazis, J., Amato, C., How, J. P., and Vian, J. (2017). Deep decentralized multi-task multi-agent reinforcement learning under partial observability. In Proceedings of the 34th International Conference on Machine Learning (ICML 2017), pages 2681–2690. PMLR.
Ostrovskii, D. M., Lowy, A., and Razaviyayn, M. (2021). Efficient search of first-order nash equilibria in nonconvex-concave smooth min-max problems. SIAM Journal on Optimization, 31(4):2508–2538.
Pang, J.-S. (1987). A posteriori error bounds for the linearly-constrained variational inequality problem. Mathematics of Operations Research, 12(3):474–484.
Rockafellar, R. (1970). On the maximal monotonicity of subdifferential mappings. Pacific Journal of Mathematics, 33(1):209–216.
Rockafellar, R. T. and Wets, R. J.-B. (2009). Variational Analysis, volume 317 of Grundlehren der mathematischen Wissenschaften. Springer Science & Business Media, second edition.
Sinha, A., Namkoong, H., and Duchi, J. (2018). Certifying some distributional robustness with principled adversarial training. In International Conference on Learning Representations.
Thekumparampil, K. K., Jain, P., Netrapalli, P., and Oh, S. (2019). Efficient algorithms for smooth minimax optimization. Advances in neural information processing systems, 32.
36

Xu, M., Jiang, B., Liu, Y.-F., and So, A. M.-C. (2026). A riemannian alternating descent ascent algorithmic framework for nonconvex-linear minimax problems on riemannian manifolds. Mathematics of Operations Research.
Xu, Z., Zhang, H., Xu, Y., and Lan, G. (2023). A unified single-loop alternating gradient projection algorithm for nonconvex–concave and convex–nonconcave minimax problems. Mathematical Programming, pages 1–72.
Yang, J., Orvieto, A., Lucchi, A., and He, N. (2022). Faster single-loop algorithms for minimax optimization without strong concavity. In International Conference on Artificial Intelligence and Statistics, pages 5485–5517. PMLR.
Zhang, J., Xiao, P., Sun, R., and Luo, Z. (2020). A single-loop smoothed gradient descent-ascent algorithm for nonconvex-concave min-max problems. Advances in neural information processing systems, 33:7377–7389.
Zhang, S. and Hu, Y. (2025). Avoid overclaims: Summary of complexity bounds for algorithms in minimization and minimax optimization. In The Fourth Blogpost Track at ICLR 2025.
Zhao, R. (2024). A primal-dual smoothing framework for max-structured non-convex optimization. Mathematics of Operations Research, 49(3):1535–1565.
Zheng, T., So, A. M.-C., and Li, J. (2025). Doubly smoothed optimistic gradients: A universal approach for smooth minimax problems. arXiv preprint arXiv:2506.07397.
Zheng, T., Zhu, L., So, A. M.-C., Blanchet, J., and Li, J. (2024). Universal gradient descent ascent method for nonconvex-nonconcave minimax optimization. Advances in Neural Information Processing Systems, 36.
A Useful Lemmas
To keep the presentation streamlined, we defer the proof details of technical lemmas to the appendix. In particular, the appendix collects proofs of the lemmas stated in the paper, as well as auxiliary lemmas invoked in the main body.
A.1 Proof of Lemma 4.3
Proof of Lemma 4.3. Our proof builds upon the argument in (Yang et al., 2022, Proposition 2.1). In particular, we extend their analysis from the unconstrained setting to the constrained case. We first compute
l
2 prox 1
2l Φ(x) − x
2
≤ Φ(x) − Φ prox 1
2l Φ(x) − l prox 1
2l Φ(x) − x
2
= Φ(x) − f (x, y)
| {z }
1
+f prox 1
2l Φ(x), y − Φ prox 1
2l Φ(x)
+ f (x, y) − f prox 1
2l Φ(x), y − l prox 1
2l Φ(x) − x
2
| {z }
2
, (A.1)
37

where the first inequality follows from the l-strong convexity of the function Φ(·) + l∥ · −x∥2. Moreover, because f (prox 1
2l Φ(x), y) − Φ(prox 1
2l Φ(x)) ≤ 0, we just focus on the two parts 1 and 2. We begin with
term 1. Note that
Φ(x) − f (x, y) = max
y′∈Y f (x, y′) − f (x, y) ≤ dist(0, −∇yf (x, y) + ∂ιY (y)) · DY , (A.2)
where the inequality follows from the Cauchy–Schwarz inequality and the concavity of the function f (x, ·) + ιY (·) for any x ∈ X . Then, we proceed to consider term 2 in (A.1),
f (x, y) − f prox 1
2l Φ(x), y − l prox 1
2l Φ(x) − x
2
≤ x − prox 1
2l Φ(x) · dist(0, ∇xf (x, y) + ∂ιX (x)) − l
2 prox 1
2l Φ(x) − x
2
≤1
2l dist2(0, ∇xf (x, y) + ∂ιX (x)), (A.3)
where the first inequality follows from the l-strong convexity of f (·, y) + l∥ · −x∥2 + ιX (·), and the second one is owing to ab ≤ 1
2 (a2 + b2) for any a, b ∈ R. Combining (A.1), (A.2) and (A.3) yields
prox 1
2l Φ(x) − x
2
≤ 2DY
l dist(0, −∇yf (x, y) + ∂ιY (y)) + 1
l2 dist2(0, ∇xf (x, y) + ∂ιX (x)).
This completes the proof. ■
A.2 Proof of Lemma 4.5
Proof of Lemma 4.5. The result follows directly from (Li et al., 2025, Corollary 5.1). Since any rty-strongly
concave function is also a KŁ-function with exponent θ = 1
2 and parameter μ =
q
2rty (Li et al., 2025,
Assumption 3.2), we may substitute these values into the expression for ωt = ω2 in (Li et al., 2025, Corollary 5.1). This yields
ωt = 1
q
rty
· 1 + αt(l + rty)(1 + σt2)
αt
q
rx − (l + rty)
=1
q
rty
· (rx − l − rty) + αt(l + rty)(3rx − 2l − 2rty)
αt(rx − l − rty)3/2 ,
where σt2 = 2rx−l−rty
rx−l−rty is given in (Li et al., 2025, Lemma A.1). This completes the proof. ■
A.3 Proof of Lemma 5.1
Proof of Lemma 5.1. Since we choose ry = Θ ε
DY , and c = Θ ε2
l3 D2
Y
, it follows that A1 < 0 and
|A1| = Θ ε
lDY
, B1 = Θ ε3
l3D3
Y
!
, and
q
A21 − 4B1 = Θ ε
lDY
.
38

First, we establish that λ1 is negative. Indeed,
λ1 = 1
2 A1 +
q
A21 − 4B1 = − 2B1
pA21 − 4B1 − A1
< 0.
Then, direct computation yields |λ1| = Θ ε2
l2 D2
Y
. This completes the proof. ■
A.4 Proof of Lemma 5.2
Proof of Lemma 5.2. We prove by induction that for all t ∈ N,
xt ∈ 0, ryDY
b , yt ∈ [0, DY ], and (xt, yt)⊤ = (1 + λ1)t(x0, y0)⊤. (A.4)
Base case (t = 0): By the choice of the initial point x0 = 2ε
l and noting that ryDY
b =Θ
q εDY
l , we have
0 < x0 < ryDY
b . Similarly, since b = Θ
q εl
DY , it follows that
0 < y0 = 2b(1 + lc)α
pA21 − 4B1 + 2lc − A1
· 2ε
l =Θ
r εDY
l
!
, (A.5)
which implies y0 ∈ [0, DY ]. Therefore, (A.4) holds for t = 0. Inductive step: Assume that (A.4) holds for some t ≥ 0. In particular, xt ∈ 0, ryDY
b , so by the definition of f in Example 5.1 (the middle branch), the Perturbed GDA update admits the linear form
(xt+1, yt+1)⊤ = (I + M1)(xt, yt)⊤.
By construction, the initialization (x0, y0)⊤ is an eigenvector of I + M1 associated with the eigenvalue 1 + λ1, which implies that (I + M1)(x0, y0)⊤ = (1 + λ1)(x0, y0)⊤. Hence
(xt+1, yt+1)⊤ = (I + M1)(xt, yt)⊤ = (I + M1) (1 + λ1)t(x0, y0)⊤ = (1 + λ1)t+1(x0, y0)⊤. (A.6)
where the second equality uses the induction hypothesis (xt, yt)⊤ = (1+λ1)t(x0, y0)⊤. Since 0 < 1+λ1 < 1 by Lemma 5.1, we have (1 + λ1)t+1 ∈ (0, 1), and thus
0 ≤ xt+1 ≤ x0 ≤ ryDY
b , 0 ≤ yt+1 ≤ y0 ≤ DY . (A.7)
Combining (A.6) and (A.7), we conclude that (A.4) holds for t + 1, which completes the induction. Consequently, (A.4) holds for all t ∈ N. This completes the proof. ■
A.5 Initial Gap Bound for Perturbed GDA on Example 5.1
Lemma A.1. Suppose that Perturbed GDA is applied to Example 5.1 and initialized as in Lemma 5.2. Then the initial gap satisfies
∆Ψ1 = O ε2
l.
39

Proof of Lemma A.1. Recall in Definition 3.1, we have that
∆Ψ1 = Ψ0
1(x0, y0) − min
x∈R max
y∈[0,DY ]
f0(x, y),
Ψ0
1(x, y) = 2 max
y′∈[0,DY ]
f0(x, y′) − f0(x, y).
By Lemma 5.2, we have x0 ∈ 0, ryDY
b and y0 ∈ [0, DY ].
Since x0 ∈ 0, ryDY
b , by the definition of Example 5.1 and f0(·), we have
f0(x0, y) = − 1
2 l(x0)2 + bx0y − ry
2 y2, ∀ y ∈ [0, DY ].
Direct computation yields that
max
y∈[0,DY ]
f0(x0, y) = − 1
2 l(x0)2 + b2
2ry
(x0)2 = l(x0)2,
where the last equality uses b2 = 3lry in (5.1). Therefore,
Ψ0
1(x0, y0) = 2l(x0)2 − − 1
2 l(x0)2 + bx0y0 − ry
2 (y0)2
=5
2 l(x0)2 − bx0y0 + ry
2 (y0)2 ≤ 5
2 l(x0)2 + ry
2 (y0)2,
where the inequality uses x0 ≥ 0 and y0 ≥ 0. Using x0 = 2ε
l , ry = Θ ε
DY , and y0 = Θ
q εDY
l from
(A.5), we obtain
Ψ0
1(x0, y0) = O l(x0)2 + ry(y0)2 = O ε2
l.
To proceed, one can verify that
Φ0(x) = max
y∈[0,DY ]
f0(x, y) =

 
 
0, x < 0,
lx2, 0 ≤ x ≤ ryDY
b,
ry D2
Y
3 , x > ryDY
b,
and hence
min
x∈R max
y∈[0,DY ]
f0(x, y) = 0. (A.8)
Putting everything together, we obtained that ∆Ψ1 = O ε2
l . This completes the proof. ■
A.6 Proof of Lemma 5.3
Proof of Lemma 5.3. (i) First, we show that λ2 is real. To this end, we verify that 4(B22 + 3C2)3 − A22 ≥ 0. A direct expansion yields
4(B2
2 + 3C2)3 − A2
2
=27c4(rx − l)2 l2β2 − 10rxlαβry + 4l2αβry + r2
xα2r2
y + 4rxlα2r2
y + 4l2α2r2
y
| {z }
=:E2
+O(r3
y ).
40

Since c4(rx − l)2 > 0, it suffices to show that E2 > 0. A straightforward calculation yields the lower bound
E2 = (rxαry − 5lβ)2 − 25l2β2 + 4l2αβry + r2
xα2r2
y + 4rxlα2r2
y + 4l2α2r2
y
≥ (rxαry − 5lβ)2 − 25l2β2 + 4l2α2r2
y. (A.9)
Given our choice of β by Condition 3.2, we see that
β≤ 1
384rxα · ryα2(rx − l − ry)3
((rx − l − ry) + αl(3rx − 2l − 2ry))2
≤1
384rxα · ryα2(rx − l − ry)3
(rx − l − ry)2 = αry
384 · rx − l − ry
rx
≤ αry
384 ,
which implies 25l2β2 ≤ 25
3842 l2α2ry2. Plugging this into (A.9) yields
E2 ≥ (rxαry − 5lβ)2 + 4 − 25
3842 l2α2r2
y > 0,
and hence 4(B22 + 3C2)3 − A22 ≥ 0. Next, we examine the closed-form expression (5.3), which involves the complex quantity
D2 := A2 +
q
4(B22 + 3C2)3 − A22 i.
We show that the two cube-root terms in (5.3) are complex conjugates, which implies that λ2 ∈ R. To this end, we rewrite (5.3) as λ2 = 1
3 B2 + Γ1 + Γ2, where
Γ1 := 1 + i√3 B22 + 3C2
3 · 22
3D
1 3 2
and Γ2 := 1
6 · 21
3
(1 − i√3)D
1 3
2.
To verify the conjugacy, we compute their product and magnitudes. First, Γ1Γ2 = B2
2 +3C2
9 > 0. Moreover, as
4(B22 + 3C2)3 − A22 ≥ 0, we have
|D2| =
q
A22 + 4(B22 + 3C2)3 − A22 = 2
q
B22 + 3C2
3
.
Therefore,
|Γ1| = 2(B22 + 3C2)
6
pB22 + 3C2
=
pB22 + 3C2
3 , and |Γ2| = 2 · 2 1
3
6 · 21
3
q
B22 + 3C2 =
pB22 + 3C2
3 = |Γ1|.
Therefore, Γ1 and Γ2 are complex conjugates, and hence λ2 ∈ R.
(ii) Next, we establish that λ2 < 0 and characterize its magnitude. Since Γ1 and Γ2 are complex conjugates, we can rewrite the eigenvalue as λ2 = 1
3 B2 + 2Re(Γ2). To characterize the magnitude of Re(Γ2),
we first analyze the order of D2. Recall D2 = A2 + p4(B22 + 3C2)3 − A22 i. Using the expansion of the discriminant term and the expression of A2, we have
q
4(B22 + 3C2)3 − A22 = 3√3 c2(rx − l)pE2 + O(r2
y ).
41

Therefore, we have
D2 = A2 +
q
4(B2 + 3C2)3 − A22 i
= −3c2(rx −l) −2rxβ +2cr2
xβ −lβ −2crxlβ +rxαry +8lαry −6crxlαry +6cl2αry
| {z }
=:D2,1
+ i 3√3 c2(rx − l)pE2
| {z }
=:D2,2
+2(crx − lc)3 + O(r2
y). (A.10)
Now, with (A.10), we compute the order of 2Re(Γ2). The direct computation yields
2Re (Γ2) (A.10)
= Re


(1 − √3i) 2(crx − lc)3 + D2,1 + D2,2i + O(ry2)
1 3
3 ∗ 21
3


=1
3 (crx − lc) · Re (1 − √3i) 1 + D2,1 + D2,2i
2(crx − lc)3 + O(r2
y)
1 3
!
=1
3 (crx − lc) · Re (1 − √3i) 1 + D2,1 + D2,2i
6(crx − lc)3 + O(r2
y)
=1
3 (crx − lc) 1 + D2,1 + √3D2,2
6(crx − lc)3 + O(r2
y)
!
=1
3 (crx − lc) + D2,1 + √3D2,2
18(crx − lc)2 + O(r2
y), (A.11)
where the third equality uses the Maclaurin expansion for (1 + ·) 1
3 . Thus, λ2 can be represented in the form of
λ2 = 1
3 B2 + 1
3 (crx − lc) + D2,1 + √3D2,2
18(crx − lc)2 + O(r2
y)
= −3lcαry − β + crxβ − αry
3 +1
6(rx − l) 3pE2 − (−2rxβ + 2cr2
xβ − lβ
−2crxlβ + rxαry + 8lαry − 6crxlαry + 6cl2αry) + O(r2
y)
= lβ − rxαry − 2lαry
2(rx − l) + 1
2(rx − l)
pE2 + O(r2
y)
=1
2(rx − l)
E2 − (lβ − rxαry − 2lαry)2
√E2 + rxαry + 2lαry − lβ + O(r2
y)
= − 8(rx − l)lαβry
2(rx − l) · 1
√E2 + rxαry + 2lαry − lβ + O(r2
y)
= − 4lαβry
√E2 + rxαry + 2lαry − lβ + O(r2
y), (A.12)
where the first equality follows from (A.11), the second equality follows from (A.10), the fourth equality
follows from the definition of E2. In fact, (A.12) implies that λ2 < 0. Moreover, note that √E2 = Θ(ry)
and rxαry + 2lαry − lβ = Θ(ry). Combining Condition 3.2 with (A.12) yields |λ2| = Θ ε
lDY . This
completes the proof. ■
42

A.7 Proof of Lemma 5.4
Proof of Lemma 5.4. We prove by induction that for all t ∈ N,
xt ∈ 0, ryDY
b , yt ∈ [0, DY ], and (xt, yt, zt)⊤ = (1 + λ2)t(x0, y0, z0)⊤. (A.13)
Base case (t = 0): By the initialization in (5.4) and noting that ryDY
b =Θ
q εDY
l , it follows that
0 < x0 < ryDY
b . Similarly, using (5.4) and b = Θ
q εl
DY , one can verify that
0 < y0 = clβ + clλ2 − crxλ2 − βλ2 + bcrxβλ2 − λ22
c(β + λ2) · 2(β + λ2)ε
rxλ2
=Θ
r εDY
l
!
,
which implies that y0 ∈ [0, DY ]. Therefore, (A.13) holds for t = 0. Inductive step: Assume that (A.13) holds for some t ≥ 0. In particular, xt ∈ [0, ryDY
b ]. By the definition
of f in Example 5.1, when xt lies in the middle branch of f (·, y), the Perturbed Smoothed GDA update admits the linear form
(xt+1, yt+1, zt+1)⊤ = (I + M2)(xt, yt, zt)⊤.
By construction, the initialization (x0, y0, z0)⊤ is an eigenvector of I + M2 associated with the eigenvalue 1 + λ2, which implies that (I + M2)(x0, y0, z0)⊤ = (1 + λ2)(x0, y0, z0)⊤. Hence
(xt+1, yt+1, zt+1)⊤ = (I + M2)(xt, yt, zt)⊤ = (I + M2) (1 + λ2)t(x0, y0, z0)⊤
= (1 + λ2)t+1(x0, y0, z0)⊤, (A.14)
where the second equality follows from the induction hypothesis (xt, yt, zt)⊤ = (1 + λ2)t(x0, y0, z0)⊤. Since 0 < 1 + λ2 < 1 by Lemma 5.3, we have (1 + λ2)t+1 ∈ (0, 1), and thus
0 ≤ xt+1 ≤ x0 ≤ ryDY
b , 0 ≤ yt+1 ≤ y0 ≤ DY . (A.15)
Combining (A.14) and (A.15), we conclude that (A.13) holds for t + 1, which completes the induction. Consequently, (A.13) holds for all t ∈ N. This completes the proof. ■
A.8 Initial Gap of Example 5.1 under Perturbed Smoothed GDA
Lemma A.2. Suppose that Perturbed Smoothed GDA on Example 5.1 is initialized in the same way as Lemma 5.4. Then the initial gap satisfies
∆Ψ2 = O ε2
l.
Proof of Lemma A.2. Recall from Definition 3.1 that
∆Ψ2 := Ψ0
2(x0, y0, z0) − min
x∈R max
y∈[0,DY ]
f0(x, y).
43

Next, by (5.4) we have x0 = Θ( ε
l ), z0 = Θ( ε
l ), and y0 = Θ( ε
b ), and moreover x0 ∈ [0, ryDY
b ]. Therefore,
F0(x0, y0, z0) = − 1
2 l(x0)2 + bx0y0 − 1
2 ry(y0)2 + rx
2 (x0 − z0)2,
d0(y0, z0) = − (by0 − rxz0)2
2(rx − l) + rx
2 (z0)2 − ry
2 (y0)2, and
p0(z0) = lrx(z0)2
rx + 2l .
Therefore, we can bound Ψ02(x0, y0, z0) with
Ψ0
2(x0, y0, z0)
= F0(x0, y0, z0) − 2d0(y0, z0) + 2p0(z0)
=−1
2 l(x0)2 + bx0y0 − 1
2 ry(y0)2 + rx
2 (x0 − z0)2
− 2 − (by0 − rxz0)2
2(rx − l) + rx
2 (z0)2 − ry
2 (y0)2 + 2lrx(z0)2
rx + 2l
≤ bx0y0 + rx
2 (x0 − z0)2 + (by0 − rxz0)2
rx − l + 2lrx(z0)2
rx + 2l = O ε2
l,
where the last equality follows from (5.4) and Condition 3.2. Combining with (A.8) yields the desired result. ■
A.9 Proof of Theorem 3.4 (i)
Proof of Theorem 3.4 (i). We initialize Perturbed Smoothed GDA as in Lemma 5.4, i.e., using (5.4). If (xT , yT ) is an ε-GS, then by definition,
ε ≥ |∇xf (xT , yT )| = |byT − lxT |
= −clβ − clλ2 + crxλ2 + βλ2 − crxβλ2 + λ22
c(−β − λ2) − l xT
= (crx(1 − β) + β + λ2)λ2
c(β + λ2) xT
≥ rxλ2
β + λ2
|xT | = rxλ2
β + λ2
(1 + λ2)T |x0| = 2ε(1 + λ2)T .
Therefore,
(1 + λ2)T ≤ 1
2 =⇒ T = Ω 1
|λ2| = Ω lDY
ε,
where the last equality follows from Lemma 5.3. Finally, Lemma A.2 gives ∆Ψ2 = O ε2
l under our
initialization, and thus
T = Ω lDY
ε = Ω lDY
ε · ∆Ψ2
ε2/l = Ω l2DY ∆Ψ2
ε3 .
This completes the proof. ■
44

A.10 l-Smoothness Property in Example 5.2
Lemma A.3. The function f (x, y) in Example 5.2 is l-smooth.
Before establishing the smoothness of f , we first introduce a technical lemma that will be used in the proof of Lemma A.3.
Lemma A.4 (Bounded derivative implies Lipschitz continuity). Let φ : R → R be continuous and piecewise C1. If |φ′(t)| ≤ l wherever φ′ exists, then φ is l-Lipschitz.
Proof of Lemma A.4. Fix t < t′ and let {t1, . . . , tn} be the (finite) set of points in (t, t′) where φ is not differentiable. Define t0 := t and tn+1 := t′. Then φ is differentiable on each interval (ti, ti+1). By the mean value theorem, for every i = 0, . . . , n, there exists ξi ∈ (ti, ti+1) such that
|φ(ti+1) − φ(ti)| = |φ′(ξi)| |ti+1 − ti| ≤ l |ti+1 − ti|.
Summing over i yields
|φ(t′) − φ(t)| ≤
n
X
i=0
|φ(ti+1) − φ(ti)| ≤ l
n
X
i=0
|ti+1 − ti| = l|t′ − t|,
which proves that φ is l-Lipschitz. This completes the proof. ■
With Lemma A.4 in hand, we are ready to prove Lemma A.3.
Proof of Lemma A.3. By Lemma A.4, to prove Lemma A.3, it suffices to show that for all x ∈ R with |x| ̸= 1 and |x| ̸= 2,
max{|∇xxf (x, y)|, |∇xyf (x, y)|, |∇yyf (x, y)|} ≤ l. (A.16)
A direct calculation shows that the second-order derivatives are given by
|∇xxf (x, y)| = l
(DY + 1) |y| ≤ lDY
DY + 1 ≤ l, ∀ x ∈ R and |x| ̸= 1 or 2
|∇xyf (x, y)| =

  
  
l
(DY +1) |x| ≤ l
(DY +1) ≤ l, |x| ≤ 1,
l
(DY +1) |2sign(x) − x| ≤ l
(DY +1) ≤ l, 1 < |x| ≤ 2,
0 ≤ l, |x| > 2,
|∇yyf (x, y)| = 0 ≤ l, ∀x ∈ R.
Therefore, (A.16) holds and we derive our desired property of f . This completes the proof. ■
A.11 Initial Gap of Example 5.2 under Perturbed GDA
Lemma A.5. Suppose that the initialization is chosen as (x0, y0) = ( 3DY+2
lDY ε, DY ). Then, it holds that
∆Ψ1 = O DY + 1
lDY
ε2 .
45

Proof of Lemma A.5. Recall that ∆Ψ1 = Ψ01(x0, y0) − minx∈R maxy∈[0,DY] f0(x, y), where Ψ01(x0, y0) =
2Φ0(x0) − f0(x0, y0). To bound ∆Ψ1, we first compute Φ0(x0), which is given by
Φ0(x0) = max
y∈[0,DY ]
f0(x0, y)
= max
y∈[0,DY ]
h(x0)y − ry
2 y2
≤ max
y∈[0,DY ]
h(x0)y
=l
2(DY + 1) (x0)2DY . (A.17)
Thus, we bound the initial gap as follows:
∆Ψ1 = Ψ0
1(x0, y0) − min
x∈R max
y∈[0,DY ]
f0(x, y)
= 2Φ0(x0) − f0(x0, y0) − min
x∈R max
y∈[0,DY ]
f0(x, y)
≤l
DY + 1 (x0)2DY − l
2(DY + 1) (x0)2y0 − ry
2 (y0)2 − min
x∈R max
y∈[0,DY ]
h(x)y − ry
2 y2
=l
2(DY + 1) (x0)2DY + ry
2 D2
Y − min
x∈R max
y∈[0,DY ]
h(x)y − ry
2 y2
≤l
2(DY + 1) (x0)2DY + ry
2 D2
Y − min
x∈R max
y∈[0,DY ]
h(x)y + ry
2 D2
Y
=l
2(DY + 1) (x0)2DY + ryD2
Y
= Θ 2(3DY + 2)2
l(DY + 1)DY
ε2 + 1
l ε2
= Θ (DY + 1)
lDY
ε2 .
Here, the first inequality follows from (A.17) and the equality y0 = DY . Moreover, for any x ∈ R, we have
maxy∈[0,DY ](h(x)y − ry
2 y2) ≥ maxy∈[0,DY ] h(x)y − ry
2 D2
Y . Therefore, taking the minimum over x ∈ R on both sides gives,
min
x∈R max
y∈[0,DY ]
h(x)y − ry
2 y2 ≥ min
x∈R max
y∈[0,DY ]
h(x)y − ry
2 D2
Y.
Next, note that h(x) ≥ 0 for all x ∈ R, for each fixed x, the maximizer of the mapping y 7→ h(x)y over [0, DY ] is y⋆ = DY , and hence minx∈R maxy∈[0,DY] h(x)y = 0, which leads to the fourth equality. Finally,
the fifth equality uses the expression of x0 and ry. This completes the proof. ■
A.12 Proof of Lemma 5.7
Proof of Lemma 5.7. First, we establish that λ3 < 0. Starting from (5.10), we have
λ3 = − A3 + B3β
2 +1
2
p(A3 + B3β)2 − 4lcβγ
= −1
2 · 4lcβγ
A3 + B3β + p(A3 + B3β)2 − 4lcβγ
46

= − 2lcγ
A3 + B3β + p(A3 + B3β)2 − 4lcβγ β. (A.18)
To show that λ3 < 0, because A3 > 0 and B3 > 0, it suffices to verify that
(A3 + B3β)2 − 4lcβγ ≥ 0.
Indeed, we have (A3 + B3β)2 − 4lcβγ = A23 + O(β), and thus the above inequality holds for β = Θ ε2
l2 Dy2
sufficiently small. Moreover, we estimate the magnitude of λ3. By (A.18), the numerator is of order Θ(γ), while the denominator is of order Θ(1) by Condition 3.2. Therefore,
|λ3| = O(γβ) = O γε2
l2Dy2
.
This completes the proof. ■
A.13 Proof of Lemma 5.8
Proof of Lemma 5.8. One can verify that an eigenvector associated with λ3 is given by
v= 1
2(1 − lcγ − crx)β
h
−lcγ − crx + β − crxβ
+
p(lcγ + crx)2 − 2(lcγ + c2γlrx + c2rx2 − crx)β + (crx − 1)2β2
i
,1
=1
2(1 − A3)β
h
−A3 + B3β + p(A3 − B3β)2 + 4crxβ(1 − A3)
i
,1 .
Let v1 denote the first entry of v in the above expression. We next show that v1 admits the representation in (5.11). In particular, we have
v1 = 1
2(1 − A3)β −A3 + B3β + p(A3 − B3β)2 + 4crxβ(1 − A3)
=1
2(1 − A3)β · 4crxβ(1 − A3)
A3 − B3β + p(A3 − B3β)2 + 4crxβ(1 − A3) = 2crx
A3 − B3β + p(A3 − B3β)2 + 4crxβ(1 − A3) = 2crx
A3 + O(β) + A3 + O(β)
= rx
rx + γl + O(β),
where the fourth equality follows from β is sufficiently small and the Maclaurin expansion of (1 + ·)− 1
2.
The last equality uses the fact that β is sufficiently small and the Maclaurin expansion of (1 + ·)−1. This completes the proof. ■
47

A.14 Initial Gap of Example 5.2 under Perturbed Smoothed GDA
Lemma A.6. Suppose that Perturbed Smoothed GDA applied to Example 5.2 is initialized with x0 =
3DY +2
lDY ε, y0 = DY and z0 = x0
v1 . Then the initial gap satisfies
∆Ψ2 = O ε2
lγ .
Proof of Lemma A.6. Recall that
∆Ψ2 = Ψ0
2(x0, y0, z0) − min
x∈R max
y∈[0,DY ]
f0(x, y)
Ψ0
2(x, y, z) = F0(x, y, z) − 2 min
x∈R F0(x, y, z) + 2 min
x∈R max
y∈[0,DY ]
F0(x, y, z).
We first compute the explicit form of Ψ02(x, y, z). Recall that d0(y0, z0) = minx∈R F0(x, y0, z0) and
p0(z0) = minx∈R maxy∈[0,DY ] F0(x, y, z0). Then, we have
F0(x0, y0, z0) = l
2(DY + 1) (x0)2y0 + rx
2 (x0 − z0)2 − ry
2 (y0)2,
d0(y0, z0) = min
x∈R h(x)y0 + rx
2 (x − z0)2 − ry
2 (y0)2
= min
x∈R h(x)DY + rx
2 (x − z0)2 − ry
2 D2
Y,
p0(z0) = min
x∈R max
y∈[0,DY ]
h(x)y + rx
2 (x − z0)2 − ry
2 y2
≤ min
x∈R max
y∈[0,DY ]
h(x)y + rx
2 (x − z0)2
= min
x∈R h(x)DY + rx
2 (x − z0)2 .
Consequently, we obtain
Ψ0
2(x0, y0, z0) = F0(x0, y0, z0) − 2d0(y0, z0) + 2p0(z0)
≤l
2(DY + 1) (x0)2y0 + rx
2 (x0 − z0)2 + ry
2 (y0)2.
Moreover, the initial gap ∆Ψ2 satisfies
∆Ψ2 = Ψ0
2(x0, y0, z0) − min
x∈R max
y∈[0,DY ]
f (x, y) − ry
2 y2
≤l
2(DY + 1) (x0)2y0 + rx
2 (x0 − z0)2 + ry
2 (y0)2 − min
x∈R max
y∈[0,DY ]
f (x, y) − ry
2 D2
Y
=l
2(DY + 1) (x0)2DY + rx
2 (x0 − z0)2 + ryD2
Y , (A.19)
where the second equality uses that minx∈R maxy∈[0,DY] f (x, y) = 0.
It remains to bound the order of the terms above. By Lemma 5.8, we have z0−x0 = Θ rx+γl
rx − 1 x0 =
Θ γx0 = Θ ε
l . Substituting these estimates yields ∆Ψ2 = O DY+1
lDY ε2 + ε2
l + ε2
l = O ε2
lγ . This
completes the proof. ■
48

A.15 A Technical Lemma for Theorem 6.1
Lemma A.7. For xˆ and yˆ defined in (6.7) and (6.8), we have
∥xt⋆+1 − xˆ∥ = O
√
δt⋆ , and ∥yt⋆+1 − yˆ∥ = O l
rt⋆
y
√
δt⋆ .
Proof of Lemma A.7. First of all, we have
∥xt⋆+1 − xˆ∥
≤ ∥xt⋆+1 − xt⋆ (yt⋆+1, zt⋆ )∥ + ∥xˆ − xt⋆ (yt⋆+1, zt⋆ )∥
≤ (2 + α(rx + l))∥xt⋆+1 − xt⋆ (yt⋆+1, zt⋆ )∥
≤ (2 + α(rx + l)) ∥xt⋆+1 − xt⋆ (yt⋆ (zt⋆ ), zt⋆ )∥ + ∥xt⋆ (yt⋆ (zt⋆ ), zt⋆ ) − xt⋆ (yt⋆+1, zt⋆ )∥
≤ (2 + α(rx + l)) xt⋆+1 −x⋆
t⋆ (zt⋆ ) + 2(rx + l + rt⋆
y)
rx − l − rt⋆
y
∥yt⋆+1 − yt⋆ (zt⋆ )∥
!
=O
√
δt⋆ ,
where the second inequality follows from the nonexpansiveness of projX (·), the fact that xt⋆(yt⋆+1, zt⋆) =
projX (xt⋆ (yt⋆+1, zt⋆ )−α∇xFt⋆ (xt⋆ (yt⋆+1, zt⋆ ), yt⋆+1, zt⋆ )), and (rx+l)-Lipschitz continuity of ∇xFt⋆ (·, yt⋆+1, zt⋆ ), the fourth inequality is from (Zhang et al., 2020, Lemma B.2), and the last equality follows from (6.3) and Condition 6.1, i.e., rx > 3l, and α < 1
rx+l .
We now turn to the dual part. By a similar argument, we have
∥yt⋆+1 − yˆ∥
≤ (2 + c(rt⋆
y + l)) yt⋆+1 − y⋆
t⋆ x⋆
t⋆ (zt⋆ ) + y⋆
t⋆ (xt⋆+1) − y⋆
t⋆ x⋆
t⋆ (zt⋆ )
≤ (2 + c(rt⋆
y + l)) ∥yt⋆+1 −yt⋆ (zt⋆ )∥+ l
rt⋆
y
xt⋆+1 −x⋆
t⋆ (zt⋆ ) = O l
rt⋆
y
√
δt⋆ ,
where the second inequality follows from that yt⋆(z) = yt⋆⋆(xt⋆⋆(z)) for all z and yt⋆⋆(·) is l
rt⋆
y -smooth
(Lin et al., 2020a, Lemma 4.3), and the last equality comes from (6.3) and c < 1
rt⋆
y +l . This completes the
proof. ■
B Convergence Analysis of Smoothed GDA
In this section, we refine the iteration–complexity result of (Zhang et al., 2020) by making explicit its dependence on the smoothness constant l and the diameter DY , which reveals how these structural problem parameters dictate the convergence rate. Moreover, unlike (Zhang et al., 2020), which only establishes guarantees for ε-GS, we further characterize how to attain an ε-OS solution. The mechanism for achieving ε-OS differs from the argument in (Li et al., 2025). Following our unified analysis framework, we adopt the same Lyapunov function as in (4.18) under the restriction rty = 0, i.e., ft = f . To proceed, we recall several parameter definitions originally introduced in (Zhang et al., 2020), which will repeatedly appear in our convergence analysis.
49

Definition B.1 (Parameters conditions for Smoothed GDA). Under Condition 3.2, we set the parameters as follows:
σ2 := 2(rx + l)
rx − l = Θ(1),
ρ1 := 384rxDY
1 + αtl + αtlσ2
rx − l = Θ(DY ),
ρ2 := 384DY ρ1
1 + αtl + αtlσ2
αt(rx − l) = Θ D2
Y,
ρ3 := 384rxctDY ρ1
1 + αtl + αtlσ2
αt(rx − l) = Θ D2
Y,
We now present the iteration–complexity result for Smoothed GDA, together with its proof.
Theorem B.1 (Iteration complexity of Smoothed GDA). Let {(xt, yt, zt)}t≥0 be generated by Smoothed GDA.
For any ε > 0, if the step sizes are chosen according to Condition 3.2, then after at most t = O l3D2
Y ∆Ψ2 ε4
iterations, (xt, yt) is an ε-GS and xt is an ε-OS for problem (1.1).
Proof of Theorem B.1. In the analysis below, we set βt = Θ
r
∆Ψ2
lD2
YT . Recall from (Zhang et al., 2020,
Proposition 4.1) that Ψt2 satisfies the basic descent bound
Ψt
2 − Ψt+1
2 ≥1
8ct ∥xt+1 − xt∥2 + 1
8αt ∥yt − yt
+(zt)∥2 + rxβt
8 ∥zt − xt+1∥2
− 24rxβt x⋆
t (zt) − xt(yt
+(zt), zt) 2 . (B.1)
This descent inequality motivates a case-based argument for bounding the progress of each component. There are two complementary situations, which we treat separately. (i): There exists some t ∈ {0, 1, . . . , T − 1} such that
1
2 max 1
8ct ∥xt+1 − xt∥2, 1
8αt ∥yt − yt
+(zt)∥2, rxβt
8 ∥zt − xt+1∥2
≤ 24rxβt x⋆
t (zt) − xt(yt
+(zt), zt) 2 .
Thanks to (Zhang et al., 2020, Theorem 3.4 (B.63)), we have
∥xt+1 − xt∥ ≤ √ρ3βt, ∥yt − yt
+(zt)∥ ≤ ρ1βt, and ∥zt − xt+1∥ ≤ pρ2βt. (B.2)
(ii): For any t ∈ {0, 1, · · · , T − 1}, we have
1
2 max 1
8ct ∥xt+1 − xt∥2, 1
8αt ∥yt − yt
+(zt)∥2, rxβt
8 ∥zt − xt+1∥2
≥ 24rxβt x⋆
t (zt) − xt(yt
+(zt), zt) 2 . (B.3)
By combining (B.1) and (B.3), we obtain that for any t ∈ {0, 1, · · · , T − 1} it holds
Ψt
2 − Ψt+1
2 ≥1
16ct ∥xt+1 − xt∥2 + 1
16αt ∥yt − yt
+(zt)∥2 + rxβt
16 ∥zt − xt+1∥2.
50

Thus, by (Zhang et al., 2020, Lemma B.1), for any integer T > 0, there exists an index t ∈ T such that
∥xt+1 −xt∥ = O
r
ct∆Ψ2
T
!
, ∥yt −yt
+(zt)∥ = O
r
αt∆Ψ2
T
!
, ∥zt −xt+1∥ = O
s
∆Ψ2
rxβtT
!
. (B.4)
Therefore, combining the two cases and substituting the parameter choices from Definition B.1 and Condition 3.2 into (B.2) and (B.4), we obtain that there exists some t ∈ [T ] such that
∥xt+1 −xt∥ = O
r ∆Ψ2 lT
!
, ∥yt −yt
+(zt)∥ = O
r ∆Ψ2 lT
!
, ∥zt −xt+1∥ = O


4
s
D2
Y ∆Ψ2
lT

. (B.5)
Then, the necessary optimality condition of y-update yields
dist(0, −∇yf (xt+1, yt+1) + ∂ιY (yt+1))
≤1
αt + l ∥yt − yt
+(zt)∥ + αtl 1 + ct(rx − l)
ct(rx − l) ∥xt+1 − xt∥ (B.6)
=O
r l∆Ψ2
T
!
, (B.7)
where the first inequality is obtained by applying (4.20) with rty = 0, and the last inequality follows from (B.5) and Condition 3.2, namely αt = Θ( 1
l ). Moreover, from (4.22), we have
dist(0, ∇xf (xt+1, yt+1) + ∂ιX (xt+1))
≤1
ct +l ∥xt+1 −xt∥+l ∥yt −yt
+(zt)∥+αtl 1+ct(rx −l)
ct(rx −l) ∥xt+1 −xt∥ +rx∥xt+1 −zt∥
=O


4
s
l3D2
Y ∆Ψ2
T

 , (B.8)
where the last inequality comes from (B.5) and Condition 3.2, i.e, ct = Θ( 1
l ), rx = Θ(l). Combining (B.7)
and (B.8), we conclude that (xt+1, yt+1) is an ε-GS. Rewriting the bound in terms of ε shows that achieving
an ε-GS requires T = O l3D2
Y ∆Ψ2
ε4 , which corresponds to choosing βt = Θ ε2
l2 D2
Y
.
For the OS analysis, Lemma 4.3 allows us to replicate the argument in the proof of Theorem 3.1 (ii), with (B.7) and (B.8) plugged in accordingly, which yields,
2l prox 1
2l Φ(xt+1) − xt+1 = O


4
s
l3D2
Y ∆Ψ2
T

.
Following the similar argument, we will need at least T = O l3D2
Y ∆Ψ2
ε4 to reach an ε-OS if we choose
βt = Θ ε2
l2 D2
Y
. This completes the proof. ■
Proof of Corollary 3.1. Similar to Section 5, we consider Smoothed GDA with constant choice of rty for
simplicity and drop iteration superscripts. We write rty = ry, and βt = β.
51

(i) We consider the following hard instance:
f (x, y) =

 
 
− ry
2 y2, if x < 0,
−1
2 lx2 + bxy − ry
2 y2, if 0 ≤ x ≤ ryDY
b,
− lry2D2
Y
2b2 + ryDY y − ry
2 y2, if x > ryDY
b,
where x ∈ R and y ∈ [0, DY ], ry = ε
DY , and b = p3lry. Applying Smoothed GDA to f (x, y) is essentially
identical to applying Perturbed Smoothed GDA to Example 5.1, except for the choice of β. Thus we apply the analysis of Theorem 3.4 (i) to this problem. We initialize the algorithm at the same point (x0, y0, z0) specified in Lemma 5.4. Since the lower bound proof follows the same argument as in Section 5.1.2, we omit the details. As a result, the iteration complexity satisfies
T = Ω ∆Ψ2
lβε2 = Ω l3D2
Y ∆Ψ2 ε4
!
,
which matches Theorem B.1 (i). (ii) We consider the same hard instance as in Example 5.2. We initialize Smoothed GDA by selecting (x0, z0) as an eigenvector associated with the eigenvalue λ3. Specifically, x0 = 3DY+2
lDY ε and y0 = DY . Under this initialization, the resulting recursion coincides with (5.9). Indeed, for all x ∈ X , we have ∇yF (x, y, z) ≥ 0, and therefore the y-iterate generated by Smoothed GDA is nondecreasing. Since y0 = DY , it follows that yt = DY for all t ≥ 0. Consequently, the updates
reduce to (5.9). By Section 5.2.1, the recursion (5.9) requires Ω l3D2
Y ∆Ψ2
ε4 iterations to reach an ε-OS point
on Example 5.2. Hence, Smoothed GDA also needs Ω l3D2
Y ∆Ψ2
ε4 iterations to find an ε-OS point. This
completes the proof. ■
52
