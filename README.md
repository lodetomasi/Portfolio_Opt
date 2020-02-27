# Portfolio_Opt
How to use Gurobipy to optimize the allocation of securities in an equity portfolio

The goal of this repository is to show an optimization approach through the use of Gurobi. The necessary files are the two present in the repository, I am interested in having feedback and I would be very happy if someone had ideas on how to improve it. 

The idea is based on:



# Problem

Let $I = \{1, \ldots, n\}$a set of financial products. For every $ i \ in I $ are given:
- $r_i \in\mathbb{R}$ expected return in the next period;
$q_i \in\mathbb{Z}$ equivalent value of the security in the current portfolio;<-->
- $s_i \in S$ type of assets;
$\delta_i$ maximum variation in the invested value allowed with respect to the current value<-->
- $\phi^{max}_k$, $\phi^{min}_k \in [0,1]$ maximum and minimum fraction, respectively, of budget allocable on a single product $k, \forall k \in S$;
- $\gamma^{max}_i$, $\gamma^{min}_i \in [0,1]$ maximum and minimum ration, respectively, of budget allocable on a single product $i, \forall i \in I$.


**Product types**
The $S$ set contains possible types, for example, $S={A,O,M}$, with $A$ equity, $O$ bond, $M$ mixed. 

**Risk** The risk associated with the portfolio is modeled with the *parametric model*, i.e. through an assigned $Q$ covariance matrix and a maximum risk $R_{max}$ associated with the investor profile.

**Budget** Let's be $B$ the budget. This can be equal to the current $\sum_i^n{q_i}$ value, or specified otherwise, if you decide to invest new capital or disinvest.
Let this be the percentage countervalue of the single security, I define deltai as the maximum percentage variation.


# Optimization model
We define the decision variables (real)  $x_i$, $i \in I$, representing the percentage of value to be allocated on the $i$ product. The model is as follows:

\begin{align}
\label{eq:obj}  \max & \sum_{i = 1}^n r_i x_i & \nonumber \\
 \nonumber \mbox{s.t. } &\\
 & \sum_{i \in I} x_i \le 1, & \\
  & x^T Q x \leq R_{max}, & \\
  %& |x_i - q_i| \le \delta_i q_i, & i \in I \\
  & \sum_{i:s_i = k} x_i \ge \phi^{min}_k, & k \in S \\
  & \sum_{i:s_i = k} x_i \le \phi^{max}_k, & k \in S \\
  & x_i \ge \gamma^{min}_i, & i \in I \\
  & x_i \le \gamma^{max}_i, & i \in I \nonumber
\end{align}
